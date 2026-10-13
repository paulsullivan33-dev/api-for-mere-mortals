# Query Params and Headers

The URL path names the resource. **Query params** narrow it down;
**headers** carry metadata about the request itself.

## Query params: questions about the collection

```text
/items?color=red&sort=price&limit=10
```

After the `?`: `key=value` pairs joined by `&`. They're for
*filtering, sorting, and paging* — never for the resource's
identity (that's the path's job):

```text
/items/42          → the item (path)
/items?color=red   → items, but only red ones (query)
```

```bash
curl "http://127.0.0.1:8471/items?limit=1"
```

**Quote the URL** — the shell treats `&` as "run in background"
and `?` as a wildcard. Unquoted, your query silently becomes two
commands. Single or double quotes both work; pick one and be
consistent.

Special characters get **percent-encoded**: a space becomes `%20`,
`&` inside a value becomes `%26`. Most HTTP libraries (Python's
`requests`, curl's `--data-urlencode`) do this for you — hand-rolled
URLs with raw spaces are a classic 400.

## Headers you send

| Header | Purpose | Example |
|--------|---------|---------|
| `Authorization` | Who you are | `Bearer abc123` |
| `Content-Type` | What your body is | `application/json` |
| `Accept` | What you want back | `application/json` |
| `User-Agent` | Which client you are | (curl sends its own) |

```bash
curl -H "Authorization: Bearer secret123" http://127.0.0.1:8471/secret
```

**Check (verified):**

```json
{"msg": "ok"}
HTTP 200
```

And without the header:

```bash
curl -s -w "\nHTTP %{http_code}\n" http://127.0.0.1:8471/secret
```

**Check (verified):**

```json
{"error": "unauthorized"}
HTTP 401
```

Same URL, different header, different universe. Headers are
invisible in the browser address bar, which is why API debugging
lives in curl and devtools, not in pasting URLs.

## Headers you receive

```bash
curl -v http://127.0.0.1:8471/items 2>&1 | grep -E "^[<>]"
```

**Check (verified shape):**

```text
> GET /items HTTP/1.1
> Host: 127.0.0.1:8471
> User-Agent: curl/8.5.0
> Accept: */*
>
< HTTP/1.0 200 OK
< Content-Type: application/json
```

`>` lines are what you sent; `<` lines are what came back. This is
the single most illuminating view in API work: the *actual*
request, not what you *thought* you sent.

**Next:** [Authentication](authentication.md)
