# Your First API Call

**curl** is the command-line way to talk HTTP. If you learn one API
tool, make it this one — it's on every system and shows you exactly
what's happening.

## The simplest call

```bash
curl http://127.0.0.1:8471/items
```

**Check (verified):**

```json
[{"id": 1, "name": "Widget"}, {"id": 2, "name": "Gadget"}]
```

That's a `GET` to `/items` — curl defaults to GET, prints the
response body, and exits. (Run the test server from [Building an
API](building-an-api.md) to follow along, or substitute any real
API URL.)

## Dissecting a POST

```bash
curl -X POST \
  -H "Content-Type: application/json" \
  -d '{"name": "Sprocket"}' \
  http://127.0.0.1:8471/items
```

| Piece | Meaning |
|-------|---------|
| `-X POST` | Use the POST method (default is GET) |
| `-H "Content-Type: application/json"` | Header: "my body is JSON" |
| `-d '{"name": "Sprocket"}'` | The body — `-d` implies POST, but `-X` is explicit |
| The URL | Where it's going |

**Check (verified):**

```json
{"id": 3, "name": "Sprocket"}
```

And the status (curl hides it by default — ask):

```bash
curl -s -w "\nHTTP %{http_code}\n" -X POST \
  -H "Content-Type: application/json" \
  -d '{"name": "Sprocket"}' \
  http://127.0.0.1:8471/items
```

**Check (verified):**

```json
{"id": 4, "name": "Sprocket"}
HTTP 201
```

`201 Created` — the REST convention for "made it." (The id
increments because the server remembers — each run starts fresh.)

## Quoting: single quotes save you

```bash
curl -d '{"name": "Sprocket"}' ...   # single quotes outside: safe
curl -d "{\"name\": \"Sprocket\"}" ... # double quotes: escape hell
```

JSON needs double quotes inside; the shell eats double quotes. So
wrap the whole body in **single quotes** — the shell passes it
through untouched. Same instinct as regex's raw strings.

## DELETE and the quiet success

```bash
curl -s -w "\nHTTP %{http_code}\n" -X DELETE http://127.0.0.1:8471/items/4
```

**Check (verified):**

```text
HTTP 204
```

No body — `204 No Content` means "done, nothing to say." An empty
response to a DELETE is *success*, not failure.

**Next:** [Query Params and Headers](query-params-and-headers.md)
