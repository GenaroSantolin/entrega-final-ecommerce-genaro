# Entrega Final — Ecosistema de Automatización IA Autónomo (E-commerce)

**Curso:** AI Automation — Coderhouse
**Alumno:** Genaro Santolin
**Caso de uso:** Agente de Atención al Cliente para e-commerce — análisis de sentimiento y tipo de consulta, con validación humana (HITL) antes de responder o emitir cualquier cupón.

---

## 📂 Contenido de este repositorio

| Archivo | Descripción |
|---|---|
| `Entrega-Final-Ecommerce-Documentacion.pdf` | Documento con los 4 entregables: diagrama de arquitectura, manual de datos, matriz de costos y documentación de seguridad/resiliencia |
| `blueprint-ecosistema-ecommerce.json` | Blueprint del escenario de Make, incluyendo la lógica del HITL |
| `capturas/` | Screenshots de evidencia del flujo funcionando en Make |
| `README.md` | Este archivo |

## 🔗 Enlaces obligatorios

- **Dashboard de Control (Airtable Shared View):** https://airtable.com/apprtdTzVLCtORMNr/shrsYhmZKGKtR9MLB
- **Base de datos en modo lectura:** https://airtable.com/apprtdTzVLCtORMNr/shrsYhmZKGKtR9MLB
- **Video demo:** https://drive.google.com/file/d/1m8iN5c986nVWOzAOsO2dFBJXRdZIQz9u/view?usp=sharing

## 🧩 Resumen del sistema

1. **Trigger:** Google Sheets detecta una fila nueva (nuevo ticket de atención al cliente) en la hoja "Tickets".
2. **IA:** Make AI Toolkit (Categorize text) analiza sentimiento y tipo de consulta en un solo llamado, clasificando en Negativo_Reclamo / Neutro_Consulta / Positivo.
3. **Memoria:** El resultado se escribe en Airtable (tabla *Tickets*) con Estado = "Procesado por IA" y Aprobado = FALSE.
4. **Router:** Divide el flujo en 3 rutas mutuamente excluyentes, registrando cada una en la pestaña de Google Sheets correspondiente (Reclamos_Urgentes / Consultas_Generales / Clientes_Satisfechos).
5. **HITL (obligatorio):** El sistema se detiene. Un humano revisa el ticket en Airtable, edita la respuesta si hace falta, y marca el checkbox "Aprobado".
6. **Salida multicanal:** Solo si Aprobado = TRUE, un segundo trigger dispara la respuesta al cliente por Gmail (con Thread-ID mapeado) y, si corresponde, genera un cupón de fidelización.
7. **Resiliencia:** Error Handler (Retry automático con 3 reintentos) sobre el nodo de IA; los fallos se registran en la tabla *Registro de Errores*.

**Nota de diseño:** el diagrama de arquitectura (sección 1 del PDF) documenta la versión conceptual del sistema con Gmail como trigger de entrada, pensada para producción sobre una bandeja real de soporte. La implementación funcional demostrada en el video y las capturas usa Google Sheets como trigger, replicando exactamente la misma lógica de clasificación, ruteo, HITL y resiliencia — la migración de Sheets a Gmail solo requiere reemplazar el módulo de trigger, sin alterar el resto del flujo.

## ✅ Criterios de evaluación cubiertos

- [x] Mapa de arquitectura (20%) — ver PDF, sección 1
- [x] Estructuras de datos documentadas (20%) — ver sección 2 (esquema Airtable + JSONs)
- [x] Optimización de costos (20%) — ver sección 3 (matriz de modelos por tarea)
- [x] Seguridad y resiliencia (20%) — ver sección 4 (minimización de datos, error handlers, HITL)
- [x] Dashboard de control (20%) — link público arriba + instrucciones de configuración en sección 5

## 🎥 Video demo

**Link:** https://drive.google.com/file/d/1m8iN5c986nVWOzAOsO2dFBJXRdZIQz9u/view?usp=sharing

El video muestra el flujo real funcionando en Make (Google Sheets → Make AI Toolkit → Router → Google Sheets), incluyendo:
1. Carga de nuevos tickets en Google Sheets (trigger)
2. Ejecución del escenario en Make (IA clasificando en tiempo real)
3. Resultado reflejado en las hojas de salida según categoría
4. Vista del Dashboard de control en Airtable
