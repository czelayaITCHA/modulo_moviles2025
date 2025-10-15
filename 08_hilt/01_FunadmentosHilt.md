# Hilt: Inyección de dependencias en aplicaciones Android
Hilt es una biblioteca de una biblioteca de inyección de dependencias (DI). Se integra muy bien con Jetpack Compose.

## ¿Por qué usar la inyección de dependencias (DI)? 
Hilt le ayuda a escribir **código más limpio, mantenible y escalable**, reduciendo la complejidad de gestionar manualmente las dependencias.
- Hilt permite que las dependencias (como DAO, servicios de red, etc.) se inyecten automáticamente sin tener que crearlas manualmente en cada clase.
- Hilt elimina la necesidad de escribir mucho código repetido para inicializar objetos.

# Pasos para implementar inyección de dependencias con Hilt

## Preparación Previa: Clonación del código inicial
Clona el siguiente repositorio en AndroidStudio.

https://github.com/itcha-organization/ListaUsuarios

## 1. **Agregar plug-in y dependencias**
Primero, necesitas agregar plug-in y dependencias.

Añade el siguiente plugin a `build.gradle.kts` en el directorio raíz.
```kotlin
plugins {
  ...Arriba omitido...
    id("com.google.dagger.hilt.android") version "2.56.2" apply false
}
```

Necesita agregar plugin y dependencias. Añade siguiente codigo en tu archivo build.gradle.kts (:app).
```kotlin
plugins {
  ...Arriba omitido...
    id("com.google.dagger.hilt.android")
}
```
```kotlin
dependencies {
    ...Arriba omitido...
    val hilt_version = "2.56.2"
    implementation("com.google.dagger:hilt-android:$hilt_version")
    ksp("com.google.dagger:hilt-android-compiler:$hilt_version")
}
```
Haz clic en `Sync Now` para sincronizar.
![image](https://github.com/user-attachments/assets/85874ffe-2187-451f-94f1-ba4e9a29018c)


## 2. **Crear la clase Application Anotado con @HiltAndroidApp**
Crear una clase `RoomApplication`.
```kotlin
import android.app.Application
import dagger.hilt.android.HiltAndroidApp

@HiltAndroidApp
class RoomApplication: Application()
```
En AndroidManifest.xml, añada la siguiente línea.
```xml
<application
    android:name=".RoomApplication"
    ... >
</application>
```
- **`@HiltAndroidApp`**
  - Crea el contenedor raíz de Hilt para toda la aplicación.
  - Necesario para permitir que Hilt funcione correctamente en todo el proyecto.

## 3. **Anotar MainActivity con @AndroidEntryPoint**
```kotlin
@AndroidEntryPoint
class MainActivity : AppCompatActivity() { ... }
```
- **`@AndroidEntryPoint`**:
  - Esto permite a Hilt inyectar dependencias como ViewModel dentro de esta actividad.

## 4. **Crear un módulo y Definir cómo se crean las instancias.**
Crear una clase `AppModule`.
```kotlin
import android.content.Context
import androidx.room.Room
import com.example.ejemploroom3.data.UsuarioDao
import com.example.ejemploroom3.data.UsuarioDatabase
import dagger.Module
import dagger.Provides
import dagger.hilt.InstallIn
import dagger.hilt.android.qualifiers.ApplicationContext
import dagger.hilt.components.SingletonComponent
import javax.inject.Singleton

@Module
@InstallIn(SingletonComponent::class)
object AppModule {

    @Provides
    @Singleton
    fun provideUsuarioDatabase(@ApplicationContext context: Context): UsuarioDatabase {
        // Crear una instancia de UsuarioDatabase
        return Room.databaseBuilder(
            context,
            UsuarioDatabase::class.java,
            "db_usuarios"
        ).build()
    }

    @Provides
    @Singleton
    fun provideUsuarioDao(database: UsuarioDatabase): UsuarioDao {
        // Obtener una instancia de DAO de UsuarioDatabase.
        return database.usuarioDao()
    }
}
```
- **`@Module`**:
  - Declara una clase como módulo que proporciona dependencias.
- **`@InstallIn(...)`**:
  - Especifica el ciclo de vida (scope) del módulo, como `SingletonComponent`.
  - `SingletonComponent` representa el contenedor de dependencias que vive durante toda la vida de la aplicación.
  - Se combina con `@Singleton` para que los objetos creados también sean únicos (singleton) a nivel de aplicación.
- **`@Provides`**:
  - Define cómo crear una instancia manualmente.
- **`@Singleton`**:
  - Una sola instancia en toda la app.
- `@ApplicationContext`:
  - Permite inyectar un Context global que vive durante toda la vida de la app.
  - Sin @ApplicationContext, se producirá el error «No sé qué Contexto utilizar».

## 5. **Anotar ViewModel con @HiltViewModel y constructor anotado con @Inject**
```
@HiltViewModel
class UsuarioViewModel @Inject constructor(
    private val dao: UsuarioDao
) : ViewModel() {
    ...
}
```
-  **`@HiltViewModel`**
    -  Habilita la inyección automática del ViewModel.
-  **`@Inject`**
    -  Permite a Hilt saber cómo crear la instancia.

## 6. **Usar `by viewModels()` para inicializar el ViewModel **

Usar by viewModels() o hiltViewModel() para inicializar el ViewModel y eliminar el código manual de creación.
```diff
import androidx.activity.viewModels //★Agregar

 class MainActivity : ComponentActivity() {
-    @SuppressLint("ViewModelConstructorInComposable")
     override fun onCreate(savedInstanceState: Bundle?) {
         super.onCreate(savedInstanceState)
         enableEdgeToEdge()
         setContent {
             EjemploRoom3Theme {
-                // Crear una instancia de UsuarioDatabase
-                val database = Room.databaseBuilder(
-                    this,
-                    UsuarioDatabase::class.java,
-                    "db_usuarios"
-                ).build()
-                // Obtener una instancia de DAO de UsuarioDatabase.
-                val dao = database.usuarioDao()
-                // Inicializar el ViewModel dando la instancia DAO al constructor.
-                val viewModel = UsuarioViewModel(dao)
                 val viewModel: UsuarioViewModel by viewModels() //★Agregar
                 ListaUsuariosView(viewModel)
             }
         }
```
- **`viewModels()`**:
  - Cuando se usa con Hilt, permite inyectar automáticamente las dependencias del ViewModel.
