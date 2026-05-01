# KAN-49: HU-CC-Investigar

**Épica Padre:** KAN-20 (SmartDimension-Orq)  
**Agente:** SD-Investiga  
**Fecha:** 2026-05-01  
**Estado:** 🔄 **IN REVIEW** (Esperando aprobación de Iván para pasar a Done)

---

## Historia de Usuario

> **Como** SD-Investiga,  
> **quiero** analizar sistemas de visualización existentes (Grafana, ReactFlow, LangSmith, n8n) para entender qué es técnicamente factible, qué riesgos existen y qué decisiones de arquitectura tomar antes de especificar el dashboard de Cognitive Control,  
> **para** entregar una investigación fundamentada que permita a SD-Spec tomar decisiones informadas en la especificación.

---

## Criterios de Aceptación

| # | Criterio | Estado |
|---|----------|--------|
| 1 | Se identificaron al menos 3 sistemas de referencia con análisis de fortalezas/debilidades | ✅ |
| 2 | Se documentó la coherencia pragmática (qué es factible hoy vs. futuro) | ✅ |
| 3 | Se identificaron riesgos de confiabilidad con mitigaciones | ✅ |
| 4 | Se generó recomendación clara para la siguiente fase (KAN-50) | ✅ |
| 5 | Los productos de trabajo están en el repositorio documental | ✅ |

---

## Subtareas Completadas

### ✅ KAN-55: Analizar sistemas de visualización existentes
**Estado:** Finalizada  
**Tiempo real:** 2h  
**Producto de trabajo:** Tabla comparativa de sistemas de referencia

| Sistema | Fortaleza | Debilidad | Relevancia |
|---------|-----------|-----------|------------|
| Grafana | Dashboards flexibles | No grafo de agentes | UI de métricas |
| ReactFlow | Grafos interactivos | No persistencia | Motor visual |
| LangSmith | Tracing de LLMs | Cerrado, costoso | Patrón de bitácora |
| n8n | Workflow visual | No agentes autónomos | Inspiración UI |

---

### ✅ KAN-56: Evaluar coherencia pragmática
**Estado:** Finalizada  
**Tiempo real:** 2h  
**Producto de trabajo:** Tabla de factibilidad técnica + Decisiones de arquitectura

| Capacidad | Feasible | Esfuerzo | Nota |
|-----------|----------|----------|------|
| Grafo estático con datos de archivo JSON | ✅ Sí | 2h | Ya existe base |
| Actualización cada 30s desde archivo | ✅ Sí | 4h | File watcher |
| Conexión API Jira para estado real | ⚠️ Parcial | 8h | Necesita auth |
| Bitácora con historial completo | ⚠️ Parcial | 12h | Necesita DB |
| Telemetría en tiempo real (WebSocket) | ❌ No | 24h+ | Infraestructura |

**Decisión de arquitectura:**
- **Fase 1 (Ahora):** Archivo JSON como "fuente de verdad temporal"
- **Fase 2 (Próxima semana):** API REST simple para actualizar estado
- **Fase 3 (Futuro):** WebSocket + base de datos persistente

---

### ✅ KAN-57: Identificar riesgos de confiabilidad
**Estado:** Finalizada  
**Tiempo real:** 1.5h  
**Producto de trabajo:** Matriz de riesgos con mitigaciones

| Riesgo | Probabilidad | Impacto | Mitigación |
|--------|-------------|---------|------------|
| Datos desactualizados | Alta | Medio | Timestamp + indicador de freshness |
| Agente caído sin detectar | Media | Alto | Heartbeat cada 5 min |
| Confusión entre estados | Media | Medio | Semántica clara: idle/active/completed/error |
| Sobrecarga visual | Baja | Medio | Filtrado por orquestador |

---

### ✅ KAN-58: Generar recomendaciones para especificación
**Estado:** Finalizada  
**Tiempo real:** 1h  
**Producto de trabajo:** Lista de recomendaciones para KAN-50

1. **Modelo de datos:** Definir cómo se representa un agente en el sistema
2. **Flujo de estado:** Definir transiciones idle → active → completed
3. **Protocolo de actualización:** Definir quién y cuándo actualiza el estado
4. **UI/UX:** Definir cómo se ve la bitácora y qué información se muestra

---

## 📦 Productos de Trabajo Generados (Consolidado)

### Producto 1: Investigación Completa
**Generado por:** KAN-55 + KAN-56 + KAN-57 + KAN-58  
**Ubicación:** `docs/COGNITIVE_CONTROL_INVESTIGACION.md`  
**Adjunto en Jira:** KAN-49 (ID: 10000, 7975 bytes) ✅  
**Interrelación:** Este documento consolida todos los hallazgos y sirve como entrada para KAN-50 (Especificación)

### Producto 2: Datos de Agentes con Estado Cognitivo
**Generado por:** KAN-57 (Identificación de riesgos)  
**Ubicación:** `swarm-ctrl/agents/*.json`  
**Adjunto en Jira:** KAN-49 (ID: 10001, 1955 bytes) ✅  
**Interrelación:** Define el modelo de datos que se usará en KAN-50 (Modelo JSON) y KAN-51 (Servicio de estado)

### Producto 3: Estructura de Navegación del Docsify
**Generado por:** KAN-58 (Recomendaciones)  
**Ubicación:** `docs/_sidebar.md`  
**Interrelación:** Organiza toda la documentación del SDD, visible en el dashboard de documentación

### Producto 4: Repositorio Documental SmartDimension Docs
**Generado por:** KAN-58 (Recomendaciones)  
**Ubicación:** `github.com/magnumriverosmusic/smartdimension-docs`  
**Interrelación:** Aloja toda la documentación del proyecto, incluyendo futuros artefactos de KAN-50-54

### Producto 5: Estructura Jira Completa
**Generado por:** KAN-58 (Recomendaciones)  
**Ubicación:** Jira (KAN-49 a KAN-81)  
**Interrelación:** Define el flujo de trabajo del SDD que se seguirá en todas las historias futuras

---

## 🔗 Mapa de Interrelaciones

```
KAN-55 (Análisis de sistemas)
    │
    ├──► Producto 1: Tabla comparativa
    │       │
    │       └──► Usado en KAN-59 (Definir modelo de datos) → KAN-50
    │
KAN-56 (Coherencia pragmática)
    │
    ├──► Producto 1: Tabla de factibilidad
    │       │
    │       └──► Usado en KAN-61 (Protocolo de actualización) → KAN-50
    │
    ├──► Producto 4: Decisiones de arquitectura (Fase 1/2/3)
    │       │
    │       └──► Usado en KAN-60 (Flujo de estados) → KAN-50
    │
KAN-57 (Riesgos)
    │
    ├──► Producto 2: Datos de agentes con estado cognitivo
    │       │
    │       ├──► Usado en KAN-59 (Modelo de datos) → KAN-50
    │       └──► Usado en KAN-64 (Servicio de estado) → KAN-51
    │
KAN-58 (Recomendaciones)
    │
    ├──► Producto 3: Sidebar del docsify
    │       │
    │       └──► Usado en KAN-73 (README técnico) → KAN-53
    │
    ├──► Producto 4: Repositorio docsify
    │       │
    │       └──► Usado en KAN-74 (Manual operación) → KAN-53
    │
    └──► Producto 5: Estructura Jira
            │
            └──► Referencia para todas las historias futuras
```

---

## 📋 Checklist de Productos de Trabajo

| Producto | Subtarea Origen | Ubicación | Estado | Usado en |
|----------|-----------------|-----------|--------|----------|
| Investigación completa | KAN-55,56,57,58 | `docs/COGNITIVE_CONTROL_INVESTIGACION.md` | ✅ Entregado | KAN-50 |
| Datos de agentes | KAN-57 | `swarm-ctrl/agents/*.json` | ✅ Actualizado | KAN-50, KAN-51 |
| Sidebar docsify | KAN-58 | `docs/_sidebar.md` | ✅ Actualizado | KAN-53 |
| Repo docsify | KAN-58 | `smartdimension-docs` | ✅ Creado | KAN-53 |
| Estructura Jira | KAN-58 | Jira (KAN-49-81) | ✅ Creado | Todo el SDD |

---

## 🔄 Estado de la Historia

**Estado actual:** In Review  
**Próximo estado:** Done (requiere aprobación de Iván)  
**Bloqueo:** Ninguno  
**Dependencias:** Ninguna (es la primera historia del SDD)

**¿Aprobamos KAN-49 para avanzar a KAN-50?**

---

## 📎 Enlaces Relacionados

- **Siguiente historia:** [KAN-50: HU-CC-Especificar](COGNITIVE_CONTROL_ESPECIFICACION.md)
- **Épica padre:** [KAN-20: SmartDimension-Orq](COGNITIVE_CONTROL_EPIKA.md)
- **Feature:** KAN-47 (Cognitive Control)
- **Productos de trabajo:**
  - [Investigación completa](COGNITIVE_CONTROL_INVESTIGACION.md)
  - [Datos de agentes](https://github.com/magnumriverosmusic/swarm-ctrl/tree/main/agents)
  - [Docsify](https://github.com/magnumriverosmusic/smartdimension-docs)
  - [Jira Board](https://magnumproject.atlassian.net/jira/software/projects/KAN/boards/2)

---

*Generado por SD-Investiga bajo supervisión de Magnum*  
*2026-05-01 | SmartDimension*  
*Revisión: Pendiente aprobación Iván*
