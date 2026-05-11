---
name: structure-monitor-sync
description: Invoca al agente Structure Monitor para sincronizar la documentación con la estructura actual del proyecto.
---

# Structure Monitor — Invocación

Plantilla de invocación reutilizable para el agente [`structure-monitor`](../../agents/core/core-structure-monitor.agent.md).

---

## Invocación Estándar

```text
Sincroniza la documentación con los cambios actuales.
```

o equivalente:

```text
@structure-monitor revisa la estructura del proyecto y actualiza READMEs y diagramas si hace falta.
```

---

## Invocación con Alcance

Cuando solo quieras escanear una subcarpeta específica:

```text
@structure-monitor sincroniza únicamente .github/agents/ops/
```

```text
@structure-monitor regenera el diagrama Mermaid de .github/README.md
```

---

## Comportamiento Esperado

El agente:

1. Detecta diferencias entre estructura real y documentada
2. Actualiza únicamente los `README.md` y diagramas afectados
3. Aplica las reglas de formato definidas en [`core-markdown-style.instructions.md`](../../instructions/core-markdown-style.instructions.md)
4. Entrega un reporte breve y factual de los cambios

Si no hay discrepancias, reporta:

> No se requieren cambios de documentación.

---

## Notas

- El agente se activa también automáticamente — esta invocación manual es para forzar una pasada
- Las reglas técnicas (qué detectar, cómo generar diagramas, cómo validar) viven en [`core-structure-monitor/SKILL.md`](../../skills/core-structure-monitor/SKILL.md)
- El control de qué rutas se vigilan automáticamente vive en [`core-auto-sync/SKILL.md`](../../skills/core-auto-sync/SKILL.md)
