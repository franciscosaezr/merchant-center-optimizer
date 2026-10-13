# Productos relacionados [related_product]

Fuente: https://support.google.com/merchants/answer/17085213

Atributo conversacional: se usa en experiencias de IA para sugerir productos que se compran junto a este o en su lugar. Opcional y válido en todos los países.

## Formato de Google

Atributo de grupo con 3 subatributos obligatorios: `relationship_type:identifier_type:identifier`

- `relationship_type`:
  - `part_of_set`: de la misma serie o línea (una silla del mismo juego que una mesa).
  - `required_part`: necesario para que funcione (la batería de una lámpara a pilas).
  - `often_bought_with`: se compra a menudo junto a este (funda y móvil).
  - `substitute`: puede sustituirlo (otra impresora de función equivalente).
  - `different_brand`: el mismo producto con otra marca (marca blanca).
  - `accessory`: accesorio (webcam para un ordenador).
- `identifier_type`: `gtin` o `id` (el id del producto en tu fuente de datos).
- `identifier`: el valor. Solo letras, números, guiones bajos y guiones. Nada de comas, dos puntos, comillas ni barras invertidas.
- Repetido: hasta 30.

## Requisitos de Google

- Los 3 subatributos en cada valor.
- Un valor por producto relacionado: nunca varios identificadores separados por comas dentro del mismo identifier.

## Criterio de la skill

- Solo productos que existan en el catálogo, con su id real del feed o su GTIN real. Si no tienes los IDs, deja el atributo vacío y pídelos en Notas.
- La relación tiene que ser cierta y comprobable en la tienda (accesorio que encaja de verdad, sustituto de la misma función). `often_bought_with` solo con datos de pedidos; si no los hay, no lo uses.
- Un producto puede tener varias relaciones de tipos distintos.
- En el JSON: `{"relationship_type": "accessory", "identifier_type": "id", "identifier": "..."}`. Una columna por relación.
