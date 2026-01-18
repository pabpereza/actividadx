### Binary Search Tree

**Ejercicio Global: Gestión Avanzada de Árbol Binario de Búsqueda**

**MUY IMPORTANTE:** Respetar paquete `exam`, nombres de clases y firmas de métodos.

Se proporciona la implementación básica de `Node` y `BinarySearchTree` (con método `insert`). Se pide implementar en la clase `BinarySearchTree` los siguientes métodos (respetando si se pide iterativo o recursivo):

1. **Conteo por valor (Recursivos):**
* `public int countOddNodes()`: Devuelve el número de nodos con valor impar.
* `public int countEvenNodes()`: Devuelve el número de nodos con valor par.


2. **Búsqueda de Máximos:**
* `public Integer recursiveFindMax()`: Devuelve el valor máximo del árbol usando **recursividad**. Si está vacío, devuelve `null`.
* `public Integer iterativeFindMax()`: Devuelve el valor máximo del árbol usando un **algoritmo iterativo**. Si está vacío, devuelve `null`.


3. **Conteo por estructura (Recursivos):**
* `public int countLeaves()`: Devuelve el número de nodos hoja (sin hijos).
* `public int countInnerNodes()`: Devuelve el número de nodos internos (con 1 o 2 hijos). Si el árbol tiene solo la raíz sin hijos o está vacío, devuelve 0.



---

### 2. Estructura de Clases

Generaremos tres archivos en el paquete `exam`:

1. **`Node.java`**: Estructura del nodo (sin cambios).
2. **`BinarySearchTree.java`**: Clase principal con la lógica de inserción y los **6 métodos solicitados**.
3. **`Main.java`**: Clase de prueba que valida todos los casos.

---

### 3. Implementación de las clases Java

#### Archivo: `Node.java`

```java
package exam;

public class Node {
    Integer value;
    Node left;
    Node right;

    public Node(int v) {
        value = v;
        left = null;
        right = null;
    }
}

```

#### Archivo: `BinarySearchTree.java`

Esta es la clase que contiene toda la lógica combinada de los ejercicios.

```java
package exam;

public class BinarySearchTree {
    private Node root;

    // CONSTRUCTOR
    public BinarySearchTree() {
        root = null;
    }

    // --- LÓGICA BASE (Proporcionada) ---
    public void insert(Integer v) {
        if (v == null) return;
        root = recursiveInsert(root, v);
    }

    private Node recursiveInsert(Node node, int v) {
        if (node == null)
            node = new Node(v);
        else if (v < node.value)
            node.left = recursiveInsert(node.left, v);
        else if (v > node.value)
            node.right = recursiveInsert(node.right, v);
        return node;
    }

    // =========================================================
    // GRUPO 1: CONTEO POR VALOR (PARES / IMPARES)
    // =========================================================

    public int countOddNodes() {
        return countOddRecursive(root);
    }

    private int countOddRecursive(Node node) {
        if (node == null) return 0;
        int esImpar = (node.value % 2 != 0) ? 1 : 0;
        return esImpar + countOddRecursive(node.left) + countOddRecursive(node.right);
    }

    public int countEvenNodes() {
        return countEvenRecursive(root);
    }

    private int countEvenRecursive(Node node) {
        if (node == null) return 0;
        int esPar = (node.value % 2 == 0) ? 1 : 0;
        return esPar + countEvenRecursive(node.left) + countEvenRecursive(node.right);
    }

    // =========================================================
    // GRUPO 2: BÚSQUEDA DE MÁXIMOS (RECURSIVO E ITERATIVO)
    // =========================================================

    /**
     * En un BST, el máximo siempre está lo más a la derecha posible.
     */
    public Integer recursiveFindMax() {
        if (root == null) return null;
        return recursiveFindMaxHelper(root);
    }

    private Integer recursiveFindMaxHelper(Node node) {
        // Caso base: si no hay hijo derecho, este nodo es el máximo
        if (node.right == null) {
            return node.value;
        }
        // Llamada recursiva: seguir buscando a la derecha
        return recursiveFindMaxHelper(node.right);
    }

    public Integer iterativeFindMax() {
        if (root == null) return null;

        Node current = root;
        // Mientras exista un hijo a la derecha, avanzamos
        while (current.right != null) {
            current = current.right;
        }
        return current.value;
    }

    // =========================================================
    // GRUPO 3: CONTEO POR ESTRUCTURA (HOJAS / INTERNOS)
    // =========================================================

    public int countLeaves() {
        return countLeavesRecursive(root);
    }

    private int countLeavesRecursive(Node node) {
        if (node == null) return 0;

        // Si no tiene hijo izq NI hijo der, es una hoja. Devuelve 1.
        if (node.left == null && node.right == null) {
            return 1;
        }

        // Si no es hoja, suma las hojas de sus subárboles
        return countLeavesRecursive(node.left) + countLeavesRecursive(node.right);
    }

    public int countInnerNodes() {
        return countInnerRecursive(root);
    }

    private int countInnerRecursive(Node node) {
        if (node == null) return 0;

        // Un nodo es interno si tiene al menos un hijo (no es hoja).
        boolean esInterno = (node.left != null || node.right != null);

        // Si es interno sumamos 1, más lo que encontremos abajo
        int count = esInterno ? 1 : 0;
        
        return count + countInnerRecursive(node.left) + countInnerRecursive(node.right);
    }
}

```

#### Archivo: `Main.java`

He diseñado un árbol específico para probar todas las condiciones a la vez.

```java
package exam;

public class Main {
    public static void main(String[] args) {
        BinarySearchTree bst = new BinarySearchTree();

        /* CONSTRUCCIÓN DEL ÁRBOL DE PRUEBA
         * 10
         * /  \
         * 5    15
         * / \     \
         * 3   7     20
         * * Análisis visual:
         * - Nodos: 6
         * - Valores Impares: 5, 15, 3, 7 (Total: 4)
         * - Valores Pares: 10, 20 (Total: 2)
         * - Máximo: 20
         * - Hojas (sin hijos): 3, 7, 20 (Total: 3)
         * - Internos (con hijos): 10, 5, 15 (Total: 3)
         */

        bst.insert(10);
        bst.insert(5);
        bst.insert(15);
        bst.insert(3);
        bst.insert(7);
        bst.insert(20);

        System.out.println("--- EJECUCIÓN DE PRUEBAS UNIFICADAS ---");
        
        // 1. Pruebas de Pares/Impares
        System.out.println("1. Impares (Esperado 4): " + bst.countOddNodes());
        System.out.println("   Pares   (Esperado 2): " + bst.countEvenNodes());

        // 2. Pruebas de Máximos
        System.out.println("2. Max Recursivo (Esperado 20): " + bst.recursiveFindMax());
        System.out.println("   Max Iterativo (Esperado 20): " + bst.iterativeFindMax());

        // 3. Pruebas de Estructura (Hojas/Internos)
        System.out.println("3. Hojas    (Esperado 3): " + bst.countLeaves());
        System.out.println("   Internos (Esperado 3): " + bst.countInnerNodes());
        
        System.out.println("\n--- PRUEBA DE CASO BORDE (ÁRBOL VACÍO) ---");
        BinarySearchTree emptyTree = new BinarySearchTree();
        System.out.println("Max en vacio (Esperado null): " + emptyTree.recursiveFindMax());
        System.out.println("Hojas en vacio (Esperado 0): " + emptyTree.countLeaves());
    }
}

```

---

### 4. Explicación paso a paso de la implementación

Aquí te explico la lógica didáctica detrás de los nuevos métodos añadidos:

#### A. Búsqueda del Máximo (`FindMax`)

En un árbol binario de búsqueda (BST), los valores mayores siempre se insertan a la derecha. Por tanto, el valor máximo es el nodo que está **más a la derecha** del árbol (el nodo que ya no tiene hijo derecho).

1. **Versión Recursiva (`recursiveFindMax`):**
* Llama a un ayudante privado.
* **Caso base:** Si `node.right` es `null`, significa que no podemos ir más a la derecha. Hemos encontrado el máximo. Devolvemos `node.value`.
* **Recursión:** Si `node.right` existe, nos llamamos a nosotros mismos pasándole ese hijo derecho.


2. **Versión Iterativa (`iterativeFindMax`):**
* No usa métodos auxiliares. Usamos un bucle `while`.
* Empezamos en la raíz (`current = root`).
* Mientras `current.right` no sea `null`, actualizamos `current` para que sea su hijo derecho.
* Al salir del bucle, `current` es el nodo más a la derecha.



#### B. Conteo de Estructura (`Leaves` e `InnerNodes`)

Estos métodos analizan la "forma" del árbol, no los valores numéricos.

1. **Contar Hojas (`countLeaves`):**
* Una hoja se define como un nodo donde `left` es `null` **Y** `right` es `null`.
* Si detectamos esto, devolvemos 1.
* Si no, sumamos lo que devuelvan las llamadas recursivas izquierda y derecha.


2. **Contar Nodos Internos (`countInnerNodes`):**
* Un nodo interno es lo contrario a una hoja: tiene al menos un hijo.
* La condición es `(left != null || right != null)`.
* Si se cumple, sumamos 1, y seguimos explorando recursivamente hacia abajo para ver si hay más nodos internos.
* *Nota:* Este método cuenta la raíz como interna si tiene hijos.



Con este código cubres prácticamente todas las variantes que te pueden pedir sobre recorridos y conteos básicos en un BST para el examen. ¡Mucha suerte!