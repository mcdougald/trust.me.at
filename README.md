<p align="center">
  <picture>
    <source media="(prefers-color-scheme: dark)" srcset=".github/assets/hero-dark.png">
    <img src=".github/assets/hero.png" alt="trust.me.at landing page" width="880">
  </picture>
</p>

<h1 align="center">trust.me.at</h1>

<p align="center"><strong>Keep your secrets. Share them safely.</strong></p>

<p align="center">
  <img src="https://img.shields.io/badge/status-live-171717?style=flat-square" alt="Status: live">
  <img src="https://img.shields.io/badge/license-source--available-404040?style=flat-square" alt="License: source-available">
  <img src="https://img.shields.io/badge/self--host-supported-525252?style=flat-square" alt="Self-host: supported">
  <img src="https://img.shields.io/badge/part%20of-me.at-737373?style=flat-square" alt="Part of me.at">
</p>

<p align="center">
  <a href="https://trust.me.at">Website</a> ·
  <a href="#features">Features</a> ·
  <a href="#how-it-works">How it works</a> ·
  <a href="#screenshots">Screenshots</a> ·
  <a href="#built-with">Built with</a>
</p>

API keys live in a `.env` on one laptop, recovery codes in a notes app, shared passwords in a pinned chat message, and every so often one of them gets pasted into a ticket that keeps it readable for years. I built trust.me.at as a zero-knowledge secrets manager: a vault for passwords, API keys and env vars, encrypted with AES-256-GCM in your browser under a key only your passphrase or recovery key opens, plus a way to hand any of them to someone as a link that stops working once it has been read. My server holds ciphertext and never receives a key. It also gets credentials out of CI, and in from customers, without anyone making an account.

<p align="center">
  <picture>
    <source media="(prefers-color-scheme: dark)" srcset=".github/assets/overview-dark.svg">
    <img src=".github/assets/overview.svg" alt="trust.me.at at a glance: encrypted in your browser, gone after one view, safe from link previews, no account or an API" width="880">
  </picture>
</p>

## Why trust.me.at

| The usual way to keep or share a credential | trust.me.at |
| --- | --- |
| A `.env` on one laptop, a notes app, a pinned message | An encrypted vault with search, tags, environments and version history |
| Pasted into a chat, an inbox or a ticket, where it stays | A link that stops working once it has been opened |
| The service can read what it stores | Encrypted in your browser; the server holds only ciphertext |
| A chat app unfurling the link can use it up | Opening shows a static page, and only an explicit reveal burns it |
| Getting one back from a customer means an account or an email thread | An ask link lets them encrypt it for you, no account needed |

## Features

- **An encrypted vault**: passwords, API keys, env vars and notes, sealed in your browser under a vault key wrapped by your passphrase (PBKDF2-SHA-256, 600,000 iterations) and by a recovery key shown once. Names, tags and notes are encrypted too; the server stores ids, revision numbers, timestamps and ciphertext. Free accounts hold 50 items.
- **Version history**: the last 10 versions of every item are kept, on every plan, and any of them can be restored.
- **`.env` in and out**: paste a `.env` file to import it, and export any filtered set as `.env` or JSON. Both happen in the browser, alongside a generator for new random secrets.
- **Share from the vault**: any item becomes a one-time link encrypted under its own fresh key. The vault locks itself after 15 idle minutes.
- **No recovery without you**: lose both your passphrase and your recovery key and nobody, including me, can open the vault. That is the price of a server that never holds a key.
- **Encrypted in the browser**: the key is generated in your tab and never reaches the server. The server stores a format version, the ciphertext and an IV, and nothing beside it names a recipient, a title or an IP address.
- **Burn on read**: revealing a secret deletes the stored ciphertext in the same step, and a burned, expired or never-existing link all give the same response.
- **Safe from link previews**: opening a link shows a static page and only an explicit reveal burns the secret, so a chat app unfurling it cannot use it up.
- **No account needed**: anonymous secrets are one view and expire within 24 hours. Signing in with GitHub or Google raises the view count and expiry and adds a history of ids and timestamps, never contents.
- **Passphrases and burn tokens**: an optional passphrase guards the reveal (repeated guessing destroys the secret), and a one-shot burn token destroys a secret without the link or an account.
- **Ask links**: someone outside your organisation can send you a secret without an account. They encrypt it in their browser and only you can open it.
- **Cookie-free REST API**: push ciphertext from CI and scheduled jobs with a bearer key that is shown once and stored only as a hash.
- **Self-hostable**: storage is your own Redis or Upstash, deployment files ship with the app, and the licence is verified offline with no call home.

## How it works

<p align="center">
  <picture>
    <source media="(prefers-color-scheme: dark)" srcset=".github/assets/flow-dark.svg">
    <img src=".github/assets/flow.svg" alt="How trust.me.at works: encrypt, store, share, reveal, burn" width="880">
  </picture>
</p>

Your browser encrypts everything with AES-256-GCM before anything is sent. Vault items are sealed under a vault key that only your passphrase or recovery key unwraps, and the server stores the two wrapped copies of that key beside each item's ciphertext. To share an item, it is encrypted again under a fresh key that travels in the URL fragment, the part of a link browsers never transmit, so whoever holds the link holds the key and nobody else does. A reveal is an explicit click, and the stored ciphertext is deleted in the same atomic step, so a race between two readers has exactly one winner.

## Screenshots

<table>
  <tr>
    <td width="50%"><picture><source media="(prefers-color-scheme: dark)" srcset=".github/assets/hero-dark.png"><img src=".github/assets/hero.png" alt="Landing page"></picture><br><sub>The landing page: the vault, and a no-account form that turns a secret into a one-time link.</sub></td>
    <td width="50%"><picture><source media="(prefers-color-scheme: dark)" srcset=".github/assets/pricing-dark.png"><img src=".github/assets/pricing.png" alt="Plans"></picture><br><sub>Plans: the anonymous, signed-in, Pro and self-hosted limits.</sub></td>
  </tr>
</table>

## Built with

<p>
  <img src="https://img.shields.io/badge/Next.js-16-000000?style=flat-square&logo=nextdotjs&logoColor=white" alt="Next.js 16">
  <img src="https://img.shields.io/badge/React-19-171717?style=flat-square&logo=react&logoColor=white" alt="React 19">
  <img src="https://img.shields.io/badge/TypeScript-5-262626?style=flat-square&logo=typescript&logoColor=white" alt="TypeScript">
  <img src="https://img.shields.io/badge/Tailwind%20CSS-4-404040?style=flat-square&logo=tailwindcss&logoColor=white" alt="Tailwind CSS 4">
  <img src="https://img.shields.io/badge/Redis-Upstash-525252?style=flat-square&logo=redis&logoColor=white" alt="Redis or Upstash">
  <img src="https://img.shields.io/badge/Docker-self--host-737373?style=flat-square&logo=docker&logoColor=white" alt="Self-hostable with Docker">
</p>

- Next.js 16 (App Router), React 19, TypeScript
- Tailwind CSS 4
- Web Crypto (AES-256-GCM, PBKDF2) for client-side encryption, Argon2id for share-link passphrases
- Redis or Upstash for vault and secret storage, with compare-and-set edits and an atomic consume for one-winner reveals
- Better Auth (GitHub, Google, and OpenID Connect sign-in)
- oRPC + TanStack Query
- Self-hostable with Docker

## Part of the me.at family

trust.me.at is one of my [`*.me.at`](https://me.at) apps. They share one design system, account, and app shell. Development happens in a private monorepo; this repository is the project's public-facing home.

## License

Source-available. See [LICENSE](LICENSE).
