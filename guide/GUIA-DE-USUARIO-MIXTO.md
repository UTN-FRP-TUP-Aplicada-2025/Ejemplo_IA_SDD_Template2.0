# Guía de Usuario — Flujo Mixto (Claude Web + Claude Code)

**Documento:** GUIA-DE-USUARIO-MIXTO.md
**Versión:** 1.0
**Fecha:** 2026-04-26
**Audiencia:** Desarrolladores que parten del repo `Ejemplo_IA_SDD_Template2.0` ya pre-instalado y prefieren un flujo simplificado
**Complementa a:** [GUIA-DE-USUARIO.md](GUIA-DE-USUARIO.md) (flujo original, completo)

---

## Índice

1. [Por qué un flujo mixto](#1-por-qué-un-flujo-mixto)
2. [Diferencias frente al flujo original](#2-diferencias-frente-al-flujo-original)
3. [Prerequisitos](#3-prerequisitos)
4. [Paso A — Conversación e intakes en Claude Web](#4-paso-a--conversación-e-intakes-en-claude-web)
5. [Paso B — Volcar los intakes al repo](#5-paso-b--volcar-los-intakes-al-repo)
6. [Paso C — Ejecutar el orquestador en Claude Code (por referencia)](#6-paso-c--ejecutar-el-orquestador-en-claude-code-por-referencia)
7. [Paso D — Compuerta de aprobación](#7-paso-d--compuerta-de-aprobación)
8. [Paso E — Fase de codeo](#8-paso-e--fase-de-codeo)
9. [Cheat sheet del flujo mixto](#9-cheat-sheet-del-flujo-mixto)
10. [Ventajas, costos y cuándo NO usar este flujo](#10-ventajas-costos-y-cuándo-no-usar-este-flujo)

---

## 1. Por qué un flujo mixto

El flujo original de [GUIA-DE-USUARIO.md](GUIA-DE-USUARIO.md) asume que arrancás desde un repo vacío y que vas a:

1. Instalar la metodología con `PLAN-RECONVERSION-METODOLOGIA.md` y `CLAUDE-CODE-EXECUTION-PROMPT.md` (Pasos 4 y 5).
2. Ejecutar el orquestador documental en un **Proyecto de Claude.ai web**, copiando los outputs a mano (Paso 6).

El **flujo mixto** parte de dos hechos:

- El template **ya viene pre-instalado** en este repo: [devs/rules/](../devs/rules/), [devs/orchestrator/master-prompt.md](../devs/orchestrator/master-prompt.md) y los templates de [devs/intake/](../devs/intake/) ya existen.
- **Claude Code puede leer y escribir archivos del workspace directamente**, así que no hace falta el ida-y-vuelta manual de copiar bloques markdown.

Resultado: un flujo más corto que combina lo mejor de cada herramienta.

| Etapa | Herramienta usada | Por qué ahí |
|---|---|---|
| Discovery + intakes | Claude Web (chat) | Ideal para conversación libre, subir capturas, replantear |
| Orquestador documental | Claude Code (VS Code) | Lee filesystem y escribe `/docs/` directo, sin pegar bloques |
| Codeo | Claude Code (VS Code) | Igual que el flujo original |

---

## 2. Diferencias frente al flujo original

| Aspecto | Flujo original ([GUIA-DE-USUARIO.md](GUIA-DE-USUARIO.md)) | Flujo mixto (este documento) |
|---|---|---|
| Punto de partida | Repo vacío | Repo `Ejemplo_IA_SDD_Template2.0` clonado |
| Pasos 4 y 5 (instalar template) | Necesarios | **Saltados** — el template ya está |
| Archivos `PLAN-RECONVERSION-...` y `CLAUDE-CODE-EXECUTION-...` | Requeridos | **No usados** |
| Orquestador documental | Claude.ai Proyecto (web) | Claude Code (VS Code) |
| Cómo Claude obtiene los rules | Cargados como knowledge del Proyecto | Los lee del workspace bajo demanda |
| Cómo se guardan los outputs en `/docs/` | Copiar y pegar bloques markdown | **Claude Code los crea directo** |
| Master prompt | Se copia y pega su contenido | **Se referencia el archivo** `@devs/orchestrator/master-prompt.md` |
| Iteración / discusión | Más fluida (chat web) | Igual, dentro del chat de Claude Code |

---

## 3. Prerequisitos

| Herramienta | Para qué |
|---|---|
| Cuenta en [claude.ai](https://claude.ai) (Pro o Team) | Discovery + redacción de intakes |
| VS Code | IDE |
| Claude Code (extensión VS Code) | Orquestador documental + codeo |
| Git | Versionado |
| Repo `Ejemplo_IA_SDD_Template2.0` clonado | Punto de partida con template pre-instalado |

Estructura mínima que ya debe tener el repo (viene del template):

```
[tu-proyecto]/
├── devs/
│   ├── intake/
│   │   ├── PROJECT-README-template.md   ← plantilla
│   │   └── PROJECT-BRIEF-template.md    ← plantilla
│   ├── rules/
│   │   ├── 00_rules.md ... 09_rules.md  ← 10 archivos
│   └── orchestrator/
│       └── master-prompt.md
├── docs/
│   └── .gitkeep
└── guide/
    ├── GUIA-DE-USUARIO.md
    ├── GUIA-DE-USUARIO-MIXTO.md          ← este documento
    ├── INTAKE-HARVEST-PROMPT.md
    └── FUNDAMENTOS-Y-DEFINICIONES.md
```

---

## 4. Paso A — Conversación e intakes en Claude Web

### A.1 Iniciar el discovery

1. Abrí un chat nuevo en [claude.ai](https://claude.ai).
2. Conversá libremente sobre el proyecto: cliente, problema, idea de solución, restricciones, decisiones tomadas, metodología (vertical/horizontal slicing).
3. Subí capturas (Figma, sistemas existentes, diagramas) arrastrándolas al chat.
4. Hacé preguntas abiertas para que Claude proponga alternativas y discutan pros/contras.

### A.2 Cargar las plantillas como referencia

Para que el harvest produzca intakes con la **estructura exacta** que el template espera, subí al chat (o pegá su contenido) las dos plantillas:

- [devs/intake/PROJECT-README-template.md](../devs/intake/PROJECT-README-template.md)
- [devs/intake/PROJECT-BRIEF-template.md](../devs/intake/PROJECT-BRIEF-template.md)

Pedile a Claude algo como:

```
Estas son las dos plantillas que debe seguir la generación de los intakes.
Respetá su estructura, secciones y nomenclatura cuando me los devuelvas.
```

### A.3 Cosechar la conversación

Pegá el contenido de la **Sección 1** de [INTAKE-HARVEST-PROMPT.md](INTAKE-HARVEST-PROMPT.md) como último mensaje. Claude generará dos bloques markdown:

- **Bloque 1** → contenido de `PROJECT-README.md`
- **Bloque 2** → contenido de `PROJECT-BRIEF.md`

Cada bloque incluye al final:
- Lista de campos `[REQUIERE_INFO]` que faltó cubrir
- Lista de **DECISIONES IMPLÍCITAS** que Claude infirió

### A.4 Iterar hasta que estén bien

Si las decisiones implícitas no son correctas o falta info crítica, **respondé en el mismo chat** corrigiendo o aportando datos, y pedí: *"Regenerá los dos bloques actualizados"*. Repetí hasta que estés conforme.

---

## 5. Paso B — Volcar los intakes al repo

1. Copiá el **Bloque 1** y guardalo como [devs/intake/PROJECT-README.md](../devs/intake/PROJECT-README.md).
2. Copiá el **Bloque 2** y guardalo como [devs/intake/PROJECT-BRIEF.md](../devs/intake/PROJECT-BRIEF.md).
3. En VS Code, abrí ambos archivos y:
   - Buscá todas las ocurrencias de `[REQUIERE_INFO]` y completalas.
   - Confirmá o corregí las DECISIONES IMPLÍCITAS.
   - Si subiste capturas en Claude Web, copiá los archivos a `devs/assets/screenshots/` con los nombres que la tabla `SS-XX` del BRIEF referencia.

Tu repo ahora se ve así (ejemplo real del usuario):

```
[tu-proyecto]/
├── .gitignore
├── README.md
├── vs.bat
├── devs/
│   ├── intake/
│   │   ├── PROJECT-BRIEF.md          ← generado en Paso A
│   │   └── PROJECT-README.md         ← generado en Paso A
│   ├── orchestrator/
│   │   └── master-prompt.md
│   └── rules/
│       ├── 00_rules.md ... 09_rules.md
└── docs/
    └── .gitkeep
```

> Las plantillas `*-template.md` podés borrarlas, conservarlas como referencia, o dejarlas — no afectan al orquestador.

---

## 6. Paso C — Ejecutar el orquestador en Claude Code (por referencia)

### C.1 Por qué referenciar el archivo en lugar de pegar el prompt

El flujo original te hace **copiar y pegar** el contenido del bloque "El Prompt" de [devs/orchestrator/master-prompt.md](../devs/orchestrator/master-prompt.md). En Claude Code esto es innecesario y subóptimo porque:

- **Errores de copia**: si el bloque es largo, es fácil omitir líneas o cortar en lugares no esperados.
- **Versionado**: si actualizás el `master-prompt.md`, todo lo pegado en chats viejos queda desincronizado.
- **Claridad**: una referencia `@archivo` deja explícito qué está leyendo Claude.
- **Tokens**: el contenido se carga una sola vez al ser leído, no se transporta en cada turno del chat.

Claude Code tiene soporte nativo para referenciar archivos del workspace usando el prefijo `@`.

### C.2 Abrir el workspace en VS Code

Abrí en VS Code la carpeta raíz del proyecto (la que contiene `devs/` y `docs/`) y abrí el panel de Claude Code.

### C.3 Pegar el prompt de arranque

Pegá esto en el chat de Claude Code (un solo mensaje):

```
Sos el orquestador maestro de la cadena de subagentes SDD de este proyecto.

Leé y seguí estrictamente las instrucciones del master prompt:
@devs/orchestrator/master-prompt.md

Insumos del proyecto (leelos primero):
- @devs/intake/PROJECT-README.md
- @devs/intake/PROJECT-BRIEF.md

Reglas constructivas (leé el archivo correspondiente ANTES de ejecutar cada SA-XX):
- @devs/rules/00_rules.md hasta @devs/rules/09_rules.md

Reglas operativas para Claude Code:
1. Trabajá directamente sobre el filesystem del workspace, sin pedirme que pegue contenido.
2. CREÁ los archivos de salida directamente en /docs/ con la estructura indicada en cada rules.md.
   No me devuelvas bloques markdown para que yo los pegue.
3. Antes de cada SA-XX: decime qué archivos vas a leer y qué vas a producir.
4. Después de cada SA-XX: listá los archivos creados con su ruta.
5. Respetá el grafo de dependencias del master-prompt (SA-00 → SA-01 → ... → SA-09).
6. Pará al finalizar SA-09 y presentame el Informe de Completitud.
7. NO inicies la fase de codeo hasta que yo escriba "APROBADO".

Empezá leyendo los insumos y devolviéndome un resumen de 3 puntos del proyecto
para confirmar que entendiste el contexto. Después arrancá con SA-00.
```

### C.4 Durante la ejecución

- Vas a ver los archivos aparecer en [docs/](../docs/) en tiempo real, con diffs en VS Code.
- Si Claude Code pausa entre subagentes, respondé `continuá con SA-XX`.
- Si trunca un archivo (caso raro pero posible):
  ```
  El archivo [ruta] está incompleto desde la sección [X].
  Releé @devs/rules/XX_rules.md y regenerá el archivo completo sin abreviar.
  ```
- Si la cadena se corta:
  ```
  Continuá con SA-XX. Releé @devs/rules/XX_rules.md y los upstream necesarios.
  Producí todos los artefactos marcados como "Obligatorio: Sí".
  ```

### C.5 Al terminar

Claude Code presenta el **Informe de Completitud**: tabla de artefactos, campos `[REQUIERE_INFO]` pendientes, mapa de trazabilidad NB → CU → US → Sprint, y puntuación por sección.

---

## 7. Paso D — Compuerta de aprobación

Idéntico al Paso 7 del flujo original ([GUIA-DE-USUARIO.md](GUIA-DE-USUARIO.md#9-paso-7--la-compuerta-de-aprobación)):

**Revisión mínima (30 min)** — leer al menos:
- `docs/00_contexto/vision-producto_v1.0.md`
- `docs/01_necesidades_negocio/necesidades-negocio_v1.0.md`
- `docs/06_backlog-tecnico/product-backlog_v1.0.md`
- `docs/07_plan-sprint/plan-iteracion_sprint-01_v1.0.md`

Ventaja del flujo mixto: como ya están como archivos en VS Code, podés revisarlos con tu editor + extensiones (preview markdown, diff con git, etc.).

**Sugerir cambios** en el mismo chat de Claude Code:
```
Modificaciones antes de aprobar:
1. En docs/00_contexto/vision-producto_v1.0.md, la propuesta de valor dice X pero debería decir Y.
2. Agregá una NB sobre [tema X].
3. Reducí el Sprint 1 a US-01 y US-02.
```

**Aprobar**:
```
APROBADO
```

---

## 8. Paso E — Fase de codeo

Idéntico al Paso 8 del flujo original. Como ya estás en Claude Code, el flujo es directo:

1. Abrí cada `docs/06_backlog-tecnico/user-stories/US-XX-[nombre].md`.
2. Pedile a Claude Code: *"Implementá esta User Story leyendo el CU relacionado, la arquitectura de @docs/05_arquitectura_tecnica/ y verificando el DoD de @docs/00_contexto/acuerdo-equipo_v1.0.md."*
3. Confirmá tests pasando antes de la siguiente US.
4. Actualizá el estado del CU/US a `Implementado` con el hash del commit.

Si surge un cambio de spec durante el codeo: **primero actualizar `/docs/`, después el código** — nunca al revés.

---

## 9. Cheat sheet del flujo mixto

```
[CLAUDE WEB]
  A.1 Conversación libre + capturas + replanteos
  A.2 Subir plantillas PROJECT-*-template.md como referencia
  A.3 Pegar Sección 1 de INTAKE-HARVEST-PROMPT.md → recibir 2 bloques
  A.4 Iterar correcciones hasta estar conforme

[VS CODE]
  B.  Guardar bloques en devs/intake/PROJECT-README.md y PROJECT-BRIEF.md
      Completar [REQUIERE_INFO], confirmar DECISIONES IMPLÍCITAS
      Copiar capturas a devs/assets/screenshots/

[CLAUDE CODE en VS CODE]
  C.  Pegar el prompt de arranque del Paso C.3 (referencia archivos con @)
      Claude Code lee, ejecuta SA-00 → SA-09 y escribe /docs/ directo
  D.  Revisar /docs/, pedir ajustes, escribir APROBADO
  E.  Implementar US por US, con tests y DoD verificado
```

---

## 10. Ventajas, costos y cuándo NO usar este flujo

### Ventajas

- **Menos copy-paste manual**: Claude Code escribe los archivos en `/docs/` directamente.
- **Sin fricción de Proyectos**: no hace falta crear un Proyecto en Claude.ai ni cargar 12 archivos como knowledge.
- **Trazabilidad git inmediata**: cada SA-XX puede ser un commit, ves los diffs en VS Code.
- **Master prompt referenciado, no copiado**: si actualizás `master-prompt.md`, el flujo siguiente lo usa automáticamente.
- **Discovery sigue siendo conversacional**: Claude Web sigue siendo el mejor entorno para discutir ideas y subir imágenes.

### Costos

- Una sola sesión de Claude Code procesa una cantidad grande de archivos: si tenés cuota limitada, monitoreá el consumo.
- Si Claude Code se detiene a mitad, tenés que reanudar manualmente con el prompt de continuación (igual que el flujo original).
- La revisión de la documentación generada requiere disciplina: como aparece sola en el repo, es tentador aprobar sin leer.

### Cuándo NO usar este flujo

- **Si no tenés Claude Code instalado** o tu suscripción no lo incluye → usá el flujo original.
- **Si querés revisar cada documento turno a turno** con discusión iterativa antes de pasar al siguiente → el Proyecto en Claude Web es más natural.
- **Si tu equipo no tiene VS Code como IDE estándar** → el flujo original es agnóstico al editor.

---

**Fin del documento — GUIA-DE-USUARIO-MIXTO.md**
