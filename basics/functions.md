# Funzioni

Una funzione è già stata introdotta: `main()` - il punto di partenza di ogni programma D. Una funzione può restituire un valore (o essere dichiarata con `void` se non restituisce nulla) e accettare un numero arbitrario di argomenti:

    int add(int lhs, int rhs) {
        return lhs + rhs;
    }

### Tipi di ritorno `auto`

Se il tipo di ritorno è definito come `auto`, il compilatore D deduce automaticamente il tipo di ritorno. Di conseguenza, più istruzioni `return` devono restituire valori con tipi compatibili.

    auto add(int lhs, int rhs) { // restituisce `int`
        return lhs + rhs;
    }

    auto lessOrEqual(int lhs, int rhs) { // restituisce `double`
        if (lhs <= rhs)
            return 0;
        else
            return 1.0;
    }

### Argomenti predefiniti

Le funzioni possono opzionalmente definire argomenti predefiniti.
Questo evita il noioso lavoro di dichiarare ridondanti
overload di funzioni.

    void plot(string msg, string color = "red") {
        ...
    }
    plot("D rocks");
    plot("D rocks", "blue");

Una volta specificato un argomento predefinito, tutti gli argomenti
successivi devono essere anch'essi predefiniti.

### Funzioni locali

Le funzioni possono essere dichiarate anche all'interno di altre funzioni, dove possono essere
utilizzate localmente e non sono visibili al mondo esterno.
Queste funzioni possono comunque accedere agli oggetti che sono locali allo
scope del genitore:

    void fun() {
        int local = 10;
        int fun_secret() {
            local++; // questo è legale
        }
        ...

Queste funzioni annidate sono chiamate delegati e verranno spiegate più approfonditamente
[a breve](basics/delegates).

### Approfondimenti

- [Funzioni in _Programming in D_](http://ddili.org/ders/d.en/functions.html)
- [Parametri delle funzioni in _Programming in D_](http://ddili.org/ders/d.en/function_parameters.html)
- [Specifiche delle funzioni](https://dlang.org/spec/function.html)

## {SourceCode}

```d
import std.stdio : writeln;
import std.random : uniform;

void randomCalculator()
{
    // Definisce 4 funzioni locali per
    // 4 diverse operazioni matematiche
    auto add(int lhs, int rhs) {
        return lhs + rhs;
    }
    auto sub(int lhs, int rhs) {
        return lhs - rhs;
    }
    auto mul(int lhs, int rhs) {
        return lhs * rhs;
    }
    auto div(int lhs, int rhs) {
        return lhs / rhs;
    }

    int a = 10;
    int b = 5;

    // uniform genera un numero tra START
    // e END, dove END NON è incluso.
    // A seconda del risultato chiamiamo una
    // delle operazioni matematiche.
    switch (uniform(0, 4)) {
        case 0:
            writeln(add(a, b));
            break;
        case 1:
            writeln(sub(a, b));
            break;
        case 2:
            writeln(mul(a, b));
            break;
        case 3:
            writeln(div(a, b));
            break;
        default:
            // codice speciale che marca
            // il codice come IRRAGGIUNGIBILE
            assert(0);
    }
}

void main()
{
    randomCalculator();
    // add(), sub(), mul() e div()
    // NON sono visibili fuori dal loro scope
    static assert(!__traits(compiles,
                            add(1, 2)));
}
```