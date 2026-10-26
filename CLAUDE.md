# StreamIPTV

Reproductor IPTV web de demo, estilo Netflix: canales de listas M3U con HLS.js, relay Express
contra CORS y mixed-content, versión de escritorio con Electron y una plantilla de relay en
Cloudflare Worker. JS vanilla, sin build. Repo `devsart95/streamjr`, público.
Hereda las reglas generales del autor; acá sólo lo específico.

**Magnitud: `descartable`** (`project.yaml`). Entra completo: que el demo corra y reproduzca un
canal. Queda como techo: nada — si deja de ser un demo, la banda se revisa antes de tocarlo.

- **No es `streamflix`**, la app Android de otro repo, aunque su `package.json` se
  llame `streamflix`: son proyectos distintos.
- **Es un fork público de `chriz-3656/streamflix` (MIT) y el código es de su autor**: al
  2026-10-26, 57 de 58 commits son de «Chris» o «MISTER CHRIS». No hay remoto `upstream`
  configurado.
- **El demo y el relay que cita el README son del autor** (`streamflix-watch.pages.dev`,
  `streamflix-relay.chrizmonsaji.workers.dev`). Que un push acá despliegue algo está sin verificar
  → `CONTEXT.md` § Deploy.
- **El relay es un proxy de URLs arbitrarias con un filtro de hosts incompleto**
  (`CONTEXT.md` § Pendientes): `npm start` escucha en todas las interfaces, así que no se expone
  fuera de la máquina.
- **Repo público**: ninguna credencial, host interno ni ID de cuenta en lo que se commitea.
- Sin tests ni linter: `package.json` no define `test` ni `lint`.
