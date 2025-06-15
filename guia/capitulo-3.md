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

## 2. Números

Los **números** son otro tipo de dato fundamental en YAML, utilizados para representar valores cuantitativos. YAML es bastante flexible y puede inferir automáticamente si un valor es un entero o un flotante. No necesitas usar comillas para la mayoría de los números, a menos que quieras forzarlos a ser tratados como cadenas de texto.

### Enteros

Los **enteros** son números sin componentes fraccionarios. YAML puede reconocer enteros en varias bases:

* **Decimal:** La forma más común, números que usamos a diario.
    ```yaml
    edad: 30
    cantidad_productos: 150
    puntos: -50
    ```

* **Octal:** Números que comienzan con `0o` (cero seguido de la letra 'o').
    ```yaml
    permisos_octal: 0o755 # Representa 493 en decimal
    ```

* **Hexadecimal:** Números que comienzan con `0x`.
    ```yaml
    color_hex: 0xFF # Representa 255 en decimal
    error_code: 0xA4 # Representa 164 en decimal
    ```

* **Binario:** Números que comienzan con `0b`.
    ```yaml
    banderas_binarias: 0b101101 # Representa 45 en decimal
    ```

### Flotantes

Los **flotantes** (o números de coma flotante) son números que incluyen una parte fraccionaria, indicada por un punto (`.`).

* **Sintaxis básica:**
    ```yaml
    temperatura: 25.5
    precio_unitario: 19.99
    gravedad: 9.81
    ```

* **Valores especiales:** YAML también soporta representaciones para infinito y no-un-número.
    * **Infinito:** `inf`, `Inf`, `INF`, `+inf`, `+Inf`, `+INF` para infinito positivo. `-inf`, `-Inf`, `-INF` para infinito negativo.
        ```yaml
        limite_superior: .inf # Equivalente a infinito positivo
        limite_inferior: -.inf # Equivalente a infinito negativo
        ```
    * **No es un número (NaN):** `.nan`, `.Nan`, `.NAN`. Se usa para representar un valor indefinido o no representable numéricamente (por ejemplo, el resultado de una división por cero).
        ```yaml
        resultado_indefinido: .nan
        ```

### Notación Exponencial

Para números muy grandes o muy pequeños, puedes usar la **notación exponencial** (también conocida como notación científica o notación E), donde un número es seguido por `e` o `E` y un exponente.

* **Sintaxis:** `[número][e|E][+/-][exponente]`
* **Ejemplo:**
    ```yaml
    distancia_luz: 3.0e+8 # 3.0 * 10^8 (300,000,000)
    carga_electron: -1.602176634e-19 # -1.602176634 * 10^-19
    ```

**Consideraciones clave para números en YAML:**

* **Comillas:** Si un número contiene caracteres que no son dígitos (excepto el punto decimal o los prefijos de base) o si quieres que un número sea tratado estrictamente como una cadena de texto (ej. un ID que parece número pero tiene ceros iniciales que deben preservarse), debes **encerrarlo entre comillas**.
    ```yaml
    id_producto: "007" # Esto será tratado como la cadena "007", no el número 7
    numero_de_telefono: "555-1234" # Esto es una cadena debido al guion
    ```
* **Separadores de miles:** YAML **no soporta** separadores de miles (como comas o puntos, dependiendo de la configuración regional) dentro de los números. Si los usas, el valor se interpretará como una cadena.
    ```yaml
    valor_incorrecto: 1,000,000 # Esto será la cadena "1,000,000"
    valor_correcto: 1000000 # Esto será el número 1000000
    ```

Comprender cómo YAML maneja los diferentes tipos de números te permitirá representar datos cuantitativos de forma precisa y evitar interpretaciones erróneas de tus configuraciones.

---

## 3. Booleanos

Los **booleanos** son un tipo de dato lógico que puede tener solo uno de dos valores: **verdadero** o **falso**. Se utilizan comúnmente para configurar interruptores (on/off), banderas (flags) o condiciones binarias en archivos de configuración.

YAML es muy flexible y amigable con el usuario en cuanto a la representación de valores booleanos. Reconoce un conjunto de palabras que, sin comillas, son automáticamente interpretadas como `true` o `false`. Esta flexibilidad mejora la legibilidad para diferentes audiencias.

### Valores Verdaderos y Falsos

YAML es "case-insensitive" para la mayoría de las representaciones booleanas, lo que significa que no distingue entre mayúsculas y minúsculas para estas palabras clave.

**Representaciones de `true` (verdadero):**

* `true`
* `True`
* `TRUE`
* `on`
* `On`
* `ON`
* `yes`
* `Yes`
* `YES`

**Representaciones de `false` (falso):**

* `false`
* `False`
* `FALSE`
* `off`
* `Off`
* `OFF`
* `no`
* `No`
* `NO`

**Ejemplos de uso:**

```yaml
# Ejemplos de valores verdaderos
habilitar_notificaciones: true
activar_cache: ON
modo_debug: yes

# Ejemplos de valores falsos
deshabilitar_envio_email: false
permitir_publico: No
uso_experimental: off

# Un booleano dentro de una lista
opciones:
  - nombre: guardar_log
    valor: True
  - nombre: procesar_datos
    valor: False
```

**Consideraciones importantes:**

* **Sin comillas:** Para que YAML interprete el valor como un booleano, **no debe estar entre comillas**. Si pones ` "true" ` o ` 'false' `, YAML lo tratará como una cadena de texto, no como un valor booleano. Esto es crucial si tu sistema espera un tipo de dato booleano y no una cadena.
    ```yaml
    # Esto es un booleano (correcto)
    estado_activo: true

    # Esto es una cadena de texto (¡no es un booleano!)
    cadena_booleana: "true"
    ```
* **Contexto:** Aunque YAML es inteligente, siempre considera el contexto. Si una palabra como "No" está claramente en un contexto donde se espera una cadena (por ejemplo, `nombre: Noé`), la mayoría de los analizadores modernos no la confundirán con un booleano `false`. Sin embargo, para evitar cualquier ambigüedad, si una cadena de texto coincide con una palabra booleana y el contexto no es obvio, es una buena práctica encerrarla entre comillas.
    ```yaml
    # Aquí "No" es una cadena porque se espera un nombre
    primer_nombre: No

    # Aquí "no" es un booleano porque el contexto implica una condición
    permitir_acceso: no
    ```

El uso adecuado de los booleanos facilita la configuración lógica de tus aplicaciones y sistemas, permitiendo una fácil activación o desactivación de funcionalidades con solo un vistazo al archivo YAML.

---

[:point_up_2: Volver al Índice](README.md) | [:point_left: Capítulo 2](capitulo-2.md) | [:point_right: Capítulo 4](capitulo-4.md)
