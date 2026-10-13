# Preguntas y respuestas [question_and_answer]

Fuente: https://support.google.com/merchants/answer/17085211

Atributo conversacional: pensado sobre todo para experiencias como AI Mode en la Búsqueda de Google. Opcional y válido en todos los países.

## Formato de Google

- Atributo de grupo con 2 subatributos obligatorios: `question` y `answer`, hasta 1.000 caracteres cada uno.
- Repetido: hasta 30 pares.
- Límite total de todos los pares de un producto: 10.000 caracteres.
- En Google Sheets: `pregunta:respuesta`, con comas, dos puntos y barras invertidas del texto escapadas (`\,` `\:` `\\`).

## Requisitos de Google

- Pregunta y respuesta siempre juntas.
- Solo información del producto. Nada de precios, fechas ni datos de la oferta: eso va en sus atributos.
- Sin listas de palabras clave.
- Respuestas correctas e informativas.

## Buenas prácticas de Google

- Revisar gramática, ortografía y mayúsculas.
- Variedad: especificaciones, ingredientes, contenido del paquete, ocasiones, deportes, actividades o temáticas para las que sirve el producto.
- **No duplicar** lo que ya está en title, description, product_detail o product_highlight.
- **No enviarlo** si también se envía document_link y la misma información está en esos documentos: Google extrae las FAQ de los PDF.

## Criterio de la skill

Este es el atributo donde más fácil es meter relleno. Antes de escribir una pregunta:

1. ¿La respuesta ya está en la descripción, los highlights o product_detail? Si sí, fuera.
2. ¿Es una duda real? Fuentes buenas: FAQ de la ficha, reseñas, preguntas de clientes que aporte el usuario, objeciones típicas de la categoría.
3. ¿La respuesta se puede comprobar en la ficha o en los datos aportados? Si no, fuera.

Tipos de pregunta que suelen aportar: compatibilidad ("¿funciona con iPhone?"), uso y mantenimiento, para quién es o no es, qué no incluye, diferencias entre variantes del propio catálogo, cómo se hace algo concreto con el producto.

- Mejor 3 preguntas útiles que 8 que repiten la ficha.
- Revisa que no haya dos respuestas que se contradigan ni dos preguntas que digan lo mismo con otras palabras.
- Respuestas que empiecen respondiendo ("Sí.", "No.", el dato) y luego maticen.
- Escribe enumeraciones con comas normales en el JSON: el script las escapa. No uses " / " como sustituto.
