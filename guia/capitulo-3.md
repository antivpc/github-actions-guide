[:point_up_2: Volver al Índice](README.md) | [:point_left: Capítulo 2](capitulo-2.md) | [:point_right: Capítulo 4](capitulo-4.md)

---

# Capítulo 3 - Tipos de Datos y Representación

## 1. Cadenas de Texto (Strings)

Las **cadenas de texto** o **strings** son el tipo de dato más común en YAML y se utilizan para representar texto. YAML es muy flexible en cómo se definen las cadenas, lo que permite una gran legibilidad en la mayoría de los casos.

### Cadenas sin Comillas

La forma más sencilla y común de definir una cadena de texto en YAML es **sin usar comillas**. YAML es lo suficientemente inteligente como para inferir que un valor es una cadena de texto si no parece ser otro tipo de dato (número, booleano, nulo, etc.).

* **Cuándo usarlas:** Para la mayoría de los textos simples que no contienen caracteres especiales de YAML o no son ambiguos con otros tipos de datos.
* **Ejemplo:**

    ```yaml
    mensaje: Hola mundo
    nombre_usuario: alice123
    version: 1.0.0 # Aunque tenga números y puntos, YAML lo interpreta como cadena aquí
    ```

* **Consideraciones:**
    * No deben comenzar con los caracteres `-`, `?`, `:`, `[`, `{`, `#`, `&`, `*`, `!`, `|`, `>`, `'`, `"`, `%`, `@`, `` ` `` (espacio al inicio sí está permitido después de la indentación).
    * No deben ser palabras que YAML interprete como booleanos (`true`, `false`, `yes`, `no`, `on`, `off`) o nulos (`null`, `~`).
    * Si la cadena contiene `: ` (dos puntos seguidos de un espacio), el analizador YAML podría interpretarla como una nueva clave, lo que causaría un error o una interpretación incorrecta.

### Cadenas con Comillas Simples y Dobles

Cuando una cadena de texto contiene caracteres especiales, necesita ser **encerrada entre comillas**. YAML soporta tanto comillas simples (`'`) como comillas dobles (`"`), cada una con sus propias características.

**a) Cadenas con Comillas Dobles (`""`)**

* **Uso:** Las comillas dobles permiten el uso de **secuencias de escape** (como `\n` para un salto de línea, `\t` para una tabulación, o `\"` para incluir comillas dobles dentro de la cadena).
* **Cuándo usarlas:** Ideales cuando necesitas incluir caracteres especiales que de otra manera serían problemáticos, o cuando necesitas saltos de línea explícitos dentro de la cadena.
* **Ejemplo:**

    ```yaml
    saludo_escape: "Hola a todos.\nEsto es un salto de línea."
    ruta_windows: "C:\\Users\\Usuario\\Documentos" # Se necesita \\ para escapar \
    frase_citada: "Ella dijo: \"Hola.\""
    ```

**b) Cadenas con Comillas Simples (`''`)**

* **Uso:** Las comillas simples son útiles cuando quieres que el contenido de la cadena se interprete **literalmente**, sin procesar secuencias de escape. La única excepción es para incluir una comilla simple dentro de la cadena, donde se escapa duplicándola (`''`).
* **Cuándo usarlas:** Cuando tu texto contiene caracteres que podrían ser interpretados como secuencias de escape (`\`) o cuando deseas asegurarte de que el valor sea tratado como una cadena, incluso si parece un número, booleano, etc.
* **Ejemplo:**

    ```yaml
    literal_barra: 'C:\data\file.txt' # La barra inversa se mantiene tal cual
    mensaje_literal: 'Esto es un mensaje literal, \n no se escapa la barra n.'
    frase_con_apostrofe: 'No puedo ir' # Para incluir una comilla simple: 'No puedo''t ir'
    numero_como_texto: '12345' # Forzar que sea una cadena, no un número
    ```

### Cadenas Multilínea (Literales y Plegadas)

Para textos largos que abarcan múltiples líneas, YAML ofrece dos estilos de bloque que mejoran drásticamente la legibilidad: el **estilo literal** (`|`) y el **estilo plegado** (`>`).

**a) Estilo Literal (`|`)**

* **Uso:** Conserva **todos los saltos de línea** y los espacios de indentación iniciales (excepto la indentación del bloque YAML en sí) exactamente como están escritos.
* **Cuándo usarlo:** Cuando el formato exacto del texto es importante, como en el caso de bloques de código, poesía o mensajes donde cada salto de línea es relevante.
* **Ejemplo:**

    ```yaml
    poema: |
      Un día la tierra
      tembló y la luna
      se hizo pedazos.
    ```
    Cuando este YAML se procesa, la cadena resultante sería: `"Un día la tierra\ntembló y la luna\nse hizo pedazos.\n"` (nota el salto de línea final, que se incluye por defecto).

    Puedes controlar el salto de línea final añadiendo un indicador:
    * `|+`: Conserva el salto de línea final explícitamente.
    * `|-`: Elimina el salto de línea final.
    * `|`: Comportamiento por defecto (generalmente mantiene un salto de línea final si lo hay, pero puede variar ligeramente entre procesadores).

    ```yaml
    mensaje_final: |-
      Gracias por usar
      nuestro servicio.
    ```
    Esto resultaría en `"Gracias por usar\nnuestro servicio."` (sin el salto de línea al final de la cadena).

**b) Estilo Plegado (`>`)**

* **Uso:** Convierte los saltos de línea en **espacios individuales**, "plegando" el texto en una sola línea lógica. Los saltos de línea en blanco (líneas vacías) se conservan como saltos de párrafo.
* **Cuándo usarlo:** Para textos largos donde el flujo del párrafo es más importante que la conservación exacta de los saltos de línea, como descripciones o párrafos de documentación.
* **Ejemplo:**

    ```yaml
    descripcion_producto: >
      Este producto es innovador y ofrece
      soluciones avanzadas para la gestión de datos.
      Está diseñado para optimizar el rendimiento
      y la escalabilidad en entornos complejos.
    ```
    Cuando este YAML se procesa, la cadena resultante sería: `"Este producto es innovador y ofrece soluciones avanzadas para la gestión de datos. Está diseñado para optimizar el rendimiento y la escalabilidad en entornos complejos.\n"`

    Al igual que con el estilo literal, puedes controlar el salto de línea final con `>+` o `>-`.

    ```yaml
    parrafo_largo: >-
      Primer párrafo.

      Segundo párrafo.
    ```
    Esto resultaría en `"Primer párrafo.\nSegundo párrafo."` (el salto de línea entre párrafos se conserva, pero los saltos de línea simples se convierten en espacios, y no hay salto de línea final).

Entender estas diferentes formas de manejar cadenas es crucial para escribir archivos YAML claros y para evitar sorpresas al parsear tus datos.

---

[:point_up_2: Volver al Índice](README.md) | [:point_left: Capítulo 2](capitulo-2.md) | [:point_right: Capítulo 4](capitulo-4.md)
