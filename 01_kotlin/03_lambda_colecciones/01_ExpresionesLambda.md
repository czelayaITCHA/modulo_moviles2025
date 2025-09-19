# Expresiones Lambda

## 1. ¿Qué es una Expresión Lambda?
Una **expresión lambda** es una función anónima que se puede tratar como una **primera clase** en Kotlin. Esto significa que se puede asignar a una variable, pasarla como parámetro o devolverla desde otras funciones. Las lambdas permiten escribir código más conciso y expresivo, especialmente al trabajar con funciones de orden superior.

### **Sintaxis de las Expresiones Lambda**
La sintaxis de una lambda en Kotlin es muy compacta. Aquí está su estructura básica:
```kotlin
{ parámetros -> cuerpo de la función }
```
- **Parámetros**: Son los valores de entrada de la lambda, que pueden ser uno o más.
- **Cuerpo de la función**: Es el bloque de código que se ejecuta cuando la lambda es llamada. Puede ser una sola expresión o un bloque de varias líneas.
  
#### **Ejemplo básico:**
```kotlin
val suma: (Int, Int) -> Int = { a, b ->
    a + b
}
```
- Aquí, `suma` es una variable que contiene una lambda que toma dos enteros (`a` y `b`) y devuelve su suma.
- `(Int, Int) -> Int` indica que el tipo de datos es un tipo de función que acepta dos parámetros `Int` y devuelve un valor de tipo `Int`.
- Se Puede invocar a esta lambda como cualquier otra función:
```kotlin
println(suma(3, 4))  // Salida: 7
```
#### **Comparación con formas de función normales:**
Las funciones normales que realizan el mismo proceso son las siguientes.
```kotlin
fun suma(a: Int, b: Int): Int {
    return a + b
}
```
### **Sintaxis del tipo de dato de Lambda(las funciones)**
Los tipos de funciones consisten en un conjunto de paréntesis que contienen una lista de parámetros, el símbolo -> y un tipo de datos que se muestra.

![image](https://github.com/user-attachments/assets/f6b9c0c5-33da-4f7a-a3c1-72b610049140)
#### **Ejemplos**
```kotlin
// Parámetros: Sin parámetros
// Tipo de retorno: Unit - No devuelve nada
val saludo: () -> Unit = {
    println("Hola mundo!")
}

// Parámetros: 3 valores de Int
// Tipo de retorno: Int
val sumaTres: (Int, Int, Int) -> Int = { a, b, c ->
    a + b + c
}

// Parámetros: 1 valor de Int
// Tipo de retorno: String
val mostrarEdad: (Int) -> String = { edad ->
    "Tengo $edad años."
}
```

### **Minicuestionario**
- ¿Cuáles son los parámetros y tipo de retorno de los siguientes tipos de datos de función?
  ```kotlin
  () -> Boolean
  ```
  ```kotlin
  (Boolean, String, Int) -> Unit
  ```
- Responda al siguiente código de ejemplo para el tipo de datos de cada expresión lambda.
  ```kotlin
  // #1
  val imprimirMensaje = { mensaje: String ->
      println(mensaje)
  }
  
  // #2
  val procesarTexto = { texto: String ->
      val textoEnMayusculas = texto.toUpperCase()
      val longitud = textoEnMayusculas.length
      println("Texto en mayúsculas: $textoEnMayusculas")
      println("Longitud del texto: $longitud")
      longitud
  }

  // #3
  val compararNumeros = { a: Int, b: Int ->
        val resultado = if (a > b) {
            "El primer número es mayor"
        } else if (a < b) {
            "El segundo número es mayor"
        } else {
            "Los números son iguales"
        }
        println("Comparación: $resultado")
        resultado
  }
  ```
### Miniejercicio

#### Ejercicio 1:
Escribe la siguiente función como una expresión lambda.

```kotlin
fun suma(a: Int, b: Int): Int {
    return a + b
}
```

<details>
  <summary>Respuesta</summary>
  
  **Lambda:**
  ```kotlin
  val suma: (Int, Int) -> Int = { a, b -> a + b }
  ```
  **Explicación:**
  - El tipo de función `(Int, Int) -> Int` indica que la lambda toma dos parámetros de tipo `Int` y devuelve un `Int`.
  - `{ a, b -> a + b }` es la sintaxis de la lambda donde `a` y `b` son los parámetros, y `a + b` es la operación.
</details>

#### Ejercicio 2:
Escribe la siguiente función como una expresión lambda.

```kotlin
fun esPar(n: Int): Boolean {
    return n % 2 == 0
}
```

<details>
  <summary>Respuesta</summary>
  
  **Lambda:**
  ```kotlin
  val esPar: (Int) -> Boolean = { n -> n % 2 == 0 }
  ```
  **Explicación:**
  - `(Int) -> Boolean` indica que la lambda toma un `Int` y devuelve un `Boolean`.
  - `{ n -> n % 2 == 0 }` es la lógica que verifica si el número es par.
</details>

#### Ejercicio 3:
Escribe la siguiente función como una expresión lambda.

```kotlin
fun cuadrado(n: Int): Int {
    return n * n
}
```

<details>
  <summary>Respuesta</summary>
  
  **Lambda:**
  ```kotlin
  val cuadrado: (Int) -> Int = { n -> n * n }
  ```
  **Explicación:**
  - `(Int) -> Boolean` indica que la lambda toma un `Int` y devuelve un `Boolean`.
  - `{ n -> n % 2 == 0 }` es la lógica que verifica si el número es par.
</details>

#### Ejercicio 4:
Escribe la siguiente función como una expresión lambda.

```kotlin
fun saludo(nombre: String): String {
    return "Hola, $nombre"
}
```

<details>
  <summary>Respuesta</summary>
  
  **Lambda:**
  ```kotlin
  val saludo: (String) -> String = { nombre -> "Hola, $nombre" }
  ```
  **Explicación:**
  - `(String) -> String` indica que toma un `String` como argumento y devuelve otro `String`.
  - `{ nombre -> "Hola, $nombre" }` genera el saludo con el nombre dado.
</details>


#### Ejercicio 5:
Escribe la siguiente función como una expresión lambda.

```kotlin
fun longitud(str: String): Int {
    return str.length
}
```

<details>
  <summary>Respuesta</summary>
  
  **Lambda:**
  ```kotlin
  val longitud: (String) -> Int = { str -> str.length }
  ```
  **Explicación:**
  - `(String) -> Int` indica que toma una cadena (`String`) y devuelve su longitud (`Int`).
  - `{ str -> str.length }` es la operación que calcula la longitud de la cadena.
</details>

#### Ejercicio 6:
Escribe la siguiente función como una expresión lambda.

```kotlin
fun maximo(a: Int, b: Int): Int {
    return if (a > b) a else b
}
```

<details>
  <summary>Respuesta</summary>
  
  **Lambda:**
  ```kotlin
  val maximo: (Int, Int) -> Int = { a, b -> if (a > b) a else b }
  ```
  **Explicación:**
  - `(Int, Int) -> Int` indica que la lambda toma dos enteros y devuelve el mayor de ellos.
  - `{ a, b -> if (a > b) a else b }` contiene la lógica condicional para comparar los valores.
</details>

#### Ejercicio 7:
Escribe la siguiente función como una expresión lambda.

```kotlin
fun saludoFormal(nombre: String, titulo: String): String {
    return "Hola, $titulo $nombre"
}
```

<details>
  <summary>Respuesta</summary>
  
  **Lambda:**
  ```kotlin
  val saludoFormal: (String, String) -> String = { nombre, titulo -> "Hola, $titulo $nombre" }
  ```
  **Explicación:**
  - `(String, String) -> String` indica que la lambda toma dos cadenas, `nombre` y `titulo`, y devuelve un saludo formal.
  - `{ nombre, titulo -> "Hola, $titulo $nombre" }` genera el saludo con el título y el nombre.
</details>

#### Ejercicio 8:
Escribe la siguiente función como una expresión lambda.

```kotlin
fun esMayorDeEdad(edad: Int): Boolean {
    return edad >= 18
}
```

<details>
  <summary>Respuesta</summary>
  
  **Lambda:**
  ```kotlin
  val esMayorDeEdad: (Int) -> Boolean = { edad -> edad >= 18 }
  ```
  **Explicación:**
  - `(Int) -> Boolean` indica que la lambda toma un entero y devuelve `true` o `false`.
  - `{ edad -> edad >= 18 }` verifica si la edad es mayor o igual a 18.
</details>

#### Ejercicio 9:
Escribe la siguiente función como una expresión lambda.

```kotlin
fun multiplicar(a: Int, b: Int): Int {
    return a * b
}
```

<details>
  <summary>Respuesta</summary>
  
  **Lambda:**
  ```kotlin
  val multiplicar: (Int, Int) -> Int = { a, b -> a * b }
  ```
  **Explicación:**
  - `(Int, Int) -> Int` indica que la lambda toma dos enteros y devuelve su producto.
  - `{ a, b -> a * b }` realiza la multiplicación entre `a` y `b`.
</details>

#### Ejercicio 10:
Escribe la siguiente función como una expresión lambda.

```kotlin
fun esNegativo(n: Int): Boolean {
    return n < 0
}
```

<details>
  <summary>Respuesta</summary>
  
  **Lambda:**
  ```kotlin
  val esNegativo: (Int) -> Boolean = { n -> n < 0 }
  ```
  **Explicación:**
  - `(Int) -> Boolean` indica que la lambda toma un entero y devuelve `true` si es negativo.
  - `{ n -> n < 0 }` es la lógica que verifica si el número es negativo.
</details>

## 2. Funciones que aceptan lambdas como argumentos

Es posible pasar lambdas como argumentos a funciones, lo que permite definir el comportamiento dinámicamente. En otras palabras, puedes pasar una función como parámetro para ser ejecutada dentro de otra función.

### Ejemplos

La siguiente función `ejecutarOperacion` toma dos enteros y una función lambda que acepta dos enteros como parámetros y devuelve un entero.
Esta función ejecuta la operación proporcionada sobre los valores `a` y `b`, y devuelve el resultado.
```kotlin
fun ejecutarOperacion(a: Int, b: Int, operacion: (Int, Int) -> Int): Int {
    return operacion(a, b)
}
```
**Ejemplo 1: Usando una lambda para realizar una suma**
```kotlin
fun main() {
    // Lambda para sumar dos números
    val sumar = { x: Int, y: Int -> x + y }

    // Pasar la lambda a la función
    val resultado = ejecutarOperacion(5, 10, sumar)

    println("El resultado de la suma es: $resultado")  // Salida: El resultado de la suma es: 15
}

fun ejecutarOperacion(a: Int, b: Int, operacion: (Int, Int) -> Int): Int {
    return operacion(a, b)
}
```
En este ejemplo, se pasa la lambda `sumar` a la función `ejecutarOperacion` para calcular la suma de `5 + 10`.

**Ejemplo 2: Usando una lambda para realizar una multiplicación**
```kotlin
fun main() {
    // Lambda para multiplicar dos números
    val multiplicar = { x: Int, y: Int -> x * y }

    // Pasar la lambda a la función
    val resultado = ejecutarOperacion(5, 10, multiplicar)

    println("El resultado de la multiplicación es: $resultado")  // Salida: El resultado de la multiplicación es: 50
}

fun ejecutarOperacion(a: Int, b: Int, operacion: (Int, Int) -> Int): Int {
    return operacion(a, b)
}
```
Aquí, utilizamos una lambda `multiplicar` para realizar una multiplicación con la misma función `ejecutarOperacion`.

**Ejemplo 3: Pasar lambda directamente como argumentos**

En los ejemplos anteriores, la lambda se asignaba a una variable y luego la variable se pasaba a la función como argumento.En este ejemplo, la lambda encerrada en llaves se pasa directamente a la función como argumento.

Usando lambdas, podemos pasar diferentes tipos de operaciones a la misma función, permitiendo ejecutar diferentes lógicas sin duplicar código.
Aquí, usamos la misma función `ejecutarOperacion` para sumar o restar números según la lambda que se pase.
```kotlin
fun main() {
    val a = 8
    val b = 4

    // Sumar
    val resultadoSuma = ejecutarOperacion(a, b, { x, y -> x + y })
    println("Suma: $resultadoSuma")  // Salida: Suma: 12

    // Restar
    val resultadoResta = ejecutarOperacion(a, b, { x, y -> x - y })
    println("Resta: $resultadoResta")  // Salida: Resta: 4
}

fun ejecutarOperacion(a: Int, b: Int, operacion: (Int, Int) -> Int): Int {
    return operacion(a, b)
}
```

## 3. Sintaxis simplificada en lambdas (uso de `it`)

Cuando una expresión lambda tiene un solo parámetro, es posible omitir la declaración del parámetro y usar la palabra clave **`it`** en Kotlin. Este `it` se utiliza automáticamente como el único parámetro de la lambda.

### Forma habitual: Con el parámetro explícito
```kotlin
val duplicar: (Int) -> Int = { numero: Int ->
    numero * 2
}
val resultado = duplicar(5)
println(resultado)  // Salida: 10
```
### Sintaxis simplificada usando `it`
```kotlin
val duplicar: (Int) -> Int = {
    it * 2
}
val resultado = duplicar(5)
println(resultado)  // Salida: 10
```
Ambos ejemplos realizan la misma operación, pero el segundo ejemplo utiliza `it` para simplificar la sintaxis.

### Cuándo usar `it`:
El uso de `it` es útil cuando queremos escribir funciones de una manera concisa. Sin embargo, si la operación es compleja o se requiere más de un parámetro, es recomendable declarar explícitamente los nombres de los parámetros para mejorar la legibilidad.

### Miniejercicio
Reescribe las siguientes lambdas con la sintaxis simplificada usando `it`
#### Ejercicio 1:
```kotlin
val esPar: (Int) -> Boolean = { n -> n % 2 == 0 }
```
<details>
  <summary>Respuesta</summary>
  
  **Sintaxis simplificada usando `it`**
  ```kotlin
  val esPar: (Int) -> Boolean = { it % 2 == 0 }
  ```
</details>

#### Ejercicio 2:
```kotlin
val cuadrado: (Int) -> Int = { n -> n * n }
```
<details>
  <summary>Respuesta</summary>
  
  **Sintaxis simplificada usando `it`:**
  ```kotlin
  val cuadrado: (Int) -> Int = { it * it }
  ```
</details>

#### Ejercicio 3:
```kotlin
val saludo: (String) -> String = { nombre -> "Hola, $nombre" }
```
<details>
  <summary>Respuesta</summary>
  
  **Sintaxis simplificada usando `it`:**
  ```kotlin
  val saludo: (String) -> String = { "Hola, $it" }
  ```
</details>

#### Ejercicio 4:
```kotlin
  val longitud: (String) -> Int = { str -> str.length }
```

<details>
  <summary>Respuesta</summary>
  
  **Sintaxis simplificada usando `it`:**
  ```kotlin
  val longitud: (String) -> Int = { it.length }
  ```
</details>

#### Ejercicio 5:
Escribe la siguiente función como una expresión lambda.

```kotlin
val esMayorDeEdad: (Int) -> Boolean = { edad -> edad >= 18 }
```

<details>
  <summary>Respuesta</summary>
  
  **Sintaxis simplificada usando `it`:**
  ```kotlin
  val esMayorDeEdad: (Int) -> Boolean = { it >= 18 }
  ```
</details>

#### Ejercicio 6:
Escribe la siguiente función como una expresión lambda.

```kotlin
val esNegativo: (Int) -> Boolean = { n -> n < 0 }
```

<details>
  <summary>Respuesta</summary>
  
  **Sintaxis simplificada usando `it`:**
  ```kotlin
  val esNegativo: (Int) -> Boolean = { it < 0 }
  ```
</details>

## 4. Lambda de última posición
En Kotlin, si el último argumento de una función es una lambda, puedes colocar esa lambda **fuera de los paréntesis** de la llamada a la función. Esto se conoce como **trailing lambda** o **lambda de última posición**.

### Forma habitual
```kotlin
fun procesarNumeros(a: Int, b: Int, operacion: (Int, Int) -> Int): Int {
    return operacion(a, b)
}

val resultado = procesarNumeros(5, 10, { x, y -> x + y })
println(resultado)  // Salida: 15
```
### Usando lambda de última posición
```kotlin
val resultado = procesarNumeros(5, 10) { x, y -> x + y }
println(resultado)  // Salida: 15
```
Como la lambda es el último argumento de la función, podemos sacarla fuera de los paréntesis, lo que hace que el código sea más fácil de leer.
Esto se debe a que las funciones con una lambda como último argumento aparecen con frecuencia en el framework JetpackCompose que conocerás en el futuro. Por lo tanto, esta sintaxis es muy importante.

### **Minicuestionario**
- Reescribe las siguientes llamadas de funciones usando lambda de última posición.
  
  ```kotlin
  // # 1
  // Reescribe la siguiente llamada usando lambda de última posición
  val resultado = operar(5, 3, { x, y -> x + y })
  
  fun operar(a: Int, b: Int, operacion: (Int, Int) -> Int): Int {
    return operacion(a, b)
  }
  ```
  
  ```kotlin
  // # 2
  // Reescribe la siguiente llamada usando lambda de última posición
  ejecutarSiCondicionEsVerdadera(true, {
      println("¡Es de día!")
  })

  fun ejecutarSiCondicionEsVerdadera(esDia: Boolean, accion: () -> Unit) {
      if (esDia) {
          accion()
      }
  }
  ```
  
  ```kotlin
  // # 3
  // Reescribe la siguiente llamada usando lambda de última posición
  ejecutarAccion ({ println("He ejectado algo") })
  
  fun ejecutarAccion(accion: () -> Unit) {
      accion()
  }
  ```
