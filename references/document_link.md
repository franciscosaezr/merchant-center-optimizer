# Enlace a documento [document_link]

Fuente: https://support.google.com/merchants/answer/17084656

Atributo conversacional: Google usa los documentos para responder preguntas detalladas sobre el producto. Opcional y válido en todos los países.

## Formato de Google

- URL que empiece por http:// o https://, ASCII, conforme a RFC 3986, hasta 2.000 caracteres.
- Solo PDF.
- Repetido: hasta 5.
- Caracteres especiales de la URL codificados (una coma se escribe %2C).

## Requisitos de Google

- Archivo de 50 MB como máximo.
- Derechos para usar el documento con fines de marketing.
- URL pública, accesible para Googlebot sin login ni bloqueos en robots.txt, estable y que no caduque.

## Buenas prácticas de Google

- Documentos sobre el producto (pueden incluir otros productos relacionados): manuales, guías de uso, instrucciones de montaje, prospectos, diseños de packaging.
- No documentos sobre la empresa.
- Documentos de calidad, con estructura clara.

## Criterio de la skill

- Solo URLs que aporte el usuario o que estén enlazadas en la ficha. Nunca construyas una URL suponiendo que existirá.
- Si se envía document_link, recorta question_and_answer: Google extrae las FAQ del PDF y pide no duplicarlas.
- Si no hay PDF, déjalo vacío. Una URL inválida da error en el feed.
