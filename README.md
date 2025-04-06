# Botón Justificar para Editor de Bloques WordPress (Gutenberg)

Este snippet de PHP añade un botón de **Justificar** a la barra de herramientas de bloques específicos (Párrafo y Encabezado) en el editor de bloques de WordPress (Gutenberg). A diferencia de otros métodos que aplican justificación solo al texto seleccionado (inline), este snippet funciona a nivel de bloque, similar a los botones de alineación Izquierda, Centro y Derecha.

Al hacer clic en el botón Justificar, se añade la clase CSS `has-text-align-justify` al bloque completo (`<p>`, `<h1>`, etc.) y se eliminan otras clases de alineación (`has-text-align-left`, `has-text-align-center`, `has-text-align-right`) para evitar conflictos. Al hacer clic de nuevo, se elimina la clase de justificación.

Creado con ❤️ por Gemini y Victor

## Características

* Añade un botón "Justificar" a la barra de herramientas principal de los bloques seleccionados.
* Funciona a nivel de bloque (aplica/quita la clase `has-text-align-justify` al elemento principal del bloque).
* Elimina otras clases de alineación (izquierda, centro, derecha) al justificar para evitar conflictos CSS.
* Aplica los estilos CSS necesarios tanto en el editor como en el frontend.
* Actualmente configurado para bloques de Párrafo (`core/paragraph`) y Encabezado (`core/heading`).

## Instalación

1.  **Requiere un plugin de Snippets:** Necesitas tener instalado un plugin que te permita añadir snippets de código PHP fácilmente, como [Code Snippets](https://wordpress.org/plugins/code-snippets/) o similar.
2.  **Añadir Nuevo Snippet:**
    * Ve a `Snippets` -> `Añadir nuevo` en tu panel de WordPress.
    * Dale un título (ej. "Botón Justificar Bloques").
    * Copia **todo el contenido** del archivo `justify-button-snippet.php` (o el código proporcionado por Gemini) y pégalo en el área de código del snippet.
3.  **Activar:** Selecciona "Ejecutar snippet en todas partes" (o asegúrate de que se ejecute al menos en el área de administración) y haz clic en "Guardar cambios y activar".

## Uso

1.  Edita una entrada o página con el editor de bloques.
2.  Selecciona un bloque de Párrafo o Encabezado.
3.  Busca el nuevo icono de justificar (similar a líneas de texto alineadas) en la barra de herramientas principal del bloque.
4.  Haz clic en el botón para justificar el bloque completo. Haz clic de nuevo para quitar la justificación (volverá a la alineación por defecto, izquierda).

## Compatibilidad

* Desarrollado y probado en WordPress 6.7.2.
* Diseñado para funcionar con el editor de bloques (Gutenberg).
* Aplica el botón a los bloques `core/paragraph` y `core/heading`. Puedes modificar la constante `allowedBlocks` en el código JavaScript si deseas añadir soporte para otros tipos de bloque.

## Licencia

Puedes usar, modificar y distribuir este código libremente. Licencia MIT etc.
