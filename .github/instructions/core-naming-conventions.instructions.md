---
applyTo: "**"
---

# Convenciones de Nombres — ai-carib-intelligence-agent

## Sistema de Prefijos

Todos los archivos del proyecto usan un prefijo de dominio:

| Prefijo | Dominio | Cuándo usarlo |
| -------- | ------- | ------------- |
| `dyn-` | Dynatrace | DQL, Davis AI, métricas, anomalías |
| `dp-` | DataPower | Gateway, dominios, políticas, reportes |
| `ops-` | Operaciones | Usa ambos dominios (incidentes, resúmenes) |
| `core-` | Infraestructura | Transversal al proyecto |

## Estructura de Carpetas

Los agentes, skills y prompts se organizan en subgrupos por dominio:

```text
agents/{dominio}/{prefijo}-{nombre}/README.md
skills/{dominio}/{prefijo}-{nombre}/README.md
prompts/{dominio}/{prefijo}-{nombre}.prompt.md
instructions/{prefijo}-{nombre}.instructions.md
```

## Nombres de Archivos

| Tipo | Convención |
| ---- | ---------- |
| Agente (carpeta) | `agents/{dom}/{prefijo}-{nombre}/README.md` |
| Agente (archivo) | `{prefijo}-{nombre}.agent.md` |
| Prompt | `{prefijo}-{nombre}.prompt.md` |
| Skill | `skills/{dom}/{prefijo}-{nombre}/README.md` |
| Instrucción | `{prefijo}-{nombre}.instructions.md` |

Todos los nombres usan `kebab-case`.

## Convenciones de Commits

```text
feat: add {nombre} agent/skill/prompt
refactor: update {nombre} {tipo}
docs: sync README.md diagrams
docs: add new agent {nombre}
fix: {descripción}
```

Un commit = una responsabilidad.
