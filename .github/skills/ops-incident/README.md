# Incident Responder Skill

Skill que implementa la lógica de correlación y generación de reportes para el agente `incident-responder`.

---

## Paso 1: Recopilación de Datos

### Desde Dynatrace

Ejecutar las siguientes queries DQL en orden:

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
- Códigos de error que expliquen el síntoma
- Variaciones en `response_time` y `queue_depth`

---

## Paso 2: Correlación

Determinar si el problema está en:

| Escenario | Señales | Causa probable |
| ----------- | --------- | ---------------- |
| **Solo Dynatrace** | Anomalía en spans, DataPower OK | Problema en la aplicación o backend |
| **Solo DataPower** | Errores en gateway, Dynatrace sin anomalía | Problema de conectividad o política |
| **Ambos sistemas** | Anomalías simultáneas en el mismo servicio | Problema de infraestructura compartida |
| **Ninguno** | Sin datos | Servicio no instrumentado o problema de monitoreo |

---

## Paso 3: Clasificación de Severidad

Usar los umbrales de `.github/toolboxes/common-metrics.md`:

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

## Paso 4: Generación del Reporte

Completar la **Plantilla 1: Reporte de Incidente** de `.github/toolboxes/report-templates.md` con:

- Datos recopilados en el Paso 1
- Resultado de la correlación del Paso 2
- Severidad clasificada en el Paso 3
- Acciones de remediación específicas al patrón identificado

---

## Paso 5: Acciones de Remediación por Patrón

### Backend caído (0x00d30003 + latencia alta en Dynatrace)

1. Verificar estado del servidor backend
2. Revisar logs del backend para causa raíz
3. Activar failover si está configurado
4. Escalar a equipo de infraestructura

### Problema de certificados (0x00d30006)

1. Verificar fecha de expiración del certificado afectado
2. Si expiró: renovar y redeployar en DataPower
3. Si no expiró: verificar chain de confianza y hostname
4. Escalar a equipo de seguridad/PKI

### Sobrecarga de tráfico (CPU alta + latencia gradual)

1. Verificar si hay pico de tráfico anormal
2. Activar escalamiento horizontal si está configurado
3. Implementar rate limiting temporal si es posible
4. Escalar a equipo de arquitectura

### Falla de política (0x80e00014)

1. Revisar logs de DataPower para identificar qué regla rechazó
2. Verificar si hubo cambio reciente en políticas
3. Si es falso positivo: ajustar regla con aprobación del equipo de seguridad
4. Escalar a equipo de DataPower

---

## Integración con Otros Agentes

- Datos de entrada vienen de `@dyn-analyst` y `@dp-analyst`
- El reporte generado puede enviarse al chatbot orquestador para distribución
- Si el incidente requiere cambios en Jira: pasar al agente `@atlassian-requirements-to-jira`
