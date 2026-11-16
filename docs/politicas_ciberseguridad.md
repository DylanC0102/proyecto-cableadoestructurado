# Políticas de Ciberseguridad - RackScope v2.0.0 SEC-AI

## 1. Control de Frontera de Confianza y Normalización
* **Frontera de Confianza:** Todo dato proveniente del chat del Agente de IA o de la importación de archivos `.json` externos cruza una frontera de confianza y se trata como entrada no confiable hasta ser validado.
* **Normalización:** Antes de cualquier evaluación, las entradas pasan por un proceso de sanitización que elimina etiquetas HTML, caracteres de control y estandariza el texto a minúsculas, además de validar los límites físicos de las unidades del gabinete (1U a 42U).

## 2. Evaluación mediante Motor de Políticas
Cada solicitud dirigida al **Agente de IA (RackScope Copilot)** es interceptada por el **Motor de Políticas** antes de interactuar con el inventario, clasificándose en tres decisiones:
* **ALLOW:** Autoriza consultas de lectura sobre capacidad disponible, ocupación de racks o circuitos pendientes.
* **REVIEW:** Detiene operaciones destructivas o críticas (como la eliminación de equipos activos) y exige aprobación explícita mediante un mecanismo de **Humano en el circuito**.
* **BLOCK:** Rechaza de inmediato peticiones vacías, intentos de inyección de instrucciones o acciones denegadas durante un bloqueo de seguridad.

## 3. Protección contra Prompt Injection (Directa e Indirecta)
* **Prompt Injection Directa:** El sistema inspecciona los comandos ingresados en el asistente para bloquear patrones que intenten anular las reglas del sistema (ej. *"ignora las instrucciones anteriores"*, *"borra todo el inventario"*).
* **Prompt Injection Indirecta:** Al importar respaldos externos en formato JSON, se analizan todos los campos de metadatos (`nombre`, `subtipo`, `circuito`) para neutralizar instrucciones maliciosas ocultas antes de actualizar la aplicación.

## 4. Falla Segura (Fail-Safe)
* Ante errores de sintaxis en archivos JSON, estructuras corruptas, detección de inyección indirecta o activación de alertas críticas por correlación, la aplicación entra en **Modo Falla Segura (Solo Lectura)**: bloquea toda escritura o modificación y conserva intacto el último estado válido del inventario.

## 5. Auditoría, Colector Syslog y Correlación
* **Auditoría y Colector Syslog:** Cada acción o incidente genera un registro estructurado en el **Colector Syslog** local bajo el `Facility: LOCAL4`, incluyendo marca de tiempo, `Event ID` único, `Severidad` (`INFO`, `WARN`, `ERR`, `CRIT`) y `Mnemónico` (ej. `%RACK-IA-ALLOW`, `%RACK-POL-REVIEW`, `%RACK-SEC-INJECT`).
* **Correlación:** Si el colector detecta **3 eventos `BLOCK` (`ERR`) consecutivos**, dispara automáticamente una alerta de severidad `CRIT` (`%RACK-CORR-FAILSAFE`) y activa el **Modo Falla Segura** hasta la intervención del administrador.
