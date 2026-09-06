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

1. **Trigger:** Gmail detecta un email nuevo en la bandeja de soporte (modo *From now on*, sin reprocesar histórico).
2. **IA:** Make AI Toolkit (Categorize text) analiza sentimiento (Positivo/Negativo/Neutro) y tipo de consulta (Reclamo/Consulta) en un solo llamado.
3. **Memoria:** El resultado se escribe en Airtable (tabla *Tickets*) con Estado = "Procesado por IA" y Aprobado = FALSE.
4. **Router:** Notifica al equipo interno vía Slack en el canal correspondiente (reclamos-urgentes / consultas-generales / clientes-satisfechos) — nunca al cliente.
5. **HITL (obligatorio):** El sistema se detiene. Un humano revisa el ticket en Airtable, edita la respuesta si hace falta, y marca el checkbox "Aprobado".
6. **Salida:** Solo si Aprobado = TRUE, un segundo trigger dispara la respuesta al cliente por Gmail (con Thread-ID mapeado) y, si corresponde, genera un cupón de fidelización.
7. **Resiliencia:**
