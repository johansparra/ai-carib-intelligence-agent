# Auto-Sync Configuration

Configuración para que el agente Structure Monitor se ejecute automáticamente después de cada cambio.

## Activación Automática

Este agente se ejecuta automáticamente cuando:

1. **Después de crear/modificar archivos en:**
   - `.github/agents/`
   - `.github/skills/`
   - `.github/prompts/`
   - `.github/customizations/`
   - `.github/toolboxes/`

2. **Cambios en estructura de carpetas**
   - Nueva carpeta creada
   - Carpeta eliminada
   - Estructura reorganizada

3. **Cambios en lógica**
   - Archivos de configuración modificados
   - README.md de componentes actualizados

## Instrucciones para Copilot

Cuando realices cambios en cualquier parte del proyecto:

```text
Ejecutar: /structure-monitor-sync
```

O simplemente di:

```text
"Sincroniza la documentación con los cambios actuales"
```

El agente automáticamente:

- Detectará los cambios
- Actualizará todos los README.md necesarios
- Corregirá automáticamente el formato Markdown en archivos `.md`, incluyendo saltos de línea después de encabezados y alrededor de listas tras `:`
- Regenerará los diagramas Mermaid
- Validará la consistencia
- Confirmará los cambios completados

## Cambios Detectables

### Carpetas

- ✓ Nueva carpeta en agents/
- ✓ Nueva carpeta en skills/
- ✓ Nueva carpeta en prompts/
- ✓ Renombrado de carpetas
- ✓ Eliminación de carpetas

### Archivos

- ✓ Nuevo archivo .md
- ✓ Nuevo archivo .prompt.md
- ✓ Nuevo archivo .yml/.yaml
- ✓ Eliminación de archivos
- ✓ Renombrado de archivos
- ✓ Corrección automática de formato Markdown en archivos `.md` (encabezados con línea en blanco)
- ✓ Corrección automática de listas tras `:` para evitar MD032 (listas deben estar rodeadas por líneas en blanco)

### Contenido

- ✓ Cambios en descripción de componentes
- ✓ Cambios en lógica de agentes
- ✓ Cambios en flujo de procesamiento

## Notas

- El agente **no requiere activación manual** después de cambios
- Los README.md se mantendrán siempre sincronizados
- Los diagramas Mermaid se regenerarán automáticamente
- Se aplicará corrección automática de formato Markdown en archivos `.md`, especialmente encabezados `##` y `###`
- Se aplicará corrección automática de listas tras `:` para evitar MD032
- La documentación reflejará la estructura actual del proyecto
