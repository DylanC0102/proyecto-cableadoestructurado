# Inventario de Rack 18U

Sistema de gestion e inventario para gabinetes de telecomunicaciones.

## Estructura
- data/: Archivos de respaldo JSON.
- docs/: Documentacion tecnica del proyecto.
- index.html: Panel de control visual.

## Repositorio Oficial
[Descargar proyecto (.zip)](https://github.com/user-attachments/files/34923690/proyecto-cableadoestructurado-main.zip)
# RackScope - Gestión de Cableado Estructurado e Inventario de Racks (v2.0.0 SEC-AI)

Aplicación web para el control, auditoría física y administración de gabinetes de telecomunicaciones bajo los estándares **ANSI/TIA-606** y **ANSI/TIA-568**, actualizada con controles de ciberseguridad e Inteligencia Artificial.

## Novedades de la Versión 2.0.0 (Ciberseguridad e IA)
* **Agente de IA (RackScope Copilot) con Frontera de Confianza:** Asistente integrado para consultar capacidad de unidades (U) y estado de circuitos con sanitización y **normalización** previa de todas las entradas.
* **Motor de Políticas:** Evalúa cada solicitud antes de tocar el inventario bajo tres decisiones:
  * `ALLOW`: Consultas seguras de solo lectura.
  * `REVIEW`: Operaciones críticas que exigen aprobación explícita mediante **Humano en el circuito**.
  * `BLOCK`: Detección y bloqueo inmediato de comandos destructivos o ataques.
* **Protección contra Prompt Injection (Directa e Indirecta):** Bloquea instrucciones maliciosas tanto en el chat del asistente como ocultas en metadatos de archivos `.json` importados.
* **Colector Syslog y Correlación:** Bitácora de auditoría en tiempo real bajo `Facility: LOCAL4` con `Event ID`, `Severidad` (`INFO`, `WARN`, `ERR`, `CRIT`) y `Mnemónico`. Detecta 3 bloqueos consecutivos para disparar alerta crítica.
* **Modo Falla Segura (Fail-Safe):** Bloquea modificaciones y conserva intacto el inventario ante errores de sintaxis JSON o intentos de intrusión.
