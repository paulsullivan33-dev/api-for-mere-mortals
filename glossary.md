# Glossary

| Term | Meaning |
|------|---------|
| API | Application Programming Interface — a contract between programs |
| authentication | Proving who you are (API key, token, OAuth2) |
| authorization | Proving you're *allowed* (happens after authentication) |
| Bearer token | A short-lived credential sent as `Authorization: Bearer ...` |
| body | The data payload of a request or response |
| curl | The command-line HTTP client |
| endpoint | One URL + method combination (`GET /items`) |
| header | Metadata line on a request/response (`Content-Type: ...`) |
| HTTPS | HTTP over TLS — encrypted and authenticated |
| idempotent | Safe to repeat (processing twice = processing once) |
| JSON | JavaScript Object Notation — the data format of APIs |
| method (verb) | GET, POST, PUT, PATCH, DELETE — the action |
| OAuth2 | The "log in with..." delegation flow |
| OpenAPI | A standard file format describing an API |
| paging | Big lists split across requests (`?page=2`) |
| query param | `?key=value` filters on a URL |
| rate limit | How many requests you're allowed per window (429 when exceeded) |
| resource | The thing a URL names (`/items/1`) |
| REST | The dominant API style: nouns in URLs, verbs in methods |
| status code | The 3-digit outcome (`200`, `404`, `500`) |
| TLS | The encryption under HTTPS |
| webhook | The API calls *you*: an event POSTed to your URL |
