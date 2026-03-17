# Tareas progresivas para el desarrollo de la app SFZ

## Fase 0 — Preparación
- [ ] Revisar y validar requisitos del documento `AGENTS.rtf`.
- [ ] Definir estructura del proyecto estático para GitHub Pages (`index.html`, `styles.css` opcional, `app.js` opcional).
- [ ] Confirmar que no se utilizará backend ni Node.js.

## Fase 1 — Base de interfaz (MVP visual)
- [ ] Crear `index.html` con:
  - [ ] Input de carpeta: `<input type="file" webkitdirectory multiple>`.
  - [ ] Botón **Generate SFZ**.
  - [ ] Área de errores/mensajes.
  - [ ] Vista previa en `<pre>`.
  - [ ] Botón de descarga de `instrument.sfz` (inicialmente deshabilitado).
- [ ] Añadir estilos mínimos para legibilidad y estados de error/éxito.

## Fase 2 — Ingesta y normalización de archivos
- [ ] Capturar lista de archivos seleccionados preservando `webkitRelativePath`.
- [ ] Filtrar solo archivos `.wav` (case-insensitive).
- [ ] Detectar articulaciones dinámicamente a partir de carpetas de primer nivel.
- [ ] Ignorar articulaciones vacías sin romper flujo.

## Fase 3 — Detección de nota y conversión MIDI
- [ ] Implementar función `extractNoteName(filename)`:
  - [ ] Detectar patrones como `C3`, `D#4`, `A#5`.
  - [ ] Parsing case-insensitive.
- [ ] Implementar función `noteToMidi(noteName)`:
  - [ ] Soporte para sostenidos `#`.
  - [ ] Validar rango MIDI permitido.
- [ ] Ignorar archivos sin nota válida y registrar advertencias.

## Fase 4 — Construcción de regiones por articulación
- [ ] Ordenar muestras por MIDI ascendente dentro de cada articulación.
- [ ] Implementar `buildRegions(samples)` con cálculo automático de `lokey/hikey`:
  - [ ] Primera muestra define límite inferior.
  - [ ] Última muestra define límite superior.
  - [ ] Muestras intermedias usan midpoint entre notas vecinas.
- [ ] Asignar `pitch_keycenter` = MIDI original de la muestra.

## Fase 5 — Keyswitches y grupos
- [ ] Asignar keyswitches por orden de articulación desde MIDI 36 (C2).
- [ ] Implementar en cada grupo:
  - [ ] `sw_lokey=36`
  - [ ] `sw_hikey=48`
  - [ ] `sw_last=<nota asignada>`
- [ ] Verificar que cada articulación tenga un keyswitch único.

## Fase 6 — Generación de SFZ
- [ ] Implementar `generateSfz(groups)`:
  - [ ] Crear bloques `<group>` con parámetros de keyswitch.
  - [ ] Crear bloques `<region>` por muestra.
- [ ] Generar rutas relativas limpias compatibles con espacios y UTF-8.
- [ ] Verificar que el SFZ funcione con archivo `instrument.sfz` en la carpeta raíz.

## Fase 7 — Integración UI + descarga
- [ ] Mostrar SFZ generado en `<pre>`.
- [ ] Habilitar botón de descarga al generar contenido válido.
- [ ] Exportar archivo como `instrument.sfz`.
- [ ] Mostrar errores claros si no hay muestras válidas.

## Fase 8 — Robustez y casos borde
- [ ] Probar carpetas vacías y archivos sin nota.
- [ ] Probar nombres con mayúsculas/minúsculas mixtas.
- [ ] Probar rutas con espacios y caracteres UTF-8.
- [ ] Confirmar que no hay crashes en entradas incompletas.

## Fase 9 — Opcionales de valor
- [ ] Mostrar en UI lista de articulaciones detectadas.
- [ ] Permitir reordenar articulaciones para cambiar orden de keyswitches.

## Fase 10 — Validación final y entrega
- [ ] Revisión manual del SFZ en Sforzando.
- [ ] Validar continuidad de rango tonal (sin huecos extraños).
- [ ] Limpieza de código y comentarios finales.
- [ ] Verificar despliegue en GitHub Pages.

---

## Definición de terminado (DoD)
- [ ] La app corre como sitio estático (sin backend).
- [ ] Genera `instrument.sfz` válido y descargable.
- [ ] Detecta articulaciones sin depender de nombres predefinidos.
- [ ] Calcula `lokey/hikey` automáticamente por regiones.
- [ ] Implementa keyswitches desde C2 (MIDI 36).
- [ ] Maneja errores y casos borde sin fallar.
