# HTTP in 10 Minutes

**HTTP** is the language web APIs speak. Every interaction is a
**request** ("here's what I want") and a **response** ("here's your
answer, and how it went").

## The request

```text
GET /items/1 HTTP/1.1
Host: api.example.com
Accept: application/json
```

Three parts: the **method** (`GET` — what kind of action), the
**path** (`/items/1` — what it's acting on), and **headers**
(metadata: who I am, what formats I accept).

## The methods (verbs)

| Method | Meaning | Analogy |
|--------|---------|---------|
| `GET` | Read something | Looking at the menu |
| `POST` | Create something | Placing an order |
| `PUT` | Replace something | "Redo my order, all of it" |
| `PATCH` | Update part of something | "Change the fries to a salad" |
| `DELETE` | Remove something | Canceling the order |

`GET` is the only one your browser does when you visit a page.
`GET` requests should never *change* anything — they're "safe" to
repeat, refresh, and bookmark. (A `GET` that deletes data is a
design bug.)

## The response

```text
HTTP/1.1 200 OK
Content-Type: application/json

{"id": 1, "name": "Widget"}
```

Three parts: the **status code** (`200` — how it went), **headers**
(`Content-Type` — what the body *is*), and the **body** (the actual
data, here in JSON).

## Status codes: the first digit tells the story

| Range | Meaning | Examples |
|-------|---------|----------|
| 2xx | Success | `200 OK`, `201 Created`, `204 No Content` |
| 3xx | Redirect | `301 Moved`, `304 Not Modified` |
| 4xx | *You* messed up | `400 Bad Request`, `401 Unauthorized`, `404 Not Found` |
| 5xx | *They* messed up | `500 Internal Error`, `503 Unavailable` |

**4xx = your fault, 5xx = their fault.** That's the single most
useful thing in this chapter. `404`? You asked for something that
isn't there. `500`? Their code crashed — nothing you can fix, but
now you know who to tell. (Full tour in [Status
Codes](status-codes.md).)

## Headers: the metadata

Headers are `Name: value` lines on both requests and responses:

| Header | Meaning |
|--------|---------|
| `Content-Type: application/json` | "This body is JSON" |
| `Authorization: Bearer abc123` | "Here's my credential" |
| `Accept: application/json` | "I'd like JSON back, please" |
| `User-Agent: curl/8.5.0` | "I'm curl, version 8.5.0" |

**The #1 beginner API bug:** POSTing JSON without
`Content-Type: application/json`. The server can't tell your body
is JSON and rejects it (or misreads it). When a POST fails
mysteriously, check this header first.

## HTTPS: HTTP with a lock

**HTTPS** is HTTP over TLS encryption. It gives you two things:
**privacy** (no eavesdropper reads the traffic) and **identity**
(the certificate proves you're talking to the real server, not an
impostor). Browsers warn about plain HTTP now; APIs should *always*
use HTTPS in production. (On your own machine while learning,
`http://localhost` is fine — there's no network to snoop.)

**Next:** [REST](rest.md)
