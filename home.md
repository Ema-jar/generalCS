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

Un'aspetto importante legato ad una risorsa è il suo URI. Una risorsa deve aver associato
un URI. Una risorsa a cui non è associato almeno un URI non è una risorsa e, in genere,
non è nemmeno sul web.

### URI


