# Sesión 1: Presentación e historia de C++
**Primeros pasos en un lenguaje compilado — proyecto Sonora**

---

Todo el código que has escrito hasta ahora en Python lo ejecutó un intérprete,
línea por línea, en el momento en que lo corriste. Hoy vas a escribir tu
primer programa en C++, un lenguaje que funciona distinto. Antes de que
puedas correrlo, alguien tiene que traducirlo completo a instrucciones que
el computador entienda. Ese alguien es el **compilador**, y hoy lo vas a
conocer.

Por ahora todo tu código vive en un solo archivo, `main.cpp`. Hoy también
vas a escribir tus primeras funciones propias, pero todavía no las vas a
separar en archivos aparte, van a vivir dentro del mismo `main.cpp`,
definidas antes de `int main()`. Esa separación en archivos `.h` y `.cpp`
empieza en la Sesión 2, cuando `main.cpp` pase a ser el que orquesta y no
el que hace todo el trabajo.

## Tabla de contenido

1. [Enciende tu Codespace](#1-enciende-tu-codespace)
2. [Qué significa que C++ sea un lenguaje compilado](#2-qué-significa-que-c-sea-un-lenguaje-compilado)
3. [De dónde viene C++](#3-de-dónde-viene-c)
4. [Partes mínimas de un programa](#4-partes-mínimas-de-un-programa)
5. [Variables: materia prima de Sonora](#5-variables-la-materia-prima-de-sonora)
6. [Funciones y procedimientos](#6-funciones-y-procedimientos)

---

## 1. Enciende tu Codespace

Hasta ahora, para programar en Python seguramente abriste un notebook o un
editor en línea, escribiste código y lo corriste con un botón. El editor,
el intérprete y la consola ya venían resueltos por esa plataforma. Hoy vas
a necesitar tres piezas trabajando juntas y visibles al mismo tiempo, un
editor de texto, un compilador y una terminal donde escribir comandos. Un
programa que junta esas tres piezas en una sola ventana se llama **IDE**
(entorno de desarrollo integrado).

El IDE de este curso es **Visual Studio Code** (VS Code), uno de los más
usados para C++. Todavía no lo vas a instalar. Vas a abrirlo dentro de un
**Codespace**, una máquina que GitHub enciende para ti en la nube, con VS
Code ya corriendo en el navegador y el compilador ya instalado. Nada de
esto vive en tu computador hasta que decidas descargarlo.

Más adelante en el curso vas a instalar **Code::Blocks**, un IDE de
escritorio hecho específicamente para C++, en tu propio computador.
Empezar con la versión en línea te deja concentrarte en el lenguaje sin
pelear primero con una instalación. Los conceptos que aprendas de aquí en
adelante (compilar, variables, funciones) no cambian según el IDE que
uses después.

El repositorio del curso ya trae la configuración lista. No necesitas
instalar un compilador ni configurar nada en tu computador. Abre el
repositorio del curso y crea tu Codespace desde ahí. La primera vez tarda
un poco más porque construye el entorno completo; las siguientes veces
abre en segundos.

Cuando el Codespace termine de cargar vas a ver VS Code corriendo en el
navegador, con una terminal en la parte inferior. Ábrela y escribe:

```bash
g++ --version
```

La salida esperada es algo como:

```
g++ (Ubuntu 11.4.0-1ubuntu1~22.04) 11.4.0
Copyright (C) 2021 Free Software Foundation, Inc.
```

Esa línea confirma que el compilador está instalado y listo. No importa
el número exacto de versión; lo que importa es que el comando responda en
lugar de decir `g++: command not found`.

🔧 **Consejo de entorno:** si al abrir la terminal el `postCreateCommand`
todavía está corriendo, vas a ver texto de instalación pasando. Espera a
que termine y vuelva a aparecer el prompt antes de escribir comandos.

El repositorio ya incluye el archivo `src/sesion1/main.cpp`. Ábrelo desde
el explorador de archivos a la izquierda, pues vas a modificarlo durante
los ejercicios de hoy.

Compílalo desde la terminal:

```bash
g++ src/sesion1/main.cpp -o sonora
```

`-o sonora` le dice al compilador cómo debe llamarse el ejecutable que
produce. Si no lo indicas, el compilador igual genera un ejecutable, pero
con un nombre genérico. Ahora corre ese ejecutable:

```bash
./sonora
```

La salida esperada es:

```
Bienvenido a Sonora
```

⚠️ **Error frecuente:** escribir `sonora` sin el `./` al inicio produce
`bash: sonora: command not found`. Linux no busca ejecutables en la
carpeta actual a menos que se lo indiques explícitamente con `./`; en
Windows estás acostumbrado a que esto no haga falta, pero el Codespace
corre Linux por dentro.

---

## 2. Qué significa que C++ sea un lenguaje compilado

Cuando corres `python archivo.py`, el intérprete lee y ejecuta cada línea
en el momento, sin pasos previos. Cuando trabajas con C++, ese proceso se
parte en dos. Primero el compilador traduce todo el archivo a un
ejecutable, y solo después ese ejecutable corre, de forma independiente
del código fuente que lo produjo.

Piensa en el intérprete de Python como un traductor simultáneo en una
conferencia que traduce cada frase apenas la escucha. El compilador de C++
se parece más a un traductor de libros pues toma el texto completo, lo revisa
de principio a fin, y solo entrega la traducción cuando terminó. Si
encontró algo que no pudo traducir, te lo dice todo de una vez, antes de
que puedas leer una sola página.

La consecuencia es que en Python, un error de sintaxis en la línea 200
no te detiene hasta que el intérprete llega ahí — las líneas anteriores sí
alcanzan a ejecutarse. En C++, el compilador revisa el archivo completo
antes de generar el ejecutable. Un error de sintaxis en cualquier parte
impide que compiles, sin importar en qué línea esté ni qué tan lejos esté
de donde estás trabajando.

💡 **Por qué importa:** esto cambia cómo depuras. En Python puedes correr
el programa hasta donde llegue y ver qué alcanzó a hacer. En C++, si hay
un error de compilación, no se genera ningún ejecutable — no hay "correr
una parte". El mensaje del compilador es tu única pista en ese momento, y
aprender a leerlo es una de las habilidades centrales de este curso.

Esos mensajes se ven intimidantes al principio. Son largos, mencionan
nombres de archivo, números de línea, y términos que todavía no conoces.
Pero cada uno contiene exactamente qué salió mal y dónde. Fallar la
compilación no es un problema que hay que evitar a toda costa sino que es
información que el compilador te está dando antes de que el error llegue
más lejos. El compilador es el primer lector de tu
código, y el más estricto que vas a tener.

> **Pausa y piensa:** en Python, ¿qué pasa si hay un error de sintaxis en
> la última línea de un script de 200 líneas, pero nunca llegas a
> ejecutarla porque el programa termina antes? ¿Crees que en C++ pasaría
> lo mismo?

---

## 3. De dónde viene C++

Antes de que existiera C++, ya existía **C**. Dennis Ritchie lo creó a
inicios de los años setenta en los Laboratorios Bell, como un lenguaje de
propósito general lo bastante cercano al hardware para escribir sistemas
operativos completos. Con C, Ritchie y su equipo construyeron **Unix**, y
desde ahí C se volvió el lenguaje de referencia para todo lo que
necesitara hablar casi directamente con la máquina.

**Bjarne Stroustrup**, un ingeniero danés, tomó C como punto de partida y
empezó a desarrollar C++ en 1979, en esos mismos Laboratorios Bell, bajo
el nombre "C with Classes". Quería agregar a C una forma de organizar
código en unidades más grandes —lo que hoy conoces como clases— sin
perder el control de bajo nivel y el desempeño que ya hacían de C un
lenguaje valioso para sistemas. En 1983 el proyecto se renombró **C++**.
El `++` es el operador de incremento del propio C, un chiste interno de
quienes lo crearon para decir "una versión más de C".

El lenguaje se estandarizó formalmente por ISO en 1998, y desde entonces
ha tenido revisiones importantes cada pocos años. C++11 marcó un salto
grande hacia el C++ moderno que vas a aprender en este curso, y le
siguieron C++14, C++17, C++20 y C++23, cada una añadiendo herramientas
sin romper el código escrito con versiones anteriores.

Hoy C++ se usa donde el rendimiento y el control preciso de la memoria
importan, por ejemplo en motores de videojuegos, sistemas operativos,
aplicaciones de trading de alta frecuencia, sistemas embebidos en
hardware y aplicaciones de escritorio grandes. Esa combinación —control
cercano a la máquina con abstracciones de más alto nivel que C— es la
misma razón por la que Stroustrup lo creó hace más de cuarenta años.

Ya conoces algunas de las consecuencias de esa combinación, aunque no lo
sepas. Librerías de Python que usas seguido, como **NumPy** o **pandas**,
están implementadas por debajo en C o C++, justamente para ganar la
velocidad de ejecución que Python solo no ofrece. El motor de
**Minecraft: Bedrock Edition** (la versión para consolas, celulares y
Windows, distinta de la Java Edition original, que sí está en Java) está
escrito en C++. Y el motor central de **Roblox** también está escrito en
C++, aunque los scripts de los juegos que corren dentro de la plataforma
se programan en **Luau**, una variante de Lua, no en C++ directamente.

---

## 4. Partes mínimas de un programa

Este es el programa más simple que puedes escribir en C++:

```cpp
#include <iostream>

int main() {
    std::cout << "Bienvenido a Sonora\n";
    return 0;
}
```

`#include <iostream>` trae las herramientas para leer y escribir en la
consola —el equivalente funcional de un `import` en Python, aunque el
mecanismo por debajo es distinto—. Sin esa línea, `std::cout` no existe
para el compilador.

`int main()` es el punto de entrada. Cuando ejecutas el programa, la
ejecución empieza ahí, sin excepción. `std::cout` escribe en la consola;
el prefijo `std::` indica que `cout` viene del **espacio de nombres**
estándar de C++, una forma de agrupar herramientas para evitar que choquen
nombres de distintas librerías. El operador `<<` envía lo que está a su
derecha hacia `cout`; `"\n"` es un salto de línea, igual que en Python.

> **Pausa y piensa:** ¿qué imprime este programa? Escribe tu predicción
> antes de compilarlo.

```cpp
#include <iostream>

int main() {
    int reproducciones = 950;
    std::cout << reproducciones << " reproducciones de \"" << "La Gozadera" << "\"\n";
    return 0;
}
```

<details>
<summary>💡 Ver respuesta</summary>

La salida es:

```
950 reproducciones de "La Gozadera"
```

`<<` imprime cada valor en el orden exacto en que aparece en la línea, sin
importar si es una variable o un texto literal entre comillas.

</details>

`return 0;` le informa al sistema operativo que el programa terminó sin
errores. Python no te pide esto explícitamente porque lo maneja por
detrás; en C++ es parte del contrato de `main()`.

✅ **Buena práctica:** este curso escribe `std::cout` explícito en lugar de
agregar `using namespace std;` al inicio del archivo. Es una línea más
larga, pero deja claro de dónde viene cada herramienta que usas. Vas a ver
`using namespace std;` en código de otras personas; ahora sabes qué hace
esa línea y por qué aquí se evita.

El problema aparece cuando nombras algo igual a lo que ya existe en
`std`. Mira qué pasa si declaras una variable con el mismo nombre que la
herramienta que querías usar.

```cpp
#include <iostream>
using namespace std;

int main() {
    int cout = 5;
    cout << "Reproduciendo cancion" << endl;
    return 0;
}
```

El compilador marca

```
error: invalid operands of types 'int' and 'const char [22]' to
binary 'operator<<'
```

`using namespace std;` trajo `cout` al alcance sin el prefijo `std::`,
así que cuando declaraste tu propia variable `cout`, esa variable tapó a
la de la librería. El operador `<<` ya no tiene un flujo de salida a su
izquierda, tiene un entero, y el compilador no sabe qué hacer con eso.

```cpp
#include <iostream>

int main() {
    int cout = 5;
    std::cout << cout << "\n";
    return 0;
}
```

Con `std::cout` explícito, tu variable `cout` y la herramienta de la
librería conviven sin problema, porque nunca comparten el mismo nombre
completo.

Hay dos detalles más que Python no te exigía.

**Cada instrucción termina en punto y coma (`;`).** El punto y coma le
dice al compilador dónde acaba una instrucción y empieza la siguiente. La
indentación con espacios en C++ es solo para que las personas lean mejor
el código; al compilador no le importa cuántos espacios uses, pero el
punto y coma sí es obligatorio.

**Los bloques van entre llaves (`{ }`).** En Python, todo lo que pertenece
a una función o un `if` se identifica por la indentación. En C++ la
indentación ayuda a leer, pero lo que de verdad delimita un bloque son las
llaves.

⚠️ **Error frecuente:** olvidar un punto y coma produce un mensaje como
`expected ';' before 'return'`. Lo confuso es que el compilador casi
siempre señala la línea **siguiente** a la que realmente le falta el punto
y coma, no la línea del error. Si el mensaje apunta a una línea que se ve
perfecta, revisa la línea inmediatamente anterior.

---

## 5. Variables: la materia prima de Sonora

En Python, una variable puede cambiar de tipo en cualquier momento.
`x = 5` y después `x = "cinco"` son ambas válidas, y el intérprete no se
queja. En C++, cuando declaras una variable declaras también su tipo, y
ese tipo no cambia después.

```cpp
std::string titulo = "Ojos Verdes";
int duracion_segundos = 245;
double calificacion = 4.8;
bool disponible_offline = true;
```

`std::string` guarda texto y requiere `#include <string>` al inicio del
archivo. `int` guarda números enteros. `double` guarda números con
decimales. `bool` guarda solamente `true` o `false`, sin comillas.

Para imprimir varias cosas en una sola línea, encadenas `<<`:

```cpp
std::cout << "Título: " << titulo << " (" << duracion_segundos << "s)\n";
```

```
Título: Ojos Verdes (245s)
```

💡 **Por qué importa:** que el tipo no cambie no es una restricción
arbitraria. Le permite al compilador reservar exactamente la memoria que
cada variable necesita desde el momento en que la declaras, y detectar en
tiempo de compilación —antes de ejecutar nada— si intentas usarla de una
forma incompatible con su tipo.

> **Pausa y piensa:** ¿qué esperarías que pasara si escribes
> `int duracion_segundos = 245;` y en la línea siguiente intentas
> `duracion_segundos = "cuatro minutos";`? Escríbelo y compílalo antes de
> seguir leyendo.

Ese mismo problema aparece si intentas asignar el tipo equivocado desde el
principio, no solo al reasignar más adelante.

> **Pausa y piensa:** ¿cuál es el error en este código? Compílalo y
> confirma tu respuesta.

```cpp
#include <iostream>

int main() {
    int reproducciones = "950";
    std::cout << reproducciones << "\n";
    return 0;
}
```

<details>
<summary>💡 Ver respuesta</summary>

El compilador marca

```
error: invalid conversion from 'const char*' to 'int' [-fpermissive]
```

`"950"` es un texto (`const char*`), no un número. Aunque los caracteres
sean dígitos, C++ no convierte automáticamente un texto a `int`, así que
la asignación es incompatible con el tipo declarado de `reproducciones`.

</details>

⚠️ **Error frecuente:** dividir dos enteros en C++ trunca el resultado.
En Python, `7 / 2` da `3.5`. En C++, si ambos operandos son `int`,
`7 / 2` da `3`, sin decimales — no redondea, simplemente descarta la
parte fraccionaria. Si necesitas un resultado con decimales, al menos uno
de los dos operandos debe ser `double`.

```cpp
int total = 7;
int partes = 2;
std::cout << total / partes << "\n";        // → 3  (división entera)
std::cout << total / 2.0 << "\n";           // → 3.5  (uno es double)
```

Vas a necesitar este detalle en el ejercicio Avanzado de hoy.

---

## Ejercicios de variables

### Inducción al error

Abre `src/sesion1/main.cpp` y reemplaza su contenido con esto:

```cpp
#include <iostream>
#include <string>

int main() {
    std::string cancion = "Ojos Verdes"
    std::cout << "Reproduciendo: " << cancion << "\n";
    return 0;
}
```

Compílalo:

```bash
g++ src/sesion1/main.cpp -o sonora
```

¿Qué dice el error? ¿A qué línea apunta el compilador, y por qué no es
exactamente la línea donde falta el punto y coma?

### Básico

📂 `src/sesion1/main.cpp`

Declara tres variables para una canción de Sonora. `titulo` y `artista`
son `std::string`, y `duracion_segundos` es `int`. Imprime cada una en su
propia línea con `std::cout`.

La salida esperada es:

```
Título: Ojos Verdes
Artista: Los Ángeles Negros
Duración en segundos: 245
```

<details>
<summary>💡 Ver solución</summary>

```cpp
#include <iostream>
#include <string>

int main() {
    std::string titulo = "Ojos Verdes";
    std::string artista = "Los Ángeles Negros";
    int duracion_segundos = 245;

    std::cout << "Título: " << titulo << "\n";
    std::cout << "Artista: " << artista << "\n";
    std::cout << "Duración en segundos: " << duracion_segundos << "\n";
    return 0;
}
```

</details>

### Intermedio

📂 `src/sesion1/main.cpp`

Extiende el programa anterior calculando cuántos minutos y segundos
completos representa `duracion_segundos`, usando división entera (`/`)
para los minutos y el operador módulo (`%`) para los segundos restantes.
Imprime el resultado como `4:10`.

La salida esperada, con `duracion_segundos = 250`, es:

```
Duración: 4:10
```

<details>
<summary>💡 Ver solución</summary>

```cpp
#include <iostream>
#include <string>

int main() {
    int duracion_segundos = 250;
    int minutos = duracion_segundos / 60;
    int segundos = duracion_segundos % 60;

    std::cout << "Duración: " << minutos << ":" << segundos << "\n";
    return 0;
}
```

</details>

⚠️ Si `duracion_segundos` fuera, por ejemplo, `245`, el resultado sería
`4:5` en lugar de `4:05`, porque `std::cout` no agrega ceros a la
izquierda por su cuenta. Formatear con cero relleno requiere `<iomanip>`,
que vas a ver más adelante en el curso; por ahora, basta con que el
cálculo sea correcto.

### Avanzado

📂 `src/sesion1/main.cpp`

Declara dos canciones con sus reproducciones totales (`int`) y los días
que llevan publicadas (`int`). Calcula el promedio de reproducciones por
día de cada una como `double`, y muestra cuál de las dos tiene mejor
promedio diario — sin usar `if` todavía, solo calculando e imprimiendo
ambos promedios para que tú los compares.

```cpp
// Canción A
int reproducciones_a = 18500;
int dias_a = 12;

// Canción B
int reproducciones_b = 9800;
int dias_b = 4;
```

La salida esperada es:

```
Promedio diario A: 1541.67
Promedio diario B: 2450
```

⚠️ Si tu resultado da `1541` sin decimales, revisa qué tipo tienen los
operandos de la división y recuerda lo que viste en la sección 5.

<details>
<summary>💡 Ver solución</summary>

```cpp
#include <iostream>

int main() {
    int reproducciones_a = 18500;
    int dias_a = 12;

    int reproducciones_b = 9800;
    int dias_b = 4;

    double promedio_a = reproducciones_a / static_cast<double>(dias_a);
    double promedio_b = reproducciones_b / static_cast<double>(dias_b);

    std::cout << "Promedio diario A: " << promedio_a << "\n";
    std::cout << "Promedio diario B: " << promedio_b << "\n";
    return 0;
}
```

`static_cast<double>(dias_a)` convierte ese valor a `double` antes de la
división, para que el resultado conserve los decimales. Vas a ver
`static_cast` con más detalle en sesiones futuras; por ahora, es la forma
explícita de decirle al compilador "trata este entero como un decimal
para esta operación".

</details>

### Libre / Reto

Elige una canción real que te guste. Declara variables para su título,
artista, duración en segundos, género (`std::string`) y una calificación
tuya del 1 al 5 (`double`). Imprime una "ficha" de la canción para Sonora,
con el formato que prefieras. No hay solución de referencia, porque el
diseño de la ficha es tuyo.

---

## 6. Funciones y procedimientos

Ya calculaste, en el ejercicio Intermedio de la sección anterior, cuántos
minutos y segundos representa una duración en segundos. Si Sonora necesita
mostrar esa misma conversión en diez pantallas distintas de la aplicación,
copiar esas tres líneas diez veces funciona, hasta que cambias la fórmula
y tienes que encontrar y corregir las diez copias, una por una.

Piensa en una función como una fórmula a la que le pones nombre una sola
vez. Después, en vez de repetir el cálculo, solo la invocas por ese
nombre.

```cpp
#include <iostream>
#include <string>

std::string formatear_duracion(int duracion_segundos) {
    int minutos = duracion_segundos / 60;
    int segundos = duracion_segundos % 60;
    return std::to_string(minutos) + ":" + std::to_string(segundos);
}

int main() {
    std::cout << formatear_duracion(250) << "\n";
    std::cout << formatear_duracion(195) << "\n";
    return 0;
}
```

La salida esperada es:

```
4:10
3:15
```

`formatear_duracion` guarda el cálculo una sola vez. Si la fórmula cambia,
la corriges en un solo lugar y las dos llamadas la reciben actualizada.
Eso es una **función**.

Fíjate en `std::string` antes del nombre `formatear_duracion`. Ese tipo es
lo que la función entrega con `return`, y es lo que distingue a una
**función** de un **procedimiento**. Una función retorna un valor de un
tipo distinto de `void`, siempre con `return valor;`. Un procedimiento se
declara `void`, no retorna ningún valor, y si usa `return` va solo, sin
nada después, para salir antes de tiempo. Es el mismo criterio que ya
usaste en Python, donde una función sin `return` explícito devolvía
`None`; en C++ el compilador te obliga a declarar por adelantado cuál de
las dos estás escribiendo.

```cpp
#include <iostream>
#include <string>

std::string formatear_duracion(int duracion_segundos) {
    int minutos = duracion_segundos / 60;
    int segundos = duracion_segundos % 60;
    return std::to_string(minutos) + ":" + std::to_string(segundos);
}

void mostrar_ficha(std::string titulo, int duracion_segundos) {
    std::cout << "Título: " << titulo << "\n";
    std::cout << "Duración: " << formatear_duracion(duracion_segundos) << "\n";
}

int main() {
    mostrar_ficha("Ojos Verdes", 250);
    return 0;
}
```

La salida esperada es:

```
Título: Ojos Verdes
Duración: 4:10
```

`mostrar_ficha` no retorna nada, su trabajo es imprimir, y por eso su tipo
de retorno es `void`. La sintaxis general es siempre la misma, el tipo de
retorno primero, el nombre de la función, los parámetros entre paréntesis
y el cuerpo entre llaves.

> **Pausa y piensa:** ¿cuál es el error en este código? Compílalo y
> confirma tu respuesta.

```cpp
#include <iostream>
#include <string>

void mostrar_genero(std::string genero) {
    std::cout << "Género: " << genero << "\n";
}

int main() {
    std::cout << mostrar_genero("Vallenato") << "\n";
    return 0;
}
```

<details>
<summary>💡 Ver respuesta</summary>

El compilador marca

```
error: no match for 'operator<<' (operand types are 'std::ostream' and 'void')
```

`mostrar_genero` es un procedimiento, `void`, y no produce ningún valor
que `std::cout` pueda imprimir. Un procedimiento se llama por su efecto,
no por su resultado, así que se invoca en su propia línea, sin envolverlo
en un `std::cout <<`.

</details>

Todas las funciones y procedimientos de esta sesión van definidos antes de
`int main()`, nunca después. C++ lee el archivo de arriba hacia abajo una
sola vez, así que si `main()` llama algo que aparece más adelante en el
archivo, el compilador todavía no sabe que existe.

> **Pausa y piensa:** ¿cuál es el error en este código? Compílalo y
> confirma tu respuesta.

```cpp
#include <iostream>

int main() {
    std::cout << duplicar(21) << "\n";
    return 0;
}

int duplicar(int reproducciones) {
    return reproducciones * 2;
}
```

<details>
<summary>💡 Ver respuesta</summary>

El compilador marca

```
error: 'duplicar' was not declared in this scope
```

Cuando el compilador llega a la línea de `main()`, todavía no ha leído la
definición de `duplicar`, porque está más abajo en el archivo. Mover
`duplicar` antes de `main()` resuelve el error.

</details>

Sonora también necesita comparar qué tan bien le está yendo a una canción
frente a otra. Una función y un procedimiento pueden repartirse ese
trabajo, la función calcula y el procedimiento presenta el resultado.

```cpp
#include <iostream>
#include <string>

double calcular_promedio_diario(int reproducciones, int dias) {
    return reproducciones / static_cast<double>(dias);
}

void mostrar_promedio(std::string titulo, int reproducciones, int dias) {
    double promedio = calcular_promedio_diario(reproducciones, dias);
    std::cout << titulo << ": " << promedio << " reproducciones/día\n";
}

int main() {
    mostrar_promedio("Ojos Verdes", 18500, 12);
    mostrar_promedio("La Diabla", 9800, 4);
    return 0;
}
```

La salida esperada es:

```
Ojos Verdes: 1541.67 reproducciones/día
La Diabla: 2450 reproducciones/día
```

`calcular_promedio_diario` no imprime nada, solo calcula y retorna.
`mostrar_promedio` no calcula nada directamente, llama a la función y
presenta lo que recibió. Cada una hace una sola cosa.

A partir de ahora, cada vez que repitas el mismo cálculo o la misma
impresión en más de un lugar del código, es una señal de que necesitas
una función o un procedimiento nuevo.

---

## Ejercicios de funciones

### Inducción al error

Abre `main.cpp` y reemplaza su contenido con esto:

```cpp
#include <iostream>
#include <string>

mostrar_titulo(std::string titulo) {
    std::cout << "Título: " << titulo << "\n";
}

int main() {
    mostrar_titulo("Ojos Verdes");
    return 0;
}
```

Compílalo:

```bash
g++ main.cpp -o sonora
```

¿Qué dice el error? `mostrar_titulo` no imprime nada raro, solo le falta
una palabra antes del nombre, ¿cuál es, y qué le indica al compilador?

### Básico

📂 `main.cpp`

Declara una función `segundos_a_minutos` que reciba un `int` con la
duración en segundos y retorne un `double` con los minutos exactos
(`duracion_segundos / 60.0`). Llámala desde `main()` con
`duracion_cancion = 250` e imprime el resultado.

La salida esperada es:

```
Minutos: 4.16667
```

<details>
<summary>💡 Ver solución</summary>

```cpp
#include <iostream>

double segundos_a_minutos(int duracion_segundos) {
    return duracion_segundos / 60.0;
}

int main() {
    int duracion_cancion = 250;
    std::cout << "Minutos: " << segundos_a_minutos(duracion_cancion) << "\n";
    return 0;
}
```

</details>

### Intermedio

📂 `main.cpp`

Escribe la función `formatear_duracion` (retorna `std::string`, formato
`m:ss`) y el procedimiento `mostrar_duracion` (`void`, recibe el título y
la duración, y usa `formatear_duracion` para imprimir la línea completa).
Llama a `mostrar_duracion` para dos canciones distintas.

La salida esperada es:

```
Duración de Ojos Verdes: 4:10
Duración de La Diabla: 3:15
```

<details>
<summary>💡 Ver solución</summary>

```cpp
#include <iostream>
#include <string>

std::string formatear_duracion(int duracion_segundos) {
    int minutos = duracion_segundos / 60;
    int segundos = duracion_segundos % 60;
    return std::to_string(minutos) + ":" + std::to_string(segundos);
}

void mostrar_duracion(std::string titulo, int duracion_segundos) {
    std::cout << "Duración de " << titulo << ": "
               << formatear_duracion(duracion_segundos) << "\n";
}

int main() {
    mostrar_duracion("Ojos Verdes", 250);
    mostrar_duracion("La Diabla", 195);
    return 0;
}
```

</details>

### Avanzado

📂 `main.cpp`

Convierte el ejercicio Avanzado de la sección de variables en funciones.
Escribe `calcular_promedio_diario` (retorna `double`) y `mostrar_promedio`
(`void`, recibe un título, las reproducciones y los días, calcula con la
primera función y presenta el resultado). Llámala para las mismas dos
canciones de aquel ejercicio.

La salida esperada es:

```
Promedio diario A: 1541.67
Promedio diario B: 2450
```

<details>
<summary>💡 Ver solución</summary>

```cpp
#include <iostream>
#include <string>

double calcular_promedio_diario(int reproducciones, int dias) {
    return reproducciones / static_cast<double>(dias);
}

void mostrar_promedio(std::string titulo, int reproducciones, int dias) {
    double promedio = calcular_promedio_diario(reproducciones, dias);
    std::cout << "Promedio diario " << titulo << ": " << promedio << "\n";
}

int main() {
    mostrar_promedio("A", 18500, 12);
    mostrar_promedio("B", 9800, 4);
    return 0;
}
```

</details>

### Libre / Reto

Elige otra métrica de Sonora, por ejemplo el total de reproducciones de
una playlist completa o el porcentaje de la duración de un álbum que
ocupa una sola canción. Escribe tu propia pareja función más
procedimiento, una función que calcule el valor y un procedimiento que lo
imprima con el formato que prefieras. No hay solución de referencia,
porque tú decides qué calcular.

---

## Referencias

- **cppreference.com** — referencia técnica completa del lenguaje y su
  librería estándar: [https://en.cppreference.com/w/](https://en.cppreference.com/w/)
- **Bjarne Stroustrup — página personal**, con la historia del lenguaje
  contada por su creador: [https://www.stroustrup.com/](https://www.stroustrup.com/)
- **GitHub Codespaces — documentación oficial**:
  [https://docs.github.com/es/codespaces](https://docs.github.com/es/codespaces)
- **GCC / g++ — documentación de opciones del compilador**:
  [https://gcc.gnu.org/onlinedocs/](https://gcc.gnu.org/onlinedocs/)
