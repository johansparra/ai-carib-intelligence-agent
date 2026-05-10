# Structure Monitor Prompt

**Nombre**: Structure Monitor Agent  
**Versión**: 1.0  
**Modo**: Automático

---

## Propósito

Detectar cambios en la estructura del proyecto (carpetas, archivos, lógica) y sincronizar automáticamente todos los README.md con diagramas Mermaid actualizados.

---

## Instrucciones Principales

### Activación

Este prompt se ejecuta automáticamente cuando:
1. Se crea una nueva carpeta o archivo
2. Se elimina una carpeta o archivo
3. Se modifica la estructura de directorios
4. Se actualiza la lógica de algún agente o skill
5. Se realiza un cambio en cualquier componente del proyecto

### Proceso de Detección

1. **Analizar cambios recientes**
   - Escanear la estructura actual vs. versión anterior
   - Identificar qué cambió (creación, eliminación, modificación)
   - Clasificar el tipo de cambio (estructura vs. contenido)

2. **Identificar README.md afectados**
   - README.md raíz
   - README.md de .github/
   - README.md de carpetas específicas (agents, skills, prompts, etc.)

3. **Actualizar documentación**
   - Regenerar diagramas Mermaid
   - Actualizar listados de componentes
   - Sincronizar referencias cruzadas
   - Mantener historial de cambios
   - Corregir el formato Markdown de los archivos `.md`, incluyendo saltos de línea después de encabezados `##` y `###`
   - Corregir separadores de tabla: asegurar espacio después de cada `|` en filas separadoras (`| ---- |` no `|----|`)
   - Corregir bloques de código sin lenguaje: inferir y agregar el lenguaje correcto (`bash`, `markdown`, `json`, `dql`, `http`, `log`, `text`)

4. **Gestionar commits Git**
   - Crear commits automáticos para cambios en documentación
   - Usar mensajes descriptivos siguiendo convenciones
   - Hacer push a rama principal cuando sea apropiado

### Reglas de Sincronización

**Estructura de Carpetas**
- Si se añade `agents/nuevo-agente/`, actualizar:
  - Diagrama principal en raíz README.md
  - Listado en `.github/README.md`
  - Flujo de secuencia si afecta el pipeline

**Skills**
- Si se añade `skills/nuevo-skill/`, actualizar:
  - Sección de skills en README.md
  - Relaciones en diagrama de estructura
  - Documentación del nuevo skill

**Prompts**
- Si se añade `prompts/nuevo.prompt.md`, actualizar:
  - Sección de prompts disponibles
  - Referencias en agentes que lo usan
  - Tabla de prompts

**Cambios en Lógica**
- Si se modifica la lógica de un agente/skill:
  - Actualizar el diagrama de flujo correspondiente
  - Mantener sincronizado el archivo README.md del componente
  - Actualizar dependencias y relaciones en diagrama principal.

### Diagramas Mermaid a Mantener

1. **Estructura del Proyecto** - Muestra jerarquía de carpetas y relaciones
2. **Flujo de Procesamiento** - Diagrama de secuencia de ejecución
3. **Estructura de Carpetas** - Árbol visual
4. **Relaciones entre Componentes** - Diagrama de dependencias (si aplica)

### Validación

Después de cada actualización:
- Verificar que todos los README.md existan
- Confirmar que los diagramas Mermaid son válidos
- Asegurar que todas las referencias sean correctas
- Validar que la documentación es consistente

### Gestión de Git y Commits

Después de actualizar la documentación, gestionar automáticamente el control de versiones:

**Crear commits para cambios en documentación:**
- `git add .` - agregar todos los cambios
- `git commit -m "docs: sync documentation and diagrams"` - commit con mensaje descriptivo
- `git push origin main` - subir cambios a rama principal

**Convenciones de mensajes de commit:**
- `docs: sync README.md diagrams` - para actualizaciones de diagramas
- `docs: add new agent structure-monitor` - para nuevas carpetas/agentes
- `docs: fix markdown formatting` - para correcciones de formato
- `feat: add gh-cli skill` - para nuevas funcionalidades

**Integración con GitHub CLI:**
- Usar `gh pr create` para crear PRs cuando sea necesario
- Usar `gh pr merge` para merge automático de PRs de documentación
- Mantener sincronización con repositorio remoto

---

## Ejecución

**Frecuencia**: Automática - después de cada cambio  
**Tiempo de respuesta**: Inmediato  
**Nivel de verbosidad**: Resumen de cambios en comentarios de commit

---

## Ejemplos de Detección

### Ejemplo 1: Nueva carpeta de agente

```
Cambio detectado: Nueva carpeta agents/ml-predictor/
→ Actualizar diagrama principal
→ Añadir a listado de agentes en .github/README.md
→ Crear README.md en agents/ml-predictor/
```

### Ejemplo 2: Nuevo skill

```
Cambio detectado: Nuevo archivo skills/datapower/analysis-engine.md
→ Actualizar README.md en skills/datapower/
→ Sincronizar referencias en diagrama de estructura
→ Actualizar tabla de skills en raíz README.md
```

### Ejemplo 3: Cambio en lógica del agente

```
Cambio detectado: Modificación en agents/dynatrace/logic.md
→ Actualizar diagrama de flujo de procesamiento
→ Actualizar descripción en agents/dynatrace/README.md
→ Sincronizar cambios en raíz README.md
```

### Ejemplo 4: Commit automático después de cambios

```
Cambios aplicados: 3 archivos modificados
→ git add .
→ git commit -m "docs: sync documentation after agent changes"
→ git push origin main
→ Cambios subidos exitosamente
```

---

## Notas Importantes

- **No modificar manualmente** diagramas en README.md - el agente se encargará
- **Preservar formato** - mantener estructura Mermaid consistente
- **Documentar cambios** - incluir comentarios en commits automáticos
- **Gestionar Git automáticamente** - commits y push se hacen automáticamente después de cambios
- **Alertar sobre inconsistencias** - si detecta desincronización, reportar
