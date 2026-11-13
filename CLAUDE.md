# CreceMujer Comunidad — contexto maestro para Claude Code / Claude Design

> Este archivo es el punto de entrada. Léelo completo antes de tocar código o diseño.
> Los documentos de `docs/` profundizan cada tema; las skills en `.claude/skills/` te dicen *cómo* trabajar cada frente.

## 1. Qué estamos construyendo

**CreceMujer Comunidad** es la webapp mobile-first (PWA instalable) que convierte a CreceMujer.cl —el programa de BancoEstado para mujeres micro y pequeñas empresarias de Chile— en una **comunidad viva de emprendedoras**, no solo en un sitio de contenidos.

La tesis: hoy las conexiones reales entre emprendedoras ocurren en grupos de WhatsApp. No vamos a "ganarle" a WhatsApp en mensajería; vamos a ofrecer lo que WhatsApp no puede: **conocimiento que no se pierde, conexiones por afinidad de negocio, capacitación integrada y un espacio seguro solo para ellas, con el respaldo institucional de BancoEstado**. WhatsApp pasa a ser canal de entrada y de notificación, no el competidor.

Cinco módulos (ver `docs/03-propuesta-de-solucion.md`):

| Módulo | Nombre de cara a la usuaria | Qué resuelve |
|---|---|---|
| Círculos | **Círculos** | Grupos por tema / región / momento del negocio. Feed con hilos + chat liviano. Fotos y documentos. |
| Preguntas | **Preguntas** | Q&A entre emprendedoras sobre gestión del negocio, con respuesta destacada y reputación. |
| Aprende | **Aprende** | Oferta de capacitación VMP+CM, videos cortos, charlas en vivo (YouTube Live embebido) y grabaciones. |
| Conecta | **Conecta** | "Match" de emprendedoras: ofrezco / busco → doble opt-in → conversación. |
| Vitrina | **Vitrina** | Perfil público de cada emprendedora y su negocio; base del Sello CreceMujer. |

## 2. A quién servimos

- Mujeres micro y pequeñas empresarias en Chile, de todas las regiones; mayoría usa el celular como único dispositivo, con datos móviles limitados y poco tiempo.
- Cuatro **momentos del negocio** (taxonomía VMP vigente, no lineal): **Iniciando, Crecimiento, Reinvención, Consolidación**. Todo el contenido y el match se segmentan por momento.
- No asumimos alfabetización digital alta: patrones conocidos (WhatsApp, Instagram), textos cortos, iconos claros, nada que requiera tutorial.

Personas y journeys en `docs/06-personas-y-journeys.md`.

## 3. Principios de producto (no negociables)

1. **Mobile first, web después.** Se diseña en 360×800 primero. El desktop es la misma app con más aire, y además se embebe como "la gran funcionalidad" de crecemujer.cl.
2. **Cada pantalla tiene un "para qué" en 3 segundos.** Si una emprendedora no entiende qué hacer en una pantalla sin leer, la pantalla está mal.
3. **Conocimiento que se acumula.** Lo que se conversa debe poder encontrarse después (búsqueda, respuestas destacadas, guardados). Esto es lo que WhatsApp no da.
4. **Seguridad y confianza.** Comunidad solo de mujeres verificadas; reportar/bloquear a un toque; moderación humana + automática; reglas visibles.
5. **Interoperable con WhatsApp, no contra WhatsApp.** Compartir a WhatsApp desde cualquier pieza; notificaciones por WhatsApp (API Cloud, plantillas) y push; deep links que abren la app instalada.
6. **Integrada al ecosistema VMP.** No duplicamos editorial: la oferta de capacitación viene de vamosmipyme.cl vía API y se marca con distintivo CM. SSO compartido.
7. **Datos con criterio bancario.** Mínimo dato personal necesario, consentimiento explícito, Ley 19.628 / nueva Ley de Protección de Datos Personales (Ley 21.719) en mente; nada de scraping de WhatsApp ni de contactos del teléfono.

## 4. Decisiones técnicas por defecto (ver `docs/05-arquitectura-e-integraciones.md`)

- **Frontend:** Next.js (App Router) + TypeScript + Tailwind + shadcn/ui, como **PWA** (manifest, service worker, instalable, offline básico para lectura). Push vía Web Push (VAPID). En iOS el push solo funciona si la app está agregada a la pantalla de inicio → el onboarding debe enseñar a instalarla.
- **Backend:** Supabase (Postgres + Auth + Storage + Realtime + Edge Functions) o equivalente self-hosted si Seguridad del banco lo exige. Row Level Security en todas las tablas.
- **Mensajería en tiempo real:** fase MVP sobre Supabase Realtime (hilos + chat liviano). Si el volumen de chat 1:1 crece, evaluar Stream Chat (SDK con moderación, hilos, push) antes que construir más.
- **Video:** YouTube (embeds y YouTube Live) — es el estándar ya definido en la especificación de CreceMujer 2026. Nada de hosting propio de video en MVP.
- **Notificaciones:** Web Push + WhatsApp Business Cloud API (plantillas de utilidad: "nueva respuesta a tu pregunta", "tu charla empieza en 1 hora", "tienes una conexión nueva"). Email como respaldo.
- **Integración WordPress (crecemujer.cl):** la comunidad vive en `comunidad.crecemujer.cl`; el sitio WordPress la enlaza y embebe widgets (últimas preguntas, próximas charlas) vía API pública de solo lectura.
- **Analítica:** eventos propios (tabla `events`) + GA4. Dashboard interno con las métricas de `docs/07-metricas-y-moderacion.md`.

Si una decisión de este bloque choca con una restricción real del banco, **no la fuerces**: documenta la alternativa en `docs/decisiones/` como ADR y sigue.

## 5. Cómo trabajar en este repo

- Idioma: **todo en español** (UI, commits, docs, comentarios). Español de Chile, tono cercano y profesional: "tú", nunca "usted"; "emprendedora", nunca "usuario".
- Terminología fija: CreceMujer (CM), Vamos MiPyme (VMP), Sello CreceMujer, momentos del negocio, módulo de Educación Financiera, mantención evolutiva, banco de horas.
- Antes de diseñar o construir un módulo, carga la skill correspondiente:
  - `cm-producto` → PRD, historias de usuario, criterios de aceptación.
  - `cm-diseno-mobile` → sistema de diseño, componentes, pantallas.
  - `cm-match` → el módulo Conecta (perfiles, señales, algoritmo, seguridad).
  - `cm-comunidad-moderacion` → reglas, flujos de reporte, roles, moderación asistida.
  - `cm-contenido-aprende` → integración VMP, charlas en vivo, microformatos.
  - `cm-stack-tecnico` → arquitectura, modelo de datos, PWA, integraciones.
  - `cm-datos-metricas` → eventos, KPIs, dashboard, experimentos.
- Entregables de diseño: primero flujo (texto) → luego wireframe móvil → luego hi-fi. Pide feedback entre etapas.
- Entregables de código: cada feature con migración SQL, políticas RLS, componente, test básico y entrada en `CHANGELOG.md`.
- Nunca inventes datos de emprendedoras reales. Usa el set de datos ficticio en `docs/06-personas-y-journeys.md`.

## 6. Roadmap resumido

- **Fase 0 (4–6 semanas) — Piloto en WhatsApp + prototipo clicable.** 2–3 grupos piloto de 40–60 emprendedoras. Observar qué preguntan, qué comparten, cuándo. Prototipo Figma/Claude Design validado con 8–10 de ellas.
- **Fase 1 (MVP, ~3 meses) — Círculos + Preguntas + Aprende + Perfil + notificaciones.** Invitación cerrada. Meta: 500 emprendedoras activas, 60% de preguntas con respuesta en 24 h.
- **Fase 2 (~3 meses) — Conecta (match) + charlas en vivo con chat + Vitrina.** Meta: 30% de las activas con al menos una conexión aceptada.
- **Fase 3 — Integración profunda VMP/Sello, embebido en crecemujer.cl, reconocimiento (insignias, "Emprendedora del mes"), apertura general.**

## 7. Dónde está cada cosa

```
CLAUDE.md                      ← este archivo
docs/01-vision-y-problema.md
docs/02-benchmark.md           ← referencias analizadas (genias.cl, FAW, Thrive, etc.)
docs/03-propuesta-de-solucion.md
docs/04-match-emprendedoras.md
docs/05-arquitectura-e-integraciones.md
docs/06-personas-y-journeys.md
docs/07-metricas-y-moderacion.md
docs/decisiones/               ← ADRs (crear al tomar decisiones)
.claude/skills/*/SKILL.md      ← skills por frente de trabajo
prompts/                       ← prompts listos para pegar en Claude Code / Claude Design
```
