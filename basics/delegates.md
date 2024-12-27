# Delegati

### Funzioni come argomenti

Una funzione può anche essere un parametro di un'altra funzione:

    void doSomething(int function(int, int) doer) {
        // chiama la funzione passata
        doer(5,5);
    }

    doSomething(&add); // usa la funzione globale `add` qui
                       // add deve avere 2 parametri int

`doer` può quindi essere chiamato come qualsiasi altra funzione normale.

### Funzioni locali con contesto

L'esempio precedente utilizza il tipo `function` che è
un puntatore a una funzione globale. Non appena viene referenziata
una funzione membro o una funzione locale, è necessario
utilizzare i `delegate`. È un puntatore a funzione
che contiene inoltre informazioni sul suo contesto
o *chiusura*, quindi chiamato anche **closure**
in altri linguaggi. Per esempio, un `delegate`
che punta a una funzione membro di una classe include anche
il puntatore all'oggetto della classe. Un `delegate` creato da
una funzione annidata include invece un collegamento al contesto
che la racchiude. Tuttavia, il compilatore D può automaticamente fare una copia
del contesto nell'heap se necessario per la sicurezza della memoria -
in questo caso un delegate si collegherà a quest'area heap.

    void foo() {
        void local() {
            writeln("local");
        }
        auto f = &local; // f è di tipo delegate()
    }

La stessa funzione `doSomething` che accetta un `delegate`
apparirebbe così:

    void doSomething(int delegate(int,int) doer);

Gli oggetti `delegate` e `function` non possono essere mescolati. Ma la
funzione standard
[`std.functional.toDelegate`](https://dlang.org/phobos/std_functional.html#.toDelegate)
converte una `function` in un `delegate`.

### Funzioni anonime e Lambda

Poiché le funzioni possono essere salvate come variabili e passate ad altre funzioni,
può essere laborioso dare loro un nome proprio e definirle. Quindi D permette
funzioni senza nome e _lambda_ in una riga.

    auto f = (int lhs, int rhs) {
        return lhs + rhs;
    };
    auto f = (int lhs, int rhs) => lhs + rhs; // Lambda - internamente convertita come sopra

È anche possibile passare stringhe come argomenti template alle parti funzionali
della libreria standard di D. Per esempio, offrono un modo conveniente
per definire una riduzione (anche detta reducer):

    [1, 2, 3].reduce!`a + b`; // 6

Le funzioni stringa sono possibili solo per _uno o due_ argomenti e usano `a`
come primo e `b` come secondo argomento.

### Approfondimento

- [Specifiche dei Delegate](https://dlang.org/spec/function.html#closures)

## {SourceCode}

```d
import std.stdio : writeln;

enum IntOps {
    add = 0,
    sub = 1,
    mul = 2,
    div = 3
}

/**
Fornisce un calcolo matematico
Params:
    op = operazione matematica selezionata
Returns: delegate che esegue un'operazione matematica
*/
auto getMathOperation(IntOps op)
{
    // Definisce 4 funzioni lambda per
    // 4 diverse operazioni matematiche
    auto add = (int lhs, int rhs) => lhs + rhs;
    auto sub = (int lhs, int rhs) => lhs - rhs;
    auto mul = (int lhs, int rhs) => lhs * rhs;
    auto div = (int lhs, int rhs) => lhs / rhs;

    // possiamo assicurarci che lo switch copra
    // tutti i casi
    final switch (op) {
        case IntOps.add:
            return add;
        case IntOps.sub:
            return sub;
        case IntOps.mul:
            return mul;
        case IntOps.div:
            return div;
    }
}

void main()
{
    int a = 10;
    int b = 5;

    auto func = getMathOperation(IntOps.add);
    writeln("Il tipo di func è ",
        typeof(func).stringof, "!");

    // esegue il delegate func che fa tutto il
    // vero lavoro per noi!
    writeln("risultato: ", func(a, b));
}
```
