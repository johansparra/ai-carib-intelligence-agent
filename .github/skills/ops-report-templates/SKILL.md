# Report Templates

Plantillas de reportes compartidas por todos los agentes. Usar estas plantillas garantiza consistencia en la documentación de incidentes, análisis y resúmenes.

---

## Plantilla 1: Reporte de Incidente

Usar cuando: se detecta un problema activo que afecta servicios en producción.

```markdown
# Reporte de Incidente — {ID-INCIDENTE}

**Fecha:** {YYYY-MM-DD HH:MM}
**Severidad:** CRITICAL | WARNING
**Estado:** ACTIVO | RESUELTO | EN INVESTIGACIÓN

---

## Resumen Ejecutivo

{1-2 oraciones en lenguaje no técnico describiendo qué pasó y el impacto}

---

## Detalle Técnico

**Servicio afectado:** {nombre del servicio}
**Componente:** Dynatrace | DataPower | Ambos
**Síntoma observado:** {descripción técnica}
**Primer registro:** {timestamp}
**Última ocurrencia:** {timestamp o "en curso"}

---

## Métricas en el Momento del Incidente

| Métrica | Valor | Umbral normal |
|--------|-------|---------------|
| {métrica} | {valor} | {umbral} |

---

## Causa Raíz

{Descripción de la causa identificada o hipótesis más probable si aún se investiga}

---

## Impacto

- **Usuarios afectados:** {estimado o "desconocido"}
- **Transacciones fallidas:** {número o "desconocido"}
- **Servicios degradados:** {lista}
- **Tiempo de afectación:** {duración o "en curso"}

---

## Acciones Tomadas

| Hora | Acción | Responsable |
|------|--------|-------------|
| {HH:MM} | {descripción} | {nombre/equipo} |

---

## Próximos Pasos

1. {acción inmediata pendiente}
2. {acción a mediano plazo}
3. {acción preventiva}

---

## Lecciones Aprendidas

{completar al cierre del incidente}
```

---

## Plantilla 2: Resumen Ejecutivo

Usar cuando: se necesita comunicar el estado de los sistemas a audiencia no técnica (management, clientes).

```markdown
# Resumen Ejecutivo — {período}

**Generado:** {fecha}
**Período cubierto:** {inicio} — {fin}

---

## Estado General del Sistema

{VERDE / AMARILLO / ROJO} — {descripción en una oración}

---

## Disponibilidad

| Servicio | Disponibilidad | SLA objetivo |
|---------|----------------|--------------|
| {servicio} | {X.XX%} | {99.9%} |

---

## Incidentes del Período

| ID | Severidad | Duración | Impacto | Estado |
|----|-----------|----------|---------|--------|
| {ID} | {severidad} | {min/hrs} | {descripción} | {resuelto/abierto} |

---

## Tendencias

- **Latencia promedio:** {valor} ({+/-X%} vs período anterior)
- **Tasa de error:** {valor} ({+/-X%} vs período anterior)
- **Volumen de transacciones:** {valor} ({+/-X%} vs período anterior)

---

## Puntos de Atención

{Lista de 1-3 elementos que requieren seguimiento}
```

---

## Plantilla 3: Análisis de Tendencias

Usar cuando: se compara el desempeño de un período contra el anterior.

```markdown
# Análisis de Tendencias — {servicio} — {período}

**Generado:** {fecha}
**Período actual:** {inicio} — {fin}
**Período anterior:** {inicio} — {fin}

---

## Comparación de Métricas Clave

| Métrica | Período actual | Período anterior | Variación | Tendencia |
|--------|----------------|------------------|-----------|-----------|
| Latencia P95 | {valor} | {valor} | {+/-X%} | {↑↓→} |
| Error rate | {valor} | {valor} | {+/-X%} | {↑↓→} |
| Throughput | {valor} | {valor} | {+/-X%} | {↑↓→} |

---

## Top 3 Servicios con Mayor Degradación

1. **{servicio}** — {métrica degradada}: {valor actual} vs {valor anterior}
2. **{servicio}** — {métrica degradada}: {valor actual} vs {valor anterior}
3. **{servicio}** — {métrica degradada}: {valor actual} vs {valor anterior}

---

## Top 3 Servicios con Mejor Desempeño

1. **{servicio}** — {métrica mejorada}: {valor actual} vs {valor anterior}

---

## Análisis y Contexto

{Explicación de las variaciones más significativas, factores externos si aplica}

---

## Recomendaciones

{Acciones basadas en las tendencias identificadas}
```

---

## Plantilla 4: Alerta de Anomalía

Usar cuando: Davis AI o el análisis DataPower detecta una anomalía que requiere atención inmediata.

```markdown
# ALERTA DE ANOMALÍA — {severidad}

**Timestamp:** {YYYY-MM-DD HH:MM:SS}
**Fuente:** Dynatrace Davis AI | DataPower Analysis
**Servicio:** {nombre del servicio}

---

## Descripción

{Descripción clara de la anomalía en 1-2 oraciones}

---

## Evidencia

| Campo | Valor |
|-------|-------|
| Métrica afectada | {métrica} |
| Valor detectado | {valor} |
| Baseline esperado | {valor normal} |
| Desviación | {X%} |

---

## Acción Requerida

**Urgencia:** Inmediata | En las próximas 2 horas | Monitorear

{Descripción de la acción recomendada}

---

## Contacto de Escalamiento

<!-- TODO: completar con equipo y canal real -->
- **Equipo responsable:** {nombre del equipo}
- **Canal:** {Slack/Teams/email}
```
