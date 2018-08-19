# Rest

## Definizione
REST (REpresentational State Transfer) è un tipo di architettura software che usa il protocollo 
HTTP per la comunicazione.
Alla base di REST c'è l'idea secondo cui ogni componente è una risorsa acceduta usando metodi
standard dell'HTTP.
Ogni risorsa è identificata da un URI (Uniform Resource Identifier) ed è rappresentata usando 
diversi formati come JSON o XML.
È importante sottolineare che REST non è un protocollo ma uno stile architetturale, questa 
è una delle prime differenze rispetto a SOAP.

## Verbi
I verbi alla base di REST sono i seguenti:

* GET − fornisce accesso in sola lettura ad una risorsa
* PUT − permette di creare o sovrascrivere una risorsa
* DELETE − permette di rimuovere una risorsa
* POST − permette di creare una nuova risorsa
* PATCH − permette di modificare un sottoinsieme dei campi di una risorsa
* OPTION - permette di inviare un preflight
* HEAD - permette di ottenere una risorsa esattamente come farebbe una GET ma senza body, usata per esempio prima di richiedere una risorsa molto pesante

GET è un'operazione definita _sicura_ perché non modifica la risorsa verso la quale viene eseguita.
GET, PUT e DELETE sono operazioni dette _idempotenti_. Questo significa che il risultato è lo stesso
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

## PUT vs PATCH
Spesso non è chiara la differenza tra PUT e PATCH perchè entrambe le operazioni eseguono una
modifica su una risorsa.
Innanzitutto è richiesto che la PUT sia idempotente mentre la PATCH può anche non esserlo [[1]](https://stackoverflow.com/questions/28459418/rest-api-put-vs-patch-with-real-life-examples), per capire il motivo di questa affermazione dobbiamo fissare
due punti nell'utilizzo di questo verbo:

 * ci riferiamo ad una entità e non ad una collezione.
 * l'entità con cui facciamo la PUT è completa, quindi passiamo un oggetto che rappresenta 
 tutti i campi della risorsa.

Per capire meglio questo concetto immaginiamo di effettuare una GET come segue:

```
## /users/1

{
    "username": "skwee357",
    "email": "skwee357@domain.com"
}
```

Ora, se volessimo modificare solamente l'email di questa risorsa potremmo usare una PATCH o una PUT,
nel primo caso potremmo passare un oggetto che rappresenta solo le modifiche da effettuare sulla 
risorsa, nel secondo caso dovremmo passare un oggetto completo.

```
PATCH /users/1
{
    "email": "skwee357@gmail.com"       // new email address
}
```
```
PUT /users/1
{
    "username": "skwee357",
    "email": "skwee357@gmail.com"       // new email address
}
```
## PUT vs POST

Sia la PUT che la POST possono essere usate per creare una risorsa [[2]](https://stackoverflow.com/questions/630453/put-vs-post-in-rest), vediamo le differenze tra i due verbi.

Solitamente la POST è usata per creare una risorsa che non esiste o per modificare una risorsa esistente.
Nel primo caso avremo una cosa del genere: `POST /questions` nel secondo caso invece avremo: `POST /questions/<existing_question>`. Creare una nuova risorsa specificando l'id di quest'ultima è invece un errore, quindi questo andrebbe evitato: `POST /questions/<new_question>`.
La POST **non è** idempotente, potremmo assimilarla all'operazione di post-incremento `x++`.

La PUT invece è usata per creare una nuova risorsa specificando un url preciso oppure per sovrascrivere una risorsa esistente fornendone una nuova.
Ne consegue che una cosa del genere è corretta `PUT /questions/<new_question>` perchè cre una risorsa in un punto ben preciso specificat dall'url, stessa cosa vale per questa chiamata: `PUT /questions/<existing_question>`, usata per _sovrascrivere_ una risorsa esistente con una nuova.
La PUT **è idempotente**, potremmo assimilarla all'operazione di settaggio di una variabile, `x = 5`. 

## Web Services
I web services sono sistemi software basati su tecnologie open utilizzati principalmente
per scambiare dati tra applicazioni differenti.
In questo modo diverse applicazioni, scritte in linguaggi diversi, possono comunicare tra 
loro e scambiarsi dati.
I web services possono essere progettati utilizzando uno stile architetturale di tipo REST.

## Risorse
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

## URI
L'acronimo URI sta per Uniform Resource Identifier.
Come detto precedentemente un URI identifica una risorsa sul web. E ogni risorsa, per essere
considerata tale, deve avere almeno un URI che la identifica.
Nell'identificare una risorsa, un URI deve seguire delle regole.

La prima regola da seguire riguarda la descrizione delle risorse.
Gli uri devono descrivere la risorsa a cui si vuole accedere.
* http://www.example.com/sales/2004/Q4
* http://www.example.com/software/releases/latest.tar.gz
* http://www.example.com/bugs/by-state/open

Questi sono buoni esempi di URI che descrivono accuratamente la risorsa REST a cui 
si vuole accedere.
Leggendo il primo si comprende con facilità che si stanno cercando le vendite (sales) dell'anno 2004 (2004), relative al 
quarto trimestr (Q4).
Il secondo URI è usato per accedere all'ultima release di un software mentre il terzo URI
ci restituisce tutti i bug aperti.

È importante sottolinerare che gli URI non devono obbligatoriamente essere formattati
in questo modo ma è sicuramente una best practice da tenere bene in mente.
