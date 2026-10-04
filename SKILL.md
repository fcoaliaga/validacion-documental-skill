---
name: validacion-documental
description: Valida un documento ya redactado (.docx, .pptx, PDF o texto pegado) con un checklist núcleo más un módulo según su tipo, incluyendo la verificación de que las fuentes existen y respaldan lo afirmado. Úsala siempre que se pida validar, revisar antes de entregar, pasar el checklist, comprobar bibliografía, citas, normas o referencias, o confirmar que un documento está listo, aunque no se use la palabra "checklist". Cubre informes clínicos y de laboratorio, informes de calibración, guías clínicas, documentos académicos, presentaciones docentes, protocolos o procedimientos y tests o exámenes de evaluación, en español. Puede entregar el resultado en el chat o como informe Word (.docx).
---

# Validación documental

## Principio

Comprobar y reportar. No reescribir ni completar contenido salvo petición expresa.
Lo que no se pueda confirmar se marca "no verificable"; nunca se da por válido.
No inventar datos, referencias ni resultados de comprobación.

## Flujo

1. Leer el documento completo, con el método adecuado al formato (ver «Lectura por formato»).
2. Detectar el tipo de documento y cargar su módulo desde `references/`. Si el tipo es ambiguo, preguntar antes de validar.
3. Comprobar si hay búsqueda web disponible. Si no la hay, avisarlo al inicio: la existencia de las fuentes no se podrá confirmar y quedarán como "no verificable".
4. Si el documento remite a otros documentos (guía, PNT, manual, norma) que no se han aportado, listarlos y pedirlos. Mientras tanto, esas referencias quedan "no contrastable".
5. Aplicar el núcleo (A–D) y después el módulo del tipo.
6. Devolver el resultado en el formato descrito abajo.

## Lectura por formato

- **.docx:** `extract-text` para el contenido. Revisar además pies y cabeceras (campos PAGE/NUMPAGES frente a texto fijo), comentarios y control de cambios sin resolver, y metadatos.
- **PDF y presentaciones:** extraer el texto y rasterizar las páginas (skill `pdf-reading`). El texto extraído puede salir truncado o con ligaduras rotas («Pronós)co»); los contenidos en imagen (tablas, esquemas, figuras) se leen sobre el render, no sobre el texto. Indicar qué diapositivas no se pudieron comprobar visualmente.
- **Texto pegado:** validar tal cual y avisar de que no se ve el formato.

## Selección de módulo

| Tipo de documento | Archivo |
|---|---|
| Informe clínico o de laboratorio | references/informe-clinico-laboratorio.md |
| Informe de calibración | references/informe-calibracion.md |
| Guía clínica | references/guia-clinica.md |
| Documento académico (artículo, memoria, TFM, proyecto) | references/documento-academico.md |
| Presentación docente | references/presentacion-docente.md |
| Protocolo o procedimiento | references/protocolo-procedimiento.md |
| Test o examen de evaluación | references/test-evaluacion.md (+ scripts/analizar_test.py) |

Si el documento mezcla tipos, aplicar el módulo del tipo principal y añadir los puntos relevantes del otro.

## Núcleo (siempre)

### A. Consistencia interna
- Cifras, unidades y fechas coinciden entre texto, tablas, figuras y resumen
- Siglas definidas en el primer uso y usadas igual después
- Las conclusiones se derivan de los resultados presentados

### B. Verificación de fuentes (bloque más estricto)

Fuentes = artículos, libros, guías, normas, procedimientos y certificados.

Para cada fuente citada:
1. **Existencia:** localizarla (DOI, PubMed, editorial, organismo emisor).
2. **Metadatos:** autores, año, título, revista o entidad, volumen, páginas y, si es norma o guía, edición.
3. **Respaldo:** la fuente dice realmente lo que el documento le atribuye.
4. **Vigencia:** no retractada; norma o guía en su versión actual.

Estados: **Verificada** | **Parcial** (existe, pero metadatos o respaldo no coinciden, o no se pudo leer el contenido) | **No localizada** | **No verificable** (sin acceso a búsqueda o a la fuente) | **No contrastable** (el documento remite a otro documento no aportado).

Reglas:
- Si hay bibliografía, ninguna cifra, observación o conclusión puede apoyarse en una fuente "No localizada" o "No verificable" sin quedar señalada.
- Si no hay bibliografía pero hay afirmaciones que requieren respaldo, listarlas como "sin fuente".
- Cruce en ambos sentidos: toda cita del texto está en la bibliografía y toda entrada de la bibliografía se cita en el texto.
- Nunca corregir ni completar una referencia de memoria. Proponer la corrección solo si se localizó la fuente real.
- Una fuente solo se marca "Verificada" si se comprobaron existencia y metadatos. El respaldo se declara aparte; si no se pudo leer la fuente, queda "no verificable" en ese punto.

### C. Formato
- Estilos, tipografía y márgenes uniformes
- Tablas y figuras con título, numeración y unidades
- El archivo abre bien y está en el formato pedido

### D. Cierre
- Sin marcas pendientes (comentarios, control de cambios sin resolver, "TODO", texto de relleno)
- Sin datos identificativos o sensibles que no deban figurar

## Calibración de las afirmaciones

- Distinguir lo que dice la fuente de lo que se infiere. Si la fuente solo recoge algo parcialmente, decirlo («recoge», «coincide con matiz»), sin elevarlo a «confirma» o «aprobó».
- Citar cada fuente por título, organismo y enlace. Parafrasear; no copiar pasajes.
- Preferir fuentes primarias o institucionales (organismos de salud, fabricante, guías, revistas con revisión). Si solo hay una fuente secundaria, indicarlo.
- Un procedimiento de otro centro o una ficha comercial no sustituyen el manual, el PNT ni la versión de software del servicio evaluado; avisarlo cuando se usen como referencia.
- Una cifra o unidad que parece un error pero no se pudo contrastar va como «sospecha sin verificar», nunca como error confirmado.

## Formato de salida

1. **Veredicto:** apto / apto con correcciones menores / no apto, con una línea de motivo. Es una valoración, no una norma; decirlo.
2. **Bloqueantes:** lista breve, primero lo que impide la entrega.
3. **Tabla de comprobación:** punto | estado (✔ / ✘ / no verificable / no contrastable) | ubicación | **base** | acción propuesta.
4. **Tabla de fuentes:** referencia | estado | observación.
5. **Limitaciones de la validación:** qué no se pudo comprobar y por qué.

Indicar la **base** de cada hallazgo, para que se distinga lo comprobado de lo sospechado:
- **Fuente externa:** contrastado con una fuente primaria localizada (citarla).
- **Coherencia interna:** el documento se contradice (decir qué dos pasajes).
- **Sospecha sin verificar:** probable error que no se pudo confirmar.
- **No contrastable:** depende de un documento que no se aportó.
- **Análisis objetivo:** resultado de un cálculo sobre el propio documento (p. ej. la clave de un test).

No presentar una sospecha como error confirmado. Mantener un tono profesional y objetivo; señalar los problemas sin suavizarlos.

## Entrega del informe

- **Por defecto, en el chat**, con el formato de salida anterior.
- **Si se pide como documento Word (o «documento de texto»)**, generar un .docx: leer antes la skill `docx` y usar `scripts/generar_informe.js`, que construye el informe a partir de un JSON (esquema en la cabecera del script). Estructura recomendada:
  1. Datos de la validación (fecha, documentos, método, alcance de los veredictos)
  2. Resumen: tabla documento | módulo | veredicto | bloqueantes, y leyenda de las bases
  3. Un apartado por documento: veredicto, bloqueantes, sospechas, inconsistencias internas, datos sin fuente, verificación de fuentes, menores y limitaciones
  4. Fuentes consultadas, con enlaces
- Escribir los géneros y especies en cursiva (`*Brucella*`) y los textos en español.
- Tras generarlo, convertir a PDF y revisar visualmente las páginas (tablas que se cortan, filas sueltas, pie de página) antes de entregarlo.
- Si el formato pedido es ambiguo, entregar el resultado en el chat y ofrecer el Word en una línea.

## Límites

- Terminología científica y nomenclatura: delegar en la skill `revision-terminologica-cientifica`; no duplicar sus criterios.
- Esta validación no sustituye la validación normativa, la revisión por pares ni la aprobación del responsable de calidad.
- No modificar el documento original. Si se piden correcciones, trabajar sobre una copia y avisarlo.
