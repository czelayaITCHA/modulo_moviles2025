# **Uso de componentes básicos en Jetpack Compose**

## 1. **Text** – Mostrar texto
**Text** es el componente básico para mostrar texto en pantalla.

```kotlin
@Composable
fun TextExample() {
    Text(text = "Hola, Jetpack Compose", fontSize = 24.sp)
}
```
Puntos clave:
- Usa la propiedad `text` para especificar el texto a mostrar.
- Con `fontSize` puedes cambiar el tamaño de la fuente.

---

## 2. **Column** – Layout en dirección vertical
**Column** organiza sus hijos de forma vertical.

```kotlin
@Composable
fun ColumnExample() {
    Column(
        modifier = Modifier.padding(16.dp)
    ) {
        Text(text = "Nombre: Kyo Onuma")
        Text(text = "Profesión: Profesor")
    }
}
```
Puntos clave:
- `Column` organiza varios componentes en una disposición vertical.
- Usa `padding` para ajustar el espacio interior alrededor de los componentes.

**Ejemplo: lleva sus hijos alineados**
```kotlin
@Composable
fun ColumnExampleAlineado() {
    Column(
        modifier = Modifier.padding(16.dp).fillMaxSize(),
        verticalArrangement = Arrangement.Center,
        horizontalAlignment = Alignment.CenterHorizontally

    ) {
        Text(text = "Nombre: Kyo Onuma")
        Text(text = "Profesión: Profesor")
    }
}
```
## Puntos clave:
- Usa `fillMaxSize` para llenar el tamaño máximo disponible de su contenedor padre.
- El parámetro `verticalArrangement` se establece en `Arrangement.Center`, lo que centra los elementos hijos verticalmente dentro de la columna.
- De manera similar, el parámetro `horizontalAlignment` se establece en `Alignment.CenterHorizontally`, lo que centra los elementos hijos horizontalmente.

**Variaciones del parámetro `verticalArrangement` de `Column`:**

![image](https://github.com/user-attachments/assets/4459f63d-5240-4ac1-b437-54666a419ca9)

---

## 3. **Row** – Layout en dirección horizontal
**Row** organiza sus hijos de forma horizontal.

```kotlin
@Composable
fun RowExample() {
    Row(
        modifier = Modifier.padding(16.dp)
    ) {
        Text(text = "Nombre:")
        Text(text = "Kyo Onuma")
    }
}
```
Puntos clave:
- `Row` permite alinear componentes horizontalmente.
- Usa `Modifier` para personalizar el diseño y estilo.

**Ejemplo: lleva sus hijos alineados**
```kotlin
@Composable
fun RowExampleAlineado() {
    Row(
        modifier = Modifier.padding(16.dp).fillMaxSize(),
        horizontalArrangement = Arrangement.Center,
        verticalAlignment = Alignment.CenterVertically
    ) {
        Text(text = "Nombre:")
        Text(text = "Kyo Onuma")
    }
}
```
Puntos clave:
- Usa `fillMaxSize` para llenar el tamaño máximo disponible de su contenedor padre.
- El parámetro horizontalArrangement se establece en Arrangement.Center, lo que centra los elementos hijos horizontalmente dentro de la fila.
- De manera similar, el parámetro verticalAlignment se establece en Alignment.CenterVertically, lo que centra los elementos hijos verticalmente.

**Variaciones del parámetro `horizontalArrangement` de `Row`:**

![image](https://github.com/user-attachments/assets/cf06c427-e0c9-4dab-a1d2-dd22d56af7e0)

---

## 4. **Spacer** – Componente para añadir espacio
**Spacer** es utilizado para agregar espacio entre otros componentes.

```kotlin
@Composable
fun SpacerExample() {
    Column(
        modifier = Modifier.padding(16.dp)
    ) {
        Text("Texto superior")
        Spacer(modifier = Modifier.height(16.dp)) // Añade un espacio de 16dp
        Text("Texto inferior")
    }
}
```
## Puntos clave:
- Usa `height` o `width` para definir el tamaño del espacio.
- Agrega espacio entre los componentes para mejorar el diseño visual.

## **Ejercicios: Crear diseños usando Text, Column y Row**

### **Ejercicio 1: Mostrar nombre y edad de forma vertical**

#### Enunciado:
Usa el componente `Text` para mostrar el nombre y la edad **de manera vertical** como se muestra a continuación.

#### Ejemplo de diseño:
![image](https://github.com/user-attachments/assets/14ab5857-861e-4caa-b5aa-7acbc463089f)

#### Pista:
- Utiliza `Column` para organizar el nombre y la edad de forma vertical.

<details>
  <summary>Ejemplo de solución</summary>
  
   ```kotlin
   @Composable
   fun NombreYEdad() {
       Column {
           Text(text = "Nombre: Kyo Onuma")
           Text(text = "Edad: 25 años")
       }
   }
   ```    
</details>

---

### **Ejercicio 2: Mostrar título y subtítulo de forma vertical**

#### Enunciado:
Usa el componente `Text` para mostrar un título y un subtítulo **de manera vertical**. El título debe tener un tamaño de fuente más grande, y el subtítulo uno más pequeño.

#### Ejemplo de diseño:
![image](https://github.com/user-attachments/assets/2e67dbc8-7f0c-4a41-83ea-800650a016b4)

#### Pista:
- Utiliza `Column` para organizar los textos de manera vertical.
- Cambia el tamaño de la fuente usando `fontSize`.

<details>
  <summary>Ejemplo de solución</summary>
  
   ```kotlin
   @Composable
   fun TituloYSubtitulo() {
       Column {
           Text(text = "Título", fontSize = 24.sp)
           Text(text = "Subtítulo", fontSize = 16.sp)
       }
   }
   ```    
</details>

---

### **Ejercicio 3: Mostrar información del perfil de forma horizontal**

#### Enunciado:
Usa el componente `Text` para mostrar el nombre y la edad **de manera horizontal**.

#### Ejemplo de diseño:
![image](https://github.com/user-attachments/assets/289aba65-59b4-4018-ba41-ce32048bf2ba)

#### Pista:
- Utiliza `Row` para alinear los textos horizontalmente.
- Utiliza `Spacer` para insertar un espacio de anchura `8.dp` entre `Text`.
  
<details>
  <summary>Ejemplo de solución</summary>
  
   ```kotlin
   @Composable
   fun PerfilHorizontal() {
       Row {
           Text(text = "Nombre: Kyo Onuma")
           Spacer(modifier = Modifier.width(8.dp))
           Text(text = "Edad: 35 años")
       }
   }
   ```    
</details>

---

### **Ejercicio 4: Mostrar lista de productos y precios**

#### Enunciado:
Crea una lista de productos y precios en la que los nombres de los productos y los precios se muestren de forma horizontal, y los diferentes productos se muestren en líneas verticales.

#### Ejemplo de diseño:
![image](https://github.com/user-attachments/assets/72b79d16-fa05-4af2-a745-ff2bc5dce780)

#### Pista:
- Utiliza `Column` para alinear los productos de forma vertical y `Row` para alinear el nombre y el precio horizontalmente en cada línea.
- Utiliza `Spacer` para insertar un espacio de altura `16.dp` entre `Text`.

