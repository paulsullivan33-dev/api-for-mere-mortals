# Versioning and Docs

APIs change. Clients depend on the old behavior. **Versioning** is
how both survive.

## Versioning: put it in the URL

```text
https://api.example.com/v1/items
https://api.example.com/v2/items
```

The `/v1` prefix is the dominant convention. When v2 changes
something breaking, v1 keeps working — old clients don't wake up
broken. Rules of the road:

- **Never break v1.** Add fields freely (clients ignore unknown
  fields); never rename or remove them in a released version.
- **Bump the version for breaking changes** — renamed fields,
  changed types, removed endpoints.
- **Sunset with notice**, not surprise: announce deprecation,
  give months, then turn it off.

Alternatives exist (a `Api-Version: 2` header, content negotiation)
— the URL version is the most visible and debuggable, which is why
it won.

## Docs: the API's user interface

Nobody reads code to learn your API; they read docs. The good ones
have:

1. **Every endpoint**: method, path, params, auth, example request,
   example response (like this guide's chapters).
2. **Every error**: which status codes, what the bodies look like.
3. **A changelog**: what changed in v2, and how to migrate.

## OpenAPI: docs as a file

**OpenAPI** (formerly Swagger) describes an API in YAML/JSON —
endpoints, schemas, auth — precisely enough that tools generate
docs, client libraries, and test stubs from it. A fragment:

```yaml
paths:
  /items/{id}:
    get:
      parameters:
        - name: id
          in: path
          required: true
          schema: { type: integer }
      responses:
        '200':
          description: The item
        '404':
          description: Not found
```

You don't need to memorize the format — you need to know it
*exists*. When evaluating an API, "do they publish an OpenAPI
spec?" predicts documentation quality better than anything else.
When building one, writing the spec first (contract-first design)
forces you to think before you code.

**Next:** [Rate Limits and Paging](rate-limits-and-paging.md)
