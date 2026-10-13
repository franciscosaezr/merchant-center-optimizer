# Formato del feed suplementario en Google Sheets

Fuentes:
- https://support.google.com/merchants/answer/14990942 (fuentes de datos suplementarias)
- https://support.google.com/merchants/answer/12158053 (productos desde Google Sheets y escapado)
- https://support.google.com/merchants/answer/17085370 (atributos conversacionales y plantilla TSV)

## Cómo funciona un suplementario

- Solo actualiza productos que ya existen en el feed principal. No crea ni elimina productos.
- Cruza por `id`, que tiene que ser idéntico al del principal. Si el principal segmenta por país e idioma, incluye `feed_label` y `content_language` con los mismos valores.
- Lleva el `id` y solo las columnas que quieras añadir o sobrescribir. Una columna que no está no toca nada del principal.
- Por defecto, el valor del suplementario sustituye al del principal en las columnas que envía. Las reglas de atributo de Merchant Center pueden cambiar ese comportamiento; si algo no se aplica, revísalas.

## Estructura del archivo

- **Pestaña Feed:** fila 1 con cabeceras en minúscula con el nombre técnico del atributo (`product_highlight`, no "Product highlight"). Una fila por producto (por variante si el principal va por variante).
- **Pestaña Notas:** pendientes, campos vacíos a propósito y avisos. Merchant Center solo lee la pestaña que se configure como fuente, así que Notas no interfiere.
- **Todas las celdas en formato texto.** Si no, Sheets convierte GTIN a notación científica, se come los ceros iniciales, o transforma `95.5` en fecha o en `95,5` con configuración regional española. `build_feed.py` ya marca las celdas como texto en el .xlsx; si se copia a mano a otra hoja, formato → número → texto sin formato antes de pegar.

## Atributos repetidos: una columna por valor

Para product_highlight, product_detail, question_and_answer, related_product, variant_option, document_link y video_link se usa **una columna por valor con la misma cabecera repetida** (6 columnas `product_highlight`, 8 columnas `question_and_answer`…). Es la forma que Google documenta como alternativa a meter varios valores en una celda separados por comas, y es mucho más legible y fácil de revisar.

## Escapado

En Google Sheets, dentro del valor de un atributo repetido o de grupo:

- La **coma** separa valores. Si forma parte del texto, va escapada: `\,`
- Los **dos puntos** separan subatributos en los atributos de grupo (product_detail, question_and_answer, related_product, variant_option). Si forman parte del texto, van escapados: `\:`
- La **barra invertida** del texto se escapa como `\\`

No se escapa nada en atributos simples (title, description, short_title, product_type, google_product_category, popularity_rank). Si el usuario pide structured_title / structured_description, van con el texto entre comillas (ver `contenido-ia.md`).

No sustituyas comas por " / " para evitar el escapado: el texto que leerá el usuario en AI Mode queda peor. Escapa.

Si tras subir el feed aparecen valores partidos por la mitad o subatributos desplazados, el problema casi siempre es una coma o unos dos puntos sin escapar.

## JSON de entrada para build_feed.py

Los valores se escriben **limpios, sin escapar**. El script escapa, crea las columnas repetidas y genera la pestaña Notas.

```json
{
  "productos": [
    {
      "id": "shopify_ES_123_456",
      "feed_label": "ES",
      "content_language": "es",
      "title": "Texto del título",
      "description": "Texto de la descripción",
      "short_title": "Texto corto",
      "google_product_category": "152",
      "product_type": "Nivel 1 > Nivel 2 > Nivel 3",
      "brand": "Marca",
      "gtin": "8400000000000",
      "mpn": "REF-001",
      "product_highlight": ["Fragmento 1", "Fragmento 2"],
      "product_detail": [
        {"section": "General", "name": "Resolución", "value": "8 megapíxeles"}
      ],
      "question_and_answer": [
        {"question": "¿Pregunta?", "answer": "Respuesta, con comas si hace falta."}
      ],
      "related_product": [
        {"relationship_type": "accessory", "identifier_type": "id", "identifier": "shopify_ES_789_012"}
      ],
      "item_group_title": "Nombre común del grupo de variantes",
      "variant_option": [{"name": "color", "value": "negro"}],
      "popularity_rank": 87.5,
      "document_link": ["https://tienda.com/manual.pdf"],
      "video_link": ["https://tienda.com/video.mp4"]
    }
  ],
  "notas": [
    "Texto libre que irá a la pestaña Notas."
  ]
}
```

- Solo `id` es obligatorio. Las claves que no estén o estén vacías no generan columna (salvo que otro producto del mismo archivo sí la tenga; entonces la celda queda vacía).
- Celdas vacías en columnas que el principal también envía (brand, gtin, mpn, google_product_category, product_type, title, description): no está documentado si Google las ignora o las usa para borrar el valor del principal. No lo dejes al azar. Si un producto no necesita ese atributo, sácalo a otro archivo o rellénalo con el valor correcto. El validador avisa de estos casos.
- brand, gtin y mpn solo se incluyen en el suplementario si hay que corregir el valor del principal. Si el principal ya los envía bien, no hace falta repetirlos.
- Si hay title o description, la pestaña Notas incluye el aviso sobre contenido generado con IA.
- `structured_title` / `structured_description` solo si el usuario los pide; en ese caso Notas incluye la regla que vacía title y description del principal.
