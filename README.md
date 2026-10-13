# APIs for Mere Mortals

An **API** (Application Programming Interface) is a contract between
programs: "ask me like *this*, and I'll answer like *that*." Every
app on your phone, every website you use, every smart device in your
house talks to APIs constantly. This guide teaches you to *use*
them — and to build a small one yourself.

No CS degree required. Ideas first, then real requests against a
real (tiny) API.

## What this guide covers

- **What an API is** — the contract idea, and the menu analogy
- **HTTP in 10 minutes** — requests, responses, methods, status
  codes, headers
- **REST** — the dominant style: resources, nouns not verbs
- **JSON** — the data format you'll see everywhere
- **Your first API call** — curl, dissected
- **Query params and headers** — `?key=value` and the metadata
- **Authentication** — API keys, Bearer tokens, OAuth2 in two
  minutes
- **Building an API** — a tiny Python server with GET and POST
- **Status codes** — the ones that matter, and reading errors
- **Versioning and docs** — `/v1` and OpenAPI
- **Rate limits and paging** — why big lists come in pages
- **Webhooks** — reverse APIs: they call *you*
- **Troubleshooting** — 4xx vs. 5xx, debugging with `curl -v`

## What it doesn't cover

GraphQL and gRPC (different styles, same ideas — mentioned, not
taught), API business models, or production deployment (auth at
scale, gateways). This guide is about *understanding* APIs and using
them confidently.

## How to use this guide

Every curl example was run against a live test API before shipping
— outputs are real. The test server's code is in [Building an
API](building-an-api.md); run it yourself and follow along.
**Check:** blocks show verified output. **Careful:** marks anything
that sends real data somewhere real.

## Chapters

1. [What Is an API?](what-is-an-api.md)
2. [HTTP in 10 Minutes](http.md)
3. [REST](rest.md)
4. [JSON](json.md)
5. [Your First API Call](first-call.md)
6. [Query Params and Headers](query-params-and-headers.md)
7. [Authentication](authentication.md)
8. [Building an API](building-an-api.md)
9. [Status Codes](status-codes.md)
10. [Versioning and Docs](versioning-and-docs.md)
11. [Rate Limits and Paging](rate-limits-and-paging.md)
12. [Webhooks](webhooks.md)
13. [Troubleshooting](troubleshooting.md)
14. [Glossary](glossary.md)

*Written to the [nine-point writing standard](criteria.md) shared by
every guide in this series.*
