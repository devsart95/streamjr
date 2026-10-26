# CONTEXT — StreamIPTV
> Actualizado: 2026-10-26

## Estado
Demo de un reproductor IPTV web estilo Netflix: JS vanilla + HLS.js + PWA (`sw.js`), relay Express
(`lib/streamflix-app.js`), app de escritorio Electron (`desktop/`) y una plantilla de relay en
Cloudflare Worker (`worker/`). Lee las listas públicas `iptv-org.github.io/iptv/countries/in.m3u`
(India) e `iptv-org.github.io/iptv/index.m3u` (global) desde el navegador (`js/fixed-player.js`).
- Fork de `chriz-3656/streamflix`: `origin` = `devsart95/streamjr` (público). El original sigue en
  `fcefdc6` (2026-02-19, último commit del autor), el mismo que este repo tenía antes de su
  documentación: el fork no está atrasado (`gh api repos/chriz-3656/streamflix/commits/main --jq .sha`).
- Versión: `package.json` dice 2.3.0; el README presenta la v2.5 (changelog del 2026-02-18) como
  la actual. No coinciden.
- Sin tests ni linter.

## Cómo se levanta
- `npm start` (= `node server.js`): http://localhost:8080, o el puerto de `PORT`. Probado el
  2026-10-26 con `PORT=18943`: `GET /` → 200 y `GET /api/relay/health` →
  `{"ok":true,"service":"stream-relay"}`.
- `npm run desktop`: Electron, con el relay local en `127.0.0.1` y puerto dinámico. Sin verificar:
  correr `npm run desktop`.
- `npm run dist`: instaladores con electron-builder (Linux AppImage y deb, Windows NSIS). Sin
  verificar: correr `npm run dist`.
- `node_modules` ya está instalado (`package-lock.json` en el repo).

## Deploy
El README declara dos despliegues y el repo no configura ninguno:
- **Demo estático** en Cloudflare Pages: `https://streamflix-watch.pages.dev/` (sin build, salida
  `/`, auto-deploy al pushear a `main`). GET 200 el 2026-10-26, con
  `<title>StreamFlix - Premium IPTV Experience</title>`. La conexión Pages ↔ GitHub vive en el
  dashboard de Cloudflare; el repo sólo trae `_routes.json`, sin workflow ni `functions/`.
- **Relay en Worker** (`worker/`, `cd worker && npx wrangler deploy`): el README cita
  `https://streamflix-relay.chrizmonsaji.workers.dev` (GET `/health` → 200 el 2026-10-26).

**Sin verificar de quién son.** El subdominio `chrizmonsaji` coincide con el email del autor en
`package.json`, así que es probable que un push a `devsart95/streamjr` no despliegue nada. Lo
verifica `npx wrangler pages project list` con la cuenta de Justino: ¿aparece `streamflix-watch`?

## Variables de entorno
- `PORT` — `server.js`; por defecto 8080.
- `MAX_TIMEOUT_MS` — `worker/wrangler.toml` (12000); no es secreto.
- No hay `.env.example` ni credenciales; `.gitignore` ignora `.env`, `*.pem`, `*.key` y `config.json`.

## Pendientes
- **Revisar el filtro de destinos del relay antes de exponerlo** (`npm start` escucha en `*`):
  `isBlockedHost` (`lib/streamflix-app.js`) e `isPrivateOrLoopbackHost` (`worker/src/index.js`)
  no cubren todas las direcciones privadas. En Pages no corre.
- **Definir de quién son el Pages y el Worker del README** (§ Deploy).

## Docs
- [README.md](README.md) — el del autor: funciones, uso, relay, deploy y changelog; es la doc
  principal.
- [MOBILE_GUIDE.md](MOBILE_GUIDE.md) — controles táctiles, límites conocidos y problemas en móvil.

## Revisar (auditoría del 2026-10-26)
- **El demo estático y el relay que el README presenta como vigentes no figuran en la cuenta de Cloudflare del dueño del fork** (medido el 2026-10-26): `npx wrangler pages project list` no trae ese proyecto de Pages, y `npx wrangler deployments list --name <nombre de worker/wrangler.toml>` responde «This Worker does not exist on your account». § Deploy y § Pendientes todavía los dan por «sin verificar»: corregirlos, y decidir si el README del fork deja de ofrecerlos como propios.
