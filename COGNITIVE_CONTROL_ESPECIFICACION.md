# KAN-50: HU-CC-Especificar

**Épica Padre:** KAN-20 (SmartDimension-Orq)  
**Agente:** SD-Spec  
**Dependencia:** KAN-26 (HU-CC-Investigar) ✅ APROBADA  
**Fecha:** 2026-05-01  
**Estado:** 🔄 EN ANÁLISIS

---

## Historia de Usuario

> **Como** Iván (arquitecto de sistemas),  
> **quiero** una especificación de diseño detallada (SDD) que defina cómo se visualizarán los agentes en tiempo real,  
> **para** que el equipo de desarrollo (SD-Code) pueda implementar el dashboard sin ambigüedades.

---

## Criterios de Aceptación

1. ✅ El modelo de datos del agente está completamente definido
2. ✅ Los estados posibles y sus transiciones están documentados
3. ✅ El protocolo de actualización está especificado
4. ✅ El diseño UI/UX tiene mockups o wireframes
5. ✅ Las dependencias técnicas están identificadas
6. ✅ Los productos de trabajo están en el repositorio documental

---

## Subtareas de SD-Spec

### Subtarea 1: Definir modelo de datos del agente
**Agente:** SD-Spec  
**Tiempo estimado:** 2h  
**Producto:** Esquema JSON del agente

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

---

### Subtarea 2: Definir flujo de estados
**Agente:** SD-Spec  
**Tiempo estimado:** 1.5h  
**Producto:** Diagrama de estados + semántica

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

---

### Subtarea 3: Definir protocolo de actualización
**Agente:** SD-Spec  
**Tiempo estimado:** 2h  
**Producto:** Especificación del protocolo

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

---

### Subtarea 4: Diseñar UI/UX de bitácora
**Agente:** SD-Spec  
**Tiempo estimado:** 2.5h  
**Producto:** Wireframes + especificación de componentes

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
│  [08:15] Validación mañana         │
│  [15:30] Validación tarde          │
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

---

### Subtarea 5: Identificar dependencias técnicas
**Agente:** SD-Spec  
**Tiempo estimado:** 1h  
**Producto:** Lista de dependencias + versiones

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

---

## Productos de Trabajo Pendientes

| # | Artefacto | Ubicación | Estado |
|---|-----------|-----------|--------|
| 1 | Modelo de datos JSON | `docs/COGNITIVE_CONTROL_ESPECIFICACION.md` | 🔄 |
| 2 | Diagrama de estados | `docs/COGNITIVE_CONTROL_ESPECIFICACION.md` | 🔄 |
| 3 | Especificación de protocolo | `docs/COGNITIVE_CONTROL_ESPECIFICACION.md` | 🔄 |
| 4 | Wireframes UI/UX | `docs/COGNITIVE_CONTROL_ESPECIFICACION.md` | 🔄 |
| 5 | Lista de dependencias | `docs/COGNITIVE_CONTROL_ESPECIFICACION.md` | 🔄 |

---

## Certificación

**¿Expectativa inicial = resultado?**

| Criterio | Estado |
|----------|--------|
| Modelo de datos definido | 🔄 Pendiente |
| Flujo de estados documentado | 🔄 Pendiente |
| Protocolo especificado | 🔄 Pendiente |
| UI/UX diseñada | 🔄 Pendiente |
| Dependencias identificadas | 🔄 Pendiente |

**ESTADO: EN ANÁLISIS** → Esperando aprobación para avanzar a KAN-28

---

*Generado por SD-Spec bajo supervisión de Magnum*  
*2026-05-01 | SmartDimension*
