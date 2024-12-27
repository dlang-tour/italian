# Come installare D sul proprio computer

Sul sito ufficiale del linguaggio D [dlang.org](https://dlang.org) è possibile [scaricare](http://dlang.org/download.html) l'ultima versione del compilatore di riferimento **DMD** (Digital Mars D):

### Windows

* Tramite il [programma di installazione](http://downloads.dlang.org/releases/2.x/{{latest-release}}/dmd-{{latest-release}}.exe)
* oppure come [archivio compresso](http://downloads.dlang.org/releases/2.x/{{latest-release}}/dmd.{{latest-release}}.windows.7z)
* o tramite [chocolatey](https://chocolatey.org/packages/dmd): `choco install dmd`

### Mac OS X

* Come [pacchetto `.dmg`](http://downloads.dlang.org/releases/2.x/{{latest-release}}/dmd.{{latest-release}}.dmg)
* oppure come [archivio compresso](http://downloads.dlang.org/releases/2.x/{{latest-release}}/dmd.{{latest-release}}.osx.tar.xz)
* o tramite [Homebrew](http://brew.sh): `brew install dmd`

### Linux / FreeBSD

Per installare DMD nella propria directory utente è sufficiente eseguire:
`curl -fsS https://dlang.org/install.sh | bash -s dmd`

Sono disponibili pacchetti per diverse distribuzioni:

* [ArchLinux](https://wiki.archlinux.org/index.php/D_(programming_language))
* [Debian/Ubuntu](http://d-apt.sourceforge.net).
* [Fedora/CentOS](http://dlang.org/download.html#dmd)
* [Gentoo](https://wiki.gentoo.org/wiki/Dlang)
* [OpenSuse](http://dlang.org/download.html#dmd)

### Altri compilatori

Oltre al compilatore di riferimento DMD, che utilizza un proprio backend, sono disponibili altri due compilatori che si possono trovare nella sezione download di [dlang.org](https://dlang.org):

* [**LDC**](https://github.com/ldc-developers/ldc#installation), basato sul backend LLVM
* [**GDC**](http://gdcproject.org/downloads), che utilizza il backend GCC

LDC e GDC non sono sempre aggiornati all'ultima versione del frontend DMD, ma offrono livelli di ottimizzazione superiori e supportano piattaforme aggiuntive, come ARM.

Per maggiori informazioni consultare la [wiki](https://wiki.dlang.org/Compilers).

## Configurare il proprio editor

La bellezza di D sta nel fatto che non serve un IDE sofisticato. Tuttavia, programmare in D è più piacevole quando si è nella zona di comfort del proprio editor preferito.
Esistono plugin D per almeno i seguenti editor:

- [Atom](https://github.com/Pure-D/atomize-d)
- [Eclipse](http://ddt-ide.github.io)
- [Emacs](https://github.com/Emacs-D-Mode-Maintainers/Emacs-D-Mode)
- [IntelliJ](https://github.com/intellij-dlanguage/intellij-dlanguage)
- [Sublime Text](https://github.com/yazd/DKit)
- [Vim](https://wiki.dlang.org/D_in_Vim)
- [VS Code](https://marketplace.visualstudio.com/items/webfreak.code-d)
- [__Visual Studio__](http://rainers.github.io/visuald/visuald/StartPage.html)

È anche possibile provare un IDE dedicato a D:

- [Dlang IDE](https://github.com/buggins/dlangide)

La wiki di D contiene una panoramica più dettagliata degli [editor](https://wiki.dlang.org/Editors) e [IDE](https://wiki.dlang.org/IDEs) disponibili.

