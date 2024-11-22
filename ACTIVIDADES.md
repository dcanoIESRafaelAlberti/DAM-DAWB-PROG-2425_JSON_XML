## ACTIVIDADES

### **1. Recorrer una lista con un iterador**

Crea una función llamada `recorrer_con_iterador` que cumpla con las siguientes condiciones:

   1. Reciba como argumento una lista de elementos.
   2. Use un **iterador** para recorrer los elementos de la lista.
   3. Concatene los elementos en una cadena de caracteres con el formato:
      ```
      "elemento1 - elemento2 - elemento3 - ... - elementoN."
      ```
   4. Devuelva la cadena resultante.

#### **Restricciones:**

- No se permite usar un bucle `for`. Debes utilizar un **iterador** con `iter()` y `next()`.

#### **Ejemplo de uso:**

```python
lista = ["Ana", "Juan", "Pedro", "Lucía"]
resultado = recorrer_con_iterador(lista)
print(resultado)
# Salida esperada:
# "Ana - Juan - Pedro - Lucía."
```

### **2. Generar una matriz regular con las dos diagonales marcadas**

Crea una función llamada `crear_matriz_diagonales` que reciba como argumento un número entero positivo `dimension`, que representa las dimensiones de una matriz cuadrada `n x n`. La función debe generar y devolver la matriz con las siguientes condiciones:

   1. Todas las celdas deben estar inicialmente rellenas con `0`.
   2. Las celdas de las **dos diagonales** (principal y secundaria) deben estar rellenas con `1`.

Se deben crear las siguientes funciones como mínimo:

   1. `crear_matriz_diagonales`: Retorna una matriz cuadrada con todas las celdas a 0, excepto las diagonales a 1.
   2. `generar_matriz_regular`: Retorna una matriz cuadrada con todos los valores por defecto a 0.
   3. `imprimir_matriz`: Imprime la matriz con los elementos separados por ",".

#### **Restricciones:**

- Solo se permite usar el bucle `for` para cualquier función del programa.
- Debes usar `join` para imprimir la matriz.

#### **Ejemplo de uso:**

```python
matriz = crear_matriz_diagonales(5)

# La matriz generada será:
# (
#   [1, 0, 0, 0, 1],
#   [0, 1, 0, 1, 0],
#   [0, 0, 1, 0, 0],
#   [0, 1, 0, 1, 0],
#   [1, 0, 0, 0, 1]
# )

imprimir_matriz(matriz)
```

#### **Salida esperada sería la siguiente:**

```
1, 0, 0, 0, 1
0, 1, 0, 1, 0
0, 0, 1, 0, 0
0, 1, 0, 1, 0
1, 0, 0, 0, 1
```

#### **Extra:**

Si tienes tiempo haz que el programa limpie la pantalla, pida al usuario el tamaño de la matriz *(no será posible generar una matriz inferior a 1, ni superior a 25)*, la genere e imprima por pantalla, haga una pausa hasta que pulse una tecla y permanezca en bucle hasta que introduzcas una cadena vacía.

#### **Salida esperada ahora:**

```
Dime el tamaño de la matriz >> 3
1, 0, 1
0, 1, 0
1, 0, 1
Presione una tecla para continuar...

<<Limpia la consola>>

Dime el tamaño de la matriz >> 4
1, 0, 0, 1
0, 1, 1, 0
0, 1, 1, 0
1, 0, 0, 1
Presione una tecla para continuar...

<<Limpia la consola>>

Dime el tamaño de la matriz >> <<Pulsamos ENTER directamente>>

<<Acaba el programa>>
```
