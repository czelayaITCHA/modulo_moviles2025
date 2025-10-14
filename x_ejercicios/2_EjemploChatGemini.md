# Ejemplo de Chatbot usando Gemini AI

## Introducción
## Creación del Proyecto e instalación de dependencias
### 1. Crear nuevo proyecto con el nombre ChatWithGeminiAI
### 2. Obtener una API KEY de Google, para usar el modelo de Gemini AI en androd
Obtenga la API KEY en esta url 1.	Obtener la API Key de Gemini para Android, en 
https://aistudio.google.com/prompts/new_chat
### 3. Agregar permisos para usar internet en el archivo AndroidManifest.xml
```xml
<uses-permission android:name="android.permission.INTERNET" />
```
### 4. Agregar dependencia de Google Gemini Generative en el build-gradle.kts (module:app)

```kotlin
implementation("com.google.ai.client.generativeai:generativeai:0.9.0")
```
Luego sincronizar el proyecto, despues puede implementar la forma de implementacion que sugiere el IDE

### 5. Crear variable para apikey en el archivo local.properties
```xml
apikey="<TU_API_KEY>"
```
### 6. Hacer configuraciones para acceder al valor de la apikey
#### 6.1 Agregar al build-gradle.kts del proyecto 
```kotlin
  buildscript{
    dependencies{
        classpath("com.google.android.libraries.mapsplatform.secrets-gradle-plugin:secrets-gradle-plugin:2.0.1")
    }
}

```
No sincronice proyecto aún
#### 6.2 Agregar plugin al build-gradle.kts del module:app
```kotlin
  id("com.google.android.libraries.mapsplatform.secrets-gradle-plugin")
```
y también 
```kotlin
   buildFeatures {
        compose = true
        buildConfig = true
    }
```
Ahora si sincronizar el proyecto y se puede volver a la forma de implementacion sugerida

### 7. crear packages necesarios para organizar el código de la aplicación

<img width="265" height="195" alt="image" src="https://github.com/user-attachments/assets/254362a7-f689-4c5b-a219-2880f4f81769" />

### 8. Crear ViewModel inicial
```kotlin
  package com.devsoft.chatwithgeminiai.viewmodels

import androidx.lifecycle.ViewModel
import com.devsoft.chatwithgeminiai.BuildConfig
import com.google.ai.client.generativeai.GenerativeModel

class GeminiViewModel: ViewModel() {

    //inicializar la AI
    private val generativeModel = GenerativeModel(
        modelName = "gemini-2.5-flash",
        apiKey = BuildConfig.apikey //la de nuestro proyecto
    )

}
```
### 9.  Crear el archivo BodyComponentes dendro del package components, y crear la función composable MessageInput

antes definir colores personalizados en el archivo Color.kt
```kotlin
val focusColor = Color(0xFF27252B)
val unFocusedColor = Color(0xFF27252B)
val backColor = Color(0xFF332F36)
val accentColor = Color(0xFF4A90E2)
```

```kotlin
@Composable
fun MessageInput(onClick: (String) -> Unit) {
    var message by remember { mutableStateOf("") }

    Row(
        modifier = Modifier
            .fillMaxWidth()
            .padding(8.dp)
            .background(backColor, RoundedCornerShape(24.dp))
            .padding(horizontal = 12.dp, vertical = 4.dp),
        verticalAlignment = Alignment.CenterVertically
    ) {
        TextField(
            modifier = Modifier.weight(1f),
            value = message,
            onValueChange = { message = it },
            placeholder = {
                Text(text = "Escribe algo...", color = Color.Gray.copy(alpha = 0.7f))
            },
            maxLines = 4,
            colors = TextFieldDefaults.colors(
                focusedContainerColor = Color.Transparent,
                unfocusedContainerColor = Color.Transparent,
                focusedTextColor = Color.White,
                cursorColor = Color.White
            ),
            textStyle = LocalTextStyle.current.copy(color = Color.White),
            shape = RoundedCornerShape(20.dp)
        )

        IconButton(
            onClick = {
                if (message.isNotBlank()) {
                    onClick(message.trim())
                    message = ""
                }
            },
            modifier = Modifier
                .size(48.dp)
                .clip(CircleShape)
                .background(Color(0xFF4A90E2))
        ) {
            Icon(
                Icons.AutoMirrored.Default.Send,
                contentDescription = "Enviar",
                tint = Color.White
            )
        }
    }
}
```
### 10. el modelo MessageChat, dentro del package models

```kotlin
package com.devsoft.chatwithgeminiai.models

data class MessageChat(
    val message: String,
    val role: String
)

```
### 11. Crear el componente para mostrar los mensajes de user y el modelo de AI
```kotlin
@Composable
fun GlobeMessage(messageChat: MessageChat) {
    val isModel = messageChat.role == "model"
    val bubbleColor = if (isModel) Color(0xFF1E1E1E) else Color(0xFF4A90E2)
    val alignment = if (isModel) Alignment.Start else Alignment.End

    Row(
        modifier = Modifier
            .fillMaxWidth()
            .padding(horizontal = 12.dp, vertical = 4.dp),
        horizontalArrangement = if (isModel) Arrangement.Start else Arrangement.End
    ) {
        Box(
            modifier = Modifier
                .clip(RoundedCornerShape(16.dp))
                .background(bubbleColor)
                .padding(12.dp)
                .widthIn(max = 280.dp)
                .shadow(2.dp, RoundedCornerShape(16.dp))
        ) {
            Text(
                text = messageChat.message,
                color = Color.White,
                fontSize = 16.sp,
                lineHeight = 20.sp
            )
        }
    }
}

// agregar esta función para el título
@Composable
fun Title() {
    Text(
        text = "💬 Chat Gemini",
        fontWeight = FontWeight.Bold,
        color = Color.White,
        fontSize = 22.sp,
        modifier = Modifier
            .fillMaxWidth()
            .background(backColor)
            .padding(vertical = 12.dp),
        textAlign = TextAlign.Center
    )
}
```

### 12. Crear la función HomeView dentro del package views

```kotlin
@OptIn(ExperimentalMaterial3Api::class)
@Composable
fun HomeView(){
    Scaffold(
        topBar = {
            TopAppBar(title = { Title() },
                colors = TopAppBarDefaults.topAppBarColors(
                    containerColor = backColor
                ))
        }
    )
    {innerPadding ->
        Column(
            verticalArrangement = Arrangement.Center,
            horizontalAlignment = Alignment.CenterHorizontally,
            modifier = Modifier
                .padding(innerPadding)
                .background(backColor)
        ) {
            MessageInput { 
                //Pendiente
            }
        }

    }
}
```
Nota: probar la aplicación para ver como va el diseño, antes llamar a HomeView desde el evento onCreate de MainActivity

```kotlin
class MainActivity : ComponentActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        enableEdgeToEdge()
        setContent {
            ChatWithGeminiAITheme {
                Surface(
                    modifier = Modifier.fillMaxSize()
                ) {
                    HomeView()
                }
            }
        }
    }
}
```

Hasta el momento el diseño debe ser como la imágen siguiente:

<img width="259" height="547" alt="image" src="https://github.com/user-attachments/assets/c4cd25a9-90e4-4ec1-8ee5-ddaefedb9dab" />


### 13. Definir funciones para gestionar el chat en GeminiViewModel

```kotlin
private val chat by lazy {
        generativeModel.startChat()
    }

    val messageList by lazy {
        mutableStateListOf<MessageChat>()
    }

    fun sendMessage(question: String){
        viewModelScope.launch {
            try{
                messageList.add(MessageChat(question, "user"))
                val response = chat.sendMessage(question)
                messageList.add(MessageChat(response.text.toString(), role = "model"))
            }catch (e: Exception){
                messageList.add(MessageChat(message = "Error en la conversación: ${e.message}", role = "model"))
            }
        }
    }
```

### 14. Ajustar HomeView y crear funcion ChatContent

```kotlin
@OptIn(ExperimentalMaterial3Api::class)
@Composable
fun HomeView(viewModel: GeminiViewModel){
    Scaffold(
        topBar = {
            TopAppBar(title = { Title() },
                colors = TopAppBarDefaults.topAppBarColors(
                    containerColor = backColor
                ))
        }
    )
    {innerPadding ->
        Column(
            verticalArrangement = Arrangement.Center,
            horizontalAlignment = Alignment.CenterHorizontally,
            modifier = Modifier
                .padding(innerPadding)
                .background(backColor)
        ) {
            ChatContent(modifier = Modifier.weight(1f), viewModel)
            MessageInput {
                viewModel.sendMessage(it)
            }
        }

    }
}

@Composable
fun ChatContent(modifier: Modifier, viewModel: GeminiViewModel){
    LazyColumn(
        modifier = modifier,
        reverseLayout = true
    ) {
        items(viewModel.messageList.reversed()){
            GlobeMessage(it)
        }
    }
}
```

*Nota:* actualiza MainActivity
```kotlin
    val viewModel: GeminiViewModel by viewModels()
        setContent {
            ChatWithGeminiAITheme {
                Surface(
                    modifier = Modifier.fillMaxSize()
                ) {
                    HomeView(viewModel)
                }
            }
        }
```
Probar la aplicación y el modelo debe responder ...

### 15. Implementar persistencia de datos con ROOM para guardar las conversaciones
#### 15.1 Agregar dependencias 
En la sección de plugin del build-gradle.kts del module:app
```kotlin
  id("com.google.devtools.ksp") version "2.0.20-1.0.24"
```
En la sección de dependencies 
```kotlin
 val room_version = "2.6.1"
    implementation("androidx.room:room-runtime:$room_version")
    annotationProcessor("androidx.room:room-compiler:$room_version")
    implementation("androidx.room:room-ktx:$room_version")
    ksp("androidx.room:room-compiler:$room_version")
```
#### 15.2 Crear la entidad ChatModel, en el package models
```kotlin
import androidx.room.Entity
import androidx.room.PrimaryKey

@Entity(tableName = "chatbot")
data class ChatModel(
    @PrimaryKey(autoGenerate = true)
    val id: Long = 0,
    val chat: String,
    val role: String
)
```
### 15.3 Programar la interface en el package room
```kotlin
@Dao
interface ChatDao {

    @Query("SELECT * FROM chatbot ORDER BY id ASC")
    suspend fun getChat() : List<ChatModel>

    @Insert(onConflict = OnConflictStrategy.REPLACE)
    suspend fun insertChat(item: ChatModel)

    @Query("DELETE FROM chatbot")
    suspend fun deleteChat()
}
```
### 15.4 Crear la clase abstracta para definir la base de datos

```kotlin
import androidx.room.Database
import androidx.room.RoomDatabase
import com.devsoft.chatwithgeminiai.models.ChatModel

@Database(entities = [ChatModel::class], version = 1, exportSchema = false)
abstract class AppDatabase: RoomDatabase() {
    abstract fun chatDao() : ChatDao
}
```
### 15.5 Modificar el ViewModel

```kotlin
class GeminiViewModel(application: Application): AndroidViewModel(application) {

    private val db = Room.databaseBuilder(
        application,
        AppDatabase::class.java,
        "chatbot_db"
    ).build()

    //inicializar la AI
    private val generativeModel = GenerativeModel(
        modelName = "gemini-2.5-flash",
        apiKey = BuildConfig.apikey //la de nuestro proyecto
    )

    //lazy retrasar el inicio de las variables hasta que se inicie la funcion de enviar un mensaje
    private val chat by lazy {
        generativeModel.startChat()
    }

    val messageList by lazy {
        mutableStateListOf<MessageChat>()
    }

    /*
    fun sendMessage(question: String){
        viewModelScope.launch {
            try{
                messageList.add(MessageChat(question, "user"))
                val response = chat.sendMessage(question)
                messageList.add(MessageChat(response.text.toString(), role = "model"))
            }catch (e: Exception){
                messageList.add(MessageChat(message = "Error en la conversación: ${e.message}", role = "model"))
            }
        }
    }*/

    // ROOM
    fun sendMessage(question: String){
        viewModelScope.launch {
            try{
                messageList.add(MessageChat(question, "user"))
                val response = chat.sendMessage(question)
                messageList.add(MessageChat(response.text.toString(), role = "model"))
                //implementacion de room
                val chatDao = db.chatDao()
                chatDao.insertChat(ChatModel(chat=question, role = "user"))
                chatDao.insertChat(ChatModel(chat=response.text.toString(), role = "model"))
            }catch (e: Exception){
                messageList.add(MessageChat(message = "Error en la conversación: ${e.message}", role = "model"))
            }
        }
    }

    fun loadChat(){
        try{
            viewModelScope.launch {
                val chatDao = db.chatDao()
                val savedChat = chatDao.getChat()
                messageList.clear()
                for (chat in savedChat){
                    messageList.add(MessageChat(message = chat.chat, role = chat.role))
                }
            }
        }catch (e: Exception){
            messageList.add(MessageChat(message = "Error al cargar el chat: ${e.message}", role = "model"))
        }
    }

}
```
### 15.6 Actualizar la funcion ChatContent, del archivo HomeView

```kotlin
@Composable
fun ChatContent(modifier: Modifier, viewModel: GeminiViewModel){
    LaunchedEffect(Unit){
        viewModel.loadChat()
    }
    LazyColumn(
        modifier = modifier,
        reverseLayout = true
    ) {
        items(viewModel.messageList.reversed()){
            GlobeMessage(it)
        }
    }
}
```

Ejecutar nuevamente la aplicación

### 15.7 crear función para eliminar el chat, en el ViewModel

```kotlin
fun deleteChat(){
        viewModelScope.launch {
            try{
                val chatDao = db.chatDao()
                chatDao.deleteChat()
                messageList.clear()
            }catch (e: Exception){
                messageList.add(MessageChat(message = "Error al eliminar el chat: ${e.message}", role = "model"))
            }
        }
    }
```
Implementar IconButton en topBar de HomeView para eliminar el chat
```kotlin
 Scaffold(
        topBar = {
            TopAppBar(title = { Title() },
                colors = TopAppBarDefaults.topAppBarColors(
                    containerColor = backColor
                ),
                actions = {
                    IconButton(onClick = {viewModel.deleteChat()}) {
                        Icon(Icons.Default.Delete, "Delete", tint = Color.White)
                    }
                }
                )
        }
    )
```
### 16. Probar aplicación

<img width="259" height="549" alt="image" src="https://github.com/user-attachments/assets/d6eacfd1-de73-4518-9c12-146c2ecd8807" />

