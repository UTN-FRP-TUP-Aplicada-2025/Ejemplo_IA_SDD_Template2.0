# PROJECT-BRIEF — Delineamientos Técnicos y Metodológicos

> Este documento complementa PROJECT-README.md con información técnica específica.
> Debe ser completado por el equipo técnico o arquitecto del proyecto.
> Incluir capturas de pantalla, diagramas o referencias a archivos cuando sea útil.

---

## Sección 1 — Arquitectura propuesta o existente

**Tipo de arquitectura:**  
<!-- Monolito, microservicios, serverless, híbrida, etc. -->

**Diagrama de arquitectura:**  
<!-- Insertar imagen, link a archivo, o descripción textual del diagrama.
     Si hay capturas de pantalla de la arquitectura, referenciarlas aquí. -->

**Servicios / componentes principales:**

| Componente | Responsabilidad | Tecnología | ¿Nuevo o existente? |
|---|---|---|---|
|  |  |  |  |
|  |  |  |  |

**Microservicios auxiliares o servicios externos:**

| Servicio | Propósito | Protocolo (REST/gRPC/AMQP/etc.) |
|---|---|---|
|  |  |  |

---

## Sección 2 — Capturas de pantalla de referencia

> Para cada pantalla o flujo existente que el cliente mostró, documentar aquí.
> Incluir el archivo (en `/devs/assets/screenshots/`) o describir el contenido.

| ID | Descripción de la pantalla | Archivo / URL | Notas relevantes |
|---|---|---|---|
| SS-01 |  |  |  |
| SS-02 |  |  |  |

---

## Sección 3 — Esquema de aplicaciones

**¿Es una aplicación standalone o parte de un ecosistema?**  
<!-- Describir el landscape de aplicaciones en el que vive este sistema. -->

**Mapa de aplicaciones:**  
<!-- Diagrama o descripción de las apps involucradas, sus relaciones y flujos de datos. -->

**Flujos de datos principales:**

| Flujo | Origen | Destino | Formato | Frecuencia |
|---|---|---|---|---|
|  |  |  |  |  |

---

## Sección 4 — Decisiones de diseño ya tomadas

> Decisiones que ya fueron resueltas antes de arrancar y que los subagentes deben respetar.

| ID | Decisión | Justificación | Impacto en diseño |
|---|---|---|---|
| DD-01 |  |  |  |
| DD-02 |  |  |  |

---

## Sección 5 — Metodología de ejecución

**Metodología de desarrollo:**  
- [ ] Scrum con sprints de 2 semanas
- [ ] Scrum con sprints de 1 semana
- [ ] Kanban continuo
- [ ] Otra: _______________

**Estrategia de entregables:**

- [ ] **Vertical slicing** (entregables funcionales end-to-end por sprint — recomendado)
  - Cada sprint entrega una funcionalidad completa usable por el usuario.
  - Ej: Sprint 1 → "El usuario puede loguearse y ver su perfil"
- [ ] **Horizontal slicing** (entregables por capa técnica)
  - Cada sprint entrega una capa completa (ej: toda la capa de datos).
  - Ej: Sprint 1 → "Toda la base de datos diseñada e implementada"

**Justificación de la elección:**  
<!-- Por qué se eligió esta estrategia para este proyecto. -->

**Velocidad estimada (story points por sprint):**  
**Cantidad de sprints estimados para el MVP:**  

---

## Sección 6 — Infraestructura y deploy

**Ambiente de destino:**  
<!-- Cloud (AWS/Azure/GCP), on-premise, hosting compartido, etc. -->

**Estrategia de deploy:**  
<!-- CI/CD con GitHub Actions, deploy manual, contenedores Docker, etc. -->

**Ambientes requeridos:**
- [ ] Development
- [ ] Staging / QA
- [ ] Production
- [ ] Otro: _______________

**Estrategia de versionado:**  
<!-- SemVer 2.0, fecha-based, otro. -->

---

## Sección 7 — Restricciones técnicas adicionales

**Restricciones de seguridad:**  
<!-- Autenticación requerida, cifrado, compliance (GDPR, SOC2, etc.). -->

**Restricciones de performance:**  
<!-- Tiempo de respuesta máximo, cantidad de usuarios concurrentes, SLA. -->

**Restricciones de compatibilidad:**  
<!-- Versiones mínimas de OS, browsers, dispositivos. -->

---

## Sección 8 — Referencias y assets

**Repositorio del proyecto:**  
**Documentación existente:**  
**Carpeta de assets (capturas, diagramas):** `/devs/assets/`

---

**Fin del documento — PROJECT-BRIEF**
