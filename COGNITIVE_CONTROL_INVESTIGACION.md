# KAN-26: Investigación Preliminar — Cognitive Control

**Feature:** KAN-47 (Cognitive Control)  
**Agente:** SD-Investiga  
**Fecha:** 2026-05-01  
**Estado:** En análisis

---

## 1. Intención del Usuario (Iván)

> *"Quiero un mecanismo de visualización en tiempo real de cada agente, con características y bitácora, considerando todos los aspectos relevantes analizados en nuestra conversación."*

### Contexto de la Solicitud
- Magnum es superorquestador + personal agent
- SmartDimension-Orq coordina 6 skill units
- Abrazzia-Orq coordina cuidado de Carlotica
- Dana es personal bot de bienestar
- Centinela es exoesqueleto intelectual senior
- Hospedaje gestiona suite moderna

### Requerimientos Implícitos
1. **Visualización:** Grafo dinámico Hub & Spoke
2. **Tiempo real:** Actualización de estado cada 30s
3. **Bitácora:** Historial de acciones por agente
4. **Características:** Identidad, canales, tareas, ética

---

## 2. Análisis de Sistemas Existentes

### 2.1 Cognitive Control Actual (Legacy)
- Stack: Astro + React + ReactFlow
- Estado: Nodos hardcodeados en `CognitiveGraph.jsx`
- Datos: `consciousness.js` con stats estáticos
- Limitaciones: No conecta con Jira, no es dinámico

### 2.2 Sistemas de Referencia

| Sistema | Fortaleza | Debilidad | Relevancia |
|---------|-----------|-----------|------------|
| Grafana | Dashboards flexibles | No grafo de agentes | UI de métricas |
| ReactFlow | Grafos interactivos | No persistencia | Motor visual |
| LangSmith | Tracing de LLMs | Cerrado, costoso | Patrón de bitácora |
| n8n | Workflow visual | No agentes autónomos | Inspiración UI |

---

## 3. Coherencia Pragmática

### 3.1 ¿Qué es técnicamente factible HOY?

| Capacidad | Feasible | Esfuerzo | Nota |
|-----------|----------|----------|------|
| Grafo estático con datos de archivo JSON | ✅ Sí | 2h | Ya existe base |
| Actualización cada 30s desde archivo | ✅ Sí | 4h | File watcher |
| Conexión API Jira para estado real | ⚠️ Parcial | 8h | Necesita auth |
| Bitácora con historial completo | ⚠️ Parcial | 12h | Necesita DB |
| Telemetría en tiempo real (WebSocket) | ❌ No | 24h+ | Infraestructura |

### 3.2 Decisiones de Arquitectura

**Fase 1 (Ahora):** Archivo JSON como "fuente de verdad temporal"  
**Fase 2 (Próxima semana):** API REST simple para actualizar estado  
**Fase 3 (Futuro):** WebSocket + base de datos persistente

---

## 4. Aspectos de Confiabilidad

### 4.1 Riesgos Identificados

| Riesgo | Probabilidad | Impacto | Mitigación |
|--------|-------------|---------|------------|
| Datos desactualizados | Alta | Medio | Timestamp + indicador de freshness |
| Agente caído sin detectar | Media | Alto | Heartbeat cada 5 min |
| Confusión entre estados | Media | Medio | Semántica clara: idle/active/completed/error |
| Sobrecarga visual | Baja | Medio | Filtrado por orquestador |

### 4.2 Métricas de Confiabilidad

```
Confiabilidad = (Agentes reportados OK / Agentes esperados) × 100
 freshness < 5 min = ✅ fresco
 freshness 5-15 min = ⚠️ stale
 freshness > 15 min = ❌ offline
```

---

## 5. Productos de Trabajo

### 5.1 Página de Investigación (este documento)
- ✅ Análisis preliminar
- ✅ Coherencia pragmática
- ✅ Aspectos de confiabilidad

### 5.2 Artefactos Generados

| Artefacto | Ubicación | Estado |
|-----------|-----------|--------|
| Investigación | `docs/COGNITIVE_CONTROL_INVESTIGACION.md` | ✅ |
| Especificación (KAN-27) | `docs/COGNITIVE_CONTROL_ESPECIFICACION.md` | 🔄 Pendiente |
| Datos de agentes | `swarm-ctrl/agents/*.json` | ✅ Base |
| Dashboard legacy | `cognitive-control/src/` | ✅ Existente |

---

## 6. Recomendaciones para KAN-27 (Especificación)

### 6.1 Alcance de la SDD
Definir:
1. **Modelo de datos:** ¿Cómo se representa un agente en el sistema?
2. **Flujo de estado:** ¿Cómo cambia un agente de idle → active → completed?
3. **Protocolo de actualización:** ¿Quién y cuándo actualiza el estado?
4. **UI/UX:** ¿Cómo se ve la bitácora? ¿Qué información se muestra?

### 6.2 Dependencias
- KAN-26 (este documento) ✅
- Definiciones de agentes en `swarm-ctrl/agents/` ✅
- Acceso a Jira API (parcial) ⚠️

---

## 7. Próximos Pasos

1. **Aprobar** esta investigación → Avanzar a KAN-27
2. **Refinar** requerimientos con Iván
3. **Definir** formato de bitácora (¿Markdown? ¿JSON? ¿DB?)
4. **Priorizar** Fase 1 vs Fase 2 vs Fase 3

---

**Certificación:** ¿Expectativa inicial = resultado?  
- Investigación completa: ✅  
- Decisiones de arquitectura documentadas: ✅  
- Riesgos identificados: ✅  
- **APROBADA** para pasar a especificación

---

*Generado por SD-Investiga bajo supervisión de Magnum*  
*2026-05-01 | SmartDimension*
