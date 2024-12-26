# Array Associativi

D ha incorporati gli *array associativi*, noti anche come hashmap.
Un array associativo con un tipo di chiave `string` e un tipo di valore
`int` si dichiara come segue:

    int[string] arr;

Il valore può essere acceduto tramite la sua chiave e quindi essere impostato:

    arr["key1"] = 10;

Per verificare se una chiave è presente nell'array associativo, si può
utilizzare l'espressione `in`:

    if ("key1" in arr)
        writeln("Yes");

L'espressione `in` restituisce un puntatore al valore se questo
può essere trovato o un puntatore `null` altrimenti. Quindi il controllo dell'esistenza
e la scrittura possono essere convenientemente combinati:

    if (auto val = "key1" in arr)
        *val = 20;

L'accesso a una chiave che non esiste genera un `RangeError`
che termina immediatamente l'applicazione. Per un accesso sicuro
con un valore predefinito, si può utilizzare `get(key, defaultValue)`.

Gli array associativi hanno la proprietà `.length` come gli array normali e forniscono
un membro `.remove(key)` per rimuovere le voci tramite la loro chiave.
È lasciato come esercizio al lettore esplorare
i range speciali `.byKey` e `.byValue`.

### Approfondimenti

- [Array associativi in _Programming in D_](http://ddili.org/ders/d.en/aa.html)
- [Specifiche degli array associativi](https://dlang.org/spec/hash-map.html)
- [std.array.byPair](http://dlang.org/phobos/std_array.html#.byPair)

## {SourceCode}

```d
import std.array : assocArray;
import std.algorithm.iteration: each, group,
    splitter, sum;
import std.string: toLower;
import std.stdio : writefln, writeln;

void main()
{
    string text = "Rock D with D";

    // Itera su tutte le parole e conta
    // ogni parola una volta
    int[string] words;
    text.toLower()
        .splitter(" ")
        .each!(w => words[w]++);

    foreach (key, value; words)
        writefln("key: %s, value: %d",
                       key, value);

    // `.keys` e `.values` restituiscono array
    writeln("Words: ", words.keys);

    // `.byKey`, `.byValue` e `.byKeyValue`
    // restituiscono range pigri e iterabili
    writeln("# Words: ", words.byValue.sum);

    // Un nuovo array associativo può essere creato
    // con `assocArray` passando un
    // range di tuple chiave/valore
    auto array = ['a', 'a', 'a', 'b', 'b',
                  'c', 'd', 'e', 'e'];

    // `.group` raggruppa elementi consecutivi equivalenti
    // in una singola tupla contenente
    // l'elemento e il numero delle sue ripetizioni
    auto keyValue = array.group;
    writeln("Key/Value range: ", keyValue);
    writeln("Associative array: ",
             keyValue.assocArray);
}
```