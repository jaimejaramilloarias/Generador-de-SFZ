# Tareas progresivas para el desarrollo de la app SFZ

## Fase 0 — Preparación
- [x] Revisar y validar requisitos del documento `AGENTS.rtf`.
- [x] Definir estructura del proyecto estático para GitHub Pages (`index.html`, `styles.css` opcional, `app.js` opcional).
- [x] Confirmar que no se utilizará backend ni Node.js.

## Fase 1 — Base de interfaz (MVP visual)
- [x] Crear `index.html` con:
  - [x] Input de carpeta: `<input type="file" webkitdirectory multiple>`.
  - [x] Botón **Generate SFZ**.
  - [x] Área de errores/mensajes.
  - [x] Vista previa en `<pre>`.
  - [x] Botón de descarga de `instrument.sfz` (inicialmente deshabilitado).
- [x] Añadir estilos mínimos para legibilidad y estados de error/éxito.

## Fase 2 — Ingesta y normalización de archivos
- [x] Capturar lista de archivos seleccionados preservando `webkitRelativePath`.
- [x] Filtrar solo archivos `.wav` (case-insensitive).
- [x] Detectar articulaciones dinámicamente a partir de carpetas de primer nivel.
- [x] Ignorar articulaciones vacías sin romper flujo.

## Fase 3 — Detección de nota y conversión MIDI
- [x] Implementar función `extractNoteName(filename)`:
  - [x] Detectar patrones como `C3`, `D#4`, `A#5`.
  - [x] Parsing case-insensitive.
- [x] Implementar función `noteToMidi(noteName)`:
  - [x] Soporte para sostenidos `#`.
  - [x] Validar rango MIDI permitido.
- [x] Ignorar archivos sin nota válida y registrar advertencias.

## Fase 4 — Construcción de regiones por articulación
- [x] Ordenar muestras por MIDI ascendente dentro de cada articulación.
- [x] Implementar `buildRegions(samples)` con cálculo automático de `lokey/hikey`:
  - [x] Primera muestra define límite inferior.
  - [x] Última muestra define límite superior.
  - [x] Muestras intermedias usan midpoint entre notas vecinas.
- [x] Asignar `pitch_keycenter` = MIDI original de la muestra.

## Fase 5 — Keyswitches y grupos
- [x] Asignar keyswitches por orden de articulación desde MIDI 36 (C2).
- [x] Implementar en cada grupo:
  - [x] `sw_lokey=36`
  - [x] `sw_hikey=48`
  - [x] `sw_last=<nota asignada>`
- [x] Verificar que cada articulación tenga un keyswitch único.

## Fase 6 — Generación de SFZ
- [x] Implementar `generateSfz(groups)`:
  - [x] Crear bloques `<group>` con parámetros de keyswitch.
  - [x] Crear bloques `<region>` por muestra.
- [x] Generar rutas relativas limpias compatibles con espacios y UTF-8.
- [x] Verificar que el SFZ funcione con archivo `instrument.sfz` en la carpeta raíz.

## Fase 7 — Integración UI + descarga
- [x] Mostrar SFZ generado en `<pre>`.
- [x] Habilitar botón de descarga al generar contenido válido.
- [x] Exportar archivo como `instrument.sfz`.
- [x] Mostrar errores claros si no hay muestras válidas.

## Fase 8 — Robustez y casos borde
- [x] Probar carpetas vacías y archivos sin nota.
- [x] Probar nombres con mayúsculas/minúsculas mixtas.
- [x] Probar rutas con espacios y caracteres UTF-8.
- [x] Confirmar que no hay crashes en entradas incompletas.

## Fase 9 — Opcionales de valor
- [x] Mostrar en UI lista de articulaciones detectadas.
- [ ] Permitir reordenar articulaciones para cambiar orden de keyswitches.

## Fase 10 — Validación final y entrega
- [ ] Revisión manual del SFZ en Sforzando.
- [x] Validar continuidad de rango tonal (sin huecos extraños).
- [x] Limpieza de código y comentarios finales.
- [ ] Verificar despliegue en GitHub Pages.

---

## Definición de terminado (DoD)
- [x] La app corre como sitio estático (sin backend).
- [x] Genera `instrument.sfz` válido y descargable.
- [x] Detecta articulaciones sin depender de nombres predefinidos.
- [x] Calcula `lokey/hikey` automáticamente por regiones.
- [x] Implementa keyswitches desde C2 (MIDI 36).
- [x] Maneja errores y casos borde sin fallar.
