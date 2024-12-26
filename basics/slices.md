# Slice

Le slice sono oggetti di tipo `T[]` per qualsiasi tipo `T` dato.
Le slice forniscono una vista su un sottoinsieme di un array
di valori `T` - o semplicemente puntano all'intero array.
**Le slice e gli array dinamici sono la stessa cosa.**

Una slice è composta da due membri - un puntatore all'elemento iniziale e la
lunghezza della slice:

    T* ptr;
    size_t length; // unsigned 32 bit su 32bit, unsigned 64 bit su 64bit

### Ottenere una slice tramite nuova allocazione

Quando viene creato un nuovo array dinamico, viene restituita una slice
alla memoria appena allocata:

    auto arr = new int[5];
    assert(arr.length == 5); // memoria referenziata in arr.ptr

In questo caso, la memoria effettivamente allocata è completamente gestita dal garbage
collector. La slice restituita agisce come una "vista" sugli elementi sottostanti.

### Ottenere una slice sulla memoria esistente

Utilizzando un operatore di slicing si può anche ottenere una slice che punta a della memoria
già esistente. L'operatore di slicing può essere applicato a un'altra slice, array statici,
struct/classi che implementano `opSlice` e alcune altre entità.

In un'espressione di esempio `origin[start .. end]`, l'operatore di slicing viene utilizzato per ottenere
una slice di tutti gli elementi di `origin` da `start` fino all'elemento _prima_ di `end`:

    auto newArr = arr[1 .. 4]; // l'indice 4 NON è incluso
    assert(newArr.length == 3);
    newArr[0] = 10; // modifica newArr[0] ovvero arr[1]

Tali slice generano una nuova vista sulla memoria esistente. *Non* creano
una nuova copia. Se nessuna slice mantiene più un riferimento a quella memoria - o a una parte
*affettata* di essa - questa verrà liberata dal garbage collector.

Usando le slice, è possibile scrivere codice molto efficiente per cose (come i parser, per esempio)
che operano solo su un blocco di memoria, e ne estraggono solo le parti su cui devono
effettivamente lavorare. In questo modo, non c'è bisogno di allocare nuovi blocchi di memoria.

Come visto nella [sezione precedente](basics/arrays), l'espressione `[$]` è una forma abbreviata per
`arr.length`. Quindi `arr[$]` indicizza l'elemento successivo alla fine della slice, e
genererebbe un `RangeError` (se il controllo dei limiti non è stato disabilitato).

### Approfondimenti

- [Introduzione alle Slice in D](http://dlang.org/d-array-article.html)
- [Slice in _Programming in D_](http://ddili.org/ders/d.en/slices.html)

## {SourceCode}

```d
import std.stdio : writeln;

void main()
{
    int[] test = [ 3, 9, 11, 7, 2, 76, 90, 6 ];
    test.writeln;
    writeln("Primo elemento: ", test[0]);
    writeln("Ultimo elemento: ", test[$ - 1]);
    writeln("Escludi i primi due elementi: ",
        test[2 .. $]);

    writeln("Le slice sono viste sulla memoria:");
    auto test2 = test;
    auto subView = test[3 .. $];
    test[] += 1; // incrementa ogni elemento di 1
    test.writeln;
    test2.writeln;
    subView.writeln;

    // Crea una slice vuota
    assert(test[2 .. 2].length == 0);
}
```