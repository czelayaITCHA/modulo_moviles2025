## Introducción
   ¿Sabes qué es Kotlin? Descúbrelo y habla de Kotlin con los demás.
   1. ### Características y ventajas de Kotlin
      1. **Un lenguaje de programación moderno y fácil de usar:**
         <br>
         Kotlin es un lenguaje de programación moderno con una sintaxis concisa y fácil de entender. Esto te permite crear rápidamente programas que realmente funcionan sin tener que escribir código complejo. Puedes ver resultados inmediatos a medida que aprendes, haciendo que el aprendizaje sea divertido.
      1. **El lenguaje oficial de desarrollo de Android:**
         <br>
         Kotlin ha sido adoptado por Google como el lenguaje oficial para el desarrollo de aplicaciones Android, por lo que al aprender Kotlin, los estudiantes pueden adquirir habilidades directamente relacionadas con el desarrollo real de aplicaciones.
      1. **Un lenguaje popular y útil para tu carrera profesional:**
         <br>
         Kotlin crece en popularidad año tras año y está especialmente demandado en el sector del desarrollo de aplicaciones móviles. Las empresas también buscan ingenieros con conocimientos de Kotlin, lo que supone un gran activo en tu futura carrera profesional.
      1. **Buena comunidad y soporte:**
         <br>
         Kotlin tiene una comunidad activa y hay muchos recursos y soporte disponibles en caso de que tengas alguna pregunta o problema. Si tienes algún problema mientras aprendes, es fácil encontrar una solución y tu aprendizaje irá sobre ruedas, haciendo menos probable que te quedes atrás.

   1. ### Comparamos la sintaxis de Kotlin y C#
      La sintaxis de Kotlin y C# es más o menos similar.Aquí hay algunos ejemplos para algunas de las sintaxis.
      1. **Variables (Variables)**
         - **Kotlin** usa `val` para variables inmutables y `var` para variables mutables.
         - **C#** usa `var` para declarar variables con inferencia de tipos, pero las variables pueden ser reasignadas.
         #### Kotlin
         ```kotlin
         val nombre: String = "Juan"  // Inmutable
         var edad: Int = 25           // Mutable
         ```
         #### C#
         ```csharp
         const string nombre = "Juan";  // Inmutable
         int edad = 25;                 // Mutable
         ```
      
      1. **Funciones (Funciones)**
         - Ambas lenguas definen funciones de manera similar, pero la sintaxis varía.
         #### Kotlin
         ```kotlin
         fun saludar(nombre: String): String {
             return "Hola, $nombre"
         }
         ```
         #### C#
         ```csharp
         string Saludar(string nombre) {
             return $"Hola, {nombre}";
         }
         ```
      
      1. **Clases (Clases)**
         Ambos lenguajes usan una sintaxis similar para definir clases, aunque con ligeras diferencias.
         #### Kotlin
         ```kotlin
         class Persona(val nombre: String, var edad: Int)
         ```
         #### C#
         ```csharp
         class Persona {
             public string Nombre { get; }
             public int Edad { get; set; }
         
             public Persona(string nombre, int edad) {
                 Nombre = nombre;
                 Edad = edad;
             }
         }
         ```
      
      1. **Herencia de clases (Herencia)**
         Ambos lenguajes permiten herencia, pero Kotlin usa `open` para permitir que una clase sea heredada.
         #### Kotlin
         ```kotlin
         open class Animal
         class Perro: Animal()
         ```
         #### C#
         ```csharp
         class Animal {}
         class Perro : Animal {}
         ```
      
      1. **Null Safety (Seguridad ante null)**
         - **Kotlin** tiene un sistema de seguridad para `null` que obliga a manejar posibles valores `null` explícitamente.
         - **C#** también ofrece tipos anulables (`nullable`) usando `?` en los tipos.
         #### Kotlin
         ```kotlin
         val nombre: String? = null  // Tipo nullable
         ```
         #### C#
         ```csharp
         string? nombre = null;  // Tipo nullable
         ```
      
      1. **Bucles for (Bucles)**
         Los bucles `for` son muy parecidos en ambos lenguajes.
         #### Kotlin
         ```kotlin
         for (i in 1..10) {
             println(i)
         }
         ```
         #### C#
         ```csharp
         for (int i = 1; i <= 10; i++) {
             Console.WriteLine(i);
         }
         ```
   - Referencia
      - https://developer.android.com/codelabs/basic-android-kotlin-compose-first-program?hl=ja&continue=https%3A%2F%2Fdeveloper.android.com%2Fcourses%2Fpathways%2Fandroid-basics-compose-unit-1-pathway-1%3Fhl%3Dja%23codelab-https%3A%2F%2Fdeveloper.android.com%2Fcodelabs%2Fbasic-android-kotlin-compose-first-program#0

