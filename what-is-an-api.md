# What Is an API?

**The one-sentence version:** An API is an agreement about how two
programs talk to each other: what you can ask for, how to ask, and
what you'll get back.

## The menu analogy

A restaurant menu is an API. It lists what's available (the
*endpoints*), what each dish costs and contains (the *contract*),
and you don't need to know how the kitchen works (the
*implementation*). You order "burger, medium" — the kitchen handles
the rest. If the kitchen changes its grill, your ordering doesn't
change. That's the whole point: **the contract is stable while the
insides can evolve.**

In software: your weather app doesn't contain weather data. It calls
a weather API — "give me the forecast for Dallas" — and displays the
answer. The API owner can rebuild their entire backend; as long as
the contract holds, your app keeps working.

## APIs are everywhere (you just don't see them)

- Your phone's apps → their companies' APIs
- This guide's GitHub Pages sites → GitHub's API built them
- `docker pull` → the registry's API
- Your thermostat app → the manufacturer's API
- Programs calling *libraries* — that's an API too (a function
  signature is a tiny contract)

When people say "API" without qualification today, they usually mean
a **web API**: an API you reach over HTTP. That's this guide's
subject.

## What an API is not

- **Not a user interface.** UIs serve humans; APIs serve programs.
  (Though good API docs *are* a kind of UI — for developers.)
- **Not magic.** An API is just a program listening for requests.
  Everything in this guide demystifies into "a computer running code
  that answers questions."
- **Not automatically public.** Most APIs are private (a company's
  app talking to its own servers). Public ones usually need a key —
  see [Authentication](authentication.md).

## The shape of the guide

You'll learn the *language* APIs speak (HTTP, JSON), the *style*
most use (REST), how to *call* them (curl), how to *prove who you
are* (auth), and finally how to *build* a small one yourself. By the
end, "the API returned a 422" will be a sentence you understand.

**Next:** [HTTP in 10 Minutes](http.md)
