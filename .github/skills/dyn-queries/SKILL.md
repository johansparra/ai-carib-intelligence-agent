# Dynatrace Skill

Skills que definen las capacidades del agente Dynatrace: conexión a la API, consultas DQL y detección de anomalías.

---

## Configuración de Conexión

<!-- TODO: reemplazar con valores reales al momento de integración -->

```http
ENVIRONMENT_ID=abc12345          # Formato: 8 caracteres alfanuméricos
API_TOKEN=dt0c01.XXXXXXXXXXXX    # Generar en: Settings > Access Tokens
BASE_URL=https://{ENVIRONMENT_ID}.live.dynatrace.com/api/v2
```

**Scopes requeridos para el token:**

- `metrics.read` — leer métricas de servicios y hosts
- `logs.read` — acceder a logs
- `problems.read` — leer problemas y anomalías detectadas por Davis AI
- `entities.read` — leer entidades monitoreadas (servicios, hosts, procesos)

**Dónde encontrar el Environment ID:**

En la URL de Dynatrace: `https://{ENVIRONMENT_ID}.live.dynatrace.com`

---

## API REST — Endpoints principales

```http
# Ejecutar query DQL
POST {BASE_URL}/metrics/query

# Obtener problemas activos
GET  {BASE_URL}/problems?problemSelector=status(open)

# Obtener logs
POST {BASE_URL}/logs/search

# Listar entidades monitoreadas
GET  {BASE_URL}/entities?entitySelector=type(SERVICE)
```

**Headers requeridos en cada request:**

```http
Authorization: Api-Token {API_TOKEN}
Content-Type: application/json
```

---

## Biblioteca de Queries DQL

### Errores HTTP 5xx por servicio (última hora)

```dql
fetch spans
| filter http.status_code >= 500
| summarize count = count(), by: {service.name}
| sort count desc
| limit 10
```

### Latencia P95 por endpoint

```dql
fetch spans
| summarize p95 = percentile(duration, 95), by: {service.name, http.url}
| sort p95 desc
| limit 20
```

### CPU promedio por host (últimas 2 horas)

```dql
fetch metrics
| metric builtin:host.cpu.usage
| summarize avg = avg(value), by: {host.name}
| sort avg desc
```

### Memoria utilizada por host

```dql
fetch metrics
| metric builtin:host.mem.usage
| summarize avg = avg(value), by: {host.name}
| sort avg desc
```

### Logs de errores en últimas N horas

```dql
fetch logs
| filter loglevel == "ERROR"
| filter timestamp > now() - 4h
| summarize count = count(), by: {service.name, content}
| sort count desc
| limit 50
```

### Servicios con mayor tasa de fallos

```dql
fetch spans
| summarize
    total = count(),
    errors = countIf(http.status_code >= 500),
    by: {service.name}
| fields service.name, error_rate = errors / total * 100
| filter error_rate > 1
| sort error_rate desc
```

### Anomalías activas (Davis AI)

```dql
fetch problems
| filter status == "OPEN"
| fields title, severity, impactLevel, startTime, affectedEntities
| sort startTime desc
```

### Tiempo de respuesta hoy vs ayer

```dql
fetch spans
| summarize
    hoy = avg(duration) { timestamp > now() - 24h },
    ayer = avg(duration) { timestamp > now() - 48h AND timestamp < now() - 24h },
    by: {service.name}
| fields service.name, hoy, ayer, delta = hoy - ayer
| sort delta desc
```

### Throughput (requests por minuto) por servicio

```dql
fetch spans
| filter timestamp > now() - 1h
| summarize rpm = count() / 60, by: {service.name}
| sort rpm desc
| limit 15
```

### Disponibilidad de monitores sintéticos

```dql
fetch metrics
| metric builtin:synthetic.http.availability
| summarize availability = avg(value), by: {monitor.name}
| filter availability < 99
| sort availability asc
```

---

## Cómo Construir Queries DQL

### Estructura básica

```dql
fetch {fuente}           -- spans | logs | metrics | problems | events
| filter {condición}     -- filtrar registros
| summarize {agregación} -- agrupar y agregar
| fields {columnas}      -- seleccionar campos
| sort {campo} {asc|desc}
| limit {n}
```

### Filtros temporales

```dql
| filter timestamp > now() - 1h    -- última hora
| filter timestamp > now() - 24h   -- últimas 24 horas
| filter timestamp > now() - 7d    -- última semana
```

### Agregaciones comunes

```dql
count()                      -- contar registros
avg(campo)                   -- promedio
sum(campo)                   -- suma
percentile(campo, 95)        -- percentil 95
countIf(condición)           -- contar condicionalmente
```

---

## Ejemplo de Respuesta JSON (API)

```json
{
  "resolution": "1m",
  "result": {
    "metricId": "builtin:service.response.time",
    "data": [
      {
        "dimensionMap": { "service.name": "payment-service" },
        "timestamps": [1700000000000, 1700000060000],
        "values": [245.3, 312.7]
      }
    ]
  }
}
```

El agente extrae `dimensionMap` para identificar el servicio y `values` para las métricas.

---

## Integración Futura

Cuando se conecte la API real:

1. Reemplazar `ENVIRONMENT_ID` y `API_TOKEN` en la configuración
2. Validar scopes con: `GET {BASE_URL}/entities?entitySelector=type(SERVICE)&pageSize=5`
3. Si el entorno es **Dynatrace Managed** (on-premise), el BASE_URL es: `https://{servidor}/e/{ENVIRONMENT_ID}/api/v2`
