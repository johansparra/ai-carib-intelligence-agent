# Output Polisher Skill

Skill que mejora la calidad léxica y gramatical de las salidas generadas por los agentes del proyecto. Actúa como post-procesador de cualquier texto antes de entregarlo al usuario.

Inspirado en el patrón del **Finnish Humanizer** de [awesome-copilot](https://awesome-copilot.github.com/), adaptado para español técnico-profesional.

---

## Propósito

Los agentes (Dynatrace, DataPower, Incident Responder, Daily Summary) generan textos funcionales pero con patrones típicos de IA que suenan artificiales, redundantes o poco naturales. Este skill los detecta y corrige antes de que el texto llegue al usuario.

---

## Patrones a Detectar y Corregir

### 1. Frases de relleno de IA — eliminar

| Detectar | Reemplazar con |
| ---------- | ---------------- |
| `cabe destacar que` | *(eliminar, ir directo al punto)* |
| `es importante señalar que` | *(eliminar)* |
| `en este sentido` | *(eliminar)* |
| `a tal efecto` | *(eliminar)* |
| `como se puede observar` | *(eliminar)* |
| `en el contexto de` | *(simplificar)* |
| `con respecto a` | `sobre` / `en` |
| `en lo que se refiere a` | `sobre` / `en cuanto a` |
| `a los efectos de` | `para` |
| `en virtud de` | `por` / `dado que` |
| `de conformidad con` | `según` |
| `con miras a` | `para` |

### 2. Nominalizaciones innecesarias — descomponer

| Detectar | Reemplazar con |
| ---------- | ---------------- |
| `la realización de` | `realizar` |
| `el análisis de` | `analizar` |
| `la implementación de` | `implementar` |
| `el procesamiento de` | `procesar` |
| `la detección de` | `detectar` |
| `la generación de` | `generar` |

### 3. Voz pasiva excesiva — activar

| Detectar | Reemplazar con |
| ---------- | ---------------- |
| `se puede observar que` | *(eliminar, afirmar directamente)* |
| `se realizó el análisis` | `el análisis muestra` |
| `fue detectado un error` | `se detectó un error` / `hay un error` |
| `ha sido identificado` | `identificamos` / `el sistema identificó` |

### 4. Corporatespeak inflado — desinflar

| Detectar | Reemplazar con |
| ---------- | ---------------- |
| `robusto` (sin justificación) | *(eliminar o especificar qué lo hace robusto)* |
| `solución integral` | *(describir qué resuelve exactamente)* |
| `de primer nivel` | *(eliminar)* |
| `innovador` | *(eliminar o justificar)* |
| `sólido` | *(eliminar o especificar)* |
| `óptimo` | *(especificar qué lo hace óptimo)* |
| `de manera eficiente` | *(eliminar si no agrega info)* |

### 5. Aperturas formulaicas — reemplazar

| Detectar | Reemplazar con |
| ---------- | ---------------- |
| `En primer lugar, ...` | *(ir directo al primer punto)* |
| `Por otro lado, ...` | *(usar el conector apropiado al contenido)* |
| `En conclusión, ...` | `En resumen:` / *(ir directo)* |
| `A modo de resumen, ...` | `En resumen:` |
| `Dicho lo anterior, ...` | *(eliminar)* |
| `Habiendo analizado todo lo anterior, ...` | *(eliminar)* |

### 6. Redundancias — condensar

| Detectar | Reemplazar con |
| ---------- | ---------------- |
| `actualmente en este momento` | `actualmente` |
| `período de tiempo` | `período` |
| `resultado final` | `resultado` |
| `planificar con anticipación` | `planificar` |
| `volver a repetir` | `repetir` |
| `colaborar conjuntamente` | `colaborar` |

### 7. Hedging excesivo — afirmar con confianza

| Detectar | Reemplazar con |
| ---------- | ---------------- |
| `podría potencialmente` | `puede` |
| `en cierta medida` | *(especificar o eliminar)* |
| `sería posible que` | `puede que` / afirmar directamente si hay certeza |
| `aparentemente` | *(eliminar si hay datos que lo respaldan)* |
| `presumiblemente` | *(eliminar si hay evidencia)* |

---

## Reglas de Estilo Profesional

### Brevedad con precisión

- Preferir oraciones de 15-25 palabras
- Una idea por oración
- Si una oración supera 30 palabras: dividirla

### Terminología consistente

- Usar siempre el mismo término para el mismo concepto (no alternar "gateway" con "pasarela" con "servidor")
- En este proyecto: `gateway`, `servicio`, `agente`, `métrica`, `anomalía`, `reporte`

### Números y datos siempre al frente

- ❌ "Se observó una latencia elevada de aproximadamente 1200 milisegundos"
- ✅ "Latencia: 1200ms — supera el umbral de 500ms"

### Verbos activos sobre sustantivos abstractos

- ❌ "Se llevó a cabo la implementación de la corrección"
- ✅ "Se implementó la corrección"

---

## Flujo de Aplicación

Para textos **cortos** (< 200 palabras): aplicar correcciones directamente sin consultar.

Para textos **largos** (> 200 palabras):

1. Escanear el texto completo e identificar patrones presentes
2. Listar qué patrones se encontraron y cuántas instancias
3. Aplicar todas las correcciones
4. Mostrar el texto corregido

---

## Alcance

Este skill aplica a las salidas de:

- `@dyn-analyst` — análisis de métricas y anomalías
- `@dp-analyst` — análisis de reportes
- `@ops-incident-responder` — reportes de incidente
- `@ops-daily-summary` — resúmenes diarios
- `@dyn-dql-assistant` — explicaciones de queries

**No aplica a:** queries DQL, código, JSON, tablas de datos crudos, o contenido dentro de bloques de código.
