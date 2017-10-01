# Imports e moduli

Per creare un semplice programma ciao mondo in D, sono necessari degli `import`.
Le istruzioni `import` permettono di accedere alle funzioni e ai tipi pubblici di un dato **modulo**.

La libreria standard, chiamata [Phobos](https://dlang.org/phobos/),
è collocata nel **package** `std`
e i suoi moduli sono referenziati con l' uso della riga `import std.MODULE`.

L'istruzione `import` permette anche di selezionare uno specifico simbolo dal modulo:

    import std.stdio : writeln, writefln;

Queste importazioni selettive possono essere utilizzate per migliorare la leggibilità del codice
palesando l'origine di un determinato simbolo. Previene inoltre la collisione di simboli che hanno
lo stesso nome, ma origini differenti.

Un istruzione `import` non deve necessariamente apparire in cima al listato del codice sorgente.
Può essere tranquillamente usata all' interno di funzioni o da altre parti.

## {SourceCode}

```d
void main()
{
    import std.stdio;
    // oppure import std.stdio : writeln;
    writeln("Ciao Mondo!");
}
```
