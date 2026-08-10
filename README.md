# serchi-web

Sitio de marketing de **Serchi** (serchi.ai). Solo la landing pública: este repo
no contiene la aplicación. La app vive en otro repo y se despliega en
**app.serchi.ai**; ambos productos comparten únicamente la marca.

## Estructura

- `brand/` — el contrato de marca. `brand/design-system.html` es la fuente de
  verdad; `tokens.css`, los SVG del Bloom, `voice.md` y `favicon/` se derivan
  de él. Nada visual se decide fuera de ese documento.
- `content/copy-legacy.md` — copy rescatado de la landing anterior
  (serchi-talent-flow, Lovable), como materia prima editorial.
- `.claude/rules/brand.md` — reglas de marca para sesiones futuras.
- `NOTES.md` — deuda conocida y decisiones pendientes.

## Fase actual

**Scaffolding y extracción del brand kit.** Todavía no hay sitio: sin
framework, sin build. La siguiente fase es una exploración visual que ocurre
fuera de este repo.
