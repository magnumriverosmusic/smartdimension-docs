# KAN-50: HU-CC-Especificar

**Épica Padre:** KAN-20 (SmartDimension-Orq)  
**Agente:** SD-Spec  
**Dependencia:** KAN-49 (HU-CC-Investigar) ✅ Done  
**Fecha:** 2026-05-01  
**Estado:** 🔄 **IN PROGRESS**

---

## Historia de Usuario

> **Como** SD-Spec,  
> **quiero** una especificación de diseño detallada (SDD) que defina cómo se visualizarán los agentes en tiempo real,  
> **para** que el equipo de desarrollo (SD-Code) pueda implementar el dashboard sin ambigüedades.

---

## Criterios de Aceptación

| # | Criterio | Estado |
|---|----------|--------|
| 1 | El modelo de datos del agente está completamente definido | ✅ |
| 2 | Los estados posibles y sus transiciones están documentados | ✅ |
| 3 | El protocolo de actualización está especificado | ✅ |
| 4 | El diseño UI/UX tiene especificación de componentes | ✅ |
| 5 | Las dependencias técnicas están identificadas | ✅ |
| 6 | Los productos de trabajo están en el repositorio documental | ✅ |

---

## Subtareas de SD-Spec

### ✅ KAN-59: Definir modelo de datos del agente
**Estado:** Finalizada  
**Tiempo estimado:** 2h  
**Producto de trabajo:** Esquema JSON del agente

```json
{
  "id": "string",
  "name": "string",
  "type": "hub|spoke|agent",
  "status": "idle|active|completed|error",
  "orchestrator": "string|null",
  "emoji": "string",
  "colors": {
    "primary": "hex",
    "secondary": "hex"
  },
  "personality": {
    "tone": "string",
    "approach": "string"
  },
  "tasks": ["string"],
  "channels": {
    "webchat": "string|null",
    "telegram": "string|null",
    "email": "string|null"
  },
  "scheduled_activities": {
    "hora": "actividad"
  },
  "ethics": ["string"],
  "jira_epic": "string",
  "cognitive_state": {
    "status": "idle|active|completed|error",
    "last_heartbeat": "ISO8601",
    "current_task": "string",
    "load": "0.0-1.0",
    "channels_active": ["string"],
    "alerts": ["string"]
  },
  "bitacora": [
    {
      "timestamp": "ISO8601",
      "evento": "string",
      "detalle": "string"
    }
  ]
}
```

**Interrelación:** Este modelo se usa en KAN-64 (Servicio de estado) y KAN-65 (CognitiveGraph)

---

### ✅ KAN-60: Definir flujo de estados
**Estado:** Finalizada  
**Tiempo estimado:** 1.5h  
**Producto de trabajo:** Diagrama de estados + semántica visual

**Estados:**
- `idle` → Agente disponible, sin tarea activa
- `active` → Agente ejecutando tarea
- `completed` → Tarea terminada exitosamente
- `error` → Fallo detectado, requiere atención

**Transiciones:**
```
idle → active: Cuando se asigna tarea
active → completed: Cuando tarea termina OK
active → error: Cuando falla o timeout
completed → idle: Después de 5 min sin nueva tarea
error → idle: Después de intervención manual
```

**Semántica visual:**
| Estado | Color | Animación |
|--------|-------|-----------|
| idle | Gris #666 | Estático |
| active | Ámbar #F59E0B | Pulso suave |
| completed | Verde #10B981 | Brillo momentáneo |
| error | Rojo #EF4444 | Pulso rápido + alerta |

**Interrelación:** Usado en KAN-65 (CognitiveGraph) y KAN-67 (Métricas + Alertas)

---

### ✅ KAN-61: Definir protocolo de actualización
**Estado:** Finalizada  
**Tiempo estimado:** 2h  
**Producto de trabajo:** Especificación del protocolo por fases

**Fase 1 (JSON File):**
- Archivo: `cognitive-control/public/agents-state.json`
- Formato: Array de objetos agente
- Actualización: Cada agente escribe su estado cada 30s
- Consumo: Dashboard lee vía fetch cada 5s

**Fase 2 (API REST):**
- Endpoint: `GET /api/agents` → Lista de agentes
- Endpoint: `POST /api/agents/:id/heartbeat` → Actualizar estado
- Auth: Token por agente
- Rate limit: 1 req/30s por agente

**Fase 3 (WebSocket):**
- Conexión persistente por agente
- Push de actualizaciones en tiempo real
- Reconexión automática con backoff

**Interrelación:** Implementado en KAN-64 (Servicio de estado)

---

### ✅ KAN-62: Diseñar UI/UX de bitácora
**Estado:** Finalizada  
**Tiempo estimado:** 2.5h  
**Producto de trabajo:** Especificación de componentes

**Panel Principal:**
```
┌─────────────────────────────────────┐
│  COGNITIVE CONTROL v1.0             │
├─────────────────────────────────────┤
│                                     │
│  ┌─────┐    ┌─────┐    ┌─────┐   │
│  │HUB  │◄──►│HUB  │◄──►│SPOKE│   │
│  │MAG  │    │SMART│    │ABRAZ│   │
│  └──┬──┘    └──┬──┘    └──┬──┘   │
│     │          │          │      │
│  ┌──┴──┐    ┌─┴──┐    ┌──┴──┐   │
│  │AGENT│    │AGEN│    │AGENT│   │
│  │DANA │    │SPEC│    │CODE │   │
│  └─────┘    └────┘    └─────┘   │
│                                     │
├─────────────────────────────────────┤
│  BITÁCORA: Dana - 3 eventos        │
│  [07:15] Tip de independencia       │
│  [08:15] Validación mañana          │
│  [15:30] Validación tarde           │
└─────────────────────────────────────┘
```

**Componentes:**
- `CognitiveGraph`: Grafo Hub & Spoke (ReactFlow)
- `AgentCard`: Tarjeta de agente con estado + métricas
- `BitacoraPanel`: Lista cronológica de eventos
- `MetricPanel`: Gráficas de latencia, carga, éxito
- `AlertBanner`: Notificaciones de errores

**Interacciones:**
- Click en nodo → Abre `AgentCard` con detalles
- Doble click → Abre `BitacoraPanel` completo
- Hover → Tooltip con resumen
- Filtro por orquestador → Colapsa/expande spokes

**Interrelación:** Implementado en KAN-65, KAN-66, KAN-67

---

### ✅ KAN-63: Identificar dependencias técnicas
**Estado:** Finalizada  
**Tiempo estimado:** 1h  
**Producto de trabajo:** Lista de dependencias + versiones

| Dependencia | Versión | Propósito | Alternativa |
|-------------|---------|-----------|-------------|
| ReactFlow | ^11.11.4 | Grafo interactivo | D3.js, Cytoscape |
| Recharts | ^3.8.1 | Gráficas de métricas | Chart.js, Victory |
| Framer Motion | ^12.38.0 | Animaciones | GSAP |
| Astro | ^6.1.9 | Framework | Next.js |
| TailwindCSS | ^4.2.4 | Estilos | Styled Components |

**Infraestructura:**
- Servidor estático: Python http.server (dev)
- Producción: Vercel / GitHub Pages
- Datos: JSON file (Fase 1), luego API

**Interrelación:** Usado en KAN-64-KAN-68 (todo el desarrollo)

---

## 📦 Productos de Trabajo Generados

| # | Producto | Generado por | Ubicación | Adjunto Jira |
|---|----------|-------------|-----------|--------------|
| 1 | Modelo de datos JSON | KAN-59 | Este documento | Pendiente |
| 2 | Diagrama de estados | KAN-60 | Este documento | Pendiente |
| 3 | Protocolo de actualización | KAN-61 | Este documento | Pendiente |
| 4 | Especificación UI/UX | KAN-62 | Este documento | Pendiente |
| 5 | Lista de dependencias | KAN-63 | Este documento | Pendiente |

---

## 🔗 Mapa de Interrelaciones

```
KAN-59 (Modelo de datos)
    │
    ├──► Producto 1: JSON Schema
    │       │
    │       ├──► Usado en KAN-64 (Servicio de estado)
    │       └──► Usado en KAN-65 (CognitiveGraph)
    │
KAN-60 (Flujo de estados)
    │
    ├──► Producto 2: Diagrama + semántica visual
    │       │
    │       ├──► Usado en KAN-65 (Colores/animaciones)
    │       └──► Usado en KAN-67 (Alertas de error)
    │
KAN-61 (Protocolo)
    │
    ├──► Producto 3: Fase 1/2/3
    │       │
    │       └──► Implementado en KAN-64 (Servicio de estado)
    │
KAN-62 (UI/UX)
    │
    ├──► Producto 4: Wireframes + componentes
    │       │
    │       ├──► Usado en KAN-65 (CognitiveGraph)
    │       ├──► Usado en KAN-66 (AgentCard)
    │       └──► Usado en KAN-67 (Métricas)
    │
KAN-63 (Dependencias)
    │
    └──► Producto 5: Lista técnica
            │
            └──► Referencia para KAN-64-KAN-68
```

---

## Certificación

**¿Expectativa inicial = resultado?**

| Criterio | Estado |
|----------|--------|
| Modelo de datos definido | ✅ |
| Flujo de estados documentado | ✅ |
| Protocolo especificado | ✅ |
| UI/UX diseñada | ✅ |
| Dependencias identificadas | ✅ |

**ESTADO: EN REVIEW** → Esperando aprobación para avanzar a KAN-51

---

*Generado por SD-Spec bajo supervisión de Magnum*  
*2026-05-01 | SmartDimension*
