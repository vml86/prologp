

**Soluciones Aritméticas**
----------


----------


    
**% 2.01 ( * * ) Determina si un número entero dado es primo.**

% Is_prime (P): - P es un número primo
% (Entero) (+)

    Is_prime (2).
    Is_prime (3).
    Is_prime (P): - entero (P), P> 3, P mod 2 = \ = 0, \ + tiene_factor (P, 3).

% Has_factor (N, L): - N tiene un factor impar F> = L.
% (Entero, entero) (+, +)

    Tiene_factor (N, L): - N mod L =: = 0.
    Tiene_factor (N, L): - L * L <N, L2 es L + 2, tiene_factor (N, L2).

**% 2.02 ( * * ) Determine los factores primos de un entero positivo dado.**

% Prime_factors (N, L): - N es la lista de factores primos de N.
% (Entero, lista) (+,?)
factores primos (N, L): - N> 0, factores primos (N, L, 2).
% Prime_factors (N, L, K): - L es la lista de factores primos de N.Es
% Conocido que N no tiene factores primos menores que K.

    Factores_principales (1, [], _): -.
    (N, [F | L], F): -% N es múltiplo de F   R es N // F, N =: = R * F,!, Primofactores (R, L, F).
    Factores primos (N, L, F): - Factor siguiente (N, F, NF), factores primos (N, L, NF).

% N no es múltiplo de F
   
**% 2.03 ( * * ) Determine los factores primos de un entero positivo dado (2).**
% Construya una lista que contenga los factores primos y su multiplicidad. 

**% Ejemplo:**

%? - prime_factors_mult (315, L).
% L = [[3,2], [5,1], [7,1]]: - ensure_loaded (p2_02).
% Asegúrese de que next_factor / 3 esté cargado
% Prime_factors_mult (N, L): - L es la lista de factores primos de N. Esta

% compuesto por términos [F, M] donde F es un factor primo y M su multiplicidad.
% (Entero, lista) (+,?)

    Prime_factors_mult (N, L): - N> 0, prime_factors_mult (N, L, 2).

% Prime_factors_mult (N, L, K): - L es la lista de factores primos de N.It es
% Conocido que N no tiene factores primos menores que K.

    Prime_factors_mult (1, [], _): -.
    (N, F, M, R),!,,,,,,,,,,,,,,,,,,% F divide N
    Siguiente_factor (R, F, NF), prime_factors_mult (R, L, NF).
    Prime_factors_mult (N, L, F): -!,

% F no divide N
 

    Siguiente_factor (N, F, NF), prime_factors_mult (N, L, NF).

% De división (N, F, M, R): - N = R * F ** M, M> = 1, y F no es un factor de R.
% (Entero, entero, entero, entero) (+, +, -, -)

    Dividir (N, F, M, R): - divi (N, F, M, R, 0), M> 0.
    Divi (N, F, M, R, K): - S es N // F, N =: = S * F,

% F divide N

    K1 es K + 1, divi (S, F, M, R, K1).
    Divi (N, _, M, N, M).

% Next_factor (N, F, NF): - al calcular los factores primos de N
% Y si F no divide N entonces NF es el siguiente candidato más grande a
% Sea un factor de N.

    Next_factor (_, 2,3): -.Factor siguiente (N, F, NF): - F * F <N,!, NF es F + 2.
    Siguiente_factor (N, _, N). % F> sqrt (N)

**% 2.04 (*) Una lista de números primos.**

% Dado un rango de enteros por su límite inferior y superior, construye una
% Lista de todos los números primos en ese rango.

    : - ensure_loaded (p2_01). % Asegúrese de que is_prime / 1 esté cargado

% Prime_list (A, B, L): - L es la lista del número primo P con A <= P <= B

    Lista de primos (A, B, L): - A = <2,!, P_list (2, B, L).
    Lista de primos (A, B, L): - A1 es (A // 2) * 2 + 1, p_list (A1, B, L).
    P_list (A, B, []): - A> B,!.
    P_list (A, B, [A | L]): - is_prime (A),!,
    Siguiente (A, A1), p_list (A1, B, L).
    P_list (A, B, L): -
    Siguiente (A, A1), p_list (A1, B, L).
    Siguiente (2,3): -.
    Siguiente (A, A1): - A1 es A + 2.

**% 2.05 ( * * ) Conjetura de Goldbach.**

La conjetura de Goldbach dice que cada número positivo mayor
% De 2 es la suma de dos números primos. Ejemplo: 28 = 5 + 23.

    : - ensure_loaded (p2_01).

% Goldbach (N, L): - L es la lista de los dos números primos que
% Suma hasta el N dado (que debe ser par).
% (Entero, entero) (+, -)

    Goldbach (4, [2,2]): -.
    Goldbach (N, L): - N mod 2 =: = 0, N> 4, goldbach (N, L, 3).
    
    Goldbach (N, [P, Q], P): - Q es N - P, is_prime (Q),.
    Goldbach (N, L, P): - P <N, next_prime (P, P1), goldbach (N, L, P1).
    
    Next_prime (P, P1): - P1 es P + 2, is_prime (P1),!.
    Next_prime (P, P1): - P2 es P + 2, next_prime (P2, P1).

**% 2.06 (*) Una lista de composiciones de Goldbach.**
% Dado un rango de enteros por su límite inferior y superior,
% Imprimir una lista de todos los números pares y su composición Goldbach.

    : - secure_loaded (p2_05).

% Goldbach_list (A, B): - imprimir una lista de la composición de Goldbach
% De todos los números pares N en el intervalo A <= N <= B
% (Entero, entero) (+, +)

    Goldbach_list (A, B): - goldbach_list (A, B, 2).

% Goldbach_list (A, B, L): - realiza goldbach_list (A, B), pero suprime
% Toda la salida cuando el primer número primo es menor que el límite L.

    Goldbach_list (A, B, L): - A = <4,!, G_list (4, B, L).
    Goldbach_list (A, B, L): - A1 es ((A + 1) // 2) * 2, g_list (A1, B, L).

    G_list (A, B, _): - A> B,!.
    G_list (A, B, L): -    Goldbach (A, [P, Q]),
    Print_goldbach (A, P, Q, L),    A2 es A + 2,    G_list (A2, B, L).
    
    Print_goldbach (A, P, Q, L): - P> = L,!,
    Writef ('% t =% t +% t', [A, P, Q]), nl.
    Print_goldbach (_, _, _, _).

**% 2.07 ( * * ) Determine el divisor común más grande de dos enteros positivos.**

% Gcd (X, Y, G): - G es el mayor divisor común de X e Y
% (Entero, entero, entero) (+, +,?)

    Gcd (X, O, X): - X> 0.
    Gcd (X, Y, G): - Y> 0, Z es X mod Y, gcd (Y, Z, G).

% Declare gcd como una función aritmética; Para que puedas usarlo
% Como esto:? - G es gcd (36,63).

    : - arithmetic_function (gcd / 2).

**% 2.08 (*) Determine si dos números enteros positivos son coprimos.**
% Dos números son coprimos si su máximo divisor común es igual a 1.

% Coprime (X, Y): - X e Y son coprime.
% (Entero, entero) (+, +)

    : - ensure_loaded (p2_07).
    Coprima (X, Y): - gcd (X, Y, 1).

**% 2.09 ( * * ) Calcule la función de totita de Euler phi (m).**

La función llamada totient phi (m) de Euler se define como el número
% De enteros positivos r (1 < = r < m) que son coprime a m.
% Ejemplo: m = 10: r = 1,3,7,9; Así phi (m) = 4. Nota: phi (1) = 1.

% Totient_phi (M, Phi): - Phi es el valor de la función totient de Euler
% Phi para el argumento M.
% (Entero, entero) (+, -)

    : - ensure_loaded (p2_08).
    : - arithmetic_function (totient_phi / 1).
    
    Totient_phi (1,1): -.
    Totient_phi (M, Phi): - t_phi (M, Phi, 1,0).

% T_phi (M, Phi, K, C): - Phi = C + N, donde N es el número de enteros R
% Tal que K < = R < M y R es coprime a M.
% (Entero, entero, entero, entero) (+, -, +, +)

    T_phi (M, Phi, M, Phi): -.
    T_phi (M, Phi, K, C): -
    K <M, coprime (K, M),!,
    {1} es C + 1, K _ {1} es K + 1,
    T_phi (M, Phi, K1, C1).
    T_phi (M, Phi, K, C): -
    K <M, K _ {1} es K + 1,
    T_phi (M, Phi, K1, C).

**% 2.10 ( * * ) Calcule la función totent de Euler phi( m )(2).**

% Véase el problema 2.09 para la definición de la función totient de Euler.
% Si la lista de los factores primos de un número m es conocida en la
% Forma de problema 2.03 entonces la función phi (m) puede ser eficientemente
% Calculado como sigue:

% Sea [p1, m1], [p2, m2], [p3, m3], ...] la lista de factores primos (y su
% Multiplicidades) de un número dado m. Entonces phi (m) se puede calcular
% Con la siguiente fórmula:
%(P1 - 1) * p1 * * ( m1 - 1) * (p2 - 1) * p2 ** (m2 - 1) *
% (P3 - 1) * p3 ** (m3 - 1) * ...
% Observe que a ** b representa la b'th potencia de a.

    : - ensure_loaded (p2_03). % Asegúrese de cargar prime_factors_mult

% Totient_phi_2 (N, Phi): - Phi es el valor de la función totient de Euler
% Para el argumento N.
% (Entero, entero) (+,?)

    Totient_phi_2 (N, Phi): - prime_factors_mult (N, L), to_phi (L, Phi).

    To_phi ([], 1).
    To_phi ([[F, 1] | L], Phi): -!
    To_phi (L, Phi1), Phi es Phi1 * (F - 1).
    To_phi ([[F, M] | L], Phi): - M> 1,
    M1 es M - 1, to_phi ([[F, M1] | L], Phi1), Phi es Phi1 * F.

**% 2.11 (*) Comparar los dos métodos de cálculo de la función totémica de Euler.**

% Utilice las soluciones de los problemas 2.09 y 2.10 para comparar los algoritmos.
% Tome el número de inferencias lógicas como una medida de eficiencia.

    : - ensure_loaded (p2_09).
    : - ensure_loaded (p2_10).

     Totient_test (N): -
     Escribe ('totient_phi (p2_09):'),
     Tiempo (totient_phi (N, Phi1)),
     Escribir ('resultado ='), escribir (Phi1), nl,
     Escribe ('totient_phi_2 (p2_10):'),
     Tiempo (totient_phi_2 (N, Phi2)),
     Escribe ('resultado ='), escribe (Phi2), nl.




