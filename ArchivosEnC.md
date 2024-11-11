
---

## 1. Abrir un Archivo

### `fopen`

Para abrir un archivo, utiliza la función `fopen`, que devuelve un puntero de tipo `FILE`. Necesita dos argumentos:

- **Nombre del archivo**: Cadena con el nombre o ruta del archivo.
- **Modo de apertura**: Cadena que indica cómo abrir el archivo.

Ejemplo:

```c
FILE *file = fopen("ejemplo.txt", "r");
if (file == NULL) {
    perror("Error al abrir el archivo");
    return 1;
}
```

### Modos de apertura

Cada modo de apertura tiene una función específica:

- `"r"`: Abre un archivo para lectura. Falla si el archivo no existe.
- `"w"`: Abre un archivo para escritura. Crea el archivo si no existe y lo sobrescribe si ya existe.
- `"a"`: Abre un archivo para agregar datos al final (sin borrar el contenido existente). Crea el archivo si no existe.
- `"r+"`: Abre un archivo para lectura y escritura sin borrar el contenido. Falla si el archivo no existe.
- `"w+"`: Abre un archivo para lectura y escritura. Crea un archivo nuevo o sobrescribe el existente.
- `"a+"`: Abre un archivo para lectura y escritura. Permite escribir solo al final del archivo. Crea el archivo si no existe.

Ejemplo de apertura en modo de escritura:

```c
FILE *file = fopen("salida.txt", "w");
if (file == NULL) {
    perror("Error al abrir el archivo");
    return 1;
}
```

---

## 2. Lectura de un Archivo

### `fgetc`

Lee un solo carácter del archivo. Devuelve el carácter leído o `EOF` si llega al final.

Ejemplo:

```c
int c;
FILE *file = fopen("lectura.txt", "r");
if (file == NULL) {
    perror("Error al abrir el archivo");
    return 1;
}

while ((c = fgetc(file)) != EOF) {
    putchar(c);  // Imprime el carácter en la consola
}

fclose(file);
```

### `fgets`

Lee una línea de texto del archivo y la guarda en una cadena. Muy útil para archivos de texto.

Ejemplo:

```c
char linea[100];
FILE *file = fopen("lectura.txt", "r");
if (file == NULL) {
    perror("Error al abrir el archivo");
    return 1;
}

while (fgets(linea, sizeof(linea), file) != NULL) {
    printf("%s", linea);  // Imprime la línea
}

fclose(file);
```

### `fread`

Lee un bloque de datos de un archivo, útil para archivos binarios.

Ejemplo:

```c
char buffer[20];
FILE *file = fopen("archivo.bin", "rb");
if (file == NULL) {
    perror("Error al abrir el archivo");
    return 1;
}

size_t bytes_leidos = fread(buffer, sizeof(char), sizeof(buffer), file);
if (bytes_leidos > 0) {
    // Procesar los datos leídos
}

fclose(file);
```

---

## 3. Escritura en un Archivo

### `fputc`

Escribe un solo carácter en el archivo.

Ejemplo:

```c
FILE *file = fopen("salida.txt", "w");
if (file == NULL) {
    perror("Error al abrir el archivo");
    return 1;
}

fputc('H', file);
fputc('o', file);
fputc('l', file);
fputc('a', file);

fclose(file);
```

### `fputs`

Escribe una cadena de caracteres en el archivo.

Ejemplo:

```c
FILE *file = fopen("salida.txt", "w");
if (file == NULL) {
    perror("Error al abrir el archivo");
    return 1;
}

fputs("Este es un texto de ejemplo.\n", file);

fclose(file);
```

### `fprintf`

Es similar a `printf`, pero permite escribir en un archivo.

Ejemplo:

```c
FILE *file = fopen("salida.txt", "w");
if (file == NULL) {
    perror("Error al abrir el archivo");
    return 1;
}

int valor = 42;
fprintf(file, "El valor es: %d\n", valor);

fclose(file);
```

### `fwrite`

Escribe un bloque de datos en un archivo, útil para archivos binarios.

Ejemplo:

```c
char data[] = "Datos binarios";
FILE *file = fopen("archivo.bin", "wb");
if (file == NULL) {
    perror("Error al abrir el archivo");
    return 1;
}

fwrite(data, sizeof(char), sizeof(data), file);

fclose(file);
```

---

## 4. Cerrar un Archivo

Siempre que termines de trabajar con un archivo, ciérralo con `fclose` para asegurarte de que los datos se guarden y liberar los recursos.

```c
fclose(file);
```

Si intentas usar el archivo después de cerrarlo, ocurrirá un error. Siempre revisa si `fclose` se ejecutó correctamente (devuelve 0 si tuvo éxito).

---

## 5. Otras Funciones Útiles

### `feof`

Verifica si se alcanzó el final del archivo.

Ejemplo:

```c
while (!feof(file)) {
    // Lee hasta el final del archivo
}
```

### `fseek` y `ftell`

Permiten moverse dentro de un archivo y conocer la posición actual.

- **`fseek`**: Cambia la posición en el archivo.
  ```c
  fseek(file, 0, SEEK_END); // Moverse al final del archivo
  ```
  
- **`ftell`**: Devuelve la posición actual en el archivo.
  ```c
  long posicion = ftell(file); // Posición actual en bytes
  ```

Ejemplo de mover el cursor al inicio y luego obtener la posición:

```c
fseek(file, 0, SEEK_SET);  // Mover el cursor al inicio
long pos = ftell(file);    // Debería ser 0
```

### `rewind`

Lleva el cursor al principio del archivo sin necesidad de usar `fseek`.

```c
rewind(file);
```

### `remove`

Borra un archivo del sistema de archivos.

Ejemplo:

```c
if (remove("archivo_para_borrar.txt") == 0) {
    printf("Archivo eliminado con éxito\n");
} else {
    perror("Error al eliminar el archivo");
}
```

---

## 6. Ejemplo Completo: Lectura y Escritura en un Archivo

Aquí tienes un programa que abre un archivo, escribe en él, lee su contenido y lo muestra en pantalla:

```c
#include <stdio.h>

int main() {
    FILE *file = fopen("ejemplo.txt", "w+");
    if (file == NULL) {
        perror("Error al abrir el archivo");
        return 1;
    }

    // Escribimos en el archivo
    fprintf(file, "Este es un texto de ejemplo.\n");
    fprintf(file, "Escribiendo más texto.\n");

    // Volvemos al inicio del archivo para leer desde allí
    rewind(file);

    // Leemos y mostramos el contenido
    char linea[100];
    while (fgets(linea, sizeof(linea), file) != NULL) {
        printf("%s", linea);  // Imprime cada línea leída
    }

    // Cerramos el archivo
    fclose(file);
    return 0;
}
```

### Explicación

1. Abrimos el archivo `ejemplo.txt` en modo `w+`.
2. Escribimos texto usando `fprintf`.
3. Usamos `rewind` para mover el cursor al inicio.
4. Leemos el contenido usando `fgets`.
5. Cerramos el archivo.

---

## Ejemplo 1: Lectura de un Archivo con `fgetc`

### Código

Este programa lee un archivo carácter por carácter.

```c
#include <stdio.h>

int main() {
    FILE *file = fopen("lectura.txt", "r");
    if (file == NULL) {
        perror("Error al abrir el archivo");
        return 1;
    }

    int c;
    while ((c = fgetc(file)) != EOF) {
        putchar(c);  // Imprime el carácter en la consola
    }

    fclose(file);
    return 0;
}
```

### Contenido de `lectura.txt`

Supongamos que el archivo `lectura.txt` contiene:

```
Hola, este es un archivo de ejemplo.
```

### Salida Esperada en la Terminal

```
Hola, este es un archivo de ejemplo.
```

Este programa lee cada carácter del archivo y lo imprime en la terminal. Al final, la salida de la terminal es el contenido completo del archivo.

---

## Ejemplo 2: Lectura de un Archivo con `fgets`

### Código

Este programa lee un archivo línea por línea.

```c
#include <stdio.h>

int main() {
    FILE *file = fopen("lectura.txt", "r");
    if (file == NULL) {
        perror("Error al abrir el archivo");
        return 1;
    }

    char linea[100];
    while (fgets(linea, sizeof(linea), file) != NULL) {
        printf("%s", linea);  // Imprime cada línea en la consola
    }

    fclose(file);
    return 0;
}
```

### Contenido de `lectura.txt`

```
Línea 1: Bienvenidos.
Línea 2: Este es un ejemplo de lectura línea por línea.
Línea 3: Fin del archivo.
```

### Salida Esperada en la Terminal

```
Línea 1: Bienvenidos.
Línea 2: Este es un ejemplo de lectura línea por línea.
Línea 3: Fin del archivo.
```

Cada línea se lee y se imprime en la terminal hasta que se llega al final del archivo.

---

## Ejemplo 3: Escritura de un Archivo con `fputs` y `fprintf`

### Código

Este programa escribe texto en un archivo utilizando `fputs` y `fprintf`.

```c
#include <stdio.h>

int main() {
    FILE *file = fopen("salida.txt", "w");
    if (file == NULL) {
        perror("Error al abrir el archivo");
        return 1;
    }

    fputs("Esta es la primera línea de texto.\n", file);
    fprintf(file, "Este es un número: %d\n", 42);

    fclose(file);
    return 0;
}
```

### Contenido Esperado en `salida.txt`

Después de ejecutar el programa, el archivo `salida.txt` debería contener:

```
Esta es la primera línea de texto.
Este es un número: 42
```

### Salida Esperada en la Terminal

No hay salida en la terminal en este caso, ya que el programa solo escribe en el archivo y no muestra nada en pantalla.

---

## Ejemplo 4: Escritura y Lectura en un Archivo con `fprintf` y `fgets`

### Código

Este programa escribe datos en un archivo, luego los lee y los imprime en la terminal.

```c
#include <stdio.h>

int main() {
    FILE *file = fopen("ejemplo.txt", "w+");
    if (file == NULL) {
        perror("Error al abrir el archivo");
        return 1;
    }

    // Escribimos en el archivo
    fprintf(file, "Este es un texto de ejemplo.\n");
    fprintf(file, "Escribiendo más texto en el archivo.\n");

    // Volvemos al inicio del archivo para leer desde allí
    rewind(file);

    // Leemos y mostramos el contenido
    char linea[100];
    while (fgets(linea, sizeof(linea), file) != NULL) {
        printf("%s", linea);  // Imprime cada línea leída en la consola
    }

    // Cerramos el archivo
    fclose(file);
    return 0;
}
```

### Contenido Esperado en `ejemplo.txt`

Después de ejecutar el programa, el archivo `ejemplo.txt` debería contener:

```
Este es un texto de ejemplo.
Escribiendo más texto en el archivo.
```

### Salida Esperada en la Terminal

```
Este es un texto de ejemplo.
Escribiendo más texto en el archivo.
```

Este programa escribe dos líneas en el archivo, luego usa `rewind` para mover el cursor al principio y vuelve a leer el archivo, mostrando su contenido en la terminal.

---

## Ejemplo 5: Escritura y Lectura Binaria con `fwrite` y `fread`

### Código

Este programa escribe datos binarios en un archivo y luego los lee.

```c
#include <stdio.h>

int main() {
    FILE *file = fopen("datos.bin", "wb+");
    if (file == NULL) {
        perror("Error al abrir el archivo");
        return 1;
    }

    int datos[] = {1, 2, 3, 4, 5};
    size_t elementos_escritos = fwrite(datos, sizeof(int), 5, file);

    // Verificamos si se escribieron todos los elementos
    if (elementos_escritos != 5) {
        perror("Error al escribir en el archivo");
        fclose(file);
        return 1;
    }

    // Volvemos al inicio del archivo para leer desde allí
    rewind(file);

    int buffer[5];
    size_t elementos_leidos = fread(buffer, sizeof(int), 5, file);

    // Mostramos los datos leídos en la terminal
    for (int i = 0; i < elementos_leidos; i++) {
        printf("Elemento %d: %d\n", i + 1, buffer[i]);
    }

    fclose(file);
    return 0;
}
```

### Contenido Esperado en `datos.bin`

El archivo `datos.bin` contendrá datos binarios, que no son visibles en un editor de texto. Si lo abrieras con un editor hexagonal, verías los valores binarios de `{1, 2, 3, 4, 5}` en formato binario.

### Salida Esperada en la Terminal

```
Elemento 1: 1
Elemento 2: 2
Elemento 3: 3
Elemento 4: 4
Elemento 5: 5
```

Este programa escribe un arreglo de enteros en un archivo binario, luego lo lee y muestra cada elemento en la terminal.
