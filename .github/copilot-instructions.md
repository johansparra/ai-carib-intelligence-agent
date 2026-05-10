# Copilot Instructions

## Propósito del Proyecto

Este repositorio es la **plataforma de inteligencia para análisis de Dynatrace y DataPower**.

Proporciona:

- **Agente Dynatrace** - Conexión con Davis IA de Dynatrace, ejecución de DQL, detección de anomalías
- **Agente DataPower** - Análisis profesional de reportes de DataPower
- **Chatbot Copilot** - Orquestador central que coordina ambos agentes
- **Structure Monitor** - Agente automático que mantiene la documentación sincronizada
- **Incident Responder** - Correlaciona anomalías de Dynatrace y DataPower y genera reportes de incidente
- **Daily Summary** - Genera resúmenes automáticos diarios del estado de los sistemas
- **DQL Assistant** - Especializado en construir y validar queries DQL

---

## Contexto de Dominio

### Dynatrace

**Dynatrace** es una plataforma de observabilidad y monitoreo APM (Application Performance Monitoring). En este proyecto se usa para:

- Monitorear el rendimiento de servicios, hosts y procesos en producción
- Detectar anomalías automáticamente mediante **Davis AI** (motor de IA de Dynatrace)
- Ejecutar **queries DQL** (Dynatrace Query Language) para extraer métricas, logs y trazas
- Gestionar **SLO/SLA** de los servicios de la plataforma

Cuando el usuario menciona "métricas", "latencia", "errores", "trazas", "spans" o "anomalías", generalmente se refiere a datos de Dynatrace.

### DataPower

**IBM DataPower Gateway** es un appliance de integración empresarial que actúa como gateway de seguridad, transformación y enrutamiento de mensajes entre sistemas. En este proyecto se usa para:

- Procesar transacciones entre sistemas internos y externos
- Aplicar políticas de seguridad (autenticación, autorización, cifrado)
- Transformar formatos de mensajes (XML/SOAP/JSON/REST)
- Generar reportes de transacciones con métricas de rendimiento del gateway

Cuando el usuario menciona "gateway", "reporte", "transacciones", "domain", "policy" o códigos de error `0x00dXXXXX`, se refiere a DataPower.

---

## Estructura de Carpetas

```text
ai-carib-intelligence-agent/
├── README.md
├── CLAUDE.md
├── Comandos.md
└── .github/
    ├── copilot-instructions.md (este archivo)
    ├── agents/
    │   ├── dyn/
    │   │   ├── dyn-analyst.agent.md             ← Análisis DQL, métricas y anomalías
    │   │   └── dyn-dql-assistant.agent.md       ← Constructor y validador de queries DQL
    │   ├── dp/
    │   │   └── dp-analyst.agent.md              ← Análisis de reportes DataPower
    │   ├── ops/
    │   │   ├── ops-incident-responder.agent.md  ← Respuesta a incidentes (dyn + dp)
    │   │   └── ops-daily-summary.agent.md       ← Resumen diario de métricas
    │   └── core/
    │       ├── core-structure-monitor.agent.md  ← Sincronización automática de docs
    │       ├── core-output-polisher.agent.md    ← Mejora léxica y gramatical
    │       ├── core-ai-team-dev.agent.md
    │       └── core-atlassian-jira.agent.md
    ├── skills/
    │   ├── dyn-queries/SKILL.md             ← Biblioteca DQL, API config, patrones
    │   ├── dp-analysis/SKILL.md             ← Patrones de análisis, códigos de error
    │   ├── ops-incident/SKILL.md            ← Correlación y pasos de remediación
    │   ├── ops-report-templates/SKILL.md    ← Plantillas de reportes compartidas
    │   ├── ops-metrics-thresholds/SKILL.md  ← SLO/SLA, umbrales y glosario
    │   ├── core-structure-monitor/SKILL.md  ← Lógica de detección y sincronización
    │   ├── core-output-polisher/SKILL.md    ← Reglas de estilo y patrones de mejora
    │   ├── core-gh-cli/SKILL.md             ← Referencia completa GitHub CLI
    │   ├── core-auto-sync/SKILL.md          ← Config de auto-sincronización
    │   └── core-naming-conventions/SKILL.md ← Convenciones y reglas de formato
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

---

## Cómo Trabajar con Este Proyecto

### 1. Agente Dynatrace

Usa para:
- Conectar con Davis IA de Dynatrace
- Ejecutar consultas DQL
- Detectar anomalías
- Extraer métricas y logs

### 2. Agente DataPower

Usa para:
- Analizar reportes
- Generar insights profesionales
- Hacer recomendaciones basadas en datos

### 3. Structure Monitor (AUTOMÁTICO)

**No requiere activación manual.** Se ejecuta automáticamente cuando:
- Se crea/modifica cualquier archivo o carpeta
- Se cambia la estructura del proyecto
- Se actualiza la lógica de algún agente

---

## Instrucciones Clave para Copilot

### Regla #1: Separación de Componentes

**Mantén Dynatrace y DataPower completamente independientes.**
- Cambios en DataPower NO deben afectar a Dynatrace
- Cada agente tiene sus propios skills y prompts
- Si necesitas compartir lógica, usa `toolboxes/`

### Regla #2: Actualización Automática de Documentación

**El agente Structure Monitor actualiza README.md automáticamente.**
- NO edites diagramas Mermaid manualmente
- Cuando hagas cambios, el agente los detecta automáticamente
- Los README.md siempre reflejarán la estructura actual

### Regla #3: Commits Atómicos

**Cada cambio debe tener su propio commit.**
- Commit de estructura de carpetas
- Commit de lógica/skills
- Commit de prompts
- Permite rastrear cambios individuales

### Regla #4: Documentación Primero

**Actualiza la documentación junto con el código.**
- Si creas un nuevo skill, crea su README.md también
- Si modificas lógica, actualiza la descripción del componente
- Los diagramas se actualizarán automáticamente vía Structure Monitor

---

## Flujo de Trabajo Típico

1. **Usuario hace petición** → Chatbot Copilot recibe la solicitud
2. **Chatbot coordina agentes** → Activa Dynatrace o DataPower según sea necesario
3. **Agente ejecuta** → Realiza su trabajo (DQL queries, análisis, etc.)
4. **Datos fluyen** → De Dynatrace a DataPower si es necesario
5. **Reporte generado** → Chatbot ensambla respuesta final
6. **Structure Monitor verifica** → Detecta si hubo cambios en la estructura
7. **Documentación actualizada** → README.md y diagramas se sincronizan automáticamente

---

## Comandos Rápidos

### Sincronizar documentación manualmente

```
Copilot: "Sincroniza la documentación con los cambios actuales"
```
Esto ejecuta el agente `structure-monitor-sync` manualmente.

### Ver estructura actual

```
Copilot: "Muéstrame la estructura actual del proyecto"
```

### Listar cambios detectados

```
Copilot: "¿Qué cambios se han hecho en la estructura?"
```

---

## Consideraciones Importantes

- **Dynatrace y DataPower son independientes** - Actualiza uno sin afectar al otro
- **Documentación automática** - Structure Monitor mantiene todo sincronizado
- **Separación clara** - Cada componente en su propia carpeta
- **Escalabilidad** - Fácil agregar nuevos agentes sin romper los existentes

---

## Próximos Pasos

- [ ] Implementar conexión con Davis IA de Dynatrace
- [ ] Crear scripts de DQL queries en `skills/dyn/dyn-queries/`
- [ ] Desarrollar lógica de análisis en `skills/dp/dp-analysis/`
- [ ] Implementar orquestación en Chatbot principal
- [ ] Crear tests para cada componente

