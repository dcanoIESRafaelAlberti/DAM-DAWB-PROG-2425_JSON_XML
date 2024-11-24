## JSON

Los archivos JSON (JavaScript Object Notation) son un formato de datos ligero utilizado para intercambiar información de manera sencilla. 
En Python, se gestionan principalmente con el módulo integrado `json`, que permite leer, escribir y manipular datos JSON de forma sencilla.

---

### **Conceptos básicos de JSON**

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

### **JSON en Python**

(Resumen del Manejo de Archivos JSON en Python)[JSON_Python.md]

---

## XML

XML (eXtensible Markup Language) es un formato estándar utilizado para estructurar y almacenar datos. 
En Python, se gestiona principalmente con los módulos integrados `xml.etree.ElementTree` (más simple) o `xml.dom.minidom` (para representación legible). 
También puedes usar librerías externas como `lxml` para funcionalidades avanzadas.

---

### **Conceptos básicos de XML**

Un archivo XML contiene elementos (tags), atributos y valores anidados. Es más estructurado que JSON pero también más verboso, es decir, requiere más texto para representar la misma cantidad de información.

Ejemplo de XML:
```xml
<usuario>
    <nombre>Juan</nombre>
    <edad>30</edad>
    <habilidades>
        <habilidad>Python</habilidad>
        <habilidad>JavaScript</habilidad>
    </habilidades>
    <activo>true</activo>
</usuario>
```

---

### **XML en Python**

(Resumen del Manejo de Archivos XML en Python)[XML_Python.md]

---

## ACTIVIDADES

- ACTIVIDAD 1 => [Completar la Gestión de Usuarios usando JSON](ACTIVIDAD_JSON.md)
- ACTIVIDAD 2 => [Completar la Gestión de Usuarios usando XML](ACTIVIDAD_XML.md)
