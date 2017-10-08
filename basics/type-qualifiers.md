# Mutabilità

D è un linguaggio statico: una volta che una variabile viene dichiarata,
non è più possibile cambiarne il tipo. Questo permette al compilatore di prevenire
errori e di imporre alcune restrizioni in fase di compilazione.
Con una buona sicurezza rispetto ai tipi si permette di progettare programmi più sicuri e manutenibili.

Ci sono svariati qualificatori di tipo in D ma i più comunemente usati sono
`const` e `immutable`.

### `immutable`

In aggiunta al sistema di tipi statici, il D fornisce dei qualificatori di tipo
(chiamati anche costruttori di tipo) che conferiscono a certi oggetti proprietà aggiuntive.
Per esempio un oggetto `immutable` può essere instanziato una volta sola e in seguito non più essere modificato.

    immutable int err = 5;
    // oppure: immutable err = 5 e viene dedotto che è int.
    err = 5; // non compila

Oggetti `immutable` possono così essere tranquillamente condivisi tra piu threads asincroni perchè non possono
essere modificati per definizione.
Questo implica inoltre che gli oggetti `immutable` possono essere posti in cache in sicurezza.

### `const`

Anche gli oggetti `const` non possono essere modificati. Ma la restrizione è valida solo nello scope di appartenenza.
Un puntatore `const` può essere creato sia da un oggetto *mutable*, sia da un oggetto *immutable*.
Questo significa che un oggetto è `const` nello scope corrente, ma altri protrebbero modificarlo da un differente contesto.
E' uso comune nelle APIs di accettare argomenti `const` per assicurarsi che non modifichino l'input, perché permette alla stessa funzione di processare sia dati `mutable` che `immutable`.

    void foo(const char[] s)
    {
        // se decommentata, la prossima riga risulterà in un errore
        // (non si possono modificare i const):
        // s[0] = 'x';

        import std.stdio : writeln;
        writeln(s);
    }

    // grazie a `const`, entrambe le chiamate compilano:
    foo("abcd"); // string è un array immutable
    foo("abcd".dup); // .dup ritorna una copia mutable

Entrambi `immutable` e `const` sono detti qualificatori _transitivi_.
Il che assicura che una volta applicato `const` a un tipo, questo viene applicato ricorsivamente a ogni suo sottocomponente.

### Approfondimenti

#### Riferimenti di base

- [Immutable in _Programming in D_](http://ddili.org/ders/d.en/const_and_immutable.html)
- [Scopes in _Programming in D_](http://ddili.org/ders/d.en/name_space.html)

#### Riferimenti avanzati

- [const(FAQ)](https://dlang.org/const-faq.html)
- [Type qualifiers in D](https://dlang.org/spec/const3.html)

## {SourceCode}

```d
import std.stdio : writeln;

void main()
{
    /**
    * Le variabili sono mutabili di default ed è
    * permesso modificarle:
    */
    int m = 100; // mutabile
    writeln("m: ", typeof(m).stringof);
    m = 10; // va bene

    /**
    * Puntare a memoria mutabile:
    */
    // Un puntatore const a un'area mutabile
    // è consentito
    const int* cm = &m;
    writeln("cm: ", typeof(cm).stringof);
    // Per definizione `const` non
    // può essere modificato:
    // *cm = 100; // errore!

    // Si garantisce che un `immutable` rimarrà
    // invariato, inoltre non può puntare a
    // memoria mutabile
    // immutable int* im = &m; // errore!

    /**
    * Puntare a memoria readonly:
    */
    immutable v = 100;
    writeln("v: ", typeof(v).stringof);
    // v = 5; // errore!

    // `const` può puntare a memoria readonly,
    // ma è esso stesso readonly
    const int* cv = &v;
    writeln("*cv: ", typeof(cv).stringof);
    // *cv = 10; // error!
}
```
