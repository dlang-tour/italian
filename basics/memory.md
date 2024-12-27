# Memoria

Il D è un linguaggio di sistema che permette di manipolare direttamente la memoria. Poiché la gestione manuale della memoria è soggetta a errori, D utilizza per default un *garbage collector* che ne automatizza l'allocazione e la deallocazione.

Come in C, D fornisce il supporto ai puntatori:

    int a;
    int* b = &a; // b contiene l'indirizzo di a
    auto c = &a; // c è di tipo int* e contiene l'indirizzo di a

Per allocare un nuovo blocco di memoria nell'heap si utilizza l'operatore `new`, che restituisce un puntatore alla memoria allocata:

    int* a = new int;

Quando la memoria puntata da `a` non è più referenziata da nessuna variabile nel programma, il garbage collector si occuperà automaticamente di liberarla.

D implementa tre livelli di sicurezza per le funzioni: `@system`, `@trusted`, e `@safe`.
- `@system` è il livello predefinito
- `@safe` è un sottoinsieme di D progettato per prevenire errori di gestione della memoria
- `@trusted` sono funzioni verificate manualmente che fungono da ponte tra il codice safe e le operazioni di basso livello

Il codice marcato come `@safe` può chiamare solo altro codice `@safe` o funzioni `@trusted`.
Inoltre, in codice `@safe` non è permessa l'aritmetica dei puntatori esplicita:

    void main() @safe {
        int a = 5;
        int* p = &a;
        int* c = p + 5; // errore: l'aritmetica dei puntatori non è permessa in codice @safe
    }

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
