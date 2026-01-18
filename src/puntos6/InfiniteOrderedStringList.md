### Infinite Ordered String List

## 1. Enunciado del ejercicio

Implementar la clase `InfiniteOrderedStringList` dentro del paquete `extra1`, que represente una lista ordenada e ilimitada de palabras (`String`) en la que no se admiten elementos `null`. La clase debe usar como estructura de datos auxiliar una lista enlazada de nodos. Es tarea tuya construir una clase apropiada para representar este nodo, que también debe pertenecer al mismo paquete.

La lista debe almacenar cualquier número de elementos (letras minúsculas). Métodos requeridos:

* `public InfiniteOrderedStringList()`: Constructor de lista vacía.
* `public boolean add(String word)`: Añade manteniendo el orden. No `null`.
* `public boolean delete(String word)`: Elimina todas las apariciones de la palabra.
* `public boolean find(String word)`: Busca si la palabra existe.
* `public int wordsBeginningWithLetter(char letter)`: Conteo de palabras que empiezan por `letter`.
* `public int wordsEndingWithLetter(char letter)`: Conteo de palabras que terminan por `letter`.
* `public String toString()`: Formato "palabra1-palabra2" o "no words".

---

## 2. Estructura de clases

Para este proyecto, utilizaremos tres clases dentro del paquete `extra1`:

1. **`Node`**: Clase de apoyo que actúa como el "eslabón" de la cadena, guardando el dato y la referencia al siguiente.
2. **`InfiniteOrderedStringList`**: La clase principal que gestiona la lógica de la lista ordenada.
3. **`Main`**: Clase de prueba para validar el funcionamiento.

---

## 3. Implementación de las clases

### Clase Node.java

```java
package extra1;

/**
 * Clase que representa un nodo individual en la lista enlazada.
 */
class Node {
    String data;
    Node next;

    public Node(String data) {
        this.data = data;
        this.next = null;
    }
}

```

### Clase InfiniteOrderedStringList.java

```java
package extra1;

/**
 * Lista enlazada simple que mantiene sus elementos ordenados alfabéticamente.
 */
public class InfiniteOrderedStringList {
    private Node head;

    /**
     * Constructor que crea una nueva lista vacía.
     */
    public InfiniteOrderedStringList() {
        this.head = null;
    }

    /**
     * Añade un elemento manteniendo el orden alfabético.
     * @param word Palabra a añadir (no null).
     * @return true si se añade, false si es null.
     */
    public boolean add(String word) {
        if (word == null) return false;

        Node newNode = new Node(word);

        // Caso 1: Lista vacía o el nuevo elemento va al principio
        if (head == null || head.data.compareTo(word) >= 0) {
            newNode.next = head;
            head = newNode;
            return true;
        }

        // Caso 2: Buscar la posición de inserción en medio o al final
        Node current = head;
        while (current.next != null && current.next.data.compareTo(word) < 0) {
            current = current.next;
        }
        
        newNode.next = current.next;
        current.next = newNode;
        return true;
    }

    /**
     * Elimina todas las apariciones de una palabra.
     * @param word Palabra a eliminar.
     * @return true si se eliminó al menos una, false si no existía.
     */
    public boolean delete(String word) {
        if (head == null || word == null) return false;

        boolean removed = false;

        // Eliminar ocurrencias al inicio de la lista
        while (head != null && head.data.equals(word)) {
            head = head.next;
            removed = true;
        }

        // Eliminar ocurrencias en el resto de la lista
        Node current = head;
        while (current != null && current.next != null) {
            if (current.next.data.equals(word)) {
                current.next = current.next.next;
                removed = true;
            } else {
                current = current.next;
            }
        }

        return removed;
    }

    /**
     * Busca una palabra en la lista.
     */
    public boolean find(String word) {
        Node current = head;
        while (current != null) {
            if (current.data.equals(word)) return true;
            // Optimización: si ya pasamos alfabéticamente la palabra, no está
            if (current.data.compareTo(word) > 0) break;
            current = current.next;
        }
        return false;
    }

    /**
     * Cuenta palabras que comienzan con una letra específica.
     */
    public int wordsBeginningWithLetter(char letter) {
        int count = 0;
        Node current = head;
        while (current != null) {
            if (!current.data.isEmpty() && current.data.charAt(0) == letter) {
                count++;
            }
            current = current.next;
        }
        return count;
    }

    /**
     * Cuenta palabras que terminan con una letra específica.
     */
    public int wordsEndingWithLetter(char letter) {
        int count = 0;
        Node current = head;
        while (current != null) {
            if (!current.data.isEmpty() && current.data.charAt(current.data.length() - 1) == letter) {
                count++;
            }
            current = current.next;
        }
        return count;
    }

    @Override
    public String toString() {
        if (head == null) return "no words";

        StringBuilder sb = new StringBuilder();
        Node current = head;
        while (current != null) {
            sb.append(current.data);
            if (current.next != null) {
                sb.append("-");
            }
            current = current.next;
        }
        return sb.toString();
    }
}

```

### Clase Main.java (Validador)

```java
package extra1;

public class Main {
    public static void main(String[] args) {
        InfiniteOrderedStringList list = new InfiniteOrderedStringList();

        System.out.println("--- Test Inserción Ordenada ---");
        list.add("pelota");
        list.add("ahora");
        list.add("juego");
        list.add("juego"); // Duplicado permitido
        System.out.println("Lista: " + list.toString()); 

        System.out.println("\n--- Test Búsqueda ---");
        System.out.println("¿Existe 'juego'?: " + list.find("juego"));
        System.out.println("¿Existe 'casa'?: " + list.find("casa"));

        System.out.println("\n--- Test Contadores ---");
        System.out.println("Empiezan por 'j': " + list.wordsBeginningWithLetter('j'));
        System.out.println("Terminan por 'o': " + list.wordsEndingWithLetter('o'));

        System.out.println("\n--- Test Eliminación ---");
        System.out.println("Eliminando 'juego'...");
        list.delete("juego");
        System.out.println("Lista tras delete: " + list.toString());

        System.out.println("\n--- Test Lista Vacía ---");
        list.delete("ahora");
        list.delete("pelota");
        System.out.println("Estado final: " + list.toString());
    }
}

```

---

## 4. Explicación paso a paso

1. **La estructura del Nodo**: Creamos una clase `Node` muy sencilla. Su único propósito es contener un `String` y una "flecha" (`next`) que apunta al siguiente objeto `Node`. Al ser del mismo paquete, la clase principal puede acceder a sus campos fácilmente.
2. **Inserción Ordenada (`add`)**: Esta es la parte más compleja. Usamos `compareTo` de la clase String.
* Si la palabra es menor (alfabéticamente) que la que está en la "cabeza" (`head`), se convierte en la nueva cabeza.
* Si no, recorremos la lista hasta encontrar un nodo cuyo "siguiente" sea mayor que la palabra que queremos insertar.


3. **Borrado Masivo (`delete`)**: El ejercicio pide borrar **todas** las apariciones.
* Primero limpiamos si la palabra está al principio (usando un `while` por si hay varios duplicados al inicio).
* Luego recorremos el resto saltándonos los nodos que coincidan con la palabra.


4. **Búsqueda Eficiente (`find`)**: Aunque es una lista enlazada, aprovechamos que está ordenada. Si buscamos "casa" y vamos por la "m", dejamos de buscar porque sabemos que no aparecerá después.
5. **Formatos y Conteo**: Para `toString`, usamos `StringBuilder` por eficiencia, ya que concatenar Strings con `+` en bucles consume mucha memoria. Los métodos de conteo simplemente recorren la lista de principio a fin verificando el primer o último carácter de cada `String`.
