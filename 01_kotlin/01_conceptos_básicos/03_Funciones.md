# Funciones en Kotlin
## 1. Cómo definir una función y cómo llamarla
Para definir una función, comienza con la palabra clave `fun` en Kotlin.

Sintaxis:

![image](https://github.com/user-attachments/assets/0b881cbe-3625-409e-872b-60f99a55f4b4)

Ejecute el siguiente código de ejemplo para ver cómo se definen y llaman las funciones.

```kotlin
fun main() {
    // Llamada a la funcion
    println(saludoCumpleaños("Juan", 25))
}

// Definición de la función
fun saludoCumpleaños(nombre: String, edad: Int): String {
    val saludoNombre = "¡Feliz cumpleaños, $nombre!"
    val saludoEdad = "¡Ahora tienes $edad años!"
    return "$saludoNombre\n$saludoEdad"
}
```

- **El tipo Unit:**
  <br>
  De forma predeterminada, si no se especifica un tipo de datos que se muestra, el predeterminado es `Unit`. `Unit` significa que la función no muestra ningún valor. `Unit` es equivalente a el tipo `void` en Java y C#.

  Ejemplo de código para funciones que no devuelven ningún valor:
   ```kotlin
   fun birthdayGreeting(): Unit { // El tipo de retorno es Unit
       println("Happy Birthday, Rover!")
       println("You are now 5 years old!")
   }
   ```

## 2. Argumentos con nombre
En Kotlin, puedes llamar a una función con muchos parámetros o pasar los argumentos en un orden diferente; por ejemplo, colocar el parámetro `edad` antes del parámetro `nombre`. Cuando incluyes un nombre de parámetro si llamas a una función, esta se denomina argumento con nombre.

```kotlin
fun main() {
    // El orden en que se pasan los parámetros es diferente,
    // pero el resultado es el mismo.
    println(saludoCumpleaños(nombre = "Juan", edad = 25))
    println(saludoCumpleaños(edad = 25, nombre = "Juan"))
}

fun saludoCumpleaños(nombre: String, edad: Int): String {
    val saludoNombre = "¡Feliz cumpleaños, $nombre!"
    val saludoEdad = "¡Ahora tienes $edad años!"
    return "$saludoNombre\n$saludoEdad"
} 
```
## 3. Argumentos predeterminados
Los parámetros de la función también pueden especificar argumentos predeterminados.Cuando llamas a una función, puedes decidir omitir los argumentos para los que haya un valor predeterminado, en cuyo caso se usa el predeterminado.

Para agregar un argumento predeterminado, agrega un operador de asignación (=) después del tipo de datos para el parámetro y configúralo del mismo modo que a un valor.

```kotlin
fun main() {
    // Si se llama a la función sin argumentos, se aplican los valores por defecto
    println(saludoCumpleaños())
}

// Los valores por defecto se asignan en la definición del parámetro.
fun saludoCumpleaños(nombre: String = "Juan", edad: Int = 25): String {
    val saludoNombre = "¡Feliz cumpleaños, $nombre!"
    val saludoEdad = "¡Ahora tienes $edad años!"
    return "$saludoNombre\n$saludoEdad"
}
```
## 4. Ejercicio: crear programas de cálculo básico utilizando funciones

### Ejercicio 1: Crear una función que sume dos números
1. Crea una función que reciba dos números enteros y devuelva su suma. El nombre de la función será `sumar`.
2. Imprime el resultado de ejecutar la función con la siguiente entrada utilizando `println()`.
   - Entrada: `sumar(5, 10)`
   - Salida: `15`

Pista: 
- Copia este código para empezar.
   ```kotlin
   fun sumar(a: Int, b: Int): Int {
       // TODO:Crear el cuerpo de la función
   }
   fun main() {
       println(sumar(5, 10))  // Salida: 15
   }
   ```
<details>
  <summary>Respuesta</summary>
   
   ```kotlin
   fun sumar(a: Int, b: Int): Int {
       return a + b
   }
   
   fun main() {
       println(sumar(5, 10))  // Salida: 15
   }
   ```

   **Explicación:**
   - La función `sumar` recibe dos enteros y devuelve su suma.
   - `a` y `b` son los parámetros de la función, y el resultado de la suma se devuelve con la palabra clave `return`.  
</details>

---

### Ejercicio 2: Crear una función que calcule el área de un círculo
1. Crea una función que reciba el radio de un círculo y devuelva su área. El nombre de la función será `calcularAreaCirculo`.
2. Imprime el resultado de ejecutar la función con la siguiente entrada utilizando `println()`.
   - Entrada: `calcularAreaCirculo(5.0)`
   - Salida: `78.53981633974483`

Pista:
- La fórmula para el área de un círculo es `π * radio * radio`. En Kotlin, puedes usar `Math.PI` para obtener el valor de π.
- Copia este código para empezar.
   ```kotlin
   fun calcularAreaCirculo(radio: Double): Double {
       // el valor de π
       val pi = Math.PI
       // TODO:La lógica de cálculo
   }
   
   fun main() {
       println(calcularAreaCirculo(5.0))  // Salida: 78.53981633974483
   }
   ```
<details>
  <summary>Respuesta</summary>
   
   ```kotlin
   fun calcularAreaCirculo(radio: Double): Double {
       // el valor de π
       val pi = Math.PI
       return pi * radio * radio
   }
   
   fun main() {
       println(calcularAreaCirculo(5.0))  // Salida: 78.53981633974483
   }
   ```
   **Explicación:**
   - La función `calcularAreaCirculo` toma el radio de un círculo como argumento y devuelve el área.
   - Se utiliza la fórmula `π * radio^2` para calcular el área, donde la variable `pi` proporciona el valor de π.
</details>

---

### Ejercicio 3: Crear una función que determine si un número es par o impar
1. Crea una función que reciba un número entero y devuelva un valor booleano. Devuelve `true` si el entero es par, `false` si es impar. El nombre de la función será `esPar`.
2. Imprime el resultado de ejecutar la función con la siguiente entrada utilizando `println()`.
   - Entrada: `esPar(7)`
   - Salida: `false`

<details>
  <summary>Respuesta</summary>
  
   ```kotlin
   fun esPar(numero: Int): Boolean {
       return numero % 2 == 0
   }
   
   fun main() {
       println(esPar(7))  // Salida: false
   }
   ```
   #### Explicación:
   - La función `esPar` determina si un número es par o impar.
   - Si el resto de la división del número entre 2 es 0, entonces es par; de lo contrario, es impar.
</details>

## Referencia：
- [Cómo crear y usar funciones en Kotlin](https://developer.android.com/codelabs/basic-android-kotlin-compose-functions?hl=es-419#0)
