# Kuni Front — running your OpenFront website

This fork is an independent community build of the **real** OpenFront client and game server. It is **not** the old iframe wrapper.

## What is already customized

- "KUNI FRONT" floating site label on larger screens (hidden while playing or when another page is open).
- Fullscreen button and a source link.
- Custom browser-tab title.
- The underlying OpenFront game, maps, and server remain part of the upstream project.

The copyright/legal notices in the original OpenFront UI must remain visible. This site is not affiliated with the original OpenFront developers.

## Run the site on your computer (Windows)

1. Install Git and **Node.js 24.15.0 or newer within Node 24**, plus **npm 12.1.0 or newer within npm 12**.
2. Open PowerShell and enter:

   ```powershell
   git clone https://github.com/kuni7-gif/OpenFrontIO.git
   cd OpenFrontIO
   npm run inst
   npm run dev
   ```

3. Open the local address Vite prints in the terminal. The game client and local game server start together.

Use `npm run inst` rather than plain `npm install`, as instructed by the upstream README.

You can also use the existing `Dockerfile` to build and run the application in a host that supports Docker and WebSockets.

## Why GitHub Pages alone will not work

This is a TypeScript/Vite application with an Express/Node game server, background workers, HTTP APIs and WebSockets. A static `.html` page or GitHub Pages site cannot provide those parts.

## Public hosting

This requires a host that can:

- Build the full repository using its included `Dockerfile` (Node 24 + npm 12).
- Keep a Node/Express multiplayer server and worker processes running.
- Route HTTP and WebSocket traffic through the Docker image's Nginx server (inside container, port **80**).
- Provide sufficient memory/CPU and network bandwidth.
- Attach a public hostname and HTTPS.

The original `Dockerfile` starts Nginx + the game master + workers. It exposes the service through Nginx on container port 80. Ensure your host routes the public HTTP service to **port 80**. Some hosts use a dynamic `PORT` requirement, which needs host-specific adaptation.

## Important limitation: account API is separate

**The public OpenFront repository does not include OpenFront's complete account/matchmaking API.** It expects the account API at `https://api.<DOMAIN>` outside local development. This includes account authentication, join verification, server discovery, and certain matchmaking flows.

For local development, the upstream `npm run dev` script uses `GAME_ENV=dev` and local test-server defaults. Some API-driven online features may be unavailable without the separate API.

**Do not use the development bypass values as production credentials.** To run an independent public multiplayer service, you must implement/host an account/API component compatible with the game's client and server contracts, configure proper security and identity verification, and supply real environment configuration. The existing game server alone is not a guarantee that public matchmaking works.

Do not claim that Kuni Front is connected to official OpenFront multiplayer servers or user accounts unless those services are expressly configured and permitted.

## Next practical step

Set up a Docker-capable web hosting provider with a public HTTP service on container port 80. Add the repository `kuni7-gif/OpenFrontIO` as the deployment source. The frontend and game server code are already together in this fork; full independent public multiplayer additionally needs the API dependencies described above.

## Source and licensing

Original: https://github.com/openfrontio/OpenFrontIO

Fork: https://github.com/kuni7-gif/OpenFrontIO

OpenFront program source: GNU AGPL v3. Assets have separate CC BY-SA or proprietary restrictions; consult `LICENSE`, `LICENSE-ASSETS`, `LICENSING.md`, and `proprietary/LICENSE`. Retain the upstream OpenFront copyright notices and make the modified source available in accordance with applicable license requirements.
