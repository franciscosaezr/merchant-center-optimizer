# Merchant Center Optimizer

Skill para Claude que prepara feeds suplementarios de Google Merchant Center listos para Google Sheets.

Le das un producto (URL, export del feed, informe de términos de búsqueda o una descripción) y te devuelve una hoja de Google Sheets en tu Drive (o un `.xlsx` si no tienes los conectores de Google) con:

- **Pestaña Feed:** la que conectas como fuente suplementaria en Merchant Center.
- **Pestaña Notas:** lo que hay que revisar antes de subirlo, los campos vacíos a propósito y por qué.

## Qué atributos trabaja

| Grupo | Atributos |
|---|---|
| Texto | title, description, short_title |
| Clasificación | google_product_category, product_type |
| Detalle | product_highlight, product_detail |
| Conversacionales (AI Mode) | question_and_answer, related_product, item_group_title, variant_option, popularity_rank, document_link |
| Vídeo | video_link |
| Cruce e identificadores (se copian, no se inventan) | id, feed_label, content_language, brand, gtin, mpn |

## Cómo trabaja

1. Lee la ficha y los datos que le des. No inventa nada: si un dato no está en la ficha, no entra en el feed.
2. Antes de cada atributo lee su reference en `references/`, con los requisitos de Google enlazados a la fuente y separados de los criterios propios de la skill.
3. Escribe los valores en un JSON limpio.
4. `scripts/build_feed.py` monta el `.xlsx`: columnas repetidas, escapado de comas y dos puntos, celdas en texto y aviso sobre contenido IA en Notas.
5. Si tienes conectados Google Drive y Google Sheets, crea la hoja directamente en tu Drive, con las celdas en formato texto para que no se estropeen los GTIN.
6. `scripts/validate_feed.py` revisa límites, formato, GTIN, lenguaje promocional y FAQs que repiten lo que ya está en otros atributos.

## Tres cosas que conviene saber antes de usarla

**Los títulos y descripciones van en title y description, con aviso.** Google pide declarar el texto generado con IA en structured_title y structured_description, pero en un suplementario esos atributos no sustituyen al title del feed principal. La skill entrega title y description para que el cambio se aplique, y en cada feed deja escrito el riesgo para que revises los textos antes de subirlos. Si prefieres los structured, pídeselo y te explica cómo vaciar el title del principal.

**Las FAQ no son relleno.** Google pide no repetir en question_and_answer lo que ya está en el título, la descripción, los highlights o product_detail. La skill prefiere tres preguntas útiles a ocho que repiten la ficha, y el validador avisa cuando una respuesta duplica datos.

**popularity_rank va con ventas, no con clics.** Si no hay datos de ventas, la skill lo deja vacío.

## Instalación

**Claude Code:** copia la carpeta `merchant-center-optimizer` en `~/.claude/skills/` (o en `.claude/skills/` dentro de un proyecto).

**Claude.ai / app de escritorio:** comprime la carpeta en un `.zip` y súbela como skill desde la configuración de Claude.

Para que cree la hoja en tu Drive, conecta en Claude los conectores de **Google Drive** y **Google Sheets**. Sin ellos, te entrega el `.xlsx`.

Los scripts necesitan Python 3 y `openpyxl`:

```bash
pip install openpyxl
```

## Uso de los scripts a mano

```bash
python scripts/build_feed.py productos.json salida/feed.xlsx
python scripts/validate_feed.py salida/feed.xlsx
```

El validador también lee `.csv` y `.tsv` descargados de Google Sheets, así que sirve para revisar un feed suplementario que ya tengas.

En `examples/` hay un producto ficticio con su JSON y su `.xlsx`. En `templates/` hay una plantilla vacía.

## Qué no hace

- No sube nada a Merchant Center ni crea las reglas de atributo.
- No corrige problemas del feed principal (precio distinto entre feed y web, imágenes, disponibilidad). Los detecta cuando puede y te los deja en Notas.
- No genera GTIN, marcas ni IDs.

## Mantenimiento

Google cambia la especificación de producto cada año. Cada reference lleva el enlace a su fuente: si algo cambia, se actualiza la reference y la skill lo aplica.

## Autor

Francisco Sáez (Paco) · Google Ads para tiendas online · [franciscosaezr.com](https://franciscosaezr.com)

Licencia MIT. Úsala, cámbiala y compártela.
