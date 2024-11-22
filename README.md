## **Iterables e Iteradores en Python**

### **1. Iterable**

Un **iterable** es cualquier objeto que puede ser "recorrido" o iterado con un bucle como `for`. Ejemplos comunes son listas, tuplas, diccionarios, conjuntos y cadenas.

Para que un objeto sea iterable:

- Debe implementar el método especial `__iter__()` que devuelve un **iterador**.

### **2. Iterador**

Un **iterador** es un objeto que:

- Implementa el método `__next__()` para obtener el siguiente elemento de una secuencia.

- Recuerda en qué posición se encuentra en la secuencia.

Los iteradores se obtienen llamando a `iter(iterable)`. Podemos usar `next(iterator)` para avanzar por los elementos.

#### **Ejemplo en Python**

```python
# Un iterable: una lista
numeros = [1, 2, 3]

# Obtenemos un iterador con iter()
iterador = iter(numeros)

# Usamos next() para avanzar
print(next(iterador))  # 1
print(next(iterador))  # 2
print(next(iterador))  # 3

# Cuando no hay más elementos, se lanza StopIteration
# print(next(iterador))  # Error: StopIteration
```

### **Bucle for**

El bucle `for` automáticamente llama a `iter()` y `next()` internamente por nosotros *(es una abstracción que encapsula este proceso)*:

```python
for numero in numeros:
    print(numero)
```

---

## **Iterables e Iteradores en otros lenguajes**

### **1. Kotlin**

En Kotlin, puedes iterar sobre colecciones como listas, arreglos o mapas utilizando bucles como `for`. Internamente, usa **iteradores** que implementan la interfaz `Iterator`.

```kotlin
val numeros = listOf(1, 2, 3)

// Bucle for
for (numero in numeros) {
    println(numero)
}

// Manualmente con un iterador
val iterador = numeros.iterator()
while (iterador.hasNext()) {
    println(iterador.next())
}
```

### **2. Java**

Java tiene una interfaz llamada `Iterable`, y las colecciones como `ArrayList` son iterables. El método `iterator()` devuelve un objeto `Iterator`.

```java
import java.util.ArrayList;

public class Main {
    public static void main(String[] args) {
        ArrayList<Integer> numeros = new ArrayList<>();
        numeros.add(1);
        numeros.add(2);
        numeros.add(3);

        // Usando un iterador
        var iterador = numeros.iterator();
        while (iterador.hasNext()) {
            System.out.println(iterador.next());
        }

        // Usando un bucle for-each (azúcar sintáctica)
        for (int numero : numeros) {
            System.out.println(numero);
        }
    }
}
```

### **3. C**

En C no hay iteradores como tal porque no tiene soporte directo para programación orientada a objetos ni iteradores integrados. Sin embargo, podemos simular el comportamiento de un iterador usando punteros.

```c
#include <stdio.h>

int main() {
    int numeros[] = {1, 2, 3};
    int *iterador = numeros; // Puntero al inicio del array

    // Usando punteros como iteradores
    for (int i = 0; i < 3; i++) {
        printf("%d\n", *iterador); // Imprime el valor apuntado
        iterador++;               // Avanza el puntero
    }

    return 0;
}
```

---

## **Resumen Comparativo**

| Lenguaje | Iterable                              | Iterador                     |
|----------|---------------------------------------|------------------------------|
| Python   | Colecciones con `__iter__()`          | Objeto con `__next__()`      |
| Kotlin   | Colecciones (`listOf`, `arrayOf`)     | `iterator()` y `hasNext()`   |
| Java     | Colecciones que implementan `Iterable`| `iterator()` y `next()`      |
| C        | Arrays y punteros (manual)            | Punteros y control manual    |

Cada lenguaje tiene su forma de manejar iterables, pero el concepto básico es similar: un **iterable** es un objeto que se puede recorrer y un **iterador** es un objeto que hace el recorrido paso a paso.

---

## **¿Para qué nos puede servir el uso de iteradores?**

Aunque el `for` es adecuado para la mayoría de los casos, los iteradores son esenciales cuando necesitamos:

- Procesar datos parciales o detenernos en cualquier punto.
- Trabajar con datos infinitos o grandes conjuntos que no caben en memoria.
- Controlar exactamente cómo y cuándo se consumen los elementos.
- Personalizar la lógica de iteración, como al crear nuestros propios iterables o generadores.

Los iteradores son una muy buena herramienta que nos proporciona control y eficiencia en casos avanzados. Para tareas cotidianas, el `for` sigue siendo la opción más sencilla y legible.

---

## ACTIVIDADES

El enunciado de las actividades que tenéis que realizar están en el fichero [ACTIVIDADES.md](ACTIVIDADES.md)
