## JSON en Kotlin

### **1. Conceptos básicos de JSON**

El formato JSON en Kotlin es similar al de Python: se utiliza para representar pares clave-valor y listas. Es compatible con las estructuras de datos de Kotlin, como `Map` y `List`.

Ejemplo de archivo JSON básico (`datos.json`):
```json
{
    "usuarios": [
        {"id": 1, "nombre": "Juan", "edad": 30},
        {"id": 2, "nombre": "Ana", "edad": 25}
    ]
}
```

En Kotlin, estos datos se pueden mapear a clases de datos (`data class`) para trabajar con ellos de forma estructurada.

---

### **2. Importar y configurar Gson**

Kotlin no tiene soporte nativo para JSON, pero librerías como **Gson** permiten manejarlo fácilmente.

#### **Configuración del proyecto**

1. Añade la dependencia de Gson al archivo `build.gradle.kts`:

```kotlin
dependencies {
    implementation("com.google.code.gson:gson:2.8.9")
}
```

2. Importa la librería Gson en tu código:

```kotlin
import com.google.gson.Gson
```

---

### **3. Leer archivos JSON**

#### **3.1 Leer JSON desde un archivo**

Usamos la clase `File` de Kotlin para leer el archivo como texto y luego `Gson` para convertirlo en objetos de Kotlin.

Ejemplo:

```kotlin
import com.google.gson.Gson
import java.io.File

data class Usuario(val id: Int, val nombre: String, var edad: Int)
data class Datos(val usuarios: MutableList<Usuario>)

fun cargarJson(nombreFichero: String): Datos? {
    return try {
        val json = File(nombreFichero).readText()
        Gson().fromJson(json, Datos::class.java)
    } catch (e: Exception) {
        println("Error al cargar el archivo: ${e.message}")
        null
    }
}
```

Este código:
- Lee el archivo JSON como texto.
- Usa `Gson().fromJson()` para convertir el texto JSON en un objeto `Datos` (con una lista de `Usuario`).

---

#### **3.2 Leer JSON desde una cadena**

Puedes trabajar con cadenas JSON directamente en lugar de un archivo. 

Ejemplo:
```kotlin
val cadenaJson = """
    {"usuarios": [{"id": 1, "nombre": "Juan", "edad": 30}]}
"""
val datos = Gson().fromJson(cadenaJson, Datos::class.java)
println(datos.usuarios[0].nombre) // Salida: Juan
```

---

### **4. Escribir archivos JSON**

#### **4.1 Escribir JSON en un archivo**

Usamos `Gson().toJson()` para convertir un objeto Kotlin a JSON y luego lo guardamos con `File.writeText()`.

Ejemplo:

```kotlin
fun guardarJson(nombreFichero: String, datos: Datos) {
    try {
        val json = Gson().toJson(datos)
        File(nombreFichero).writeText(json)
    } catch (e: Exception) {
        println("Error al guardar los datos: ${e.message}")
    }
}
```

---

#### **4.2 Convertir objetos Kotlin a cadenas JSON**

Si no quieres guardar los datos en un archivo, puedes simplemente convertirlos a una cadena JSON.

Ejemplo:
```kotlin
val datos = Datos(mutableListOf(Usuario(1, "Juan", 30)))
val cadenaJson = Gson().toJson(datos)
println(cadenaJson)
```

Salida:
```json
{
  "usuarios": [
    {
      "id": 1,
      "nombre": "Juan",
      "edad": 30
    }
  ]
}
```

---

### **5. Manipular datos JSON**

Una vez que los datos JSON se cargan en un objeto Kotlin, puedes manipularlos fácilmente como cualquier otra colección.

#### **Actualizar un dato**

Actualiza la edad de un usuario específico buscando su `id`:

```kotlin
fun actualizarUsuario(datos: Datos, idUsuario: Int, nuevaEdad: Int) {
    val usuario = datos.usuarios.find { it.id == idUsuario }
    if (usuario != null) {
        usuario.edad = nuevaEdad
        println("Usuario con ID $idUsuario actualizado.")
    } else {
        println("Usuario con ID $idUsuario no encontrado.")
    }
}
```

#### **Insertar un nuevo usuario**

Añade un nuevo usuario a la lista:

```kotlin
fun insertarUsuario(datos: Datos, nuevoUsuario: Usuario) {
    datos.usuarios.add(nuevoUsuario)
    println("Usuario ${nuevoUsuario.nombre} añadido con éxito.")
}
```

#### **Eliminar un usuario**

Elimina un usuario de la lista buscando su `id`:

```kotlin
fun eliminarUsuario(datos: Datos, idUsuario: Int) {
    val usuario = datos.usuarios.find { it.id == idUsuario }
    if (usuario != null) {
        datos.usuarios.remove(usuario)
        println("Usuario con ID $idUsuario eliminado.")
    } else {
        println("Usuario con ID $idUsuario no encontrado.")
    }
}
```

---

### **6. Manejo de errores**

Usamos bloques `try-catch` para manejar excepciones al leer o escribir archivos JSON.

Ejemplo:

```kotlin
fun cargarJsonSeguro(nombreFichero: String): Datos? {
    return try {
        val json = File(nombreFichero).readText()
        Gson().fromJson(json, Datos::class.java)
    } catch (e: FileNotFoundException) {
        println("Archivo no encontrado: $nombreFichero")
        null
    } catch (e: JsonSyntaxException) {
        println("Error en el formato del archivo JSON.")
        null
    }
}
```

---

### **7. Ejemplo completo**

Con todas las piezas juntas, el flujo completo de operaciones sería:

```kotlin
fun main() {
    val nombreFichero = "datos.json"

    // 1. Cargar datos desde el fichero JSON
    val datos = cargarJson(nombreFichero) ?: Datos(mutableListOf())

    // 2. Actualizar la edad de un usuario
    actualizarUsuario(datos, idUsuario = 1, nuevaEdad = 31)

    // 3. Insertar un nuevo usuario
    val nuevoUsuario = Usuario(id = 3, nombre = "Pedro", edad = 40)
    insertarUsuario(datos, nuevoUsuario)

    // 4. Eliminar un usuario
    eliminarUsuario(datos, idUsuario = 2)

    // 5. Guardar los datos de nuevo en el fichero JSON
    guardarJson(nombreFichero, datos)

    println("Operaciones completadas. Archivo actualizado.")
}
```

---

### **8. Resultado final**

Después de ejecutar el programa, el archivo `datos.json` se verá así:
```json
{
  "usuarios": [
    {"id": 1, "nombre": "Juan", "edad": 31},
    {"id": 3, "nombre": "Pedro", "edad": 40}
  ]
}
```

---

### **9. JSON en un `Map<String, Any>`**

Si necesitas mayor flexibilidad, puedes deserializar el JSON en un `Map`. Esto es útil si no conoces la estructura del JSON o prefieres manipular los datos dinámicamente.

#### **Deserialización a un `Map`**

Con **Gson**, puedes hacerlo utilizando un `TypeToken`:

```kotlin
import com.google.gson.Gson
import com.google.gson.reflect.TypeToken
import java.io.File

fun cargarJsonComoMap(nombreFichero: String): Map<String, Any>? {
    return try {
        val json = File(nombreFichero).readText()
        val tipo = object : TypeToken<Map<String, Any>>() {}.type
        Gson().fromJson<Map<String, Any>>(json, tipo)
    } catch (e: Exception) {
        println("*ERROR* Problemas al cargar el JSON como Map: ${e.message}")
        null
    }
}

fun main() {
    val nombreFichero = "datos.json"
    val datosComoMap = cargarJsonComoMap(nombreFichero)
    if (datosComoMap != null) {
        println("\nDatos cargados como Map:")
        datosComoMap.forEach { (clave, valor) ->
            println("$clave: $valor")
        }
    }
}
```

---

#### **Ejemplo de Salida**

Dado este archivo `datos.json`:

```json
{
    "usuarios": [
        {"id": 1, "nombre": "Juan", "edad": 30},
        {"id": 2, "nombre": "Ana", "edad": 25}
    ]
}
```

**Salida del Programa**:

```
Datos cargados como Map:
usuarios: [{id=1.0, nombre=Juan, edad=30.0}, {id=2.0, nombre=Ana, edad=25.0}]
```

---

### **Conclusión**

El manejo de JSON en Kotlin es muy sencillo gracias a librerías como Gson. Al igual que en Python, puedes realizar operaciones como leer, escribir, actualizar e 
insertar datos con un enfoque modular y utilizando clases de datos (`data class`) para estructurar los datos.
