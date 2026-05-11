# Incident Response — Skill

Procedimiento técnico que sigue el agente [`ops-incident-responder`](../../agents/ops/ops-incident-responder.agent.md) para correlacionar anomalías, clasificar severidad y generar reportes de incidente.

---

## Paso 1 — Recopilación de Datos

### Desde Dynatrace

Ejecutar estas queries DQL en orden:

```dql
-- 1a. Problemas activos relacionados al servicio
fetch problems
| filter status == "OPEN"
| fields title, severity, impactLevel, startTime, affectedEntities
| sort startTime desc
```

```dql
-- 1b. Métricas del servicio en las últimas 2 horas
fetch spans
| filter service.name == "{servicio_afectado}"
| filter timestamp > now() - 2h
| summarize
    error_rate = countIf(http.status_code >= 500) / count() * 100,
    p95_ms = percentile(duration, 95),
    p99_ms = percentile(duration, 99),
    total_requests = count()
```

```dql
-- 1c. Servicios dependientes con errores
fetch spans
| filter timestamp > now() - 1h
| filter http.status_code >= 500
| summarize errors = count(), by: {service.name, http.status_code}
| sort errors desc
| limit 10
```

### Desde DataPower

<!-- TODO: conectar con API real cuando esté disponible -->

Revisar el reporte DataPower del período afectado buscando:

- Errores en el mismo `service` detectado en Dynatrace
- Códigos de error que expliquen el síntoma (ver [`dp-analysis/SKILL.md`](../dp-analysis/SKILL.md))
- Variaciones en `response_time` y `queue_depth`

---

## Paso 2 — Correlación

| Escenario | Señales | Causa probable |
| --------- | ------- | -------------- |
| **Solo Dynatrace** | Anomalía en spans, DataPower OK | Problema en la aplicación o backend |
| **Solo DataPower** | Errores en gateway, Dynatrace sin anomalía | Problema de conectividad o política |
| **Ambos sistemas** | Anomalías simultáneas en el mismo servicio | Problema de infraestructura compartida |
| **Ninguno** | Sin datos correlacionables | Servicio no instrumentado o problema de monitoreo |

---

## Paso 3 — Clasificación de Severidad

Aplicar los umbrales de [`ops-metrics-thresholds/SKILL.md`](../ops-metrics-thresholds/SKILL.md):

```log
CRITICAL si:
  - Error rate > 5% O
  - Latencia P95 > 2000ms O
  - Servicio completamente caído (availability = 0%)

WARNING si:
  - Error rate entre 1% y 5% O
  - Latencia P95 entre 500ms y 2000ms O
  - Degradación parcial (algunos endpoints afectados)

INFO si:
  - Métricas dentro de umbrales pero con tendencia negativa
```

---

## Paso 4 — Generación del Reporte

Completar la **Plantilla 1: Reporte de Incidente** de [`ops-report-templates/SKILL.md`](../ops-report-templates/SKILL.md) con:

- Datos recopilados en el Paso 1
- Resultado de la correlación del Paso 2
- Severidad clasificada en el Paso 3
- Acciones de remediación específicas al patrón identificado (Paso 5)

---

## Paso 5 — Acciones de Remediación por Patrón

### Backend caído (`0x00d30003` + latencia alta en Dynatrace)

1. Verificar estado del servidor backend
2. Revisar logs del backend para causa raíz
3. Activar failover si está configurado
4. Escalar a equipo de infraestructura

### Problema de certificados (`0x00d30006`)

1. Verificar fecha de expiración del certificado afectado
2. Si expiró: renovar y redeployar en DataPower
3. Si no expiró: verificar chain de confianza y hostname
4. Escalar a equipo de seguridad / PKI

### Sobrecarga de tráfico (CPU alta + latencia gradual)

1. Verificar si hay pico de tráfico anormal
2. Activar escalamiento horizontal si está configurado
3. Implementar rate limiting temporal si es posible
4. Escalar a equipo de arquitectura

### Falla de política (`0x80e00014`)

1. Revisar logs de DataPower para identificar qué regla rechazó
2. Verificar si hubo cambio reciente en políticas
3. Si es falso positivo: ajustar regla con aprobación del equipo de seguridad
4. Escalar a equipo de DataPower

---

## Integración con Otros Agentes

- Entrada: datos provistos por [`@dyn-analyst`](../../agents/dyn/dyn-analyst.agent.md) y [`@dp-analyst`](../../agents/dp/dp-analyst.agent.md)
- Salida final: pasar por [`@output-polisher`](../../agents/core/core-output-polisher.agent.md) antes de entregar al usuario
- Si el incidente requiere abrir tickets: derivar a [`@atlassian-requirements-to-jira`](../../agents/core/core-atlassian-jira.agent.md)

---

## Referencias

| Recurso | Propósito |
| ------- | --------- |
| [`ops-incident-responder.agent.md`](../../agents/ops/ops-incident-responder.agent.md) | Rol del agente y triggers |
| [`dyn-queries/SKILL.md`](../dyn-queries/SKILL.md) | Biblioteca DQL completa |
| [`dp-analysis/SKILL.md`](../dp-analysis/SKILL.md) | Códigos de error y patrones DataPower |
| [`ops-metrics-thresholds/SKILL.md`](../ops-metrics-thresholds/SKILL.md) | Umbrales SLO/SLA y glosario |
| [`ops-report-templates/SKILL.md`](../ops-report-templates/SKILL.md) | Plantillas de reportes |
