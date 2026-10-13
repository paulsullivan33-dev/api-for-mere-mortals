# Authentication

APIs need to know *who's asking* — to bill you, to show your data,
to keep strangers out. The methods, simplest first.

## API keys: the password approach

```bash
curl -H "X-API-Key: abc123" https://api.example.com/items
```

A long random string identifies you. Simple, and fine for
server-to-server calls. **Treat it like a password**: never commit
it to git, never put it in a URL (URLs get logged everywhere),
rotate it if exposed. Keys in client-side JavaScript are visible to
everyone — that's not a leak, it's architecture; scope such keys
narrowly.

## Bearer tokens: the temporary pass

```bash
curl -H "Authorization: Bearer eyJhbG..." https://api.example.com/items
```

Same shape as a key, but **short-lived** (minutes to hours) and
obtained via a login flow. When it expires, you get a new one
instead of reusing a password. "Bearer" means "whoever bears this
token gets in" — which is exactly why it must travel over HTTPS and
expire quickly. If it's stolen, the thief *is* you until it dies.

## Basic auth: the legacy

```bash
curl -u paul:s3cret https://api.example.com/items
```

Username and password, base64-encoded into an `Authorization`
header. Base64 is *encoding*, not encryption — trivially reversible.
Only ever over HTTPS, and prefer tokens for anything new. You'll
meet it on old hardware (routers, cameras) and some internal tools.

## OAuth2: "log in with..."

The flow behind "Log in with Google/GitHub":

1. The app sends *you* to Google: "this app wants your email."
2. You approve. Google sends the app a **code**.
3. The app trades the code (plus its own secret) for **tokens**.

You never give the app your Google password — it gets a scoped,
revocable token instead. That's the security win: the app can't
change your password, and you can revoke its access without changing
anything else. Implementing OAuth2 yourself is genuinely complex —
use a library. *Using* one as a developer mostly means "redirect,
handle the callback, store the token."

## What to use when

| Situation | Use |
|-----------|-----|
| Your script → your server | API key or Bearer token |
| A user logging into your app | OAuth2 (don't build your own) |
| Quick internal tool | API key, rotated occasionally |
| Browser JavaScript | Short-lived tokens, narrow scope |

## The rules that prevent disasters

1. **HTTPS always** for anything with credentials. No exceptions.
2. **Never commit secrets.** Use environment variables
   (`API_KEY=... python app.py`), never hardcoded strings.
3. **Least privilege.** A read-only key for the dashboard script;
   the write key lives with the deploy system.
4. **Have a rotation story.** Keys leak (logs, screenshots, chat
   pastes). Know *how* you'd replace one before you need to.

**Next:** [Building an API](building-an-api.md)
