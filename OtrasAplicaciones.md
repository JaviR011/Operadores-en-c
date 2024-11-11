

---

### Apuntadores a Funciones
Los apuntadores a funciones permiten almacenar la dirección de una función y llamar a esa función a través del apuntador. Esto es útil para pasar funciones como parámetros, o para implementar callbacks y simplificar el manejo de código modular.

#### Ejemplo de Apuntadores a Funciones
Primero, veamos cómo declarar un apuntador a una función y cómo usarlo:

```c
#include <stdio.h>

void saludar() {
    printf("¡Hola!\n");
}

int sumar(int a, int b) {
    return a + b;
}

int main() {
    void (*ptr_saludar)() = &saludar; // Apuntador a la función saludar
    ptr_saludar(); // Llama a saludar a través del apuntador

    int (*ptr_sumar)(int, int) = &sumar; // Apuntador a la función sumar
    int resultado = ptr_sumar(5, 3); // Llama a sumar a través del apuntador
    printf("Resultado: %d\n", resultado); // Imprime 8

    return 0;
}
```

Aquí hemos declarado dos apuntadores a funciones: uno para una función sin parámetros (`saludar`) y otro para una función que recibe dos enteros (`sumar`). Estos apuntadores permiten llamar a las funciones sin usar sus nombres directamente.

---

### Memoria Dinámica para Estructuras
La memoria dinámica en estructuras permite crear estructuras que pueden cambiar su tamaño o asignarse solo cuando se necesitan, lo cual ahorra memoria y es especialmente útil en estructuras de datos complejas.

#### Ejemplo de Asignación Dinámica en Estructuras
En este ejemplo, vamos a crear una estructura llamada `Persona` y usaremos memoria dinámica para asignar espacio solo cuando sea necesario.

```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

typedef struct {
    char *nombre;
    int edad;
} Persona;

int main() {
    Persona *p = (Persona *)malloc(sizeof(Persona)); // Asigna memoria para una estructura Persona
    if (p == NULL) {
        printf("Error al asignar memoria.\n");
        return 1;
    }

    p->nombre = (char *)malloc(50 * sizeof(char)); // Asigna memoria para el nombre
    if (p->nombre == NULL) {
        printf("Error al asignar memoria para el nombre.\n");
        free(p);
        return 1;
    }

    strcpy(p->nombre, "Carlos");
    p->edad = 25;

    printf("Nombre: %s, Edad: %d\n", p->nombre, p->edad);

    // Libera la memoria
    free(p->nombre);
    free(p);

    return 0;
}
```

En este ejemplo, asignamos memoria para un apuntador a `Persona`, así como para su miembro `nombre`. Esto es útil para manejar estructuras más complejas sin desperdiciar memoria.

---

### Listas Enlazadas con Apuntadores
Una lista enlazada es una estructura de datos que utiliza nodos, donde cada nodo tiene un apuntador que apunta al siguiente nodo. Las listas enlazadas se usan cuando necesitamos una estructura dinámica que permita agregar o quitar elementos sin reorganizar toda la memoria.

#### Ejemplo de Lista Enlazada Simple
Aquí crearemos una lista enlazada de enteros.

```c
#include <stdio.h>
#include <stdlib.h>

typedef struct Nodo {
    int dato;
    struct Nodo *siguiente;
} Nodo;

void agregar_nodo(Nodo **cabeza, int valor) {
    Nodo *nuevo_nodo = (Nodo *)malloc(sizeof(Nodo));
    nuevo_nodo->dato = valor;
    nuevo_nodo->siguiente = *cabeza;
    *cabeza = nuevo_nodo;
}

void imprimir_lista(Nodo *nodo) {
    while (nodo != NULL) {
        printf("%d -> ", nodo->dato);
        nodo = nodo->siguiente;
    }
    printf("NULL\n");
}

void liberar_lista(Nodo *nodo) {
    Nodo *temp;
    while (nodo != NULL) {
        temp = nodo;
        nodo = nodo->siguiente;
        free(temp);
    }
}

int main() {
    Nodo *cabeza = NULL;

    agregar_nodo(&cabeza, 3);
    agregar_nodo(&cabeza, 5);
    agregar_nodo(&cabeza, 9);

    printf("Lista enlazada: ");
    imprimir_lista(cabeza);

    liberar_lista(cabeza);

    return 0;
}
```

En este código:
- La función `agregar_nodo` añade un nuevo nodo al inicio de la lista.
- La función `imprimir_lista` recorre la lista e imprime cada valor.
- La función `liberar_lista` libera la memoria de cada nodo de la lista.

---

### Apuntadores Constantes y Constantes Apuntadas
La combinación de `const` con apuntadores permite crear apuntadores cuyos valores o direcciones no pueden cambiar, aumentando la seguridad en el manejo de variables.

1. **Apuntador constante:** La dirección de memoria no puede cambiar, pero el valor al que apunta sí.

   ```c
   int valor = 10;
   int otro = 20;
   int *const ptr = &valor; // Apuntador constante

   *ptr = 15; // Esto es válido
   ptr = &otro; // Error: no se puede cambiar la dirección de un apuntador constante
   ```

2. **Apuntador a constante:** La dirección puede cambiar, pero el valor al que apunta no puede modificarse.

   ```c
   const int valor = 10;
   const int *ptr = &valor; // Apuntador a constante

   *ptr = 15; // Error: no se puede cambiar el valor al que apunta
   int otro = 20;
   ptr = &otro; // Esto es válido
   ```

3. **Apuntador constante a constante:** Ni la dirección ni el valor pueden cambiar.

   ```c
   const int valor = 10;
   const int *const ptr = &valor; // Apuntador constante a constante

   *ptr = 15; // Error: no se puede cambiar el valor
   ptr = &otro; // Error: no se puede cambiar la dirección
   ```

---

### Arrays Dinámicos con Apuntadores
Los arreglos dinámicos permiten ajustar el tamaño de un arreglo en tiempo de ejecución, usando `malloc` o `realloc`.

```c
#include <stdio.h>
#include <stdlib.h>

int main() {
    int *arr;
    int tamano = 5;

    arr = (int *)malloc(tamano * sizeof(int)); // Reserva espacio para 5 enteros
    if (arr == NULL) {
        printf("Error al asignar memoria.\n");
        return 1;
    }

    // Inicializa el arreglo
    for (int i = 0; i < tamano; i++) {
        arr[i] = i + 1;
    }

    // Redimensiona el arreglo a 10 elementos
    tamano = 10;
    arr = (int *)realloc(arr, tamano * sizeof(int));

    // Imprime el arreglo redimensionado
    for (int i = 0; i < tamano; i++) {
        printf("%d ", arr[i]);
    }

    free(arr); // Libera la memoria

    return 0;
}
```

En este código:
- Inicialmente asignamos espacio para un arreglo de 5 enteros y luego lo redimensionamos a 10 usando `realloc`.
- Al final, liberamos la memoria asignada.

---
