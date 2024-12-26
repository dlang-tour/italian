# Array

In D esistono due tipi di Array: **statici** e **dinamici**.
L'accesso agli array di qualsiasi tipo viene controllato per verificare che sia all'interno dei limiti (tranne quando il compilatore può dimostrare
che i controlli dei limiti non sono necessari).
Un controllo dei limiti fallito produce un `RangeError` che interrompe l'applicazione.
I più temerari possono disabilitare questa funzione di sicurezza con il
flag del compilatore `-boundscheck=off`
per ottenere miglioramenti delle prestazioni a scapito della sicurezza.

### Array Statici

Gli array statici sono memorizzati nello stack se definiti all'interno di una funzione,
o nella memoria statica altrimenti. Hanno una lunghezza fissa,
nota al momento della compilazione. Il tipo di un array statico include
la dimensione fissa:

    int[8] arr;

Il tipo di `arr` è `int[8]`. Nota che la dimensione dell'array viene indicata
accanto al tipo, e non dopo il nome della variabile come in C/C++.

### Array Dinamici

Gli array dinamici sono memorizzati nell'heap e possono essere espansi
o ridotti durante l'esecuzione. Un array dinamico viene creato usando un'espressione `new`
e la sua lunghezza:

    int size = 8; // variabile runtime
    int[] arr = new int[size];

Il tipo di `arr` è `int[]`, che viene anche chiamato **slice**. Le slice
sono viste su un blocco contiguo di memoria e verranno spiegate
più in dettaglio nella [prossima sezione](basics/slices).
Gli array multidimensionali possono essere creati facilmente
usando la sintassi `auto arr = new int[3][3]`.

### Operazioni e Proprietà degli Array

Sia gli array statici che quelli dinamici forniscono la proprietà `.length`,
che è di sola lettura per gli array statici, ma può essere utilizzata nel caso di
array dinamici per modificarne dinamicamente la dimensione. La
proprietà `.dup` crea una copia dell'array.

L'indicizzazione di un array si riferisce a un elemento di quell'array.
Quando si indicizza un array attraverso la sintassi `arr[idx]`, un simbolo speciale
`$` denota la lunghezza di un array. Per esempio, `arr[$ - 1]` fa riferimento
all'ultimo elemento ed è una forma abbreviata di `arr[arr.length - 1]`.

Gli array possono essere concatenati usando l'operatore `~`, che
creerà un nuovo array dinamico.

    int[] a = [1, 2];
    a ~= [3, 4];
    assert(a.length == 4);
    a[0] = 10;
    a.length--;
    assert(a == [10, 2, 3]);

#### Operazioni Vettoriali

Le operazioni matematiche possono
essere applicate a interi array usando una sintassi come `c[] = a[] + b[]`, per esempio.
Questo aggiunge tutti gli elementi di `a` e `b` in modo che
`c[0] = a[0] + b[0]`, `c[1] = a[1] + b[1]`, ecc. È anche possibile
eseguire operazioni su un intero array con un singolo
valore:

    a[] *= 2; // moltiplica tutti gli elementi per 2
    a[] %= 26; // calcola il modulo per 26 per tutti gli elementi di a

Queste operazioni potrebbero essere ottimizzate
dal compilatore per utilizzare istruzioni speciali del processore che
eseguono le operazioni in una volta sola.

### Esercizio

Completa la funzione `encrypt` per cifrare il messaggio segreto.
Il testo deve essere cifrato usando la *cifratura di Cesare*,
che sposta i caratteri nell'alfabeto usando un certo indice.
Il testo da cifrare contiene solo caratteri nell'intervallo `a-z`,
il che dovrebbe rendere le cose più semplici.

Puoi consultare la soluzione [qui](https://github.com/dlang-tour/core/issues/227).

### Approfondimenti

- [Array in _Programming in D_](http://ddili.org/ders/d.en/arrays.html)
- [D Slices](https://dlang.org/d-array-article.html)
- [Specifiche degli Array](https://dlang.org/spec/arrays.html)

## {SourceCode:incomplete}

```d
import std.stdio : writeln;

/**
Sposta ogni carattere nell'array
`input` di `shift` posizioni.
L'intervallo dei caratteri è limitato a `a-z`
e il carattere successivo dopo z è a.

Parametri:
    input = array da cifrare
    shift = numero di posizioni di spostamento per ogni carattere
Ritorna:
    Array di caratteri cifrato
*/
char[] encrypt(char[] input, char shift)
{
    auto result = input.dup;
    // TODO: spostare ogni carattere
    return result;
}

void main()
{
    // Ora cifreremo il messaggio con
    // la cifratura di Cesare e un
    // fattore di spostamento di 16!
    char[] toBeEncrypted = [ 'w','e','l','c',
      'o','m','e','t','o','d',
      // L'ultima virgola è ok e verrà
      // semplicemente ignorata!
    ];
    writeln("Before: ", toBeEncrypted);
    auto encrypted = encrypt(toBeEncrypted, 16);
    writeln("After: ", encrypted);

    // Verifichiamo che l'algoritmo funzioni
    // come previsto
    assert(encrypted == [ 'm','u','b','s','e',
            'c','u','j','e','t' ]);
}
```