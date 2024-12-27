# Interfacce

D permette di definire le `interface` che sono tecnicamente simili ai tipi `class`, ma le cui funzioni membro devono essere implementate da qualsiasi classe che eredita dall'`interface`.

    interface IAnimal {
        void makeNoise();
    }

La funzione membro `makeNoise` deve essere implementata da `Dog` poiché eredita dall'interfaccia `Animal`. In sostanza, `makeNoise` si comporta come una funzione membro `abstract` in una classe base.

    class Dog : IAnimal {
        void makeNoise() {
            ...
        }
    }

    IAnimal animal = new Dog(); // cast implicito all'interfaccia
    animal.makeNoise();

Sebbene una classe possa ereditare direttamente da una *sola* classe base, può implementare un *numero qualsiasi* di interfacce.

### Pattern NVI (non virtual interface)

Il [pattern NVI](https://en.wikipedia.org/wiki/Non-virtual_interface_pattern) è un design pattern che aiuta a mantenere l'incapsulamento fornendo un'interfaccia pubblica non virtuale che internamente chiama metodi virtuali privati. In D, questo pattern viene implementato usando funzioni `final` nelle interfacce.

I vantaggi principali:

1. Garantisce un comportamento comune che non può essere modificato dalle classi derivate
2. Permette di aggiungere logica prima/dopo la chiamata al metodo virtuale
3. Centralizza il controllo sul flusso di esecuzione

Per esempio:

```d
interface IAnimal {
    // Metodo virtuale che le classi devono implementare
    void makeNoise();

    // Metodo final (non virtual) che usa makeNoise
    // Non può essere sovrascritto dalle classi derivate
    final void safeNoise() {
        // Qui possiamo aggiungere logica pre-esecuzione
        writeln("Preparing to make noise...");

        makeNoise(); // Chiama l'implementazione specifica

        // Qui possiamo aggiungere logica post-esecuzione
        writeln("Noise completed!");
    }
}
```

In questo modo, `safeNoise()` definisce un "template" di esecuzione che non può essere alterato, mentre le classi derivate possono personalizzare solo il comportamento di `makeNoise()`.

### Approfondimenti

- [Interfacce in _Programming in D_](http://ddili.org/ders/d.en/interface.html)
- [Interfacce in D](https://dlang.org/spec/interface.html)

## {SourceCode}

```d
import std.stdio : writeln;

interface IAnimal {
    void makeNoise();

    /*
    Pattern NVI. Usa makeNoise internamente
    per personalizzare il comportamento delle
    classi che ereditano.

    Parametri:
        n = numero di ripetizioni
    */
    final void multipleNoise(int n) {
        for(int i = 0; i < n; ++i) {
            makeNoise();
        }
    }
}

class Dog: IAnimal {
    void makeNoise() {
        writeln("Woof!");
    }
}

class Cat: IAnimal {
    void makeNoise() {
        writeln("Meeoauw!");
    }
}

void main() {
    IAnimal dog = new Dog;
    IAnimal cat = new Cat;
    IAnimal[] animals = [dog, cat];
    foreach(animal; animals) {
        animal.multipleNoise(5);
    }
}
```
