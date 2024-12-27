# Template

**D** offre la possibilità di definire funzioni template analoghe a C++ e Java,
che rappresentano un modo per creare funzioni o oggetti **generici** in grado di operare
con qualsiasi tipo che soddisfi le operazioni definite nel corpo della funzione:

    auto add(T)(T lhs, T rhs) {
        return lhs + rhs;
    }

Il parametro template `T` è definito in un set di parentesi
davanti ai parametri effettivi della funzione. `T` è un segnaposto
che viene sostituito dal compilatore quando effettivamente *istanzia*
la funzione usando l'operatore `!`:

    add!int(5, 10);
    add!float(5.0f, 10.0f);
    add!Animal(dog, cat); // non compilerà; Animal non implementa +

### Deduzione Implicita dei Parametri Template

Le funzioni template hanno due tipi di parametri:
- parametri compile-time (primo set)
- parametri runtime (secondo set)

Le funzioni non template possono accettare solo parametri runtime.
Quando si chiama una funzione template senza specificare esplicitamente i parametri compile-time,
il compilatore tenta di dedurli automaticamente dai tipi degli argomenti runtime forniti.

    int a = 5; int b = 10;
    add(a, b); // T viene dedotto come `int`
    float c = 5.0;
    add(a, c); // T viene dedotto come `float`

### Caratteristiche dei Template

Una funzione template può accettare un numero arbitrario di parametri template,
che vengono specificati durante l'istanziazione usando la sintassi `func!(T1, T2 ..)`.
I parametri template possono essere di qualsiasi tipo base,
compresi `string` e numeri in virgola mobile.

A differenza dei generics di Java, i template in D operano esclusivamente a compile-time e generano
codice ottimizzato specifico per ciascuna combinazione di tipi
utilizzata nelle chiamate della funzione.

Naturalmente, anche i tipi `struct`, `class` e `interface` possono essere definiti come
tipi template.

    struct S(T) {
        // ...
    }

### Approfondimenti

- [Tutorial sui Template D](https://github.com/PhilippeSigaud/D-templates-tutorial)
- [Template in _Programming in D_](http://ddili.org/ders/d.en/templates.html)

#### Avanzato

- [Specifiche dei Template D](https://dlang.org/spec/template.html)
- [Templates Rivisitati](http://dlang.org/templates-revisited.html): Walter Bright scrive su come D migliora i template C++.
- [Template Variadici](http://dlang.org/variadic-function-templates.html): Articoli sull'idioma D per implementare funzioni variadiche con template variadici

## {SourceCode}

```d
import std.stdio : writeln;

/**
Classe template che permette
l'implementazione generica di animali.
Parametri:
    noise = stringa da scrivere
*/
class Animal(string noise) {
    void makeNoise() {
        writeln(noise ~ "!");
    }
}

class Dog: Animal!("Woof") {
}

class Cat: Animal!("Meeoauw") {
}

/**
Funzione template che accetta qualsiasi
tipo T che implementa una funzione
makeNoise.
Parametri:
    animal = oggetto che può fare rumore
    n = numero di chiamate makeNoise
*/
void multipleNoise(T)(T animal, int n) {
    for (int i = 0; i < n; ++i) {
        animal.makeNoise();
    }
}

void main() {
    auto dog = new Dog;
    auto cat = new Cat;
    multipleNoise(dog, 5);
    multipleNoise(cat, 5);
}
```
