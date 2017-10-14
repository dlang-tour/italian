# Controllo di flusso

Il flusso di esecuzione di un applicazione può essere condizionato dalle
istruzioni `if` e `else`:

    if (a == 5) {
        writeln("Condizione verificata");
    } else if (a > 10) {
        writeln("Altra condizione verificata");
    } else {
        writeln("Nessuna condizione verificata!");
    }

Quando i blocchi di `if` o `else` contengono solo un'istruzione,
le parentesi graffe posso essere omesse.

D fornisce gli stessi operatori del C/C++ e del java per comparare
le variabili tra loro:

* `==` e `!=` per testare uguaglianza e disuguaglianza
* `<`, `<=`, `>` e `>=` per il test di minore (o minore eguale) e maggiore (o minore eguale)

Per combinare tra loro più condizioni, l'operatore || rappresenta
l'*OR* logico, e `&&` l'*AND* logico.

D fornisce anche un'istruzione `switch`..`case` che permette di eseguire
un case dipendente dal valore di *una* variabile.
`switch` funziona con tutti i tipi base comprese le stringhe!
È persino possibile definire dei range per i tipi interi utilizzando
la sintassi `case START: .. case END:`.
Assicuratevi di dare un'occhiata al codice sorgente d'esempio.

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
