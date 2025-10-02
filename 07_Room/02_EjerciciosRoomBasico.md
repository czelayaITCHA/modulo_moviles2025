# Ejercicio Room Basico
Cree una aplicación para almacenar una lista de amigos, como la siguiente.<br>
Hay tres campos que rellenar: `Nombre`, `Apellido`, `Edad`<br>
![image](https://github.com/user-attachments/assets/5547f01b-4e1f-45d4-9002-449119295f6c)

## PREPARACIÓN PREVIA: Creación de una aplicación de vista de lista sin base de datos
- Crear un proyecto `ListaAmigoApp`
- Añadir la dependencia `androidx.lifecycle:lifecycle-viewmodel-compose:2.8.7` y sincronizar
- Crear el paquete `components` bajo `ui`.
- Crear `ListaAmigosView` y `AmigoViewModel` bajo el paquete `components`.
- Escribir código en `ListaAmigosView` para crear la UI.
- Escribir código en `AmigoViewModel` para crear un proceso que añada elementos a la lista después de pulsar un botón.
- Editar la `MainActivity` para que la `ListaAmigosView` sea visible y ejecutar la aplicación.

## PASO1: Agregar plug-in y dependencias
- Añadir el plug-in `com.google.devtools.ksp` y sincronizar
- Añadir las dependencias de Room de vercion `2.6.1` y sincronizar

## PASO2: Crear Entity
- Crear un paquete `data`
- Crear una clase `Amigo` en el paquete `data`
- Definir tablas en `Amigo` con anotaciones

## PASO3: Crear DAO
- Crear una interfaz `AmigoDao` en el paquete `data`
- definir métodos de CRUD con anotaciones

## PASO4: Crear clase de Base de Datos
- Crear una clase abstracta `AmigoDatabase` en el paquete `data`
- Definir la configuración de la base de datos con anotaciones
- Crear un método abstracto que devuelve una instancia de `AmigoDao`

## PASO5: Utilizar DAO dentro del ViewModel
- Añadir `AmigoDao` al parámetro del constructor `AmigoViewModel`.
- Editar para llamar a un método de DAO para añadir datos a la BD.
- Editar para llamar a un método de DAO recuperar los datos de la BD y convertirlos en el valor inicial de la lista de amigos.

## PASO6: Construir la UI con Jetpack Compose
- Importar entidad `Amigo` en `ListaUsuariosView`.
- En MainActivity, crear las instancias de la base de datos y DAO
- En MainActivity, inicializar el ViewModel pasando la instancia de DAO.
- Añadir `@SuppressLint("ViewModelConstructorInComposable")` cuando aparezcan advertencias en el constructor de `AmigoViewModel`.

## PASO7: Usar `Card` para mejorar el diseño
- En `ListaAmigosView`, aplica una `Card` a cada elemento de la `LazyColumn` para mejorar el diseño como se muestra en la imagen de abajo.
  <br>
  ![image](https://github.com/user-attachments/assets/9f2c40a4-fc33-41b2-8b0a-15c5b52e2fd8)

## PASO8: Añadir una función de borrado
- En `AmigoViewModel`, crear un método para borrar datos de la base de datos usando `delete` de DAO.
- En `ListaAmigosView`, añadir un `IconButton` de borrado como se muestra en la imagen de abajo.
- Añadir un método para borrar del ViewModel como acción para el `IconButton` añadido.
  <br>
  ![image](https://github.com/user-attachments/assets/21812715-042c-4688-9990-9901cb9aa232)

