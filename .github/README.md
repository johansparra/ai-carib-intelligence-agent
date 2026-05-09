# GitHub Copilot Structure

Esta carpeta contiene la estructura base para agentes, skills y prompts de GitHub Copilot.

## Carpetas Principales

- **`agents/`** - Agentes independientes por dominio
  - `dynatrace/` - Agente de Dynatrace para DQL y anomalías
  - `datapower/` - Agente de DataPower para análisis profesional
  - `structure-monitor/` - Agente automático de sincronización de docs

- **`skills/`** - Skills lógicos separados por integración
  - `dynatrace/` - Skills de consultas y detección
  - `datapower/` - Skills de análisis
  - `structure-monitor/` - Skills de detección y sincronización

- **`prompts/`** - Plantillas de prompts para cada rol/chatbot
  - `dynatrace-chatbot.prompt.md` - Chatbot para Dynatrace
  - `datapower-analyst.prompt.md` - Analista profesional de DataPower
  - `structure-monitor.prompt.md` - Agente automático de sincronización

- **`customizations/`** - Ajustes e instrucciones específicas
  - `auto-sync.md` - Configuración de auto-sincronización de documentación

- **`toolboxes/`** - Herramientas auxiliares y utilidades compartidas

## Cómo Funciona Structure Monitor

El agente `structure-monitor` funciona automáticamente:

1. **Detecta** cambios en carpetas, archivos o lógica
2. **Identifica** qué README.md necesitan actualizarse
3. **Regenera** diagramas Mermaid
4. **Sincroniza** referencias cruzadas
5. **Valida** consistencia de documentación

**No requiere activación manual** - trabaja transparentemente en segundo plano.
