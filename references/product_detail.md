# Detalles del producto [product_detail]

Fuente: https://support.google.com/merchants/answer/9218260

## Formato de Google

- Atributo de grupo con 3 subatributos: `section_name` (opcional, hasta 140 caracteres), `attribute_name` (obligatorio, hasta 140) y `attribute_value` (obligatorio, hasta 1.000).
- En texto: `section_name:attribute_name:attribute_value`. Sin sección: `:attribute_name:attribute_value`.
- Repetido: hasta 1.000 valores.

## Requisitos de Google

- Sin texto promocional.
- Solo valores confirmados: no rellenes atributos sí/no por defecto.
- Sin palabras clave ni términos de búsqueda.
- No duplicar datos dentro del atributo ni datos ya enviados en title, description, question_and_answer o product_highlight.

## Buenas prácticas de Google

- Mayúscula solo inicial: "Distancia focal", no "DISTANCIA FOCAL" ni "distancia focal".
- Úsalo para cualquier información útil: contenido del paquete, conectividad, ingredientes, instalación, alimentación, funciones.

## Criterio de la skill

- Agrupa en secciones con sentido (General, Pantalla, Batería, Conectividad, Contenido del paquete…).
- Cada especificación en una entrada propia. Las enumeraciones dentro de un valor van con comas normales: el script las escapa.
- En el JSON: `{"section": "...", "name": "...", "value": "..."}`. Una columna `product_detail` por entrada.
