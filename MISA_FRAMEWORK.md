# SmartDimension: Marco de Implementación de Sistemas de Agentes (MISA) v1.0

## 1. Concepción: El Nexo Soberano
SmartDimension no es una colección de scripts; es un **ecosistema cognitivo orquestado**. La arquitectura se basa en un modelo **Hub & Spoke** donde el Hub (Magnum) actúa como el director de orquesta y los Spokes (Abrazzia, Soberanía Financiera, Desarrollo) son dominios de ejecución con agentes especializados (como Dana o el Senior Dev).

## 2. El Centinela Epistémico: Mapeo de las 7 Habilidades
Hemos mapeado nuestra ejecución actual contra las mejores prácticas de la industria (basado en el framework de Agent Engineering):

| Habilidad | Implementación en SmartDimension | Estado |
| :--- | :--- | :--- |
| **1. System Design** | Arquitectura Hub & Spoke. Orquestación Magnum -> Sub-agentes. | ✅ Sólido |
| **2. Tool Design** | Uso de SKILL.md como contratos de herramientas (CLI-first). | ✅ Sólido |
| **3. RAG / Facts** | Memoria distribuida (`MEMORY.md`, `memory/*.md`, `dana_inventory.json`). | 🔄 Iterando |
| **4. Reliability** | Cron jobs para tareas críticas (Dana). Validación de fotos vs facturas. | 🔄 Iterando |
| **5. Security** | Aislamiento de sesiones (`visibility=tree`). Guardrails de sistema. | ✅ Sólido |
| **6. Evaluation** | Dashboard de "Control Cognitivo" (Entropía, Latencia, Éxito). | 🚀 En Desarrollo |
| **7. Product Thinking** | Enfoque humano: Dana no es un bot médico, es una "compañera de vida". | ✅ Sólido |

## 3. Modelo de Mejores Prácticas (MISA)
Para asegurar que cada nuevo Spoke o Agente sea "SmartDimension Ready", debe cumplir con:
1. **Contrato de Identidad:** Todo agente debe tener un `AGENTS_<NOMBRE>.md` con tono y prohibiciones claras.
2. **Protocolo de Memoria:** Definir qué datos son efímeros y cuáles van al núcleo (`MEMORY.md`).
3. **Interfaz de Acción:** Las herramientas deben ser CLI o API con contratos de error definidos.
4. **Supervisión Silenciosa:** Los agentes deben pedir apoyo ("Pasivo-Solicitante"), no dar órdenes.

## 4. Próximos Pasos (Fase 6)
- **Consolidar el Radar Epistémico:** Automatizar la búsqueda de papers para mantener el `STATE_OF_THE_ART.md` vivo.
- **Telemetría de Agentes:** Inyectar métricas de éxito de Dana y otros sub-agentes en el Dashboard interactivo.
