# I tipi di base

D fornisce un insieme di tipi base che mantengono la stessa
dimensione **indipendentemente** dalla piattaforma.
L'unica eccezione è il tipo `real`, che fornisce la massima precisione
in virgola mobile disponibile. Non esistono differenze nelle dimensioni degli
interi, anche quando l'applicazione viene compilata su sistemi a 32 o 64 bit.

| tipo                          | dimensione
|-------------------------------|------------
|`bool`                         | 8-bit
|`byte`, `ubyte`, `char`        | 8-bit
|`short`, `ushort`, `wchar`     | 16-bit
|`int`, `uint`, `dchar`         | 32-bit
|`long`, `ulong`                | 64-bit

#### Tipi a virgola mobile:

| tipo    | dimensione
|---------|--------------------------------------------------
|`float`  | 32-bit
|`double` | 64-bit
|`real`   | >= 64-bit (generalmente 64-bit, ma 80-bit su piattaforme Intel x86 a 32-bit)

Il prefisso `u` denota i tipi *senza segno* (unsigned). `char` rappresenta caratteri UTF-8,
`wchar` viene utilizzato per le stringhe UTF-16 e `dchar`
per le stringhe UTF-32.

Il compilatore permette la conversione tra variabili di tipi differenti
solo quando non si verifica perdita di precisione.
Tuttavia, le conversioni tra tipi in virgola mobile (es: da `double` a `float`)
sono sempre consentite.

È possibile forzare la conversione in un altro tipo utilizzando l'espressione
`cast(TYPE) myVar`. Questa operazione va utilizzata con cautela, poiché
il `cast` bypassa il sistema di controllo dei tipi.

La keyword `auto` permette di dichiarare una variabile lasciando che il compilatore
ne deduca il tipo dall'espressione di inizializzazione.
Per esempio, `auto myVar = 7` assegnerà automaticamente il tipo `int` alla variabile `myVar`.
È importante notare che il tipo viene determinato durante la compilazione e non può essere
modificato successivamente, esattamente come se fosse stato dichiarato esplicitamente.

### Le proprietà dei tipi

Tutti i tipi dato hanno una proprietà `.init` dalla quale vengono inizializzati.
Per gli interi è `0` e per i numeri a virgola mobile è `nan` (*not a number*).

I tipi interi e a virgola mobile hanna una proprietà `.max` che contiente il massimo
valore che riescono a rappresentare.
I tipi interi hanno anche una proprietà `.min` per il valore più piccolo, mentre i numeri a
virgola mobile hanno un `.min_normal` che è definito come il valore più piccolo rappresentabile
diverso da 0.

I numeri a virgola mobile prevedono anche una proprietà `.nan` (NaN - Not a Number), e `.infinity`
(valori infiniti), `.dig` (numero di cifre di precisione decimale), `.mant_dig`
(numero di bit per mantissa) e altre.

Ogni tipo prevede inoltre una proprietà `.stringof` che tramuta il suo valore in una striga.

### Indici in D

In D, gli indici tipicamente hanno un tipo alias `size_t`, perché è un tipo grande abbastanza per rappresentare l' offset all'interno della memoria indicizzabile - che è il
`uint` per le architetture a 32-bit e l' `ulong` per quelle a 64-bit.

### Assert

`assert` è un'espressione che verifica la presenza di condizioni nella modalità di debug e gli abort
con `AssertionError` in caso di fallimenti.
`assert(0)` viene usato per contrassegnare codice irraggiungibile.

### Approfondimenti

#### Riferimenti di base

- [Assegnazioni](http://ddili.org/ders/d.en/assignment.html)
- [Variabili](http://ddili.org/ders/d.en/variables.html)
- [Aritmetica](http://ddili.org/ders/d.en/arithmetic.html)
- [Virgola Mobile](http://ddili.org/ders/d.en/floating_point.html)
- [Tipi fondamentali in _Programming in D_](http://ddili.org/ders/d.en/types.html)

#### Riferimenti avanzati

- [Panoramica dei tipi in D](https://dlang.org/spec/type.html)
- [`auto` e `typeof` in _Programming in D_](http://ddili.org/ders/d.en/auto_and_typeof.html)
- [Proprietà dei tipi](https://dlang.org/spec/property.html)
- [Assert](https://dlang.org/spec/expression.html#AssertExpression)

## {SourceCode}

```d
import std.stdio : writeln;

void main()
{
    // Grandi numeri possono essere separati
    // con un underscore "_"
    // per migliorare la leggibilità.
    int b = 7_000_000;
    short c = cast(short) b; // serve un cast
    uint d = b; // fine
    int g;
    assert(g == 0);

    auto f = 3.1415f; // f indica un float

    // typeid(VAR) restituisce informazioni
    // su tipi
    writeln("type of f is ", typeid(f));
    double pi = f; // fine
    // per i tipi a virgola mobile
    // il cast implicito è permesso
    float demoted = pi;

    // accedere a proprietà
    assert(int.init == 0);
    assert(int.sizeof == 4);
    assert(bool.max == 1);
    writeln(int.min, " ", int.max);
    writeln(int.stringof); // int
}
```
