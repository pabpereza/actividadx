### Infinite Integer Stack

**Ejercicio 1 (6 puntos)**

**MUY IMPORTANTE:** Es imprescindible, para que el ejercicio se califique, respetar estrictamente:

* Nombres de las clases y paquetes mencionados.
* Signatura de cada método mencionado: nombre, tipo de valor devuelto, número, tipo y orden de los parámetros si los tuviese.

**Descripción:**
Una pila (stack) es una estructura de datos de tipo LIFO (Last In, First Out), es decir, el último elemento que se introduce en la pila es el primero en salir.

Implementar la clase `InfiniteIntegerStack` dentro del paquete `exam`. Esta clase debe representar una pila ilimitada de números enteros (`Integer`) que no admita valores `null`. La clase debe usar como estructura de datos auxiliar un array básico de Java (`Integer[]`). No se permite utilizar estructuras de datos avanzadas como `ArrayList` ni otras colecciones.

La pila debe ser capaz de almacenar cualquier número de elementos. Para ello, el array debe redimensionarse dinámicamente **duplicando su tamaño** cuando se alcance su capacidad máxima.

**La clase debe incluir los siguientes métodos:**

* `public InfiniteIntegerStack()`: Constructor que crea una nueva pila vacía.
* `public boolean push(Integer element)`: Añade a la pila (en la cima). Si es `null`, devuelve `false`. Si tiene éxito, devuelve `true`.
* `public Integer pop()`: Devuelve y elimina el elemento de la cima. Si está vacía, devuelve `null`.
* `public Integer top()`: Devuelve sin eliminar el elemento de la cima. Si está vacía, devuelve `null`.
* `public int size()`: Devuelve el número de elementos.
* `public void empty()`: Vacía la pila (similar al constructor).
* `public String toString()`: Formato string `[cima, elemento_2, ..., fondo]`. Ejemplo `[1,2,3]` siendo 1 la cima. Si vacía: `[]`.

---

### 2. Estructura de Clases

Para cumplir con el ejercicio, trabajaremos dentro del paquete `exam`. Adicionalmente, he creado una clase `Main` para probar que todo funcione correctamente.

**Paquete:** `exam`

1. **Clase `InfiniteIntegerStack**`: La implementación principal de la pila dinámica.
2. **Clase `Main**`: Clase de prueba (no pedida explícitamente, pero necesaria para ejecutar y validar el código).

---

### 3. Implementación de las clases en Java

Aquí tienes el código completo. Puedes copiar cada bloque en un archivo `.java` separado dentro de una carpeta llamada `exam`.

#### Archivo: `InfiniteIntegerStack.java`

```java
package exam;

/**
 * Implementación de una pila (Stack) dinámica de enteros utilizando un array básico.
 * Sigue la política LIFO (Last In, First Out).
 */
public class InfiniteIntegerStack {

    // Array básico para almacenar los elementos.
    private Integer[] data;
    
    // Contador para rastrear el número de elementos actuales en la pila.
    // También sirve como índice para la próxima posición libre.
    private int size;

    /**
     * Constructor que crea una nueva pila vacía.
     * Inicializamos el array con una capacidad pequeña inicial.
     */
    public InfiniteIntegerStack() {
        this.data = new Integer[5]; // Capacidad inicial arbitraria (ej. 5)
        this.size = 0;
    }

    /**
     * Añade a la pila (en la cima) el elemento pasado por parámetro.
     * * @param element El entero a añadir.
     * @return false si el parámetro es null, true si la operación es exitosa.
     */
    public boolean push(Integer element) {
        // Validación estricta de null
        if (element == null) {
            return false;
        }

        // Comprobar si el array está lleno
        if (this.size == this.data.length) {
            resize();
        }

        // Añadir elemento en la posición actual de 'size' y luego incrementar size
        this.data[this.size] = element;
        this.size++;
        
        return true;
    }

    /**
     * Método de apoyo privado para redimensionar el array.
     * Duplica el tamaño actual del array.
     */
    private void resize() {
        // 1. Calcular nueva capacidad (duplicar)
        int newCapacity = this.data.length * 2;
        
        // 2. Crear el nuevo array
        Integer[] newData = new Integer[newCapacity];
        
        // 3. Copiar los elementos del array viejo al nuevo
        for (int i = 0; i < this.data.length; i++) {
            newData[i] = this.data[i];
        }
        
        // 4. Reemplazar la referencia del array
        this.data = newData;
    }

    /**
     * Devuelve y elimina de la pila el elemento situado en la cima.
     * * @return El elemento en la cima, o null si la pila está vacía.
     */
    public Integer pop() {
        if (this.size == 0) {
            return null;
        }

        // Decrementamos size para apuntar al elemento que era la cima
        this.size--;
        
        // Recuperamos el elemento
        Integer element = this.data[this.size];
        
        // Limpiamos la referencia en el array para evitar memory leaks (buena práctica)
        this.data[this.size] = null;
        
        return element;
    }

    /**
     * Devuelve sin eliminar de la pila el elemento situado en la cima.
     * * @return El elemento en la cima, o null si la pila está vacía.
     */
    public Integer top() {
        if (this.size == 0) {
            return null;
        }
        // La cima está en la posición size - 1
        return this.data[this.size - 1];
    }

    /**
     * Devuelve el número de elementos de la pila.
     * * @return Cantidad de elementos.
     */
    public int size() {
        return this.size;
    }

    /**
     * Vacía la pila.
     * Trabaja de manera similar al constructor, reiniciando el array y el contador.
     */
    public void empty() {
        this.data = new Integer[5]; // Reiniciamos a capacidad inicial
        this.size = 0;
    }

    /**
     * Devuelve la representación de la pila en formato string.
     * Formato: [cima, elemento_2, ..., fondo]
     * * @return String representativo.
     */
    public String toString() {
        if (this.size == 0) {
            return "[]";
        }

        StringBuilder sb = new StringBuilder();
        sb.append("[");

        // IMPORTANTE: El enunciado pide que la cima esté en primera posición.
        // Como llenamos el array de 0 hacia adelante, la cima está al final (índice size - 1).
        // Debemos recorrer el array inversamente desde size-1 hasta 0.
        for (int i = this.size - 1; i >= 0; i--) {
            sb.append(this.data[i]);
            
            // Añadir coma si no es el último elemento a imprimir (el fondo de la pila)
            if (i > 0) {
                sb.append(",");
            }
        }

        sb.append("]");
        return sb.toString();
    }
}

```

#### Archivo: `Main.java` (Para validación)

```java
package exam;

public class Main {
    public static void main(String[] args) {
        System.out.println("--- Iniciando pruebas de InfiniteIntegerStack ---");

        InfiniteIntegerStack stack = new InfiniteIntegerStack();

        // 1. Prueba de pila vacía
        System.out.println("Pila inicial (debe ser []): " + stack.toString());
        System.out.println("Tamaño inicial (debe ser 0): " + stack.size());

        // 2. Prueba de push y validación null
        System.out.println("\n--- Insertando elementos ---");
        stack.push(10); // Fondo
        stack.push(20);
        stack.push(30); // Cima actual
        System.out.println("Insertar null (debe ser false): " + stack.push(null));
        
        // 3. Prueba toString (Debe salir [30,20,10])
        System.out.println("Estado actual (esperado [30,20,10]): " + stack.toString());

        // 4. Prueba Redimensionamiento Dinámico
        System.out.println("\n--- Probando redimensionamiento ---");
        // El array interno iniciaba con 5 (según mi código). Insertamos más para forzar duplicación.
        stack.push(40);
        stack.push(50);
        stack.push(60); // Aquí debería haber ocurrido un resize interno
        System.out.println("Tras insertar 6 elementos (esperado [60,50,40,30,20,10]): " + stack.toString());
        System.out.println("Tamaño actual: " + stack.size());

        // 5. Prueba Top
        System.out.println("\n--- Probando Top ---");
        System.out.println("Elemento en la cima (esperado 60): " + stack.top());
        System.out.println("Tamaño tras top (no debe cambiar): " + stack.size());

        // 6. Prueba Pop
        System.out.println("\n--- Probando Pop ---");
        System.out.println("Sacando cima (esperado 60): " + stack.pop());
        System.out.println("Nueva cima (esperado 50): " + stack.top());
        System.out.println("Estado actual: " + stack.toString());

        // 7. Prueba Empty
        System.out.println("\n--- Probando Empty ---");
        stack.empty();
        System.out.println("Estado tras empty (esperado []): " + stack.toString());
        System.out.println("Pop en vacio (esperado null): " + stack.pop());
    }
}

```

---

### 4. Explicación paso a paso de la implementación

Aquí te explico cómo he abordado los puntos clave del ejercicio para asegurar la máxima calificación:

#### A. Gestión del Array y `size`

El corazón de este ejercicio es no usar `ArrayList`. He definido `private Integer[] data` y `private int size`.

* **`data`**: Es el almacén real. Comienza pequeño.
* **`size`**: Cumple una doble función vital. Indica cuántos elementos hay "de verdad" (ignora los huecos vacíos del array) y, a la vez, actúa como **puntero** al siguiente índice libre donde insertar.

#### B. El método `resize()` (La parte "infinita")

El ejercicio pide redimensionar "duplicando su tamaño". Esto lo hago en el método privado `resize()` que es llamado por `push` solo cuando el array está lleno.

1. Creo un `newData` con el tamaño `data.length * 2`.
2. Copio elemento a elemento con un bucle `for`.
3. Asigno `this.data = newData`. Así, la pila crece transparentemente para el usuario.

#### C. Lógica LIFO en `push` y `pop`

* **Push:** Inserta en el final del array (`data[size]`) y luego aumenta el contador.
* **Pop:** Disminuye el contador (`size--`) y luego devuelve ese elemento.
* *Nota Pro:* He añadido `this.data[this.size] = null` en el `pop`. Esto no es obligatorio para que funcione la lógica de pila, pero en Java es importante para que el **Garbage Collector** pueda liberar la memoria del objeto eliminado.



#### D. El formato del `toString()`

El enunciado tiene una "trampa" visual: pide mostrar `[cima, ..., fondo]`.
Dado que mi implementación añade elementos al final del array (índice 0 es fondo, índice `size-1` es cima):

* Si imprimo el array normal `[0, 1, 2]`, estaría imprimiendo `[fondo, ..., cima]`.
* Por tanto, el bucle `for` en `toString` empieza en `size - 1` y va hacia atrás hasta `0`.

#### E. Restricciones

* Se ha respetado el paquete `exam`.
* Se han respetado todas las firmas de métodos (`public Integer pop()`, etc).
* Se ha incluido la validación de `null` en el `push`.