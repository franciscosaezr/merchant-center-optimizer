# Crear el feed directamente en Google Sheets

Si la conversación tiene los conectores de **Google Drive** y **Google Sheets**, el feed se entrega como una hoja de Google en el Drive del usuario, lista para conectarla a Merchant Center como fuente suplementaria. Si no los tiene, se entrega el .xlsx (ver "Sin conectores").

## 1. Comprobar los conectores

Hacen falta estas herramientas (los nombres exactos cambian según la superficie; en algunas hay que cargarlas antes con una búsqueda de herramientas):

| Conector | Herramienta | Para |
|---|---|---|
| Google Drive | `create_file` | Crear la hoja vacía |
| Google Sheets | `update_spreadsheet` | Renombrar pestaña, crear Notas, formato |
| Google Sheets | `update_values` | Escribir los valores |
| Google Sheets | `get_values` | Verificar |

Si falta Drive o Sheets, díselo al usuario: qué conector falta y que, con él activado, el feed sale directamente en su Drive. Mientras tanto, entrega el .xlsx.

## 2. Generar los datos

```bash
python scripts/build_feed.py productos.json salida/feed.xlsx --sheets-json salida/sheets.json
python scripts/validate_feed.py salida/feed.xlsx
```

`sheets.json` contiene:

- `requests`: las peticiones de `update_spreadsheet`, ya construidas.
- `feed_values` y `feed_range`: la pestaña Feed, ya escapada y con columnas repetidas.
- `notas_values` y `notas_range`: la pestaña Notas.

No reconstruyas estos valores a mano ni los copies del JSON de productos: usa los de `sheets.json`, que son los que han pasado por el escapado y el validador.

## 3. Crear la hoja (4 llamadas, en este orden)

1. **Drive `create_file`** con `contentMimeType: "application/vnd.google-apps.spreadsheet"` y un título descriptivo, por ejemplo `Feed suplementario - <tienda> - <fecha>`. La primera pestaña de una hoja nueva tiene `sheetId` 0.
2. **Sheets `update_spreadsheet`** con la lista `requests` de `sheets.json`, sin `writeControl`. Amplía la cuadrícula (una hoja nueva solo tiene 26 columnas y un feed suele pasar de 26; sin ampliarla, el lote entero falla con "exceeds grid limits"), renombra la pestaña a Feed, congela la cabecera, crea la pestaña Notas (`sheetId` 1), pone **toda la pestaña Feed en formato texto**, pone la cabecera en negrita y en verde las columnas conversacionales.
3. **Sheets `update_values`** dos veces: `feed_values` en `Feed!A1` y `notas_values` en `Notas!A1`.
4. **Sheets `get_values`** sobre `Feed!A:ZZ` para verificar.

El orden importa: el formato texto tiene que estar aplicado **antes** de escribir. `update_values` interpreta los datos como si se tecleasen en Sheets, y sin formato texto un GTIN pierde los ceros iniciales o pasa a notación científica, y `87.5` puede convertirse en fecha o en `87,5`.

Con muchos productos, escribe `feed_values` por bloques de unas 200 filas (`Feed!A1`, `Feed!A202`…) para no mandar una llamada enorme.

## 4. Verificar

Este flujo se ha probado con el conector de Google Sheets: con la pestaña en formato texto antes de escribir, los GTIN con ceros iniciales, `popularity_rank` con punto decimal y las barras invertidas del escapado se conservan tal cual.

En el resultado de `get_values`, comprueba:

- Número de filas = `filas` + 1 (cabecera) y número de columnas = `columnas`.
- Los GTIN se leen completos, con todos sus dígitos.
- `popularity_rank` sale con punto decimal (`87.5`).
- Las barras invertidas del escapado siguen ahí (`3000 K\, 4000 K`).
- Ninguna celda muestra `#ERROR!` ni empieza a calcular una fórmula.

Si algo no cuadra, corrige en la misma hoja (`update_values` sobre el rango afectado). No crees otra hoja nueva: el usuario perdería el enlace que ya tiene.

## 5. Entregar

- Enlaza la hoja en la frase que dice lo que has hecho.
- Recuerda que en Merchant Center hay que añadirla como fuente de datos suplementaria desde Google Sheets, eligiendo la pestaña **Feed**, y vincularla a la fuente principal.
- Incluye el aviso de contenido IA y los pendientes de Notas en el resumen.
- Adjunta también el .xlsx como copia.

## Cambios posteriores

Si el usuario pide cambiar algo del feed ya creado, se edita la misma hoja con `update_values`. Vuelve a ejecutar `build_feed.py` con el JSON corregido para obtener los valores escapados, y sobrescribe solo las filas o columnas que cambian. Si cambia el número de columnas repetidas (por ejemplo, una FAQ más), reescribe la pestaña Feed completa: antes, `batch_clear_values` sobre `Feed!A:ZZ` para no dejar columnas viejas.

## Sin conectores

Entrega el .xlsx. El usuario lo sube a Drive y lo abre con Google Sheets (o lo importa desde Archivo > Importar en una hoja nueva). Las celdas ya van en formato texto.
