# Variantes: item_group_title y variant_option

Fuentes:
- https://support.google.com/merchants/answer/17085146 (item_group_title)
- https://support.google.com/merchants/answer/17085214 (variant_option)
- https://support.google.com/merchants/answer/6324507 (item_group_id)

Atributos conversacionales. Solo para productos que son **variantes** de un mismo producto.

## Formato de Google

- `item_group_title`: título común del grupo de variantes, el mismo para todas las variantes con el mismo `item_group_id`. Ejemplo: `Camiseta de algodón orgánico para hombre`.
- `variant_option`: atributo de grupo con 2 subatributos, `name:value`. Ejemplo en una celda: `color:negro,talla:M`. Repetido: una entrada por dimensión de variante.

## Requisitos de Google

- Se envían junto a `item_group_id` (que viene del principal) e `item_group_title`.
- Cada variante tiene una combinación de valores única.
- Todas las variantes del grupo usan el mismo conjunto de `name`.
- Además de variant_option, se siguen enviando los atributos estándar de variante cuando aplican: color, pattern, material, age_group, gender, size.
- Los valores tienen que coincidir con lo que se ve en la landing de esa variante.
- No enviarlos si el producto no es una variante, aunque se parezca a otros.

## Criterio de la skill

- item_group_title es el título sin la parte que cambia entre variantes. Mismo estilo que el título, sin variante.
- Usa los mismos `name` en todo el grupo y en minúscula (`color`, `talla`, `capacidad`).
- Si no tienes el item_group_id y la lista de variantes del principal, no los generes.
