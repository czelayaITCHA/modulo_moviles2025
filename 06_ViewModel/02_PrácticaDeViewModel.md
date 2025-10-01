# Práctica:Compartir una Lista en varias pantallas usando ViewModel

Creamos una aplicación que comparta una lista de texto entre dos pantallas utilizando un `ViewModel`, como se muestra a continuación.

![image](https://github.com/user-attachments/assets/9d7794a7-5487-432d-a551-f2a950bb77ba)

## Creación de paquetes y archivos.
- Crear los paquetes `components`, `navigation` y `viewmodels`.
- Crear los archivos `PrimeraPantalla` y `SegundaPantalla` en el paquete `components`.
- Crear los archivos `NavManager` en el paquete `navigation`.
- Crear los archivos `VistaCompartidaViewModel` en el paquete `viewmodels`.

![image](https://github.com/user-attachments/assets/1b91006c-35ef-486d-aa86-6fc10fca7ce7)

## Crear la clase ViewModel
Deje la declaración del paquete en la línea 1.
Pegue el siguiente código en el archivo que ha creado.
```kotlin
import androidx.compose.runtime.getValue
import androidx.compose.runtime.mutableStateOf
import androidx.compose.runtime.setValue
import androidx.lifecycle.ViewModel

class VistaCompartidaViewModel : ViewModel() {

    var textos by mutableStateOf(listOf<String>())
        private set

    fun addTexto(nuevoTexto: String) {
        textos = textos + nuevoTexto
    }
}
```
Este código es un ejemplo simple de cómo usar `ViewModel` en Jetpack Compose para gestionar una lista de datos (una lista de cadenas de texto) y agregar nuevos datos. Aquí se explica cada parte en detalle.

### Definición de la clase: `VistaCompartidaViewModel`
```kotlin
class VistaCompartidaViewModel : ViewModel() {
```
- **`VistaCompartidaViewModel`**: Esta clase hereda de `ViewModel`. El `ViewModel` mantiene el estado de la pantalla y evita que se pierda durante eventos del ciclo de vida, como la rotación de la pantalla o la recreación de la interfaz de usuario. En este caso, esta clase mantiene una lista de cadenas de texto (`textos`).

### Variable: `textos`
```kotlin
    var textos by mutableStateOf(listOf<String>())
        private set
```

- **`var textos`**: Esta es una variable que almacena una lista de cadenas de texto (`List<String>`) y contiene los datos de texto que se muestran en la UI. La variable `textos` se define usando **MutableState**.
- **`by mutableStateOf`**: `mutableStateOf` permite que Compose detecte los cambios en la variable `textos` y redibuje automáticamente la UI cuando su valor cambie. Aquí, `textos` se inicializa como una lista vacía (`listOf<String>()`).
- **`private set`**: El modificador `private set` hace que la variable solo pueda ser modificada dentro del `ViewModel`, evitando que el estado se modifique directamente desde fuera de la clase. Esto asegura que la gestión del estado sea segura y controlada.

### Método: `addTexto`
```kotlin
    fun addTexto(nuevoTexto: String) {
        textos = textos + nuevoTexto
    }
```

- **`addTexto`**: Esta función toma como parámetro un nuevo texto (`nuevoTexto`) y lo agrega a la lista `textos`.
- **`textos = textos + nuevoTexto`**: En Kotlin, el operador `+` se puede usar para añadir elementos a una lista. En este caso, `nuevoTexto` se añade a la lista `textos` existente, creando una nueva lista con el valor añadido. Aunque las listas en Kotlin son inmutables, se está generando una nueva lista y asignándola a `textos`.

### Flujo general

1. La clase `VistaCompartidaViewModel` hereda de `ViewModel` para mantener el estado de la pantalla.
2. La variable `textos` contiene una lista de cadenas de texto. Al usar `mutableStateOf`, Compose puede redibujar la UI automáticamente cuando se cambia esta lista.
3. Aunque `textos` no se puede modificar directamente desde fuera del `ViewModel`, la función `addTexto` permite agregar nuevos textos a la lista de manera controlada.

### Ejemplo de uso

Este `ViewModel` puede ser utilizado en una aplicación que gestione mensajes o notas. Cada vez que un usuario introduce un nuevo texto, se agrega a la lista usando `addTexto`, y la UI se actualiza automáticamente para reflejar el nuevo contenido.

### Resumen

- El uso de `ViewModel` permite gestionar de manera eficiente el estado de la UI en Compose, asegurando que los datos se mantengan incluso durante la rotación de pantalla o la recreación de la interfaz.
- La variable `textos` es inmutable desde fuera del `ViewModel`, lo que garantiza una gestión segura del estado.
- La función `addTexto` permite agregar nuevos textos a la lista de manera controlada, asegurando que la UI se actualice automáticamente cuando los datos cambian.

## Configurar la navegación para mostrar las funcies componibles que usan `ViewModel` en la aplicación
Deje la declaración del paquete en la línea 1.
Pegue el siguiente código en el archivo que ha creado.
```kotlin
import androidx.navigation.compose.NavHost
import androidx.navigation.compose.composable
import androidx.navigation.compose.rememberNavController

@Composable
fun NavManager(viewModel: VistaCompartidaViewModel) {
    // Recibe una instancia de ViewModel
    // y la pasa a las dos pantallas para compartir misma lista
    val navController = rememberNavController()

    NavHost(navController = navController, startDestination = "primera") {
        composable(
            route = "primera"
        ) {
            PrimeraPantalla(viewModel, navController)
        }
        composable(
            route = "segunda",
        ) {
            SegundaPantalla(viewModel, navController)
        }
    }
}
```
Este código es una función que gestiona la navegación entre pantallas usando la función de `Navigation` Jetpack Compose. La navegación se realiza utilizando `NavController`, `NavHost` y funcion `composable`. Aquí se gestiona la transición entre dos pantallas (PrimeraPantalla y SegundaPantalla).

NavManager recibe una instancia de `ViewModel` como un parametro y la pasa a las dos pantallas.
Por lo tanto, no se definen parámetros durante la navegación, pero **las dos pantallas pueden acceder a la misma lista a través del `ViewModel`**.

## Crea una instancia de `ViewModel` y pásala a funcies componibles a través de `NavManager`

```diff
class MainActivity : ComponentActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        enableEdgeToEdge()
        setContent {
            ViewModelSampleTheme {
+                val viewModel: VistaCompartidaViewModel = viewModel()
+                NavManager(viewModel)
            }
        }
    }
}
```
En `MainActivity`, puedes crear una instancia de `ViewModel` y llamar a `NavManager` para compartir la misma instancia.

El método `viewModel()` se puede utilizar para crear una instancia `ViewModel`.
En este caso, no se puede omitir la especificación del tipo de datos.

## Usar ViewModel en funciones componibles
Deje la declaración del paquete en la línea 1.
Pegue el siguiente código en el archivo que ha creado.
```kotlin
import androidx.compose.foundation.layout.Row
import androidx.compose.foundation.lazy.items
import androidx.compose.material3.OutlinedButton
import androidx.compose.material3.OutlinedTextField
import androidx.compose.ui.Alignment
import androidx.compose.ui.Modifier
import androidx.compose.ui.graphics.Color
import androidx.compose.material3.Text


@Composable
fun PrimeraPantalla(
    // Este parámetro recibe una instancia de `VistaCompartidaViewModel`,
    // que se utiliza para gestionar y actualizar el estado de la pantalla
    viewModel: VistaCompartidaViewModel,
    navController: NavController
) {
    Column(
        horizontalAlignment = Alignment.CenterHorizontally,
        modifier = Modifier
            .fillMaxSize()
            .padding(top = 48.dp)
    ) {
        var texto by remember{ mutableStateOf("") }

        Text(text = "Primera Pantalla", fontSize = 20.sp)
        Row(
            modifier = Modifier.padding(horizontal = 8.dp)
        ) {
            OutlinedTextField(
                value = texto,
                onValueChange = { texto = it },
                label = { Text("Introduce Texto Nuevo") },
                colors = OutlinedTextFieldDefaults.colors(
                    focusedTextColor = Color.Black,
                    unfocusedTextColor = Color.Black,
                )
            )
            OutlinedButton(
                onClick = {
                    // Llama a la función `addTexto` en el `ViewModel`,
                    // lo que añade el texto al estado gestionado por `viewModel`
                    viewModel.addTexto(texto)
                    texto = ""
                },
                modifier = Modifier.padding(8.dp),
                enabled = texto.isNotBlank()
            ) {
                Text("Añadir")
            }
        }

        OutlinedButton(
            onClick = { navController.navigate("segunda") }
        ) {
            Text("Navegar a la Segunda")
        }
        HorizontalDivider(thickness = 4.dp, color = Color.Black)
        // Accede a la lista de textos almacenada en el `ViewModel`
        // y muestra cada texto en la lista.
        LazyColumn {
            items(viewModel.textos) { texto: String ->
                Text(texto, fontSize = 20.sp)
            }
        }
    }
}
```

```kotlin
import androidx.compose.foundation.layout.Row
import androidx.compose.foundation.lazy.items
import androidx.compose.material3.OutlinedButton
import androidx.compose.material3.OutlinedTextField
import androidx.compose.ui.Alignment
import androidx.compose.ui.Modifier
import androidx.compose.ui.graphics.Color
import androidx.compose.material3.Text

@Composable
fun SegundaPantalla(
    // Este parámetro recibe una instancia de `VistaCompartidaViewModel`,
    // que se utiliza para gestionar y actualizar el estado de la pantalla
    viewModel: VistaCompartidaViewModel,
    navController: NavController
) {
    Column(
        horizontalAlignment = Alignment.CenterHorizontally,
        modifier = Modifier
            .fillMaxSize()
            .padding(top = 48.dp)
    ) {
        var texto by remember{ mutableStateOf("") }

        Text(text = "Segunda Pantalla", fontSize = 20.sp)
        Row(
            modifier = Modifier.padding(horizontal = 8.dp)
        ) {
            OutlinedTextField(
                value = texto,
                onValueChange = { texto = it },
                label = { Text("Introduce Texto Nuevo") },
                colors = OutlinedTextFieldDefaults.colors(
                    focusedTextColor = Color.Black,
                    unfocusedTextColor = Color.Black,
                )
            )
            OutlinedButton(
                onClick = {
                    // Llama a la función `addTexto` en el `ViewModel`,
                    // lo que añade el texto al estado gestionado por `viewModel`
                    viewModel.addTexto(texto)
                    texto = ""
                },
                modifier = Modifier.padding(8.dp),
                enabled = texto.isNotBlank()
            ) {
                Text("Añadir")
            }
        }

        OutlinedButton(
            onClick = { navController.navigate("primera") }
        ) {
            Text("Navegar a la Primera")
        }
        HorizontalDivider(thickness = 4.dp, color = Color.Black)
        // Accede a la lista de textos almacenada en el `ViewModel`
        // y muestra cada texto en la lista.
        LazyColumn {
            items(viewModel.textos) { texto: String ->
                Text(text = texto)
            }
        }
    }
}
```
Se implementa una interfaz de usuario en la que el usuario introduce texto y pulsa un botón para añadir ese texto al `ViewModel` o para pasar a otra pantalla.

> [!NOTE]
> La función `items` utilizada dentro de `LazyColumn` toma los elementos de una lista dada uno a uno y crea un componente UI con ellos.
> En este ejemplo, cada elemento de `viewModel.textos` se muestra como un componente `Text`.
> En este ejemplo, las cadenas de la lista `viewModel.textos` se muestran como componentes `Text`, pero otros componentes de interfaz de usuario pueden mostrar listas del mismo modo.
