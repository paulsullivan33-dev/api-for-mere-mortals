# Status Codes

The full registry has dozens. You need twelve.

## Success (2xx)

| Code | Meaning | When |
|------|---------|------|
| `200 OK` | It worked | GETs, updates |
| `201 Created` | Made a new thing | POST that creates (verified in this guide) |
| `204 No Content` | Done, nothing to say | DELETE (verified — empty body is *success*) |

## Your fault (4xx)

| Code | Meaning | What to do |
|------|---------|------------|
| `400 Bad Request` | Malformed request | Check your JSON/body syntax |
| `401 Unauthorized` | Missing/bad credentials | Check the `Authorization` header (verified) |
| `403 Forbidden` | Valid credentials, not allowed | You need different permissions, not a different login |
| `404 Not Found` | No such thing | Check the URL/id (verified) |
| `405 Method Not Allowed` | Right URL, wrong verb | `DELETE` on a read-only endpoint |
| `422 Unprocessable` | Valid JSON, invalid data | Missing `"name"` (verified) — fix the *content* |
| `429 Too Many Requests` | Slow down | Back off and retry (see Rate Limits) |

**401 vs. 403** trips everyone up: 401 = "I don't know who you
are" (authenticate); 403 = "I know who you are, and no"
(authorize). The names are historically backwards — "unauthorized"
really means "unauthenticated" — but the distinction matters.

## Their fault (5xx)

| Code | Meaning | What to do |
|------|---------|------------|
| `500 Internal Server Error` | Their code crashed | Not yours to fix — report it with the request details |
| `502 Bad Gateway` | Their proxy couldn't reach their app | Their infrastructure; retry once, then report |
| `503 Service Unavailable` | Down on purpose (or overloaded) | Retry with backoff; check their status page |

## Reading an error like a developer

A good error response looks like this (verified shape):

```json
{"error": "missing 'name'"}
HTTP 422
```

Three questions, in order:

1. **What class?** 4xx → look at *your* request. 5xx → look at
   *their* status page.
2. **What exactly?** The body's message — `missing 'name'` names
   the field. Vague messages ("error") are the API's bug, not
   yours; try `curl -v` to see the full exchange.
3. **Is it consistent?** Same request, same error → deterministic,
   debuggable. Intermittent → suspect rate limits, deploys, or
   flaky infrastructure.

**Next:** [Versioning and Docs](versioning-and-docs.md)
