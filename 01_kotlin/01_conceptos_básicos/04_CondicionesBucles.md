# Condiciones y bucles
## 1. Cómo usar sentencias if/else para expresar condiciones
En Kotlin, las sentencias `if/else` funcionan de manera similar a otros lenguajes. A continuación te muestro cómo se usan las sentencias `if/else` en diferentes contextos.
1. **Uso básico de `if/else`**
   <br>
   Este es el uso más común para verificar una condición:
   ```kotlin
   val numero = 10

   if (numero > 5) {
       println("El número es mayor que 5")
   } else {
       println("El número es menor o igual a 5")
   }
   ```

1. **Anidación de `if/else`**
   <br>
   Puedes anidar múltiples condiciones `if/else` para manejar más casos:
   ```kotlin
   val numero = 10

   if (numero > 10) {
       println("El número es mayor que 10")
   } else if (numero == 10) {
       println("El número es igual a 10")
   } else {
       println("El número es menor que 10")
   }
   ```

1. **Condiciones más complejas**
   <br>
   Puedes combinar varias condiciones en un `if` utilizando operadores lógicos como `&&` (y) y `||` (o):
   ```kotlin
   val numero = 10

   if (numero > 5 && numero < 15) {
       println("El número está entre 5 y 15")
   }
   ```
   ```kotlin
   val edad = 20

   if (edad < 18 || edad > 65) {
       println("Eres menor de 18 o mayor de 65 años.")
   } else {
       println("Tienes entre 18 y 65 años.")
   }
   ```
### Minicuestionario

#### Pregunta 1:
¿Cuál será la salida del siguiente código?
```kotlin
val number = 7
val result = if (number % 2 == 0) {
    "Even"
} else {
    "Odd"
}
println(result)
```

1. `Even`
2. `Odd`
3. `Error`

<details>
  <summary>Respuesta</summary>
  
   2. `Odd`
   
   **Explicación:**
   - La condición `number % 2 == 0` verifica si el número es divisible por 2. Dado que 7 no es divisible por 2, la condición es falsa y se ejecuta el bloque `else`, que asigna "Odd" a `result`.
</details>

---

#### Pregunta 2:
Completa el siguiente código en Kotlin para que muestre "Adult" si el valor de `age` es mayor o igual a 18, y "Minor" en caso contrario.
```kotlin
val age = 20
if (age >= 18) {
    // Añade aquí el código
} else {
    // Añade aquí el código
}
```

<details>
  <summary>Respuesta</summary>
  
   ```kotlin
   val age = 20
   if (age >= 18) {
       println("Adult")
   } else {
       println("Minor")
   }
   ```
   
   **Explicación:**
   - La función verifica si `age` es mayor o igual a 18. Si es verdadero, imprime "Adult"; de lo contrario, imprime "Minor".
</details>

---

#### Pregunta 3:
Crea una función `checkPass` que muestre "Pass" si el `score` es 60 o más, y "Fail" en caso contrario.
```kotlin
fun checkPass(score: Int) {
    // Añade aquí el código
}
```

<details>
  <summary>Respuesta</summary>
  
   ```kotlin
   fun checkPass(score: Int) {
       if (score >= 60) {
           println("Pass")
       } else {
           println("Fail")
       }
   }
   ```
   
   **Explicación:**
   - La función `checkPass` recibe un parámetro `score`. Dependiendo de si `score` es mayor o igual a 60, imprime "Pass" o "Fail".
</details>

## 2. Cómo usar una sentencia when para varias ramas
En Kotlin, la sentencia `when` es una estructura de control muy flexible que se utiliza para manejar múltiples condiciones.
Puede ser usada como un reemplazo más potente y expresivo de las sentencias `switch` en otros lenguajes.
A continuación te explico cómo usar `when` con varios casos.
### Sintaxis básica de `when`
```kotlin
val numero = 3

when (numero) {
    1 -> println("El número es 1")
    2 -> println("El número es 2")
    3 -> println("El número es 3")
    else -> println("El número no es 1, 2, ni 3")
}
```
- `when (numero)` evalúa la variable `numero`.
- Cada rama del `when` compara el valor de `numero` con el valor especificado (`1`, `2`, `3`).
- `else` es una rama opcional que se ejecuta si ninguna de las otras condiciones es verdadera.

1. **Ejemplo con múltiples valores por rama**
   <br>
   Puedes manejar múltiples valores en una sola rama:
   ```kotlin
   val dia = 3

   when (dia) {
       1, 2, 3 -> println("Es el inicio de la semana")
       4, 5 -> println("Es el final de la semana laboral")
       6, 7 -> println("Es el fin de semana")
       else -> println("Día no válido")
   }
   ```

1. **Ejemplo con rangos**
   <br>
   También puedes usar rangos usando la palabla clave `in` en las condiciones de `when`:
   ```kotlin
   val edad = 25

   when (edad) {
       in 0..12 -> println("Eres un niño")
       in 13..19 -> println("Eres un adolescente")
       in 20..64 -> println("Eres un adulto")
       else -> println("Eres un anciano")
   }
   ```

1. **Ejemplo con expresiones booleanas**
   <br>
   En lugar de valores simples, puedes usar expresiones booleanas para las ramas:
   ```kotlin
   val numero = 15

   when {
       numero % 2 == 0 -> println("El número es par")
       numero % 2 != 0 -> println("El número es impar")
       else -> println("Número desconocido")
   }
   ```

1. **Ejemplo con tipo de dato**
   <br>
   `when` también puede manejar el tipo de datos de una variable usando la palabla clave `is`:
   ```kotlin
   fun describir(objeto: Any) {
       when (objeto) {
           is String -> println("Es una cadena de texto")
           is Int -> println("Es un entero")
           else -> println("Tipo desconocido")
       }
   }

   describir("Hola")   // Salida: Es una cadena de texto
   describir(42)       // Salida: Es un entero
   describir(3.14)     // Salida: Tipo desconocido
   ```

### Minicuestionario

#### Pregunta 1:
¿Cuál será la salida de la siguiente declaración `when`?
```kotlin
val x = 2
when (x) {
    1 -> println("Uno")
    2 -> println("Dos")
    3 -> println("Tres")
    else -> println("Desconocido")
}
```
1. Uno  
2. Dos  
3. Tres  
4. Desconocido


<details>
  <summary>Respuesta</summary>
  
   2. Dos
   
   **Explicación:**
   -La variable `x` tiene el valor de 2. El bloque `when` evalúa `x` y encuentra el caso `2`, por lo que ejecuta `println("Dos")`. Si no hubiera coincidencia con los valores 1, 2 o 3, se habría ejecutado el bloque `else`.
</details>


---

#### Pregunta 2:
¿Cuál será la salida de la siguiente declaración `when`?
```kotlin
val color = "azul"
when (color) {
    "rojo" -> println("El color es rojo")
    "azul" -> println("El color es azul")
    "verde" -> println("El color es verde")
    else -> println("Color desconocido")
}
```
1. El color es rojo  
2. El color es azul  
3. El color es verde  
4. Color desconocido

<details>
  <summary>Respuesta</summary>
  
   2. El color es azul
   
   **Explicación:**
   - La variable `color` tiene el valor "azul". El bloque `when` evalúa los casos y encuentra una coincidencia con "azul", por lo que imprime "El color es azul". Si el valor de `color` hubiera sido diferente a "rojo", "azul" o "verde", se habría ejecutado el bloque `else`.

</details>

---

#### Pregunta 3:
Usa `when` para completar el siguiente código que cumpla con las siguientes condiciones:
- Si el número es 0, imprime "Cero".
- Si el número está entre 1 y 5, imprime "Número pequeño".
- En cualquier otro caso, imprime "Número grande".

```kotlin
val numero = 4
when (numero) {
    _______________ -> println("Cero")
    in _______________ -> println("Número pequeño")
    else -> println("Número grande")
}
```

<details>
  <summary>Respuesta</summary>
  
   **Código Completo:**
   ```kotlin
   val numero = 4
   when (numero) {
       0 -> println("Cero")
       in 1..5 -> println("Número pequeño")
       else -> println("Número grande")
   }
   ```
   **Explicación:**
   - La declaración `when` evalúa `numero`. Si el número es 0, imprime "Cero". Si está en el rango de 1 a 5 (usando la expresión `in 1..5`), imprime "Número pequeño". Para cualquier otro valor fuera de este rango, se ejecuta el bloque `else` con el mensaje "Número grande".

</details>

---

#### Pregunta 4:
1. Usa `when` para completar el siguiente función `determinarLongitud` que imprima un mensaje basado en la longitud de una cadena de texto:
   ```kotlin
   fun determinarLongitud(texto: String): Unit {
      val longitud: Int = texto.length
      // TODO:Crear el cuerpo de la función usando `when`
   }
   
   fun main() {
      determinarLongitud("hola")  // Salida: Corto
   }
   ```
1. las condiciones del bloque `when` deben ser:
   - Si la longitud es 0, imprime "Vacío".
   - Si la longitud está entre 1 y 5, imprime "Corto".
   - Si la longitud está entre 6 y 10, imprime "Medio".
   - Si la longitud es mayor, imprime "Largo".

<details>
  <summary>Respuesta</summary>
  
   **Código Completo:**
   ```kotlin
   fun determinarLongitud(texto: String): Unit {
       val longitud: Int = texto.length
       when (longitud) {
           0 -> println("Vacío")
           in 1..5 -> println("Corto")
           in 6..10 -> println("Medio")
           else -> println("Largo")
       }
   }
   
   fun main() {
      determinarLongitud("hola")  // Salida: Corto
   }
   ```

   **Explicación:**
   - El método length devuelve la longitud de la cadena texto.
   - Si la longitud es 0, imprime "Vacío". Si la longitud está en el rango de 1 a 5, imprime "Corto". Para longitudes entre 6 y 10, imprime "Medio", y para cualquier valor mayor a 10, se ejecuta el bloque else que imprime "Largo". En este caso, la longitud de "hola" es 4, por lo que se imprime "Corto".

</details>

---

## 3. Cómo usar if/else y when como expresiones
En Kotlin, tanto `if/else` como `when` pueden usarse como **expresiones**, lo que significa que pueden devolver un valor y asignarse a una variable. Esto los hace más flexibles y elegantes que en otros lenguajes.
La sintaxis de los condicionales como expresiones es similar a las de las sentencias, pero la última línea de cuerpos de cada rama debe mostrar un valor.

1. **`if/else` como expresión**
   <br>
   Cuando utilizas `if/else` como expresión, puedes asignar directamente el valor resultante a una variable. Aquí te muestro cómo hacerlo:

   **Ejemplo con `if/else`:**
   ```kotlin
   val edad = 18

   val estado = if (edad >= 18) {
       "Mayor de edad"
   } else {
       "Menor de edad"
   }

   println(estado)  // Salida: Mayor de edad
   ```
   - Aquí, el resultado de la expresión `if/else` se asigna a la variable `estado`.
   - Si la condición `edad >= 18` es verdadera, se asigna `"Mayor de edad"`. Si es falsa, se asigna `"Menor de edad"`.

1. **`when` como expresión**
   <br>
   De manera similar, `when` también puede devolver un valor. Esto es útil cuando tienes varias condiciones y quieres que la expresión devuelva diferentes valores.

   **Ejemplo con `when`:**
   ```kotlin
   val dia = 3

   val mensaje = when (dia) {
       1 -> "Lunes"
       2 -> "Martes"
       3 -> "Miércoles"
       4 -> "Jueves"
       5 -> "Viernes"
       6, 7 -> "Fin de semana"
       else -> "Día no válido"
   }

   println(mensaje)  // Salida: Miércoles
   ```
   - Aquí, `when` devuelve un valor dependiendo del valor de `dia`, y ese valor se asigna a la variable `mensaje`.

## 4. Cómo utilizar los bucles
En Kotlin, al igual que en otros lenguajes, puedes usar bucles para repetir bloques de código. Los bucles más comunes en Kotlin son `for`, `while`.

Voy a explicarte cómo se usan cada uno de ellos con ejemplos.

1. **Bucle `for`**
   <br>
   El bucle `for` en Kotlin se utiliza para recorrer rangos, colecciones (como listas o arreglos), y otras estructuras iterables.

   **Ejemplo: Iterar sobre un rango de números**
   ```kotlin
   for (i in 1..5) {
       println(i)
   }
   ```
   - Esto imprimirá los números del 1 al 5.
   - `1..5` es un rango que incluye tanto el 1 como el 5.

   **Comparación con el código en C#:**
   ```csharp
   for (int i = 1; i <= 5; i++)
   {
       Console.WriteLine(i);
   }
   ```
   
   **Ejemplo: Recorrer una lista**
   ```kotlin
   // Función para crear una instancia de List
   val frutas = listOf("Manzana", "Banana", "Cereza")

   for (fruta in frutas) {
       println(fruta)
   }
   ```
   - Este código imprimirá cada elemento de la lista `frutas`.

   **Comparación con el código en C#:**
   ```csharp
   var frutas = new List<string> { "Manzana", "Banana", "Cereza" };
   
   foreach (var fruta in frutas)
   {
       Console.WriteLine(fruta);
   }
   ```

2. **Bucle `while`**
   <br>
   El bucle `while` continúa ejecutándose mientras una condición sea verdadera.

   **Ejemplo: Usar `while`**
   ```kotlin
   var contador = 5

   while (contador > 0) {
       println(contador)
       contador--
   }
   ```
   - Este bucle imprimirá los números del 5 al 1, disminuyendo el valor de `contador` en cada iteración.

### Minicuestionario

#### Pregunta 1:
¿Qué número imprimiría el siguiente código?
   ```kotlin
   for (i in 6..10) {
       println(i)
   }
   ```
<details>
  <summary>Respuesta</summary>
   
   Esto imprimirá los números del 6 al 10.
   
   **Explicación:**
   - `6..10` es un rango que incluye tanto el 6 como el 10.
</details>

#### Pregunta 2:
¿Qué imprimiría el siguiente código?
   ```kotlin
   val meses = listOf("Septiembre", "Octubre", "Noviembre", "Diciembre")

   for (mes in meses) {
       println(mes)
   }
   ```
<details>
  <summary>Respuesta</summary>
   
   Esto imprimirá los meses de Septiembre, Octubre, Noviembre y Diciembre..
</details>

## 5. Ejercicios opcionales
Para seguir practicando, prueba los ejercicios de la web oficial de Android.
- [Problemas prácticos:Conceptos básicos de Kotlin](https://developer.android.com/codelabs/basic-android-kotlin-compose-intro-kotlin-practice-problems?continue=https%3A%2F%2Fdeveloper.android.com%2Fcourses%2Fpathways%2Fandroid-basics-compose-unit-1-pathway-1%3Fhl%3Dja&hl=es-419#0)

## 6. 
