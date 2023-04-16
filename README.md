# WPFThreads

Consegna dell'esercizio:

Si vuole visualizzare un conteggio sfruttando le istruzioni multithreads messe a disposizione dal linguaggio C#.

Si prevedano quindi tre oggetti TextBlock WPF che visualizzano l'avanzamento di tre contatori a velocità diverse.

Il primo scatta ogni ms e conta fino a 5000 (durata totale 5 secondi). Il secondo scatta ogni 10ms e conta fino a 500 (durata totale 5 secondi). Il terzo scatta ogni 100ms e conta fino a 50 (durata totale 5 secondi).

Si preveda un quarto TextBlock che visualizzi il totale dei tre contatori (alla fine visualizzerà 5.550)

Si preveda un pulsante "Start" per far partire i conteggi che rimanga disattivato durante il conteggio e si riattivi alla fine

Opzionale: visualizzare il conteggio totale tramite una ProgressBar WPF

# Diario di bordo WPFThreads

## Inizio 13 ottobre 2022
## Introduzione a WPF

L'ultima versione di Dotnet è .Net7

<img src="images\creazione progetto WPF.png" width="350" title="creazione progetto WPF">

<img src="images\dotnet6.png" width="350" title="dotnet6">

Applicazione WPF (.net framework)
nome delle soluzioni: cognome.nome.4i.primoWPF
Ogni componente xaml ha una serie di eventi che gli corrispondono (Proprietà->Fulmine).
Questa linea di codice è contenuta nella pagina C# relativa al documento xaml:

<img src="images\starter WPF.png" width="350" title="starter WPF">

Richiamo della funzione nella pagina xaml:

<img src="images\gestore di eventi.png" width="350" title="gestore di eventi">

## 02/02/2023
## Introduzione a WPF

MAUI -> Multiple Architectal User Interface: è molto vicina a xamarin ma riprende la programmazione Windows.
WPF introduce per la prima volta il sistema di programmazione in Xamarin: esso risulta molto minimale e perciò anche semplice da imparare. E’ un linguaggio strutturato che descrive le User Interface tramite (ad esempio) delle grid.

Per creare il nostro progetto abbiamo utilizzato .NET 6.0 (più facilmente supportato).

Pulsante destro sulla soluzione->apri cartella in esplora file (serve per sapere dove si trova il progetto nel caso in cui, ad esempio, si perda).

Si crea una cartella per ogni progetto: la cartella conterrà oltretutto il progetto stesso (con estensione csproj).

Tasto destro sulla barra nera in alto a destra->spuntare “Compilazione” (aggiunge due tasti all’interfaccia di visual studio, in questo modo si crea un file eseguibile. Questo comporta che quando si avvia il programma scritto, la compilazione potrebbe richiedere più tempo risultando più pesante. Il pulsante Compilazione può essere utile per eseguire il codice senza lanciare il programma a scopo di verificare che esso non presenti errori. Potrebbe essere utile altrimenti compilare solo il progetto corrente).

<img src="images\modalita di compilazione.png" width="350" title="modalita di compilazione">

In xml ogni oggetto è chiuso all’interno di un tag di apertura ed uno di chiusura. All’interno di ogni tag (tra le due parentesi acute) vi sono gli attributi.

E’ possibile vedere in modo specifico le proprietà di un attributo cliccando sul pulsante della chiave inglese e selezionando l’attributo. Le proprietà hanno un tipo che ha lo stesso nome del tag.

Disponi per Nome->WindowStartupLocation->Center of screen (posiziona l’intera finestra al centro dello schermo, disponi per nome ordina le proprietà in base al loro nome in ordine alfabetico).

La sintassi degli attributi di sml è molto precisa, presenta una struttura semplice del tipo nome=”valore”.

## 02/03/2023
## INTRODUZIONE AI SEMAFORI

Due metodi che incrementano un valore (for) contenuto in una TextBox. Sono indipendenti l’uno dall’altro.

A un certo punto uno dei due contatori si blocca, mentre l’altro riesce a terminare il conteggio.

Abbiamo constatato che non si tratta di un problema di conteggio in quanto uno dei due threads riesce a completare l’azione.

E’ necessario un meccanismo che faccia partire i due contatori per poi eseguire un controllo (semaforo) che entrambi abbiano finito e passare alle prossime operazioni.

Il problema è quindi il fatto che il programma si ferma troppo presto quindi uno dei due processi non riesce a terminare l’operazione.

Caratteristiche dei semafori:

- intero che non può essere negativo
- ha due funzioni principali: signal (incremento di 1 del contatore), wait (aspetta che il contatore diventi 0). Wait è una procedura bloccante in quanto lascia procedere l’esecuzione solo quando le altre operazioni sono completate.

CountdownEvent => semaforo

```
CountdownEvent semaforo = new CountdownEvent (2);
```

Abbiamo inizializzato il semaforo fuori dai metodi per poi chiamare il costruttore all’interno del metodo richiamato quando si preme il bottone.

Prima si crea il semaforo con valore 2 per poi bloccarsi => semaforo.Wait();

Abbiamo inoltre introdotto un MessageBox dopo il Wait che indicasse la presenza o meno di eventuali problemi.

Quando i due metodi incrementa finiscono la loro esecuzione, lo segnalano al semaforo.

Abbiamo notato che l'esecuzione dell’intero programma si bloccava, abbiamo perciò proceduto alla comprensione del punto in cui ciò è avvenuto.

Non è possibile utilizzare liberamente un semaforo all’interno di un metodo richiamato a un evento => anche per il semaforo bisogna creare un nuovo thread.

Creazione thread semaforo:

```
private void attendi(){
	semaforo.Wait();
}
```

Per creare un nuovo thread si deve creare un nuovo metodo, siccome gli altri metodi (thread) non ritornano alcun valore (void), bisogna trovare un modo per farli comunicare tra di loro.
Dispatcher.Invoke => aspetta (wait), quando i thread hanno fatto Signal (hanno segnalato il termine della loro esecuzione) aggiorna i due contatori.

```
private void attendi(){
	semaforo.Wait();
	Dispatcher.Invoke(
		() =>
		{
			Message.Show(“Finito!!”);
			lbl.Counter1.Text = _counter.ToString();
			lbl.Counter2.Text = _counter.ToString();
		}
	)
}
Dispatcher.Invoke è un metodo, tutto ciò che c’è dentro è un parametro.
() => { … }	=> lambda expression


Thread thread = new Thread (
	() =>			//passaggio di una lambda: il thread3 si deve 
inizializzare con il codice seguente poi può partire
	{
		semaforo.Wait();
		Dispatcher.Invoke(
			() =>
			{
				MessageBox.Show(“Finito!!”);
				lbl.Counter1.Text = _counter.ToString();
				lbl.Counter2.Text = _counter.ToString();
			}
		};
	}
thread3.Start();
}
```

A questo punto il metodo “attendi” non è più necessario, il vantaggio è la leggibilità del codice.

Cosa succede se si clicca un’altra volta il bottone?
Il semaforo usato in precedenza viene eliminato, quindi si generano errori. Il bottone non può essere cliccato più volte quando si arriva in fondo ai threads.
btnGo.IsEnabled = false;

Tramite questo comando il pulsante viene disabilitato momentaneamente. L’abbiamo scritto all’interno di Button_Click .

## 19/03/2023
## ProgressBar

VIsualizzare il conteggio progressivo dei vari threads tramite una ProgressBar.

```
<ProgressBar x:Name="prgBar1"></ProgressBar>
```

Assegnamo un nome alla ProgressBar Tramite x:Name.

```
<ProgressBar x:Name="prgBar1" Height="50" Width="1000" Minimum="0" Maximum="3000"></ProgressBar>
```

Si specifica la lunghezza, la lunghezza, il valore minimo e quello massimo della ProgressBar. Questa modifica si esegue lato xaml.

```
pegBar1.Maximum += (GIRI1 + GIRI2);	//lato C#
```

Il massimo valore che la ProgressBar può rappresentare si adatta al numero massimo di giri eseguiti da entrambi i thread (GIRI1 e GIRI2). Questo si scrive all’interno della funzione che viene richiamata quando si preme un bottone (evento).
Abbiamo notato che nonostante questo vincolo rimaneva comunque uno spazio vuoto, per questo lato xaml abbiamo aggiornato il massimo inizializzandolo a 0.

## 16/03/2023
## Aggiornamenti WPFThreads

Cartella zippata -> Proprietà -> Annulla blocco (i file che vengono dalla rete non vengono più bloccati come tali, questo può essere utile per evitare complicazioni con Visual Studio).
Aggiungiamo un thread agli altri due già esistenti, modifichiamo i tempi di esecuzione ed il conteggio che i vari thread eseguiranno.
Aggiungiamo poi un contatore che tenga conto del totale degli altri contatori ed una progress bar che permette di visualizzare l’andamento del totale stabilendo un massimo.
Prima di consegnare eliminiamo .vs , bin e obj.
