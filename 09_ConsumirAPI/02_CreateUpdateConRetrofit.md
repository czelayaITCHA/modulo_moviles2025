# Utilice Retrofit para consumir las API de creación y actualización.

# Implementar función de Agregar nuevo producto

## 1. Crear método en la interface `ApiProduct`
```kotlin
// Método para registrar un nuevo producto
@POST(ENDPOINT)
suspend fun createProduct(@Body product: ProductModel): Response<ProductModel>
```

## 2. Programar método en el repositorio `ProductRepository`
```kotlin
suspend fun createProduct(product: ProductModel): ProductModel? {
    val response = apiProduct.createProduct(product)
    if (response.isSuccessful) {
        return response.body()
    }
    Log.e("ProductRepository", "Error createProduct: ${response.code()}")
    return null
}
```

## 3. Crear método en `ProductViewModel`
```kotlin
fun createProduct(product: ProductModel) {
    viewModelScope.launch {
        withContext(Dispatchers.IO) {
            // Intentar crear el nuevo producto llamando al repositorio
            val result = repository.createProduct(product)
            if (result != null) {
                // Actualizar el estado del ViewModel si la creación fue exitosa
                val updatedList = _products.value.toMutableList().apply {
                    add(product)  // Agregamos el nuevo producto a la lista
                }
                _products.value = updatedList  // Emitimos la nueva lista
                state = state.copy(successMessage = "Producto creado con éxito")
            } else {
                state = state.copy(errorMessage = "Error al crear el producto")
            }
        }
    }
}
```

## 4. Crear archivo `AddProductView` en el paquete `components` y pegar los siguiente código
```kotlin
import android.Manifest
import android.net.Uri
import androidx.compose.material3.Icon
import androidx.compose.material3.Text
import androidx.compose.ui.Alignment
import androidx.compose.ui.Modifier
import androidx.compose.ui.graphics.Color
import androidx.compose.material.icons.automirrored.filled.ArrowBack
import androidx.compose.runtime.getValue
import androidx.compose.runtime.setValue
import androidx.compose.material3.Button
import androidx.compose.foundation.Image

@OptIn(ExperimentalMaterial3Api::class)
@Composable
fun AddProductView(navController: NavController, viewModel: ProductViewModel){
    Scaffold(
        topBar = {
            CenterAlignedTopAppBar(title = {
                Text(text = "Agregar Producto", color = Color.White,fontWeight = FontWeight.Bold) },
                colors = TopAppBarDefaults.centerAlignedTopAppBarColors(
                    containerColor = MaterialTheme.colorScheme.primary
                ),
                navigationIcon = {
                    IconButton(onClick = { navController.popBackStack()}) {
                        Icon(imageVector = Icons.AutoMirrored.Filled.ArrowBack, contentDescription = "Back", tint = Color.White)
                    }
                }
            )
        }
    ) {
        RegisterProductScreen(it, navController, viewModel)
    }
}

@Composable
fun RegisterProductScreen(
    paddingValues: PaddingValues, navController: NavController, viewModel: ProductViewModel
) {
    var title by remember { mutableStateOf("") }
    var price by remember { mutableStateOf("") }
    var description by remember { mutableStateOf("") }
    var category by remember { mutableStateOf("") }
    var rating by remember { mutableFloatStateOf(0f) }
    var imageUri by remember { mutableStateOf<Uri?>(null) }
    val context = LocalContext.current

    // Launcher para seleccionar la imagen de la galería
    val galleryLauncher = rememberLauncherForActivityResult(
        contract = ActivityResultContracts.GetContent(),
        onResult = { uri: Uri? ->
            imageUri = uri
        }
    )

    // Launcher para solicitar el permiso
    val permissionLauncher = rememberLauncherForActivityResult(
        contract = ActivityResultContracts.RequestPermission(),
        onResult = { isGranted: Boolean ->
            if (isGranted) {
                // Permiso concedido, lanzar la galería
                galleryLauncher.launch("image/*")
            } else {
                // Permiso denegado
                Toast.makeText(context, "Permiso denegado", Toast.LENGTH_SHORT).show()
            }
        }
    )

    Column(
        modifier = Modifier
            .fillMaxSize()
            .padding(paddingValues)
            .padding(top = 30.dp),
        verticalArrangement = Arrangement.spacedBy(8.dp),
        horizontalAlignment = Alignment.CenterHorizontally
    ) {
        TextField(
            value = title,
            onValueChange = { title = it },
            label = { Text("Title") },
            modifier = Modifier.fillMaxWidth()
                .padding(horizontal = 16.dp)
                .padding(top = 8.dp)
        )
        TextField(
            value = price,
            onValueChange = { price = it },
            label = { Text("Price") },
            modifier = Modifier.fillMaxWidth()
                .padding(horizontal = 16.dp)
                .padding(top = 8.dp),
            keyboardOptions = KeyboardOptions(keyboardType = KeyboardType.Number)
        )
        TextField(
            value = description,
            onValueChange = { description = it },
            label = { Text("Description") },
            modifier = Modifier.fillMaxWidth()
                .padding(horizontal = 16.dp)
                .padding(top = 8.dp)
        )
        TextField(
            value = category,
            onValueChange = { category = it },
            label = { Text("Category") },
            modifier = Modifier.fillMaxWidth()
                .padding(horizontal = 16.dp)
                .padding(top = 8.dp),
            maxLines = 3,
            singleLine = false
        )
        Column {
            Text(text = "Calificación: ${rating.toInt()} Estrellas",
                modifier = Modifier.fillMaxWidth()
                    .padding(horizontal = 16.dp)
            )
            Slider(
                value = rating,
                onValueChange = { newRating -> rating = newRating },
                valueRange = 0f..5f,  // Rango del rating de 0 a 5
                steps = 4,  // Cantidad de pasos, para permitir medias estrellas
                modifier = Modifier.padding(horizontal = 16.dp)
            )
        }

        imageUri?.let {
            Image(
                painter = rememberAsyncImagePainter(it),
                contentDescription = null,
                modifier = Modifier
                    .size(200.dp)
                    .clip(RoundedCornerShape(8.dp))
                    .background(Color.Gray),
                contentScale = ContentScale.Crop
            )
        }

        Button(
            onClick = {
                // Verificar versión de Android y solicitar permiso adecuado
                if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.TIRAMISU) {
                    permissionLauncher.launch(Manifest.permission.READ_MEDIA_IMAGES)
                } else {
                    // Para versiones anteriores a Android 13
                    permissionLauncher.launch(Manifest.permission.READ_EXTERNAL_STORAGE)
                }
            }
        ) {
            Text("Seleccionar Imagen")
        }

        Button(
            onClick = {
                if (title.isNotEmpty() && price.isNotEmpty() && description.isNotEmpty() && category.isNotEmpty() && imageUri != null) {
                    // Convertir el precio a Double
                    val priceValue = price.toDoubleOrNull() ?: 0.0

                    // Crear el nuevo producto
                    val newProduct = ProductModel(
                        id = 0,
                        title = title,
                        price = priceValue,
                        description = description,
                        category = category,
                        image = imageUri.toString(), // Guardamos la URI de la imagen seleccionada
                        rating = Rating(rating.toDouble(), 0)
                    )
                    // Registrar el producto
                    viewModel.createProduct(newProduct)

                    Toast.makeText(context, "Producto registrado", Toast.LENGTH_SHORT).show()
                    navController.popBackStack()
                } else {
                    Toast.makeText(context, "Completa todos los campos", Toast.LENGTH_SHORT).show()
                }
            }
        ) {
            Text("Registrar Producto")
        }
    }
}
```
- **`rememberLauncherForActivityResult(...)`:**
  En Jetpack Compose, esta función permite **lanzar una actividad externa** (por ejemplo, la galería o la cámara) y **recibir su resultado**.
    * `rememberLauncherForActivityResult` es un **envoltorio para utilizar el API de resultados de actividades** dentro de Compose.
    * `contract` define **qué acción deseas realizar** (como seleccionar un archivo o pedir permisos).
    * `onResult` define **qué hacer cuando se recibe un resultado**.
    * La función usa `remember` para **mantener el estado durante recomposiciones** de Compose.

- **`permissionLauncher.launch(Manifest.permission.READ_MEDIA_IMAGES)` :**
  Se utiliza para **solicitar permisos al usuario**, por ejemplo, para acceder a imágenes.
    * `permissionLauncher` es un lanzador creado con `rememberLauncherForActivityResult`.
    * `.launch(...)` muestra al usuario una solicitud de permiso.
    * `Manifest.permission.READ_MEDIA_IMAGES` es el **permiso requerido a partir de Android 13 (TIRAMISU)** para leer imágenes.
      * En versiones anteriores, se usa `READ_EXTERNAL_STORAGE`.
    * El resultado (`permitido` o `denegado`) se gestiona en el bloque `onResult`.

- **`val context = LocalContext.current` :**
  Se utiliza para **obtener el `Context` actual** dentro de una función `@Composable`.
    * `Context` es un objeto importante en Android que permite hacer cosas como:
      * mostrar `Toast`
      * acceder a recursos etc.
    * `LocalContext.current` es la forma **recomendada en Compose para acceder al contexto actual**.

- **`Toast.makeText(context, "...", Toast.LENGTH_SHORT).show()` :**
  Muestra un **mensaje corto tipo Toast**, que aparece unos segundos en la pantalla.
    * `Toast` es una forma sencilla de **mostrar un mensaje temporal** al usuario.
    * `makeText(...)` crea el Toast y `.show()` lo muestra.
    * Parámetros:
      * `context`: el contexto actual (obtenido con `LocalContext.current`)
      * `"Producto registrado"`: el texto que se va a mostrar
      * `Toast.LENGTH_SHORT`: duración del mensaje (también hay `LENGTH_LONG`)

### 5. Crear la ruta de navegación en `NavManager`
```kotlin
composable("agregar"){
   AddProductView(navController, viewModel)
}
```

### 6. Habilitar permisos para utilizar la galeria de imágenes del dispositivo en el archivo AndroidManifest.xml
```xml
<uses-permission android:name="android.permission.INTERNET" />
<uses-permission android:name="android.permission.READ_MEDIA_IMAGES" /> <!-- ★Agregar -->
```

# Implementar función de Actualizar producto

## 1. Crear método en la interface `ApiProduct`
```kotlin
// Método para actualizar un producto
@PUT("$ENDPOINT/{id}")
suspend fun updateProduct(@Path("id") id: Int, @Body product: ProductModel): Response<ProductModel>
```

## 2. Programar método en el repositorio `ProductRepository`
```kotlin
suspend fun updateProduct(product: ProductModel): ProductModel? {
    val response = apiProduct.updateProduct(product.id, product)
    if (response.isSuccessful) {
        return response.body()
    }
    Log.e("ProductRepository", "Error updateProduct: ${response.code()}")
    return null
}
```

## 3. Crear método en `ProductViewModel`
```kotlin
// Actualizar el estado de para redibujar la interfaz de usuario
fun updateState(updatedState: ProductState) {
    state = state.copy(
        title = updatedState.title,
        price = updatedState.price,
        description = updatedState.description,
        category = updatedState.category,
        rating = updatedState.rating
    )
}

// Llamar al método de ProductRepository para actualizar el producto
fun updateProduct(product: ProductModel) {
    viewModelScope.launch {
        withContext(Dispatchers.IO) {
            // Intentar actualizar el producto llamando al repositorio
            val result = repository.updateProduct(product)
            if (result != null) {
                // Simulación del comportamiento real tras la actualización
                // Actualizar el estado del ViewModel si la actualizarción fue exitosa
                val updatedList = _products.value.map {
                    if (it.id == product.id) product else it
                }
                _products.value = updatedList
                state = state.copy(successMessage = "Producto creado con éxito")
            } else {
                state = state.copy(errorMessage = "Error al crear el producto")
            }
        }
    }
}
```

## 4. Crear archivo `UpdateProductView` en el paquete `components` y pegar los siguiente código
```kotlin
import androidx.compose.material3.Text
import androidx.compose.ui.Alignment
import androidx.compose.ui.Modifier
import androidx.compose.material3.Button
import androidx.compose.foundation.layout.fillMaxSize
import androidx.compose.foundation.layout.fillMaxWidth
import androidx.compose.foundation.layout.padding

@OptIn(ExperimentalMaterial3Api::class)
@Composable
fun UpdateProductView(viewModel: ProductViewModel, navController: NavController, id: Int) {
    // Al inicio de la pantalla, obtener información del producto de API
    LaunchedEffect(Unit) {
        viewModel.getProductById(id)
    }
    // Al salir de la pantalla, inicializar el estado.
    DisposableEffect(Unit) {
        onDispose {
            viewModel.clean()
        }
    }
    Scaffold(
        topBar = {
            MainTopBar(
                title = "Actualizar Producto",
                showBackButton = true,
                onCickBackButton = { navController.popBackStack() },
                onCickAction = {}
            )
        }
    ) {
        ContentUpdateProductView(it, viewModel, id, navController)
    }
}

@Composable
fun ContentUpdateProductView(padd: PaddingValues, viewModel: ProductViewModel, id: Int, navController: NavController) {
    val state = viewModel.state
    val context = LocalContext.current

    Column(
        modifier = Modifier
            .fillMaxSize()
            .padding(padd)
            .padding(top = 30.dp),
        verticalArrangement = Arrangement.spacedBy(8.dp),
        horizontalAlignment = Alignment.CenterHorizontally
    ) {
        TextField(
            value = state.title,
            onValueChange = { viewModel.updateState(state.copy(title = it)) },
            label = { Text("Title") },
            modifier = Modifier.fillMaxWidth()
                .padding(horizontal = 16.dp)
                .padding(top = 8.dp)
        )
        TextField(
            value = state.price.toString(),
            onValueChange = { viewModel.updateState(state.copy(price = it.toDouble())) },
            label = { Text("Price") },
            modifier = Modifier.fillMaxWidth()
                .padding(horizontal = 16.dp)
                .padding(top = 8.dp),
            keyboardOptions = KeyboardOptions(keyboardType = KeyboardType.Number)
        )
        TextField(
            value = state.description,
            onValueChange = { viewModel.updateState(state.copy(description = it)) },
            label = { Text("Description") },
            modifier = Modifier.fillMaxWidth()
                .padding(horizontal = 16.dp)
                .padding(top = 8.dp)
        )
        TextField(
            value = state.category,
            onValueChange = { viewModel.updateState(state.copy(category = it)) },
            label = { Text("Category") },
            modifier = Modifier.fillMaxWidth()
                .padding(horizontal = 16.dp)
                .padding(top = 8.dp),
            maxLines = 3,
            singleLine = false
        )
        Column {
            Text(text = "Calificación: ${state.rating.rate.toFloat().toInt()} Estrellas",
                modifier = Modifier.fillMaxWidth()
                    .padding(horizontal = 16.dp)
            )
            Slider(
                value = state.rating.rate.toFloat(),
                onValueChange = { viewModel.updateState(state.copy(rating = Rating(it.toDouble(), 0))) },
                valueRange = 0f..5f,  // Rango del rating de 0 a 5
                steps = 4,  // Cantidad de pasos, para permitir medias estrellas
                modifier = Modifier.padding(horizontal = 16.dp)
            )
        }

        Button(
            onClick = {
                if (state.title.isNotEmpty() && state.price != 0.0 && state.description.isNotEmpty() && state.category.isNotEmpty()) {
                    // Crear el nuevo producto
                    val updatedProduct = ProductModel(
                        id = id,
                        title = state.title,
                        price = state.price,
                        description = state.description,
                        category = state.category,
                        image = state.image, // Guardamos la URI de la imagen seleccionada
                        rating = state.rating
                    )
                    // Actualizar el producto
                    viewModel.updateProduct(updatedProduct)

                    Toast.makeText(context, "Producto actualizado", Toast.LENGTH_SHORT).show()
                    navController.popBackStack()
                } else {
                    Toast.makeText(context, "Completa todos los campos", Toast.LENGTH_SHORT).show()
                }
            }
        ) {
            Text("Actualizar Producto")
        }
    }
}
```

### 5. Crear la ruta de navegación en `NavManager`
```kotlin
composable(
    route = "Update/{id}",
    arguments = listOf(
        navArgument("id") { type = NavType.IntType }
    )
)
{
    val id = it.arguments?.getInt("id") ?: 0
    UpdateProductView(viewModel, navController, id)
}
```

### 6. Establecer la navegación a `UpdateProductView` en el boton de editar en `ContentHomeView`
```kotlin
@Composable
fun ContentHomeView(
    viewModel: ProductViewModel,
    paddingValues: PaddingValues,
    navController: NavController
) {
    val products by viewModel.products.collectAsState()

    Column(
        modifier = Modifier.padding(paddingValues),
        verticalArrangement = Arrangement.Center,
        horizontalAlignment = Alignment.CenterHorizontally
    ) {
        LazyColumn() {
            items(products) { product ->
                CardProduct(
                    product = product,
                    onClick = { navController.navigate("DetailView/${product.id}") },
                    onEditClick = { navController.navigate("Update/${product.id}") }, // ★Agregar
                    onDeleteClick = {
                        viewModel.deleteProductById(product.id)
                    }
                )
            }
        }
    }
}
```
