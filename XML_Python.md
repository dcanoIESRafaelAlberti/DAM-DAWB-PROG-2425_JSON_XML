## XML en Python

### **1. Importar el módulo `xml.etree.ElementTree`**

El módulo `xml.etree.ElementTree` permite leer y manipular XML de manera sencilla.

```python
import xml.etree.ElementTree as ET
```

---

### **2. Leer archivos XML**

#### **2.1 Leer XML desde un archivo**
Se usa `ET.parse()` para convertir el contenido del archivo XML en un árbol de elementos.

Ejemplo:
```python
import xml.etree.ElementTree as ET

# Leer el archivo XML
arbol = ET.parse("datos.xml")
raiz = arbol.getroot()

# Acceder a los datos
print(raiz.find("nombre").text)  # Ejemplo: 'Juan'
```

#### **2.2 Leer XML desde una cadena de caracteres**
Para leer XML desde una cadena, se usa `ET.fromstring()`.

Ejemplo:
```python
import xml.etree.ElementTree as ET

cadena_xml = """
<usuario>
    <nombre>Juan</nombre>
    <edad>30</edad>
</usuario>
"""
raiz = ET.fromstring(cadena_xml)

print(raiz.find("edad").text)  # Ejemplo: '30'
```

---

### **3. Escribir archivos XML**

#### **3.1 Escribir XML en un archivo**
Se construyen los elementos con `ET.Element` y luego se escribe en un archivo con `ET.ElementTree.write()`.

Ejemplo:
```python
import xml.etree.ElementTree as ET

# Crear el elemento raíz
raiz = ET.Element("usuarios")

# Añadir un usuario
usuario = ET.SubElement(raiz, "usuario")
ET.SubElement(usuario, "id").text = "1"
ET.SubElement(usuario, "nombre").text = "Ana"
ET.SubElement(usuario, "edad").text = "25"

# Escribir en un archivo XML
arbol = ET.ElementTree(raiz)
arbol.write("salida.xml", encoding="utf-8", xml_declaration=True)
```

#### **3.2 Convertir objetos Python a cadenas XML**

Para convertir datos de Python a XML como cadena, usa `ET.tostring()`.

Ejemplo:
```python
import xml.etree.ElementTree as ET

# Crear el elemento raíz
raiz = ET.Element("usuario")
ET.SubElement(raiz, "nombre").text = "Pedro"
ET.SubElement(raiz, "edad").text = "40"

# Convertir a cadena XML
cadena_xml = ET.tostring(raiz, encoding="unicode")
print(cadena_xml)
```

Salida:
```xml
<usuario>
    <nombre>Pedro</nombre>
    <edad>40</edad>
</usuario>
```

---

### **4. Manipular datos XML**

Después de cargar un archivo XML como un árbol de elementos, puedes manipular los datos con operaciones estándar de Python.

Ejemplo:
```python
import xml.etree.ElementTree as ET

# Leer y modificar datos XML
arbol = ET.parse("datos.xml")
raiz = arbol.getroot()

# Modificar un elemento
raiz.find("edad").text = str(int(raiz.find("edad").text) + 1)

# Añadir un nuevo elemento
habilidades = raiz.find("habilidades")
nueva_habilidad = ET.SubElement(habilidades, "habilidad")
nueva_habilidad.text = "SQL"

# Guardar los cambios
arbol.write("datos.xml", encoding="utf-8", xml_declaration=True)
```

---

### **4. Manejo de errores**

Es importante manejar posibles excepciones al trabajar con XML para evitar errores inesperados.

Ejemplo:
```python
import xml.etree.ElementTree as ET

try:
    arbol = ET.parse("datos.xml")
    raiz = arbol.getroot()
except FileNotFoundError:
    print("*ERROR* Archivo no encontrado.")
except ET.ParseError:
    print("*ERROR* Problemas al analizar el archivo XML.")
except Exception as e:
    print(f"*ERROR* {e}.")
```

---

### **5. Conclusión**

El manejo de archivos XML en Python es versátil y eficaz gracias al módulo integrado xml.etree.ElementTree. 
Este módulo permite trabajar con datos estructurados de forma jerárquica, ofreciendo una interfaz intuitiva para crear, leer, modificar y guardar archivos XML.

XML tiene una estructura basada en etiquetas, que lo hace ideal para representar datos complejos con relaciones jerárquicas claras. 
Las funciones parse, fromstring, y ElementTree proporcionan herramientas esenciales para manipular archivos XML en proyectos pequeños o grandes.

Con un manejo adecuado de excepciones y el uso de funciones auxiliares como find, findall y SubElement, puedes trabajar eficientemente con documentos XML, 
incluso en escenarios avanzados que requieren validación o extensibilidad.

En resumen, Python ofrece una solución robusta y extensible para gestionar datos XML, facilitando su integración en aplicaciones que necesiten intercambiar información en este formato estándar.

---

### **6. Ejemplo Completo**

#### **6.1. Archivo XML inicial (`datos.xml`)**

```xml
<usuarios>
    <usuario>
        <id>1</id>
        <nombre>Juan</nombre>
        <edad>30</edad>
    </usuario>
    <usuario>
        <id>2</id>
        <nombre>Ana</nombre>
        <edad>25</edad>
    </usuario>
</usuarios>
```

#### **6.2. Código en Python**

```python
import xml.etree.ElementTree as ET


def cargar_xml(nombre_fichero: str) -> ET.ElementTree:
    """
    Carga el contenido de un archivo XML.

    Args:
        nombre_fichero (str): Nombre del archivo XML.

    Returns:
        ET.ElementTree: Árbol del XML.
    """
    try:
        return ET.parse(nombre_fichero)
    
    except FileNotFoundError:
        print(f"*ERROR* El archivo {nombre_fichero} no existe.")

    except ET.ParseError:
        print("*ERROR* El archivo XML tiene un formato incorrecto.")

    except Exception as e:
        print(f"*ERROR* Problemas al cargar el XML: {e}")
    
    return None


def guardar_xml(arbol: ET.ElementTree, nombre_fichero: str) -> bool:
    """
    Guarda un árbol XML en un archivo.

    Args:
        arbol (ET.ElementTree): Árbol XML.
        nombre_fichero (str): Nombre del archivo de salida.

    Returns:
        (bool): True si se guardó correctamente y False si se produjo algún problema.
    """
    try:
        arbol.write(nombre_fichero, encoding = "utf-8", xml_declaration = True)

        return True

    except FileNotFoundError:
        print(f"*ERROR* La ruta especificada '{nombre_fichero}' no existe.")

    except PermissionError:
        print(f"*ERROR* No tienes permisos para escribir en el archivo '{nombre_fichero}'.")

    except Exception as e:
        print(f"*ERROR* Problemas al guardar el archivo XML: {e}")

    return False


def actualizar_usuario(raiz: ET.Element, id_usuario: int, nueva_edad: int):
    """
    Actualiza la edad de un usuario dado su ID.

    Args:
        raiz (ET.Element): Nodo raíz del XML.
        id_usuario (int): ID del usuario a actualizar.
        nueva_edad (int): Nueva edad.
    """
    for usuario in raiz.findall("usuario"):
        if usuario.find("id").text == str(id_usuario):
            usuario.find("edad").text = str(nueva_edad)
            print(f"Usuario con ID {id_usuario} actualizado.")
            return
    
    print(f"Usuario con ID {id_usuario} no encontrado.")


def insertar_usuario(raiz: ET.Element, nuevo_usuario: dict):
    """
    Inserta un nuevo usuario en el XML.

    Args:
        raiz (ET.Element): Nodo raíz del XML.
        nuevo_usuario (dict): Datos del nuevo usuario.
    """
    usuario = ET.SubElement(raiz, "usuario")
    ET.SubElement(usuario, "id").text = str(nuevo_usuario["id"])
    ET.SubElement(usuario, "nombre").text = nuevo_usuario["nombre"]
    ET.SubElement(usuario, "edad").text = str(nuevo_usuario["edad"])

    print(f"Usuario {nuevo_usuario['nombre']} añadido con éxito.")


def eliminar_usuario(raiz: ET.Element, id_usuario: int):
    """
    Elimina un usuario por su ID.

    Args:
        raiz (ET.Element): Nodo raíz del XML.
        id_usuario (int): ID del usuario a eliminar.
    """
    for usuario in raiz.findall("usuario"):
        if usuario.find("id").text == str(id_usuario):
            raiz.remove(usuario)
            print(f"Usuario con ID {id_usuario} eliminado.")
            return
    
    print(f"Usuario con ID {id_usuario} no encontrado.")


def main():
    """
    Función principal.
    """
    nombre_fichero = "src/otros/datos_usuarios.xml"

    # 1. Cargar XML
    arbol = cargar_xml(nombre_fichero)

    if arbol is None:
        # Inicializamos datos vacíos si hay error
        raiz = ET.Element("usuarios")
        arbol = ET.ElementTree(raiz)
    
    # Obtenemos el nodo principal y padre de todos
    raiz = arbol.getroot()

    # 2. Actualizar la edad de un usuario
    actualizar_usuario(raiz, id_usuario = 1, nueva_edad = 31)

    # 3. Insertar un nuevo usuario
    nuevo_usuario = {"id": 3, "nombre": "Pedro", "edad": 40}
    insertar_usuario(raiz, nuevo_usuario)

    # 4. Eliminar un usuario
    eliminar_usuario(raiz, id_usuario=2)

    # 5. Guardar los datos de nuevo en el fichero XML
    guardar_xml(arbol, nombre_fichero)

    print("Operaciones completadas. Archivo actualizado.\n")


if __name__ == "__main__":
    main()
```

#### **6.3. Resultado final (`datos.xml`)**

```xml
<usuarios>
    <usuario>
        <id>1</id>
        <nombre>Juan</nombre>
        <edad>31</edad>
    </usuario>
    <usuario>
        <id>3</id>
        <nombre>Pedro</nombre>
        <edad>40</edad>
    </usuario>
</usuarios>
```

El manejo de **XML** en Python requiere más pasos que JSON, pero es igualmente flexible. Este ejemplo muestra cómo cargar, modificar y guardar datos XML de manera eficiente con `xml.etree.ElementTree`.
