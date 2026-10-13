# Contenido generado con IA en title y description

Fuentes:
- https://support.google.com/merchants/answer/6324415 (title y structured_title)
- https://support.google.com/merchants/answer/6324468 (description y structured_description)

## Qué dice Google

- Los títulos y descripciones creados con IA generativa deben enviarse en `structured_title` y `structured_description`, con `digital_source_type` = `trained_algorithmic_media`, en lugar de `title` y `description`.
- Si un producto tiene a la vez `title` y `structured_title`, Google usa `title`. Lo mismo con description.
- No cumplir los requisitos de título puede provocar desaprobación.

## Por qué la skill entrega title y description

En un feed suplementario, un `structured_title` no sustituye al `title` que ya envía el feed principal (Shopify, WooCommerce, apps de feeds): Google seguiría mostrando el del principal. Para que el structured se use habría que vaciar title y description del principal con reglas de atributo, lo que complica la implantación y deja la optimización a merced de una regla bien configurada.

Por eso la skill entrega los textos en `title` y `description`, que sí sustituyen al principal, y deja constancia del riesgo.

## El aviso (obligatorio en cada entrega)

En el resumen del chat y en la pestaña Notas (build_feed.py lo añade solo cuando hay title o description):

> Los títulos y descripciones de este feed se han redactado con ayuda de IA y se envían en title y description. Google pide declarar el contenido generado con IA en structured_title y structured_description; enviarlo en title y description puede generar advertencias o desaprobaciones. Revísalos y edítalos antes de subir, y vigila la sección de problemas de Merchant Center tras la carga.

## Si el usuario quiere los atributos structured

Úsalos solo si lo pide expresamente. En el JSON se ponen `structured_title` / `structured_description` en lugar de `title` / `description`; build_feed.py monta el formato `trained_algorithmic_media:"texto"` (el texto no puede llevar comillas dobles). Explícale entonces que necesita una regla de atributo en Merchant Center que vacíe (Borrar / Clear) title y description del principal para esos productos, y que lo compruebe en el diagnóstico de un producto.

## Lo que no cambia

short_title, product_highlight, product_detail y question_and_answer no tienen versión structured: se envían en su atributo normal.
