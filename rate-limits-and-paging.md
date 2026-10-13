# Rate Limits and Paging

Two facts of API life: providers limit how fast you can ask, and
big lists come in pages. Both are the provider protecting *their*
servers from *your* enthusiasm.

## Rate limits

```text
HTTP 429 Too Many Requests
Retry-After: 60
```

Exceed the quota and you get `429` — plus (on good APIs) headers
telling you the quota and when it resets:

```text
X-RateLimit-Limit: 1000
X-RateLimit-Remaining: 0
X-RateLimit-Reset: 1699999999
```

**How to behave:**

1. **Read the headers.** Don't guess the limit; it's printed.
2. **Back off exponentially.** Wait 1s, then 2s, then 4s... not a
   hammering retry loop.
3. **Honor `Retry-After`.** It says exactly when to come back.
4. **Cache.** The cheapest request is the one you don't make —
   store responses for their sensible lifetime.

Hitting 429 isn't an error in *your* code — it's the API saying
"pace yourself." Design for it from the start; bolting it on later
is painful.

## Paging: lists come in pages

No sane API returns a million rows at once. Instead:

```text
GET /items?page=2&per_page=50
```

```json
{
  "items": [ ...50 items... ],
  "page": 2,
  "per_page": 50,
  "total": 1250
}
```

Or **cursor-based** (better for live data — page numbers shift as
rows are added):

```text
GET /items?cursor=eyJpZCI6NTB9&limit=50
```

```json
{
  "items": [ ... ],
  "next_cursor": "eyJpZCI6MTAwfQ"
}
```

Follow `next_cursor` until it's absent. Cursor paging never skips
or duplicates rows when the underlying data changes mid-walk —
offset paging (`page=2`) can do both.

## The polite client checklist

- Page through lists; never assume one response has everything.
- Respect 429s with backoff, not retries-at-full-speed.
- Set a `User-Agent` identifying your app — providers debugging
  *their* overload will thank you, and some require it.
- If you need bulk data regularly, ask about a bulk endpoint or
  export — hammering a paged API for a data warehouse is how you
  get your key revoked.

**Next:** [Webhooks](webhooks.md)
