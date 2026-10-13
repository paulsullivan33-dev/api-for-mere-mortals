# REST

**REST** is the dominant style for web APIs — not a standard, but a
set of conventions so common that "API" and "REST API" are nearly
synonyms. Learn the conventions and every REST API feels familiar.

## Resources: nouns, not verbs

REST models the world as **resources** (things) addressed by URLs,
with HTTP methods as the verbs:

```text
GET     /items        → list all items
POST    /items        → create an item
GET     /items/1      → get item #1
PUT     /items/1      → replace item #1
DELETE  /items/1      → delete item #1
```

The URL names the *thing*; the method says the *action*. Contrast
with the non-REST style: `GET /getItem?id=1`, `GET /deleteItem?id=1`
— verbs in the URL, everything a GET. It works, but every API
invents its own verbs, and you must learn each one. REST's uniform
interface means you already know how a new API behaves.

## The conventions

1. **Plural nouns:** `/items`, `/users` — not `/item` or
   `/getUsers`.
2. **Hierarchy in the path:** `/users/42/orders` — user 42's orders.
3. **Filtering/sorting in the query string:** `/items?color=red&
   sort=price` — the path identifies the collection; `?...`
   narrows it.
4. **POST returns 201:** creating a thing answers `201 Created`
   (often with the new object, including its assigned id).
5. **DELETE returns 204:** `204 No Content` — "done, nothing to say."
6. **Errors as JSON with a message:** `{"error": "not found"}` plus
   the right status code — not a bare number, not an HTML page.

## Statelessness

Each request carries everything the server needs — no "session
memory" on the server between calls. Your auth token goes on *every*
request (that's why the `Authorization` header exists). This is what
lets APIs scale: any server can handle any request, no sticky
memory. The price: *you* hold the state (the token, the ids).

## What REST isn't

- **Not a law.** You'll meet APIs that break every convention
  above and work fine. Conventions are for *your* benefit as a
  reader of new APIs — and for others reading yours.
- **Not the only style.** **GraphQL** lets the client specify
  exactly which fields it wants (one endpoint, flexible queries);
  **gRPC** uses binary protocols for speed between services. Both
  solve real problems REST handles awkwardly. But REST is what
  you'll meet 90% of the time, so it goes first.

**Next:** [JSON](json.md)
