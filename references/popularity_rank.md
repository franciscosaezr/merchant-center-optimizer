# Ranking de popularidad [popularity_rank]

Fuente: https://support.google.com/merchants/answer/17085297

Atributo conversacional. Opcional y válido en todos los países.

## Formato de Google

- Número entre 0 y 100, con un decimal como máximo. Sin el signo %.
- No repetido.
- Representa la popularidad del producto como percentil del inventario: cuanto más alto, mejor se vende respecto al resto.

## Requisitos de Google

- Tiene que reflejar con exactitud **cómo se vende** el producto frente al resto del catálogo.
- Actualizarlo cuando la popularidad cambie de forma notable según las ventas recientes.

## Criterio de la skill

- Solo con datos de ventas (unidades o pedidos de un periodo reciente, por ejemplo 90 días) de todo el catálogo. Clics, impresiones o tráfico no son ventas: si es lo único que hay, deja el atributo vacío y anótalo en Notas.
- Cálculo recomendado: percentil del producto en unidades vendidas sobre el total de productos activos. El más vendido se acerca a 100, el que no vende se acerca a 0.
- Con pocos productos en el catálogo, el percentil es muy sensible: avísalo.
- Al ser un dato que caduca, recomienda actualizarlo de forma periódica (mensual, por ejemplo).
- En Sheets, celda en formato texto y con punto decimal (`87.5`).
