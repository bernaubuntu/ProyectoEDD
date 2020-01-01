# Trabajos de automatización

Trabajo de clase de EDD sobre la automatización con make

> Este trabajo se divide en tres partes:
> 1. Realizar un programa en `C` que muestre "`hola mundo`" y compilarlo con `gcc` y probar como se hace con `make`.
> 2. Realizar un programa en `C` que `sume, reste, multiplique, divida y diga cual de los dos números es mayor` en el que se usarán funciones en el mismo archivo y habría que compilarlo para comprobar que funcione.
> 3. Realizar el mismo programa que en el apartado pero separándolo en tres archivos, uno con las funciones, otro con las cabeceras de las funciones y el otro el programa principal. También habría que añadir un archivo `Makefile` con los objetivos `calcula, calc.o, clean, dist, targz e install`.

## Primera parte

Contenido del archivo `hola.c`

```C
#include <stdio.h>

int main(){
    printf("Hola mundo\n");
    return 0;
}
```

Orden para compilar el archivo `hola.c`.

```bash
    gcc -Wall -g hola.c -o hola.exe
```
Compilación con `make` sin tener un archivo `Makefile`. Asume por defecto que quieres compilar el archivo

```
    make hola
```

Captura de comprobación:

![Imagen de comprobación hola](hola.jpg)

#### Captura commit parte1

![Imagen captura commit parte1](make-parte1.jpg)

## Segunda parte

Contenido del archivo `calc.c`

> Contiene las funciones suma, resta, multiplica, divideix, major y main

```C
#include <stdio.h>

int suma(int op1, int op2){
    return (op1+op2);
}

int resta(int op1, int op2){
    return (op1-op2);
}

int multiplica(int op1, int op2){
    return (op1*op2);
}

int divideix(int op1, int op2){
    return (op1/op2);
}

int major(int op1, int op2){
    if (op1>op2)
    {
        return op1;
    }else
    {
        return op2;
    }
}//major

int main(){
    int a=10;
    int b=5;
    printf("La suma de %d i %d és %d\n", a, b, suma(a,b));
    printf("La resta entre %d i %d és %d\n", a, b, resta(a,b));
    printf("La multiplicació de %d i %d és %d\n", a, b, multiplica(a,b));
    printf("La divisió entre %d i %d és %d\n", a, b, divideix(a,b));
    printf("El major entre %d i %d és %d\n", a, b, major(a,b));
}
```
Orden para compilar el archivo `calc.c`.

```bash
    gcc -Wall -g calc.c -o calc.exe
```
Captura de comprobación:

![Imagen de comprobación parte2](parte2.jpg)

#### Captura commit parte2

![Imagen captura commit parte2](make-parte2.jpg)

## Tercera parte

Contenido del archivo `calc.c` que se ha modificado para esta parte.

> Contiene las funciones suma, resta, multiplica, divideix, major

```C
//#include <stdio.h>

int suma(int op1, int op2){
    return (op1+op2);
}

int resta(int op1, int op2){
    return (op1-op2);
}

int multiplica(int op1, int op2){
    return (op1*op2);
}

int divideix(int op1, int op2){
    return (op1/op2);
}

int major(int op1, int op2){
    if (op1>op2)
    {
        return op1;
    }else
    {
        return op2;
    }
}//major
```

Contenido del archivo `calc.h`

> Contiene las cabeceras de las funciones suma, resta, multiplica, divideix, major

```C
#ifndef MYCALC
#define MYCALC

int suma(int op1, int op2);
int resta(int op1, int op2);
int multiplica(int op1, int op2);
int divideix(int op1, int op2);
int major(int op1, int op2);

#endif
```

Contenido del archivo `calcula.c`

> Contiene la función main y una inclusión al archivo `calc.h`

```C
#include <stdio.h>
#include "calc.h"

int main(){
    int a=10;
    int b=5;
    printf("La suma de %d i %d és %d\n", a, b, suma(a,b));
    printf("La resta entre %d i %d és %d\n", a, b, resta(a,b));
    printf("La multiplicació de %d i %d és %d\n", a, b, multiplica(a,b));
    printf("La divisió entre %d i %d és %d\n", a, b, divideix(a,b));
    printf("El major entre %d i %d és %d\n", a, b, major(a,b));
}
```

Orden para compilar el archivo `calc.c`.

> Primero compila el archivo `calc.c` para crear el archivo objeto `calc.o`
> Después compila el archivo `calcula.c` incluyendo el archivo objeto `calc.o`, necesario para crear el ejecutable.

```bash
    gcc -Wall -g -c calc.c -o calc.o
    gcc -Wall -g calcula.c calc.o -o calcula.exe
```

Contenido del archivo `Makefile`

> Contiene los targets calcula, calc.o, clean, cleanall, dist, targz e install

```M
CC=gcc
CFLAGS=-Wall -g

calcula: calcula.c calc.o
	$(CC) $(CFLAGS) calcula.c calc.o -o calcula.exe

calc.o: calc.c calc.h
	$(CC) $(CFLAGS) -c calc.c -o calc.o

.PHONY: clean
clean:
	-rm -rf *.o
	-rm calcula.exe

.PHONY: cleanall
cleanall: clean
	-rm -rf calcula.tar.gz
    -rm -rf ../dist;

.PHONY: dist
dist: clean calcula
	-rm -rf ../dist;
	-mkdir -p ../dist/usr/bin/calc
	cp calcula.exe ../dist/usr/bin/calc

.PHONY: targz
targz: clean
	mkdir -p source
	cp *.c *.h Makefile source
#	tar -cvf calcula.tar source
#	gzip calcula.tar
	tar -czvf calcula.tar.gz source
	rm -rf calcula.tar
	rm -rf source

.PHONY: install
install: dist
	sudo cp -r ../dist/* /
```

Captura de comprobación:

![Imagen de comprobación parte3](parte3.jpg)

Capturas de comprobación con el archivo `Makefile`

![Imagen de comprobación parte4](parte4.jpg)

![Imagen de comprobación parte5](parte5.jpg)


#### Captura commit parte3

![Imagen captura commit parte3](make-parte3.jpg)

## Subir los datos a github

Para subir los datos a github solo faltaría el siguiente comando:

```
    git push -u origin master
```
