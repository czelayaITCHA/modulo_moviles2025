# Consumir APIs con Retrofit

Retrofit es una potente librería de Android para realizar solicitudes HTTP de forma sencilla y eficiente. Permite interactuar con APIs REST y procesar sus respuestas de manera automática, convirtiéndolas en objetos Kotlin. En aplicaciones que usan Jetpack Compose, Retrofit es una excelente opción para manejar datos de servidores y mostrarlos en las interfaces modernas y reactivas que Compose ofrece.

Para este ejemplo vamos a consumir [Fake Store API](https://fakestoreapi.com/), que esta en linea y se puede usar para propósitos de aprendizaje.

# Pasos para consumir APIs con Retrofit

## Preparación Previa: Creación de una aplicación sin llamada al API

Crear un Proyecto `ProductsApiApp`

Necesita agregar dependencias. Añade siguiente codigo en tu archivo `build.gradle.kts (:app)`.

```kotlin
dependencies {
    ...Arriba omitido...
    // ViewModel
    implementation("androidx.lifecycle:lifecycle-viewmodel-compose:2.9.0")
    // Navigation
    implementation("androidx.navigation:navigation-compose:2.9.0")
}
```
Haga clic en `Sync Now` para sincronizar.
![image](https://github.com/user-attachments/assets/79dcefdd-b5c2-49d2-ac7a-020075ea255e)

Crear un paquete `components` bajo `ui`.

Crear archivos `RatingBar`, `MainTopBar`, `HomeView`, `DetailView` en el paquete `components` y pegar los siguiente código.
- RatingBar
```kotlin
import androidx.compose.foundation.layout.Row
import androidx.compose.material3.Icon
import androidx.compose.material3.Text
import androidx.compose.ui.Alignment
import androidx.compose.ui.Modifier
import androidx.compose.ui.graphics.Color

@Composable
fun RatingBar(rating: Double) {
    Row(
        verticalAlignment = Alignment.CenterVertically,
        horizontalArrangement = Arrangement.Start,
        modifier = Modifier.padding(horizontal = 8.dp)
    ) {
        // Mostrar estrellas según el rating (entre 0 y 5)
        repeat(5) { index ->
            Icon(
                imageVector = Icons.Default.Star,
                contentDescription = null,
                tint = if (index < rating.toInt()) Color.Blue else Color.Gray,
                modifier = Modifier.size(20.dp)
            )
        }
        Spacer(modifier = Modifier.width(8.dp))
        Text(text = String.format("%.1f", rating))
    }
}
```
- MainTopBar
```kotlin
import androidx.compose.material3.Icon
import androidx.compose.material3.Text
import androidx.compose.ui.graphics.Color
import androidx.compose.material.icons.automirrored.filled.ArrowBack

@OptIn(ExperimentalMaterial3Api::class)
@Composable
fun MainTopBar(
    title: String,
    showBackButton: Boolean = false,
    onCickBackButton: () -> Unit,
    onCickAction: () -> Unit
) {
    TopAppBar(
        title = { Text(text = title, color = Color.White, fontWeight = FontWeight.ExtraBold) },
        colors = TopAppBarDefaults.centerAlignedTopAppBarColors(
            containerColor = MaterialTheme.colorScheme.primary
        ),
        navigationIcon = {
            if (showBackButton) {
                IconButton(onClick = { onCickBackButton() }) {
                    Icon(
                        imageVector = Icons.AutoMirrored.Filled.ArrowBack,
                        contentDescription = "Back",
                        tint = Color.White
                    )
                }
            }
        },
        actions = {
            if (!showBackButton) {
                IconButton(onClick = { onCickAction() }) {
                    Icon(
                        imageVector = Icons.Default.Search,
                        contentDescription = "Search",
                        tint = Color.White
                    )
                }
            }
        }
    )
}
```
- HomeView
```kotlin
import androidx.compose.foundation.Image
import androidx.compose.foundation.layout.Row
import androidx.compose.material3.Icon
import androidx.compose.material3.Text
import androidx.compose.ui.Alignment
import androidx.compose.ui.Modifier
import androidx.compose.ui.graphics.Color
import android.R

@Composable
fun HomeView(viewModel: ProductViewModel, navController: NavController) {
    Scaffold(
        topBar = {
            MainTopBar(
                title = "API Products",
                onCickBackButton = { },
                onCickAction = { }
            )
        },
        floatingActionButton = {
            FloatingActionButton(
                onClick = { navController.navigate("agregar") },
                containerColor = MaterialTheme.colorScheme.primary,
                contentColor = Color.White
            ) {
                Icon(imageVector = Icons.Default.Add, contentDescription = "Agregar")
            }
        }
    ) {
        ContentHomeView(viewModel, it, navController)
    }
}

@Composable
fun ContentHomeView(
    viewModel: ProductViewModel,
    paddingValues: PaddingValues,
    navController: NavController
) {
    val products = viewModel.products

    Column(
        modifier = Modifier.padding(paddingValues),
        verticalArrangement = Arrangement.Center,
        horizontalAlignment = Alignment.CenterHorizontally
    ) {
        LazyColumn() {
            items(products) { product ->
                Card(
                    shape = RoundedCornerShape(8.dp),
                    modifier = Modifier
                        .fillMaxWidth()
                        .padding(10.dp)
                        .shadow(elevation = 10.dp)
                        .clickable { navController.navigate("DetailView/0") }
                ) {
                    Column(
                        modifier = Modifier
                            .fillMaxWidth()
                            .padding(8.dp)
                    ) {
                        // Imagen del producto
                        Image(
                            painter = painterResource(id = R.drawable.star_on),
                            contentDescription = null,
                            contentScale = ContentScale.Fit, // Escala la imagen para que se ajuste completamente al contenedor sin recortarla.
                            modifier = Modifier
                                .fillMaxWidth()
                                .height(250.dp)
                                .padding(8.dp)
                        )

                        // Detalles del producto
                        Text(
                            text = product,
                            style = MaterialTheme.typography.titleLarge,
                            modifier = Modifier.padding(horizontal = 8.dp)
                        )
                        Text(
                            text = "$${product.length}",
                            fontWeight = FontWeight.ExtraBold,
                            style = MaterialTheme.typography.titleMedium.copy(color = Color.Black),
                            modifier = Modifier.padding(horizontal = 8.dp),
                        )
                        RatingBar(rating = 1.0)

                        // Botones de Editar y Eliminar
                        Row(
                            horizontalArrangement = Arrangement.SpaceBetween,
                            modifier = Modifier
                                .fillMaxWidth()
                                .padding(horizontal = 8.dp)
                        ) {
                            IconButton(onClick = { }) {
                                Icon(
                                    imageVector = Icons.Default.Edit,
                                    contentDescription = "Editar",
                                    tint = Color.White
                                )
                            }
                            IconButton(onClick = { }) {
                                Icon(
                                    imageVector = Icons.Default.Delete,
                                    contentDescription = "Eliminar",
                                    tint = Color.Red
                                )
                            }
                        }
                    }
                }
            }
        }
    }
}
```
- DetailView
```kotlin
import androidx.compose.foundation.Image
import androidx.compose.foundation.layout.Row
import androidx.compose.material3.Icon
import androidx.compose.material3.Text
import androidx.compose.ui.Alignment
import androidx.compose.ui.Modifier
import androidx.compose.ui.graphics.Color
import android.R

@OptIn(ExperimentalMaterial3Api::class)
@Composable
fun DetailView(viewModel: ProductViewModel, navController: NavController, id: Int) {
    Scaffold(
        topBar = {
            MainTopBar(
                title = "Product Detail",
                showBackButton = true,
                onCickBackButton = { navController.popBackStack() },
                onCickAction = {}
            )
        }
    ) {
        ContentDetailView(it, viewModel)
    }
}

@Composable
fun ContentDetailView(padd: PaddingValues, viewModel: ProductViewModel) {
    Column(
        modifier = Modifier
            .padding(padd)
    ) {
        Image(
            painter = painterResource(id = R.drawable.star_on),
            contentDescription = null,
            contentScale = ContentScale.Fit, // Escala la imagen para que se ajuste completamente al contenedor sin recortarla.
            modifier = Modifier
                .fillMaxWidth()
                .height(250.dp)
                .padding(8.dp)
        )
        Row(
            horizontalArrangement = Arrangement.Center,
            modifier = Modifier.fillMaxWidth()
        ) {
            Text(text = "Price: ", color = Color.Green, style = MaterialTheme.typography.titleLarge)
            Text(
                text = "1",
                color = Color.Green,
                style = MaterialTheme.typography.titleLarge
            )
        }

        //creando el text para la descripcion del producto
        Column(modifier = Modifier.fillMaxWidth()) {
            Text(
                text = "Description",
                color = Color.Red,
                style = MaterialTheme.typography.bodyLarge,
                modifier = Modifier.padding(start = 15.dp),
            )
            Text(
                text = "description producto",
                color = Color.White,
                textAlign = TextAlign.Justify,
                modifier = Modifier.padding(start = 15.dp, end = 15.dp, bottom = 10.dp)
            )
        }
        Row(
            horizontalArrangement = Arrangement.SpaceBetween,
            verticalAlignment = Alignment.CenterVertically,
            modifier = Modifier
                .fillMaxWidth()
                .padding(start = 20.dp, end = 10.dp, bottom = 10.dp)
        ) {
            RatingBar(rating = 1.0)
            Text(
                text = "Cat: category producto",
                color = Color.Green,
                style = MaterialTheme.typography.bodyLarge
            )
        }
    }
}
```

Crear archivos `NavManager`, `ProductViewModel` en el paquete `ui` y pegar los siguiente código.
- NavManager
```kotlin
import androidx.navigation.compose.NavHost

@Composable
fun NavManager(viewModel: ProductViewModel) {
    val navController = rememberNavController()
    NavHost(navController = navController, startDestination = "Home") {
        composable("Home") {
            HomeView(viewModel, navController)
        }
        composable(
            "DetailView/{id}", arguments = listOf(
            navArgument("id") { type = NavType.IntType }
        )) {
            val id = it.arguments?.getInt("id") ?: 0
            DetailView(viewModel, navController, id)
        }
    }
}
```
- ProductViewModel
```kotlin
class ProductViewModel() : ViewModel() {

    // Lista de productos
    val products = listOf<String>("Producto 1", "Producto 2", "Producto 3")
}
```

En `MainActivity`, sustituye el código del bloque `ProductsApiAppTheme` por el siguiente código.
```kotlin
Surface(
    modifier = Modifier.fillMaxSize(),
    color = MaterialTheme.colorScheme.background
)
{
    val productViewModel: ProductViewModel = viewModel()
    NavManager(productViewModel)
}
```

## 1. Agregar plug-in y dependencias
Añade el siguiente plugin a `build.gradle.kts (ProductsApiApp)` en el directorio raíz.
```kotlin
plugins {
    ...Arriba omitido...
    id("com.google.devtools.ksp") version "2.0.21-1.0.28" apply false
    id("com.google.dagger.hilt.android") version "2.56.2" apply false
}
```

Necesita agregar plugin y dependencias. Añade siguiente codigo en tu archivo `build.gradle.kts (:app)`.
```kotlin
plugins {
  ...Arriba omitido...
    id("com.google.devtools.ksp")
    id("com.google.dagger.hilt.android")
}
```
```kotlin
dependencies {
    ...Arriba omitido...
    // Hilt
    implementation("com.google.dagger:hilt-android:2.56.2")
    ksp("com.google.dagger:hilt-android-compiler:2.56.2")
    // Retrofit
    implementation("com.squareup.retrofit2:retrofit:2.9.0")
    implementation("com.squareup.retrofit2:converter-gson:2.9.0")
    // Coil
    implementation("io.coil-kt.coil3:coil-compose:3.2.0")
    implementation("io.coil-kt.coil3:coil-network-okhttp:3.2.0")
}
```
Haga clic en `Sync Now` para sincronizar.
![image](https://github.com/user-attachments/assets/79dcefdd-b5c2-49d2-ac7a-020075ea255e)

## 2. Crear estructura de paquete
Crear paquetes `data`, `di`, `model`, `utils` de acuerdo a la siguiente imagen
<br>
![image](https://github.com/user-attachments/assets/0b1d39ec-15cd-4f15-98d2-393a6e24c9f2)

## 3. **Crear la clase Application Anotado con @HiltAndroidApp**
Crear una clase `ProductApplication` en el paquete principal
```kotlin
import android.app.Application
import dagger.hilt.android.HiltAndroidApp

@HiltAndroidApp
class ProductApplication: Application()
```
En AndroidManifest.xml, añada la siguiente línea.
```xml
<application
    android:name=".ProductApplication"
    ... >
</application>
```

## 4. **Anotar MainActivity con @AndroidEntryPoint**
```kotlin
@AndroidEntryPoint
class MainActivity : ComponentActivity() { ... }
```

## 5. Crear Constants para definir constantes
Crear archivos `Constants` en el paquete `utils` y pegar los siguiente código.

La palabra clave `object` hace que `Constants` sea la única instancia de la aplicación.<br>
Definir la URL base y la ruta de la API como constantes. Ademas, definir los códigos de color habitual de la aplicación como constantes.

```kotlin
object Constants {
    const val BASE_URL = "https://fakestoreapi.com/"
    const val ENDPOINT = "products"
    const val CUSTOM_BLACK = 0xFF2B2626
}
```

## 6. Crear un modelo para la respuesta JSON de la API
Para definir el o los modelos a utilizar debe analizarse la estructura del JSON devuelto por la API en el endpoint principal: https://fakestoreapi.com/

Crear el archivo `ProductModel` en el paquete `model` y pegar los siguiente código.
```kotlin
data class ProductModel(
    val id : Int,
    val title : String,
    val price : Double,
    val description : String,
    val category : String,
    val image : String,
    val rating : Rating
)

data class Rating(
    val rate: Double,
    val count: Int
)
```

## 7. Crear interfaz de API
El componente clave de Retrofit es la interfaz de API, que define los endpoints y métodos HTTP de forma declarativa.

Crear el archivo `ApiProduct` en el paquete `data` y pegar los siguiente código.
```kotlin
import retrofit2.Response

interface ApiProduct {

    @GET(ENDPOINT)
    suspend fun getProducts(): Response<List<ProductModel>>

    @GET("$ENDPOINT/{id}")
    suspend fun getProductById(@Path("id") id: Int): Response<ProductModel>

    //faltan métodos para eliminar, agregar y editar.
}
```
- **`@GET`**:
  - Indica que se realizará una solicitud HTTP GET al endpoint especificado (por ejemplo, "products").
- **`$ENDPOINT/{id}`:**
  - Se accede a un producto específico por su ID, usando una ruta como `/products/1`.
- **`@Path("id") id: Int`:**
  - El parámetro `id` se inserta en la URL como parte del endpoint.

## 8. Crear módulo para la configuración de la inyección de dependencias
Crear el archivo `AppModule` en el paquete `di` y pegar los siguiente código.

Este módulo configura Retrofit como cliente HTTP y prepara la interfaz ApiProduct para hacer peticiones a la API. Ambas instancias se crean una vez y se reutilizan gracias al patrón Singleton, y están listas para ser inyectadas en las clases que las necesiten usando Hilt.

```kotlin
import javax.inject.Singleton

@Module
@InstallIn(SingletonComponent::class)
object AppModule {
    //para inyectar dependencia primero se define el Singleton y despues el provider
    @Singleton
    @Provides
    fun providesRetrofit(): Retrofit {
        return Retrofit.Builder()
            .baseUrl(BASE_URL)
            .addConverterFactory(GsonConverterFactory.create())
            .build()
    }

    @Singleton
    @Provides
    fun providesApiProducts(retrofit: Retrofit): ApiProduct {
        return retrofit.create(ApiProduct::class.java)
    }
}
```
- **`Retrofit.Builder()`**: Construye una nueva instancia de Retrofit.
- **`.baseUrl(BASE_URL)`**: Establece la URL base de la API (por ejemplo: `"https://miapi.com/"`).
- **`.addConverterFactory(GsonConverterFactory.create())`**: Agrega un convertidor de JSON a objetos Kotlin usando `Gson`.
- **`.build()`**: Crea la instancia de Retrofit lista para ser usada.
- **`providesApiProducts()`**:
  * Esta función crea una **implementación automática** de la interfaz `ApiProduct` usando Retrofit.
  * Toma como parámetro la instancia de `Retrofit` ya creada.
  * Gracias a esto, puedes usar `@Inject` para obtener una instancia de `ApiProduct` en cualquier parte de tu app.

## 9. Crear una clase Repository
Crear la clase `ProductRepository` en el paquete `data` y pegar los siguiente código.

Un repositorio es una capa que separa la lógica de obtención de datos de la lógica de la interfaz de usuario (UI).

```kotlin
import javax.inject.Inject

class ProductRepository @Inject constructor(private val apiProduct: ApiProduct) {

    suspend fun getProducts(): List<ProductModel>? {
        val response = apiProduct.getProducts()
        if (response.isSuccessful) {
            return response.body()
        }
        Log.e("ProductRepository", "Error getProducts: ${response.code()}")
        return null
    }

    suspend fun getProductById(id: Int): ProductModel? {
        val response = apiProduct.getProductById(id)
        if (response.isSuccessful) {
            return response.body()
        }
        Log.e("ProductRepository", "Error getProductById: id: $id, ${response.code()}")
        return null
    }
}
```

## 10. Crear la clase ProductState para gestionar variables de estado
Crear la clase `ProductState` en el paquete `ui` y pegar los siguiente código.
Todas las variables de datos utilizadas en la interfaz de usuario se definen en esta clase.

```kotlin
data class ProductState(
    val errorMessage: String = "",
    val successMessage: String = "",

    val title: String = "",
    val price: Double = 0.0,
    val description: String = "",
    val category: String = "",
    val image: String = "",
    val rating: Rating = Rating(0.0, 0)
)
```

## 11. Añadir métodos para la obtención de datos al ViewModel.

```kotlin
import androidx.compose.runtime.getValue
import androidx.compose.runtime.mutableStateOf
import androidx.compose.runtime.setValue
import javax.inject.Inject

@HiltViewModel
class ProductViewModel @Inject constructor(
    private val repository: ProductRepository
) : ViewModel() {

    // Lista de productos
    private val _products = MutableStateFlow<List<ProductModel>>(emptyList())
    val products = _products.asStateFlow()

    // Estado del producto en detalle
    var state by mutableStateOf(ProductState())
        private set

    init {
        getProducts()
    }

    private fun getProducts() {
        viewModelScope.launch {
            withContext(Dispatchers.IO) {
                val products = repository.getProducts()
                Log.d("ProductViewModel", "Fetched products: $products")
                _products.value = products ?: emptyList()
            }
        }
    }

    fun getProductById(id: Int) {
        viewModelScope.launch {
            withContext(Dispatchers.IO) {
                val product = repository.getProductById(id)
                state = state.copy(
                    title = product?.title ?: "",
                    price = product?.price ?: 0.0,
                    description = product?.description ?: "",
                    category = product?.category ?: "",
                    image = product?.image ?: "",
                    rating = product?.rating ?: Rating(0.0, 0)
                )
            }
        }
    }
    
    fun clean() {
        state = state.copy(
            title = "",
            price = 0.0,
            description = "",
            category = "",
            image = "",
            rating = Rating(0.0, 0)
        )
    }
}
```
- **`withContext(Dispatchers.IO)`**:
  -  Se utiliza en Kotlin Coroutines para ejecutar tareas pesadas en un hilo adecuado que no bloquee el hilo principal (UI).
  -  Si realizas tareas pesadas como llamadas a red o lectura de archivos en el hilo principal (UI), la aplicación se puede **congelar o cerrar inesperadamente**.

## 11. Crear CardProduct
Crear el archivo `CardProduct` en el paquete `components` y pegar los siguiente código.

```kotlin
import androidx.compose.ui.Modifier
import androidx.compose.material3.Text
import androidx.compose.ui.graphics.Color
import androidx.compose.material3.Icon
import androidx.compose.foundation.layout.Row

@Composable
fun CardProduct(
    product: ProductModel,
    onClick: () -> Unit,
    onEditClick: () -> Unit,
    onDeleteClick: () -> Unit,
) {
    Card(
        shape = RoundedCornerShape(8.dp),
        modifier = Modifier
            .fillMaxWidth()
            .padding(10.dp)
            .shadow(elevation = 10.dp)
            .clickable { onClick() }
    ) {
        Column(
            modifier = Modifier
                .fillMaxWidth()
                .padding(8.dp)
        ) {
            // Imagen del producto
            AsyncImage(
                model = product.image,
                contentDescription = null,
                contentScale = ContentScale.Fit, // Escala la imagen para que se ajuste completamente al contenedor sin recortarla.
                modifier = Modifier
                    .fillMaxWidth()
                    .height(250.dp)
                    .padding(8.dp)
            )

            // Detalles del producto
            Text(
                text = product.title,
                style = MaterialTheme.typography.titleLarge,
                modifier = Modifier.padding(horizontal = 8.dp)
            )
            Text(
                text = "$${product.price}",
                fontWeight = FontWeight.ExtraBold,
                style = MaterialTheme.typography.titleMedium.copy(color = Color.Black),
                modifier = Modifier.padding(horizontal = 8.dp),
            )
            RatingBar(rating = product.rating.rate)

            // Botones de Editar y Eliminar
            Row(
                horizontalArrangement = Arrangement.SpaceBetween,
                modifier = Modifier
                    .fillMaxWidth()
                    .padding(horizontal = 8.dp)
            ) {
                IconButton(onClick = { onEditClick() }) {
                    Icon(
                        imageVector = Icons.Default.Edit,
                        contentDescription = "Editar",
                        tint = Color.White
                    )
                }
                IconButton(onClick = { onDeleteClick()}) {
                    Icon(
                        imageVector = Icons.Default.Delete,
                        contentDescription = "Eliminar",
                        tint = Color.Red
                    )
                }
            }
        }
    }
}
```
- **`AsyncImage(...)`**: Composable de librería `Coil` para cargar imágenes desde una URL o recurso.
- **`model = product.image`**: La URL o fuente de la imagen del producto (`product.image`). Puede ser un `String` o un `Uri`.

## 12. Modicar Homeview

```diff
import androidx.compose.runtime.getValue // ★Agregar

...Omitido...

fun ContentHomeView(
    viewModel: ProductViewModel,
    paddingValues: PaddingValues,
    navController: NavController
) {
-    val products = viewModel.products
    val products by viewModel.products.collectAsState() // ★Agregar

    Column(
        modifier = Modifier.padding(paddingValues),
        verticalArrangement = Arrangement.Center,
        horizontalAlignment = Alignment.CenterHorizontally
    ) {
        LazyColumn() {
            items(products) { product ->
-                Card(
-                    shape = RoundedCornerShape(8.dp),
-                    modifier = Modifier
-                        .fillMaxWidth()
-                        .padding(10.dp)
-                        .shadow(elevation = 10.dp)
-                        .clickable { navController.navigate("DetailView/0") }
-                ) {
-                    Column(
-                        modifier = Modifier
-                            .fillMaxWidth()
-                            .padding(8.dp)
-                    ) {
-                        // Imagen del producto
-                        Image(
-                            painter = painterResource(id = R.drawable.star_on),
-                            contentDescription = null,
-                            contentScale = ContentScale.Fit, // Escala la imagen para que se ajuste completamente al contenedor sin recortarla.
-                            modifier = Modifier
-                                .fillMaxWidth()
-                                .height(250.dp)
-                                .padding(8.dp)
-                        )
-
-                        // Detalles del producto
-                        Text(
-                            text = product,
-                            style = MaterialTheme.typography.titleLarge,
-                            modifier = Modifier.padding(horizontal = 8.dp)
-                        )
-                        Text(
-                            text = "$${product.length}",
-                            fontWeight = FontWeight.ExtraBold,
-                            style = MaterialTheme.typography.titleMedium.copy(color = Color.Black),
-                            modifier = Modifier.padding(horizontal = 8.dp),
-                        )
-                        RatingBar(rating = 1.0)
-
-                        // Botones de Editar y Eliminar
-                        Row(
-                            horizontalArrangement = Arrangement.SpaceBetween,
-                            modifier = Modifier
-                                .fillMaxWidth()
-                                .padding(horizontal = 8.dp)
-                        ) {
-                            IconButton(onClick = { }) {
-                                Icon(
-                                    imageVector = Icons.Default.Edit,
-                                    contentDescription = "Editar",
-                                    tint = Color.White
-                                )
-                            }
-                            IconButton(onClick = { }) {
-                                Icon(
-                                    imageVector = Icons.Default.Delete,
-                                    contentDescription = "Eliminar",
-                                    tint = Color.Red
-                                )
-                            }
-                        }
-                    }
-                }
                // ★Agregar hacia abajo desde aquí.
                CardProduct(
                    product = product,
                    onClick = { navController.navigate("DetailView/${product.id}") },
                    onEditClick = {},
                    onDeleteClick = {
                        // TODO: Eliminar el producto
                    }
                )
                // ★Agregar hasta aquí.
           }
        }
    }
}
```

## 13. Modicar Detailview

```diff
 @Composable
 fun DetailView(viewModel: ProductViewModel, navController: NavController, id: Int) {
    // ★Agregar hacia abajo desde aquí.
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
    // ★Agregar hasta aquí.
     Scaffold(
         topBar = {
             MainTopBar(
-                title = "Product Detail",
                 title = viewModel.state.title, // ★Agregar
                 showBackButton = true,
                 onCickBackButton = { navController.popBackStack() },
                 onCickAction = {}
...Omitido...
 @Composable
 fun ContentDetailView(padd: PaddingValues, viewModel: ProductViewModel) {
    val state = viewModel.state // ★Agregar
     Column(
         modifier = Modifier
             .padding(padd)
     ) {
-        Image(
-            painter = painterResource(id = R.drawable.star_on),
        AsyncImage(               // ★Agregar
            model = state.image,  // ★Agregar
             contentDescription = null,
             contentScale = ContentScale.Fit, // Escala la imagen para que se ajuste completamente al contenedor sin recortarla.
             modifier = Modifier
...Omitido...
         ) {
             Text(text = "Price: ", color = Color.Green, style = MaterialTheme.typography.titleLarge)
             Text(
-                text = "1",
                text = String.format("$%.2f", state.price), // ★Agregar
                 color = Color.Green,
                 style = MaterialTheme.typography.titleLarge
             )
...Omitido...
                 modifier = Modifier.padding(start = 15.dp),
             )
             Text(
-                text = "description producto",
                text = state.description, // ★Agregar
                 color = Color.White,
                 textAlign = TextAlign.Justify,
                 modifier = Modifier.padding(start = 15.dp, end = 15.dp, bottom = 10.dp)
...Omitido...
                 .fillMaxWidth()
                 .padding(start = 20.dp, end = 10.dp, bottom = 10.dp)
         ) {
-            RatingBar(rating = 1.0)
            RatingBar(rating = state.rating.rate) // ★Agregar
             Text(
-                text = "Cat: category producto",
                text = "Cat: ${state.category}",  // ★Agregar
                 color = Color.Green,
                 style = MaterialTheme.typography.bodyLarge
             )
```
- **`LaunchedEffect(Unit) {...}`**:
  - Se ejecuta una sola vez cuando el Composable entra en la pantalla (es decir, cuando se muestra).
  - Usando `Unit` como clave, se asegura que solo se ejecute **una vez**, no en cada recomposición.
  - Funciona de forma similar a `onMounted()` en Vue.js.
- **`DisposableEffect(Unit) {...}`**:
  - `DisposableEffect` se usa para ejecutar lógica **cuando el Composable se elimina de la pantalla** (por ejemplo, al navegar hacia atrás o cambiar de vista).
  - `onDispose` define el bloque de código que se ejecutará cuando el Composable sea descartado.
  - Funciona de forma similar a `onUnmounted()` en Vue.js.

## 14. Modicar MainActivity

```diff
{
-    val productViewModel: ProductViewModel = viewModel()
    val productViewModel: ProductViewModel by viewModels() // ★Agregar
    NavManager(productViewModel)
}
```

## 15. Add parmission a `AndroidManifest.xml`
Es una línea que se agrega en el archivo `AndroidManifest.xml` para declarar que tu aplicación necesita acceso a Internet.
```kotlin
 <manifest xmlns:android="http://schemas.android.com/apk/res/android"
     xmlns:tools="http://schemas.android.com/tools">

    <uses-permission android:name="android.permission.INTERNET" /> // ★Agregar

    <application
        android:name=".ProductApplication"
```

# Añadir la función de eliminación de productos (DELETE)

## 1. Añadir el método DELETE a la interfaz `ApiProduct`
En `ApiProduct`, añadir un método para las solicitudes DELETE de acuerdo con la documentación de la API: https://fakestoreapi.com/docs#tag/Products/operation/deleteProduct

```kotlin
@DELETE("$ENDPOINT/{id}")
suspend fun deleteProduct(@Path("id") id: Int): Response<Unit>
```

## 2. Añadir el método a la clase `ProductRepository`
```kotlin
suspend fun deleteProduct(id: Int): Boolean {
    val response = apiProduct.deleteProduct(id)
    return response.isSuccessful
}
```

## 3. Llamar a métodos del repositorio desde `ProductViewModel`.
```kotlin
fun deleteProductById(id: Int) {
    viewModelScope.launch {
        withContext(Dispatchers.IO) {
            val result = repository.deleteProduct(id)
            if (result) {
                // Eliminar el producto de la lista localmente si la solicitud fue exitosa
                _products.value = _products.value.filter { it.id != id }
            } else {
                // Manejar el error si la eliminación falla
                Log.e("ProductViewModel", "Error deleting product with id: $id")
            }
        }
    }
}
```
## 4. En la UI, establecer el método de ViewModel como manejadores de eventos.
En el archivo `HomeView`, en la función componible `ContentHomeView`, establecer el método `deleteProductById` como manejadores de eventos de botón eliminar.
```kotlin
LazyColumn(
    modifier = Modifier
        .background(Color(Constants.CUSTOM_BLACK))
) {
    items(products) { product ->
        CardProduct(
            product = product,
            onClick = { navController.navigate("DetailView/${product.id}") },
            onEditClick = {},
            onDeleteClick = {
                viewModel.deleteProductById(product.id) // ★Agregar
            }
        )
    }
}
```

# Añadir diálogo de confirmación antes de la eliminación
Se puede mostrar un diálogo de confirmación antes de la eliminación modificando `CardProduct` de la siguiente manera.
- Utilice el componente `AlertDialog` para mostrar un diálogo de confirmación.
- Defina la bandera `showDialog` para controlar si el diálogo se muestra u oculta, y sólo mostrar el diálogo si ture.
```diff
fun CardProduct(
     onEditClick: () -> Unit,
     onDeleteClick: () -> Unit,
 ) {
    var showDialog by remember { mutableStateOf(false) } // ★Agregar

     Card(
         shape = RoundedCornerShape(8.dp),
         modifier = Modifier
...Omitido...
                         tint = Color.White
                     )
                 }
-                IconButton(onClick = { onDeleteClick()}) {
                IconButton(onClick = { showDialog = true }) { // ★Agregar
                     Icon(
                         imageVector = Icons.Default.Delete,
                         contentDescription = "Eliminar",
...Omitido...
             }
         }
     }
    // ★Agregar hacia abajo desde aquí.     
    // mostrar dialogo de confirmacion
    if (showDialog) {
        AlertDialog(
            onDismissRequest = {
                showDialog = false
            },
            title = { Text(text = "Confirmar eliminación") },
            text = { Text("¿Está seguro/a de eliminar este producto?") },
            confirmButton = {
                Button(onClick = {
                    onDeleteClick() // Llamar a la función para eliminar el producto
                    showDialog = false
                }) {
                    Text("Eliminar")
                }
            },
            dismissButton = {
                Button(onClick = {
                    showDialog = false // Cerrar el diálogo sin eliminar
                }
                ) { Text("Cancelar") }
            }
        )//fin del AlertDialog
    }
    // ★Agregar hasta aquí.
 }

```
