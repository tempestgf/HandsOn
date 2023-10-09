# Pokemon

Un pokemon el podem entendre com una estructura de dades que conté diferents camps. En aquest cas, els camps que ens interessen són:

* **pokemon_id**: identificador únic del pokemon
* **name**: nom del pokemon
* **height**: altura del pokemon
* **weight**: pes del pokemon

Per poder implementar aquesta estructura de dades en C, necessitem definir un tipus de dades que ens permeti agrupar aquests camps. Això ho podem fer mitjançant la paraula reservada **struct**.

```c    
struct pokemon {
    int          pokemon_id;
    char[50]     name;
    double       height;
    double       weight;
};
```

Podem fer un programa molt senzill per crear un pokemon i mostrar-lo per pantalla.

```c
/*
 * main.c
 */

#include <stdio.h>
#include <stdlib.h>
#include <string.h> //strcpy

struct pokemon {
    int          pokemon_id;
    char         name[50];
    double       height;
    double       weight;
};

int main() {
    struct pokemon pikachu;
    pikachu.pokemon_id = 25;
    strcpy(pikachu.name, "Pikachu");
    pikachu.height = 0.4;
    pikachu.weight = 6.0;

    printf("Pokemon: %s\n", pikachu.name);
    printf("Pokemon ID: %d\n", pikachu.pokemon_id);
    printf("Pokemon Height: %f\n", pikachu.height);
    printf("Pokemon Weight: %f\n", pikachu.weight);

    return 0;
}
```

Si compilem i executem el programa, funcionarà i obtindrem el resultat esperat:

```sh
gcc -o main main.c
```

```sh
./pokemon 
    Pokemon: Pikachu
    Pokemon ID: 25
    Pokemon Height: 0.400000
    Pokemon Weight: 6.000000
```

En aquesta primera versió hem utilitzat una mida estàtica pel camp **name** utilizant la *stack*. Això vol dir que el nom del pokemon no pot ser més gran de 50 caràcters. També, indica que estem desaprofitant memòria en tots els noms de pokemons inferiors a 50 caràcters. Recordeu que la mèmoria és un recurs molt valuós i que hem d'aprofitar al màxim.

Per tant, per poder solucionar aquest problema, podem utilitzar la *heap* per reservar memòria dinàmicament per al camp **name**. Això ens permetrà utilitzar la memòria de forma més eficient i no tindrem cap limitació en la mida del nom del pokemon. D'aquesta manera podem garantir que cada nom ocupi l'espai que requereixi.


```c    
struct pokemon {
    int          pokemon_id;
    char         *name;
    double       height;
    double       weight;
};
```


Per tant el nostre programa quedaria de la següent manera, on podem veure com reservem memòria per al camp **name** mitjançant la funció **malloc** i alliberem la memòria reservada mitjançant la funció **free**:

```c
/*  
 * main.c
 */

#include <stdio.h>
#include <stdlib.h>
#include <string.h> //strcpy()

struct pokemon {
    int          pokemon_id;
    char *       name;
    double       height;
    double       weight;
};

int main() {
    struct pokemon pikachu;
    pikachu.pokemon_id = 25;
    pikachu.height = 0.4;
    pikachu.weight = 6.0;

    // Reservem memòria per al camp name
    pikachu.name = malloc(8 * sizeof(char));
    strcpy(pikachu.name, "Pikachu");
    
    printf("Pokemon: %s\n", pikachu.name);
    printf("Pokemon ID: %d\n", pikachu.pokemon_id);
    printf("Pokemon Height: %f\n", pikachu.height);
    printf("Pokemon Weight: %f\n", pikachu.weight);

    // Alliberem la memòria reservada per al camp name
    free(pikachu.name);

    return 0;
}
```

**OBSERVACIÓ**: Tot i això, es aconsellable definir un llindar màxim per evitar problemes. *En aquest exemple, no afegirem aquest llindar, però en un cas real caldria avaluar si es necessari o no i els problemes de afegir o no afegir aquest llindar.*

**NOTA 1**: Quan reserveu memòria per una cadena de caràcters recordeu de reservar 1 byte més per el caràcter de final de cadena **'\0'**.

**NOTA 2**: Si feu anar **strlen** per calcular la mida de la cadena, us retorna la mida en bytes sense comptar el caràcter de final de cadena **'\0'**. 

```c
pikachu.name = malloc( (strlen("Pikachu")+1) * sizeof(char) );
```

Ara anem analitzar els següents supòsits:

```
char name[] = "Pikachu";
pikachu.name = name;
pikachu.name = &name;
pikachu.name = strdup(name);
strcpy(pikachu.name, name);
```

Us deixo les signatures de les funcions que es troben a la llibreria *string.h*:
    
```c
char *strdup(const char *s);
char *strcpy(char *dest, const char *src);
```

* ```pikachu.name = name;```: Aquesta assignació és vàlida ja que *name* és un array de caràcters i, en aquest context, es comporta com un punter al seu primer element (és equivalent a *&name[0]*), que és el que espera pikachu.name. Però si modifiquem la variable name en un altre punt del programa, pikachu.name també canviarà, ja que apunta a la mateixa memòria.

```c
char name[] = "Pikachu";
pikachu.name = name;
printf("Pokemon: %s\n", pikachu.name); // Pikachu
strcpy(name,"Raichu");
printf("Pokemon: %s\n", pikachu.name); // Raichu
```

* ```pikachu.name = &name;```: **&name** és l'adreça de l'array *name*, i **pikachu.name** és un punter a char, així que aquesta assignació no és vàlida, ja que l'adreça de name no és compatible amb un punter a char.

* ```pikachu.name = strdup(name);```: Aquesta assignació és vàlida ja que **strdup** retorna un punter a char, i això és el que espera **pikachu.name**. A més, com que **strdup** reserva memòria nova per a la cadena, no hi ha cap problema si modifiquem la variable name en un altre punt del programa. Es pot fer servir sense reserva prèvia de memòria per a pikachu.name, ja que **strdup** reserva memòria nova per a la cadena i retorna un punter a aquesta memòria.

* ```strcpy(pikachu.name, name);```: Això és vàlid si **pikachu.name** ja té memòria reservada prèviament (per exemple, a través de *malloc* o *calloc*) en la qual es pot realitzar la còpia.

**RECORDATORI**: *Malloc* i *Calloc* ens permeten reservar memòria dinàmicament. La diferència entre malloc i calloc és que malloc no inicialitza la memòria reservada, mentre que calloc inicialitza la memòria reservada a 0. 

## Ús de typedef

Ara podem utilitzar **typedef** per definir un nou tipus de dades que ens permeti crear pokemons de forma més senzilla.

```c
/*
 * main.c
*/
#include <stdio.h>
#include <stdlib.h>
#include <string.h> //strdup(),

typedef struct pokemon {
    int          pokemon_id;
    char *       name;
    double       height;
    double       weight;
} Pokemon;

int main() {
    Pokemon pikachu;
    pikachu.pokemon_id = 25;

    pikachu.name = strdup("Pikachu");
    pikachu.height = 0.4;
    pikachu.weight = 6.0;

    printf("Pokemon: %s\n", pikachu.name);
    printf("Pokemon ID: %d\n", pikachu.pokemon_id);
    printf("Pokemon Height: %f\n", pikachu.height);
    printf("Pokemon Weight: %f\n", pikachu.weight);

    return 0;
}
```

**OBSERVACIÓ:** Si utilitzem la funció *strdup*, no cal reservar ni alliberar memòria per al camp *name*.

## Creació i ús de llibreries

Ara podem crear una llibreria que ens permeti fer operacions amb pokemons. Per exemple, podem crear un pokemon amb la funció **create_pokemon** i mostrar-lo per pantalla amb la funció **print_pokemon**. Per fer-ho, crearem un fitxer anomenat **pokemon.h** on definirem les funcions i un fitxer anomenat **pokemon.c** on implementarem les funcions.

En el fitxer **pokemon.h** descriurem la interfície de la nostra llibreria. Això vol dir que definirem les funcions que volem que estiguin disponibles per a l'usuari de la llibreria. També farem accessible la nostra estructura de dades **Pokemon**.

```c
/*
 * pokemon.h
 */

#ifndef _POKEMON_H_
#define _POKEMON_H_

typedef struct pokemon {
    int          pokemon_id;
    char *       name;
    double       height;
    double       weight;
} Pokemon;

Pokemon create_pokemon(int pokemon_id, char *name, double height, double weight);
void print_pokemon(Pokemon pokemon);

#endif // _POKEMON_H_
```

En el fitxer **pokemon.c** implementarem les funcions que hem definit a la interfície de la nostra llibreria.

```c
/*
 * pokemon.c
 */

#include <stdio.h>
#include <stdlib.h>
#include <string.h> //strlen(), strcpy()
#include "pokemon.h"

Pokemon create_pokemon(int pokemon_id, char *name, double height, double weight) {
    Pokemon pokemon;
    pokemon.pokemon_id = pokemon_id;
    pokemon.name = malloc( (strlen(name) +1) * sizeof(char));
    strcpy(pokemon.name, name);
    pokemon.height = height;
    pokemon.weight = weight;

    return pokemon;
}

void print_pokemon(Pokemon pokemon) {
    printf("Pokemon: %s\n", pokemon.name);
    printf("Pokemon ID: %d\n", pokemon.pokemon_id);
    printf("Pokemon Height: %f\n", pokemon.height);
    printf("Pokemon Weight: %f\n", pokemon.weight);
}
```

Ara podem utilitzar la nostra llibreria:

```c
/*
 * main.c
 */

#include <stdio.h>
#include "pokemon.h"

int main() {
    Pokemon pikachu = create_pokemon(25, "Pikachu", 0.4, 6.0);
    print_pokemon(pikachu);
    return 0;
}
```

Si compilem i executem:

```sh
 gcc -o pokemon pokemon.c main.c
```

```sh
 ./pokemon 
```

Obtindrem el següent resultat, on semblaria que tot funciona correctament:

```sh
Pokemon: Pikachu
Pokemon ID: 25
Pokemon Height: 0.400000
Pokemon Weight: 6.000000
```

**NOTEU**: En aquesta implementació, no estem alliberant la memòria reservada per al camp **name**. Això vol dir que estem creant una fuita de memòria. Podem utiltizar les funcions del compilador -fsanitize=undefined -fsanitize=address -fsanitize=leak per detectar errors de memòria. 

```sh
gcc -c main.c
gcc -c pokemon.c
gcc -fsanitize=undefined -fsanitize=address -fsanitize=leak -o main main.o pokemon.o
```

```sh
./main
```

Observareu un resultat com el que mostro a continuació:

```shell
Pokemon: Pikachu
Pokemon ID: 25
Pokemon Height: 0.400000
Pokemon Weight: 6.000000

=================================================================
==23251==ERROR: LeakSanitizer: detected memory leaks

Direct leak of 8 byte(s) in 1 object(s) allocated from:
    #0 0x7f0060eb89cf in __interceptor_malloc 
        ../../../../src/libsanitizer/asan/asan_malloc_linux.cpp:69
    #1 0x55a52044921c in create_pokemon 
        /home/jordi/handson00-JordiMateoUdL/Setmana2/pokemon/pokemon.c:9
    #2 0x55a5204491ab in main 
        /home/jordi/handson00-JordiMateoUdL/Setmana2/pokemon/main.c:7
    #3 0x7f00604461c9 in __libc_start_call_main 
        ../sysdeps/nptl/libc_start_call_main.h:58

SUMMARY: AddressSanitizer: 8 byte(s) leaked in 1 allocation(s).
```

Fixeu-vos que ens indica que hi ha una fuita de memòria de 8 bytes. Això és degut a que no estem alliberant la memòria reservada per al camp **name**. Per solucionar aquest problema, crearem una funció anomenada **destroy_pokemon** que alliberi la memòria reservada per al camp **name**.  

**Observació**: El programa anterior era funcional ja que al acabar el procés el Sistema Operatiu allibera tota la memòria reservada pel procés. 

**Compte**: Si aquesta variable estigues en una funció i continues el programa podriam tenir problemes de memòria. 

**Recomanació**: Sigueu curosos amb la memòria i allibereu-la sempre que sigui necessari.

```c
void destroy_pokemon(Pokemon pokemon) {
    free(pokemon.name);
}
```

Ara podem utilitzar la nostra llibreria:

```c
#include <stdio.h>
#include "pokemon.h"

int main() {
    Pokemon pikachu = create_pokemon(25, "Pikachu", 0.4, 6.0);
    print_pokemon(pikachu);
    destroy_pokemon(pikachu);
    return 0;
}
```

També podem modificar ```pokemon.name = malloc( (strlen(name) ) * sizeof(char));``` per veure com el compilador ens indica que hi ha un error de memòria. 

```sh
=================================================================
==23501==ERROR: AddressSanitizer: heap-buffer-overflow on address 0x602000000017 
at pc 0x7f43fb2602ca bp 0x7ffe3f3a04b0 sp 0x7ffe3f39fc60
WRITE of size 8 at 0x602000000017 thread T0
    #0 0x7f43fb2602c9 in __interceptor_strcpy 
        ../../../../src/libsanitizer/asan/asan_interceptors.cpp:425
    #1 0x55b09b84526e in create_pokemon 
        (/home/jordi/handson00-JordiMateoUdL/Setmana2/pokemon/main+0x126e)
    #2 0x55b09b8451bb in main 
        (/home/jordi/handson00-JordiMateoUdL/Setmana2/pokemon/main+0x11bb)
    #3 0x7f43fa8461c9 in __libc_start_call_main 
        ../sysdeps/nptl/libc_start_call_main.h:58
    #4 0x7f43fa846284 in __libc_start_main_impl ../csu/libc-start.c:360
    #5 0x55b09b8450b0 in _start 
        (/home/jordi/handson00-JordiMateoUdL/Setmana2/pokemon/main+0x10b0)

0x602000000017 is located 0 bytes to the right of 7-byte 
region [0x602000000010,0x602000000017)
allocated by thread T0 here:
    #0 0x7f43fb2b89cf in __interceptor_malloc 
        ../../../../src/libsanitizer/asan/asan_malloc_linux.cpp:69
    #1 0x55b09b845257 in create_pokemon 
        (/home/jordi/handson00-JordiMateoUdL/Setmana2/pokemon/main+0x1257)
    #2 0x55b09b8451bb in main 
        (/home/jordi/handson00-JordiMateoUdL/Setmana2/pokemon/main+0x11bb)
    #3 0x7f43fa8461c9 in __libc_start_call_main 
        ../sysdeps/nptl/libc_start_call_main.h:58

SUMMARY: AddressSanitizer: 
    heap-buffer-overflow ../../../../src/libsanitizer/asan/asan_interceptors.cpp:425 
    in __interceptor_strcpy
Shadow bytes around the buggy address:
  0x0c047fff7fb0: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
  0x0c047fff7fc0: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
  0x0c047fff7fd0: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
  0x0c047fff7fe0: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
  0x0c047fff7ff0: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
=>0x0c047fff8000: fa fa[07]fa fa fa fa fa fa fa fa fa fa fa fa fa
  0x0c047fff8010: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
  0x0c047fff8020: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
  0x0c047fff8030: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
  0x0c047fff8040: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
  0x0c047fff8050: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
Shadow byte legend (one shadow byte represents 8 application bytes):
  Addressable:           00
  Partially addressable: 01 02 03 04 05 06 07 
  Heap left redzone:       fa
  Freed heap region:       fd
  Stack left redzone:      f1
  Stack mid redzone:       f2
  Stack right redzone:     f3
  Stack after return:      f5
  Stack use after scope:   f8
  Global redzone:          f9
  Global init order:       f6
  Poisoned by user:        f7
  Container overflow:      fc
  Array cookie:            ac
  Intra object redzone:    bb
  ASan internal:           fe
  Left alloca redzone:     ca
  Right alloca redzone:    cb
==23501==ABORTING
```

Bàsicament ens esta indicant que estem escrivint més enllà de la memòria reservada.

### Visibilitat de les llibreries

En aquest punt observarem la visibilitat de les nostres llibreries. Si intentem accedir a la nostra estructura de dades **Pokemon** des de la funció **main** com la estructura pokemon esta definida al fitxer *pokemon.h* tots els atributs seran visibles a main. Per exemple:

```c
/*
 * main.c
 */

#include <stdio.h>
#include "pokemon.h"

int main() {
    Pokemon pikachu = create_pokemon(25, "Pikachu", 0.4, 6.0);
    pikachu.pokemon_id = 26;
    print_pokemon(pikachu);
    destroy_pokemon(pikachu);
    return 0;
}
```

En canvi, si movem la definició de la estructura **Pokemon** al fitxer *pokemon.c* i la declarem al fitxer *pokemon.h* com a una estructura anònima, els atributs de la estructura **Pokemon** no seran visibles a la funció **main**. Per exemple:

```c
/*
 * pokemon.h
 */

#ifndef _POKEMON_H_
#define _POKEMON_H_

typedef struct pokemon Pokemon;

Pokemon create_pokemon(int pokemon_id, char *name, double height, double weight);
void print_pokemon(Pokemon pokemon);
void destroy_pokemon(Pokemon pokemon);

#endif // _POKEMON_H_
```

```c
/*
 * pokemon.c
 */
 
#include <stdio.h>
#include <stdlib.h>
#include <string.h> //strlen()
#include "pokemon.h"

struct pokemon {
    int          pokemon_id;
    char         *name;
    double       height;
    double       weight;
};

Pokemon create_pokemon(int pokemon_id, char *name, double height, double weight) {
    Pokemon pokemon;
    pokemon.pokemon_id = pokemon_id;
    pokemon.name = malloc( (strlen(name) + 1 )* sizeof(char));
    strcpy(pokemon.name, name);
    pokemon.height = height;
    pokemon.weight = weight;

    return pokemon;
}

void print_pokemon(Pokemon pokemon) {
    printf("Pokemon: %s\n", pokemon.name);
    printf("Pokemon ID: %d\n", pokemon.pokemon_id);
    printf("Pokemon Height: %f\n", pokemon.height);
    printf("Pokemon Weight: %f\n", pokemon.weight);
}

void destroy_pokemon(Pokemon pokemon) {
    free(pokemon.name);
}
```

Ara el programa no compilarà degut a que no podem accedir als atributs de la estructura **Pokemon** des de la funció **main**. Fixeu-vos que aquesta manera de fer simularia un objecte en java on els atributs són privats i només es poden accedir a través de mètodes públics. Per tant, per poder accedir als atributs de la estructura **Pokemon** des de la funció **main** necessitem crear mètodes públics que ens permetin accedir als atributs de la estructura **Pokemon**. Per exemple, podem afegir a **pokemon.c**, un *getter* in un *setter* del camp **pokemon_id**:

```c
int get_pokemon_id(Pokemon pokemon) {
    return pokemon.pokemon_id;
}

void set_pokemon_id(Pokemon pokemon, int pokemon_id) {
    pokemon.pokemon_id = pokemon_id;
}
```

**SOLUCIÓ 1**: El setter no funciona ja que la funció **set_pokemon_id** rep una còpia de la variable **pokemon**. Per tant, quan modifiquem el camp **pokemon_id** de la variable **pokemon** dins de la funció **set_pokemon_id**, en realitat estem modificant una còpia de la variable **pokemon**. Per solucionar aquest problema, necessitem passar un punter a la variable **pokemon**. Per tant, la funció **set_pokemon_id** quedaria de la següent manera:

```c
void set_pokemon_id(Pokemon *pokemon, int pokemon_id) {
    pokemon->pokemon_id = pokemon_id;
}
```

**SOLUCIÓ 2**: No podem declarar la variable ```Pokemon pikachu``` ja que els atributs de la estructura **Pokemon** no són visibles a la funció **main**. Quina memòria ha de reservar si no sap la mida de la estructura **Pokemon**? Per solucionar aquest problema, podem utilitzar un punter a la estructura **Pokemon**. Per tant, la funció **main** quedaria de la següent manera:

```c
/*
* main.c
*/

#include <stdio.h>
#include "pokemon.h"

int main() {
    Pokemon *pikachu;
    pikachu = create_pokemon(25, "Pikachu", 0.4, 6.0);
    set_pokemon_id(pikachu, 26);
    print_pokemon(pikachu);
    destroy_pokemon(pikachu);
    return 0;
}
```

```c
/*
* pokemon.h
*/

#ifndef _POKEMON_H_
#define _POKEMON_H_

typedef struct pokemon Pokemon;

Pokemon* create_pokemon(int pokemon_id, char *name, double height, double weight);
void print_pokemon(Pokemon* pokemon);
void destroy_pokemon(Pokemon* pokemon);
void set_pokemon_id(Pokemon *pokemon, int pokemon_id);

#endif // _POKEMON_H_
```

```c
/*
* pokemon.c
*/
#include <stdio.h>
#include <stdlib.h>
#include <string.h> //strlen(), strcpy()
#include "pokemon.h"

struct pokemon {
    int          pokemon_id;
    char         *name;
    double       height;
    double       weight;
};

Pokemon* create_pokemon(int pokemon_id, char *name, double height, double weight) {
    Pokemon* pokemon = malloc(sizeof(Pokemon));
    pokemon->pokemon_id = pokemon_id;
    pokemon->name = malloc( (strlen(name) + 1) * sizeof(char) );
    strcpy(pokemon->name, name);
    pokemon->height = height;
    pokemon->weight = weight;

    return pokemon;
}

void print_pokemon(Pokemon* pokemon) {
    printf("Pokemon: %s\n", pokemon->name);
    printf("Pokemon ID: %d\n", pokemon->pokemon_id);
    printf("Pokemon Height: %f\n", pokemon->height);
    printf("Pokemon Weight: %f\n", pokemon->weight);
}

void destroy_pokemon(Pokemon* pokemon) {
    free(pokemon->name);
    free(pokemon);
}

void set_pokemon_id(Pokemon *pokemon, int pokemon_id) {
    pokemon->pokemon_id = pokemon_id;
}
```

Ara modificarem la funció *print_pokemon* per permetre a l'usuari seleccionar a quin descriptor de fitxer vol mostrar la informació. Tenim 2 alternatives. Podem utilitzar **fprintf** o **write**. 

Anem a analitzar les signatures de les funcions:

```c
int fprintf(FILE *stream, const char *format, ...);
ssize_t write(int fd, const void *buf, size_t count);
```

Per utilitzar **fprintf** necessitem un *apuntador a un fitxer* i per utilitzar **write** necessitem un *descriptor de fitxer*. 

```c
void print_pokemon(Pokemon pokemon, FILE *stream) {
    if (stream == NULL) {
        stream = stdout;
    }
    fprintf(stream, "Pokemon: %s\n", pokemon.name);
    fprintf(stream, "Pokemon ID: %d\n", pokemon.pokemon_id);
    fprintf(stream, "Pokemon Height: %f\n", pokemon.height);
    fprintf(stream, "Pokemon Weight: %f\n", pokemon.weight);
}
```

Podem cridar a la funció **print_pokemon** de la següent manera:

```c
print_pokemon(pikachu, NULL);  // Mostrarà per pantalla (normal)
print_pokemon(pikachu, stdout); // Mostrarà per pantalla (normal)
print_pokemon(pikachu, stderr); // Mostrarà per pantalla (errors)
File *file = fopen("pikachu.txt", "w"); // Escriura un fitxer
print_pokemon(pikachu, file);
```

El seu programa equivalent utilitzant **write** seria el següent:

```c
void print_pokemon(Pokemon pokemon, int fd) {

    ssize_t bytes_written = 0;
    if (fd == NULL || fd <= 0) {
        fd = STDOUT_FILENO;
    }
    char *buffer = malloc(100 * sizeof(char));
    sprintf(buffer, "Pokemon: %s\n", pokemon.name);
    bytes_written = write(fd, buffer, strlen(buffer));

    sprintf(buffer, "Pokemon ID: %d\n", pokemon.pokemon_id);
    bytes_written = write(fd, buffer, strlen(buffer));

    sprintf(buffer, "Pokemon Height: %f\n", pokemon.height);
    bytes_written = write(fd, buffer, strlen(buffer));

    sprintf(buffer, "Pokemon Weight: %f\n", pokemon.weight);
    bytes_written = write(fd, buffer, strlen(buffer));

    free(buffer);
}
```

La funció **sprintf** ens permet escriure en un buffer de caràcters. 

**NOTA**:  En cas d'error, retornarà -1. El tipus *ssize_t* és un enter signat de 64 bits es possible utilitzar el tipus *int*, però per tenir compatibilitat amb sistemes de 32 bits, és millor utilitzar el tipus *ssize_t*.

Podem cridar a la funció **print_pokemon** de la següent manera:

```c
print_pokemon(pikachu, NULL);  // Mostrarà per pantalla (normal)
print_pokemon(pikachu, -1); // Mostrarà per pantalla (normal)
print_pokemon(pikachu, STDOUT_FILENO); // Mostrarà per pantalla (normal)
print_pokemon(pikachu, STDERR_FILENO); // Mostrarà per pantalla (errors)
File *file = open("pikachu.txt", O_WRONLY | O_CREAT, 0644); // Escriura un fitxer
print_pokemon(pikachu, file);
```

## Test de la llibreria

Es recomanable sempre que programem llibreries incloure un fitxer de test on puguem provar les funcionalitats de la llibreria. En aquest cas, crearem un fitxer anomenat **test.c** on provarem les funcionalitats. I ens permetrà en un futur fer modificacions a la llibreria sense por de trencar la funcionalitat. En aquest punt (**s'assumeix que tenim la struct i el typedef al punt h i són visibles i no cal treballar amb la modalitat en punters**).


```c
/*
 * test.c
 */

#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include "pokemon.h"

void test_crear_pokemon() {
    Pokemon pikachu = create_pokemon(25, "Pikachu", 0.4, 6.0);
    if (pikachu.pokemon_id != 25 || strcmp(pikachu.name, "Pikachu") != 0 ||
        pikachu.height != 0.4 || pikachu.weight != 6.0) {
        printf("ERROR: test_crear_pokemon NO SUPERAT. Dades incorrectes.\n");
    } else {
        printf("Test de test_crear_pokemon passat amb èxit.\n");
    }
    destroy_pokemon(pikachu);
}

int main() {
    test_crear_pokemon();
    return 0;
}
```

## Automatització de la compilació

Ara podem automatizar-ho amb un makefile:

```makefile
CC = gcc
CFLAGS = -Wall -Wextra -Werror -pedantic
LDFLAGS = -fsanitize=undefined -fsanitize=address -fsanitize=leak
EXECUTABLE = test
SOURCES = $(wildcard *.c)
OBJS = $(SOURCES:.c=.o)

all: $(EXECUTABLE) 

$(EXECUTABLE): $(OBJS)
	$(CC) $(CFLAGS) $(LDFLAGS) -o $(EXECUTABLE) $(OBJS)

.c.o:
	$(CC) $(CFLAGS) -c $<

execute: $(EXECUTABLE)
	./$(EXECUTABLE)

clean:
	rm -f $(TARGET) $(OBJS)

.PHONY: all clean
```

En aquest **Makefile** utilitzem:
* **-Wall**: Permet mostrar tots els warnings.
* **-Wextra**: Permet mostrar warnings addicionals a **-Wall**.
* **-Werror**: Permet tractar els warnings com a errors.
* **-pedantic**: Permet assegurar que el codi és compatible amb el estàndard ANSI C.

```sh
make execute
```
**NOTA**: Si conservem el *main.c* original al mateix directori *make* fallarà ja que tindrà dos entrades **int main()** una al fitxer *main.c* i l'altra al fitxer *test.c*. Com el nostre Makefile intenta enllaçar tots els fitxers per formar un únic executable, fallarà ja que no pot enllaçar dos funcions **int main()**. Per tant, elimineu el fitxer *main.c*.

## Reptes

1. Completeu la llibreria `pokemon.c` amb les funcions **getter** i **setter** per a cada un dels atributs de la classe `Pokemon`.
2. Afegiu una funció de test per assegurar que la funció print_pokemon(Pokemon pokemon, int fd) funciona de forma correcta.
3. Implementeu una funció que permeti inicialitzar un pokemon a partir d'un string amb el format: `pokemon_id name height weight`.
4. Afegiu una matriu per guardar un màxim de dos tipus de pokemon. La matriu tindrà la següent forma: `char *types[2]`. Actualtizeu totes les funcions implementades per treballar amb aquest nou atribut.


## Solució i Problemes generals

He creat un [repositori](https://github.com/OS-GEI-IGUALADA-2223/HandsOn01-pokemon) amb una solució possible dels exercicis. 

### Problemes generals a tenir en compte

1. **Neteja del projecte**: Assegureu-vos de fer un make clean abans de recompilar i tornar a testejar per garantir que tots els objectes es regenerin i no hi hagi falsos positius.

2. **Compilació i Makefile**: Compileu utilitzant el Makefile i totes les opcions proporcionades. Si apareixen errors o avisos, haureu de solucionar-los.

3. **Gestió de memòria**: No cal alliberar i reallocar memòria cada vegada que es fa el setter per al nom o els tipus. Només cal fer-ho una vegada. Podeu comprovar que el punter no sigui NULL abans de realitzar l'assignació.

4. **Control d'errors d'impressió**: Assegureu-vos que les operacions d'impressió no es realitzin amb punteres NULL per evitar errors.

5. **Alliberament de memòria**: Recorda que tota la memòria reservada amb malloc o calloc s'ha de alliberar utilitzant free.

6. **Optimització de codi**: Eviteu el copiar i enganxar excessiu de codi. Per exemple, en comptes de fer múltiples crides a malloc per type[0] i type[1], seria més eficient fer un bucle i definir el nombre de tipus com a una constant utilitzant #define.

7. **Testeu**: Intenteu executar el vostre codi amb diferents condicions per assegurar que funciona en diferents situacions.
