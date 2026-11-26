# Discovery — SaaS de agenda odontológica

**Proyecto**: odontologia (identificador del repositorio; nombre comercial por definir).
**Fecha de aprobación**: 2026-11-18.
**Estado**: aprobado por el usuario, con decisiones pendientes explícitas.
**Confirmación**: «no existe presupuesto minimo, todo con tegnologias gratuitas en lo posible, queda todo OK».

Este documento es la referencia del producto acordado. El [estudio de mercado](informe-mercado.md) y sus recomendaciones previas son antecedentes de investigación; el alcance aprobado aquí prevalece para la futura Knowledge Base y el roadmap. La aprobación de Discovery no significa que el producto esté implementado ni validado en un consultorio.

## 1. Problema que resuelve

Reducir el trabajo de reservar, confirmar y reprogramar turnos de un odontólogo independiente en Argentina mediante un SaaS comercial de agenda única por consultorio.

El resultado buscado es que el paciente pueda gestionar su turno desde un enlace web y que el odontólogo mantenga su disponibilidad en un solo lugar. El problema y la solución son una definición del usuario; su magnitud, disposición a pagar y adecuación al trabajo real aún no se contrastaron con odontólogos.

## 2. Usuarios / roles

- **Odontólogo**: único administrador interno de su agenda. Configura motivos de consulta y duración, disponibilidad semanal, pausas y bloqueos; consulta y gestiona los turnos y los datos básicos de sus pacientes.
- **Paciente**: consulta disponibilidad, reserva y recibe los enlaces para gestionar su turno por email. No crea una cuenta ni verifica previamente su email.

Cada consultorio cuenta con su propia agenda independiente. El alcance inicial corresponde a un odontólogo por consultorio, sin recepción con acceso propio. La identidad técnica, el alta comercial y la gestión operativa de los consultorios dentro del SaaS se definirán durante las siguientes fases; no se agrega un rol funcional de recepción o una administración de cadenas al MVP.

## 3. Casos de uso

1. Como odontólogo, quiero configurar los motivos de consulta y su duración para que la disponibilidad contemple el tiempo necesario de cada turno.
2. Como odontólogo, quiero establecer mi horario semanal con pausas, vacaciones y excepciones para ofrecer solo horarios disponibles.
3. Como paciente, quiero elegir un motivo de consulta y un horario libre, ingresar mis datos y recibir una confirmación automática para reservar sin coordinación manual.
4. Como paciente, quiero cancelar o reprogramar desde los enlaces enviados por email, dentro del plazo permitido, para gestionar mi turno sin llamadas.
5. Como odontólogo, quiero consultar mi agenda y los datos básicos asociados a cada turno para organizar mi trabajo y contactar al paciente cuando sea necesario.

Los emails de confirmación, cambios y recordatorio apoyan estos flujos. La forma exacta de gestionar excepciones manuales y fallas de comunicación sigue pendiente.

## 4. Competidores / soluciones existentes

Se realizó un relevamiento documental de **19 productos** y **112 fuentes registradas**, con fecha de consulta 2026-11-18. Incluye Bilog, Dentalink, DrApp, DentalSoft, DentalTec, Dentidad, Doctoralia Pro Argentina, AgendaPro, OdontoSoft Millennium, Odontosys, Dentify, Odonthia, Doctocliq, DENTIDESK, Clinic Cloud, Clinicorp, Simples Dental, Open Dental y CareStack.

- [Informe comparativo A–D](informe-mercado.md).
- [Matrices detalladas](matrices-detalladas.md) y [comparativa CSV](comparativa-completa.csv).
- [Registro de fuentes, URLs, fechas y forma de acceso](fuentes.md).
- [Fichas individuales](sources/).

Las funciones declaradas, la documentación y las reseñas se diferencian en las matrices. No se ejecutaron demos ni pruebas autenticadas. “No evidenciado” no significa inexistente. Las puntuaciones son valoraciones editoriales de la muestra, no mediciones de rendimiento.

El estudio propuso demos de Bilog, Dentalink, DrApp, DentalSoft y DentalTec, y referencias de experiencia del paciente en Doctoralia, DentalSoft y Simples Dental. Esas propuestas no acreditan validación real ni son un requisito de contratar esos productos.

Para el segmento elegido, la hipótesis de diferenciación es reducir la coordinación manual con un flujo de reserva y autogestión sencillo. La lista de espera, la clínica y los cobros sugeridos en el estudio no pasan automáticamente a requisitos de esta versión.

## 5. Funcionalidades necesarias

- Agenda única e independiente por consultorio, administrada únicamente por su odontólogo.
- Consulta de agenda diaria y semanal, con datos básicos de pacientes y turnos.
- Motivos de consulta con duraciones configuradas por el odontólogo.
- Disponibilidad semanal habitual con pausas y bloqueos por vacaciones o excepciones.
- Cálculo de horarios libres según duración y disponibilidad.
- Prevención de reservas superpuestas, incluso al solicitar el mismo horario simultáneamente.
- Enlace web público para reservar sin cuenta de paciente ni verificación previa del email.
- Formulario obligatorio con **nombre, apellido, email, teléfono y DNI**.
- Confirmación automática de la reserva al obtener un horario libre.
- Enlaces privados enviados por email para cancelar y reprogramar dentro del plazo permitido.
- Emails de confirmación inmediata, aviso de cambios y un recordatorio 24 horas antes.
- Acceso del odontólogo a su agenda y protección de los datos de pacientes, con el diseño de controles correspondiente en las siguientes fases.

No se define todavía framework frontend, base de datos, proveedor de email, alojamiento ni librerías. Tampoco se atribuyen garantías de entrega de correo a un proveedor no seleccionado.

## 6. Funcionalidades opcionales y alcance posterior

Quedan para etapas posteriores:

- Gestión clínica: historia clínica, anamnesis, odontograma, periodontograma, imágenes y tratamientos.
- Cobros a pacientes, señas, facturación e integraciones de pago.
- WhatsApp como canal de recordatorios o asistente conversacional.
- Acceso propio para recepción, múltiples profesionales dentro de un consultorio y varias sedes.

La lista de espera, campañas, reportes avanzados y sincronización con calendarios externos no son requisitos confirmados de la primera versión. Su prioridad futura sigue abierta.

La tarifa y el mecanismo comercial de suscripción del SaaS están pendientes. Diferenciar este punto de los cobros a pacientes, que sí se pospusieron.

## 7. Reglas de negocio

| Regla | Definición aprobada |
|---|---|
| Agenda | Una agenda por consultorio, con un odontólogo como administrador interno. |
| Duración | Depende del motivo de consulta, según configuración del odontólogo. |
| Disponibilidad | Horario semanal habitual con pausas y bloqueos por vacaciones o excepciones. |
| Ventana de reserva | El paciente puede reservar desde 24 horas antes y hasta 30 días hacia adelante. |
| Confirmación | Automática al reservar un horario disponible. |
| Cancelación autónoma | Permitida hasta 24 horas antes del turno. |
| Reprogramación autónoma | Permitida hasta 24 horas antes; el nuevo horario debe ser válido y estar libre. |
| Acceso del paciente | Sin cuenta ni verificación previa del email; recibe enlaces por correo. |
| Datos obligatorios | Nombre, apellido, email, teléfono y DNI. |
| Superposiciones | El sistema impide reservas superpuestas. |
| Correos | Confirmación inmediata, avisos de cambios y un recordatorio 24 horas antes. |

Tras el límite de autogestión se prevé contacto directo con el odontólogo; el procedimiento operativo y los cambios manuales que este podrá realizar se detallarán después.

Quedan por precisar la inclusión exacta de los límites temporales, la zona horaria, el tratamiento de turnos ya reservados al modificar duraciones o bloquear horarios y la reprogramación simultánea. No se deciden estos bordes de manera implícita.

## 8. Integraciones

La única integración externa funcional confirmada para el MVP es el **envío de email**:

- Confirmación inmediata de reserva.
- Avisos de cambios.
- Recordatorio 24 horas antes.
- Enlaces para gestionar el turno.

Proveedor, modalidad de envío, cuotas, dominio/remitente y tratamiento de fallas: pendientes de selección y verificación.

No se requieren en esta versión WhatsApp, Mercado Pago, ARCA, radiología ni integración con una historia clínica. Google Calendar y otros calendarios no están confirmados.

## 9. Restricciones

- **Objetivo**: SaaS comercial para vender a consultorios odontológicos.
- **Mercado inicial**: odontólogos independientes en Argentina.
- **Plazo solicitado**: una semana para la primera versión funcional. Fecha exacta de inicio y entrega pendiente; no se infiere un calendario.
- **Presupuesto**: no existe presupuesto mínimo definido. Se priorizarán tecnologías y servicios gratuitos cuando sea posible. Esto no fija un máximo absoluto de ARS 0, no garantiza operación gratuita y no autoriza contratar servicios pagos.
- **Tecnologías obligatorias**: Spring Boot con Gradle y TypeScript.
- **Resto del stack**: por definir; no se elige implícitamente lenguaje JVM, framework frontend, persistencia, versiones ni infraestructura.
- **Validación**: todavía no hay un odontólogo disponible para entrevistas o piloto.
- **Datos**: incluir DNI, teléfono y email según decisión del usuario; definir protección, accesos, conservación y operación antes del uso real.

La semana se entiende como objetivo de una primera versión funcional. Su aptitud para operación comercial con datos reales debe verificarse; no se afirma que el plazo garantice un servicio listo para comercializar.

## 10. Riesgos y supuestos

- **Demanda sin validar**: falta acceso a un odontólogo que contraste los flujos, el problema prioritario y la disposición a pagar.
- **Plazo ajustado**: una semana limita el alcance y deja poco margen para validación y puesta en operación.
- **Email sin verificación previa**: un error o el uso de una dirección ajena puede impedir que el paciente correcto reciba sus enlaces. La elección de no verificar previamente se conserva.
- **Entrega de correo**: fallas, demoras o filtrado pueden impedir los avisos; no existe proveedor ni mecanismo de recuperación definidos.
- **Coincidencia a 24 horas**: una reserva en el límite mínimo coincide con el momento del recordatorio. Debe definirse cómo evitar mensajes duplicados.
- **Disponibilidad simultánea**: la consulta de un hueco libre no debe permitir dos reservas superpuestas al confirmarse.
- **Cambios posteriores**: modificar horarios, bloqueos o duraciones puede afectar turnos existentes; falta definir el procedimiento.
- **Gratuidad operativa**: las opciones gratuitas pueden tener cuotas o condiciones incompatibles con el uso previsto. Deben comprobarse al elegir proveedores.
- **Datos personales**: los datos obligatorios y los enlaces requieren un tratamiento cuidadoso; los controles y la operación se diseñarán antes de usar información real.
- **Resultado esperado como hipótesis**: se busca reducir trabajo administrativo, pero todavía no hay una línea de base ni una mejora medida.

Los riesgos se registran para orientar decisiones posteriores, no como evidencia de fallas ya ocurridas o de demanda demostrada.

## 11. Preguntas abiertas

1. Nombre comercial, tarifa y mecanismo de suscripción del SaaS.
2. Fecha exacta de inicio y entrega de la semana de implementación, y criterio de aceptación de esa primera versión.
3. Proveedor de email, cuotas gratuitas, dominio/remitente y recuperación ante fallas; efectos de un email incorrecto.
4. Base de datos, lenguaje JVM, framework frontend, versiones, alojamiento y demás decisiones del stack.
5. Límites temporales inclusivos, zona horaria, vencimiento de enlaces y tratamiento de un enlace reenviado o perdido.
6. Comportamiento del recordatorio para reservas realizadas exactamente 24 horas antes.
7. Cambio de disponibilidad o duración cuando ya existen turnos, y excepciones gestionadas directamente por el odontólogo.
8. Reglas de identificación de pacientes, duplicados y corrección de DNI, teléfono y email.
9. Medidas de protección, conservación, respaldo y recuperación para la puesta en operación.
10. Acceso a un odontólogo para validar, plan de piloto y métricas de reducción de trabajo.
11. Priorización posterior de clínica, cobros, WhatsApp, lista de espera y otras extensiones.

Estas preguntas se aprobaron como pendientes explícitos. La siguiente fase de fundación es la creación de la Knowledge Base a partir de este documento. El avance de fase se decide por separado.

