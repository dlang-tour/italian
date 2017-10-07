# Memoria

Il D è un linguaggio di sistema, e questo ci permette di utilizzare direttamente la
memoria. Tuttavia, manipolare la memoria è un'attività soggetta ad errori e
per questo il D utilizza di default un *garbage collector* che gestisce l'allocazione della
memoria.

D fornisce tipi puntatori come nel C:

    int a;
    int* b = &a; // b contiene l'indirizzo di a
    auto c = &a; // c è int* e contiene l'indirizzo di a

Un nuovo blocco di memoria nell'heap viene allocato utilizzando l'espressione
`new`, che ritorna un puntatore alla memoria gestita:

    int* a = new int;

Come la memoria che viene referenziata da `a` non viene più referenziata da altre
variabili nel programma, il garbage collector libererà la sua memoria.

D prevede tre livelli di sicurezza per le funzioni: `@system`, `@trusted`, e `@safe`.
Se non diversamente specificato, il default è `@system`.
`@safe` è un sottoinsieme di D pensato per prevenire errori di memoria.
Codice `@safe` può chiamare solo altro codice `@safe` o funzioni `@trusted`.
Inoltre, l'uso esplicito dell'aritmetica dei puntatori non è permessa in codice `@safe`:

    void main() @safe {
        int a = 5;
        int* p = &a;
        int* c = p + 5; // errore
    }

Le funzioni `@trusted` sono funzioni verificate manualmente che fungono da ponte tra SafeD e il sottostante mondo del basso livello.

### Approfondimenti

* [SafeD](https://dlang.org/safed.html)

## {SourceCode}

```d
import std.stdio : writeln;

void safeFun() @safe
{
    writeln("Ciao Mondo");
    // allocare memoria con il GC è sicuro
    int* p = new int;
}

void unsafeFun()
{
    int* p = new int;
    int* fiddling = p + 5;
}

void main()
{
    safeFun();
    unsafeFun();
}
```
