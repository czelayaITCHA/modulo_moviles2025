# Variable

Aprende la sintaxis ejecutando código de ejemplo y ejercicios en el `Kotlin Playground`.

Kotlin Playground: https://play.kotlinlang.org/

## 1. Declaración de variable
Para definir una nueva variable, comienza con la palabra clave `val` o `var` de Kotlin.
- Palabra clave `val`: Úsala cuando esperes que el valor de la variable no cambie.
- Palabra clave `var`: Úsala cuando esperes que el valor de la variable pueda cambiar.

Código de ejemplo:
```kotlin
val nombre: String = "Juan"  // Inmutable
var edad: Int = 25           // Mutable
```

Sintaxis:

![image](https://github.com/user-attachments/assets/3287d540-ef9c-471a-92bf-835fcaad40e8)

## 2. Los tipos de datos (`Int`, `String`, `Boolean`, etc.)
Los tipos de datos básicos en Kotlin se muestran en la siguiente tabla.

![image](https://github.com/user-attachments/assets/2605e4c1-4a5a-40e4-b65f-34ea95a43647)

- **Plantillas de strings:**
  <br>
  En Kotlin, las plantillas de strings permiten incrustar variables o expresiones directamente dentro de una cadena de texto utilizando el símbolo `$`. Si la expresión es más compleja, se encierra entre llaves `{}`.
  <br>
   ```kotlin
   val nombre: String = "Juan"  // Inmutable
   var edad: Int = 25           // Mutable

   // $nombre inserta el valor de la variable nombre en la cadena.
   // $edad inserta el valor de la variable edad en la cadena.
   println("$nombre tiene $edad años.")

   // ${edad + 1} evalúa la expresión antes de insertarla.
   println("El año que viene $nombre cumplirá ${edad + 1} años.")
   ```
- **Inferencia de tipo:**
  <br>
  La inferencia de tipo es cuando el compilador de Kotlin puede inferir (o determinar) qué tipo de datos debe ser una variable, sin que el tipo se escriba de manera explícita en el código. Eso significa que puedes omitir el tipo de datos en una declaración de variable si proporcionas un valor inicial para ella.

  Pruebe a omitir el tipo de datos en el código de ejemplo para comprobar que no se produce ningún error.
  ```kotlin
   val nombre = "Juan"  // String
   var edad = 25           // Int
  ```
   1. ### Convención de codificación
      Algunas convenciones de codificación recomendadas por Google.

      - Los nombres de las variables deben seguir la convención de mayúsculas y minúsculas, y comenzar con una letra minúscula.
      - En una declaración de variable, debes especificar un espacio después de los dos puntos cuando se especifica el tipo de datos.

        ![image](https://github.com/user-attachments/assets/16f6db52-a936-4ee4-94fa-a615949d9ace)

      - Debe haber un espacio antes y después de un operador como la asignación (=), la suma (+), la resta (-), la multiplicación (*) la división (/) y muchos más.

        ![image](https://github.com/user-attachments/assets/c9a816c4-eb0a-4bf0-9fad-f0db4efd306b)

## 3. Ejercicio: definir algunas variables y constantes y realizar operaciones

### Ejercicio 1: Operaciones aritméticas básicas
1. Define una variable inmutable `carrera` de tipo String y asígna la cadena de `Computación`.
1. Define una variable mutable `grado` de tipo Int y asígna el numero de `2`.
1. Usando las variables, muestra `Soy 2do grado de Computación.` en el consola.

<details>
  <summary>Respuesta</summary>
  
  ```kotlin
  fun main() {
      val carrera: String = "Computación"  // Inmutable
      var grado: Int = 2  // Mutable
  
      // ${grado} inserta el valor de la variable carrera en la cadena.
      // $carrera inserta el valor de la variable carrera en la cadena.
      println("Soy ${grado}do año de $carrera.")
  }
  ```
</details>

### Ejercicio 2: Operaciones aritméticas básicas
1. Define tres variables `a`, `b`, `c` y asígnales cualquier número entero.
2. Luego, usa estas variables para realizar los siguientes cálculos y muestra los resultados en la consola:
   - a + b + c
   - a * b - c
   - (a + b) / c
   - b % a

<details>
  <summary>Respuesta</summary>
  
   ```kotlin
   fun main() {
       // Asignar valores enteros a 3 variables
       val a = 10
       val b = 5
       val c = 2
   
       // Realizar varias operaciones y mostrar los resultados
       println("a + b + c = ${a + b + c}") // 10 + 5 + 2 = 17
       println("a * b - c = ${a * b - c}") // 10 * 5 - 2 = 48
       println("(a + b) / c = ${(a + b) / c}") // (10 + 5) / 2 = 7
       println("b % a = ${b % a}") // 5 % 10 = 5
   }
   ```
   
   **Explicación:**
   - Se asignan valores a las variables `a`, `b`, y `c`, y se realizan operaciones aritméticas básicas.
   - Los resultados de las operaciones se muestran en la consola utilizando la función `println`.
</details>

### Ejercicio 3: Cálculo de la media
1. Define cinco variables `x1`, `x2`, `x3`, `x4`, `x5` y asígnales cualquier número entero.
2. Calcula la media de estos cinco valores y muestra el resultado en la consola.

<details>
  <summary>Respuesta</summary>
  
   ```kotlin
   fun main() {
       // Definir 5 números enteros
       val x1 = 10
       val x2 = 20
       val x3 = 30
       val x4 = 40
       val x5 = 50
   
       // Calcular el promedio
       val promedio = (x1 + x2 + x3 + x4 + x5) / 5
   
       // Mostrar el resultado
       println("Promedio: $promedio") // Salida: Promedio: 30
   }
   ```
   
   **Explicación:**
   - Se calculan los promedios de 5 números enteros.
   - La suma de los números se divide por 5 y se muestra el resultado.
</details>


## Referencia:
- [Crea y usa variables en Kotlin](https://developer.android.com/codelabs/basic-android-kotlin-compose-variables?hl=es-419#0)
