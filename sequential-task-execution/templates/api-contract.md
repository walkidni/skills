# Feature Name API Contract

Briefly describe the frontend-facing behavior this contract covers.

Scope of this document:

- endpoint or workflow area 1
- endpoint or workflow area 2
- important exclusion or non-goal, if useful

## Route Summary

- `GET /example`
- `POST /example/{id}`

## General Notes

- State required auth/session behavior.
- State required headers, such as `Accept: application/json`, if applicable.
- State response envelope conventions.
- State resource visibility/filtering rules that affect frontend behavior.
- State any mapper/resource metadata that appears in real responses but is omitted from examples.

## Error Envelope And Codes

Error responses use this shape unless an endpoint states otherwise:

```json
{
  "status": "error",
  "message": "Human-readable error message",
  "code": "ERROR_CODE"
}
```

### Codes Used By This Contract

| HTTP | `code` | Where it applies |
| --- | --- | --- |
| `401` | `UNAUTHORIZED` | Authenticated request without a valid session or token. |
| `403` | `ACCESS_DENIED` | Authenticated user lacks permission for the resource. |
| `404` | `NOT_FOUND` | Resource or route was not found. |
| `405` | `METHOD_NOT_ALLOWED` | Wrong HTTP method for the route. |
| `422` | `VALIDATION_FAILED` | Request validation failed. |

## 1. Endpoint Name

- `METHOD /exact/path/{param}`

Short description of what the endpoint does.

### Path Params

- `param`
  - required
  - describe lookup semantics

### Query Params

- `search`
  - optional
  - describe filtering behavior

### Request

```json
{
  "field": "value"
}
```

### Validation

- `field`
  - required
  - string
  - max `255`

### Response

```json
{
  "status": "success",
  "resource": {
    "id": "resource_id"
  }
}
```

### Errors

- `404` when the resource does not exist or is not visible to the requester.
- `422` when validation fails.

### Notes

- Include frontend-relevant gotchas, ordering guarantees, nullability, pagination, cache behavior, or side effects.
