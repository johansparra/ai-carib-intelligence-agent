# ai-carib-intelligence-agent

Plataforma de inteligencia para análisis de Dynatrace y DataPower, construida sobre GitHub Copilot con agentes especializados, skills y prompts.

## Estructura del Proyecto

```mermaid
graph TB
    subgraph ".github/agents"
        DN["@dyn-analyst"]
        DQL["@dyn-dql-assistant"]
        DPA["@dp-analyst"]
        IR["@ops-incident-responder"]
        DS["@ops-daily-summary"]
        SM["@structure-monitor"]
        OP["@output-polisher"]
        TEAM["@ai-team-dev"]
        JIRA["@atlassian-jira"]
    end

    subgraph ".github/skills"
        DQ["dyn-queries"]
        DPS["dp-analysis"]
        OPI["ops-incident"]
        ORT["ops-report-templates"]
        OMT["ops-metrics-thresholds"]
        CSM["core-structure-monitor"]
        CAS["core-auto-sync"]
        COP["core-output-polisher"]
        CGH["core-gh-cli"]
        CNC["core-naming-conventions"]
    end

    subgraph ".github/prompts"
        PDN["dyn-chatbot"]
        PDP["dp-analyst"]
        PSM["structure-monitor"]
    end

    subgraph ".github/instructions"
        INC["core-naming-conventions"]
        IMD["core-markdown-style"]
    end

    DN --> DQ
    DQL --> DQ
    DPA --> DPS
    IR --> OPI
    IR --> ORT
    IR --> OMT
    DS --> ORT
    DS --> OMT
    SM --> CSM
    SM --> CAS
    OP --> COP
    TEAM --> CGH

    DN --> OP
    DPA --> OP
    IR --> OP
    DS --> OP
    DQL --> OP

    style DN fill:#1f77b4
    style DQL fill:#8c564b
    style DPA fill:#ff7f0e
    style IR fill:#d62728
    style DS fill:#9467bd
    style SM fill:#2ca02c
    style OP fill:#e377c2
    style TEAM fill:#17becf
    style JIRA fill:#bcbd22
```

## Flujo de Procesamiento

```mermaid
sequenceDiagram
    participant U as Usuario
    participant C as Chatbot Copilot
    participant DQL as @dyn-dql-assistant
    participant DN as @dyn-analyst
    participant DP as @dp-analyst
    participant IR as @ops-incident-responder
    participant OP as @output-polisher
    participant SM as @structure-monitor

    U->>C: Solicitud de análisis
    C->>DQL: Construir query DQL si es compleja
    DQL-->>C: Query validada
    C->>DN: Ejecutar query + interpretar
    DN-->>C: Datos + severidad
    C->>DP: Analizar reporte DataPower (si aplica)
    DP-->>C: Conclusiones + patrón
    C->>IR: Correlacionar si severidad CRITICAL
    IR-->>C: Reporte de incidente
    C->>OP: Pulir texto de salida
    OP-->>U: Respuesta final

    par Monitoreo Automático
        SM->>SM: Detectar cambios estructurales
        SM->>SM: Regenerar diagramas y READMEs
    end
```

## Estructura de Carpetas

```text
ai-carib-intelligence-agent/
├── README.md
├── CLAUDE.md                        ← Guía para Claude Code
├── Comandos.md                      ← Referencia de comandos Copilot
└── .github/
    ├── copilot-instructions.md
    ├── README.md
    ├── agents/
    │   ├── dyn/
    │   │   ├── dyn-analyst.agent.md             ← Análisis DQL, métricas y anomalías
    │   │   └── dyn-dql-assistant.agent.md       ← Constructor y validador de queries DQL
    │   ├── dp/
    │   │   └── dp-analyst.agent.md              ← Análisis de reportes gateway
    │   ├── ops/
    │   │   ├── ops-incident-responder.agent.md  ← Respuesta a incidentes (dyn + dp)
    │   │   └── ops-daily-summary.agent.md       ← Resumen diario de métricas
    │   └── core/
    │       ├── core-structure-monitor.agent.md  ← Sincronización automática de docs
    │       ├── core-output-polisher.agent.md    ← Mejora léxica y gramatical
    │       ├── core-ai-team-dev.agent.md        ← Equipo Nova/Sage/Milo para implementación
    │       └── core-atlassian-jira.agent.md     ← Requisitos → Jira épicas + historias
    ├── skills/
    │   ├── dyn-queries/SKILL.md             ← Biblioteca DQL, API config, patrones
    │   ├── dp-analysis/SKILL.md             ← Patrones de análisis, códigos de error
    │   ├── ops-incident/SKILL.md            ← Correlación y pasos de remediación
    │   ├── ops-report-templates/SKILL.md    ← Plantillas de reportes compartidas
    │   ├── ops-metrics-thresholds/SKILL.md  ← SLO/SLA, umbrales y glosario
    │   ├── core-structure-monitor/SKILL.md  ← Lógica de detección y sincronización
    │   ├── core-auto-sync/SKILL.md          ← Configuración de triggers automáticos
    │   ├── core-output-polisher/SKILL.md    ← Patrones de mejora léxica
    │   ├── core-gh-cli/SKILL.md             ← Referencia completa GitHub CLI
    │   └── core-naming-conventions/SKILL.md ← Criterios de decisión para nombres
    ├── prompts/
    │   ├── dyn/
    │   │   └── dyn-chatbot.prompt.md
    │   ├── dp/
    │   │   └── dp-analyst.prompt.md
    │   └── core/
    │       └── core-structure-monitor.prompt.md
    └── instructions/
        ├── core-naming-conventions.instructions.md ← Prefijos y commits (auto)
        └── core-markdown-style.instructions.md     ← Formato Markdown (auto)
```

## Componentes Principales

### Agentes operativos

| Agente | Invocación | Propósito |
| ------ | ---------- | --------- |
| Dynatrace Analyst | `@dyn-analyst` | Construye DQL, ejecuta queries, interpreta métricas y anomalías |
| DQL Assistant | `@dyn-dql-assistant` | Especialista único en construir/validar/optimizar queries DQL |
| DataPower Analyst | `@dp-analyst` | Analiza reportes del gateway, clasifica severidad, recomienda acciones |
| Incident Responder | `@ops-incident-responder` | Correlaciona anomalías Dynatrace ↔ DataPower y produce reporte de incidente |
| Daily Summary | `@ops-daily-summary` | Resumen diario automático con tendencias y top servicios degradados |

### Agentes de infraestructura

| Agente | Invocación | Propósito |
| ------ | ---------- | --------- |
| Structure Monitor | `@structure-monitor` | Mantiene README.md y diagramas Mermaid sincronizados con la estructura real |
| Output Polisher | `@output-polisher` | Última capa de pulido léxico antes de entregar al usuario |
| AI Team Dev | `@ai-team-dev` | Equipo Nova (frontend) + Sage (backend) + Milo (visual) para implementación |
| Atlassian Jira | `@atlassian-jira` | Convierte documentos de requisitos en épicas e historias de usuario en Jira |

### Skills

- **Dominio:** `dyn-queries`, `dp-analysis`
- **Cross-domain:** `ops-incident`, `ops-report-templates`, `ops-metrics-thresholds`
- **Infraestructura:** `core-structure-monitor`, `core-auto-sync`, `core-output-polisher`, `core-gh-cli`, `core-naming-conventions`

## Estado Actual

- ✓ 9 agentes definidos con frontmatter consistente
- ✓ 10 skills con conocimiento técnico estructurado
- ✓ 3 prompts reutilizables para invocaciones específicas
- ✓ 2 instructions auto-inyectadas (naming conventions + markdown style)
- ✓ Output Polisher como última capa de pulido lingüístico
- ✓ Structure Monitor mantiene la documentación sincronizada
- ⏳ Conexión real con API de Dynatrace
- ⏳ Conexión real con DataPower REST Management Interface
- ⏳ Chatbot orquestador principal
- ⏳ Tests automatizados

## Sincronización Manual

Para forzar una pasada del Structure Monitor:

```text
Sincroniza la documentación con los cambios actuales
```

O ejecutar `/structure-monitor-sync` desde Copilot.
