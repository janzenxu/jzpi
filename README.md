# JZPI

JZPI is a personal web workspace for the [pi coding agent](https://github.com/earendil-works/pi), forked from [agegr/pi-web](https://github.com/agegr/pi-web).

It runs locally, shares pi's configuration and session files, and provides a browser/PWA interface for working with sessions, models, project files, skills, plugins, and Git worktrees.

## Quick Start

Requires Node.js 22.19.0 or newer.

```bash
npm ci
npm run dev
```

Open [http://127.0.0.1:30142](http://127.0.0.1:30142).

JZPI is designed to run in WSL/Linux and can be installed as an Edge PWA on Windows.

## Development

```bash
npm test
node_modules/.bin/tsc --noEmit
npm run lint
```

Do not run `next build` during normal development because its output can interfere with the development server.

## Security

JZPI can execute commands and access project files. It listens on `127.0.0.1` by default. Do not expose it directly to the internet; use authentication together with trusted HTTPS or a VPN when remote access is required.

## License

[MIT](./LICENSE)
