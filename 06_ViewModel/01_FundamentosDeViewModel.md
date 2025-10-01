# Fundamentos de ViewModel

## Concepto básico

El `ViewModel` es una Clase que mantiene los datos y la lógica de la interfaz de usuario (UI). Su propósito es asegurarse de que los datos no se pierdan aunque la UI se reconstruya, promoviendo la separación entre la UI y los datos.
Dentro de una función componible, utiliza una instancia de `ViewModel` para acceder a los datos y a la lógica necesarios.

### Ventajas de ViewModel
- **Mantener datos durante la rotación o redibujado**:
  <br>Las pantallas de Android pueden destruirse y regenerarse durante eventos como la rotación del dispositivo o cambios en la configuración, pero el `ViewModel` no se ve afectado por estos eventos del ciclo de vida. Mientras que las variables o estados normales se restablecen cuando la pantalla se regenera, `ViewModel` mantiene los datos, por lo que, por ejemplo, los datos en un formulario de entrada no se pierden aunque se rote la pantalla durante la escritura.
- **Lógica reutilizable**:
  <br>Mantener el código de la interfaz de usuario simple y reutilizable consolidando la lógica de negocio y la recuperación de datos en el `ViewModel`.
- **Compartir datos entre múltiples pantallas**:
  <br>Con `ViewModel`, los datos se pueden reutilizar en varias pantallas o componentes. Por ejemplo, varias pantallas que muestran la misma información del usuario o configuraciones pueden utilizar un `ViewModel` común. Esto evita la necesidad de obtener y gestionar los datos en cada pantalla por separado.
- **Manejo seguro de tareas asíncronas**:
  <br>El `ViewModel` es útil para manejar procesos asíncronos (como el acceso a la base de datos o solicitudes de red). No depende del ciclo de vida de Android, por lo que puede continuar procesando de manera segura incluso si la pantalla se destruye o regenera. En combinación con `Coroutine` o `Flow`, se pueden gestionar tareas asíncronas de manera eficiente.

## Pasos para implementar
Vamos a probar el flujo básico de implementación de ViewModel mirando un poco de código de ejemplo.

### 0. Crea un nuevo proyecto llamado `EjemploViewModel`.
Crea un nuevo proyecto. Una vez compilado, elimina la función por defecto `Greeting` y su punto de llamada en la `MainActivity`.

### 1. **Agregar dependencias de ViewModel al proyecto**  
Primero, necesitas agregar la biblioteca para utilizar `ViewModel`. Añade lo siguiente a las dependencias en tu archivo `build.gradle.kts` (:app).

```diff
dependencies {
    ...

+    implementation("androidx.lifecycle:lifecycle-viewmodel-compose:2.6.1")
}
```

### 2. **Crear la clase ViewModel**  
A continuación, crea una clase `ViewModel` para mantener los datos necesarios. Consideremos el ejemplo de una aplicación de contador.

```kotlin
// Clase ViewModel
class ContadorViewModel : ViewModel() {
    // Mantener el estado del contador
    var contador by mutableStateOf(0)
        private set

    // Función para incrementar el contador
    fun incrementarContador() {
        contador++
    }
}
```
#### Explicación:
- Se crea la clase `ViewModel` y se define la variable entera `contador` utilizando `mutableStateOf`. Esto permite que Compose redibuje la interfaz automáticamente.
- La función `incrementarContador()` incrementa el valor de `contador`.

### 3. **Usar ViewModel en una función Composable**  
Ahora, utiliza `ViewModel` dentro de una función `Composable`. Se puede obtener una instancia del `ViewModel` usando la función `viewModel()`.
```kotlin
// Utilizar ViewModel dentro de una función Composable.
// Se puede obtener una instancia del ViewModel usando la función viewModel()
@Composable
fun PantallaContador(viewModel: ContadorViewModel = viewModel()) {
    Column(
        modifier = Modifier
            .fillMaxSize()
            .padding(16.dp),
        horizontalAlignment = Alignment.CenterHorizontally,
        verticalArrangement = Arrangement.Center
    ) {
        // Mostrar el valor del contador
        Text(text = "Contador: ${viewModel.contador}", fontSize = 32.sp)
        Spacer(modifier = Modifier.height(16.dp))

        // Botón para incrementar el contador
        Button(onClick = { viewModel.incrementarContador() }) {
            Text(text = "Incrementar Contador")
        }
    }
}
```
#### Explicación:
- Se usa `viewModel()` para obtener una instancia de `ContadorViewModel`.
- La función `Text` muestra el valor de `viewModel.contador` y el botón llama a `incrementarContador()` para incrementar el contador.
- Al usar la función `viewModel()` en Compose, el estado se mantiene incluso cuando se producen eventos como la rotación de la pantalla o el redibujado.

### 4. **Mostrar la función Composable que usa ViewModel en la aplicación**  
Finalmente, incluye la función `PantallaContador` en la actividad o en otras funciones `Composable` para mostrarla en la aplicación.
```kotlin
class MainActivity : ComponentActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        enableEdgeToEdge()
        setContent {
            EjemploViewModelTheme {
                PantallaContador()
            }
        }
    }
}
```
#### Explicación:
- Dentro de la clase `MainActivity`, se utiliza `setContent` para mostrar `PantallaContador`.
- Argumento por defecto `PantallaContador`, establecido para asignar el valor de retorno del método `viewModel()`. Por lo tanto, el constructor puede ser llamado sin argumentos.

### Resumen

- **Creación de ViewModel**: Se crea una clase `ViewModel` y se definen las variables de estado y funciones para operar sobre ellas.
- **Uso de ViewModel en Composable**: Se utiliza la función `viewModel()` para aprovechar el `ViewModel` dentro de las funciones `Composable`.
- **Retención de estado**: Usando `ViewModel`, el estado se mantiene incluso durante eventos del ciclo de vida como la rotación de la pantalla o redibujos.

## Comparación con código sin ViewModel
En los códigos siguientes, el valor del contador se pone a cero al girar la pantalla.
```kotlin
@Composable
fun PantallaContadorSinViewModel() {
    // Definir estado en lugar de ViewModel
    var contador by remember { mutableStateOf(0) }

    Column(
        modifier = Modifier
            .fillMaxSize()
            .padding(16.dp),
        horizontalAlignment = Alignment.CenterHorizontally,
        verticalArrangement = Arrangement.Center
    ) {
        // Mostrar el valor del contador
        Text(text = "Contador: ${contador}", fontSize = 32.sp)
        Spacer(modifier = Modifier.height(16.dp))

        // Botón para incrementar el contador
        Button(onClick = { contador++ }) {
            Text(text = "Incrementar Contador")
        }
    }
}
```

## Ejercicios
### Ejercicio1
Debajo del componente `Text` del contador, añada un componente `Text` que muestre el valor del contador de diez veces.

<details>
  <summary>Ejemplo de solución</summary>

  ```kotlin
  // Mostrar el valor del contador　multiplicado por diez
  Text(text = "Contador multiplicado por diez: ${viewModel.contador * 10}")
  ```
</details>

### Ejercicio2
- Debajo del botón para incrementar el contador, añade un botón para disminuir el contador.
- En el `ContadorViewModel`, añade también el método correspondiente.

<details>
  <summary>Ejemplo de solución</summary>

  ```kotlin
  // Botón para decrementar el contador
  Button(onClick = { viewModel.decrementarContador() }) {
      Text(text = "Decrementar Contador")
  }

  // Función para decrementar el contador
  fun decrementarContador() {
      contador--
  }
  ```
</details>

### Ejercicio3
- Debajo del botón del ejercicio2, añade un botón para reiniciar el contador.
- En el `ContadorViewModel`, añade también el método correspondiente.

<details>
  <summary>Ejemplo de solución</summary>

  ```kotlin
  // Botón para reiniciar el contador
  Button(onClick = { viewModel.reiniciarContador() }) {
      Text(text = "Reiniciar Contador")
  }

  // Función para reiniciar el contador
  fun reiniciarContador() {
      contador = 0
  }
  ```
</details>

**Ejemplos de diseño:**

![image](https://github.com/user-attachments/assets/ba4b4b2f-1dc4-4f36-9817-ad48b59edb62)
