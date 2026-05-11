---
name: dyn-chatbot
description: >
  Invocación reutilizable del agente Dynatrace en modo conversacional.
  Traduce preguntas en español a queries DQL, ejecuta y entrega los
  resultados con interpretación en lenguaje natural.
---

# Dynatrace Chatbot — Invocación

Plantilla para conversar con el agente [`dyn-analyst`](../../agents/dyn/dyn-analyst.agent.md) sobre métricas, anomalías y observabilidad.

---

## Invocación Estándar

```text
@dyn-analyst {pregunta en español}
```

Ejemplos:

```text
@dyn-analyst ¿cuántos errores 5xx tuvo el servicio de pagos en la última hora?
@dyn-analyst ¿qué servicios tienen mayor latencia hoy?
@dyn-analyst ¿hay algún problema detectado por Davis AI ahora mismo?
@dyn-analyst ¿cómo está el CPU del servidor web-prod-01?
@dyn-analyst ¿el servicio de autenticación empeoró hoy vs ayer?
```

---

## Comportamiento del Agente

1. Identifica el tipo de dato necesario: métricas, logs, spans, problemas o entidades
2. Construye la query DQL usando la biblioteca de [`dyn-queries/SKILL.md`](../../skills/dyn-queries/SKILL.md)
3. Muestra la query antes de "ejecutarla" (permite revisarla)
4. Interpreta el resultado en lenguaje natural
5. Si detecta un problema, sugiere próximos pasos
6. Si la severidad es WARNING o CRITICAL, deriva a [`@ops-incident-responder`](../../agents/ops/ops-incident-responder.agent.md)

---

## Formato de Respuesta

```markdown
**Query DQL:**

\`\`\`dql
{query construida}
\`\`\`

**Resultado:**

| Columna 1 | Columna 2 | Columna 3 |
| --------- | --------- | --------- |
| valor     | valor     | valor     |

**Interpretación:**
{explicación breve del resultado}

**Alerta:** {solo si hay algo preocupante — usar ⚠️ o 🔴}
**Próximo paso:** {acción recomendada si aplica}
```

Indicadores visuales (usar con moderación):

- ✅ Métricas dentro de umbrales normales
- ⚠️ WARNING — monitorear de cerca
- 🔴 CRITICAL — acción inmediata requerida
- ℹ️ Información sin impacto operacional

---

## Manejo de "Sin Datos"

Si la query no retorna resultados, devolver:

```text
No se encontraron datos para esta consulta en el período indicado.

Posibles razones:
- El servicio no está siendo monitoreado en Dynatrace
- El timeframe está fuera del período de retención
- El filtro es demasiado específico

Sugerencia: {ampliar timeframe / verificar nombre del servicio / revisar instrumentación}
```

---

## Cuándo Derivar a Otros Agentes

| Situación | Derivar a |
| --------- | --------- |
| Necesitas una query DQL compleja o validarla | [`@dyn-dql-assistant`](../../agents/dyn/dyn-dql-assistant.agent.md) |
| El problema involucra DataPower también | [`@dp-analyst`](../../agents/dp/dp-analyst.agent.md) |
| Severidad CRITICAL detectada | [`@ops-incident-responder`](../../agents/ops/ops-incident-responder.agent.md) |
| Antes de entregar al usuario | [`@output-polisher`](../../agents/core/core-output-polisher.agent.md) |

---

## Referencias

| Recurso | Propósito |
| ------- | --------- |
| [`dyn-analyst.agent.md`](../../agents/dyn/dyn-analyst.agent.md) | Agente principal |
| [`dyn-queries/SKILL.md`](../../skills/dyn-queries/SKILL.md) | Biblioteca DQL |
| [`ops-metrics-thresholds/SKILL.md`](../../skills/ops-metrics-thresholds/SKILL.md) | Umbrales para clasificar severidad |
| [`ops-report-templates/SKILL.md`](../../skills/ops-report-templates/SKILL.md) | Plantillas si la respuesta requiere reporte |
