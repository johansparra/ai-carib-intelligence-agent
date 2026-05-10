# Comandos y variables de GitHub Copilot Chat

Este documento explica los comandos de barra diagonal y las variables de chat disponibles en GitHub Copilot.

## Comandos de barra diagonal integrados

|#|Comando|Descripción|
|-|-------|-----------|
|1|`/help`|Obtiene ayuda sobre el uso de GitHub Copilot|
|2|`/doc`|Genera documentación para el código|
|3|`/clear`|Inicia una nueva sesión de chat|
|4|`/explain`|Explica cómo funciona el código seleccionado|
|5|`/tests`|Genera pruebas unitarias para el código seleccionado|
|6|`/fix`|Proporciona una corrección para el código seleccionado|
|7|`/new`|Crea un nuevo área de trabajo con scaffolding. Solo usa el mensaje de chat como contexto|
|8|`/newNotebook`|Crea un nuevo Jupyter Notebook. Solo usa el mensaje de chat como contexto|
|9|`/review`|Revisa el código para identificar problemas, mejoras y mejores prácticas|
|10|`/refactor`|Refactoriza el código para mejorar legibilidad y rendimiento|
|11|`/simplify`|Simplifica código complejo manteniendo funcionalidad|
|12|`/optimize`|Optimiza el código para rendimiento o eficiencia|
|13|`/debug`|Ayuda a diagnosticar y resolver errores|
|14|`/generate`|Genera código basado en descripción natural|
|15|`/summary`|Genera resumen del código o cambios|

## Uso combinado: comandos, participantes y variables

### ¿Qué es `#codebase`?

`#codebase` es una variable especial que realiza una búsqueda en el proyecto y agrega el código más relevante al contexto de la petición.

- Cuando usas `#codebase`, el modelo conserva el control y puede combinar la búsqueda de la base de código con otras herramientas.
- `#codebase` se puede usar en todos los modos de chat: Preguntar, Agente y Plan.

Algunos comandos pueden combinarse con variables y con el participante `@vscode` para obtener resultados más precisos.

|#|Comando|Descripción|
|-|-------|-----------|
|1|`#codebase /explain`|Genera una explicación sobre el área de trabajo completa|
|2|`#codebase /fix` o `/fix`|Propone una corrección para los problemas del código seleccionado|
|3|`#codebase /tests` o `/tests`|Genera pruebas unitarias para el código seleccionado|
|4|`#codebase /new` o `/new`|Aplica scaffolding al código para una nueva área de trabajo|
|5|`#codebase /newNotebook` o `/newNotebook`|Crea un nuevo Jupyter Notebook|
|6|`@vscode`|Preguntas sobre características, configuración y API de VS Code. Ej: `@vscode how to enable word wrapping?`|

## Variables de chat

Las variables de chat permiten especificar el contexto que se debe usar en la petición. Se escriben con el símbolo `#`.

Por ejemplo:

- `#selection` - Es la selección de texto actual en el editor activo.

Usar variables ayuda a centrar la petición en un fragmento concreto de código o en un archivo específico.

Ejemplo:

- `qué algoritmo de ordenación se usa en #selection`

En este caso, la petición se enfoca en el fragmento seleccionado.

## Ejemplos de variables de chat integradas

- `#editor` - El código fuente visible en el editor activo.
- `#selection` - La selección actual en el editor activo. El contenido se incluye de forma implícita en el contexto de la vista de chat.
- `#<file or folder name>` - Agrega el contenido de un archivo o carpeta al contexto, usando su nombre.
- `#codebase` - Agrega contenido relevante del área de trabajo como contexto al mensaje.
- `#terminalSelection` - La selección actual en el terminal activo.
- `#terminalLastCommand` - El último comando ejecutado en el terminal activo.
