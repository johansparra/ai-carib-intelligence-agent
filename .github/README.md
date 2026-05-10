# GitHub Copilot Structure

Esta carpeta contiene la estructura de agentes, skills, prompts y herramientas para la plataforma de inteligencia de Dynatrace y DataPower.

## Carpetas Principales

- **`agents/`** - Agentes independientes por dominio y prefijo

  - `dyn-analyst/` - Análisis DQL, métricas y anomalías con Davis AI
  - `dyn-dql-assistant/` - Constructor y validador de queries DQL
  - `dp-analyst/` - Análisis profesional de reportes de gateway
  - `ops-incident-responder/` - Respuesta a incidentes (correlaciona dyn + dp)
  - `ops-daily-summary/` - Resumen diario automático de métricas
  - `core-structure-monitor/` - Sincronización automática de documentación
  - `core-output-polisher/` - Mejora léxica y gramatical de salidas
  - `core-ai-team-dev.agent.md` - Equipo de desarrollo (Nova/Sage/Milo)
  - `core-atlassian-jira.agent.md` - Transformación de requerimientos a Jira

- **`skills/`** - Conocimiento técnico reutilizable por los agentes

  - `dyn-queries/` - Configuración API, biblioteca de queries DQL, sintaxis
  - `dp-analysis/` - Estructura de reportes, patrones de análisis, códigos de error
  - `ops-incident/` - Pasos de correlación y remediación por patrón
  - `core-structure-monitor/` - Lógica de detección y sincronización
  - `core-output-polisher/` - 7 categorías de patrones de mejora en español
  - `core-gh-cli.md` - Referencia completa de GitHub CLI

- **`prompts/`** - Instrucciones de rol y comportamiento para cada agente

  - `dyn-chatbot.prompt.md` - Rol, casos de uso y formato de respuesta
  - `dp-analyst.prompt.md` - Rol, severidad y escalamiento
  - `core-structure-monitor.prompt.md` - Detección automática de cambios

- **`customizations/`** - Configuración y convenciones del proyecto

  - `auto-sync.md` - Configuración de sincronización automática de documentación
  - `naming-conventions.md` - Convenciones de nombres, commits y reglas de formato Markdown

- **`toolboxes/`** - Recursos compartidos por múltiples agentes

  - `report-templates.md` - 4 plantillas estándar: incidente, ejecutivo, tendencias, alerta
  - `common-metrics.md` - SLO/SLA, umbrales de alerta y glosario Dynatrace/DataPower

## Flujo de Output con Output Polisher

Todos los agentes pasan su salida por `@core-output-polisher` antes de entregarla al usuario:

```mermaid
graph LR
    A["@dyn-analyst"] --> OP["@core-output-polisher"]
    B["@dp-analyst"] --> OP
    C["@ops-incident-responder"] --> OP
    D["@ops-daily-summary"] --> OP
    E["@dyn-dql-assistant"] --> OP
    OP --> U["Usuario"]

    style OP fill:#e377c2
    style U fill:#2ca02c
```

## Cómo Funciona Structure Monitor

El agente `structure-monitor` funciona automáticamente:

1. **Detecta** cambios en carpetas, archivos o lógica
2. **Identifica** qué README.md necesitan actualizarse
3. **Regenera** diagramas Mermaid
4. **Corrige** formato Markdown (encabezados, tablas, bloques de código)
5. **Sincroniza** referencias cruzadas
6. **Valida** consistencia de documentación
7. **Hace commit y push** automáticamente

**No requiere activación manual** — trabaja transparentemente en segundo plano.

Para ejecutarlo manualmente: *"Sincroniza la documentación con los cambios actuales"*
