# Supplier Sync API Specification

**Version**: 1.0.0
**Date**: April 2026
**Owner**: Platform Engineering Team
**Status**: Ready for Implementation

## Overview

The Supplier Sync API enables procurement platforms to synchronize supplier master data from external systems (ERP, supplier portals, data providers) into our procurement analytics platform. The API supports bulk sync operations, real-time updates, and webhook notifications.

**Base URL**: `https://api.procurement.example.com/v1`
**Authentication**: OAuth 2.0 (Client Credentials)

---

## Authentication

### OAuth 2.0 Client Credentials Flow

**Endpoint**: `POST /auth/token`

```bash
curl -X POST https://api.procurement.example.com/auth/token \
  -H "Content-Type: application/x-www-form-urlencoded" \
  -d "grant_type=client_credentials&client_id=YOUR_CLIENT_ID&client_secret=YOUR_CLIENT_SECRET"
```

**Response**:
```json
{
  "access_token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
  "token_type": "Bearer",
  "expires_in": 3600
}
```

**Token Expiry**: 1 hour (refresh required)
**Scopes**: `suppliers:read`, `suppliers:write`, `suppliers:sync`

---

## Rate Limiting

**Requests per minute**: 60 (standard tier), 300 (premium tier)
**Burst allowance**: 2x limit for 30 seconds

**Headers**:
```
X-RateLimit-Limit: 60
X-RateLimit-Remaining: 45
X-RateLimit-Reset: 1712973600
```

**When exceeded**: Returns 429 Too Many Requests
```json
{
  "error": "rate_limit_exceeded",
  "retry_after": 30
}
```

---

## Endpoints

### 1. Create Supplier Record

**Endpoint**: `POST /suppliers`
**Authentication**: Required (scope: `suppliers:write`)
**Rate Limit**: 30 requests/minute

**Request**:
```json
{
  "external_id": "SUP-12345",
  "name": "Acme Manufacturing Inc.",
  "country": "US",
  "category": "Manufacturing",
  "primary_contact": {
    "name": "John Smith",
    "email": "john@acme.com",
    "phone": "+1-555-0123"
  },
  "payment_terms": "NET_30",
  "performance_score": 85,
  "active": true,
  "metadata": {
    "erp_id": "VENDOR-001",
    "source_system": "SAP"
  }
}
```

**Request Fields**:

| Field | Type | Required | Max Length | Notes |
|-------|------|----------|-----------|-------|
| external_id | string | Yes | 50 | Unique ID from source system |
| name | string | Yes | 255 | Company legal name |
| country | string | Yes | 2 | ISO 3166-1 alpha-2 code |
| category | enum | Yes | - | Manufacturing, Service, Logistics, etc. |
| primary_contact | object | No | - | Main contact for supplier |
| payment_terms | enum | No | - | NET_30, NET_60, COD, etc. |
| performance_score | integer | No | - | 0-100, from source system |
| active | boolean | No | - | Defaults to true |
| metadata | object | No | - | Custom fields (max 50 pairs) |

**Response** (201 Created):
```json
{
  "id": "supp_2q1w9e8r7t6y",
  "external_id": "SUP-12345",
  "name": "Acme Manufacturing Inc.",
  "created_at": "2026-04-12T10:30:00Z",
  "updated_at": "2026-04-12T10:30:00Z",
  "status": "active"
}
```

**Error Responses**:
- `400 Bad Request`: Invalid input (e.g., invalid country code)
- `401 Unauthorized`: Invalid token
- `409 Conflict`: Supplier with same external_id already exists
- `422 Unprocessable Entity`: Business rule violation (e.g., missing required field)

**Example Error**:
```json
{
  "error": "duplicate_external_id",
  "message": "Supplier with external_id 'SUP-12345' already exists",
  "external_id": "SUP-12345"
}
```

---

### 2. Update Supplier Record

**Endpoint**: `PUT /suppliers/{id}`
**Authentication**: Required (scope: `suppliers:write`)
**Rate Limit**: 30 requests/minute

**Request**:
```json
{
  "name": "Acme Manufacturing Inc. (Updated)",
  "performance_score": 88,
  "payment_terms": "NET_45",
  "metadata": {
    "risk_level": "medium",
    "contract_expires": "2027-12-31"
  }
}
```

**Response** (200 OK):
```json
{
  "id": "supp_2q1w9e8r7t6y",
  "external_id": "SUP-12345",
  "name": "Acme Manufacturing Inc. (Updated)",
  "updated_at": "2026-04-12T11:45:00Z",
  "status": "active"
}
```

**Error Responses**:
- `404 Not Found`: Supplier ID does not exist
- `409 Conflict`: Concurrent update detected

---

### 3. Get Supplier Details

**Endpoint**: `GET /suppliers/{id}`
**Authentication**: Required (scope: `suppliers:read`)
**Rate Limit**: 1,000 requests/minute

**Response** (200 OK):
```json
{
  "id": "supp_2q1w9e8r7t6y",
  "external_id": "SUP-12345",
  "name": "Acme Manufacturing Inc.",
  "country": "US",
  "category": "Manufacturing",
  "primary_contact": {
    "name": "John Smith",
    "email": "john@acme.com",
    "phone": "+1-555-0123"
  },
  "payment_terms": "NET_45",
  "performance_score": 88,
  "active": true,
  "created_at": "2026-04-12T10:30:00Z",
  "updated_at": "2026-04-12T11:45:00Z",
  "metadata": {
    "risk_level": "medium",
    "contract_expires": "2027-12-31"
  }
}
```

---

### 4. Bulk Sync Suppliers

**Endpoint**: `POST /suppliers/bulk-sync`
**Authentication**: Required (scope: `suppliers:sync`)
**Rate Limit**: 5 requests/minute
**Max records per request**: 1,000

**Request**:
```json
{
  "sync_id": "sync_20260412_001",
  "source_system": "SAP",
  "sync_type": "full",
  "suppliers": [
    {
      "external_id": "SUP-12345",
      "name": "Acme Manufacturing",
      "country": "US",
      "category": "Manufacturing"
    },
    {
      "external_id": "SUP-12346",
      "name": "Beta Services Ltd.",
      "country": "UK",
      "category": "Service"
    }
  ]
}
```

**Request Fields**:
- `sync_id`: Unique identifier for this sync operation (idempotency key)
- `source_system`: Origin system (SAP, Oracle, Workday, etc.)
- `sync_type`: "full" (replace all) or "incremental" (update only changed)
- `suppliers`: Array of supplier objects (max 1,000)

**Response** (202 Accepted):
```json
{
  "sync_job_id": "job_2q1w9e8r7t6y",
  "status": "processing",
  "created_at": "2026-04-12T12:00:00Z",
  "record_count": 2,
  "status_url": "https://api.procurement.example.com/v1/sync-jobs/job_2q1w9e8r7t6y"
}
```

**Async Processing**: Check status at `/sync-jobs/{job_id}`

```json
{
  "sync_job_id": "job_2q1w9e8r7t6y",
  "status": "completed",
  "record_count": 2,
  "successful": 2,
  "failed": 0,
  "completed_at": "2026-04-12T12:02:30Z",
  "results": [
    {
      "external_id": "SUP-12345",
      "status": "created",
      "id": "supp_2q1w9e8r7t6y"
    }
  ]
}
```

---

### 5. Webhook: Supplier Updated

**Webhook URL**: Customer-provided endpoint (configured during setup)
**Authentication**: HMAC SHA-256 signature verification

**Headers**:
```
X-Webhook-Signature: sha256=abc123...
X-Webhook-ID: whk_1234567890
X-Webhook-Timestamp: 1712973600
```

**Payload**:
```json
{
  "event": "supplier.updated",
  "timestamp": "2026-04-12T12:30:00Z",
  "data": {
    "id": "supp_2q1w9e8r7t6y",
    "external_id": "SUP-12345",
    "name": "Acme Manufacturing Inc.",
    "updated_fields": ["performance_score", "payment_terms"]
  }
}
```

**Webhook Events**:
- `supplier.created`: New supplier added
- `supplier.updated`: Supplier modified
- `supplier.deleted`: Supplier removed
- `supplier.sync_completed`: Bulk sync finished
- `supplier.sync_failed`: Bulk sync encountered errors

---

## Data Schemas

### Supplier Object

```yaml
Supplier:
  type: object
  required:
    - external_id
    - name
    - country
  properties:
    id:
      type: string
      description: Internal supplier ID (generated by us)
    external_id:
      type: string
      maxLength: 50
      description: Unique ID from source system
    name:
      type: string
      maxLength: 255
    country:
      type: string
      pattern: '^[A-Z]{2}$'
      description: ISO 3166-1 alpha-2 code
    category:
      type: string
      enum: [Manufacturing, Service, Logistics, Distribution, Consulting, Other]
    primary_contact:
      $ref: '#/components/schemas/Contact'
    performance_score:
      type: integer
      minimum: 0
      maximum: 100
    active:
      type: boolean
    created_at:
      type: string
      format: date-time
    updated_at:
      type: string
      format: date-time

Contact:
  type: object
  properties:
    name:
      type: string
      maxLength: 255
    email:
      type: string
      format: email
    phone:
      type: string
      pattern: '^\+?[0-9\-\s\(\)]{7,20}$'
```

---

## Error Codes

| Code | Status | Description |
|------|--------|-------------|
| bad_request | 400 | Invalid request format |
| invalid_field | 400 | Specific field is invalid |
| duplicate_external_id | 409 | Supplier with same external_id exists |
| not_found | 404 | Supplier/job not found |
| unauthorized | 401 | Invalid or missing auth token |
| forbidden | 403 | Insufficient permissions |
| rate_limit_exceeded | 429 | Too many requests |
| unprocessable_entity | 422 | Business rule violation |
| internal_error | 500 | Server error |

---

## Security & Compliance

### Data Encryption
- **In Transit**: TLS 1.2+ required (all requests must use HTTPS)
- **At Rest**: Supplier data encrypted with AES-256 in database
- **Sensitive Fields**: email, phone redacted in logs

### PII Handling
- Supplier contacts contain PII (name, email, phone)
- PII is encrypted at rest
- GDPR right-to-erasure: Delete PII if requested
- No logs contain full email/phone (redacted as xxx@xxx.com)

### Audit Trail
- All API calls logged: timestamp, client_id, endpoint, action, result
- Supplier change history: track who created/updated each record
- Retention: 7 years (compliance requirement)

### Client Credentials Rotation
- Client secret should be rotated every 90 days
- Old credentials remain valid for 30 days during rotation
- Monitor for compromised credentials (unusual access patterns)

---

## Implementation Examples

### Python
```python
import requests
import json
from datetime import datetime

class SupplierSyncClient:
    def __init__(self, client_id, client_secret):
        self.client_id = client_id
        self.client_secret = client_secret
        self.token = None
        self.base_url = "https://api.procurement.example.com/v1"
    
    def get_token(self):
        response = requests.post(
            f"{self.base_url}/auth/token",
            data={
                "grant_type": "client_credentials",
                "client_id": self.client_id,
                "client_secret": self.client_secret
            }
        )
        self.token = response.json()["access_token"]
        return self.token
    
    def create_supplier(self, supplier_data):
        headers = {"Authorization": f"Bearer {self.token}"}
        response = requests.post(
            f"{self.base_url}/suppliers",
            json=supplier_data,
            headers=headers
        )
        return response.json()

# Usage
client = SupplierSyncClient("YOUR_CLIENT_ID", "YOUR_CLIENT_SECRET")
client.get_token()
new_supplier = client.create_supplier({
    "external_id": "SUP-12345",
    "name": "Acme Inc.",
    "country": "US",
    "category": "Manufacturing"
})
```

### JavaScript/Node.js
```javascript
const axios = require('axios');

class SupplierSyncClient {
  constructor(clientId, clientSecret) {
    this.clientId = clientId;
    this.clientSecret = clientSecret;
    this.token = null;
    this.baseURL = "https://api.procurement.example.com/v1";
  }

  async getToken() {
    const response = await axios.post(`${this.baseURL}/auth/token`, null, {
      params: {
        grant_type: "client_credentials",
        client_id: this.clientId,
        client_secret: this.clientSecret
      }
    });
    this.token = response.data.access_token;
    return this.token;
  }

  async createSupplier(supplierData) {
    const response = await axios.post(
      `${this.baseURL}/suppliers`,
      supplierData,
      {
        headers: {
          Authorization: `Bearer ${this.token}`
        }
      }
    );
    return response.data;
  }
}
```

---

## Versioning & Deprecation

**Current Version**: 1.0.0 (April 2026)
**Deprecation Policy**: Endpoints deprecated with 6-month notice

**Planned Changes**:
- v1.1 (Q3 2026): Add supplier scoring endpoint
- v2.0 (2027): GraphQL support, webhooks v2

---

**Document Owner**: Platform Engineering
**Last Updated**: April 2026
**Next Review**: Q3 2026
