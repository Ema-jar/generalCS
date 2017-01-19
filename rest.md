# Domande generiche per colloqui

## Rest

### Definizione
REST (REpresentational State Transfer) è un tipo di architettura software che usa il protocollo 
HTTP per la comunicazione.
Alla base di REST c'è l'idea secondo cui ogni componente è una risorsa acceduta usando metodi
standard dell'HTTP.
Ogni risorsa è identificata da un URI (Uniform Resource Identifier) ed è rappresentata usando 
diversi formati come JSON o XML.

### Metodi
I metodi alla base di REST sono i seguenti:

* GET − fornisce accesso in sola lettura ad una risorsa.
* PUT − permette di creare una nuova risorsa.
* DELETE − permette di rimuovere una risorsa.
* POST − permette di creare o aggiornar euna nuova risorsa.
* OPTIONS − fornisce operazioni di supporto su quella risorsa.

GET è un'operazione definita sicura perché non modifica la risorsa verso la quale viene fatta.
PUT e DELETE sono operazioni dette idempotenti. Questo significa che il risultato è lo stesso
indipendentemente dal numero richieste fatte verso la risorsa.
Se proviamo a usare PUT due volte su uno stesso oggetto la seconda volta questo non ha effetto.
Se proviamo ad effettuare una DELETE molteplici volte sulla stessa risorsa questa viene cancellata
quindi le successive richieste non avranno effetto.

Per comprendere meglio il concetto di sicurezza ed idempotenza pensiamo all'operazione matematica
moltiplicazione.
La moltiplicazione per 1 è sicura e idempotente:

4 x 1 x 1 x 1 = 1

Notiamo che il numero non viene cambiato indipendentemente dal numero di moltiplicazioni
fatte (sicurezza). L'operazione inoltre non modifica il valore del numero (idempotenza).
Volendo moltiplicare invece per 0 otterremo un'operazione idempotente ma non sicura:

4 x 0 x 0 x 0 = 0

Notiamo che alla prima applicazione il risultato diventa uguale a zero e questo non cambia 
indipendentemente dal numero di volte in cui la moltiplicazione viene applicata.

### Web Services
I web services sono sistemi software basati su tecnologie open utilizzati principalmente
per scambiare dati tra applicazioni differenti.
In questo modo diverse applicazioni, scritte in linguaggi diversi, possono comunicare tra 
loro e scambiarsi dati.
I web services possono essere progettati utilizzando uno stile architetturale di tipo REST.

### Risorse
In REST una risorsa è qualcosa abbastanza importante da essere referenziato da un link o su
cui si possono eseguire delle azioni.
Genericamente una risorsa è qualcosa che può essere salvato e collezionato in uno stream di bit
come ad esempio:
* un'immagine
* un articolo di un blog
* un valore restituito da un algoritmo
* una riga in un DB
* un file .txt

Un'aspetto importante legato ad una risorsa è il suo URI. Una risorsa deve aver associato
un URI. Una risorsa a cui non è associato almeno un URI non è una risorsa e, in genere,
non è nemmeno sul web.

### URI
L'acronimo URI sta per Uniform Resource Identifier.
Come detto precedentemente un URI identifica una risorsa sul web. E ogni risorsa, per essere
considerata tale, deve avere almeno un URI che la identifica.
Nell'identificare una risorsa, un URI deve seguire delle regole.

#### Descrizione
Gli uri devono descrivere la risorsa a cui si vuole accedere.
* http://www.example.com/sales/2004/Q4
* http://www.example.com/software/releases/latest.tar.gz
* http://www.example.com/bugs/by-state/open

Questi sono buoni esempi di URI che descrivono accuratamente la risorsa REST a cui 
si vuole accedere.
Leggendo il primo si comprende con facilità che si stanno cercando le vendite (sales) del 
quarto trimestr (Q4) dell'anno 2004 (2004).
Il secondo URI è usato per accedere all'ultima release di un software mentre il terzo URI
ci restituisce tutti i bug aperti.

È importante sottolinerare che gli URI non devono obbligatoriamente essere formattati
in questo modo ma è sicuramente una best practice da tenere bene in mente.



