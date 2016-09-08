# Eseguire programmi D sul proprio computer

Il linguaggio D è accompagnato non solo dal compilatore `dmd`, ma anche dallo pseudo-interprete `rdmd` e dal gestore di librerie `dub`.

### Il compilatore DMD

Il compilatore *DMD* compila file D e genera un file binario eseguibile.
Nella riga di comando *DMD* dev'essere invocato specificando la lista dei file da compilare:

    dmd esempio.d

Il comportamento del compilatore può essere modificato in molti modi.
L'elenco completo delle opzioni si può trovare nella [documentazione online](https://dlang.org/dmd.html#switches) o eseguendo `dmd --help`.

### Compilazione al volo con `rdmd`

Il programma `rdmd`, distribuito congiuntamente al compilatore DMD,
garantisce che il file e tutte le sue dipendenze siano compilate ed esegue automaticamente l'applicazione prodotta:

    rdmd esempio.d

Su sistemi UNIX la riga shebang `#!/usr/bin/env rdmd`, posta all'inizio di un file D, ne permette l'esecuzione diretta come se si trattasse di uno script.

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
