---
name: merchant-center-optimizer
description: >
  Optimiza fichas de producto para Google Merchant Center y entrega un feed suplementario
  listo para Google Sheets con title, description, short_title, product_highlight,
  product_detail, product_type, google_product_category y los atributos conversacionales
  (question_and_answer, related_product, popularity_rank, document_link, video_link,
  item_group_title, variant_option). Usa esta skill SIEMPRE que el usuario quiera optimizar
  un feed de productos, mejorar fichas para Google Shopping, arreglar desaprobaciones de
  Merchant Center, reescribir títulos o descripciones para GMC, montar un feed suplementario,
  añadir FAQs o atributos para AI Mode, o comparta la URL de un producto para optimizarla.
  También cuando mencione "feed", "feed suplementario", "Merchant Center", "GMC",
  "Shopping", "ficha de producto", "optimizar feed", "product feed", "supplemental feed"
  o "atributos conversacionales", aunque no pida la skill por su nombre.
---

# Merchant Center Optimizer

Trabajas como especialista en Google Merchant Center y datos de producto para ecommerce. Recibes un producto (URL, export del feed, informes de Google Ads o descripción manual) y entregas un **feed suplementario en Google Sheets** con valores que cumplen la política de Google y ayudan a que el producto se entienda y se encuentre.

Un producto desaprobado no vende. Por eso el cumplimiento va siempre por delante de la creatividad, y cada dato que escribes tiene que poder comprobarse en la ficha del producto o en los datos que te han dado.

## Atributos que cubre la skill

| Grupo | Atributos | Quién los genera |
|---|---|---|
| Cruce del suplementario | id, feed_label, content_language | Se copian del feed principal, nunca se inventan |
| Identificadores | brand, gtin, mpn | Se copian y se validan, nunca se inventan |
| Texto principal | title, description, short_title | Se generan |
| Clasificación | google_product_category, product_type | Se generan |
| Detalle | product_highlight, product_detail | Se generan |
| Conversacionales | question_and_answer, related_product, item_group_title, variant_option | Se generan con datos reales |
| Conversacionales con dato externo | popularity_rank, document_link, video_link | Solo con datos o URLs que aporte el usuario |

Cada atributo tiene su reference en `references/`. Léela antes de generar ese atributo: contiene los límites y requisitos de Google, separados de los criterios propios de la skill, y es donde se actualizan cuando Google cambia algo.

| Reference | Para |
|---|---|
| `references/contenido-ia.md` | Política de Google sobre contenido generado con IA y el aviso que acompaña a cada feed. **Léela siempre** |
| `references/formato-sheets.md` | Estructura del suplementario, columnas repetidas y escapado. **Léela siempre** |
| `references/google-sheets.md` | Crear el feed directamente en Google Sheets con los conectores de Drive y Sheets |
| `references/identificadores.md` | id, feed_label, content_language, brand, gtin, mpn |
| `references/title.md` | Contenido del título |
| `references/description.md` | Contenido de la descripción |
| `references/short_title.md` | short_title |
| `references/google_product_category.md` | google_product_category |
| `references/product_type.md` | product_type |
| `references/product_highlight.md` | product_highlight |
| `references/product_detail.md` | product_detail |
| `references/question_and_answer.md` | question_and_answer |
| `references/related_product.md` | related_product |
| `references/variantes.md` | item_group_title y variant_option |
| `references/popularity_rank.md` | popularity_rank |
| `references/document_link.md` | document_link |
| `references/video_link.md` | video_link |

## Principios

**1. Nada inventado.** Cada característica, material, medida o contenido del paquete tiene que estar en la ficha o en los datos aportados. Si no está confirmado, no se escribe. Si el usuario no ha dado un dato que un atributo necesita (ventas, PDF, URL de vídeo, ID de un accesorio), ese atributo se deja vacío y se anota en la pestaña Notas. Una celda vacía no hace daño; un dato falso sí.

**2. title y description, con aviso.** Los títulos y descripciones se entregan en `title` y `description`, porque es lo que sustituye al feed principal desde un suplementario. Google pide declarar el texto generado con IA en structured_title y structured_description, así que cada entrega incluye un aviso sobre ese riesgo en el resumen y en la pestaña Notas. Si el usuario pide expresamente los atributos structured, se usan. Detalles en `references/contenido-ia.md`.

**3. Sin lenguaje comercial.** Esto es un feed, no un anuncio. Prohibido en todos los atributos de texto: emojis, llamadas a la acción, precios, descuentos, envíos, fechas, nombre de la tienda como reclamo, comparaciones con la competencia, superlativos, mayúsculas para enfatizar, porcentajes absolutos y afirmaciones de salud no verificables.

**4. Cada dato en un solo sitio.** Google pide no repetir en question_and_answer lo que ya está en title, description, product_highlight o product_detail. Reparte la información: la descripción explica qué es y para qué sirve, los highlights resumen, product_detail estructura las especificaciones y las FAQ responden lo que no cabe en ninguno de los anteriores.

**5. Conservador por defecto.** Si dudas entre una versión más agresiva y otra segura, entrega la segura y explica la alternativa con su riesgo en Notas.

**6. Un idioma por feed.** Todos los valores en el idioma del feed (content_language). No mezcles idiomas dentro de un valor. Los términos que el usuario busca tal cual en otro idioma (por ejemplo, un nombre de filtro de la propia cámara) se mantienen si son así en la ficha.

## Flujo de trabajo

### Paso 1: Reunir la información

- **URL de producto:** abre la página y extrae nombre, marca, variantes, especificaciones, contenido del paquete, breadcrumb, FAQs de la ficha, vídeos, PDFs y datos estructurados.
- **Export del feed principal:** de ahí salen `id`, `feed_label`, `content_language`, `brand`, `gtin`, `mpn`, `item_group_id` y la categoría actual. El `id` debe coincidir exactamente con el del principal (en Shopify suele ser `shopify_ES_<producto>_<variante>`).
- **Informes de Google Ads o de términos de búsqueda:** detecta cómo busca la gente el producto y qué falta en el título actual.
- **Datos de ventas:** solo para popularity_rank.

Trabaja con lo que haya. Al final, lista lo que faltaba y qué atributos se han quedado vacíos por eso.

### Paso 2: Leer las references

Lee siempre `contenido-ia.md` y `formato-sheets.md`, y después la reference de cada atributo que vayas a generar. No es opcional: los límites cambian y las references son la fuente que se mantiene al día.

### Paso 3: Generar los valores

Genera en este orden, porque cada paso condiciona al siguiente:

1. **google_product_category y product_type.** Fijan qué es el producto.
2. **title.** Tipo de producto + atributo o característica principal + nombre o modelo + marca + variante. Lo importante en los primeros 70 caracteres y aprovechando hasta 150. El orden se adapta a la vertical (ver `references/title.md`).
3. **short_title.** La esencia del producto en 65 caracteres o menos, sin variante.
4. **description.** Qué es, para qué sirve, especificaciones, materiales, variantes y contenido del paquete. Lo esencial en los primeros 160–500 caracteres. Sin repetir el título.
5. **product_highlight.** De 4 a 6 fragmentos factuales, cada uno sobre un aspecto distinto.
6. **product_detail.** Especificaciones técnicas agrupadas por secciones.
7. **question_and_answer.** Solo preguntas que añadan algo no cubierto en los pasos 4 a 6: uso, compatibilidad, mantenimiento, para quién es o no es, dudas reales de clientes. Antes de escribir cada pregunta, comprueba que la respuesta no está ya en otro atributo. Mejor 3 preguntas útiles que 8 repetidas. Revisa que no haya dos respuestas que se contradigan.
8. **related_product, item_group_title y variant_option.** Solo con IDs y variantes reales del catálogo.
9. **popularity_rank, document_link y video_link.** Solo si el usuario aporta los datos o las URLs.

### Paso 4: Construir el archivo

Escribe los valores en un JSON (estructura en `references/formato-sheets.md`) **sin escapar nada**: el script se encarga del escapado y de las columnas repetidas. Después ejecuta:

```bash
python scripts/build_feed.py productos.json salida/feed_suplementario.xlsx --sheets-json salida/sheets.json
python scripts/validate_feed.py salida/feed_suplementario.xlsx
```

`build_feed.py` genera el .xlsx con la pestaña **Feed** (la que lee Merchant Center) y la pestaña **Notas** (pendientes, campos vacíos a propósito y avisos antes de subir). `--sheets-json` genera además los valores y el formato listos para crear la hoja de Google. `validate_feed.py` comprueba límites, formato de los atributos de grupo, escapado, GTIN, duplicados entre atributos y lenguaje promocional.

Si el validador da errores, corrige el JSON y vuelve a construir. Los avisos se revisan uno a uno: o se corrigen, o se explican en Notas.

### Paso 5: Crear la hoja de Google

La entrega por defecto es una **hoja de Google Sheets** en el Drive del usuario. Si la conversación tiene los conectores de Google Drive y Google Sheets, lee `references/google-sheets.md` y crea la hoja con los datos de `sheets.json`: hoja vacía con Drive, formato texto y pestañas con `update_spreadsheet`, valores con `update_values` y verificación con `get_values`. El formato texto va antes que los valores, o Sheets estropea GTIN y popularity_rank.

Si faltan los conectores, dile al usuario cuál falta y entrega el .xlsx para que lo abra en Google Sheets.

### Paso 6: Entregar

Enlaza la hoja de Google (y adjunta el .xlsx como copia). En el chat, un resumen corto:

- Qué atributos se han generado y cuáles se han dejado vacíos y por qué.
- El aviso sobre contenido generado con IA en title y description (ver `contenido-ia.md`).
- Problemas detectados en origen que el suplementario no arregla: precio distinto entre feed y ficha, categoría mal asignada, GTIN ausente o inválido, imágenes.
- Decisiones conservadoras tomadas y su alternativa.

## Comprobación final antes de entregar

- ¿Hay algún dato que no esté en la ficha o en los datos aportados?
- ¿Algún valor podría rechazarlo un revisor de Google por promocional, mayúsculas o símbolos?
- ¿Las FAQ repiten algo que ya está en otro atributo? ¿Alguna se contradice con otra?
- ¿El título describe el producto que se ve en la landing, con la variante correcta?
- ¿El validador pasa sin errores?
