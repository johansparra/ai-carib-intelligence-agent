# Copilot Instructions

## Propósito del Proyecto

Este repositorio es la **plataforma de inteligencia para análisis de Dynatrace y DataPower**. 

Proporciona:
- **Agente Dynatrace** - Conexión con Davis IA de Dynatrace, ejecución de DQL, detección de anomalías
- **Agente DataPower** - Análisis profesional de reportes de DataPower
- **Chatbot Copilot** - Orquestador central que coordina ambos agentes
- **Structure Monitor** - Agente automático que mantiene la documentación sincronizada

---

## Estructura de Carpetas

```
ai-carib-intelligence-agent/
├── README.md (documentación principal con diagramas)
└── .github/
    ├── copilot-instructions.md (este archivo)
    ├── agents/
    │   ├── dynatrace/          ← Agente para Dynatrace/Davis
    │   ├── datapower/          ← Agente para análisis de DataPower
    │   └── structure-monitor/  ← Agente que sincroniza documentación
    ├── skills/
    │   ├── dynatrace/          ← Skills de consultas DQL y anomalías
    │   ├── datapower/          ← Skills de análisis profesional
    │   └── structure-monitor/  ← Skills de detección y sincronización
    ├── prompts/
    │   ├── dynatrace-chatbot.prompt.md
    │   ├── datapower-analyst.prompt.md
    │   └── structure-monitor.prompt.md
    ├── customizations/
    │   ├── README.md
    │   └── auto-sync.md        ← Configuración de auto-sincronización
    └── toolboxes/
        └── README.md
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
- [ ] Crear scripts de DQL queries en `skills/dynatrace/`
- [ ] Desarrollar lógica de análisis en `skills/datapower/`
- [ ] Implementar orquestación en Chatbot principal
- [ ] Crear tests para cada componente

