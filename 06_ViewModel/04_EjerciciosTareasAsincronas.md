# Ejercicios: Implementar procesamiento asíncrono en ViewModel

## Objetivo
* Aprender a usar Kotlin Coroutines con `ViewModel` para obtener datos de manera asíncrona y reflejarlos en la interfaz de usuario.

## Ejercicio1

### Preparación previa: obtención de los códigos iniciales

En Android Studio, haga clic en `File`→`New`→`Project from Version Control`.
> ![image](https://github.com/user-attachments/assets/e92fbaa1-eafe-4f5f-a004-608d71bffbe1)

Introduzca lo siguiente en el campo URL y haga clic en `Clone`.
<br>
https://github.com/itcha-organization/ejercicios-asincrono1
> ![image](https://github.com/user-attachments/assets/42dc53e8-789f-4417-8546-41121502897a)

### 🔧 Paso 1: Preparar una función `suspend` para el procesamiento asíncrono
**Tarea:**
1. Cambia la función `cargarDatos(): String` para que sea una función `suspend`.
2. Simula una espera de 2 segundos usando `delay(2000)`.

**Pista:**
* Necesitas importar `kotlinx.coroutines.delay`.

### 🔧 Paso 2: Ejecutar la función `suspend` usando `viewModelScope.launch`
**Tarea:**
1. Usa `viewModelScope.launch { ... }` dentro de `obtenerDatos()` para llamar a la función `suspend` `cargarDatos()`.

### 🔧 Paso 3: Notificar el estado a la UI usando `StateFlow`
**Tarea:**
1. Crea una variable privada `_mensaje` de tipo `MutableStateFlow` y edita su valor en lugar de lo de `mensaje` en el ViewModel.
2. Cambia la variable `mensaje` a un `StateFlow`.
3. En la UI, utiliza `mensaje.collectAsState()` para mostrar el mensaje actualizado.

## Ejercicio2

### Preparación previa: obtención de los códigos iniciales
En Android Studio, haga clic en `File`→`New`→`Project from Version Control`.

Introduzca lo siguiente en el campo URL y haga clic en `Clone`.
<br>
https://github.com/itcha-organization/ejercicios-asincrono2

### 🔧 Paso 1: Preparar una función `suspend` para el procesamiento asíncrono y ejecutar la función `suspend` usando `viewModelScope.launch`
**Tarea:**
1. Declara `cargarDatos()` como función `suspend` que simula una espera de 2 segundos (`delay(2000)`) y luego devuelve una lista de 3 elementos de texto.

**Pista:**
* Necesitas importar `kotlinx.coroutines.delay`.

### 🔧 Paso 2: Ejecutar la función `suspend` usando `viewModelScope.launch`
**Tarea:**
1. Cambia la función `obtenerDatos()` para que use `viewModelScope.launch` y llame a una función `suspend` llamada `cargarDatos()`.

### 🔧 Paso 3: Notificar el estado a la UI usando `StateFlow`
**Tarea:**
1. Crea una variable privada `_lista` de tipo `MutableStateFlow` y edita su valor en lugar de lo de `lista` en el ViewModel.
2. Cambia la variable `lista` a un `StateFlow`.
3. En la UI, utiliza `collectAsState()` para obtener dato como `State`.
