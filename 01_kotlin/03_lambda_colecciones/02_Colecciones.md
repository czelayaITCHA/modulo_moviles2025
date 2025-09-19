# **Estructura básica de las colecciones en Kotlin**

## 1. Tipos de colecciones
En Kotlin, hay tres tipos principales de colecciones:
- **List**: Una lista que mantiene el orden de los elementos y permite duplicados.
- **Set**: Un conjunto que no tiene orden y no permite elementos duplicados.
- **Map**: Un mapa que contiene pares clave-valor, donde las claves no se pueden duplicar, pero los valores sí.

## 2. List (Lista)
Una lista es una colección que puede contener elementos duplicados y mantiene el orden de los mismos.
### **Cómo crear una lista**
```kotlin
val lista = listOf("manzana", "plátano", "naranja")
val listaMutable = mutableListOf("manzana", "plátano", "naranja")
```
- `listOf` crea una lista inmutable (no se puede modificar).
- `mutableListOf` crea una lista mutable (se puede modificar).
  
### **Ejemplos de operaciones con listas**
```kotlin
val primeraFruta = lista[0]  // Obtiene el primer elemento ("manzana")
val tamaño = lista.size      // Obtiene el tamaño de la lista

listaMutable.add("uva")      // Añade un elemento a la lista mutable
listaMutable.remove("plátano") // Elimina un elemento de la lista mutable
```

## 3. Set (Conjunto)
Un conjunto es una colección que no mantiene un orden específico y no permite duplicados.
### **Cómo crear un conjunto**
```kotlin
val conjunto = setOf("manzana", "plátano", "naranja")
val conjuntoMutable = mutableSetOf("manzana", "plátano", "naranja")
```
- `setOf` crea un conjunto inmutable.
- `mutableSetOf` crea un conjunto mutable.

### **Ejemplos de operaciones con conjuntos**
```kotlin
val contieneManzana = conjunto.contains("manzana") // Verifica si el conjunto contiene "manzana"

conjuntoMutable.add("uva")     // Añade un elemento al conjunto mutable
conjuntoMutable.remove("plátano") // Elimina un elemento del conjunto mutable
```
## 4. Map (Mapa)
Un mapa es una colección que asocia claves únicas con valores.
### **Cómo crear un mapa**
```kotlin
val mapa = mapOf(1 to "manzana", 2 to "plátano", 3 to "naranja")
val mapaMutable = mutableMapOf(1 to "manzana", 2 to "plátano", 3 to "naranja")
```
- `mapOf` crea un mapa inmutable.
- `mutableMapOf` crea un mapa mutable.

### **Ejemplos de operaciones con mapas**
```kotlin
val valor1 = mapa[1]          // Obtiene el valor asociado con la clave 1 ("manzana")
val tamañoMapa = mapa.size     // Obtiene el tamaño del mapa

mapaMutable[4] = "uva"         // Añade un nuevo par clave-valor
mapaMutable.remove(2)          // Elimina el par con clave 2
```
## 5. Minicuestionario
### **1. ¿Cuál es la forma correcta de crear una lista mutable en Kotlin?**
a) `val lista = listOf("manzana", "plátano", "naranja")`

b) `val lista = mutableListOf("manzana", "plátano", "naranja")`

c) `val lista = setOf("manzana", "plátano", "naranja")`

d) `val lista = mutableSetOf("manzana", "plátano", "naranja")`

### **2. ¿Cómo puedes verificar si un conjunto contiene un elemento específico en Kotlin?**
a) `conjunto.has("elemento")`

b) `conjunto.contains("elemento")`

c) `conjunto.includes("elemento")`

d) `conjunto.in("elemento")`

### **3. ¿Cuál de las siguientes afirmaciones es correcta sobre los mapas en Kotlin?**
a) Las claves en un mapa pueden estar duplicadas, pero los valores no.

b) Los valores en un mapa pueden estar duplicados, pero las claves no.

c) Las claves y los valores en un mapa pueden estar duplicados.

d) Los mapas no permiten duplicados ni en claves ni en valores.

### **4. ¿Cuál es la sintaxis para añadir un nuevo par clave-valor a un mapa mutable en Kotlin?**
a) `map.put(4, "nuevoValor")`

b) `map.add(4 to "nuevoValor")`

c) `map[4] = "nuevoValor"`

d) `map.insert(4, "nuevoValor")`

### **5. ¿Cómo puedes eliminar un elemento específico de una lista mutable en Kotlin?**
a) `lista.remove("elemento")`

b) `lista.delete("elemento")`

c) `lista.erase("elemento")`

d) `lista.discard("elemento")`

### **6. Crea una lista inmutable con los siguientes elementos: "rojo", "verde", "azul". Luego, obtén el tamaño de la lista y el primer elemento.**

<details>
  <summary>Respuesta</summary>
  
  ```kotlin
  val colores = listOf("rojo", "verde", "azul")
  val tamaño = colores.size
  val primerColor = colores[0]
  ```
</details>

### **7. Dado un conjunto mutable con los elementos "manzana" y "plátano", ¿cómo añadirías el elemento "naranja" y eliminarías "manzana"?**

<details>
  <summary>Respuesta</summary>
  
  ```kotlin
  val frutas = mutableSetOf("manzana", "plátano")
  frutas.add("naranja")
  frutas.remove("manzana")
  ```
</details>

### **8. ¿Cómo crearías un mapa mutable con las claves 1, 2, 3 y los valores "uno", "dos", "tres"? Luego, cambiarías el valor asociado con la clave 2 a "dosModificado".**

<details>
  <summary>Respuesta</summary>
  
  ```kotlin
  val numeros = mutableMapOf(1 to "uno", 2 to "dos", 3 to "tres")
  numeros[2] = "dosModificado"
  ```
</details>

## 6. Ejercicios

### **Ejercicio 1: Operaciones con listas**
Cree una lista que cumpla con los siguientes requisitos y realice las operaciones indicadas.
1. Cree una lista con "manzana", "plátano", y "naranja".
2. Obtenga el primer y último elemento de la lista.
3. Añada "uva" a la lista.

### **Ejercicio 2: Operaciones con conjuntos**
Cree un conjunto que cumpla con los siguientes requisitos y realice las operaciones indicadas.
1. Cree un conjunto con "perro", "gato", y "pájaro".
2. Añada "pez" al conjunto.
3. Elimine "gato" del conjunto.

### **Ejercicio 3: Operaciones con mapas**
Cree un mapa que cumpla con los siguientes requisitos y realice las operaciones indicadas.
1. Cree un mapa donde la clave 1 esté asociada a "rojo", la clave 2 a "azul" y la clave 3 a "verde".
2. Cambie el valor asociado con la clave 2 a "amarillo".
3. Elimine la clave 3 del mapa.
