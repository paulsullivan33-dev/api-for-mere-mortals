# JSON

**JSON** (JavaScript Object Notation) is the data format of APIs —
how the actual *information* travels inside HTTP bodies. It's text
humans can read and programs can parse, which is why it won.

## The whole format in 30 seconds

```json
{
  "id": 1,
  "name": "Widget",
  "price": 9.99,
  "in_stock": true,
  "tags": ["sale", "new"],
  "supplier": null
}
```

Six kinds of values: **strings** (`"Widget"` — always double
quotes), **numbers** (`9.99` — no quotes), **booleans** (`true`/
`false`), **null**, **arrays** (`[...]`), **objects** (`{...}`).
Objects nest inside objects; arrays hold anything. That's the whole
format.

## Reading it

```python
import json

data = json.loads('{"id": 1, "name": "Widget"}')
print(data["name"])   # Widget
```

**Check (verified):** `json.loads` turns the text into a Python
dictionary; `data["name"]` is `'Widget'`.

```python
print(json.dumps({"id": 1, "name": "Widget"}))
# {"id": 1, "name": "Widget"}
```

`json.dumps` goes the other way — dictionary to text, for sending.

## The gotchas

- **Double quotes only.** `'name'` is not valid JSON — parsers
  reject it. (Python dicts allow single quotes; JSON doesn't.)
- **No comments.** JSON has no comment syntax. APIs document
  elsewhere.
- **No trailing commas.** `{"a": 1,}` is invalid — the last item
  gets no comma.
- **Numbers are numbers.** `"price": "9.99"` (a string) vs
  `"price": 9.99` (a number) — APIs care, and mixing them up is a
  classic 422.

When an API rejects your body, paste it into a JSON validator
(python: `json.loads(your_text)`) before blaming the API. Malformed
JSON is the cause more often than anyone admits.

## Pretty-printing API responses

Raw API output is usually one long line. Make it readable:

```bash
curl -s http://127.0.0.1:8471/items | python3 -m json.tool
```

**Check (verified):**

```json
[
    {
        "id": 1,
        "name": "Widget"
    },
    {
        "id": 2,
        "name": "Gadget"
    }
]
```

`python3 -m json.tool` is the fastest JSON pretty-printer you own —
no installs, always there.

**Next:** [Your First API Call](first-call.md)
