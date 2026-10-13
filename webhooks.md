# Webhooks

Normal APIs: **you call them** when you want something. **Webhooks**
flip it: **they call you** when something happens. "Don't call us,
we'll call you" — as an architecture.

## The idea

You give the provider a URL: "when a payment completes, POST the
details here." They do. Your server receives an ordinary HTTP POST —
a JSON body describing the event — and acts on it. No polling ("are
we there yet?" every 30 seconds), no wasted requests.

| Polling | Webhooks |
|---------|----------|
| You ask repeatedly | They tell you once |
| Latency up to your poll interval | Near-instant |
| Wasted requests when nothing happened | Traffic only on events |
| Simple to implement | You run a reachable server |

Stripe, GitHub, and nearly every payment/CI/chat platform offer
webhooks for exactly this reason.

## Receiving one (it's just a POST endpoint)

```python
def do_POST(self):
    if self.path == "/webhook":
        length = int(self.headers.get("Content-Length", 0))
        event = json.loads(self.rfile.read(length))
        print("Event:", event["type"])   # e.g. "payment.completed"
        self._send(200, {"received": True})
```

That's it — a webhook receiver is a POST endpoint that reads JSON
and answers 200 quickly. (**Quickly** matters: the sender times out
and *retries*, so do slow work — emails, database writes — after
responding, or in a background job.)

## Trust, but verify

Anyone can POST to your URL — including attackers faking events
("payment.completed" for an order nobody paid for). Providers sign
their webhooks:

1. You and the provider share a **secret**.
2. They send the payload plus a **signature header**
   (`X-Signature: sha256=abc...`) — an HMAC of the body with the
   secret.
3. You recompute the HMAC and compare. Match = genuine.

**Never act on an unsigned webhook.** And never trust fields the
sender could forge — after a "payment.completed" event, fetch the
payment's status from the API yourself before shipping the goods.
The webhook is a *notification*; the API is the *truth*.

## The operational realities

- **Retries:** senders retry failed deliveries (non-2xx or
  timeouts) — your handler must be **idempotent** (processing the
  same event twice is harmless). Track event ids you've seen.
- **Ordering isn't guaranteed.** "Created" can arrive after
  "updated." Design for out-of-order events.
- **Your URL must be reachable** — localhost won't cut it for real
  providers (tunnels like ngrok work for testing).

**Next:** [Troubleshooting](troubleshooting.md)
