---
name: ops-metrics-thresholds
description: >
  Definiciones canónicas de SLO/SLA, umbrales de alerta por métrica
  (latencia, error rate, CPU, memoria, throughput, queue depth),
  glosario Dynatrace/DataPower y códigos de error DataPower.
---

# Common Metrics — Skill

Referencia compartida por todos los agentes para clasificar severidad, interpretar métricas y entender la terminología de los dominios Dynatrace y DataPower.

---

## SLO / SLA del Proyecto

<!-- TODO: ajustar con los valores reales acordados con el negocio -->

| Métrica | SLO (objetivo interno) | SLA (compromiso con cliente) |
| ------- | ---------------------- | ---------------------------- |
| Disponibilidad | 99.95% mensual | 99.9% mensual |
| Latencia P95 | < 500ms | < 1000ms |
| Latencia P99 | < 1000ms | < 2000ms |
| Error rate | < 0.1% | < 1% |
| Tiempo de recuperación (MTTR) | < 30 minutos | < 2 horas |

---

## Umbrales de Alerta

### Latencia (response time)

| Nivel | Umbral | Acción |
| ----- | ------ | ------ |
| OK | < 500ms | Ninguna |
| WARNING | 500ms – 2000ms | Monitorear, investigar si persiste |
| CRITICAL | > 2000ms | Escalar inmediatamente |

### Tasa de Error (error rate)

| Nivel | Umbral | Acción |
| ----- | ------ | ------ |
| OK | < 1% | Ninguna |
| WARNING | 1% – 5% | Investigar causa |
| CRITICAL | > 5% | Escalar inmediatamente |

### CPU (hosts y gateways)

| Nivel | Umbral | Acción |
| ----- | ------ | ------ |
| OK | < 70% | Ninguna |
| WARNING | 70% – 85% | Revisar procesos, planificar capacidad |
| CRITICAL | > 85% | Escalar, evaluar escalamiento horizontal |

### Memoria

| Nivel | Umbral | Acción |
| ----- | ------ | ------ |
| OK | < 75% | Ninguna |
| WARNING | 75% – 90% | Investigar fugas de memoria |
| CRITICAL | > 90% | Escalar, reinicio planificado si necesario |

### Throughput (caída respecto a baseline)

| Nivel | Caída | Acción |
| ----- | ----- | ------ |
| OK | < 10% | Ninguna |
| WARNING | 10% – 30% | Verificar si es degradación o reducción de demanda |
| CRITICAL | > 30% | Escalar, verificar disponibilidad del servicio |

### Queue Depth (DataPower)

| Nivel | Umbral | Acción |
| ----- | ------ | ------ |
| OK | < 50 mensajes | Ninguna |
| WARNING | 50 – 200 mensajes | Monitorear, revisar backend |
| CRITICAL | > 200 mensajes | Escalar, riesgo de pérdida de mensajes |

---

## Glosario

### Términos de Dynatrace

| Término | Definición |
| ------- | ---------- |
| **DQL** | Dynatrace Query Language — lenguaje para consultar métricas, logs y trazas |
| **Davis AI** | Motor de IA de Dynatrace que detecta anomalías automáticamente |
| **Span** | Unidad de trabajo en una traza distribuida (operación en un servicio) |
| **Baseline** | Comportamiento histórico normal de una métrica, calculado por Davis AI |
| **Problem** | Incidente detectado por Davis AI que agrupa varias anomalías relacionadas |
| **Entity** | Componente monitoreado: servicio, host, proceso, base de datos |
| **SLO** | Service Level Objective — objetivo interno de desempeño |
| **Synthetic Monitor** | Test automatizado que simula usuarios para verificar disponibilidad |
| **Environment ID** | Identificador único del tenant de Dynatrace (`abc12345` en la URL) |

### Términos de DataPower

| Término | Definición |
| ------- | ---------- |
| **Gateway** | Servidor IBM DataPower que procesa y enruta mensajes entre sistemas |
| **Domain** | Partición lógica dentro de DataPower que agrupa servicios relacionados |
| **Service** | Punto de entrada en DataPower que expone una funcionalidad específica |
| **Policy** | Conjunto de reglas que DataPower aplica a cada mensaje (seguridad, transformación, enrutamiento) |
| **Queue depth** | Número de mensajes pendientes de procesar en un servicio |
| **TPS** | Transactions Per Second — throughput del gateway |
| **WS-Security** | Estándar de seguridad para servicios web usado por DataPower |
| **XML Firewall** | Servicio DataPower que valida y filtra mensajes XML/SOAP |
| **Multi-Protocol Gateway** | Servicio DataPower que soporta múltiples protocolos (HTTP, HTTPS, MQ, etc.) |

### Términos Generales

| Término | Definición |
| ------- | ---------- |
| **MTTR** | Mean Time To Recovery — tiempo promedio para restaurar un servicio |
| **MTBF** | Mean Time Between Failures — tiempo promedio entre fallas |
| **P95 / P99** | Percentil 95/99 — el 95%/99% de las requests son más rápidas que este valor |
| **Error budget** | Margen de errores permitidos antes de violar el SLA |
| **Anomaly** | Desviación estadística significativa del comportamiento baseline |
| **Correlation** | Relación causal entre eventos en diferentes sistemas (Dynatrace + DataPower) |
| **Service mesh** | Infraestructura que gestiona comunicación entre microservicios |

---

## Códigos de Error DataPower — Referencia Rápida

| Código | Categoría | Descripción |
| ------ | --------- | ----------- |
| `0x00d30001` | Conectividad | Connection refused |
| `0x00d30003` | Conectividad | Backend connection timeout |
| `0x00d30006` | SSL/TLS | SSL handshake failure |
| `0x00d3000b` | Disponibilidad | Service unavailable |
| `0x00d10011` | Formato | Parse error (mensaje malformado) |
| `0x80e00014` | Política | Transaction rejected by policy |
| `0x00d50001` | Autenticación | Authentication failed |
| `0x00d50003` | Autorización | Authorization denied |

---

## Frecuencias de Monitoreo Recomendadas

| Tipo de métrica | Frecuencia | Herramienta |
| --------------- | ---------- | ----------- |
| Disponibilidad de servicios | Cada 60 segundos | Dynatrace Synthetic |
| CPU / Memoria de hosts | Cada 60 segundos | Dynatrace |
| Error rate y latencia | Tiempo real (streaming) | Dynatrace Davis AI |
| Estado de gateways DataPower | Cada 60 segundos | DataPower REST API |
| Resumen diario de tendencias | 1 vez/día (08:00) | `@ops-daily-summary` |
| Reporte de incidente | Bajo demanda | `@ops-incident-responder` |

---

## Referencias

| Recurso | Propósito |
| ------- | --------- |
| [`ops-incident-responder.agent.md`](../../agents/ops/ops-incident-responder.agent.md) | Consume estos umbrales para clasificar severidad |
| [`ops-daily-summary.agent.md`](../../agents/ops/ops-daily-summary.agent.md) | Consume estos umbrales para clasificar estado general |
| [`dp-analysis/SKILL.md`](../dp-analysis/SKILL.md) | Detalle ampliado de códigos de error y patrones DataPower |
| [`dyn-queries/SKILL.md`](../dyn-queries/SKILL.md) | Queries DQL para obtener las métricas listadas |
