# KAN-29: Pruebas — Cognitive Control

**Feature:** KAN-47 (Cognitive Control)  
**Agente:** SD-Test  
**Dependencia:** KAN-28 (Desarrollo) ⏳ PENDIENTE  
**Fecha:** 2026-05-01  
**Estado:** ⏳ PENDIENTE - Esperando KAN-28

---

## Historia de Usuario

> **Como** responsable de calidad (SD-Test),  
> **quiero** validar que el dashboard funciona correctamente bajo diferentes condiciones,  
> **para** garantizar que Iván recibe información confiable y actualizada.

---

## Criterios de Aceptación

1. ✅ El grafo se renderiza con 6+ agentes sin degradación de performance
2. ✅ Las transiciones de estado son visibles en < 2 segundos
3. ✅ La bitácora muestra eventos en orden cronológico correcto
4. ✅ Las métricas son consistentes con datos reales
5. ✅ El sistema se recupera gracefully ante errores de red/archivo
6. ✅ Los productos de trabajo están documentados

---

## Subtareas de SD-Test

### Subtarea 1: Pruebas de renderizado del grafo
**Agente:** SD-Test  
**Tiempo estimado:** 2h  
**Producto:** Reporte de pruebas de UI

**Escenarios:**
- Renderizar 1 hub + 2 spokes + 3 agents = 6 nodos
- Renderizar 1 hub + 4 spokes + 12 agents = 17 nodos
- Cambio de estado en tiempo real
- Zoom, pan, fitView

**Herramienta:** React Testing Library + Jest

---

### Subtarea 2: Pruebas de actualización de estado
**Agente:** SD-Test  
**Tiempo estimado:** 2h  
**Producto:** Reporte de pruebas de estado

**Escenarios:**
- Estado idle → active (transición visual < 2s)
- Estado active → completed (confirmación + métricas)
- Estado active → error (alerta visible)
- Agentes desaparecen del archivo (manejo de error)
- Múltiples actualizaciones simultáneas

**Herramienta:** Cypress o Playwright (e2e)

---

### Subtarea 3: Pruebas de carga y performance
**Agente:** SD-Test  
**Tiempo estimado:** 1.5h  
**Producto:** Reporte de performance

**Escenarios:**
- Dashboard abierto por 30 min (memory leak?)
- 100 actualizaciones de estado en 1 minuto
- Latencia de renderizado < 100ms por frame
- Uso de CPU/GPU razonable

**Herramienta:** Chrome DevTools Performance Tab

---

### Subtarea 4: Simulación de escenarios reales
**Agente:** SD-Test  
**Tiempo estimado:** 2h  
**Producto:** Suite de simulaciones

**Escenarios:**
- "Mañana típica": Magnum, Dana, SmartDimension activos
- "Crisis": Dana reporta error, Magnum interviene
- "Mantenimiento": Todos en idle, solo heartbeats
- "Carga máxima": Todos los agentes trabajando

**Herramienta:** Scripts de simulación en Node.js

---

## Productos de Trabajo

| # | Artefacto | Ubicación | Estado |
|---|-----------|-----------|--------|
| 1 | Reporte UI | `docs/tests/ui-report.md` | ⏳ |
| 2 | Reporte estado | `docs/tests/state-report.md` | ⏳ |
| 3 | Reporte performance | `docs/tests/perf-report.md` | ⏳ |
| 4 | Suite de simulaciones | `cognitive-control/tests/simulations/` | ⏳ |

---

## Certificación

**¿Expectativa inicial = resultado?**

| Criterio | Estado |
|----------|--------|
| 6+ agentes renderizados | ⏳ Pendiente |
| Transiciones < 2s | ⏳ Pendiente |
| Bitácora cronológica | ⏳ Pendiente |
| Métricas consistentes | ⏳ Pendiente |
| Recuperación graceful | ⏳ Pendiente |

**ESTADO: PENDIENTE** → Esperando KAN-28

---

*Generado por SD-Test bajo supervisión de Magnum*  
*2026-05-01 | SmartDimension*
