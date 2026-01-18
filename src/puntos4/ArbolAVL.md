### ArbolAVL

## 1. Enunciado del ejercicio

**Árbol AVL.**
Dibuja el árbol AVL resultante al insertar la siguiente secuencia de números enteros: **5, 10, 12, 6, 8, 14, 15**.
Debes representar los diferentes pasos en la construcción del árbol, y explicar en cada caso, si procede, si se ha realizado algún tipo de rotación.

---

## 2. Estructura de clases a realizar

Para esta solución, organizaremos el código en un único paquete para facilitar su lectura:

1. **`Nodo.java`**: Representa un nodo del árbol, almacenando el valor, la altura y las referencias a los hijos.
2. **`ArbolAVL.java`**: Contiene la lógica de inserción, cálculo de alturas y las cuatro rotaciones posibles (Simple Izquierda, Simple Derecha, Doble Izquierda, Doble Derecha).
3. **`Main.java`**: Clase encargada de ejecutar la secuencia solicitada y mostrar el estado del árbol.

---

## 3. Implementación en Java

### Clase Nodo

```java
public class Nodo {
    int valor, altura;
    Nodo izquierdo, derecho;

    public Nodo(int d) {
        valor = d;
        altura = 1;
    }
}

```

### Clase ArbolAVL

```java
public class ArbolAVL {
    Nodo raiz;

    // Obtener la altura de un nodo
    int altura(Nodo n) {
        return (n == null) ? 0 : n.altura;
    }

    // Obtener el factor de equilibrio
    int obtenerEquilibrio(Nodo n) {
        return (n == null) ? 0 : altura(n.izquierdo) - altura(n.derecho);
    }

    // Rotación a la derecha
    Nodo rotarDerecha(Nodo y) {
        Nodo x = y.izquierdo;
        Nodo T2 = x.derecho;
        x.derecho = y;
        y.izquierdo = T2;
        y.altura = Math.max(altura(y.izquierdo), altura(y.derecho)) + 1;
        x.altura = Math.max(altura(x.izquierdo), altura(x.derecho)) + 1;
        return x;
    }

    // Rotación a la izquierda
    Nodo rotarIzquierda(Nodo x) {
        Nodo y = x.derecho;
        Nodo T2 = y.izquierdo;
        y.izquierdo = x;
        x.derecho = T2;
        x.altura = Math.max(altura(x.izquierdo), altura(x.derecho)) + 1;
        y.altura = Math.max(altura(y.izquierdo), altura(y.derecho)) + 1;
        return y;
    }

    public void insertar(int valor) {
        raiz = insertarRecursivo(raiz, valor);
    }

    private Nodo insertarRecursivo(Nodo nodo, int valor) {
        if (nodo == null) return new Nodo(valor);

        if (valor < nodo.valor)
            nodo.izquierdo = insertarRecursivo(nodo.izquierdo, valor);
        else if (valor > nodo.valor)
            nodo.derecho = insertarRecursivo(nodo.derecho, valor);
        else return nodo;

        nodo.altura = 1 + Math.max(altura(nodo.izquierdo), altura(nodo.derecho));
        int fe = obtenerEquilibrio(nodo);

        // Caso Izquierda-Izquierda
        if (fe > 1 && valor < nodo.izquierdo.valor) return rotarDerecha(nodo);
        // Caso Derecha-Derecha
        if (fe < -1 && valor > nodo.derecho.valor) return rotarIzquierda(nodo);
        // Caso Izquierda-Derecha
        if (fe > 1 && valor > nodo.izquierdo.valor) {
            nodo.izquierdo = rotarIzquierda(nodo.izquierdo);
            return rotarDerecha(nodo);
        }
        // Caso Derecha-Izquierda
        if (fe < -1 && valor < nodo.derecho.valor) {
            nodo.derecho = rotarDerecha(nodo.derecho);
            return rotarIzquierda(nodo);
        }
        return nodo;
    }

    public void imprimirEnOrden(Nodo nodo) {
        if (nodo != null) {
            imprimirEnOrden(nodo.izquierdo);
            System.out.print(nodo.valor + " ");
            imprimirEnOrden(nodo.derecho);
        }
    }
}

```

### Clase Main

```java
public class Main {
    public static void main(String[] args) {
        ArbolAVL arbol = new ArbolAVL();
        int[] valores = {5, 10, 12, 6, 8, 14, 15};

        System.out.println("Insertando secuencia: 5, 10, 12, 6, 8, 14, 15\n");
        for (int v : valores) {
            arbol.insertar(v);
        }

        System.out.print("Recorrido In-Order del árbol final: ");
        arbol.imprimirEnOrden(arbol.raiz);
    }
}

```

---

## 4. Explicación paso a paso (Lógica y Rotaciones)

Para entender qué hace el código, analicemos la construcción visual del árbol tras cada inserción:

1. **Insertar 5**: El 5 es la raíz.
2. **Insertar 10**: Se coloca a la derecha del 5. El árbol sigue equilibrado.
3. **Insertar 12**: Se coloca a la derecha del 10.
* *Problema:* El nodo 5 tiene un factor de equilibrio de -2.
* *Solución:* **Rotación Simple a la Izquierda** sobre el 5.
* *Resultado:* El 10 sube como raíz, el 5 es su hijo izquierdo y el 12 el derecho.


4. **Insertar 6**: Se coloca a la izquierda del 12 (hijo del 10). El árbol sigue equilibrado.
5. **Insertar 8**: Se coloca a la derecha del 6.
* *Problema:* El nodo 12 queda desequilibrado. Se detecta un caso "Izquierda-Derecha".
* *Solución:* **Rotación Doble a la Izquierda** (primero rotar 6 a la izquierda, luego 12 a la derecha).
* *Resultado:* El 8 sube, el 6 queda a su izquierda y el 12 a su derecha.


6. **Insertar 14**: Se coloca a la derecha del 12.
7. **Insertar 15**: Se coloca a la derecha del 14.
* *Problema:* El nodo 12 queda desequilibrado (Factor -2).
* *Solución:* **Rotación Simple a la Izquierda**. El 14 sube, el 12 a su izquierda y el 15 a su derecha.



### Resumen de la lógica implementada:

* **Factor de Equilibrio ():** Se calcula como . Un nodo está desequilibrado si .
* **Recursividad:** Tras cada inserción, el código "vuelve" hacia atrás por la rama de inserción actualizando alturas y verificando el equilibrio de cada ancestro.
* **Eficiencia:** Gracias a estas rotaciones, garantizamos que la altura del árbol siempre sea , lo que hace que las búsquedas sean rapidísimas.
