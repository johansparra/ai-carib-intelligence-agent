# Naming Conventions

Convenciones de nombres y estructura para todos los archivos del proyecto.

---

## Sistema de Prefijos

Todos los agentes, skills y prompts llevan un prefijo que indica su dominio:

| Prefijo | Dominio | Cuándo usarlo |
| -------- | ------- | ------------- |
| `dyn-` | Dynatrace | Específico de Dynatrace, DQL o Davis AI |
| `dp-` | DataPower | Específico de DataPower o gateway |
| `ops-` | Operaciones | Usa ambos dominios (incidentes, resúmenes) |
| `core-` | Infraestructura | Transversal al proyecto, sin dominio específico |

**Ejemplos correctos:**

```text
agents/dyn/dyn-analyst/          skills/dyn/dyn-queries/          prompts/dyn/dyn-chatbot.prompt.md
agents/dp/dp-analyst/            skills/dp/dp-analysis/           prompts/dp/dp-analyst.prompt.md
agents/ops/ops-incident-responder/  skills/ops/ops-incident/
agents/core/core-structure-monitor/  skills/core/core-output-polisher/  skills/core/core-gh-cli.md
agents/core/core-ai-team-dev.agent.md
```

---

## Nombres de Archivos

| Tipo de archivo | Convención | Ejemplo |
| ---------------- | ------------ | --------- |
| Agente Copilot | `{prefijo}-{nombre}.agent.md` | `core-ai-team-dev.agent.md` |
| Prompt Copilot | `{prefijo}-{nombre}.prompt.md` | `dyn-chatbot.prompt.md` |
| Skill / documentación | `README.md` dentro de carpeta prefijada | `skills/dyn/dyn-queries/README.md` |
| Toolbox | `{nombre-descriptivo}.md` | `report-templates.md` |
| Customización | `{nombre-descriptivo}.md` | `naming-conventions.md` |

Todos los nombres de archivos y carpetas usan `kebab-case` (minúsculas con guiones).

---

## Estructura de Carpetas

### Cuándo crear un nuevo Agente (`.github/agents/`)

Crear un nuevo agente cuando:

- Tiene un **rol claramente diferenciado** (no es variación de uno existente)
- Necesita instrucciones de comportamiento propias
- Puede ser activado de forma independiente por el usuario
- Tiene un conjunto de skills o herramientas específicas

Estructura mínima del agente:

```bash
agents/{nombre-agente}/
└── README.md   ← descripción, propósito, input/output, cómo activar
```

### Cuándo crear un nuevo Skill (`.github/skills/`)

Crear un nuevo skill cuando:

- Contiene **conocimiento técnico específico** (queries, patrones, configuración)
- Es reutilizable por más de un agente
- No es lógica de comportamiento (eso va en el agente)

Estructura mínima del skill:

```bash
skills/{nombre-skill}/
└── README.md   ← configuración, ejemplos, referencia técnica
```

### Cuándo usar Toolboxes (`.github/toolboxes/`)

Usar toolboxes para:

- Plantillas compartidas por múltiples agentes
- Glosarios y definiciones comunes
- Umbrales y configuración que aplica a todo el proyecto
- Herramientas auxiliares sin pertenencia a un agente específico

---

## Convenciones de Commits

### Para cambios en documentación (automáticos vía Structure Monitor)

```bash
docs: sync README.md diagrams
docs: add new agent {nombre}
docs: fix markdown formatting
```

### Para nuevas funcionalidades

```bash
feat: add {nombre-agente} agent
feat: add {nombre-skill} skill
feat: add {nombre} to toolboxes
```

### Para cambios en prompts

```bash
feat: implement {nombre} prompt
refactor: update {nombre} prompt instructions
```

### Para correcciones

```bash
fix: {descripción del problema corregido}
```

### Commits atómicos — una responsabilidad por commit

```bash
✅ feat: add incident-responder agent
✅ feat: add incident-responder skill
❌ feat: add incident-responder agent and skill and update readme  ← demasiado
```

---

## Reglas de Formato Markdown

Aplicadas automáticamente por el agente Structure Monitor:

1. **Línea en blanco después de encabezados** `##` y `###`
2. **Listas después de `:` deben estar rodeadas de líneas en blanco** (MD032)
3. **Una sola línea en blanco entre secciones** (no dos o más)
4. **Tablas con encabezado y separador** en cada columna
5. **Separadores de tabla con espacios** — siempre dejar espacio después de cada `|` en la fila separadora:
   - ❌ `|----------|----------------|`
   - ✅ `| ---------- | ---------------- |`
6. **Bloques de código con lenguaje obligatorio** — nunca dejar el bloque vacío:
   - ❌ ` ``` `
   - ✅ ` ```bash `, ` ```markdown `, ` ```json `, ` ```dql `, ` ```http `, ` ```log `, ` ```text `

---

## Sección TODO

Marcar integraciones pendientes con:

```markdown
<!-- TODO: reemplazar con valor real -->
<!-- TODO: configurar cuando se tenga acceso a la API -->
<!-- TODO: completar con equipo y canal real -->
```

Esto permite buscar rápidamente todos los puntos de integración pendientes con: `grep -r "TODO" .github/`
