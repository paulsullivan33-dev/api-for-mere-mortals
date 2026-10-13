# Troubleshooting

## The first question: 4xx or 5xx?

- **4xx** — the problem is in *your* request. Re-read what you
  sent (that's what `curl -v` is for).
- **5xx** — the problem is on *their* side. Check their status
  page, retry once with backoff, then report it.

This single distinction directs every debugging session. Start
here, always.

## The greatest hits

**POST fails with 400, body looks fine**

The `Content-Type` header. You sent JSON without declaring it:

```bash
curl -X POST -d '{"name":"x"}' http://.../items        # missing header!
curl -X POST -H "Content-Type: application/json" \
  -d '{"name":"x"}' http://.../items                    # fixed
```

**"It works in the browser but not in curl"**

The browser sends headers and cookies curl doesn't: auth cookies,
`Accept` variants, a session. Open devtools → Network → copy the
failing request as curl — now you see the *actual* request, and the
difference is usually one header.

**401 on every request**

- Wrong scheme? `Basic` vs `Bearer` — the server expects exactly
  one.
- Expired token? Bearer tokens die young; refresh it.
- Typo in the key? Copy-paste it fresh (and check for trailing
  whitespace — the invisible killer).

**Empty response / connection reset**

- The server crashed mid-request (check *their* logs if it's
  yours).
- A proxy/load balancer timed out a slow request — retry; if it
  recurs, the operation needs to be async (return 202, webhook
  when done).

**"SSL certificate problem"**

```text
curl: (60) SSL certificate problem: unable to get local issuer certificate
```

The server's certificate chain is broken or self-signed. For *your*
testing against a dev server: `curl -k` (insecure — never in
scripts that touch production). For a public API: it's their bug —
but check your system clock first, because an expired-looking cert
is sometimes just a wrong date.

**It worked yesterday**

What changed? Their deploy (check the changelog/status page), your
token expired, or your IP got rate-limited (429s in the logs?).
"Yesterday" is a clue — diff against it.

## The debugging sequence

1. `curl -v` — see the actual request and response. Half of all
   mysteries die here.
2. Read the status code class (4xx/5xx) — directs the search.
3. Read the error body — good APIs tell you the field and the
   rule you broke.
4. Pretty-print the JSON (`python3 -m json.tool`) — structure
   bugs hide in one-line blobs.
5. Reproduce minimally — smallest request that fails, then change
   one thing.

**Next:** [Glossary](glossary.md)
