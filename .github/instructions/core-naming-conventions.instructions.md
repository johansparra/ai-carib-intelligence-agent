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

Dentro de cada carpeta oficial, los archivos se agrupan en subgrupos por dominio. No se usan sub-subcarpetas — todo es un archivo plano dentro del subgrupo:

```text
agents/{dominio}/{prefijo}-{nombre}.agent.md
skills/{prefijo}-{nombre}/SKILL.md
prompts/{dominio}/{prefijo}-{nombre}.prompt.md
instructions/{prefijo}-{nombre}.instructions.md
```

## Nombres de Archivos

| Tipo | Convención |
| ---- | ---------- |
| Agente | `agents/{dom}/{prefijo}-{nombre}.agent.md` |
| Prompt | `prompts/{dom}/{prefijo}-{nombre}.prompt.md` |
| Skill | `skills/{prefijo}-{nombre}/SKILL.md` |
| Instrucción | `instructions/{prefijo}-{nombre}.instructions.md` |

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
