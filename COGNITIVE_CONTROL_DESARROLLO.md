# KAN-28: Desarrollo — Cognitive Control

**Feature:** KAN-47 (Cognitive Control)  
**Agente:** SD-Code  
**Dependencia:** KAN-27 (Especificación) 🔄 APROBACIÓN PENDIENTE  
**Fecha:** 2026-05-01  
**Estado:** ⏳ PENDIENTE - Esperando KAN-27

---

## Historia de Usuario

> **Como** desarrollador del ecosistema (SD-Code),  
> **quiero** implementar el dashboard de visualización en tiempo real según la especificación KAN-27,  
> **para** que Iván pueda monitorear todos sus agentes desde una interfaz unificada.

---

## Criterios de Aceptación

1. ✅ El grafo Hub & Spoke renderiza todos los agentes definidos en `agents.json`
2. ✅ Los estados (idle/active/completed/error) se visualizan correctamente
3. ✅ La bitácora muestra eventos cronológicos por agente
4. ✅ Las métricas de latencia y carga se actualizan cada 5 segundos
5. ✅ El diseño es responsive (desktop/mobile)
6. ✅ Los productos de trabajo están en el repositorio de código

---

## Subtareas de SD-Code

### Subtarea 1: Implementar modelo de datos y servicio de estado
**Agente:** SD-Code  
**Tiempo estimado:** 3h  
**Producto:** `src/services/agentState.js`

**Responsabilidades:**
- Leer `agents-state.json` cada 5 segundos
- Exponer API interna para componentes
- Manejar errores de lectura (fallback a último conocido)
- Calcular métricas derivadas (uptime, tasa de éxito)

```javascript
// Pseudo-código
class AgentStateService {
  async loadAgents() { /* fetch JSON */ }
  async updateAgentStatus(id, status) { /* write JSON */ }
  calculateMetrics() { /* latencia, carga, uptime */ }
  subscribe(callback) { /* watcher de archivo */ }
}
```

---

### Subtarea 2: Implementar componente CognitiveGraph
**Agente:** SD-Code  
**Tiempo estimado:** 4h  
**Producto:** `src/components/CognitiveGraph.jsx` (evolución del existente)

**Mejoras sobre legacy:**
- Carga dinámica desde `AgentStateService`
- Colores por estado (idle/active/completed/error)
- Animaciones de transición entre estados
- Zoom y pan habilitados
- FitView automático al cargar

**Props:**
```javascript
{
  agents: Agent[],        // Desde AgentStateService
  onNodeClick: function,  // Abre AgentCard
  onNodeDoubleClick: function, // Abre BitacoraPanel
  filterByOrchestrator: string|null // Filtra spokes
}
```

---

### Subtarea 3: Implementar AgentCard y BitacoraPanel
**Agente:** SD-Code  
**Tiempo estimado:** 3h  
**Producto:** `src/components/AgentCard.jsx` + `src/components/BitacoraPanel.jsx`

**AgentCard:**
- Avatar (emoji del agente)
- Nombre + tipo (hub/spoke/agent)
- Estado actual con indicador visual
- Canales activos (iconos)
- Carga actual (barra)
- Último heartbeat (timestamp relativo)
- Botón "Ver bitácora"

**BitacoraPanel:**
- Lista cronológica de eventos
- Filtro por tipo de evento
- Scroll infinito (si hay muchos)
- Exportar a Markdown

---

### Subtarea 4: Implementar métricas y alertas
**Agente:** SD-Code  
**Tiempo estimado:** 2.5h  
**Producto:** `src/components/MetricPanel.jsx` + `src/components/AlertBanner.jsx`

**Métricas:**
- Total de agentes / activos / caídos
- Latencia promedio del sistema
- Tasa de éxito (última hora)
- Entropía cognitiva (caos vs orden)

**Alertas:**
- Agente sin heartbeat > 15 min
- Tasa de error > 10%
- Latencia > 500ms

---

### Subtarea 5: Integrar con sistema de build y deploy
**Agente:** SD-Code  
**Tiempo estimado:** 1.5h  
**Producto:** GitHub Actions workflow

**Pipeline:**
```yaml
1. Checkout código
2. Instalar dependencias (npm ci)
3. Ejecutar tests (npm test)
4. Build (npm run build)
5. Deploy a GitHub Pages
```

---

## Productos de Trabajo

| # | Artefacto | Ubicación | Estado |
|---|-----------|-----------|--------|
| 1 | Servicio de estado | `cognitive-control/src/services/` | ⏳ |
| 2 | CognitiveGraph dinámico | `cognitive-control/src/components/` | ⏳ |
| 3 | AgentCard + BitacoraPanel | `cognitive-control/src/components/` | ⏳ |
| 4 | Métricas + Alertas | `cognitive-control/src/components/` | ⏳ |
| 5 | Pipeline CI/CD | `.github/workflows/deploy.yml` | ⏳ |

---

## Certificación

**¿Expectativa inicial = resultado?**

| Criterio | Estado |
|----------|--------|
| Grafo renderiza agentes | ⏳ Pendiente |
| Estados visuales correctos | ⏳ Pendiente |
| Bitácora funcional | ⏳ Pendiente |
| Métricas actualizadas | ⏳ Pendiente |
| Responsive design | ⏳ Pendiente |

**ESTADO: PENDIENTE** → Esperando aprobación de KAN-27

---

*Generado por SD-Code bajo supervisión de Magnum*  
*2026-05-01 | SmartDimension*
