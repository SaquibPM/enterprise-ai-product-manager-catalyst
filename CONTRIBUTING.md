# Contributing to Enterprise AI Product Manager Catalyst

Thank you for your interest in contributing. This marketplace is built for enterprise PMs who care about quality over quantity.

## How to Contribute

### Adding a New Skill

1. Identify which plugin the skill belongs to based on the [activity mapping](README.md#activity-coverage)
2. Create a new folder under the appropriate `plugin/skills/` directory
3. Write a `SKILL.md` following the quality standards below
4. Add at least one eval test case in `evals/evals.json`
5. Submit a PR with a clear description of the PM activity this skill addresses

### Skill Quality Standards

Every skill must include:

- **YAML frontmatter** with `name` and a trigger-optimized `description`
- **Purpose** — what this skill does and when to use it (one paragraph)
- **Key Concepts** — specific frameworks with citations, not generic advice
- **Application** — step-by-step instructions with embedded framework logic
- **Examples** — at least one complete worked example showing input → output
- **Common Pitfalls** — specific anti-patterns with explanations
- **References** — related skills and external resources

### What "Enterprise" Means Here

Skills must reflect enterprise B2B context. That means:

- Compliance sections in PRDs (SOC 2, GDPR, HIPAA)
- Multi-tenant considerations in technical decisions
- Buyer vs. user persona distinction in research and specs
- Procurement cycle awareness in launch and pricing
- Audit trail and data residency in AI features

Generic PM advice that could apply to any consumer app does not meet our bar.

### Adding a Command

1. Create a folder under the appropriate `plugin/commands/` directory
2. Write a command markdown file documenting the skill chain
3. Add at least one sample output in the `examples/` subfolder
4. Document parallelization opportunities if applicable

### Adding a Workflow

1. Add a markdown file to the `workflows/` directory
2. Document the full skill chain across plugin boundaries
3. Specify data handoff between skills
4. Note which steps can run in parallel vs. must be sequential

## Code of Conduct

Be respectful, be specific, be enterprise-grade. PRs that add generic advice will be asked to revise.

## License

By contributing, you agree that your contributions will be licensed under the Apache 2.0 License.
