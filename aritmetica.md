**2. Aritmética**
-------------

Las soluciones las puedes encontrar [aqui](https://github.com/vml86/prologp/blob/master/solari.md)

----------


**2.01 (* *) Determine si un número entero dado es primo.**

> **Ejemplo:**

    ? - is_prime (7).
    Sí

**2.02 (* *) Determine los factores primos de un entero positivo dado.**
Construya una lista plana que contenga los factores primos en orden ascendente.

> **Ejemplo:**

    \ Alpha - prime_factors (315, L).
    L = [3,3,5,7]

**2.03 ( * *) Determine los factores primos de un entero positivo dado (2).**
Construya una lista que contenga los factores primos y su multiplicidad.

> **Ejemplo:**

    ? - prime_factors_mult (315, L).
    L = [[3,2], [5,1], [7,1]]

**Pista:** La solución del problema 1.10 puede ser útil.

**2.04 (*) Una lista de números primos.**
Dado un rango de enteros por su límite inferior y superior, construya una lista de todos los números primos en ese rango.

**2.05 ( * *) Conjetura de Goldbach.**
La conjetura de Goldbach dice que cada número positivo igual mayor que 2 es la suma de dos números primos. Ejemplo: 28 = 5 + 23. Es uno de los hechos más famosos en la teoría numérica que no ha sido demostrado ser correcto en el caso general. Se ha confirmado numéricamente hasta números muy grandes (mucho más grandes de lo que podemos ir con nuestro sistema Prolog). Escribe un predicado para encontrar los dos números primos que suman un entero par.

> **Ejemplo:**

    \ Alpha - goldbach (28, L).
    L = [5,23]

**2.06 ( * *) Una lista de las composiciones de Goldbach.**
Dado un rango de números enteros por su límite inferior y superior, imprima una lista de todos los números pares y su composición de Goldbach.

> **Ejemplo:**

    ? - goldbach_list (9, 20).
    10 = 3 + 7
    12 = 5 + 7
    14 = 3 + 11
    16 = 3 + 13
    18 = 5 + 13
    20 = 3 + 17

En la mayoría de los casos, si un número par se escribe como la suma de dos números primos, uno de ellos es muy pequeño. Muy raramente, los primos son más grandes que dicen 50. Intente descubrir cuántos tales casos hay en el rango 2..3000.

**Ejemplo:** (para un límite de impresión de 50):

    ? - goldbach_list (1,2000,50).
    992 = 73 + 919
    1382 = 61 + 1321
    1856 = 67 + 1789
    1928 = 61 + 1867

**2.07 ( * *) Determine el máximo divisor común de dos números enteros positivos.**
Utilice el algoritmo de Euclides.

> **Ejemplo:**

    \ Alpha - gcd (36, 63, G).
    G = 9

    Definir gcd como una función aritmética; Así que puedes usarlo así:
    \ Alpha - G es gcd (36,63).
    G = 9

**2.08 (*) Determine si dos números enteros positivos son coprime.**
Dos números son coprime si su mayor divisor común es igual a 1.

> **Ejemplo:**

    \ Alpha - coprime (35, 64).
    Sí

**2.09 (* *) Calcule la función totent de Euler phi (m).**

La función llamada totient phi (m) de Euler se define como el número de enteros positivos r (1 <= r < m) que son coprime a m.

> **Ejemplo:**

    m = 10: r = 1,3,7,9; Así phi (m) = 4. Observe el caso especial: phi (1) = 1.
    ? - Phi es totient_phi (10).
    Phi = 4
Averigüe cuál es el valor de phi (m) si m es un número primo. La función totient de Euler juega un papel importante en uno de los métodos de criptografía de clave pública más utilizados (RSA). En este ejercicio debe utilizar el método más primitivo para calcular esta función. Hay una manera más inteligente que usaremos en 2.10.

 **2.10 (* *) Calcule la función totent de Euler phi (m) (2).**

Véase el problema 2.09 para la definición de la función totient de Euler. Si la lista de los factores primos de un número m es conocida en la forma del problema 2.03 entonces la función phi (m) puede calcularse eficientemente de la siguiente manera: [p1, m1], [p2, m2], [p3, M3], ...] la lista de factores primos (y sus multiplicidades) de un número dado m. Entonces phi (m) se puede calcular con la siguiente fórmula:

    (M3 - 1) * p2 * * (m2 - 1) * (p3 - 1) * p3 * * (m3 - 1) * p1 ** (m1 - 1) * p2 ...
   

 Observe que a ** b representa la b'th potencia de a.

**2.11 (*) Comparar los dos métodos de cálculo de la función totémica de Euler.**
Utilice las soluciones de los problemas 2.09 y 2.10 para comparar los algoritmos. Tomemos el número de inferencias lógicas como una medida de eficiencia. Trate de calcular phi (10090) como un ejemplo.


----------



_ _ _ _ _


