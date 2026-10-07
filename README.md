<p align="center">
  <picture>
    <source media="(prefers-color-scheme: dark)" srcset=".github/assets/hero-dark.png">
    <img src=".github/assets/hero.png" alt="trust.me.at landing page" width="880">
  </picture>
</p>

# trust.me.at

Share a secret, once.

Trust is a one-time secret service: paste a password, a token or a note, and you get a link that stops working once it has been opened. The text is encrypted with AES-256-GCM in your browser before anything is sent, and the key rides in the URL fragment, which browsers never transmit, so the server holds only ciphertext it cannot decrypt. It is for anyone who has to hand a credential to one person without leaving a live copy in a chat log, inbox or ticket, and for teams that need to get credentials out of CI or collect them from customers.

## Features

- **Encrypted in the browser**: the key is generated in your tab and never reaches the server; the server stores a format version, the ciphertext and an IV, and nothing beside it names a recipient, a title or an IP address.
- **Burn on read**: revealing a secret deletes the stored ciphertext in the same step, and a burned, expired or never-existing link all give the same response.
- **Safe from link previews**: opening a link shows a static page and only an explicit reveal burns the secret, so a chat app unfurling it cannot use it up.
- **No account needed**: anonymous secrets are one view and expire within 24 hours; signing in with GitHub or Google raises the view count and expiry and adds a history of ids and timestamps (never contents).
- **Passphrases and burn tokens**: an optional passphrase guards the reveal (repeated guessing destroys the secret), and a one-shot burn token destroys a secret without the link or an account.
- **Ask links**: let someone outside your organisation send you a secret without an account; they encrypt it in their browser and only you can open it.
- **Cookie-free REST API**: push ciphertext from CI and scheduled jobs with a bearer key that is shown once and stored only as a hash.
- **Self-hostable**: storage is your own Redis or Upstash, deployment files ship with the app, and the licence is verified offline with no call home.

## Screenshots

<table>
  <tr>
    <td width="50%"><picture><source media="(prefers-color-scheme: dark)" srcset=".github/assets/pricing-dark.png"><img src=".github/assets/pricing.png" alt="Plans"></picture><br><sub>Plans: anonymous, signed in, Pro and self-hosted limits</sub></td>
  </tr>
</table>

## Built with

- Next.js 16 (App Router), React 19, TypeScript
- Tailwind CSS 4
- Web Crypto (AES-256-GCM) for client-side encryption, Argon2id for passphrases
- Redis or Upstash for secret storage, with an atomic consume for one-winner reveals
- Better Auth (GitHub, Google, and OpenID Connect sign-in)
- oRPC + TanStack Query
- Self-hostable with Docker

## Part of the me.at family

trust.me.at is one of my [`*.me.at`](https://me.at) apps. They share one design system, account, and app shell. Development happens in a private monorepo; this repository is the project's public-facing home.

## License

See [LICENSE](LICENSE).
