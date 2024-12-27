# Eccezioni

Questa guida riguarda solo le `Eccezioni` utente - gli `Errori` di sistema sono solitamente fatali
e non dovrebbero __mai__ essere catturati.

### Catturare le Eccezioni

Un caso comune per le eccezioni è la validazione di input utente potenzialmente non valido.
Una volta che un'eccezione viene lanciata, lo stack verrà srotolato fino a trovare
il primo gestore di eccezioni corrispondente.

```d
try
{
    readText("dummyFile");
}
catch (FileException e)
{
    // ...
}
```

È possibile avere più blocchi `catch` e un blocco `finally` che viene eseguito
indipendentemente dal verificarsi di un errore. Le eccezioni vengono lanciate con `throw`.

```d
try
{
    throw new StringException("Non puoi passare.");
}
catch (FileException e)
{
    // ...
}
catch (StringException e)
{
    // ...
}
finally
{
    // ...
}
```

Ricorda che lo [scope guard](gems/scope-guards) è solitamente una soluzione migliore
al pattern `try-finally`.

### Eccezioni personalizzate

Si può facilmente ereditare da `Exception` e creare eccezioni personalizzate:

```d
class UserNotFoundException : Exception
{
    this(string msg, string file = __FILE__, size_t line = __LINE__) {
        super(msg, file, line);
    }
}
throw new UserNotFoundException("D-Man è in vacanza");
```

### Entra in un mondo sicuro con `nothrow`

Il compilatore D può garantire che una funzione non possa causare effetti collaterali catastrofici.
Tali funzioni possono essere annotate con la parola chiave `nothrow`. Il compilatore D
impedisce staticamente il lancio di eccezioni nelle funzioni `nothrow`.

```d
bool lessThan(int a, int b) nothrow
{
    writeln("mondo non sicuro"); // l'output può lanciare eccezioni, quindi questo è proibito
    return a < b;
}
```

Da notare che il compilatore è in grado di dedurre automaticamente gli attributi
per il codice template.

### std.exception

È importante evitare `assert` e la [programmazione per contratti](gems/contract-programming)
(che verrà introdotta più avanti) per l'input utente, poiché `assert` e i contratti
vengono rimossi quando si compila in modalità release. Per comodità,
[`std.exception`](https://dlang.org/phobos/std_exception.html) fornisce
[`enforce`](https://dlang.org/phobos/std_exception.html#enforce)
che può essere usato come `assert`, ma lancia `Exception`
invece di un `AssertError`.

```d
import std.exception : enforce;
float magic = 1_000_000_000;
enforce(magic + 42 - magic == 42, "La matematica in virgola mobile è divertente");

// lancia eccezioni personalizzate
enforce!StringException('a' != 'A', "Algoritmo sensibile alle maiuscole/minuscole");
```

Tuttavia c'è altro in `std.exception`. Per esempio, quando l'errore potrebbe non essere
fatale, si può optare per
[raccoglierlo](https://dlang.org/phobos/std_exception.html#collectException):

```d
import std.exception : collectException;
auto e = collectException(operazionePericolosa());
if (e)
    writeln("L'operazione pericolosa è fallita con ", e);
```

Per verificare se un'eccezione viene lanciata nei test, usa [`assertThrown`](https://dlang.org/phobos/std_exception.html#assertThrown).

### Approfondimenti

- [Sicurezza delle Eccezioni in D](https://dlang.org/exception-safe.html)
- [std.exception](https://dlang.org/phobos/std_exception.html)
- [core.exception](https://dlang.org/phobos/core_exception.html) a livello di sistema
- [object.Exception](https://dlang.org/library/object/exception.html) - classe base di tutte le eccezioni che sono sicure da catturare e gestire.

## {SourceCode}

```d
import std.file : FileException, readText;
import std.stdio : writeln;

void main()
{
    try
    {
        readText("dummyFile");
    }
    catch (FileException e)
    {
        writeln("Messaggio:\n", e.msg);
        writeln("File: ", e.file);
        writeln("Linea: ", e.line);
        writeln("Stack trace:\n", e.info);

        // Si potrebbe usare anche la formattazione predefinita
        // writeln(e);
    }
}
```
