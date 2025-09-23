Introducción a Jetpack Compose y los elementos básicos de la UI

## **1. Objetivo de la tema**
- Comprender los conceptos básicos de Jetpack Compose.
- Aprender a utilizar la anotación `@Composable`.
- Aprender como comprobar diseño en Android Studio

---

## **2. Introducción a Jetpack Compose**

### **¿Qué es Jetpack Compose?**
Jetpack Compose es el último conjunto de herramientas de UI para Android, diseñado para hacer que la creación de interfaces sea más simple e intuitiva. A diferencia de los diseños basados en XML, Compose permite describir la UI directamente en código Kotlin, adoptando un enfoque de programación reactiva.

### **Ventajas de Jetpack Compose**
- **Código simple**: Todo el diseño se maneja dentro de código Kotlin, lo que mejora la legibilidad.
- **UI reactiva**: La UI se actualiza automáticamente cuando cambia el estado.
- **Reutilización**: Puedes crear componentes UI reutilizables a través de funciones `@Composable`.

---

## **3. Funciones componibles**

### **Anotación `@Composable`**
Todos los componentes UI en Jetpack Compose se definen como funciones anotadas con `@Composable`. Esto permite que los componentes se rendericen en la pantalla.
Se llama `Funciones componibles`.

### **Diseño de UI intuitivo y flexible**
Jetpack Compose es como construir con piezas de Lego. Cada componente de la interfaz de usuario, como `Button` o `Text`, es como una pieza de Lego que puedes combinar de diferentes formas para crear el diseño que quieras. Al igual que cuando juegas con Lego, puedes armar tu interfaz visualmente sin necesidad de herramientas complicadas, solo con código.

#### **Ejemplo: Mostrar un texto simple**

```kotlin
@Composable
fun Saludo() {
    Text(text = "¡Hola, Jetpack Compose!")
}
```

#### **Ejemplo: Mostrar un Button simple**

```kotlin
@Composable
fun BotonSimple() {
    Button(onClick = { /* Acción cuando se pulsa el botón */ }) {
        Text("Haz clic aquí")
    }
}
```

#### **Ejemplo: Mostrar una pantalla de la aplicación**

```kotlin
package com.example.ejemploscomponentes

import android.os.Bundle
import androidx.activity.ComponentActivity
import androidx.activity.compose.setContent
import androidx.activity.enableEdgeToEdge
import androidx.compose.foundation.layout.Arrangement
import androidx.compose.foundation.layout.Column
import androidx.compose.foundation.layout.Row
import androidx.compose.foundation.layout.fillMaxSize
import androidx.compose.foundation.layout.fillMaxWidth
import androidx.compose.foundation.layout.padding
import androidx.compose.foundation.text.KeyboardOptions
import androidx.compose.material3.ButtonDefaults
import androidx.compose.material3.Card
import androidx.compose.material3.OutlinedButton
import androidx.compose.material3.OutlinedTextField
import androidx.compose.material3.Text
import androidx.compose.runtime.Composable
import androidx.compose.ui.Alignment
import androidx.compose.ui.Modifier
import androidx.compose.ui.graphics.Color
import androidx.compose.ui.text.input.KeyboardType
import androidx.compose.ui.tooling.preview.Preview
import androidx.compose.ui.unit.dp
import androidx.compose.ui.unit.sp
import com.example.ejemploscomponentes.ui.theme.EjemplosComponentesTheme

class MainActivity : ComponentActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        enableEdgeToEdge()
        setContent {
            EjemplosComponentesTheme {
            }
        }
    }
}

@Composable
fun CalculatorApp() {
    Column(modifier= Modifier
        .padding(10.dp)
        .fillMaxSize(),
        //verticalArrangement = Arrangement.Center,
        horizontalAlignment = Alignment.CenterHorizontally
    ) {
        // Tarjetas con el importe total y el descuento
        Row(modifier = Modifier.fillMaxWidth(),
            horizontalArrangement = Arrangement.SpaceEvenly
        ) {
            // Tarjeta con el importe total
            Card(
                modifier = Modifier.padding(8.dp).weight(1f)
            ) {
                Column(
                    verticalArrangement = Arrangement.Center,
                    horizontalAlignment = Alignment.CenterHorizontally,
                    modifier = Modifier.padding(16.dp)
                ) {
                    Text(text = "Total", color = Color.Black, fontSize = 20.sp)
                    Text(text = "$0.0", color = Color.Black, fontSize = 20.sp)
                }
            }
            // Tarjeta con el descuento
            Card(
                modifier = Modifier.padding(8.dp).weight(1f)
            ) {
                Column(
                    verticalArrangement = Arrangement.Center,
                    horizontalAlignment = Alignment.CenterHorizontally,
                    modifier = Modifier.padding(16.dp)
                ) {
                    Text(text = "Descuento", color = Color.Black, fontSize = 20.sp)
                    Text(text = "$0.0", color = Color.Black, fontSize = 20.sp)
                }
            }
        }
        // TextField para introducir el precio
        OutlinedTextField(
            value = "",
            onValueChange = {},
            label = { Text(text = "Precio")},
            //mostrando el teclado numerico
            keyboardOptions = KeyboardOptions(keyboardType = KeyboardType.Number),
            modifier = Modifier
                .fillMaxWidth()
                .padding(horizontal = 32.dp)
        )
        // TextField para introducir el porcentaje de descuento
        OutlinedTextField(
            value = "",
            onValueChange = {},
            label = { Text(text = "Descuento %")},
            //mostrando el teclado numerico
            keyboardOptions = KeyboardOptions(keyboardType = KeyboardType.Number),
            modifier = Modifier
                .fillMaxWidth()
                .padding(horizontal = 32.dp)
        )
        // Button para generar descuento
        OutlinedButton(
            onClick = { },
            modifier = Modifier.fillMaxWidth().padding(horizontal = 30.dp)
        ) {
            Text(text = "Generar Descuento")
        }
        // Button para Limpiar
        OutlinedButton(
            onClick = {},
            colors = ButtonDefaults.outlinedButtonColors(contentColor = Color.Red),
            modifier = Modifier.fillMaxWidth().padding(horizontal = 30.dp)
        ) {
            Text(text = "Limpiar")
        }
    }
}

@Preview(showBackground = true)
@Composable
fun CalculatorAppPreview() {
    EjemplosComponentesTheme {
        CalculatorApp()
    }
}
```
## **4. Comprobar diseño en Android Studio**

### REPASO: crear un proyecto de Empty Activity

1.En el diálogo Welcome to Android Studio, selecciona la opción New Project.<br>
Si ya tiene un proyecto abierto, seleccione `File` > `New` > `New Project`.

![image](https://github.com/user-attachments/assets/4142660e-4d63-45df-900d-9ec329d6b00a)


2.En el diálogo New Project, selecciona `Empty Activity` y haz clic en Next.

![image](https://github.com/user-attachments/assets/65817ddb-f61e-47cb-a011-8e4407d56eb9)

3.En el campo Name, ingresa `EjemplosComponentes`, selecciona un nivel de API mínimo de 24 (Nougat) en el campo Minimum SDK y haz clic en Finish.

![image](https://github.com/user-attachments/assets/cb006886-da5f-4f0f-b06f-4d59d1d8772c)

4.Espera a que Android Studio cree los archivos del proyecto y compílalo.

### Previsualizar el diseño del componentes

Android Studio te permite obtener una vista previa de las funciones de componibilidad dentro del IDE, en lugar de instalar la app en un emulador o dispositivo Android.
Puede definir qué previsualizar creando una función con la anotación `@Preview`

1.Eliminamos las funciones `Greeting` y `GreetingPreview` en el achivo `MainActivity.kt`.

2.Pegue el código de `Ejemplo: Mostrar una pantalla de la aplicación` en el achivo `MainActivity.kt`.

3.Esta vez, haga clic en el icono `Split` para ver tanto el código como la vista previa del diseño.

![image](https://github.com/user-attachments/assets/f94c115f-2695-4f67-9029-4fc06ba8d8ec)

### Ejecutar la aplicación para comprobar el diseño.

Para ejecutar la aplicación y comprobar el componente creado, debe ser llamado el componente dentro de la función `onCreate`.

1.Llama al componente `CalculatorApp` en el bloque `EjemplosComponentesTheme` de la función `onCreate`.

2.Ejecuta la aplicación.

- JetpackCompose te permite crear plantillas de colores y estilos para unificar el diseño en tu aplicación.
- `XXXTheme` es un componente para aplicar plantillas de color y estilo.

## **5. ¿Qué es Modifier en Jetpack Compose?**
`Modifier` en Jetpack Compose es una herramienta que te permite personalizar y ajustar la apariencia y el comportamiento de los elementos de la interfaz de usuario (**Composables**). 

Es un mecanismo flexible que se usa para tareas como:
- Cambiar el tamaño o posición.
- Agregar márgenes o relleno.
- Aplicar colores de fondo.
- Detectar eventos como clics.

---

### **Sintaxis básica**
Se aplica un `Modifier` directamente a un Composable:

```kotlin
Text(
    text = "¡Hola, Modifier!",
    modifier = Modifier
        .padding(16.dp) // Relleno
        .background(Color.Cyan) // Color de fondo
)
```

### **Ejemplos comunes de Modifier**

#### 1. **Relativo al diseño (Layout)**
- **padding**: Agrega espacio interno alrededor del contenido.
  ```kotlin
  Modifier.padding(8.dp)
  ```
- **size**: Define un tamaño fijo para el Composable.
  ```kotlin
  Modifier.size(100.dp)
  ```
- **fillMaxSize / fillMaxWidth / fillMaxHeight**: Expande el Composable para ocupar todo el espacio disponible.
  ```kotlin
  Modifier.fillMaxSize()
  ```

#### 2. **Decoración**
- **background**: Define un color o una imagen de fondo.
  ```kotlin
  Modifier.background(Color.Red)
  ```
- **border**: Agrega un borde al Composable.
  ```kotlin
  Modifier.border(2.dp, Color.Black)
  ```
---

### **¿A qué equivale `Modifier` en JavaScript?**
En el contexto de JavaScript, `Modificador` es una equivalente de las **propiedades CSS**.
`Modifier` se utiliza para ajustar el **estilo y el diseño** de los elementos de la interfaz, algo muy similar a lo que hacemos con CSS. 

#### Jetpack Compose (`Modifier`)
```kotlin
Box(
    modifier = Modifier
        .size(100.dp)
        .padding(16.dp)
        .background(Color.Gray)
)
```

#### HTML + CSS
```html
<div style="
    width: 100px;
    height: 100px;
    padding: 16px;
    background-color: gray;
"></div>
```

En este caso, las funciones de `Modifier` como `size`, `padding` o `background` se asemejan a las propiedades CSS para definir tamaño, márgenes internos y color de fondo.
