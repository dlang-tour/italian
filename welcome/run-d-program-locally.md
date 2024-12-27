# Eseguire programmi D sul proprio computer

Il linguaggio D viene distribuito con il compilatore `dmd`, lo pseudo-interprete `rdmd` e il gestore di pacchetti `dub`.

### Il compilatore DMD

Il compilatore *DMD* compila i file D e genera un file binario eseguibile.
Da riga di comando, *DMD* va invocato specificando i file da compilare:

    dmd esempio.d

È possibile modificare il comportamento del compilatore in diversi modi.
L'elenco completo delle opzioni è disponibile nella [documentazione online](https://dlang.org/dmd.html#switches) o digitando `dmd --help`.

### Compilazione ed esecuzione immediata con `rdmd`

Il programma `rdmd`, incluso nella distribuzione del compilatore DMD,
si occupa di compilare il file e tutte le sue dipendenze, eseguendo poi automaticamente l'applicazione:

    rdmd esempio.d

Sui sistemi UNIX, aggiungendo la riga shebang `#!/usr/bin/env rdmd` all'inizio di un file D, è possibile eseguirlo direttamente come uno script.

L'elenco delle opzioni si può trovare nella [documentazione online](https://dlang.org/rdmd.html) o eseguendo `rdmd --help`.

### Il gestore di librerie `dub`

[`dub`](http://code.dlang.org) è il gestore ufficiale di librerie del linguaggio D.
Se si dispone di `dub`, un nuovo progetto `esempio` può essere creato tramite il comando:

    dub init esempio

Eseguendo `dub` all'interno della cartella `esempio`, generata dal comando precedente,
tutte le librerie da cui il progetto dipende saranno scaricate, il codice sorgente compilato
e il programma risultante eseguito.
Il comando `dub build` permette di compilare il progetto senza eseguirlo.

L'elenco dei comandi e delle capacità di `dub` si possono trovare nella [documentazione online](https://code.dlang.org/docs/commandline) o eseguendo `dub help`.

L'elenco delle librerie disponibili tramite dub può essere consultato attraverso [l'interfaccia web](https://code.dlang.org).
