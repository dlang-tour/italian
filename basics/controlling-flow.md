# Controllo di flusso

Il flusso di esecuzione di un'applicazione può essere controllato attraverso le
istruzioni `if` ed `else`:

    if (a == 5) {
        writeln("Condizione verificata");
    } else if (a > 10) {
        writeln("Altra condizione verificata");
    } else {
        writeln("Nessuna condizione verificata!");
    }

Quando i blocchi `if` o `else` contengono una singola istruzione,
le parentesi graffe possono essere omesse.

D fornisce gli stessi operatori di confronto presenti in C/C++ e Java:

* `==` e `!=` per verificare uguaglianza e disuguaglianza
* `<`, `<=`, `>` e `>=` per confrontare se un valore è minore (o minore uguale) e maggiore (o maggiore uguale)

Per combinare più condizioni, si usa l'operatore `||` per l'*OR* logico
e `&&` per l'*AND* logico.

L'istruzione `switch`..`case` permette di eseguire codice diverso in base al
valore di una variabile. In D, `switch` funziona con tutti i tipi base,
stringhe incluse! Per i tipi interi, è possibile definire anche intervalli
di valori usando la sintassi `case START: .. case END:`.
Dai un'occhiata al codice di esempio per vedere come funziona.

### Approfondimenti

#### Riferimenti di base

- [Espressioni logiche in _Programming in D_](http://ddili.org/ders/d.en/logical_expressions.html)
- [Istruzione if in _Programming in D_](http://ddili.org/ders/d.en/if.html)
- [Operatore ternario in _Programming in D_](http://ddili.org/ders/d.en/ternary.html)
- [`switch` e `case` in _Programming in D_](http://ddili.org/ders/d.en/switch_case.html)

#### Riferimenti avanzati

- [Espressioni nel dettaglio](https://dlang.org/spec/expression.html)
- [Specifiche dell'istruzione if](https://dlang.org/spec/statement.html#if-statement)

## {SourceCode}

```d
import std.stdio : writeln;

void main()
{
    if (1 == 1)
        writeln("Fidati della matematica in D");

    int c = 5;
    switch(c) {
        case 0: .. case 9:
            writeln(c, " è compreso tra 0-9");
            break; // necessario!
        case 10:
            writeln("Un dieci!");
            break;
        default: // Se niente si combina
            writeln("Nulla");
            break;
    }
}
```
