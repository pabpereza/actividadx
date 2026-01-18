### BinarySearchTree

## 1. Enunciado del Ejercicio

Implementar en la clase `BinarySearchTree` los siguientes métodos, ambos deben hacer uso de la recursividad:

* **`public int countLeaves()`**: Devuelve el número de nodos hoja del árbol. Si el árbol está vacío devuelve 0, y si solo tienen un nodo, devuelve 1.
* **`public int countInnerNodes()`**: Devuelve el número de nodos internos del árbol, es decir, aquellos que tienen 1 o 2 hijos. Si el árbol está vacío o solo tiene un nodo, devuelve 0.

**NOTA:** Si el código entregado no compila, el ejercicio no será calificado.
**ENTREGA:** Ficheros `*.java` codificados y debidamente documentados.

---

## 2. Estructura de Clases

Para cumplir con los requisitos del paquete `exam` y las especificaciones de las imágenes, la estructura es la siguiente:

1. **`Node.java`**: Clase que representa el nodo del árbol (valor, hijo izquierdo e hijo derecho).
2. **`BinarySearchTree.java`**: Clase principal que contiene la lógica del árbol y los nuevos métodos recursivos.
3. **`Main.java`**: Clase de prueba para validar que el conteo de hojas y nodos internos funciona correctamente.

---

## 3. Implementación en Java

A continuación, presento el código íntegro siguiendo las firmas y estructuras solicitadas.

### Archivo: Node.java

```java
package exam;

/**
 * Clase que representa un nodo en un árbol binario de búsqueda.
 */
public class Node {
    Integer value; // Valor del nodo
    Node left;     // Referencia al hijo izquierdo
    Node right;    // Referencia al hijo derecho

    /**
     * Constructor para crear un nuevo nodo.
     * @param v Valor entero a almacenar.
     */
    public Node(int v) {
        value = v;
        left = null;
        right = null;
    }
}

```

### Archivo: BinarySearchTree.java

```java
package exam;

/**
 * Clase que implementa un Árbol Binario de Búsqueda (BST) con métodos
 * para contar hojas y nodos internos de forma recursiva.
 */
public class BinarySearchTree {
    private Node root;

    // CONSTRUCTOR DE ÁRBOL VACÍO
    public BinarySearchTree() {
        root = null;
    }

    // INSERCIÓN DE UN NUEVO VALOR
    public void insert(Integer v) {
        if (v == null) return; // Ignorar valores nulos
        root = recursiveInsert(root, v);
    }

    // MÉTODO RECURSIVO DE APOYO PARA INSERCIÓN
    private Node recursiveInsert(Node node, int v) {
        if (node == null) {
            node = new Node(v);
        } else if (v < node.value) {
            node.left = recursiveInsert(node.left, v);
        } else if (v > node.value) {
            node.right = recursiveInsert(node.right, v);
        }
        return node;
    }

    // ======================================================
    // MÉTODOS SOLICITADOS EN EL EJERCICIO
    // ======================================================

    /**
     * Cuenta el número de nodos hoja (nodos sin hijos).
     * @return Cantidad de hojas en el árbol.
     */
    public int countLeaves() {
        return recursiveCountLeaves(root);
    }

    private int recursiveCountLeaves(Node node) {
        // Caso base 1: Árbol o subárbol vacío
        if (node == null) return 0;
        
        // Caso base 2: Es una hoja (no tiene hijos)
        if (node.left == null && node.right == null) {
            return 1;
        }
        
        // Caso recursivo: Sumar hojas del lado izquierdo y derecho
        return recursiveCountLeaves(node.left) + recursiveCountLeaves(node.right);
    }

    /**
     * Cuenta el número de nodos internos (nodos con al menos un hijo).
     * @return Cantidad de nodos internos.
     */
    public int countInnerNodes() {
        return recursiveCountInnerNodes(root);
    }

    private int recursiveCountInnerNodes(Node node) {
        // Caso base 1: Árbol vacío o llegamos a un nodo nulo
        if (node == null) return 0;
        
        // Caso base 2: Es una hoja (no es un nodo interno)
        if (node.left == null && node.right == null) {
            return 0;
        }
        
        // Caso recursivo: Es un nodo interno (tiene al menos un hijo).
        // Sumamos 1 (por el nodo actual) más lo que devuelvan sus hijos.
        return 1 + recursiveCountInnerNodes(node.left) + recursiveCountInnerNodes(node.right);
    }
}

```

### Archivo: Main.java

```java
package exam;

public class Main {
    public static void main(String[] args) {
        BinarySearchTree tree = new BinarySearchTree();

        // Creamos un árbol de ejemplo:
        //        10
        //       /  \
        //      5    15
        //     / \     \
        //    2   7     20
        
        tree.insert(10);
        tree.insert(5);
        tree.insert(15);
        tree.insert(2);
        tree.insert(7);
        tree.insert(20);

        System.out.println("--- Pruebas de Árbol Binario ---");
        System.out.println("Número de hojas (esperado 3: [2, 7, 20]): " + tree.countLeaves());
        System.out.println("Número de nodos internos (esperado 3: [10, 5, 15]): " + tree.countInnerNodes());
        
        // Prueba con un solo nodo
        BinarySearchTree singleNodeTree = new BinarySearchTree();
        singleNodeTree.insert(100);
        System.out.println("\n--- Pruebas Árbol Un solo nodo ---");
        System.out.println("Hojas (esperado 1): " + singleNodeTree.countLeaves());
        System.out.println("Internos (esperado 0): " + singleNodeTree.countInnerNodes());
    }
}

```

---

## 4. Explicación paso a paso

Para resolver este ejercicio, hemos aplicado la técnica de **"Divide y Vencerás"** mediante recursividad. Aquí te explico la lógica:

### Método `countLeaves()` (Contar Hojas)

1. **Base del Abismo**: Si el nodo actual es `null`, devolvemos `0`. No hay nada que contar.
2. **Identificación de Hoja**: Un nodo es hoja si tanto su hijo izquierdo como el derecho son `null`. Si esto se cumple, devolvemos `1`.
3. **Recursión**: Si el nodo no es hoja, le preguntamos a su hijo izquierdo cuántas hojas tiene y a su hijo derecho lo mismo. Sumamos ambos resultados.

### Método `countInnerNodes()` (Contar Nodos Internos)

1. **Caso Vacío**: Si el nodo es `null`, devolvemos `0`.
2. **Descarte de Hojas**: El enunciado dice que un nodo interno es aquel que tiene 1 o 2 hijos. Por tanto, si un nodo no tiene hijos (es hoja), devolvemos `0`, ya que no cuenta como interno.
3. **Conteo de Internos**: Si el nodo tiene al menos un hijo, lo contamos a él mismo (`1`) y sumamos recursivamente los nodos internos que se encuentren en sus ramas izquierda y derecha.

### Estructura de Apoyo

He mantenido la visibilidad de los métodos solicitados como `public` y he creado métodos `private` auxiliares que aceptan el parámetro `Node`. Esto es una **buena práctica de encapsulamiento**, permitiendo que el usuario de la clase llame al método sin tener que conocer la estructura interna (la raíz) del árbol.
