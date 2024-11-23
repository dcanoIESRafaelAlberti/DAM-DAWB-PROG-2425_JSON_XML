## JSON

Los archivos JSON (JavaScript Object Notation) son un formato de datos ligero utilizado para intercambiar información de manera sencilla. 
En Python, se gestionan principalmente con el módulo integrado `json`, que permite leer, escribir y manipular datos JSON de forma sencilla.

---

### **1. Conceptos básicos de JSON**

Un archivo JSON está estructurado en pares clave-valor, similar a un diccionario de Python. Puede contener objetos (estructuras anidadas) y listas.

Ejemplo de JSON:
```json
{
    "nombre": "Juan",
    "edad": 30,
    "habilidades": ["Python", "JavaScript"],
    "activo": true
}
```

### **2. Importar el módulo `json`**

El módulo `json` está disponible en la biblioteca estándar de Python, por lo que no necesitas instalar nada adicional.

```python
import json
```

---

### **3. Leer archivos JSON**

#### **3.1 Leer JSON desde un archivo**
Se usa `json.load()` para convertir el contenido del archivo JSON en un objeto de Python (normalmente un diccionario o una lista).

Ejemplo:
```python
import json

# Leer el archivo JSON
with open("datos.json", "r") as archivo:
    datos = json.load(archivo)

# Acceder a los datos
print(datos["nombre"])  # Ejemplo: 'Juan'
```

#### **3.2 Leer JSON desde una cadena de caracteres**
Para leer JSON desde una cadena, se usa `json.loads()`.

Ejemplo:
```python
import json

cadena_json = '{"nombre": "Juan", "edad": 30}'
datos = json.loads(cadena_json)

print(datos["edad"])  # 30
```

---

### **4. Escribir archivos JSON**

#### **4.1 Escribir JSON en un archivo**
Se usa `json.dump()` para convertir un objeto de Python a JSON y escribirlo en un archivo.

Ejemplo:
```python
import json

datos = {
    "nombre": "Ana",
    "edad": 25,
    "habilidades": ["HTML", "CSS"],
    "activo": False
}

# Escribir datos en un archivo JSON
with open("salida.json", "w") as archivo:
    json.dump(datos, archivo, indent=4)  # `indent` para una salida legible
```

#### **4.2 Convertir objetos Python a cadenas JSON**

Para convertir datos de Python a una cadena JSON, se usa `json.dumps()`.

Ejemplo:
```python
import json

datos = {"nombre": "Pedro", "edad": 40}

# Convertir a cadena JSON
cadena_json = json.dumps(datos, indent=2)
print(cadena_json)
```

Salida:
```json
{
  "nombre": "Pedro",
  "edad": 40
}
```

---

### **5. Manipular datos JSON**

Después de cargar un archivo JSON como un objeto de Python (como un diccionario o lista), puedes manipular los datos con las operaciones estándar de Python.

Ejemplo:
```python
import json

# Leer y modificar datos JSON
with open("datos.json", "r") as archivo:
    datos = json.load(archivo)

# Modificar los datos
datos["edad"] += 1
datos["habilidades"].append("SQL")

# Escribir los cambios de nuevo al archivo
with open("datos.json", "w") as archivo:
    json.dump(datos, archivo, indent=4)
```

---

### **6. Manejo de errores**

Cuando trabajas con JSON, es importante manejar **posibles excepciones** para evitar errores inesperados.

Ejemplo:
```python
import json

try:
    with open("datos.json", "r") as archivo:
        datos = json.load(archivo)
except FileNotFoundError:
    print("*ERROR* Archivo no encontrado.")
except json.JSONDecodeError:
    print("*ERROR* Problemas al decodificar el archivo JSON.")
except Exception as e:
    print(f"*ERROR* {e}.")
```

---

### **7. Opciones avanzadas**

#### **7.1 Serializar objetos personalizados**

El módulo `json` puede serializar objetos personalizados usando el argumento `default`.

Ejemplo:
```python
import json
from datetime import datetime

# Crear un objeto con un tipo no JSON serializable
datos = {
    "evento": "Reunión",
    "fecha": datetime.now()
}

# Serializador personalizado
def convertir(obj):
    if isinstance(obj, datetime):
        return obj.isoformat()  # Convertir a formato ISO 8601
    raise TypeError("Tipo no serializable")

# Serializar el objeto
cadena_json = json.dumps(datos, default=convertir, indent=4)
print(cadena_json)
```

Salida:
```json
{
    "evento": "Reunión",
    "fecha": "2024-11-22T12:34:56.789123"
}
```

---

#### **7.2 Ordenar claves**

Para ordenar las claves del JSON, usa el argumento `sort_keys`.

Ejemplo:
```python
import json

datos = {"z": 1, "a": 2, "m": 3}
cadena_json = json.dumps(datos, sort_keys=True, indent=2)
print(cadena_json)
```

Salida:
```json
{
  "a": 2,
  "m": 3,
  "z": 1
}
```

---

### **8. Conclusión**

El manejo de archivos JSON en Python es sencillo y flexible gracias al módulo `json`. 
Este permite trabajar con datos estructurados de forma legible y eficiente. 
Con las funciones `load`, `loads`, `dump` y `dumps`, puedes realizar operaciones comunes como leer, escribir y manipular datos JSON en tus programas. 
Además, con personalizaciones avanzadas, puedes adaptar el manejo de JSON a necesidades específicas.

---

Claro, aquí tienes un ejemplo completo y básico con todas las etapas que mencionas:

---

### **9. Ejemplo Completo**

#### 9.1. Fichero JSON inicial (`datos.json`).

Este es el contenido inicial del archivo JSON que usaremos:

```json
{
    "usuarios": [
        {"id": 1, "nombre": "Juan", "edad": 30},
        {"id": 2, "nombre": "Ana", "edad": 25}
    ]
}
```

#### 9.2. Operaciones que el programa de ejemplo va a realizar.

El código realiza las siguientes operaciones:

1. Carga los datos desde el archivo JSON con manejo de excepciones.
2. Actualiza la edad de un usuario.
3. Inserta un nuevo usuario.
4. Elimina un usuario por su `id`.
5. Guarda los cambios en el archivo.

#### 9.3. Código en Python.

```python
import json


def cargar_json(nombre_fichero: str) -> dict:
    """
    Carga el contenido de un fichero JSON.

    Args:
        nombre_fichero (str): Nombre del fichero JSON.

    Returns:
        (dict): Contenido del archivo JSON como un diccionario, o None si no se pudo cargar.
    """
    try:
        with open(nombre_fichero, "r") as archivo:
            return json.load(archivo)
        
    except FileNotFoundError:
        print(f"*ERROR* El archivo {nombre_fichero} no existe.")
    
    except json.JSONDecodeError:
        print("*ERROR* El archivo JSON tiene un formato incorrecto.")

    except Exception as e:
        print(f"*ERROR* Problemas al cargar los datos {e}.")

    return None


def guardar_json(nombre_fichero: str, datos: dict):
    """
    Guarda los datos en un fichero JSON.

    Args:
        nombre_fichero (str): Nombre del fichero JSON.
        datos (dict): Datos a guardar.
    """
    try:
        with open(nombre_fichero, "w") as archivo:
            json.dump(datos, archivo, indent = 4)

    except PermissionError:
        print(f"*ERROR* No tienes permisos para escribir en el archivo '{nombre_fichero}'.")

    except TypeError as e:
        print(f"*ERROR* Los datos no son serializables a JSON. Detalle: {e}")        

    except Exception as e:
        print(f"*ERROR* Problemas al guardar los datos: {e}")


def actualizar_usuario(datos: dict, id_usuario: int, nueva_edad: int):
    """
    Actualiza la edad de un usuario dado su ID.

    Args:
        datos (dict): Diccionario con los datos actuales.
        id_usuario (int): ID del usuario a actualizar.
        nueva_edad (int): Nueva edad del usuario.
    """
    for usuario in datos["usuarios"]:
        if usuario["id"] == id_usuario:
            usuario["edad"] = nueva_edad
            print(f"Usuario con ID {id_usuario} actualizado.")
            return
    
    print(f"Usuario con ID {id_usuario} no encontrado.")


def insertar_usuario(datos: dict, nuevo_usuario: dict):
    """
    Inserta un nuevo usuario.

    Args:
        datos (dict): Diccionario con los datos actuales.
        nuevo_usuario (dict): Diccionario con los datos del nuevo usuario.
    """
    datos["usuarios"].append(nuevo_usuario)
    print(f"Usuario {nuevo_usuario['nombre']} añadido con éxito.")


def eliminar_usuario(datos: dict, id_usuario: int):
    """
    Elimina un usuario dado su ID.

    Args:
        datos (dict): Diccionario con los datos actuales.
        id_usuario (int): ID del usuario a eliminar.
    """
    for usuario in datos["usuarios"]:
        if usuario["id"] == id_usuario:
            datos["usuarios"].remove(usuario)
            print(f"Usuario con ID {id_usuario} eliminado.")
            return
    
    print(f"Usuario con ID {id_usuario} no encontrado.")


def main():
    """
    Función principal que realiza las operaciones de gestión de un archivo JSON.
    """
    # Nombre del fichero JSON
    nombre_fichero = "datos.json"

    # 1. Cargar datos desde el fichero JSON
    datos = cargar_json(nombre_fichero)
    if datos is None:
        # Inicializamos datos vacíos si hay error
        datos = {"usuarios": []}

    # 2. Actualizar la edad de un usuario
    actualizar_usuario(datos, id_usuario = 1, nueva_edad = 31)

    # 3. Insertar un nuevo usuario
    nuevo_usuario = {"id": 3, "nombre": "Pedro", "edad": 40}
    insertar_usuario(datos, nuevo_usuario)

    # 4. Eliminar un usuario
    eliminar_usuario(datos, id_usuario = 2)

    # 5. Guardar los datos de nuevo en el fichero JSON
    guardar_json(nombre_fichero, datos)

    print("Operaciones completadas. Archivo actualizado.\n")


if __name__ == "__main__":
    main()
```

---

#### 9.4. Explicación paso a paso

1. **Función `cargar_json`:**
   - Lee el contenido del archivo JSON.
   - Maneja excepciones como `FileNotFoundError` y `JSONDecodeError` para evitar fallos en caso de problemas con el archivo.

2. **Función `guardar_json`:**
   - Guarda el objeto Python en el archivo JSON.
   - Usa `json.dump` con `indent = 4` para que sea legible.

3. **Función `actualizar_usuario`:**
   - Busca un usuario por su `id` y actualiza su edad si existe.
   - Muestra un mensaje si no encuentra al usuario.

4. **Función `insertar_usuario`:**
   - Agrega un nuevo usuario al listado.

5. **Función `eliminar_usuario`:**
   - Elimina un usuario por su `id` si existe.
   - Muestra un mensaje si no lo encuentra.

6. **Ejecución principal:**
   - Carga el JSON inicial.
   - Realiza las operaciones necesarias (actualizar, insertar, eliminar).
   - Guarda los datos actualizados en el archivo.

---

### 9.5. Resultado final (`datos.json`).

Después de ejecutar el programa, el archivo `datos.json` tendrá el siguiente contenido:

```json
{
    "usuarios": [
        {
            "id": 1,
            "nombre": "Juan",
            "edad": 31
        },
        {
            "id": 3,
            "nombre": "Pedro",
            "edad": 40
        }
    ]
}
``` 

---

## ACTIVIDADES

El enunciado de las actividades que tenéis que realizar están en el fichero [ACTIVIDADES.md](ACTIVIDADES.md)
