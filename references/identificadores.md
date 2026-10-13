# Identificadores: id, feed_label, content_language, brand, gtin, mpn

Fuentes:
- https://support.google.com/merchants/answer/7052112 (especificación de datos de producto)
- https://support.google.com/merchants/answer/160161 (identificadores únicos de producto)
- https://support.google.com/merchants/answer/6219078 (cómo encontrar un GTIN)

Ninguno de estos atributos se genera. Se copian del feed principal o los aporta el usuario, y la skill solo los valida.

## id, feed_label, content_language

- `id`: hasta 50 caracteres, único por producto, estable en el tiempo. En un suplementario tiene que ser **idéntico** al del principal, o la fila no se aplica a nada. En Shopify con la app de Google suele tener la forma `shopify_<PAÍS>_<id producto>_<id variante>`.
- `feed_label` y `content_language`: inclúyelos con los mismos valores que el principal cuando la cuenta tenga varios países o idiomas. Si no coinciden, la fila no cruza.
- Si no tienes el export del principal, **no reconstruyas IDs a ojo**. Pídelos.

## brand

- Requisito de Google para productos nuevos (salvo películas, libros y música).
- Marca del fabricante. El nombre de la tienda solo vale si la tienda fabrica el producto o es marca propia.
- No añadas "marca genérica" ni valores como "N/A".

## gtin

- Requisito de Google para productos nuevos que tengan un GTIN asignado por el fabricante.
- Admite GTIN-8, UPC (12), EAN (13) y GTIN-14. Solo dígitos, sin espacios ni guiones.
- El último dígito es de control: el validador lo comprueba. Un GTIN con el dígito de control mal es un GTIN inválido.
- Nunca se inventa ni se "completa". Si falta, se anota en Notas.
- En Sheets, formato texto: si no, se pierden ceros iniciales o aparece notación científica.

## mpn

- Referencia del fabricante. Solo si es real; no uses el SKU interno de la tienda como MPN si no es la referencia del fabricante.

## Criterio de la skill

- Si el principal ya envía brand, gtin y mpn correctos, no se incluyen en el suplementario.
- Si detectas un GTIN inválido o ausente en el principal, avísalo en el resumen y en Notas, porque es un problema de origen.
