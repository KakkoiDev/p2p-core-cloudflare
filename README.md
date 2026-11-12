# Cloudflare deployment for p2p-core

This repository holds the deployment configuration for the WebSocket relay.
It bundles the Worker directly from [p2p-core/cloudflare](https://github.com/KakkoiDev/p2p-core/tree/main/cloudflare).

## Deploy

Enable Workers Free in Cloudflare and register a `workers.dev` subdomain
if prompted. With Node.js 22 or newer, clone the repositories side by side.

```sh
git clone https://github.com/KakkoiDev/p2p-core.git
git clone https://github.com/KakkoiDev/p2p-core-cloudflare.git
cd p2p-core-cloudflare
node --test ../p2p-core/cloudflare/protocol.test.mjs
npx wrangler@4 login
npx wrangler@4 deploy
```

The configuration already allows `https://mini-flow.kakkoi.dev`. Add the
exact HTTPS origins for your other apps to `ALLOWED_ORIGINS` in
`wrangler.toml`, separated by commas. Keep the same Worker name and
Cloudflare account to update an existing deployment.

## Connect Mini Flow

1. Copy the deployed Worker URL and change its scheme to `wss://`.
2. Set `liveRelayUrl` in
   [mini-flow-live.js](https://github.com/KakkoiDev/mini-flow/blob/main/mini-flow-live.js)
   to that origin and publish Mini Flow.
3. Start a fresh live session, open the invite in Safari, and verify that
   an edit reaches the other browser in each direction.

Each p2p-core app can use the same Worker URL with a distinct `app` name.
See the [relay documentation](https://github.com/KakkoiDev/p2p-core/tree/main/cloudflare)
for client configuration and limits. Use a relay you trust because the
Worker can read forwarded board messages.
