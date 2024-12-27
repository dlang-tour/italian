# Range (Intervalli)

Quando il compilatore incontra un `foreach`

```d
foreach (element; range)
{
    // Corpo del ciclo...
}
```

viene internamente riscritto in modo simile a questo:

```d
for (auto __rangeCopy = range;
     !__rangeCopy.empty;
     __rangeCopy.popFront())
 {
    auto element = __rangeCopy.front;
    // Corpo del ciclo...
}
```

Qualsiasi oggetto che soddisfa la seguente interfaccia è chiamato **range**
(o più specificamente `InputRange`) ed è quindi un tipo su cui si può iterare:

```d
    interface InputRange(E)
    {
        bool empty();
        E front();
        void popFront();
    }
```

Dai un'occhiata all'esempio sulla destra per esaminare più da vicino
l'implementazione e l'utilizzo di un input range.

## Lazy Evaluation (Valutazione Pigra)

I range sono __pigri__. Non verranno esaminati fino a quando non richiesto.
Quindi, è possibile prendere un range da un range infinito:

```d
42.repeat.take(3).writeln; // [42, 42, 42]
```

## Tipi per Valore vs. Tipi per Riferimento

Se l'oggetto range è un tipo per valore, allora il range verrà copiato e solo la copia
sarà consumata:

```d
auto r = 5.iota;
r.drop(5).writeln; // []
r.writeln; // [0, 1, 2, 3, 4]
```

Se l'oggetto range è un tipo per riferimento (es. `class` o [`std.range.refRange`](https://dlang.org/phobos/std_range.html#refRange)),
allora il range verrà consumato e non sarà ripristinato:

```d
auto r = 5.iota;
auto r2 = refRange(&r);
r2.drop(5).writeln; // []
r2.writeln; // []
```

### Gli `InputRange` Copiabili sono `ForwardRange`

La maggior parte dei range nella libreria standard sono struct e quindi l'iterazione
`foreach` è solitamente non distruttiva, anche se non garantita. Se questa
garanzia è importante, si può usare una specializzazione di un `InputRange`—
i **ForwardRange** con un metodo `.save`:

```d
interface ForwardRange(E) : InputRange!E
{
    typeof(this) save();
}
```

```d
// per valore (Structs)
auto r = 5.iota;
auto r2 = refRange(&r);
r2.save.drop(5).writeln; // []
r2.writeln; // [0, 1, 2, 3, 4]
```

### I `ForwardRange` possono essere estesi a range bidirezionali + range ad accesso casuale

Ci sono due estensioni del `ForwardRange` copiabile: (1) un range bidirezionale
e (2) un range ad accesso casuale.
Un range bidirezionale permette l'iterazione dalla fine:

```d
interface BidirectionalRange(E) : ForwardRange!E
{
     E back();
     void popBack();
}
```

```d
5.iota.retro.writeln; // [4, 3, 2, 1, 0]
```

Un range ad accesso casuale ha una `length` nota e ogni elemento può essere acceduto direttamente.

```d
interface RandomAccessRange(E) : ForwardRange!E
{
     E opIndex(size_t i);
     size_t length();
}
```

Il range ad accesso casuale più conosciuto è l'array di D:

```d
auto r = [4, 5, 6];
r[1].writeln; // 5
```

### Algoritmi pigri per i range

Le funzioni in [`std.range`](http://dlang.org/phobos/std_range.html) e
[`std.algorithm`](http://dlang.org/phobos/std_algorithm.html) forniscono
i blocchi di costruzione che utilizzano questa interfaccia. I range permettono la
composizione di algoritmi complessi dietro un oggetto che
può essere iterato con facilità. Inoltre, i range permettono la creazione di oggetti
**pigri** che eseguono un calcolo solo quando è realmente necessario
in un'iterazione, ad esempio quando si accede al prossimo elemento del range.
Gli algoritmi speciali per i range saranno presentati più avanti nella
sezione [Gemme di D](gems/range-algorithms).

### Approfondimenti

- [`std.algorithm`](http://dlang.org/phobos/std_algorithm.html)
- [`std.range`](http://dlang.org/phobos/std_range.html)

## {SourceCode}

```d
import std.stdio : writeln;

struct FibonacciRange
{
    // Stati del generatore di Fibonacci
    int a = 1, b = 1;

    // Il range di Fibonacci non finisce mai
    enum empty = false;

    // Legge il primo elemento
    int front() const @property
    {
        return a;
    }

    // Rimuove il primo elemento
    void popFront()
    {
        auto t = a;
        a = b;
        b = t + b;
    }
}

void main()
{
    FibonacciRange fib;

    import std.range : drop, take;
    import std.algorithm.iteration :
        filter, sum;

    // Seleziona i primi 10 numeri di Fibonacci
    auto fib10 = fib.take(10);
    writeln("Fib 10: ", fib10);

    // Eccetto i primi cinque
    auto fib5 = fib10.drop(5);
    writeln("Fib 5: ", fib5);

    // Seleziona il sottoinsieme pari
    auto even = fib5.filter!(x => x % 2 == 0);
    writeln("FibEven : ", even);

    // Somma di tutti gli elementi
    writeln("Sum of FibEven: ", even.sum);

    // Solitamente questo viene riassunto come:
    fib.take(10)
         .drop(5)
         .filter!(x => x % 2 == 0)
         .sum
         .writeln;
}
```