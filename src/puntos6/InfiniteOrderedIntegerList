### InfiniteOrderedIntegerList

**Ejercicio 1 (6 puntos)**

**MUY IMPORTANTE:** Es imprescindible respetar estrictamente nombres de clases, paquetes y signaturas de métodos.

Implementar la clase `InfiniteOrderedIntegerList` dentro del paquete `exam`. Representa una lista ordenada e ilimitada de números enteros (`Integer`) donde:

* No se admiten elementos `null`.
* Se permiten duplicados.
* Están ordenados por orden creciente.
* Estructura auxiliar: **array básico** de Java (`Integer[]`). No se permiten `ArrayList` ni colecciones.
* El array debe redimensionarse dinámicamente **duplicando su tamaño** cuando se alcance la capacidad máxima.

**Métodos a implementar:**

1. `public InfiniteOrderedIntegerList()`: Constructor (lista vacía).
2. `public boolean add(Integer element)`: Añade manteniendo el orden. Devuelve `true` si éxito, `false` si es `null`.
3. `public boolean delete(Integer element)`: Elimina **todas** las apariciones del elemento. Devuelve `true` si se eliminó algo, `false` si no estaba.
4. `public boolean search(Integer element)`: Devuelve `true` si existe.
5. `public int size()`: Número de elementos actuales.
6. `public void empty()`: Vacía la lista.
7. `public String toString()`: Formato `[elem1,elem2,elem3]`.

---

### 2. Estructura de Clases

Para cumplir con el ejercicio y validar el código, esta será la estructura de archivos dentro de tu proyecto Java:

```text
src/
└── exam/
    ├── InfiniteOrderedIntegerList.java  (La clase solicitada)
    └── Main.java                        (Clase para probar el funcionamiento)

```

---

### 3. Implementación de las Clases en Java

#### A. Clase `InfiniteOrderedIntegerList.java`

```java
package exam;

/**
 * Clase que representa una lista ordenada ilimitada de enteros.
 * Utiliza internamente un array básico y se redimensiona automáticamente.
 */
public class InfiniteOrderedIntegerList {

    // Array básico para almacenar los elementos.
    private Integer[] elements;
    
    // Contador para saber cuántos elementos reales hay en la lista.
    private int count;
    
    // Capacidad inicial del array (por defecto 10, aunque puede ser cualquier número positivo).
    private static final int INITIAL_CAPACITY = 10;

    /**
     * Constructor que crea una nueva lista vacía.
     */
    public InfiniteOrderedIntegerList() {
        this.elements = new Integer[INITIAL_CAPACITY];
        this.count = 0;
    }

    /**
     * Añade el elemento pasado por parámetro en la posición adecuada para mantener la lista ordenada.
     * * @param element El entero a añadir.
     * @return true si se añade con éxito, false si el elemento es null.
     */
    public boolean add(Integer element) {
        if (element == null) {
            return false;
        }

        // Comprobamos si el array está lleno para redimensionar
        if (this.count == this.elements.length) {
            resize();
        }

        // Buscamos la posición de inserción para mantener el orden
        int indexToInsert = 0;
        // Mientras no lleguemos al final y el elemento actual sea menor que el nuevo
        while (indexToInsert < this.count && this.elements[indexToInsert] < element) {
            indexToInsert++;
        }

        // Desplazamos los elementos hacia la derecha desde la posición de inserción
        for (int i = this.count; i > indexToInsert; i--) {
            this.elements[i] = this.elements[i - 1];
        }

        // Insertamos el elemento
        this.elements[indexToInsert] = element;
        this.count++;
        
        return true;
    }

    /**
     * Elimina de la lista todas las apariciones del elemento pasado por parámetro.
     * * @param element El entero a borrar.
     * @return true si la operación se completa con éxito (estaba en la lista), false en caso contrario.
     */
    public boolean delete(Integer element) {
        if (element == null || this.count == 0) {
            return false;
        }

        boolean found = false;
        
        // Estrategia de compactación:
        // Creamos un índice de escritura. Recorremos el array y solo copiamos
        // los elementos que NO sean el que queremos borrar.
        int writeIndex = 0;
        int originalCount = this.count;

        for (int i = 0; i < originalCount; i++) {
            if (this.elements[i].equals(element)) {
                found = true; // Hemos encontrado al menos uno para borrar
                // No incrementamos writeIndex, efectivamente "saltando" este elemento
            } else {
                // Si no es el elemento a borrar, lo mantenemos (lo movemos a la posición correcta)
                this.elements[writeIndex] = this.elements[i];
                writeIndex++;
            }
        }

        // Limpiamos las referencias sobrantes al final del array (para evitar memory leaks)
        for (int i = writeIndex; i < originalCount; i++) {
            this.elements[i] = null;
        }

        // Actualizamos el tamaño real
        this.count = writeIndex;

        return found;
    }

    /**
     * Busca en la lista el elemento pasado por parámetro.
     * * @param element El entero a buscar.
     * @return true si se encuentra, false en caso contrario.
     */
    public boolean search(Integer element) {
        if (element == null) {
            return false;
        }
        
        // Búsqueda lineal optimizada (podemos parar si encontramos un número mayor porque está ordenada)
        for (int i = 0; i < this.count; i++) {
            if (this.elements[i].equals(element)) {
                return true;
            }
            if (this.elements[i] > element) {
                // Como está ordenada, si vemos uno mayor, el nuestro ya no puede estar después
                return false;
            }
        }
        return false;
    }

    /**
     * Devuelve el número de elementos de la lista.
     * * @return Número de enteros almacenados.
     */
    public int size() {
        return this.count;
    }

    /**
     * Vacía la lista. Trabaja de manera similar al constructor.
     */
    public void empty() {
        // Reiniciamos el array y el contador
        this.elements = new Integer[INITIAL_CAPACITY];
        this.count = 0;
    }

    /**
     * Devuelve la representación de la lista en formato string.
     * Formato: [elem1,elem2,elem3] sin espacios tras las comas.
     * * @return String formateado.
     */
    public String toString() {
        if (this.count == 0) {
            return "[]";
        }

        StringBuilder sb = new StringBuilder();
        sb.append("[");
        for (int i = 0; i < this.count; i++) {
            sb.append(this.elements[i]);
            if (i < this.count - 1) {
                sb.append(",");
            }
        }
        sb.append("]");
        return sb.toString();
    }

    // --- MÉTODOS DE APOYO (privados, tal como permite el enunciado) ---

    /**
     * Método auxiliar para duplicar el tamaño del array.
     */
    private void resize() {
        int newCapacity = this.elements.length * 2;
        Integer[] newArray = new Integer[newCapacity];
        
        // Copiamos manualmente los elementos al nuevo array
        for (int i = 0; i < this.count; i++) {
            newArray[i] = this.elements[i];
        }
        
        this.elements = newArray;
    }
}

```

#### B. Clase `Main.java` (Para validación)

Esta clase no se pide explícitamente en el diagrama, pero el enunciado dice "si se considera necesario...". Como profesor, siempre recomiendo crear un Main para verificar que tu lógica funciona antes de entregar.

```java
package exam;

public class Main {
    public static void main(String[] args) {
        System.out.println("--- Iniciando pruebas de InfiniteOrderedIntegerList ---");

        InfiniteOrderedIntegerList list = new InfiniteOrderedIntegerList();

        // 1. Prueba de toString vacía
        System.out.println("Lista vacía: " + list.toString()); // Esperado: []

        // 2. Prueba de add y ordenación
        System.out.println("\n--- Prueba de inserción ---");
        list.add(5);
        list.add(2);
        list.add(8);
        list.add(2); // Duplicado
        list.add(1);
        
        System.out.println("Lista tras inserciones: " + list.toString()); 
        // Esperado: [1,2,2,5,8] (ordenado y con duplicados)

        // 3. Prueba de redimensionamiento
        System.out.println("\n--- Prueba de redimensionamiento masivo ---");
        for(int i = 10; i < 20; i++) {
            list.add(i);
        }
        System.out.println("Tamaño actual: " + list.size());
        System.out.println("Lista: " + list.toString());

        // 4. Prueba de búsqueda
        System.out.println("\n--- Prueba de búsqueda ---");
        System.out.println("¿Existe el 5?: " + list.search(5)); // true
        System.out.println("¿Existe el 99?: " + list.search(99)); // false

        // 5. Prueba de borrado (todas las apariciones)
        System.out.println("\n--- Prueba de borrado ---");
        boolean deleted = list.delete(2);
        System.out.println("¿Borrado el 2?: " + deleted);
        System.out.println("Lista tras borrar todos los 2: " + list.toString()); 
        // Esperado: El 2 no debe aparecer, el resto sigue igual.

        boolean deleteMissing = list.delete(99);
        System.out.println("¿Borrado el 99?: " + deleteMissing); // false

        // 6. Prueba de empty
        System.out.println("\n--- Prueba de vaciado ---");
        list.empty();
        System.out.println("Lista tras empty(): " + list.toString());
        System.out.println("Tamaño: " + list.size());
    }
}

```

---

### 4. Explicación paso a paso de la implementación

Como docente, quiero explicarte las decisiones clave que hemos tomado para resolver este problema sin usar herramientas avanzadas como `ArrayList`.

**1. La estructura de datos (El Array y `count`)**
Java maneja los arrays básicos (`Integer[]`) con un tamaño fijo. El problema nos pide una lista "ilimitada".

* Creamos un array de un tamaño inicial (ej. 10).
* Usamos una variable `count` para saber cuántas posiciones de ese array estamos usando realmente. `elements.length` es la capacidad total, `count` es el tamaño lógico de la lista.

**2. El método `add` y la lógica de ordenación**
No añadimos el elemento al final y luego ordenamos (eso sería ineficiente). Lo insertamos directamente en su sitio.

* **Paso 1 (Resize):** Antes de añadir, preguntamos: `¿count == elements.length?`. Si es así, el array está lleno. Llamamos al método auxiliar `resize()` que crea un array del doble de tamaño y copia todo allí.
* **Paso 2 (Buscar sitio):** Recorremos el array hasta encontrar un número mayor que el que queremos insertar. Ese es nuestro hueco ("indexToInsert").
* **Paso 3 (Desplazamiento):** Para meter el número ahí sin borrar el que estaba, movemos todos los elementos desde el final hacia la derecha una posición. Esto abre un hueco.
* **Paso 4:** Asignamos el valor.

**3. El método `delete` (Eliminar duplicados)**
El requisito clave aquí es "eliminar **todas** las apariciones".

* En lugar de buscar, borrar, mover, buscar otra vez... usamos una **técnica de compactación**.
* Imagina que tienes una baraja de cartas en fila y quieres quitar los ases. Recorres la fila: si la carta NO es un as, la pones en un "montón nuevo" (o en la parte izquierda del mismo array). Si ES un as, simplemente la ignoras.
* Al final, rellenas lo que sobre a la derecha con `null` para que el recolector de basura de Java haga su trabajo y actualizas el `count`.

**4. El método `toString**`
Aquí usamos `StringBuilder`. Es mucho más eficiente que concatenar Strings con `+` dentro de un bucle, ya que concatenar crea muchos objetos intermedios en memoria. El formato se ajusta estrictamente a lo pedido: corchetes y comas sin espacios.