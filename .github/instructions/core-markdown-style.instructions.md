---
applyTo: "**/*.md"
---

# Reglas de Formato Markdown — ai-carib-intelligence-agent

Aplica estas reglas a todos los archivos `.md` del proyecto.

## Reglas Obligatorias

1. **Línea en blanco después de encabezados** `##` y `###`

2. **Listas precedidas por `:` rodeadas de líneas en blanco** (evita MD032)

3. **Una sola línea en blanco entre secciones** — nunca dos o más consecutivas

4. **Separadores de tabla con espacios internos**

   ```markdown
   ❌ |----------|----------------|
   ✅ | ---------- | ---------------- |
   ```

5. **Bloques de código con lenguaje siempre declarado**

   ```markdown
   ❌ ```
   ✅ ```bash  ```json  ```dql  ```text  ```markdown  ```http  ```log
   ```

6. **Tablas con encabezado y separador** en todas las columnas

## Marcado de TODOs

Para integraciones pendientes con APIs reales:

```markdown
<!-- TODO: reemplazar con valor real -->
<!-- TODO: configurar cuando se tenga acceso a la API -->
```

Buscar todos los pendientes: `grep -r "TODO" .github/`
