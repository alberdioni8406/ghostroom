# Ghostroom 2.0

**Enter as a ghost. Leave no trace.**

Ghostroom is an experimental, single-file, temporary anonymous chat built on Nostr.

No accounts. No permanent profiles. No backend. No database.  
Disposable cryptographic identity. Client-side encryption. Ephemeral rooms that die.

```
GitHub → Vercel → index.html → Browser → Nostr relays
```

Live (original v2): https://ghostroom.xyz  
Repository: https://github.com/alberdioni8406/ghostroom

---

## What it is

Ghostroom is a temporary digital place where people:

**arrive → become ghosts → communicate → disappear.**

It combines:

- Old-school cypherpunk aesthetics
- Haunted terminal atmosphere
- Modern browser cryptography (Web Crypto AES-GCM)
- Nostr decentralized relay transport

You do not sign up.  
You arrive.  
You do not make a profile.  
You become a ghost.  
You do not leave a group.  
You vanish.  
Rooms do not last forever.  
They die.

---

## Architecture

**Single HTML file.**  
Everything lives in `index.html`:

- HTML + CSS + JavaScript
- Identity generation
- Room logic
- Encryption
- Nostr pool / subscriptions
- UI

External dependency (CDN only):

- `nostr-tools@2.10.4` (via esm.sh)

No `package.json`. No build step. No server.  
Deploy by dropping `index.html` on Vercel, GitHub Pages, Netlify, or any static host.

---

## Ghost Protocol v0.2

An experimental communication concept (not an industry standard):

1. Disposable identity (session-only keypair)
2. Temporary room
3. Client-side encryption (AES-GCM derived from room code + optional secret)
4. Decentralized Nostr relay transport
5. Ephemeral local state
6. No permanent profile
7. Explicit room death
8. Minimal metadata

Messages are published as Nostr kind-1 events tagged `#t=ghostroom` and `#r=<room-code>`.  
Content is encrypted in the browser before publish whenever a room key is available.

---

## Features (2.0)

- **Ghost birth / death** sequence
- Disposable identity: `GHOST-XXXX` + generated name (e.g. Cipher Wraith)
- Room modes: Normal · Burn · Solo · Two-Ghost · Broadcast
- Room temperature & anonymous presence indicators
- “You are the last ghost” state
- Client-side E2EE (AES-GCM via Web Crypto + PBKDF2)
- Private invite links using URL fragment / path secret (client-side only)
- Find a Ghost (discovers recent active public rooms via relays)
- Ephemeral messages (fade after 5 minutes)
- Typing indicators
- Ghost reactions (`◈ ECHO`, `◈ UNDERSTOOD`, `◈ SILENCE`, …)
- Lightweight commands (`/vanish`, `/help`, `/ghost`, …)
- Relay status & degraded-mode messaging
- Mobile-first, keyboard accessible, reduced-motion support
- Optional debug panel (click logo 5× or `?debug`)

---

## Privacy model (honest)

**What Ghostroom does:**

- No account, no email, no password
- No permanent identity key stored by default
- Private key never leaves the browser memory
- Messages encrypted before hitting relays (when room key is derived)
- Session state is local and temporary

**What Ghostroom cannot prevent:**

- Public relays may retain events
- Other participants can copy or screenshot messages
- Network metadata is visible to relays and network observers
- A compromised device or browser can compromise the session
- “Find a Ghost” reveals that a room had recent activity

Do not treat Ghostroom as a guarantee of anonymity or security beyond this documented model.  
It is experimental software.

---

## Room modes

| Mode       | Behaviour                                      |
|------------|------------------------------------------------|
| NORMAL     | Standard temporary room                        |
| BURN       | Intended to die when last ghost leaves         |
| SOLO       | One ghost maximum                              |
| TWO-GHOST  | Maximum two participants                       |
| BROADCAST  | One speaker, multiple listeners                |

Capacity limits are soft (enforced via presence awareness and UI messaging). True hard enforcement across decentralized relays without a coordinator is intentionally limited.

---

## Deployment

1. Place `index.html` at the root of a GitHub repository (or any static host).
2. Connect the repo to Vercel (or equivalent).
3. Deploy. No environment variables, no build command required.

Optional: point a custom domain at the deployment.

---

## Local testing

Open `index.html` via any static server, e.g.:

```bash
npx serve .
# or
python -m http.server 8080
```

Because the app loads `nostr-tools` from a CDN and opens WebSockets, a simple `file://` open may be blocked by browser CORS / module rules; use a local HTTP server.

---

## Security notes

- All untrusted content is rendered with `textContent` (no `innerHTML` for chat bodies).
- Private key exists only in memory for the tab lifetime.
- Rate limiting is session-local.
- URL fragment / path secrets stay client-side; do not log them.
- Dependencies are loaded from a pinned esm.sh URL; review supply-chain risk for production use.

---

## Limitations

- No central room index → “Find a Ghost” is best-effort over recent relay events.
- Room capacity and Burn-mode death cannot be cryptographically enforced across all relays.
- Encryption is room-key based (shared secret derived from code + optional secret), not per-participant pairwise.
- Relays can still see metadata (who published when, room tags).

---

## Roadmap / future ideas

- Stronger pairwise encryption options
- Better presence without flooding relays
- Optional short-lived NIP-44 style encryption
- Observatory (real aggregate pulse when feasible without a backend)
- More room challenges / temporary rules
- Improved mobile keyboard & safe-area handling

---

## License / spirit

Ghostroom is experimental.  
Keep it weird. Keep it small. Keep it decentralized. Keep it ephemeral.

One file.  
No account.  
No backend.  
No permanent identity.
