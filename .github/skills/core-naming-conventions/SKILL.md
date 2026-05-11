---
name: core-naming-conventions
description: >
  Guía de decisión para nombrar componentes (agentes, skills, prompts,
  instructions). Complementa core-naming-conventions.instructions.md
  con criterios para decidir cuándo crear un agente vs skill vs instruction.
---

# Naming Conventions — Skill

Esta skill da **criterios de decisión** para nombrar y crear componentes nuevos. Las **reglas de formato** ya están en [`core-naming-conventions.instructions.md`](../../instructions/core-naming-conventions.instructions.md) (auto-inyectadas a todos los archivos).

---

## 1. Sistema de Prefijos

Todo agente, skill o prompt lleva un prefijo de dominio:

| Prefijo | Dominio | Cuándo usarlo |
| ------- | ------- | ------------- |
| `dyn-` | Dynatrace | DQL, Davis AI, métricas, anomalías |
| `dp-` | DataPower | Gateway, dominios, políticas, reportes |
| `ops-` | Operaciones (cross-domain) | Usa ambos dominios (incidentes, resúmenes diarios) |
| `core-` | Infraestructura | Transversal al proyecto, sin dominio operativo |

**Si dudas entre dos prefijos:** elige el dominio donde vive la responsabilidad principal. Si la responsabilidad está repartida, es `ops-`.

---

## 2. Cuándo crear un Agente vs Skill vs Instruction

### Crear un Agente nuevo cuando:

- Tiene un **rol claramente diferenciado** (no es variación de uno existente)
- Necesita instrucciones de comportamiento propias
- Puede ser activado de forma independiente por el usuario (`@nombre-agente`)
- Tiene un conjunto de skills específicas a las que recurre

### Crear un Skill nuevo cuando:

- Contiene **conocimiento técnico específico** (queries, patrones, configuración, plantillas)
- Es **reutilizable por más de un agente**
- No es lógica de comportamiento — eso va en el agente
- No son reglas siempre-activas — eso va en `instructions/`

### Crear una Instruction nueva cuando:

- Contiene **reglas que deben estar siempre activas** (sin activación manual)
- Aplica a todos los archivos (`applyTo: "**"`) o a un tipo específico (`applyTo: "**/*.md"`)
- Es más corta y directa que un skill — son **reglas**, no conocimiento técnico
- El contenido vale como "guardrail" automático, no como referencia consultada

---

## 3. Estructura por Tipo de Archivo

| Tipo | Ruta | Frontmatter |
| ---- | ---- | ----------- |
| Agente | `agents/{dom}/{prefijo}-{nombre}.agent.md` | `name`, `description`, `tools` |
| Prompt | `prompts/{dom}/{prefijo}-{nombre}.prompt.md` | `name`, `description` |
| Skill | `skills/{prefijo}-{nombre}/SKILL.md` | `name`, `description` |
| Instrucción | `instructions/{prefijo}-{nombre}.instructions.md` | `applyTo` |

Todos los nombres usan `kebab-case` (minúsculas con guiones). Para detalles de formato Markdown, ver [`core-markdown-style.instructions.md`](../../instructions/core-markdown-style.instructions.md).

---

## 4. Ejemplos Canónicos

```text
agents/dyn/dyn-analyst.agent.md
agents/dp/dp-analyst.agent.md
agents/ops/ops-incident-responder.agent.md
agents/core/core-structure-monitor.agent.md

skills/dyn-queries/SKILL.md
skills/dp-analysis/SKILL.md
skills/ops-incident/SKILL.md
skills/core-output-polisher/SKILL.md

prompts/dyn/dyn-chatbot.prompt.md
prompts/dp/dp-analyst.prompt.md

instructions/core-naming-conventions.instructions.md
instructions/core-markdown-style.instructions.md
```

---

## 5. Casos Frecuentes de Decisión

| Caso | Resolución |
| ---- | ---------- |
| "Voy a agregar una nueva query DQL" | Va en `skills/dyn-queries/SKILL.md`, no es un agente nuevo |
| "Necesito una regla de formato Markdown" | Va en `instructions/`, no en un skill |
| "Necesito un análisis especializado de un nuevo tipo de reporte" | Si tiene flujo propio, es un agente nuevo + su skill |
| "Quiero una plantilla de reporte" | Va en `skills/ops-report-templates/SKILL.md` (cross-domain) |
| "Quiero recordar al equipo cómo hacer un commit" | Va en `instructions/core-naming-conventions.instructions.md` |

---

## 6. Antes de Crear

Antes de crear un componente nuevo, pregúntate:

1. ¿**Existe ya** un agente o skill que cubre esta responsabilidad? Si sí, extiéndelo en lugar de duplicar.
2. ¿Es **cross-domain**? Si sí, usa `ops-` o `core-`, nunca `dyn-`/`dp-`.
3. ¿Quién lo **invoca**? Si es invocable por el usuario → agente. Si es consultado por otros agentes → skill. Si debe estar siempre activo → instruction.
4. ¿Hay **tests / cambios funcionales**? Si sí, recuerda: cambios atómicos, un commit = una responsabilidad.

---

## 7. Referencias

| Recurso | Propósito |
| ------- | --------- |
| [`core-naming-conventions.instructions.md`](../../instructions/core-naming-conventions.instructions.md) | Reglas auto-inyectadas (prefijos, commits) |
| [`core-markdown-style.instructions.md`](../../instructions/core-markdown-style.instructions.md) | Reglas de formato Markdown |
| [`core-structure-monitor.agent.md`](../../agents/core/core-structure-monitor.agent.md) | Agente que detecta cambios estructurales y mantiene docs sincronizados |
