# Auto-Sync — Configuración de Triggers

Define **qué rutas y qué tipos de cambio** disparan automáticamente al agente [`structure-monitor`](../../agents/core/core-structure-monitor.agent.md).

> Este skill es **solo configuración**. La lógica de detección y actualización vive en [`core-structure-monitor/SKILL.md`](../core-structure-monitor/SKILL.md). Las reglas de formato Markdown viven en [`core-markdown-style.instructions.md`](../../instructions/core-markdown-style.instructions.md).

---

## Rutas Vigiladas

El agente se activa automáticamente cuando hay cambios en cualquiera de estas rutas:

```text
.github/agents/**
.github/skills/**
.github/prompts/**
.github/instructions/**
.github/copilot-instructions.md
README.md
```

---

## Eventos que Disparan Activación

| Evento | Activa | Notas |
| ------ | ------ | ----- |
| Carpeta creada | Sí | Verificar si requiere `README.md` base |
| Carpeta eliminada | Sí | Limpiar referencias en READMEs e índices |
| Carpeta renombrada | Sí | Actualizar todas las referencias |
| Archivo `.agent.md` agregado/eliminado | Sí | Regenerar inventario de agentes |
| Archivo `.prompt.md` agregado/eliminado | Sí | Regenerar inventario de prompts |
| Archivo `SKILL.md` agregado/eliminado | Sí | Regenerar inventario de skills |
| Archivo `.instructions.md` agregado/eliminado | Sí | Regenerar inventario de instrucciones |
| Cambio en `description` de frontmatter | Sí | Refrescar tabla del README correspondiente |
| Cambio en cuerpo (sin tocar rol/relaciones) | No | No es cambio estructural |
| Cambio de formato Markdown puro | No | Se aplica vía `core-markdown-style.instructions.md` |

---

## Activación Manual

Cuando el usuario quiera forzar una pasada sin haber hecho cambios:

```text
"Sincroniza la documentación con los cambios actuales"
/structure-monitor-sync
```

Ver la plantilla completa en [`core-structure-monitor.prompt.md`](../../prompts/core/core-structure-monitor.prompt.md).

---

## Política

- El agente **no requiere activación manual** tras cambios — el trigger automático debe bastar
- Si la documentación ya está sincronizada, el agente reporta No-Op y no toca archivos
- Las correcciones de formato Markdown se aplican como efecto secundario al actualizar un README, no como objetivo principal
