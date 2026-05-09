# ai-carib-intelligence-agent

Estructura inicial creada para soportar GitHub Copilot con carpetas de skills, agents y prompts.

## 📊 Estructura del Proyecto

```mermaid
graph TB
    subgraph ".github"
        subgraph "agents"
            DN["Dynatrace Agent"]
            DP["DataPower Agent"]
            SM["Structure Monitor<br/>(Automático)"]
        end
        subgraph "skills"
            DNSKILL["Dynatrace Skills<br/>- DQL Queries<br/>- Anomaly Detection"]
            DPSKILL["DataPower Skills<br/>- Report Analysis<br/>- Professional Insights"]
            SMSKILL["Structure Monitor Skills<br/>- Change Detection<br/>- Auto-Sync Docs"]
        end
        subgraph "prompts"
            DNPROMPT["dynatrace-chatbot<br/>Prompt"]
            DPPROMPT["datapower-analyst<br/>Prompt"]
            SMPROMPT["structure-monitor<br/>Prompt"]
        end
        CUSTOM["customizations<br/>(auto-sync.md)"]
        TOOLS["toolboxes"]
    end
    
    DN -.-> DNSKILL
    DNSKILL -.-> DNPROMPT
    DP -.-> DPSKILL
    DPSKILL -.-> DPPROMPT
    SM -.-> SMSKILL
    SMSKILL -.-> SMPROMPT
    CUSTOM -.-> SM
    
    style DN fill:#1f77b4
    style DP fill:#ff7f0e
    style SM fill:#2ca02c
    style DNSKILL fill:#1f77b4
    style DPSKILL fill:#ff7f0e
    style SMSKILL fill:#2ca02c
```

## 🔄 Flujo de Procesamiento

```mermaid
sequenceDiagram
    participant User as Usuario
    participant Chatbot as Chatbot Copilot
    participant DN as Dynatrace Agent
    participant DQL as DQL Engine
    participant DP as DataPower Agent
    participant Report as Reporte
    participant SM as Structure Monitor

    User->>Chatbot: Solicitud de análisis
    Chatbot->>DN: Activar Dynatrace Agent
    DN->>DQL: Ejecutar DQL Query
    DQL-->>DN: Resultados / Anomalías
    DN-->>Chatbot: Datos procesados
    Chatbot->>DP: Enviar datos a DataPower Agent
    DP->>DP: Análisis profesional
    DP-->>Chatbot: Conclusiones
    Chatbot-->>Report: Generar reporte
    Report-->>User: Reporte final
    
    par Monitoreo Automático
        SM->>SM: Detectar cambios
        SM->>SM: Actualizar README.md
        SM->>SM: Sincronizar diagramas
    end
```

## 📁 Estructura de Carpetas

```bash
ai-carib-intelligence-agent/
├── README.md (este archivo)
└── .github/
    ├── README.md (documentación de estructura)
    ├── agents/
    │   ├── dynatrace/
    │   │   └── README.md
    │   ├── datapower/
    │   │   └── README.md
    │   └── structure-monitor/
    │       └── README.md
    ├── skills/
    │   ├── dynatrace/
    │   │   └── README.md
    │   ├── datapower/
    │   │   └── README.md
    │   └── structure-monitor/
    │       └── README.md
    ├── prompts/
    │   ├── dynatrace-chatbot.prompt.md
    │   ├── datapower-analyst.prompt.md
    │   └── structure-monitor.prompt.md
    ├── customizations/
    │   ├── README.md
    │   └── auto-sync.md
    └── toolboxes/
        └── README.md
```

## 🎯 Componentes Principales

### 1. **Dynatrace Agent** 🔵

- Conexión con Davis IA de Dynatrace
- Ejecución de consultas DQL
- Detección de anomalías
- Extracción de métricas y logs

### 2. **DataPower Agent** 🟠

- Análisis profesional de reportes
- Generación de insights
- Recomendaciones basadas en datos
- Componente independiente sin dependencias externas

### 3. **Chatbot Copilot** 💬

- Coordinador central
- Orquesta el flujo entre agentes
- Genera reportes finales

### 4. **Structure Monitor** 🟢

- Detecta cambios automáticamente
- Sincroniza documentación
- Actualiza diagramas Mermaid
- Funciona sin intervención manual

## 🤖 Agente Structure Monitor

Un agente especial que **automáticamente**:

- Detecta cambios en la estructura del proyecto
- Actualiza todos los README.md necesarios
- Regenera diagramas Mermaid
- Sincroniza referencias cruzadas
- Valida consistencia de documentación

**No requiere activación manual** - funciona transparentemente en segundo plano.

Detalles en: [.github/prompts/structure-monitor.prompt.md](.github/prompts/structure-monitor.prompt.md)

## ✅ Estado Actual

- ✓ Estructura de carpetas creada
- ✓ Agentes separados (Dynatrace y DataPower)
- ✓ Agente Structure Monitor implementado
- ✓ Prompts inicializados
- ✓ Skills organizados por dominio
- ✓ Auto-sincronización de documentación configurada
- ✓ Espacio para customizaciones y toolboxes
- ⏳ Implementación de conexiones específicas
- ⏳ Scripts de integración
- ⏳ Tests automatizados
