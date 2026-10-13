# Building an API

Time to be the kitchen, not the customer. Here's a complete (tiny)
API in Python's standard library — no installs. Every curl example
in this guide was verified against exactly this server.

```python
import json
from http.server import BaseHTTPRequestHandler, HTTPServer

ITEMS = [{"id": 1, "name": "Widget"}, {"id": 2, "name": "Gadget"}]

class Handler(BaseHTTPRequestHandler):
    def _send(self, code, obj=None):
        body = json.dumps(obj).encode() if obj is not None else b""
        self.send_response(code)
        self.send_header("Content-Type", "application/json")
        self.send_header("Content-Length", str(len(body)))
        self.end_headers()
        self.wfile.write(body)

    def do_GET(self):
        if self.path == "/items":
            self._send(200, ITEMS)
        elif self.path.startswith("/items/"):
            try:
                item_id = int(self.path.split("/")[2])
            except ValueError:
                return self._send(400, {"error": "id must be a number"})
            item = next((i for i in ITEMS if i["id"] == item_id), None)
            self._send(200, item) if item else self._send(404, {"error": "not found"})
        else:
            self._send(404, {"error": "not found"})

    def do_POST(self):
        if self.path != "/items":
            return self._send(404, {"error": "not found"})
        length = int(self.headers.get("Content-Length", 0))
        try:
            data = json.loads(self.rfile.read(length) or b"{}")
        except json.JSONDecodeError:
            return self._send(400, {"error": "invalid JSON"})
        if "name" not in data:
            return self._send(422, {"error": "missing 'name'"})
        item = {"id": max(i["id"] for i in ITEMS) + 1, "name": data["name"]}
        ITEMS.append(item)
        self._send(201, item)

    def log_message(self, *a):
        pass  # quiet the request logging

HTTPServer(("127.0.0.1", 8471), Handler).serve_forever()
```

Save as `server.py`, run `python3 server.py`, and it's live at
`http://127.0.0.1:8471`. (Real projects use Flask/FastAPI/Express —
same ideas, less plumbing. This version shows the machinery with
zero dependencies.)

## What to notice

- **Methods are just `do_GET`, `do_POST`...** — the framework (even
  this tiny one) routes by HTTP method. Your job: fill in each one.
- **Status codes are chosen deliberately** — `201` for created,
  `404` for missing, `422` for "understood but invalid." The client
  *programs against these*, so pick honestly.
- **Errors are JSON with a message** — `{"error": "not found"}`.
  A bare status code forces the client to guess; a message tells
  them.
- **Validation before storage** — the POST checks for `"name"`
  *before* appending. Garbage in, garbage stored is how you get
  3 AM pages.

## The design checklist (for your real API)

1. Nouns in paths, verbs in methods ([REST](rest.md)).
2. Right status code for every outcome ([Status Codes](status-codes.md)).
3. Errors as `{"error": "human-readable message"}`.
4. Validate input; reject with `4xx`, never store garbage.
5. Auth on everything except what's deliberately public.
6. Document it ([Versioning and Docs](versioning-and-docs.md)) —
   an undocumented API is a rumor.

**Next:** [Status Codes](status-codes.md)
