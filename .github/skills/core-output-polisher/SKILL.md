# Output Polisher — Skill

Catálogo de patrones lingüísticos que el agente [`output-polisher`](../../agents/core/core-output-polisher.agent.md) detecta y corrige antes de entregar texto al usuario.

Inspirado en el patrón **Finnish Humanizer** de [awesome-copilot](https://awesome-copilot.github.com/), adaptado a español técnico-profesional.

---

## 1. Frases de Relleno — Eliminar

| Detectar | Reemplazar con |
| -------- | -------------- |
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

---

## 2. Nominalizaciones — Descomponer

| Detectar | Reemplazar con |
| -------- | -------------- |
| `la realización de` | `realizar` |
| `el análisis de` | `analizar` |
| `la implementación de` | `implementar` |
| `el procesamiento de` | `procesar` |
| `la detección de` | `detectar` |
| `la generación de` | `generar` |

---

## 3. Voz Pasiva Excesiva — Activar

| Detectar | Reemplazar con |
| -------- | -------------- |
| `se puede observar que` | *(afirmar directamente)* |
| `se realizó el análisis` | `el análisis muestra` |
| `fue detectado un error` | `se detectó un error` / `hay un error` |
| `ha sido identificado` | `identificamos` / `el sistema identificó` |

---

## 4. Corporatespeak — Desinflar

| Detectar | Reemplazar con |
| -------- | -------------- |
| `robusto` (sin justificación) | *(eliminar o especificar)* |
| `solución integral` | *(describir qué resuelve)* |
| `de primer nivel` | *(eliminar)* |
| `innovador` | *(eliminar o justificar)* |
| `sólido` | *(eliminar o especificar)* |
| `óptimo` | *(especificar qué lo hace óptimo)* |
| `de manera eficiente` | *(eliminar si no agrega info)* |

---

## 5. Aperturas Formulaicas — Reemplazar

| Detectar | Reemplazar con |
| -------- | -------------- |
| `En primer lugar, ...` | *(ir directo al primer punto)* |
| `Por otro lado, ...` | *(usar conector apropiado al contenido)* |
| `En conclusión, ...` | `En resumen:` / *(ir directo)* |
| `A modo de resumen, ...` | `En resumen:` |
| `Dicho lo anterior, ...` | *(eliminar)* |
| `Habiendo analizado todo lo anterior, ...` | *(eliminar)* |

---

## 6. Redundancias — Condensar

| Detectar | Reemplazar con |
| -------- | -------------- |
| `actualmente en este momento` | `actualmente` |
| `período de tiempo` | `período` |
| `resultado final` | `resultado` |
| `planificar con anticipación` | `planificar` |
| `volver a repetir` | `repetir` |
| `colaborar conjuntamente` | `colaborar` |

---

## 7. Hedging Excesivo — Afirmar con Confianza

| Detectar | Reemplazar con |
| -------- | -------------- |
| `podría potencialmente` | `puede` |
| `en cierta medida` | *(especificar o eliminar)* |
| `sería posible que` | `puede que` / afirmar directo si hay certeza |
| `aparentemente` | *(eliminar si hay datos que lo respaldan)* |
| `presumiblemente` | *(eliminar si hay evidencia)* |

---

## 8. Reglas de Estilo Profesional

### Brevedad con precisión

- Preferir oraciones de 15-25 palabras
- Una idea por oración
- Si una oración supera 30 palabras, dividirla

### Terminología consistente

- Usar siempre el mismo término para el mismo concepto (no alternar "gateway" con "pasarela" con "servidor")
- Términos canónicos del proyecto: `gateway`, `servicio`, `agente`, `métrica`, `anomalía`, `reporte`

### Números y datos al frente

- ❌ "Se observó una latencia elevada de aproximadamente 1200 milisegundos"
- ✅ "Latencia: 1200ms — supera el umbral de 500ms"

### Verbos activos sobre sustantivos abstractos

- ❌ "Se llevó a cabo la implementación de la corrección"
- ✅ "Se implementó la corrección"

---

## 9. Flujo de Aplicación

**Textos cortos (< 200 palabras):** aplicar correcciones directamente, sin lista previa.

**Textos largos (> 200 palabras):**

1. Escanear el texto e identificar patrones presentes
2. Listar qué patrones se encontraron y cuántas instancias
3. Aplicar todas las correcciones
4. Mostrar el texto corregido

---

## 10. Alcance

Aplica a las salidas de:

- `@dyn-analyst` — análisis de métricas y anomalías
- `@dp-analyst` — análisis de reportes DataPower
- `@ops-incident-responder` — reportes de incidente
- `@ops-daily-summary` — resúmenes diarios
- `@dyn-dql-assistant` — explicaciones de queries

**No aplica a:** queries DQL, código, JSON, tablas de datos crudos, ni a cualquier contenido dentro de bloques de código.

---

## 11. Modo Bilingüe (ES / EN)

Cuando el agente se invoca con `--bilingual` o frase equivalente (`bilingüe:`, `entrega también en inglés`), produce ambas versiones del texto en la misma respuesta.

### Qué se traduce y qué no

| Elemento | Acción |
| -------- | ------ |
| Texto prosa / explicaciones | Traducir aplicando los patrones de pulido en cada idioma |
| Severidad (`CRITICAL`, `WARNING`, `INFO`) | Mantener — son términos canónicos |
| Códigos de error (`0x00d30003`, `0x80e00014`) | Mantener literal |
| Nombres de servicios (`PaymentService`, `payment-service`) | Mantener literal |
| Métricas / siglas (`P95`, `P99`, `MTTR`, `MTBF`, `TPS`, `SLO`, `SLA`) | Mantener — son siglas técnicas universales |
| Queries DQL / código / JSON / bloques de código | Mantener literal — nunca traducir |
| Headers de tablas (`Métrica` → `Metric`) | Traducir |
| Valores de tablas (números, timestamps, IDs) | Mantener |
| URLs y rutas de archivo | Mantener literal |
| Comandos `gh`, `git`, shell | Mantener literal |

### Orden de operaciones

1. Aplica los patrones de pulido del español (secciones 1-7) sobre el texto original
2. Traduce el texto pulido al inglés, conservando los elementos de la tabla anterior
3. Aplica los mismos principios de estilo profesional (sección 8) en inglés:
   - Oraciones cortas (15-25 palabras)
   - Verbos activos sobre nominalizaciones
   - Sin hedging innecesario (`could potentially` → `can`)
   - Sin corporatespeak (`robust solution` → especificar qué resuelve)
4. Entrega ambas versiones con el separador visual definido en el agente

### Equivalencias clave de terminología

| Español canónico | English canonical |
| ---------------- | ----------------- |
| servicio | service |
| gateway | gateway *(no traducir)* |
| anomalía | anomaly |
| umbral | threshold |
| ventana de tiempo | timeframe |
| causa raíz | root cause |
| escalamiento | escalation |
| reporte de incidente | incident report |
| disponibilidad | availability |
| tasa de error | error rate |
| latencia | latency |
| tiempo de respuesta | response time |
| degradación | degradation |
| cola / pila | queue |
| reintento | retry |
| picos de tráfico | traffic spikes |

**Regla:** si dudas entre dos traducciones, elige el término del glosario de [`ops-metrics-thresholds/SKILL.md`](../ops-metrics-thresholds/SKILL.md).

### Ejemplo end-to-end

**Input al polisher (salida cruda con `--bilingual`):**

```text
En primer lugar, cabe destacar que el servicio PaymentService ha experimentado
una situación de incremento de la latencia que podría potencialmente estar
relacionada con una problemática de conectividad a nivel del backend.
Severidad CRITICAL. Código de error: 0x00d30003. Latencia P95: 1240ms.
```

**Output del polisher:**

```text
🇪🇸 **Español**

PaymentService presenta latencia elevada (P95: 1240ms). Causa probable:
timeout de conexión al backend (`0x00d30003`).

**Severidad:** CRITICAL

---

🇬🇧 **English**

PaymentService is showing elevated latency (P95: 1240ms). Likely cause:
backend connection timeout (`0x00d30003`).

**Severity:** CRITICAL
```

### Cuándo NO usar modo bilingüe

- Mensajes muy cortos (< 30 palabras) — agregan ruido sin valor
- Reportes que ya viven en plantillas estructuradas (`ops-report-templates`) — esos se generan en un solo idioma por consistencia con la plantilla
- Salidas de `@dyn-dql-assistant` cuando solo entrega la query — no hay prosa que traducir

---

## 12. Referencias

| Recurso | Propósito |
| ------- | --------- |
| [`core-output-polisher.agent.md`](../../agents/core/core-output-polisher.agent.md) | Rol del agente y triggers |
| [`ops-metrics-thresholds/SKILL.md`](../ops-metrics-thresholds/SKILL.md) | Terminología técnica que NO debe ser modificada y glosario canónico |
