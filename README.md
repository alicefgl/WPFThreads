# WPFThreads

Consegna dell'esercizio:

Si vuole visualizzare un conteggio sfruttando le istruzioni multithreads messe a disposizione dal linguaggio C#.

Si prevedano quindi tre oggetti TextBlock WPF che visualizzano l'avanzamento di tre contatori a velocità diverse.

Il primo scatta ogni ms e conta fino a 5000 (durata totale 5 secondi). Il secondo scatta ogni 10ms e conta fino a 500 (durata totale 5 secondi). Il terzo scatta ogni 100ms e conta fino a 50 (durata totale 5 secondi).

Si preveda un quarto TextBlock che visualizzi il totale dei tre contatori (alla fine visualizzerà 5.550)

Si preveda un pulsante "Start" per far partire i conteggi che rimanga disattivato durante il conteggio e si riattivi alla fine

Opzionale: visualizzare il conteggio totale tramite una ProgressBar WPF

# Diario di bordo WPF Threads

## Inizio 13 ottobre 2022
## Introduzione a WPF

L'ultima versione di Dotnet è .Net7

<img src="images\creazione progetto WPF.png" width="350" title="creazione progetto WPF">

<img src="images\dotnet6.png" width="350" title="creazione progetto WPF">

Applicazione WPF (.net framework)
nome delle soluzioni: cognome.nome.4i.primoWPF
Ogni componente xaml ha una serie di eventi che gli corrispondono (Proprietà->Fulmine).
Questa linea di codice è contenuta nella pagina C# relativa al documento xaml:

<img src="images\starterWPF.png" width="350" title="creazione progetto WPF">

Richiamo della funzione nella pagina xaml:

<img src="images\gestore di eventi.png" width="350" title="creazione progetto WPF">

## 02/02/2023
## Introduzione a WPF

MAUI -> Multiple Architectal User Interface: è molto vicina a xamarin ma riprende la programmazione Windows.
WPF introduce per la prima volta il sistema di programmazione in Xamarin: esso risulta molto minimale e perciò anche semplice da imparare. E’ un linguaggio strutturato che descrive le User Interface tramite (ad esempio) delle grid.

Per creare il nostro progetto abbiamo utilizzato .NET 6.0 (più facilmente supportato).

Pulsante destro sulla soluzione->apri cartella in esplora file (serve per sapere dove si trova il progetto nel caso in cui, ad esempio, si perda).

Si crea una cartella per ogni progetto: la cartella conterrà oltretutto il progetto stesso (con estensione csproj).

Tasto destro sulla barra nera in alto a destra->spuntare “Compilazione” (aggiunge due tasti all’interfaccia di visual studio, in questo modo si crea un file eseguibile. Questo comporta che quando si avvia il programma scritto, la compilazione potrebbe richiedere più tempo risultando più pesante. Il pulsante Compilazione può essere utile per eseguire il codice senza lanciare il programma a scopo di verificare che esso non presenti errori. Potrebbe essere utile altrimenti compilare solo il progetto corrente).

<img src="images\gestore di eventi.png" width="350" title="creazione progetto WPF">
