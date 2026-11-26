# Discovery — investigación de mercado odontológico

Corte: 18/11/2026. Estado: Discovery de producto aprobado por el usuario el 18/11/2026, con pendientes explícitos. El usuario autorizó el avance a Knowledge Base; la fundación se encuentra en el paso KB.

- [Discovery aprobado](discovery.md): referencia del alcance, reglas, restricciones, riesgos y preguntas abiertas del producto.
- [Stack y estructura de KB aprobados](propuesta-tecnica-kb.md): incorpora la decisión posterior de una primera entrega local para un único consultorio y la aprobación de la combinación técnica y la estructura documental.
- [KB redactada](../knowledge-base/README.md): diez documentos canónicos e índice; estados de aprobación, propuestas y preguntas pendientes.

- [Informe A/B/C/D](informe-mercado.md): comparación de 19 productos, puntuación, análisis y MVP propuesto.
- [Matrices detalladas](matrices-detalladas.md): los 70 aspectos por producto, con identificadores de evidencia.
- [Comparativa completa CSV](comparativa-completa.csv): filtro y análisis en Excel/Sheets.
- [Puntuaciones CSV](puntuaciones.csv): criterios, pesos aplicados y justificación.
- [Fuentes](fuentes.md) y [CSV de fuentes](fuentes.csv): URL, fecha de consulta y forma de acceso.
- [Datos estructurados](relevamiento.json): base de la entrega, incluidos límites y evidencia.
- [Fichas individuales](sources/): formato compatible con discovery-research/web-scraper.

C = declaración comercial; D = documento/dato público; R = reseña moderada; A = anunciado; X = exclusión expresa.
“No evidenciado” no equivale a inexistente. Ninguna capacidad fue probada mediante uso autenticado.
El estudio de mercado conserva las recomendaciones previas a la definición del producto. El alcance aprobado es agenda única por consultorio para odontólogo independiente, reserva web y email, con clínica y cobros posteriores. Para la primera entrega se acordó un único consultorio y ejecución local. El stack aprobado es Java 21 Temurin, Spring Boot, Gradle Wrapper, React con TypeScript y Vite, PostgreSQL local, SMTP mediante Spring Mail y tareas de recordatorios en el backend con estado persistido. Quedan pendientes el proveedor de email y las versiones exactas de las herramientas salvo Java 21. Se priorizan tecnologías y servicios gratuitos cuando sea posible; no se fijó un máximo absoluto de ARS 0.

El documento discovery.md prevalece sobre las hipótesis de MVP del estudio; las decisiones posteriores de entrega y stack se registran en propuesta-tecnica-kb.md. Las aprobaciones no constituyen validación con usuarios ni implementación del sistema.
