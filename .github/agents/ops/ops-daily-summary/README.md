# Daily Summary Agent

Agente que genera resúmenes automáticos diarios del estado de los sistemas, comparando métricas del día anterior y destacando servicios degradados.

---

## Propósito

- Proporcionar visibilidad diaria del estado de la plataforma
- Identificar tendencias negativas antes de que escalen a incidentes
- Generar reportes listos para compartir con stakeholders

---

## Cuándo Activar

**Automático:** Al inicio del día laboral (configurar horario real)

<!-- TODO: configurar horario y canal de distribución -->

```text
Horario sugerido: 08:00 hora local
Canal de distribución: {Slack/Teams/email - por configurar}
Destinatarios: {equipos de operaciones y management - por configurar}
```

**Bajo demanda:**

```bash
@ops-daily-summary genera el resumen del día
@ops-daily-summary resumen de la última semana
```

---

## Proceso de Generación

### 1. Recopilar métricas de Dynatrace (últimas 24 horas)

```dql
-- Disponibilidad por servicio
fetch metrics
| metric builtin:synthetic.http.availability
| filter timestamp > now() - 24h
| summarize availability = avg(value), by: {monitor.name}
| sort availability asc
```

```dql
-- Top servicios con más errores
fetch spans
| filter timestamp > now() - 24h
| summarize
    total = count(),
    errors = countIf(http.status_code >= 500),
    by: {service.name}
| fields service.name, error_rate = errors / total * 100
| filter error_rate > 0.1
| sort error_rate desc
| limit 10
```

```dql
-- Comparación latencia P95: hoy vs ayer
fetch spans
| summarize
    hoy = percentile(duration, 95) { timestamp > now() - 24h },
    ayer = percentile(duration, 95) { timestamp > now() - 48h AND timestamp < now() - 24h },
    by: {service.name}
| fields service.name, hoy, ayer, variacion = (hoy - ayer) / ayer * 100
| sort variacion desc
| limit 10
```

### 2. Revisar problemas del período

```dql
fetch problems
| filter startTime > now() - 24h
| fields title, severity, duration, impactLevel
| sort severity asc
```

### 3. Calcular resumen de DataPower

Agregar del reporte DataPower del día:

- Total de transacciones procesadas
- Error rate promedio del día
- Gateway con mayor carga
- Servicio con mayor tiempo de respuesta

### 4. Generar reporte

Usar la **Plantilla 2: Resumen Ejecutivo** y la **Plantilla 3: Análisis de Tendencias** de `.github/toolboxes/report-templates.md`.

---

## Estructura del Reporte Diario

```markdown
# Resumen del Sistema — {fecha}

## Estado General: {🟢 NORMAL | 🟡 ATENCIÓN | 🔴 CRÍTICO}

## Highlights del día
- {punto más importante del día}
- {segundo punto}
- {tercer punto}

## Disponibilidad
[tabla de disponibilidad por servicio]

## Top 3 Servicios con Mayor Degradación
1. {servicio} — {métrica y valor}
2. {servicio} — {métrica y valor}
3. {servicio} — {métrica y valor}

## Incidentes del Período
[tabla de incidentes si los hubo, "Sin incidentes" si no]

## Tendencia vs Día Anterior
[tabla comparativa]

## Puntos de Atención para Mañana
{lista de elementos a monitorear}
```

---

## Recursos

- **Umbrales y SLO:** `.github/toolboxes/common-metrics.md`
- **Plantillas:** `.github/toolboxes/report-templates.md`
- **Queries DQL:** `.github/skills/dyn/dyn-queries/README.md`
- **Análisis DataPower:** `.github/skills/dp/dp-analysis/README.md`
