# Messaging — inventario honesto y plan de secciones

Fase B: contenido y mensajes. Este documento es la única fuente de verdad
editorial hasta que Julio lo ratifique. Gobernado por `.claude/rules/brand.md`
y `brand/voice.md`.

**Base de la auditoría:** `../serchi` @ HEAD `e3b0084` (2026-08-02). Todas las
citas `archivo:línea` son relativas a ese repo, verificadas leyendo el código,
no documentación.

**Fuentes de afirmaciones auditadas:**

- `explore/` — los archivos citados en el encargo (`ruta-b-contraste.html`,
  `ruta-c-instrumento.html`) no existen con esos nombres. Lo que hay:
  «Serchi Landing - Contraste (standalone).html» (≈ ruta B),
  «Serchi Landing Editorial (standalone).html» y «Serchi Landing.html»
  (≈ ruta C: usa el tagline «El mismo instrumento, dos formas de trabajar»).
  Las tres comparten esencialmente el mismo copy; se auditó el superconjunto.
- `content/copy-legacy.md` — copy del sitio Lovable viejo.

**Regla dura:** solo lo clasificado ENVIADO se afirma en presente en cualquier
parte del sitio. PARCIAL se afirma solo hasta su frontera exacta. EN ROADMAP
vive únicamente en la sección Socios Fundadores, en futuro y como compromiso
conjunto.

---

## Task 1 — Qué envía el producto hoy

Clasificación: **ENVIADO** (funciona end-to-end hoy) · **PARCIAL** (frontera
exacta indicada) · **EN ROADMAP** (planeado, no construido) · **NO EXISTE**.

### 1.1 Rutas y superficie

| Capacidad | Estado | Evidencia |
|---|---|---|
| Login / signup / onboarding / aceptar invitación | ENVIADO | `client/src/App.tsx:47-51`, `pages/Onboarding.tsx:63` |
| Home con cola de acciones y embudo | ENVIADO | `pages/Home.tsx:79-99` |
| Procesos: lista, detalle, creación (modal con chat IA) | ENVIADO | `App.tsx:69-85`, `server/routes.ts:856-858` |
| Ficha de candidato + timeline de evidencia | ENVIADO | `pages/CandidateFullPage.tsx`, `components/candidate/EvidenceTimeline.tsx:16-24` |
| Decision Snapshot y comparador de finalistas | ENVIADO | `pages/DecisionSnapshot.tsx:47-58`, `pages/DecisionCompare.tsx:50` |
| Búsqueda global ⌘K (navegación + procesos + clientes) | PARCIAL — no indexa candidatos | `components/layout/GlobalSearch.tsx:161-202` |
| Registro de eventos / auditoría | EN ROADMAP — stub «En construcción» | `pages/UnderConstruction.tsx:59-66` |
| Campana de notificaciones in-app (bandeja) | EN ROADMAP — contador hardcodeado a 0 | `components/notifications/NotificationsBell.tsx:14` |
| Menú de ayuda (videos, agente) | EN ROADMAP — ítems `disabled` | `components/help/HelpMenu.tsx:33,37` |

### 1.2 Modo agencia vs modo empresa

La distinción es **estructural y real**, no un label:

| Diferencia | Estado | Evidencia |
|---|---|---|
| Tipo elegido en onboarding, inmutable después | ENVIADO | `shared/schema.ts:734`, `Onboarding.tsx:63`, `SettingsWorkspacePanel.tsx:76-77` |
| Agencia: hasta 10 clientes con switcher sin cerrar sesión | ENVIADO (límite solo client-side) | `workspace-context.tsx:428`, `SidebarClientSwitcher.tsx:110` |
| Roles distintos por modo, guard en DB | ENVIADO | `shared/membershipRoles.ts:11-25`, `supabase/migrations/20260701_role_assignment_guard_phase3.sql:117-119` |
| Talent pool con alcance por cliente o agencia, enforced en server | ENVIADO | `shared/talentPool.ts:29-57`, `server/talent-pool/handler.ts:28,200-206` |
| Careers público: agencia = índice de clientes; empresa = su página | ENVIADO | `server/landing/careers-handler.ts:29-72` |
| Consentimiento cross-cliente solo en agencia (doble gate server) | ENVIADO | `server/landing/apply-handler.ts:218`, `shared/consent.ts:37-52` |

### 1.3 Careers público y postulación

| Capacidad | Estado | Evidencia |
|---|---|---|
| Página pública de empleos por empresa: `/{slug}-careers` y `/{slug}-careers/{jobSlug}` | ENVIADO | `server/routes.ts:284-285`, `client/src/App.tsx:148-153` |
| Branding por empresa/cliente: color, logo, misión, valores, «Sobre nosotros» | ENVIADO | `server/landing/careers-handler.ts:44-56`, `CareersPublicLayout.tsx:15-22` |
| Publicar/despublicar en un toggle; slug inmutable, URL estable | ENVIADO | `server/landing/admin-handlers.ts:54-92,242` |
| Vista previa firmada antes de publicar (HMAC, 7 días) | ENVIADO (requiere `CAREERS_PREVIEW_TOKEN_SECRET`) | `server/landing/preview-token.ts:34-77` |
| Aislamiento cross-cliente verificado contra staging real | ENVIADO | `e2e/careers-namespace.spec.ts:88-93` |
| Postulación sin cuenta: CV + email + consentimiento; nada que instalar | ENVIADO | `server/landing/apply-handler.ts:56-109` |
| Prefill del formulario leyendo el CV con IA (no persiste) | ENVIADO | `server/landing/cv-parse-handler.ts:19-50`, `ApplicationForm.tsx:102-123` |
| Scoring del CV contra criterios del proceso al postular | ENVIADO | `apply-handler.ts:146-151` |
| Preguntas personalizadas por proceso + captura UTM | ENVIADO | `apply-handler.ts:133-142,176-181` |
| Entrada automática al pipeline en «Nuevos» + notificación in-app a reclutadores | ENVIADO | `candidate-upsert.service.ts:335-336`, `public-jobs.repository.ts:285-345` |
| Previews al compartir en WhatsApp / LinkedIn / X / Slack (OG en edge) | ENVIADO | `middleware.ts:37-72`, `shared/og.ts:12-18` |
| Honeypot + rate limit por IP | ENVIADO (limiter por instancia, sin CAPTCHA) | `apply-handler.ts:38-39,67-70` |
| Email de acuse al candidato que postula | PARCIAL — código completo, flag `PUBLIC_RECEIPT_ENABLED` OFF | `server/config.ts:73-76` |
| SEO para buscadores (Googlebot, robots.txt, sitemap, JSON-LD JobPosting) | NO EXISTE | `shared/og.ts:12-18` (solo 4 bots de share), sin robots/sitemap en repo |
| Board agregado de Serchi (jobs.serchi.ai) | EN ROADMAP — solo columna opt-in | `migrations/0010_processes_publish_to_serchi_board.sql:8-11` |
| White-label (quitar marca Serchi de la página pública) | NO EXISTE | `PublicHeader.tsx:9-21`, `PublicFooter.tsx:11-23` |
| Multi-idioma | NO EXISTE | sin dependencias i18n en `package.json` |

### 1.4 Ingesta de CVs y scoring

| Capacidad | Estado | Evidencia |
|---|---|---|
| Parsing estructurado por LLM (OpenAI `gpt-4.1-mini`): nombre, email, teléfono, LinkedIn, skills (≤20), competencias, sueldo | ENVIADO | `server/cv-import/services/llm.service.ts:79-142`, `server/config.ts:31` |
| Rating 1–5 + resumen en español contra criterios y descripción del cargo | ENVIADO | `llm.service.ts:149-210` |
| Import masivo desde carpeta pública de Drive (~50 CVs por tanda, job reanudable) | ENVIADO | `server/cv-import/trigger-handler.ts:119-143`, `server/config.ts:42-46` |
| Upload manual multi-archivo | PARCIAL — UI multi-PDF, server procesa 1 por request; **solo PDF** | `ManualCvUploadModal.tsx:72-105`, `manual-upload-handler.ts:36-46` |
| Sueldo: extracción solo con cita textual del CV, nunca inferencia | ENVIADO | `shared/salary.ts:1-19`, `llm.service.ts:96-102` |
| OCR de PDFs escaneados | NO EXISTE — devuelve vacío en silencio | `pdf.service.ts:29-51` |
| Historial laboral / educación como campos estructurados | NO EXISTE | `shared/cv-import.ts:168-179` (8 campos exactos) |
| Import por email | NO EXISTE | sin ningún ingreso por email en el repo |
| Motor de score determinista (pesos CV 25 / screening 20 / entrevistas 55), cacheado y visible en toda la UI | ENVIADO | `shared/scoreEngine.ts:108-196`, `ScoreBadge.tsx:98`, `ReviewLayout.tsx:78-81` |
| Edición de pesos del score / plan de evaluación | PARCIAL — motor lo soporta, sin UI de escritura | `ProcessEditDialog.tsx:153-183` |

### 1.5 Pipeline, evaluaciones y talent pool

| Capacidad | Estado | Evidencia |
|---|---|---|
| Pipeline de 4 etapas fijas: Nuevos → Preseleccionados → Entrevistas → Decisión final | ENVIADO (etapas no configurables) | `stageEngine.ts:15` |
| Mover etapa por botón/diálogo (sin drag & drop; sin validación server de transiciones) | PARCIAL | `stageEngine.ts:118-156`, `server/routes.ts:262-270` |
| Notas con autor tipado + timeline de evidencia completo | ENVIADO | `recruitment-context.tsx:811-840`, `EvidenceTimeline.tsx` |
| Evaluación de entrevista por enlace externo: sin cuenta, token 7 días acotado a un candidato, score 1–5 por criterio + recomendación obligatoria | ENVIADO | `server/routes.ts:701-809`, `team-handlers.ts:255-265`, `shared/interviewer.ts:28-51` |
| Agendamiento de entrevistas | NO EXISTE (descartado explícito) | `stageEngine.ts:6-8` |
| Tests de candidatos (técnicos, cognitivos, idioma) | NO EXISTE — switch `disabled` «Próximamente» | `ProcessEditDialog.tsx:138-146` |
| Talent pool con alcance cliente/agencia, filtro por skills (AND), reactivación con gate de consentimiento, import por URL | ENVIADO | `server/talent-pool/handler.ts:126-238`, `ingest-handler.ts:129-284` |
| AI Snapshot por candidato (efímero, con citas a bloques de conocimiento) | ENVIADO (requiere `ANTHROPIC_API_KEY`) | `server/routes.ts:400-453` |
| Knowledge blocks (ADN de la empresa citable por la IA) | ENVIADO (0 filas en prod al último cierre) | `server/knowledge-blocks-handlers.ts:41-94` |

### 1.6 Roles, permisos y datos

| Capacidad | Estado | Evidencia |
|---|---|---|
| 7 roles canónicos, distintos por modo | ENVIADO | `shared/schema.ts:546-554` |
| Anti-escalación de roles en 3 capas (UI + server + trigger DB) | ENVIADO — lo mejor blindado del repo | `team-handlers.ts:177-181`, `20260701_role_assignment_guard_phase3.sql:33-119` |
| Invitaciones de equipo con email y vínculo a cliente/proceso | ENVIADO | `team-handlers.ts:165-204` |
| Visibilidad por proceso (hiring manager / viewer ven solo lo suyo) | PARCIAL — **solo presentación**: el server devuelve el workspace completo; RLS no distingue rol | `server/routes.ts:171-176`, `permissions.ts:137-143` |
| CVs de postulación pública: bucket privado + URL firmada 1 h | ENVIADO | `storage.service.ts:137-164,206-235`, `server/routes.ts:358-395` |
| CVs subidos por reclutador / importados de Drive | **PARCIAL crítico** — van al bucket **público** `cv-uploads` con URL permanente | `storage.service.ts:8-14,115`, `manual-upload-handler.ts:155-167` |
| Consentimiento Ley 19.628: 3 consentimientos con texto verbatim versionado (v1.1) y snapshot persistido | ENVIADO | `shared/consent.ts:13-90`, `apply-handler.ts:200-241` |
| Aviso de transparencia a candidatos importados + opt-out con borrado de CV | PARCIAL — código completo, flag `CANDIDATE_NOTIFICATIONS_ENABLED` OFF; opt-out inerte mientras tanto | `server/config.ts:63-66`, `candidate-notification.service.ts:58` |
| Procesos confidenciales | PARCIAL — columna sin UI | `shared/schema.ts:948` |

### 1.7 Billing

| Capacidad | Estado | Evidencia |
|---|---|---|
| Proveedor: Lemon Squeezy (checkout + portal + webhook HMAC idempotente) | ENVIADO | `server/billing/router.ts`, `handle-billing-webhook/index.ts:33-46` |
| Tiers: starter / pro / growth — **sin precios, moneda ni cuotas en el código** (deliberado: fuente = checkout LS) | ENVIADO (estructura) | `shared/schema.ts:533`, `SettingsPlanPanel.tsx:181-183` |
| Cuenta nueva sin tarjeta: nace en `trial` | ENVIADO | `shared/schema.ts:736` |
| Expiración del trial | NO EXISTE — nadie escribe `trial_end_date`; el trial no expira | `requireActivePlan.ts:103` |
| Modo solo-lectura al expirar/cancelar (HTTP 402 en 16 escrituras) | ENVIADO | `requireActivePlan.ts:140-159` |
| Límites por plan (procesos, candidatos, asientos) | NO EXISTE — UI dice «Sin límite» | `SettingsPlanPanel.tsx:210-225` |

---

## Task 2 — Libro de afirmaciones

Veredictos: **USAR** (cierta hoy, presente) · **ACOTAR** (cierta en parte;
se propone el texto acotado) · **MOVER** (solo en Socios Fundadores, en
futuro) · **ELIMINAR** (insostenible).

### 2.1 Afirmaciones de los borradores de `explore/`

| # | Afirmación (como está escrita) | Estado | Evidencia | Veredicto |
|---|---|---|---|---|
| C1 | «Automatiza el filtro. Enfócate en entrevistar.» | ENVIADO | parsing + rating + orden por score: `llm.service.ts:149-210`, `ReviewLayout.tsx:78-81` | **USAR** |
| C2 | «Serchi ordena los CV que llegan, deja la evidencia a la vista y te muestra a quién entrevistar primero.» | ENVIADO | `scoreEngine.ts:108-196`, `EvidenceTimeline.tsx` | **USAR** |
| C3 | «Para consultoras de RRHH y equipos internos en Chile.» | ENVIADO (posicionamiento) | modos reales + consentimiento Ley 19.628: `shared/consent.ts` | **USAR** |
| C4 | «Creas el cargo y obtienes una página pública para recibir postulaciones. El candidato no instala nada.» | ENVIADO | `admin-handlers.ts:54-92`, `apply-handler.ts:56` | **USAR** |
| C5 | «Cada CV entra al pipeline con su información ya leída y estructurada. No copias nada a mano.» | ENVIADO con borde | `llm.service.ts:79-142`; borde: sin OCR (`pdf.service.ts:29-51`) | **USAR** — sin prometer jamás OCR ni «cualquier formato» |
| C6 | «Cada nota, entrevista y puntaje queda en la ficha del candidato. La decisión se puede explicar después.» | ENVIADO | `EvidenceTimeline.tsx:16-24`, `shared/schema.ts:186-200` | **USAR** |
| C7 | «Un espacio por cliente, con sus procesos, sus candidatos y su marca. Cambias de cliente sin cerrar sesión.» | ENVIADO | `SidebarClientSwitcher.tsx:110`, `careers-handler.ts:44-56`, `talent-pool/handler.ts:200-206` | **USAR** |
| C8 | «Un solo espacio para todos tus cargos, con tu equipo adentro y permisos por rol.» | PARCIAL | roles e invitaciones reales (`membershipRoles.ts`), pero la visibilidad por proceso es solo de presentación (`routes.ts:171-176`) | **ACOTAR** → «…con tu equipo adentro, cada uno con su rol.» No prometer «tú decides quién ve qué» |
| C9 | FAQ: «…publicar el cargo, recibir postulaciones, ordenar candidatos por etapa y registrar la evidencia… También guarda la relación con candidatos que todavía no postulan a nada.» | ENVIADO | talent pool + import por URL: `ingest-handler.ts:208-284` | **USAR** |
| C10 | FAQ: «hecho para cómo trabajan las consultoras chilenas: varios clientes a la vez, procesos cortos, y datos que se rigen por la ley chilena.» | ENVIADO en lo esencial | multi-cliente real; consentimiento Ley 19.628 implementado | **ACOTAR** → decir el mecanismo: «cada candidato acepta un consentimiento conforme a la Ley 19.628, y queda registrado qué texto aceptó y cuándo» |
| C11 | FAQ: «Los CV se guardan en un bucket privado con enlaces firmados y acceso restringido.» | **PARCIAL crítico** | cierto solo para postulaciones públicas (`storage.service.ts:137-164`); los CVs manuales/Drive van a bucket **público** (`storage.service.ts:8-14,115`) | **ACOTAR** hasta que se corrija el bucket (ver pregunta abierta №1). Texto acotado: «Los CV recibidos por la página de empleos se guardan en almacenamiento privado y solo se acceden con enlaces temporales firmados.» |
| C12 | Badge «ATS · hecho en Chile» | ENVIADO (hecho de empresa) | — | **USAR** |
| C13 | «Sin tarjeta · el candidato no instala nada» | ENVIADO | trial por defecto sin tarjeta: `shared/schema.ts:736` | **USAR** |
| C14 | Mock: URL pública `serchi.ai/p/analista-operaciones` | — | forma real: `/{slug}-careers/{jobSlug}` (`App.tsx:148-153`) | **ACOTAR** — los mocks deben mostrar la forma real de URL |
| C15 | Mock: scores 87/74/68 + «Entrevistar primero» | ENVIADO (el orden por score existe) | `ScoreBadge.tsx:98` | **ACOTAR** — copiar la escala y el formato exactos de la UI real antes de dibujar mocks |
| C16 | Mock: «Entrevista técnica · 4/5 · evaluó R. Fuentes» | ENVIADO | evaluación externa 1–5: `server/routes.ts:705-716` | **USAR** |
| C17 | Mock roles: «Admin / Evaluador / Lectura» | PARCIAL | «Evaluador» no es rol de membership (es actor por token): `permissions.ts:185-194` | **ACOTAR** — usar los nombres reales de rol en mocks |
| C18 | Socios Fundadores: pilotos, precio preferente permanente, acceso directo, «Todavía no tenemos testimonios que mostrar. Preferimos decirlo.» | — (promesa comercial, no de producto) | — | **USAR** — es la sección honesta por diseño; las promesas comerciales las ratifica Julio (pregunta №4) |
| C19 | Footer: Términos · Privacidad · Seguridad · Reembolsos | PARCIAL | existen textos legacy de Términos/Reembolsos/Seguridad (`copy-legacy.md:148-191`); **no existe página de Privacidad** en ningún lado | **ACOTAR** — no enlazar Privacidad hasta que exista (pregunta №6) |

### 2.2 Afirmaciones del copy legacy (`content/copy-legacy.md`)

| # | Afirmación | Estado | Evidencia | Veredicto |
|---|---|---|---|---|
| L1 | «Decisiones de contratación basadas en evidencia» (título SEO, l.17) | ENVIADO | timeline + score con desglose | **USAR** |
| L2 | «Organiza candidatos, entrevistas y decisiones en un solo lugar.» (l.18) | ENVIADO | — | **USAR** |
| L3 | Badge «Potenciado con IA» (l.30) | — | anti-patrón declarado en `brand.md` | **ELIMINAR** |
| L4 | «…te muestra a quién avanzar, con ayuda de IA.» (l.32) | ENVIADO | rating + resumen + orden | **USAR** (la IA nombrada donde explica un resultado) |
| L5 | «Importar CVs (Drive/Email)» (l.39) | PARCIAL | Drive sí (`trigger-handler.ts`); email NO EXISTE | **ACOTAR** → «Importa una carpeta de Drive completa» |
| L6 | «Puntuar Match Score (⭐ 1–5)» (l.39) | PARCIAL | rating 1–5 real; «Match Score» no es naming del producto; emoji prohibido | **ACOTAR** → «puntúa 1–5 contra los criterios del cargo», ícono Lucide |
| L7 | «IA procesó 12 CVs → dejó 3 listos para entrevistar» (l.41) | PARCIAL | la IA puntúa y ordena; no «deja listos» — decide el humano | **ACOTAR** → «Serchi leyó 12 CVs y te muestra a quién entrevistar primero» |
| L8 | Dolores: Drive/WhatsApp/Excel · entrevistadores sin contexto · decisiones por intuición · candidatos repetidos (l.48-52) | — (diagnóstico del cliente) | cada dolor tiene contraparte real en el producto | **USAR** — excepto «tests desconectados» (l.51): **ELIMINAR** (el producto no ofrece tests; nombrar el dolor implica la solución) |
| L9 | Chat «Pregúntale a Serchi» + «Preparado para integración con Lovable AI» (l.55-68) | — | feature del sitio viejo; jerga interna | **ELIMINAR** (las respuestas enlatadas sirven de insumo para la FAQ, reescritas) |
| L10 | «Tests técnicos, cognitivos, de idioma y cultura. Se asignan en segundos…» (l.66) | NO EXISTE | switch `disabled` «Próximamente»: `ProcessEditDialog.tsx:138-146` | **ELIMINAR** del presente; **MOVER** como roadmap si Julio quiere revelarlo |
| L11 | «puedes empezar a usarla en menos de una semana» (l.67) | — | sin dato que lo sustente; el signup es self-serve inmediato | **ACOTAR** → «Creas tu cuenta y publicas tu primer proceso el mismo día» (verificable por producto) |
| L12 | «Talent Cloud vivo — reutiliza talento entre procesos» (l.75) | ENVIADO (capacidad) | reactivación con consentimiento: `ingest-handler.ts:129-191` | **ACOTAR** → decirlo en lenguaje llano, sin el naming «Talent Cloud» |
| L13 | «Entrevistas colaborativas con scorecards — notas por rol, etapa y entrevistador. Historial visible.» (l.76) | ENVIADO | `ExternalInterview.tsx:289-298`, `EvidenceTimeline.tsx` | **USAR** (sin la palabra «scorecards» si se prefiere lenguaje llano) |
| L14 | «Tests integrados (plug-and-play)» (l.77) | NO EXISTE | ídem L10 | **ELIMINAR** / **MOVER** |
| L15 | «IA que te ayuda a decidir — sugerencias, ranking y señales. Menos sesgo, más evidencia.» (l.78) | PARCIAL | ranking sí; «menos sesgo» sin sustento — los propios ToS lo desmienten (`copy-legacy.md:159`) | **ACOTAR** → quedarse con «ranking y resumen contra los criterios del cargo»; **ELIMINAR** la promesa de reducción de sesgo |
| L16 | «5 pasos simples» incluyendo «Asigna tests» (l.85-89) | PARCIAL | pasos 1-3 y 5 reales; paso 4 (tests) no existe | **ACOTAR** → los 3 pasos de los borradores nuevos ya lo resuelven |
| L17 | Comparación «Con/Sin Serchi»: «Pensado para LATAM», «Rápido de implementar» (l.96-100) | — | formato de tabla comparativa declarando virtudes ≈ plantilla | **ELIMINAR** el formato; el contenido rescatable ya vive en la FAQ de Greenhouse/Lever (C10) |
| L18 | Prueba social completa: logos, testimonios, métricas (l.104-118) | FABRICADO | regla dura en `brand.md` | **ELIMINAR** — no se migra nada |
| L19 | Precios: Starter $49.000 / Pro $119.000 / Growth $299.000 CLP + límites por plan (l.127-136) | NO VERIFICABLE | los precios no existen en el código (deliberado: fuente = checkout Lemon Squeezy, `SettingsPlanPanel.tsx:181-183`); los límites listados (procesos, clientes, links) **no tienen enforcement** (`SettingsPlanPanel.tsx:210-225`) | **ELIMINAR** las cifras y los límites hasta confirmarlos en Lemon Squeezy (pregunta №2) |
| L20 | «Nuestra IA analiza candidatos, genera rankings basados en evidencia, y te da señales para reducir sesgos» (l.65) | PARCIAL | ídem L15 | **ACOTAR** — sin la parte de sesgos |
| L21 | Textos legales: Términos, Reembolsos, Seguridad (l.148-191) | — | reutilizables como base | **USAR** como insumo, con revisión (pregunta №6) |
| L22 | Seguridad: «Toda la información se transmite y almacena cifrada» · «Roles y permisos granulares — tú decides quién tiene acceso» · «Los candidatos pueden solicitar acceso, rectificación o eliminación» (l.185-190) | PARCIAL | cifrado: infraestructura Supabase, no verificado como claim propio; «permisos granulares» excede lo enforced (`routes.ts:171-176`); el canal de opt-out está **inerte** con los flags OFF (`candidate-notification.service.ts:58`) | **ACOTAR** las tres: prometer solo el mecanismo que existe (roles con anti-escalación; derechos ejercibles por correo hasta que el opt-out automático esté encendido) |

### 2.3 Lo real que ningún borrador menciona (infra-comunicación)

Todo esto es ENVIADO, verificado, y ninguna de las dos fuentes lo dice:

1. **Import masivo desde Drive.** La audiencia vive en carpetas de Drive — y
   el producto ingiere una carpeta completa (~50 CVs por tanda) con lectura y
   puntuación automáticas (`trigger-handler.ts:119-143`). Es el puente exacto
   desde su flujo actual y nadie lo cuenta.
2. **El formulario se rellena solo.** El candidato suelta su CV y el
   formulario se autocompleta con IA antes de enviar
   (`cv-parse-handler.ts:19-50`). Menos fricción = más postulaciones.
3. **Evaluadores externos por enlace, sin cuenta.** Un enlace de un solo uso
   (7 días, acotado a un candidato) para que el gerente del cliente evalúe con
   puntaje y recomendación, sin licencia ni registro
   (`team-handlers.ts:255-265`, `routes.ts:701-809`). Para una consultora,
   esto es cómo involucra a su cliente. Argumento comercial mayor.
4. **La página de empleos se comparte bien por WhatsApp.** Preview con logo y
   cargo al pegar el enlace en WhatsApp o LinkedIn (`shared/og.ts:12-18`).
   En Chile los procesos se mueven por WhatsApp; esto conecta directo.
5. **Consentimiento Ley 19.628 con texto versionado.** No es «cumplimos la
   ley» de brochure: cada consentimiento guarda el texto exacto aceptado, con
   versión y fecha (`shared/consent.ts`, `apply-handler.ts:200-241`), con
   consentimiento cross-cliente específico para agencias. Diferenciador real
   frente a ATS extranjeros.
6. **Sueldo solo si el CV lo declara.** La extracción de pretensión de renta
   exige cita textual; el sistema tiene prohibido inferirla
   (`shared/salary.ts:1-6`). Es una decisión de honestidad de producto que
   habla el idioma de la marca.
7. **Comparador de finalistas.** Vista lado a lado de finalistas con resumen
   IA por columna (`DecisionCompare.tsx:50`). Es la materialización de
   «decisiones basadas en evidencia».
8. **Vista previa antes de publicar.** Enlace firmado para revisar la oferta
   con el cliente antes de que sea pública (`preview-token.ts:34-77`).
9. **Preguntas personalizadas por proceso** y **atribución UTM** de cada
   postulación (`apply-handler.ts:133-142,176-181`).
10. **El equipo se entera al instante:** cada postulación notifica in-app a
    los reclutadores del proceso (`public-jobs.repository.ts:285-345`).

---

## Task 3 — Inventario de secciones

Orden del borrador actual: Hero · Problema · Cómo funciona · Dos modos ·
Socios Fundadores · FAQ · Cierre · Footer.

Orden propuesto (un cambio estructural + reasignación de contenido):

| # | Sección | Trabajo que hace | Por qué aquí |
|---|---|---|---|
| 1 | **Hero** | Decir qué es, para quién, y las dos salidas (crear cuenta / agendar). | El lector decide en segundos si esto es para él. «Para consultoras de RRHH y equipos internos en Chile» filtra a favor nuestro. |
| 2 | **Problema** | Reconocimiento: Drive, WhatsApp, Excel, decisiones de memoria. | Antes de creer la solución, el lector necesita sentirse descrito. El copy actual (C20) lo hace bien; se mantiene. |
| 3 | **Cómo funciona** (3 pasos) | Mostrar el flujo completo: publica → recibe y ordena → decide con evidencia. | Ya reconocido el problema, se muestra el mecanismo. Aquí se inyecta lo infra-comunicado: paso 1 gana la página con marca del cliente + compartible por WhatsApp; paso 2 gana el import de Drive y el formulario que se rellena solo; paso 3 gana el evaluador externo por enlace y el comparador de finalistas. Todo ENVIADO. |
| 4 | **Dos modos** | Diferenciar consultora vs equipo interno sin duplicar el sitio. | El lector ya entendió el flujo; ahora ve su caso. El copy actual es sostenible (C7 USAR, C8 acotado). |
| 5 | **Datos de candidatos** ← **NUEVA** | Confianza sin testimonios: Ley 19.628 con consentimiento versionado, almacenamiento y acceso, quién ve qué. | **El cambio más importante.** La audiencia maneja datos de terceros (los candidatos de sus clientes) y no puede permitirse un problema legal. Hoy esta carga la lleva una sola FAQ. El producto tiene sustancia real aquí (§2.3.5) — es la sección de confianza que la regla de marca permite: habla del producto y del criterio, no de clientes. Condicionada a resolver la pregunta №1 (bucket público). |
| 6 | **Socios Fundadores** | Convertir la falta de clientes en criterio de selección; único lugar donde vive el roadmap (en futuro, como construcción conjunta). | Después de creer en el producto y en el manejo de datos, la invitación a ser de los primeros tiene contexto. Aquí van los MOVER: tests, board público de empleos, notificaciones al candidato. |
| 7 | **FAQ** | Objeciones restantes: qué es exactamente, Greenhouse/Lever, datos (versión corta que enlaza a §5), precio (respuesta honesta sin cifras). | Baja la última resistencia antes del cierre. |
| 8 | **Cierre** | Repetir las dos salidas. | — |
| 9 | **Footer** | Legal + contacto. Sin enlace a Privacidad hasta que la página exista. | — |

**Qué se deja fuera deliberadamente:**

- **Precios** — las cifras legacy no son verificables desde el código (la
  fuente de verdad es el checkout de Lemon Squeezy) y los límites por plan no
  tienen enforcement. Publicar una tabla que el producto no respalda es
  exactamente el error que esta fase existe para impedir. (Pregunta №2.)
- **Prueba social** — regla dura; la sección Socios Fundadores la reemplaza.
- **Tests** — no existen; solo como roadmap en Socios Fundadores si Julio
  decide revelarlo.
- **Tabla comparativa «Con/Sin Serchi»** — formato de plantilla; el contenido
  útil vive en la FAQ.
- **Badge de IA / sección de IA** — la IA se nombra dentro de los pasos,
  donde explica un resultado.
- **Promesas de SEO / Google for Jobs** — el producto no las cumple
  (sin JSON-LD, sin sitemap; solo OG para 4 bots de share).

---

## Task 4 — Preguntas abiertas para Julio

Cada una como decisión concreta, con recomendación.

1. **Bucket público de CVs manuales.** Los CVs subidos por reclutador o
   importados de Drive quedan en el bucket público `cv-uploads` con URL
   permanente (`storage.service.ts:8-14,115`); solo los de postulación
   pública están en bucket privado con URL firmada. Opciones: (a) corregir en
   producto antes del lanzamiento y publicar la afirmación completa de la
   FAQ, o (b) publicar solo el texto acotado de C11.
   **Recomendación: (a)** — es la afirmación de confianza central de la
   sección Datos y el arreglo cierra además un riesgo real; mientras tanto el
   sitio usa el texto acotado.
2. **Exposición de precios.** Las cifras legacy ($49.000/$119.000/$299.000
   CLP) no son verificables desde el código y los límites por plan no
   existen. Opciones: (a) sin tabla de precios; la FAQ responde honesto
   («planes por tamaño de equipo; precio preferente permanente para Socios
   Fundadores») y el CTA es agendar; (b) confirmar cifras vigentes en Lemon
   Squeezy y publicarlas sin límites inventados.
   **Recomendación: (a)** para el lanzamiento — coherente con la fase de
   pilotos; revisar al salir de ella.
3. **Emails al candidato apagados.** `PUBLIC_RECEIPT_ENABLED` y
   `CANDIDATE_NOTIFICATIONS_ENABLED` están OFF: el candidato no recibe acuse
   ni aviso, y el opt-out automático está inerte. ¿Se encienden antes del
   lanzamiento del sitio? Afecta qué puede prometer la sección Datos sobre
   derechos de los candidatos.
   **Recomendación:** encender al menos el acuse de postulación; hasta
   entonces la sección Datos dice «derechos ejercibles escribiendo a
   hello@serchi.ai», no promete flujos automáticos.
4. **Promesas del programa Socios Fundadores.** «Precio preferente
   permanente», «acceso directo al equipo», «sus procesos definen el
   roadmap»: son compromisos comerciales que el código no puede validar.
   ¿Se ratifican tal cual? ¿Cupo explícito (p. ej. «5 consultoras»)?
   **Recomendación:** ratificar los tres y poner cupo numérico — hace creíble
   el criterio de selección sin inventar demanda.
5. **Cuánto roadmap revelar.** Candidatos con contrato ya visible en el
   código: tests («Próximamente» en la app), board público de empleos
   (opt-in ya recolectado), notificaciones al candidato.
   **Recomendación:** nombrar exactamente esos 2–3 en Socios Fundadores como
   «lo que construiremos con los pilotos», nada más.
6. **Página de Privacidad.** No existe (ni en el legacy ni en la app; el
   footer público de la app enlaza a `/privacy` que hoy cae en 404 —
   `PublicFooter.tsx:25-27`). ¿Quién la redacta y con qué revisión legal,
   antes del lanzamiento?
   **Recomendación:** redactarla junto con la actualización de Términos
   (base legacy utilizable), con revisión legal, antes de publicar el sitio.
7. **Dominios en el sitio.** El mock muestra `serchi.ai/p/analista-operaciones`;
   la forma real es `{origen}/{slug}-careers/{jobSlug}` y no hay dominio
   público configurado para careers (el board `jobs.serchi.ai` no existe).
   ¿Qué URL se muestra en los mocks del sitio?
   **Recomendación:** mostrar la forma real con un slug de ejemplo
   (`app.serchi.ai/tu-empresa-careers/analista-operaciones`) — el mock es una
   promesa; que prometa lo que existe.

### Anexo — Hallazgos de producto que afectan afirmaciones (no son trabajo de esta fase)

- Bucket público `cv-uploads` para CVs manuales/Drive (pregunta №1).
- Enlaces muertos en el footer público de la app: `/privacy`, `/security`,
  `/security/report` caen en 404 (`PublicFooter.tsx:25-27`).
- El trial nunca expira (`trial_end_date` sin escritor —
  `requireActivePlan.ts:103`); irrelevante para el copy, relevante para el
  negocio.
- Un `recruiter` ve botones de edición del Careers Hub que el server le
  rechaza con 403 (`careersHubModel.ts:18-20` vs
  `company-profile-handlers.ts:108`).
