---
name: /write-api-spec
description: Generate API specification with enterprise security patterns, rate limiting, and schema validation
category: pm-definition
tags: [api, specification, integration, technical]
---

## Overview
Creates a detailed API specification including request/response schemas, authentication patterns, error handling, rate limiting, and compliance requirements. Follows OpenAPI 3.0 standard with enterprise-grade patterns.

## Usage
```
/write-api-spec \
  --endpoint "supplier-sync" \
  --methods GET,POST,PUT \
  --authentication oauth2 \
  --include-rate-limiting true \
  --include-compliance true \
  --include-webhooks true
```

## Skill Chain (Sequential)

### 1. API-Spec-Design (Processing)
- **Contributes**: Endpoint definitions, request/response schemas, error handling
- **Input**: Feature requirements, integration requirements, existing API patterns
- **Process**:
  - Design RESTful endpoints
  - Define request/response payloads
  - Design error codes and handling
  - Add examples
- **Output**: OpenAPI 3.0 specification

### 2. Compliance-Review (Validation)
- **Contributes**: Security patterns, rate limiting, data protection requirements
- **Input**: API spec
- **Process**:
  - Add OAuth2/SAML authentication
  - Define rate limiting per endpoint
  - Add data encryption requirements
  - Flag PII/compliance concerns
- **Output**: API spec with compliance sections

## Data Handoff

```
Feature Requirements
    ↓
API Design
    ↓ (endpoint definitions, schemas)
Compliance Review
    ↓ (auth, rate limits, security)
Final API Spec
    → OpenAPI 3.0 document
    → Code examples (Python, JavaScript, cURL)
    → Webhook patterns
    → Security & compliance checklist
```

## Output Format

```yaml
openapi: 3.0.0
info:
  title: Procurement API
  version: 1.0.0
paths:
  /suppliers/{id}:
    get:
      summary: Get supplier details
      parameters: [...]
      responses:
        200: {...}
        401: {...}
        404: {...}
    put:
      summary: Update supplier
      requestBody: {...}
      responses:
        200: {...}
        422: {...}
```

## Estimated Timeline
- API-Spec-Design: 25-35 minutes
- Compliance-Review: 10-15 minutes
- **Total**: 35-50 minutes

## Quality Checklist
- [ ] All endpoints documented with examples
- [ ] Request/response schemas use JSON Schema
- [ ] Error codes cover 5+ scenarios
- [ ] Authentication pattern documented
- [ ] Rate limiting documented
- [ ] Compliance requirements listed
- [ ] Code examples provided (3+ languages)
