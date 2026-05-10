# DataPower Skill

Skills que definen el análisis profesional de reportes DataPower. Este componente es completamente independiente de Dynatrace.

---

## Configuración de Conexión (Integración Futura)

<!-- TODO: reemplazar con valores reales al momento de integración -->

```
DP_HOST=datapower.empresa.com    # Host del DataPower Gateway
DP_PORT=5554                     # Puerto REST Management Interface
DP_DOMAIN=default                # Dominio de DataPower a monitorear
DP_USERNAME=admin                # Usuario con permisos de monitoreo
DP_PASSWORD=XXXXXXXXXX           # Contraseña
BASE_URL=https://{DP_HOST}:{DP_PORT}/mgmt/status/{DP_DOMAIN}
```

**Endpoints REST Management Interface:**

```
GET {BASE_URL}/HTTPTransactions     -- transacciones HTTP
GET {BASE_URL}/LogTargetSummary     -- estado de logs
GET {BASE_URL}/ServicesStatus       -- estado de servicios
GET {BASE_URL}/WSOperations         -- operaciones WebService
GET {BASE_URL}/MessageFlows         -- flujos de mensajes activos
```

---

## Estructura de un Reporte DataPower

Un reporte típico contiene los siguientes campos:

```
timestamp       : 2025-05-09T14:32:00Z
service         : PaymentGatewayService
domain          : production
gateway         : DPG-Node-01
response_time   : 1243 ms
status          : error
error_code      : 0x00d30003
error_msg       : Backend connection timeout
request_size    : 4096 bytes
response_size   : 512 bytes
client_ip       : 192.168.1.45
transaction_id  : TXN-20250509-98432
protocol        : HTTPS
method          : POST
```

---

## Métricas Clave a Analizar

| Métrica | Descripción | Umbral WARNING | Umbral CRITICAL |
|--------|-------------|----------------|-----------------|
| `response_time` | Tiempo de respuesta end-to-end | > 800ms | > 2000ms |
| `error_rate` | % de transacciones con error | > 1% | > 5% |
| `throughput` | Transacciones por segundo (TPS) | Caída > 20% | Caída > 50% |
| `queue_depth` | Mensajes en cola | > 100 | > 500 |
| `cpu_usage` | CPU del gateway | > 70% | > 90% |
| `memory_usage` | Memoria del gateway | > 75% | > 90% |

---

## Códigos de Error Comunes

| Código | Descripción | Causa probable |
|--------|-------------|---------------|
| `0x00d30003` | Backend connection timeout | Backend caído o lento |
| `0x00d30006` | SSL handshake failure | Certificado expirado o no válido |
| `0x00d3000b` | Service unavailable | Servicio de backend no disponible |
| `0x00d10011` | Parse error | Mensaje malformado o schema inválido |
| `0x80e00014` | Transaction rejected | Regla de política bloqueó la transacción |
| `0x00d30001` | Connection refused | Puerto cerrado o firewall |

---

## Patrones de Análisis

### Cuello de botella en backend

**Señales:**

- `response_time` elevado con `error_code = 0x00d30003`
- `queue_depth` creciendo sostenidamente
- Errores concentrados en un solo `service`

**Diagnóstico:** El backend no está respondiendo a tiempo. DataPower está acumulando solicitudes.

### Degradación por carga

**Señales:**

- `throughput` subió > 30% vs baseline
- `cpu_usage` > 85%
- `response_time` degradándose gradualmente (no abrupto)

**Diagnóstico:** El gateway está procesando más carga de la configurada. Revisar escalamiento.

### Problema de certificados

**Señales:**

- `error_code = 0x00d30006` en múltiples transacciones
- Errores en servicios específicos (los que usan ese certificado)
- Inicio abrupto de errores

**Diagnóstico:** Certificado SSL expirado o no coincide con el backend.

### Falla de política

**Señales:**

- `error_code = 0x80e00014`
- `status = rejected` (no `error`)
- Concentrado en un `client_ip` o `transaction_id` específico

**Diagnóstico:** Una regla de seguridad o transformación está bloqueando las transacciones.

---

## Plantilla de Insight Profesional

Para cada análisis, el agente DataPower produce esta estructura:

```markdown
## Análisis DataPower — {servicio} — {fecha}

**Severidad:** INFO | WARNING | CRITICAL

### Problema Identificado
{Descripción clara en 1-2 oraciones}

### Causa Raíz
{Causa técnica probable basada en los patrones del reporte}

### Impacto
- Servicios afectados: {lista}
- Transacciones impactadas: {estimado si disponible}
- Ventana de tiempo: {inicio} — {fin o "en curso"}

### Recomendación
1. {Acción inmediata}
2. {Acción a mediano plazo}
3. {Acción preventiva}

### Escalamiento
{Si CRITICAL: escalar a [equipo] via [canal]}
{Si WARNING: monitorear durante {tiempo}}
{Si INFO: registrar para tendencias}
```

---

## Ejemplo: Reporte de Entrada y Análisis de Salida

**Reporte de entrada:**

```
[2025-05-09 14:30] PaymentGatewayService | error | 0x00d30003 | 3420ms
[2025-05-09 14:31] PaymentGatewayService | error | 0x00d30003 | 3891ms
[2025-05-09 14:32] AuthService           | ok    | -          | 145ms
[2025-05-09 14:32] PaymentGatewayService | error | 0x00d30003 | 3654ms
```

**Análisis generado:**

```markdown
## Análisis DataPower — PaymentGatewayService — 2025-05-09

**Severidad:** CRITICAL

### Problema Identificado
PaymentGatewayService presenta fallas continuas con timeout de backend
desde las 14:30, con tiempos de respuesta superiores a 3400ms.

### Causa Raíz
Código 0x00d30003 indica timeout de conexión al backend. AuthService opera
normalmente, lo que descarta problemas de red general.

### Impacto
- Servicios afectados: PaymentGatewayService
- Todas las transacciones de pago están fallando
- Ventana de tiempo: 14:30 — en curso

### Recomendación
1. Verificar estado del servidor backend de pagos
2. Activar modo de failover si está configurado
3. Revisar logs del backend para causa raíz

### Escalamiento
CRITICAL: Escalar a equipo de Plataforma y equipo de Pagos inmediatamente.
```

---

## Integración Futura

Cuando se conecte la API real:

1. Reemplazar variables de configuración arriba
2. Validar acceso con: `GET {BASE_URL}/ServicesStatus`
3. Configurar polling de métricas cada 60 segundos
4. Integrar con `@incident-responder` para correlación automática con Dynatrace
