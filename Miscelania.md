**7.Miscelania**
-------------
Las soluciones las puedes encontrar aquí

----------
**7.01 ( * * ) Problemas de ocho reinas**

Este es un problema clásico en informática. El objetivo es colocar ocho reinas en un tablero de ajedrez para que no hay dos reinas que se atacan entre sí; Es decir, no hay dos reinas en la misma fila, la misma columna o en la misma diagonal.

Sugerencia: Representar las posiciones de las reinas como una lista de números 1..N. Ejemplo: [4,2,7,3,6,8,5,1] significa que la reina en la primera columna está en la fila 4, la reina en la segunda columna está en la fila 2, etc. - paradigma de prueba.

**7.02 ( * * ) Tour de los Caballeros**
Otro problema famoso es este: ¿Cómo puede un caballero saltar sobre un tablero de ajedrez NxN de tal manera que visite cada cuadrado exactamente una vez?

Pista: Representar los cuadrados por pares de sus coordenadas de la forma X / Y, donde tanto X como Y son números enteros entre 1 y N. (Obsérvese que '/' es simplemente un functor conveniente, no una división!) Defina la relación jump ( N, X / Y, U / V) para expresar el hecho de que un caballero puede saltar de X / Y a U / V en un tablero de ajedrez NxN. Y finalmente, representan la solución de nuestro problema como una lista de N * N posiciones de caballero (el viaje del caballero).

**7.03 ( * * * ) La conjetura de Von Koch**

Hace varios años conocí a un matemático que estaba intrigado por un problema para el que no conocía una solución. Su nombre era Von Koch, y no sé si el problema se ha resuelto desde entonces.
De todos modos, el rompecabezas es como este: Dado un árbol con N nodos (y por lo tanto N-1 bordes). Encuentre una manera de enumerar los nodos de 1 a N y, en consecuencia, los bordes de 1 a N-1 de tal manera que para cada borde K la diferencia de sus números de nodo sea igual a K. La conjetura es que esto es siempre posible.

Para los árboles pequeños el problema es fácil de resolver a mano. Sin embargo, para los árboles más grandes, y 14 ya es muy grande, es extremadamente difícil encontrar una solución. Y recuerde, no sabemos con certeza si siempre hay una solución!

![enter image description here](https://lh3.googleusercontent.com/-9eDI7DZOKLg/WMn56rx4jyI/AAAAAAAAAEY/Or1BwKYTEggS4GsDt_H9wFRvBvWzFl-9QCLcB/s0/p92a.gif "p92a.gif") 

![enter image description here](https://lh3.googleusercontent.com/-Mpkz9Drv7AQ/WMn5_3aEXiI/AAAAAAAAAEg/-DofQpdgSaMRErrjreMCjsio2CXxYVj4ACLcB/s0/p92b.gif "p92b.gif")

Escribe un predicado que calcula un esquema de numeración para un árbol dado. ¿Cuál es la solución para el árbol más grande representado arriba?

**7.04 ( * * * ) Un rompecabezas aritmético**

Dada una lista de números enteros, encuentre una manera correcta de insertar signos aritméticos (operadores) de manera que el resultado sea una ecuación correcta. Ejemplo: Con la lista de úmeros [2, 3, 5, 7, 11] podemos formar las ecuaciones 2-3 + 5 + 7 = 11 o 2 = (3 * 5 + 7) / 11 (y otras diez) .

**7.05 ( * * ) Palabras numéricas en inglés**

En los documentos financieros, como los cheques, los números a veces deben ser escritos en palabras completas. Ejemplo: 175 debe escribirse como uno-siete-cinco. Escribe un predicado full_words / 1 para imprimir números enteros (no negativos) en palabras completas.

**7.06( * * ) Verificador de sintaxis**

![enter image description here](https://lh3.googleusercontent.com/-JxOMImE74bk/WMn698Egb4I/AAAAAAAAAEw/aSZ_k91tM_YyEYVCzvri6oeI7zvSI9vJwCLcB/s0/p96.gif "p96.gif")

En un cierto lenguaje de programación (Ada) los identificadores se definen por el diagrama de sintaxis (gráfico de ferrocarril) opuesto. Transformar el diagrama de sintaxis en un sistema de diagramas de sintaxis que no contengan bucles; Es decir, que son puramente recursivos. Utilizando estos diagramas modificados, escriba un identificador de predicado / 1 que puede comprobar si una cadena dada es o no un identificador legal.

% Identificador (Str): - Str es un identificador legal

**7.07 ( * * ) Sudoku**

El rompecabezas Sudoku va así:

![enter image description here](https://lh3.googleusercontent.com/-ziPJjcZ3aEg/WMn780jI06I/AAAAAAAAAFI/PtNLqdIFlgcw6LQfktfagc_p1_0d8U1bACLcB/s0/1.png "1.png")


Cada punto del rompecabezas pertenece a una fila (horizontal) y una columna (vertical), así como a un solo cuadrado de 3x3 (que llamamos "cuadrado" para abreviar). Al principio, algunos de los puntos llevan un número de un solo dígito entre 1 y 9. El problema es llenar los puntos faltantes con dígitos de tal manera que cada número entre 1 y 9 aparece exactamente una vez en cada fila, en cada columna , Y en cada cuadrado.

**7.08 ( * * * ) Nonograms**

Alrededor de 1994, un cierto tipo de rompecabezas era muy popular en Inglaterra. El periódico "Sunday Telegraph" escribió: "Los nonogramas son rompecabezas de Japón y actualmente se publican cada semana sólo en The Sunday Telegraph. Simplemente utilice su lógica y habilidad para completar la cuadrícula y revelar una imagen o diagrama". Como programador de Prolog, usted está en una situación mejor: ¡usted puede hacer que su computadora haga el trabajo!

El rompecabezas va asi: Esencialmente, cada fila y columna de un mapa de bits rectangular se anota con las longitudes respectivas de sus cadenas distintas de celdas ocupadas. La persona que resuelve el rompecabezas debe completar el mapa de bits dado sólo estas longitudes.

![enter image description here](https://lh3.googleusercontent.com/-DtfA2iSPQMI/WMn87dvNs6I/AAAAAAAAAFk/LdR6iSuLRiwmtJS7YMUaUVwhynHbjhHiwCLcB/s0/2.png "2.png")

Para el ejemplo anterior, el problema puede ser declarado como las dos listas [[3], [2,1], [3,2], [2,2], [6], [1,5], [6] , [1], [2]] y [[1,2], [3,1], [1,5], [7,1], [5], [3], [4], [3] ] Que dan las longitudes "sólidas" de las filas y columnas, de arriba hacia abajo y de izquierda a derecha, respectivamente. Los puzzles publicados son mayores que este ejemplo, p. 25 x 20, y al parecer siempre tienen soluciones únicas.

**7.09 ( * * * ) Crucigrama**

Dado un marco vacío (o casi vacío) de un crucigrama y un conjunto de palabras. El problema es colocar las palabras en el marco.

![enter image description here](https://lh3.googleusercontent.com/-QutWyJoRhzc/WM3eq2LQLjI/AAAAAAAAAGQ/e5zgQS_ynugXg6P_tsnZZXge6TTmMJE9wCLcB/s0/rompe.gif "rompe.gif")

El crucigrama particular se especifica en un archivo de texto que primero enumera las palabras (una palabra por línea) en un orden arbitrario. Luego, después de una línea vacía, se define el marco de crucigramas. En esta especificación de marco, una ubicación de carácter vacía está representada por un punto (.). Para facilitar la solución, las ubicaciones de caracteres también pueden contener valores de carácter predefinidos. El rompecabezas contrario se define en el archivo p7_09a.dat, otros ejemplos son p7_09b.dat y p7_09d.dat. También hay un ejemplo de un rompecabezas (p7_09c.dat) que no tiene una solución.

Las palabras son cadenas (listas de caracteres) de al menos dos caracteres. Una secuencia horizontal o vertical de lugares de carácter en el marco de crucigramas se denomina sitio. Nuestro problema es encontrar una forma compatible de colocar palabras en sitios.

Sugerencias:
1) El problema no es fácil. Necesitará algún tiempo para entenderlo a fondo. Por lo tanto, no te rindas demasiado pronto! Y recuerde que el objetivo es una solución limpia, no sólo un hack rápido y sucio!
(2) La lectura del archivo de datos es un problema complicado para el que se proporciona una solución en el archivo p7_09-readfile.pl. Utilice el predicado read_lines / 2.
(3) Por razones de eficiencia, es importante, al menos para rompecabezas mayores, clasificar las palabras y los sitios en un orden particular. Para esta parte del problema, la solución de 1,28 puede ser muy útil.
