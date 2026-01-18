### 3. Infinite Integer Set

## 1. Enunciado del Ejercicio

**Ejercicio 1 (6 puntos)**

Implementar la clase `InfiniteIntegerSet` dentro del paquete `exam`, que represente un conjunto ilimitado de números enteros (`Integer`). La clase debe usar como estructura de datos auxiliar un **array básico de Java** (`Integer[]`). No se permite utilizar estructuras de datos avanzadas como `ArrayList` ni otras colecciones.

El conjunto debe ser capaz de almacenar cualquier número de elementos. Para ello, el array debe redimensionarse dinámicamente **duplicando su tamaño** cuando se alcance su capacidad máxima. Además, la clase debe garantizar que no se permita la inclusión de elementos duplicados ni elementos `null`.

La clase debe incluir los siguientes métodos:

* `public InfiniteIntegerSet()`: Constructor que crea un nuevo conjunto vacío.
* `public boolean insert(Integer element)`: Añade el elemento. Devuelve `true` si se añade con éxito y `false` si ya existía. No admite `null`.
* `public boolean delete(Integer element)`: Elimina el elemento. Devuelve `true` si se borra y `false` si no estaba.
* `public boolean search(Integer element)`: Busca el elemento y devuelve `true` si se encuentra.
* `public int size()`: Devuelve el número de elementos actual.
* `public void empty()`: Vacía el conjunto.
* `public String toString()`: Devuelve el conjunto en formato `[e1, e2, ...]`.

---

## 2. Estructura de Clases

Para cumplir con el enunciado y validar el funcionamiento, la estructura de archivos es la siguiente:

* **Paquete `exam**`:
* `InfiniteIntegerSet`: Clase principal con la lógica del conjunto y redimensionamiento dinámico.
* `Main`: Clase de prueba para verificar todos los métodos y el crecimiento del array.



---

## 3. Implementación en Java

### InfiniteIntegerSet.java

```java
package exam;

import java.util.Arrays;

/**
 * Implementación de un conjunto de enteros con crecimiento dinámico.
 */
public class InfiniteIntegerSet {
    private Integer[] elements;
    private int count;
    private static final int INITIAL_CAPACITY = 10;

    /**
     * Constructor que crea un conjunto vacío con capacidad inicial.
     */
    public InfiniteIntegerSet() {
        this.empty();
    }

    /**
     * Añade un elemento si no es null y no existe previamente.
     * Si el array está lleno, duplica su tamaño.
     */
    public boolean insert(Integer element) {
        if (element == null || search(element)) {
            return false;
        }

        if (count == elements.length) {
            resize();
        }

        elements[count] = element;
        count++;
        return true;
    }

    /**
     * Elimina un elemento del conjunto.
     */
    public boolean delete(Integer element) {
        if (element == null) return false;

        for (int i = 0; i < count; i++) {
            if (elements[i].equals(element)) {
                // Desplazamos los elementos restantes para no dejar huecos
                for (int j = i; j < count - 1; j++) {
                    elements[j] = elements[j + 1];
                }
                elements[count - 1] = null;
                count--;
                return true;
            }
        }
        return false;
    }

    /**
     * Busca si un elemento existe en el conjunto.
     */
    public boolean search(Integer element) {
        if (element == null) return false;
        
        for (int i = 0; i < count; i++) {
            if (elements[i].equals(element)) {
                return true;
            }
        }
        return false;
    }

    /**
     * Devuelve la cantidad de elementos actuales.
     */
    public int size() {
        return count;
    }

    /**
     * Reinicia el conjunto a su estado inicial.
     */
    public void empty() {
        this.elements = new Integer[INITIAL_CAPACITY];
        this.count = 0;
    }

    /**
     * Representación textual: [3, 5, 8]
     */
    @Override
    public String toString() {
        if (count == 0) return "[]";
        
        StringBuilder sb = new StringBuilder("[");
        for (int i = 0; i < count; i++) {
            sb.append(elements[i]);
            if (i < count - 1) {
                sb.append(", ");
            }
        }
        sb.append("]");
        return sb.toString();
    }

    /**
     * Método privado para duplicar el tamaño del array.
     */
    private void resize() {
        int newSize = elements.length * 2;
        Integer[] newArray = new Integer[newSize];
        System.arraycopy(elements, 0, newArray, 0, elements.length);
        elements = newArray;
    }
}

```

### Main.java (Clase de validación)

```java
package exam;

public class Main {
    public static void main(String[] args) {
        InfiniteIntegerSet set = new InfiniteIntegerSet();

        System.out.println("--- Probando Inserción y Duplicados ---");
        System.out.println("Insertar 5: " + set.insert(5)); // true
        System.out.println("Insertar 10: " + set.insert(10)); // true
        System.out.println("Insertar 5 de nuevo: " + set.insert(5)); // false (duplicado)
        System.out.println("Insertar null: " + set.insert(null)); // false
        
        System.out.println("\n--- Probando Redimensionamiento (Llenando el set) ---");
        for (int i = 0; i < 15; i++) {
            set.insert(i + 100); // Esto forzará la duplicación del array inicial (10)
        }
        System.out.println("Tamaño actual: " + set.size());
        System.out.println("Contenido: " + set.toString());

        System.out.println("\n--- Probando Búsqueda ---");
        System.out.println("¿Está el 105?: " + set.search(105)); // true
        System.out.println("¿Está el 999?: " + set.search(999)); // false

        System.out.println("\n--- Probando Eliminación ---");
        System.out.println("Eliminar 10: " + set.delete(10)); // true
        System.out.println("Eliminar 10 de nuevo: " + set.delete(10)); // false
        System.out.println("Contenido tras borrar 10: " + set.toString());

        System.out.println("\n--- Probando Vaciar ---");
        set.empty();
        System.out.println("Tamaño tras empty(): " + set.size());
        System.out.println("Contenido: " + set.toString());
    }
}

```

---

## 4. Explicación del Proceso de Implementación

Para construir esta solución, me he centrado en la gestión manual de memoria (arrays) típica de las estructuras de datos fundamentales:

1. **Gestión del Atributo `count**`: Al usar un array básico, el tamaño del array (`elements.length`) no es igual al número de elementos que contiene. He introducido la variable `count` para rastrear cuántos números hay realmente y usarla como índice para la próxima inserción.
2. **Lógica de Conjunto (Set)**: Antes de cada inserción en el método `insert`, invoco a `search`. Si el elemento ya existe, el método retorna `false` inmediatamente, cumpliendo la regla de "no duplicados".
3. **Redimensionamiento Dinámico**: He implementado un método privado `resize()`. Cuando `count` alcanza el límite del array, creo un nuevo array con `elements.length * 2` y transfiero los datos. Esto permite que el conjunto sea "infinito" (limitado solo por la memoria RAM).
4. **Borrado con Compactación**: Al eliminar un elemento en `delete`, no basta con poner esa posición a `null`, ya que dejaría un "hueco". He implementado un bucle que desplaza todos los elementos a la izquierda una posición para mantener la integridad del array.
5. **Formateo de Salida**: Para el método `toString`, he utilizado `StringBuilder`. Es más eficiente que concatenar strings con `+` dentro de un bucle, especialmente cuando el conjunto crece mucho.

