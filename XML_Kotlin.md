### **XML en Kotlin**

En Kotlin, el manejo de XML puede realizarse utilizando bibliotecas externas, ya que no cuenta con un soporte nativo para trabajar con XML al nivel de Python. 

Una de las bibliotecas más utilizadas es **JDOM**, que proporciona una API intuitiva para leer, escribir y manipular documentos XML. 

A continuación, se explica cómo manejar archivos XML en Kotlin utilizando esta biblioteca.

---

### **1. Importar la biblioteca JDOM**

Para usar JDOM en Kotlin, agrega la dependencia en el archivo `build.gradle` de tu proyecto:

```kotlin
implementation("org.jdom:jdom2:2.0.6")
```

Luego, importa las clases necesarias:

```kotlin
import org.jdom2.Document
import org.jdom2.Element
import org.jdom2.input.SAXBuilder
import org.jdom2.output.XMLOutputter
import org.jdom2.output.Format
```

---

### **2. Leer archivos XML**

#### **2.1 Leer XML desde un archivo**
Usamos `SAXBuilder` para convertir el contenido de un archivo XML en un objeto `Document`.

Ejemplo:

```kotlin
import org.jdom2.input.SAXBuilder

fun cargarXML(nombreFichero: String): Document? {
    return try {
        val builder = SAXBuilder()
        builder.build(nombreFichero)
    } catch (e: Exception) {
        println("*ERROR* Problemas al cargar el archivo XML: ${e.message}")
        null
    }
}

// Uso:
val documento = cargarXML("datos.xml")
val raiz = documento?.rootElement
println(raiz?.getChild("usuario")?.getChildText("nombre")) // Ejemplo: 'Juan'
```

#### **2.2 Leer XML desde una cadena**
Usamos `StringReader` junto con `SAXBuilder`.

Ejemplo:

```kotlin
import org.jdom2.input.SAXBuilder
import java.io.StringReader

val cadenaXML = """
<usuario>
    <nombre>Juan</nombre>
    <edad>30</edad>
</usuario>
""".trimIndent()

val builder = SAXBuilder()
val documento = builder.build(StringReader(cadenaXML))
val raiz = documento.rootElement

println(raiz.getChildText("edad")) // Ejemplo: '30'
```

---

### **3. Escribir archivos XML**

#### **3.1 Escribir XML en un archivo**
Creamos los elementos con `Element` y luego guardamos el documento con `XMLOutputter`.

Ejemplo:

```kotlin
import org.jdom2.Document
import org.jdom2.Element
import org.jdom2.output.XMLOutputter
import org.jdom2.output.Format
import java.io.File

fun guardarXML(documento: Document, nombreFichero: String) {
    try {
        val outputter = XMLOutputter(Format.getPrettyFormat())
        outputter.output(documento, File(nombreFichero).writer())
        println("Archivo XML guardado en $nombreFichero")
    } catch (e: Exception) {
        println("*ERROR* Problemas al guardar el archivo XML: ${e.message}")
    }
}

// Uso:
val raiz = Element("usuarios")
val documento = Document(raiz)

val usuario = Element("usuario")
usuario.addContent(Element("id").setText("1"))
usuario.addContent(Element("nombre").setText("Ana"))
usuario.addContent(Element("edad").setText("25"))

raiz.addContent(usuario)
guardarXML(documento, "salida.xml")
```

#### **3.2 Convertir elementos a cadenas XML**
Usamos `XMLOutputter` para convertir un documento o elemento a una cadena.

Ejemplo:

```kotlin
import org.jdom2.output.XMLOutputter
import org.jdom2.output.Format

val xmlComoCadena = XMLOutputter(Format.getPrettyFormat()).outputString(documento)
println(xmlComoCadena)
```

---

### **4. Manipular datos XML**

Una vez cargado el archivo XML, puedes modificarlo agregando, actualizando o eliminando elementos.

Ejemplo:

```kotlin
// Leer y modificar datos XML
val documento = cargarXML("datos.xml")
val raiz = documento?.rootElement

// Modificar un elemento
val usuario = raiz?.getChild("usuario")
usuario?.getChild("edad")?.text = (usuario.getChildText("edad").toInt() + 1).toString()

// Añadir un nuevo elemento
val nuevoUsuario = Element("usuario")
nuevoUsuario.addContent(Element("id").setText("2"))
nuevoUsuario.addContent(Element("nombre").setText("Pedro"))
nuevoUsuario.addContent(Element("edad").setText("40"))
raiz?.addContent(nuevoUsuario)

// Guardar los cambios
guardarXML(documento!!, "datos_modificados.xml")
```

---

### **5. Manejo de errores**

Es importante manejar posibles excepciones al trabajar con XML, como archivos inexistentes o problemas de formato.

Ejemplo:

```kotlin
try {
    val documento = SAXBuilder().build("datos.xml")
    val raiz = documento.rootElement
    println(raiz.getChildText("nombre"))
} catch (e: Exception) {
    when (e) {
        is org.jdom2.JDOMException -> println("*ERROR* Problemas con el formato del archivo XML.")
        is java.io.FileNotFoundException -> println("*ERROR* Archivo no encontrado.")
        else -> println("*ERROR* ${e.message}")
    }
}
```

---

### **6. Conclusión**

El manejo de archivos XML en Kotlin es flexible y potente gracias a bibliotecas como JDOM. Estas herramientas permiten trabajar con datos estructurados de forma jerárquica, facilitando tareas como leer, modificar y guardar archivos XML.

Aunque XML puede ser más verboso que JSON, su capacidad para manejar datos complejos con relaciones jerárquicas claras lo hace ideal para muchas aplicaciones. JDOM, en combinación con Kotlin, ofrece una API clara y extensible, permitiendo la integración fluida de XML en proyectos modernos.

---

### **7. Ejemplo Completo**

#### **Archivo XML inicial (`datos.xml`)**
```xml
<usuarios>
    <usuario>
        <id>1</id>
        <nombre>Juan</nombre>
        <edad>30</edad>
    </usuario>
</usuarios>
```

#### **Código en Kotlin**
```kotlin
fun main() {
    val nombreFichero = "datos.xml"

    // 1. Cargar XML
    val documento = cargarXML(nombreFichero) ?: Document(Element("usuarios"))
    val raiz = documento.rootElement

    // 2. Actualizar usuario
    actualizarUsuario(raiz, 1, 31)

    // 3. Insertar nuevo usuario
    val nuevoUsuario = mapOf("id" to "2", "nombre" to "Ana", "edad" to "25")
    insertarUsuario(raiz, nuevoUsuario)

    // 4. Eliminar usuario
    eliminarUsuario(raiz, 1)

    // 5. Guardar los cambios
    guardarXML(documento, nombreFichero)
}

fun actualizarUsuario(raiz: Element, id: Int, nuevaEdad: Int) {
    val usuario = raiz.getChildren("usuario").find { it.getChildText("id").toInt() == id }
    usuario?.getChild("edad")?.text = nuevaEdad.toString()
}

fun insertarUsuario(raiz: Element, nuevoUsuario: Map<String, String>) {
    val usuario = Element("usuario")
    nuevoUsuario.forEach { (clave, valor) -> usuario.addContent(Element(clave).setText(valor)) }
    raiz.addContent(usuario)
}

fun eliminarUsuario(raiz: Element, id: Int) {
    val usuario = raiz.getChildren("usuario").find { it.getChildText("id").toInt() == id }
    if (usuario != null) raiz.removeContent(usuario)
}
```

#### **Resultado Final (`datos.xml`)**
```xml
<usuarios>
    <usuario>
        <id>2</id>
        <nombre>Ana</nombre>
        <edad>25</edad>
    </usuario>
</usuarios>
```
