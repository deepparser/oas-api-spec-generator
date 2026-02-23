# OAS API Spec Generator

A Claude skill for generating **OpenAPI 3.2.0 specifications** for third-party APIs. Point it at any external API — Microsoft, Google, OpenAI, Anthropic, Stripe, GitHub, Slack, AWS, and more — and get a production-quality OAS document.

## Features

- **OAS 3.2.0** — generates specs using the latest OpenAPI standard, including structured tags, mediaTypes, and webhooks
- **15+ built-in provider profiles** — pre-configured auth schemes, base URLs, and endpoint patterns for popular APIs
- **Streaming support** — Server-Sent Events (SSE) and JSON Lines (NDJSON) for AI and real-time APIs
- **Multi-auth patterns** — Bearer tokens, API keys, OAuth 2.0, AWS Signature V4, and custom header schemes
- **Pagination & error handling** — cursor-based, offset-based, and OData pagination with standardized error schemas
- **Multi-file organization** — automatically splits large specs (50+ endpoints) into modular files
- **Validation** — checks specs against 11 critical rules and 10 best-practice recommendations

## Supported Providers

| Provider | Auth Pattern |
|---|---|
| OpenAI | Bearer token |
| Anthropic | Custom header (`x-api-key`) + versioning |
| Google AI / Gemini | API key, OAuth 2.0 |
| Microsoft Graph | OAuth 2.0, OData |
| Stripe | Bearer token, form-encoded |
| GitHub REST | PAT / Bearer token |
| Slack Web API | Bot / User tokens |
| AWS | Signature V4 |
| Cloudflare | Bearer token |
| Vercel | Bearer token |
| Twilio | Basic auth |
| SendGrid | Bearer token |

…and any other REST API with accessible documentation.

## Usage

Invoke the skill in Claude with any of these trigger phrases:

```
create OAS for <provider>
generate openapi spec for <provider>
openapi for <provider>
swagger spec for <provider>
API spec for <provider>
```

### Workflow

1. The skill checks for an official OpenAPI spec from the provider
2. If one exists, it upgrades it to OAS 3.2.0; otherwise it fetches the provider's API docs
3. Endpoints, parameters, request/response bodies, and auth methods are extracted
4. A YAML spec is generated following OAS 3.2.0 conventions
5. The spec is validated against the built-in rule set
6. Output is saved to `specs/{provider}/openapi.yaml`

### Output Structure

```
specs/
└── {provider}/
    ├── openapi.yaml           # Entry document
    ├── paths/
    │   └── {resource}.yaml    # Per-resource paths
    ├── components/
    │   ├── schemas/
    │   ├── parameters/
    │   ├── responses/
    │   └── securitySchemes/
    └── README.md
```

For smaller APIs the entire spec lives in a single `openapi.yaml` file.

## Installation

Add this skill to your Claude Code configuration:

```bash
claude install-skill /path/to/oas-api-spec-generator
```

Or reference `SKILL.md` directly from your project.

## Validation Rules

### Must pass

- OpenAPI version is `3.2.0`
- `info.title` and `info.version` present
- Valid `$ref` targets
- Unique `operationId` values
- Required properties listed in `required` array
- Path parameters defined in the operation
- Security schemes declared before use
- Lowercase HTTP methods
- String status codes (`'200'`, not `200`)

### Should pass

- Every operation has an `operationId`
- Descriptions on operations, parameters, and schemas
- Response definitions for all operations
- Schema reuse via `$ref`
- Error response schemas
- Pagination documented where applicable
- Rate-limit headers
- Streaming responses use proper media types
- Tags for endpoint grouping
- Examples provided

## License

[MIT](LICENSE)
