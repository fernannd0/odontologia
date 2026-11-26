# Propuesta técnica y estructura de la KB

Fecha: 2026-11-18. Fase activa: Knowledge Base (KB).

**Estado**: decisiones de entrega, stack y estructura aprobados por el usuario el 2026-11-18.
**Confirmación de stack y estructura**: «Aprobar stack y estructura; comenzar la redacción».
No se instaló software ni se implementó la aplicación. Los documentos 01–07 de la KB se redactaron y aprobaron por separado; luego el usuario autorizó completar 08–10 en bloque sin nuevas pausas individuales.

## Decisiones confirmadas en la fase KB

El usuario eligió:

- Primera versión limitada a **un único consultorio**, un odontólogo y una agenda.
- **Versión funcional local**, con emails reales a destinatarios de prueba.
- Propuesta de una combinación técnica sencilla, con opciones gratuitas, para revisarla antes de adoptarla.

Se conserva el objetivo comercial de vender un SaaS a consultorios. La primera entrega no incluye operar varias organizaciones desde una misma aplicación. Los demás requisitos aprobados siguen en [discovery.md](discovery.md), incluidos los cinco datos obligatorios, reserva sin cuenta ni verificación previa, duraciones configurables y reglas de 24 horas y 30 días. El plazo sigue siendo una semana, sin fecha de inicio acordada.

## Combinación aprobada

| Capa | Selección aprobada | Motivo y costo de complejidad |
|---|---|---|
| JVM | Java 21, distribución Eclipse Temurin | Implementar el backend en un lenguaje compatible con el stack obligatorio. Temurin se distribuye sin costo; elegir Java frente a Kotlin evita incorporar otro lenguaje nuevo. No se presupone experiencia previa del usuario en ninguno de los dos. |
| Backend | Spring Boot, versión estable compatible; Gradle mediante Wrapper | Tecnologías obligatorias. Una sola aplicación organizada por funciones de agenda, pacientes y notificaciones. Fijar las versiones exactas al preparar el proyecto; evitar versiones preliminares. |
| Interfaz | React + TypeScript + Vite y CSS | React es la incorporación aprobada; TypeScript es obligatorio. Vite tiene plantilla oficial `react-ts`. El frontend se compila a archivos estáticos; Node.js y npm se usan para desarrollo y compilación. Hay que mantener dos herramientas de construcción: Gradle y npm. |
| Persistencia | PostgreSQL instalado localmente | Conserva turnos entre reinicios y permite reforzar la prevención de solapamientos en la base de datos. Requiere mantener un proceso de base de datos local. Versión estable soportada y control de concurrencia concreto pendientes de fijación. |
| Email | Integración SMTP mediante Spring Mail | Separar el envío de la lógica de turnos permite elegir después un proveedor. Cuenta, credenciales, remitente, límites y condiciones gratuitas aún no seleccionados. No equivale a disponer ya de un servicio gratuito de correo. |
| Recordatorios | Tarea programada dentro del backend y estado de notificaciones persistido | Evitar agregar infraestructura de mensajería para esta entrega. Persistir pendientes permite diseñar reintentos; no garantiza entrega exactamente una vez. Definir duplicados, fallas y recuperación en la KB. |
| Ejecución | Navegador, backend y PostgreSQL en la misma computadora | Cumple la entrega local; no requiere contratar alojamiento ni dominio para ejecutar la aplicación. El envío de correo sí necesita conectividad y una cuenta habilitada. |

El usuario aprobó esta combinación para el contexto de una semana y un consultorio. La selección de versiones exactas deberá comprobar compatibilidad entre herramientas y fijarse de forma reproducible; Java 21 es la versión de lenguaje acordada. Los controles de acceso y enlaces privados se analizarán en la KB; esta revisión no autoriza por sí sola escribir código de dominios CRITICAL.

## Alternativas consideradas

| Alternativa | Ventaja | Costo o limitación |
|---|---|---|
| TypeScript sin React, con Vite | Menos dependencias y una plantilla oficial disponible. | Gestionar manualmente estados, formularios y actualizaciones de agenda. Puede convenir si el usuario domina este enfoque. |
| Base de datos embebida en archivo | Reduce procesos necesarios para una demostración local. | Introduce diferencias frente al PostgreSQL propuesto para una evolución comercial y obliga a reevaluar la concurrencia y las migraciones. No se propone persistir solo en memoria. |

La selección de React y PostgreSQL se aprobó a partir de una valoración de diseño, no de un resultado medido de productividad. Las alternativas se conservan como contexto de la decisión.

## Alcance operativo de la demostración local

- Un correo real puede llegar a un destinatario externo. Un enlace que apunte a `localhost` se abre contra la computadora de quien lo pulsa: para esta demostración, reserva y autogestión se prueban desde la computadora que ejecuta la aplicación.
- Probar en otro dispositivo requiere definir acceso de red y URL alcanzable. No se presume un túnel, alojamiento público ni servicio contratado.
- El backend debe estar ejecutándose y tener conexión para enviar los recordatorios en el momento previsto. Recuperación de pendientes tras reinicios y avisos vencidos requieren una política explícita.
- Los datos de demostración serán ficticios y las direcciones pertenecerán a destinatarios de prueba. No hace falta cargar datos clínicos para comprobar los flujos acordados.
- Ni instalar el sistema localmente ni enviar un correo de prueba acredita disponibilidad continua, entrega garantizada o aptitud comercial.

## Estructura de KB aprobada

Se aprobaron los diez archivos canónicos y un índice, sin documentos adicionales por ahora:

| Archivo en `knowledge-base/` | Contenido |
|---|---|
| `README.md` | Índice, orden de lectura y estado de revisión. |
| `01_vision_y_objetivos.md` | Problema, actores, primera entrega local, objetivo comercial futuro y criterios de éxito propuestos. |
| `02_descripcion_general.md` | Stack aprobado o propuesto, componentes e integración de email. |
| `03_actores_y_roles.md` | Odontólogo, paciente, permisos y acceso público limitado. |
| `04_modelo_de_datos.md` | Consultorio, agenda, motivos, horarios, bloqueos, pacientes, turnos y notificaciones; relaciones y restricciones propuestas. |
| `05_reglas_de_negocio.md` | Reglas identificadas y trazables de disponibilidad, reserva, autogestión y avisos. |
| `06_funcionalidades.md` | Historias de usuario y criterios de aceptación del MVP. |
| `07_flujos_principales.md` | Configuración, reserva, cancelación, reprogramación y comunicaciones, con casos de error. |
| `08_arquitectura_propuesta.md` | Estructura técnica, persistencia, concurrencia, recordatorios y análisis de controles de acceso. |
| `09_decisiones_y_supuestos.md` | Decisiones confirmadas, propuestas, alternativas y supuestos por validar. |
| `10_preguntas_abiertas.md` | Pendientes priorizados, qué bloquean y quién debe resolverlos. |

El proceso inicial de revisión por archivo se aplicó hasta 07. El usuario autorizó posteriormente redactar 08–10 en bloque. La aprobación de esta estructura y del stack no cierra automáticamente los pendientes de negocio del Discovery ni aprueba propuestas técnicas nuevas.

## Pendientes antes de implementar

1. Redactar y revisar los documentos de la KB conforme al stack y la estructura aprobados.
2. Seleccionar una cuenta o servicio de email apto para las pruebas; verificar remitente, cuotas, autenticación y conectividad. No solicitar contraseñas en la conversación.
3. Precisar los bordes temporales, zona horaria, reserva exactamente a 24 horas y comportamiento de cambios sobre turnos existentes.
4. Diseñar acceso del odontólogo, gestión de enlaces, reintentos y protección de datos; respetar la gobernanza de aprobación del proyecto para su implementación.
5. Acordar fecha de inicio y aceptación de la entrega local. Siguen pendientes validación con un odontólogo y evolución a un servicio alojado.

## Fuentes técnicas verificadas

Todas consultadas el **2026-11-18**. Se contrastó documentación mediante Context7 y páginas oficiales. Las ramas `main` y las URLs `current` pueden cambiar; no se usaron sus números de versión para fijar dependencias.

| Fuente oficial | URL | Evidencia utilizada |
|---|---|---|
| Eclipse Adoptium — FAQ | https://adoptium.net/docs/faq | Distribución Temurin sin costo bajo GPLv2 con Classpath Exception. |
| Spring Boot — System Requirements | https://docs.spring.io/spring-boot/system-requirements.html | Compatibilidad con Java 21 y herramientas Gradle admitidas. |
| Vite — Getting Started | https://vite.dev/guide/ | Plantillas `react-ts` y `vanilla-ts`, requisitos de Node.js y generación de archivos estáticos. |
| React — License | https://github.com/react/react/blob/main/LICENSE | Licencia MIT del proyecto. |
| Vite — License | https://github.com/vitejs/vite/blob/main/LICENSE | Licencia MIT y avisos de componentes incluidos. |
| PostgreSQL — License | https://www.postgresql.org/about/licence/ | Uso sin cargo de licencia, con sus condiciones. |
| PostgreSQL — Range Types | https://www.postgresql.org/docs/current/rangetypes.html | Rangos y restricciones de exclusión para prevenir solapamientos; la solución concreta aún requiere diseño y pruebas. |
| Spring Boot — Sending Email | https://docs.spring.io/spring-boot/reference/io/email.html | Integración `JavaMailSender`, configuración de SMTP y necesidad de establecer tiempos de espera. |
| Spring Framework — Task Execution and Scheduling | https://docs.spring.io/spring-framework/reference/integration/scheduling.html | Soporte de tareas programadas en la aplicación. |

Las condiciones de un proveedor externo de email no están verificadas porque todavía no se eligió uno. Software sin cargo de licencia no implica operación gratuita de un servicio público.
