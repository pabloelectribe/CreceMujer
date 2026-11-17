# Backlog de ideas (para después)

> Ideas que no entran en el roadmap actual. Cada una se evalúa antes de pasar a una fase.
> Cuando una se priorice: escribir PRD con la skill `cm-producto` y, si toca datos sensibles, un ADR en `docs/decisiones/`.

---

## IDEA-001 · "Cerca de ti": emprendedoras y publicaciones en tu radio

- **Estado:** idea guardada · **Propuesta:** 2026-11-17 · **Fase tentativa:** 3 o posterior (depende de Conecta y Vitrina)
- **Módulos relacionados:** Conecta, Vitrina, Círculos

### La idea
Mostrar emprendedoras registradas cuyo negocio está dentro de un radio cercano, y avisar cuando vas pasando o viajando por un lugar: "Hay 3 pymes CreceMujer cerca de ti" o "Una emprendedora publicó algo cerca". Sirve para comprar a otras emprendedoras, conocerse en persona y armar redes locales, sobre todo cuando estás de viaje en otra región.

### Cómo podría funcionar (borrador)
1. **Vista "Cerca de ti"** (mapa + lista) dentro de Conecta o Vitrina, con filtro por radio (1, 5, 20 km) y por rubro / momento del negocio.
2. **Alertas de cercanía** (opcionales): push o WhatsApp cuando entras a una zona con pymes o publicaciones nuevas. Con tope de frecuencia (por ejemplo, máx. 1 al día) para no saturar.
3. **Publicaciones geolocalizadas:** una emprendedora puede marcar una publicación de Círculos o Vitrina con su comuna (ofertas, ferias, eventos).

### Lo que hay que resolver antes (no negociable)
- **Seguridad de las emprendedoras.** Mostrar dónde está una mujer es un riesgo real. Se muestra la **ubicación del negocio, nunca la de la persona**, y solo si ella lo activa. Para negocios sin local (desde la casa), mostrar solo la **comuna o una zona aproximada** (ej. celda de ~1 km), nunca la dirección.
- **Doble consentimiento:** para aparecer ("mostrar mi negocio en el mapa") y para recibir alertas ("avisarme cuando pase cerca"). Ambos apagados por defecto y reversibles a un toque.
- **Ley 21.719:** la ubicación es dato personal; definir finalidad, minimización, retención (no guardar historial de recorridos) y base de consentimiento. Requiere ADR y revisión de Seguridad / Legal del banco.
- **Límite técnico de la PWA:** el web no permite geofencing ni ubicación en segundo plano (y en iOS menos). Las alertas "mientras vas pasando" solo funcionan con la app abierta, o hay que aproximarlas (ej. comuna declarada de viaje, "voy a estar en Valdivia el fin de semana"). Una alerta realmente pasiva exigiría app nativa: decisión a evaluar.
- **Anti-abuso:** no permitir búsquedas que ubiquen a una persona específica; limitar consultas por usuaria; bloquear/reportar oculta además del mapa.

### Señales para priorizarla
- En el piloto de Fase 0 aparecen pedidos de "¿quién vende X en mi comuna?" o de conocerse en persona.
- Vitrina tiene suficientes perfiles con rubro y comuna para que el mapa no se vea vacío.
