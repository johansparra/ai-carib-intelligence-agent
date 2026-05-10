# Structure Monitor Agent

Agente responsable de detectar cambios en la estructura del proyecto y sincronizar automáticamente todos los README.md relevantes.

## 🎯 Responsabilidades

- Detectar cambios en carpetas y archivos
- Actualizar diagramas Mermaid en README.md
- Mantener sincronización entre documentación y código
- Ejecutarse automáticamente después de cada cambio

## ⚙️ Modo de Operación

Este agente se activa automáticamente cuando detecta:

- Nuevas carpetas creadas
- Nuevos archivos agregados
- Cambios en la estructura de directorios
- Modificaciones en la lógica de agentes o skills
- Cambios en descripción de componentes

## 📤 Salida

Actualiza automáticamente:

- `.github/README.md` (estructura general)
- `README.md` (raíz del proyecto)
- README.md de cada carpeta afectada (agents, skills, prompts, etc.)

## 🔄 Flujo de Sincronización

```mermaid
graph LR
    A["Cambio detectado"] --> B["Analizar tipo de cambio"]
    B --> C["Identificar README.md afectados"]
    C --> D["Regenerar diagramas Mermaid"]
    D --> E["Actualizar referencias"]
    E --> F["Validar consistencia"]
    F --> G["Completado ✓"]
    
    style A fill:#2ca02c
    style G fill:#2ca02c
```

## 📝 Configuración

Ver: [.github/customizations/auto-sync.md](../customizations/auto-sync.md)

## 💡 Ejemplos

### Cuando creas una nueva carpeta

```text
Nueva carpeta: .github/agents/ml-predictor/
↓
Structure Monitor detecta esto
↓
Actualiza diagramas en README.md
↓
Actualiza índice de agentes
↓
Crea README.md en la nueva carpeta
```

### Cuando modificas un agente

```text
Cambio en: .github/agents/dyn-analyst/README.md
↓
Structure Monitor detecta esto
↓
Actualiza relaciones en diagrama principal
↓
Sincroniza en README.md de raíz
```
