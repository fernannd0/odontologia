# Odontologia — Instrucciones para Agentes

> Base documental generada a partir de la KB, el roadmap y las decisiones del usuario.
> Estado al 2026-11-26. Los 16 changes del MVP están archivados. La aceptación integrada por Gmail quedó diferida por decisión de Fernando.
> `AGENTS.md` y `CLAUDE.md` son copias idénticas; sincronizar ambos al modificarlos.

El proyecto reduce el trabajo de reservar, confirmar y reprogramar turnos de un odontólogo independiente. La primera entrega es local: un consultorio, un odontólogo, una agenda, datos ficticios y emails reales a buzones de prueba habilitados. El nombre comercial y la validación con odontólogos siguen pendientes.

## Stack Tecnológico

La tabla resume la selección aprobada en [KB 02](knowledge-base/02_descripcion_general.md#stack-tecnológico) y las versiones concretadas en C-01; ver [README](README.md).

| Capa o función | Tecnología aprobada | Versión o condición vigente |
|---|---|---|
| Lenguaje del backend | Java, distribución Eclipse Temurin | JDK 21.0.13.1+2. |
| Backend | Spring Boot | 4.2.0; dependencias Java administradas por su BOM. |
| Construcción del backend | Gradle mediante Gradle Wrapper | 9.9.2, Kotlin DSL y distribución verificada por SHA-256. |
| Interfaz web | React y TypeScript, con CSS | React/React DOM 19.3.1 y TypeScript 7.0.2 estricto. |
| Desarrollo y compilación web | Vite, Node.js y npm | Vite 8.3.8, plugin React 6.3.0, Node.js 24.22.3 y npm 12.2.3. |
| Persistencia | PostgreSQL local | 17.12; Compose local autorizado en loopback:5433 o instancia propia habilitada. |
| Envío de correo | Spring Mail y SMTP | C-11 usa la cuenta dedicada de Gmail como remitente y único destinatario de prueba. SMTP usa STARTTLS y está deshabilitado por defecto. |
| Recordatorios | Tarea programada dentro del backend y estado de notificaciones persistido | C-11 sondea cada 30 segundos. Los fallos transitorios conocidos tienen hasta tres intentos y un intento incierto conserva su historial sin reenvío automático. |

En desarrollo, Vite se ejecuta separado con proxy de API. El JAR integrado de C-16 sirve React y la API desde Spring Boot en el mismo origen. C-02 implementó el esquema inicial con Spring JDBC y Flyway y el protocolo de escritura por agenda. C-03 incorporó el acceso interno con Spring Security y C-04 incorporó la credencial de gestión. Los cambios a esos mecanismos mantienen la gobernanza CRITICAL.

## Base de Conocimiento

Leer [el índice y sus estados](knowledge-base/README.md) y los documentos pertinentes antes de especificar o implementar.

| Archivo | Cuándo leerlo |
|---|---|
| [01 — Visión y objetivos](knowledge-base/01_vision_y_objetivos.md) | Alcance local y aceptación CA-VIS. |
| [02 — Descripción general](knowledge-base/02_descripcion_general.md) | Stack, componentes, empaquetado y API orientativa. |
| [03 — Actores y roles](knowledge-base/03_actores_y_roles.md) | Permisos, rutas públicas, acceso interno y enlace privado. |
| [04 — Modelo de datos](knowledge-base/04_modelo_de_datos.md) | Entidades, relaciones, restricciones e inicialización. |
| [05 — Reglas de negocio](knowledge-base/05_reglas_de_negocio.md) | RN, estados y preguntas PB. |
| [06 — Funcionalidades](knowledge-base/06_funcionalidades.md) | Doce US, 48 CA-US y seis criterios transversales. |
| [07 — Flujos principales](knowledge-base/07_flujos_principales.md) | Nueve flujos y sus resultados de error. |
| [08 — Arquitectura propuesta](knowledge-base/08_arquitectura_propuesta.md) | Propuestas PT de persistencia, concurrencia, avisos y acceso. |
| [09 — Decisiones y supuestos](knowledge-base/09_decisiones_y_supuestos.md) | Distinguir decisiones aprobadas, propuestas y supuestos. |
| [10 — Preguntas abiertas](knowledge-base/10_preguntas_abiertas.md) | Resolver pendientes del comportamiento afectado antes de implementarlo. |

01–07 están aprobados; 08–10 se redactaron en bloque autorizado y conservan propuestas y preguntas pendientes. Las decisiones explícitas posteriores del usuario precisan los documentos históricos. Consultar también [Discovery](docs/discovery/discovery.md), [entrega y stack aprobados](docs/discovery/propuesta-tecnica-kb.md) y [política visual vigente](design-system/README.md).

Para C-01 se revisan AT-01, AT-02, AT-08 y AT-10; AP-03 fija un calendario real. Las reglas temporales, pacientes repetidos, conflictos, enlaces y correo se resuelven antes de sus cambios afectados, según `CHANGES.md`. PB-12 y AT-09 condicionan el uso de datos reales. AT-11 quedó resuelto por política visual `off`.

## Skills Disponibles

El [registro local](.atl/skill-registry.md) es la fuente de nombres, triggers, rutas y reglas resumidas. Este mapa selecciona las skills pertinentes al MVP; no convierte todo el inventario en instrucciones activas.

| Agente o rol | Responsabilidad | Skills que carga según la tarea |
|---|---|---|
| Backend Core | Modelo, consultas, transacciones y concurrencia PostgreSQL | [supabase-postgres-best-practices](.agents/skills/supabase-postgres-best-practices/SKILL.md). |
| Backend Aux | Persistencia de avisos e integración SMTP | `supabase-postgres-best-practices` cuando modifique PostgreSQL; documentos 05, 07 y 08 para el flujo de comunicaciones. |
| Frontend | Reserva, autogestión y agenda en React con Vite | [vercel-react-best-practices](.agents/skills/vercel-react-best-practices/SKILL.md); solo las reglas aplicables a React cliente. |
| Verificación | Recorridos locales, errores y evidencia de navegador | [webapp-testing](.agents/skills/webapp-testing/SKILL.md). |
| Orquestación documental | Fundación, KB, roadmap e instrucciones | `active-orchestrator`, `kb-creator`, `roadmap-generator`, `find-skills`, `agents-md-generator`; rutas en el registro. |

Las tres skills de implementación/verificación se instalaron solo en `.agents/skills/` de este proyecto. `springboot-patterns` fue excluida por el usuario. Las plantillas de plugins en caché requieren comprobar disponibilidad; su presencia no activa conexiones ni publicación.

Los compact rules de cada skill los resuelve el orquestador desde `.atl/skill-registry.md` (generado por `skill-registry`; archivo local de apoyo). Esta tabla solo mapea skill a rol; no duplica esas reglas. Leer el `SKILL.md` original cuando lo exijan las instrucciones vigentes.

El mapa de roles no autoriza por sí solo lanzar subagentes. Usar delegación únicamente cuando el usuario o las instrucciones aplicables la pidan explícitamente.

## Roadmap de Changes

El índice operativo es [CHANGES.md](CHANGES.md). Los **16 changes del roadmap** y el seguimiento correctivo de C-14 están archivados en OpenSpec.

1. Entorno local y modelo persistente.
2. Acceso e intenciones durables.
3. Configuración y consulta de disponibilidad.
4. Reserva, autogestión y comunicaciones.
5. Pantallas funcionales del MVP.
6. Empaquetado y aceptación local.

Cadena máxima de precedencias con duración unitaria: `C-01 → C-02 → C-03 → C-06 → C-07 → C-08 → C-09 → C-11 → C-16` (nueve changes). Las ramas restantes también son necesarias para el cierre. El plan no promete una fecha ni duración total.

El archivo de [C-11](openspec/changes/archive/2026-11-26-envio-email-recordatorios/verification.md) registra el cierre autorizado con la campaña integrada de Gmail diferida. El [seguimiento de C-14](openspec/changes/archive/2026-11-26-c14-reapertura-enlace-misma-credencial/verification.md) conserva la evidencia automatizada de Linux y la confirmación manual de Windows informada por Fernando. La [matriz de C-16](openspec/changes/archive/2026-11-26-entrega-local-verificacion/verification.md) registra las pruebas de entrega y los criterios de Gmail que se retomarán con `CORRECCIONES.md`, en la carpeta padre del repositorio.

## Reglas Duras (específicas del proyecto)

Estas reglas recogen decisiones e instrucciones ya recibidas. Se comprobó que `~/.claude/CLAUDE.md` no existe en este entorno; no se presume que un archivo global aporte las reglas que siguen. El usuario aprobó las convenciones adicionales del stack el 2026-11-19 mediante «dale incorpora eso y cerremos la fundacion».

### Convenciones del stack aprobadas

- **TS-01 — TypeScript:** usar modo estricto. No escribir `any` manualmente; usar tipos concretos o `unknown` con validación antes de consumir el valor.
- **JAVA-01 — Java/Spring:** usar inyección de dependencias por constructor. Nombrar clases Java en `PascalCase` y métodos/variables en `camelCase`.

### Alcance y decisiones vigentes

- Mantener el alcance de un consultorio, un odontólogo y una agenda local. Clínica, cobros, WhatsApp, recepción, múltiples organizaciones y despliegue público son posteriores.
- Preservar la reserva sin cuenta de paciente ni verificación previa del email. Aplicar las reglas aprobadas de disponibilidad, prevención de solapamientos, cancelación y reprogramación, resolviendo sus límites pendientes.
- No convertir PT, PB, AT o supuestos en decisiones aprobadas por inferencia. Resolver solo los pendientes necesarios para el cambio afectado y registrar quién decidió y qué evidencia existe.
- Mantener la política visual `off`: omitir sistema de diseño y revisión rutinaria de mockups. Cambiar esta política solo por pedido explícito del usuario.
- Instalar skills únicamente dentro de este proyecto y solo cuando estén autorizadas. Conservar la exclusión de `springboot-patterns`.
- Priorizar tecnologías y servicios gratuitos cuando sea posible; no hay un tope absoluto de ARS 0 ni autorización para contratar servicios pagos.
- Verificar los criterios con evidencia adecuada: PostgreSQL real y solicitudes sincronizadas para concurrencia; recepción en buzón de prueba para email; persistencia tras reinicio para recuperación local. Una prueba de reinicio no acredita restauración de respaldos.
- Ejecutar builds y comprobaciones pertinentes al cambio autorizado. No declarar historias o criterios cumplidos por existir documentos, interfaz parcial o aceptación SMTP.

### Gobernanza por dominio

| Nivel | Dominio | Autonomía |
|---|---|---|
| CRITICAL | Autenticación, seguridad, cobros y auditoría | Análisis; código solo con aprobación humana explícita. |
| HIGH | Configuración que afecte datos, dotfiles, respaldos y restauración | Proponer y esperar revisión antes de escribir. |
| MEDIUM | Reglas de negocio, adaptadores y pipelines | Implementar con checkpoints y comunicar decisiones. |
| LOW | CRUD simple, catálogos, tipos y utilidades de consulta | Autonomía con las comprobaciones pertinentes. |

Aplicar el nivel del alcance más sensible. La autorización para documentar o aprobar el roadmap no autoriza código CRITICAL. Respetar las autorizaciones ya dadas sin solicitar otra vez la misma aprobación.

### Memoria persistente

- El protocolo Engram está siempre activo. Guardar inmediatamente decisiones de arquitectura/diseño, convenciones, cambios de workflow, elecciones de herramientas, fixes con causa raíz, implementaciones no obvias, artefactos significativos, configuración, descubrimientos, gotchas y preferencias aprendidas.
- Usar `mem_save` con título breve, tipo y alcance `project` por defecto; contenido con **What**, **Why**, **Where** y **Learned** cuando corresponda.
- Usar un `topic_key` estable para la evolución del mismo tema; no sobrescribir temas distintos. Si hay duda, consultar `mem_suggest_topic_key`; si se conoce el ID a corregir, usar `mem_update`.
- Al recordar trabajo previo: `mem_context`; si no alcanza, `mem_search` y luego `mem_get_observation` para el contenido completo.
- Buscar memoria al iniciar trabajo que pudo hacerse antes o sobre un tema sin contexto; ante el primer mensaje del usuario sobre el proyecto, una funcionalidad o un problema, buscar sus palabras clave.
- Al finalizar cada tarea, comprobar si hubo una decisión, fix, descubrimiento o convención y guardar lo aprendido antes de responder.

## Flujo de Trabajo

1. Leer estas instrucciones, la KB pertinente y sus estados de aprobación.
2. Consultar `CHANGES.md`, seleccionar un change y comprobar dependencias, pendientes y gobernanza.
3. Usar OPSX solo por invocación explícita de `$sdd-orchestrator`. El CLI OpenSpec administra el estado de los artefactos: consultarlo antes de actuar, sin adivinar estados ni crear directorios `openspec/` manualmente.
4. Preparar propuesta, especificaciones, diseño y tareas con el flujo disponible; obtener las aprobaciones necesarias antes de implementar el dominio correspondiente. En trabajo no visual, registrar `Decision: no (no UI)` si existe documento de diseño; en trabajo visual, respetar `off` y registrar `Decision: no (project policy)` cuando corresponda.
5. Implementar el alcance autorizado cargando las skills aplicables y verificando resultados con evidencia. No introducir librerías ni mecanismos de seguridad por ejemplos de una skill.
6. Archivar con el CLI cuando corresponda y marcar el índice solo después de verificar y archivar. Sincronizar `AGENTS.md` y `CLAUDE.md` al actualizar sus reglas.

El [README](README.md) documenta los comandos de build, pruebas y ejecución del JAR integrado. Incluye el inicio de PostgreSQL con Docker y la configuración de los archivos privados usados por el acceso interno y los enlaces de gestión.

