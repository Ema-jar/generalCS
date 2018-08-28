# Sicurezza

La sicurezza informatica (information security) è l'insieme dei mezzi e delle tecnologie tesi alla protezione dei sistemi informatici in termini di disponibilità, confidenzialità e integrità dei beni o asset informatici; a questi tre parametri si tende attualmente ad aggiungere l'autenticità delle informazioni. 

## Triade CIA

Alla base della sicurezza informatica c'è il il processo continuo di garantire confidenzialità, integrità e disponibilità sui vari livelli di un sistema. Questa viene chiamata la Triade CIA.

L'**integrità** di un sistema riguarda tutti quegli aspetti che assicurano che un dato sia reale, accurato, consistente e non modificabile da coloro che non ne hanno permesso. In parole povere il principio di integrità assicura che un dato venga cambiato solo da coloro che hanno un'_autorizzazione_ per cambiarlo.

La _responsabilità_ (Accountability) di una modifica è un concetto fortemente legato all'integrità del dato. UN sistema deve essere in grado di sapere chi ha modificato cosa.

La _non negabilità_ (non-repudiation) è la capacità del sistema di generare log e informazioni in seguito alle modifiche effettuate in modo da rendere innegabile l'azione di un utente.

L'integrità può essere assicurata con l'aiuto di funzioni di hashing. Passando un dato all'interno di una funzione di hash possiamo capire immediatamente se è stata effettuata una modifica. Un classico utilizzo dell'hashing viene fatto durante il download degli aggiornamenti di un programma. L'hash di tale aggiornamento viene confrontata con quella originale per capire se qualche ente malevolo ha effettuato delle modiiche durante il download.

La **confidenzialità** di un sistema riguarda la capacità di mostrare i dati solo alle entità autorizzate a vederli.

Alla base di questo aspetto si trova il concetto di _autenticazione_. Un'entità deve essere nota al sistema prima di intervenire sui dati in esso contenuti.

Subito dopo l'autenticazione è la volta dell'_autorizzazione_. Un'entità autenticata potrebbe non avere diritto di interagire con una risorsa a causa di una mancata autorizzazione.

La crittografia entra in gioco quando i dati devono essere messi al sicuro da sguardi indiscreti. Utilizzare tecniche del genere permette di nascondere il contenuto dei dati a tutti coloro che non posseggono la chiave giusta per leggerli.

La **disponibilità** assicura che un sistema rimanga accessibile agli utenti autorizzati quano questi ne hanno bisogno.
Gli attacchi di tipo DoS o DDoS vanno a colpire proprio questo aspetto.

Il _disaster recovery_ è importantissimo per garantire la disponibilità di un sistema. I gestori devono essere in grado di ricostruire il contenuto di un sistema in caso di disastro umano o naturale.

Un altro aspetto fondamentale riguarda il _failover_, cioè la capacità di attivare una seconda istanza del sistema qualora la prima non dovesse essere disponibile.

La _resilienza_ è l'ultimo aspetto fondamentale. Durante un attacco il sistema deve essere in grado di reagire scalando oppure evitando dei single point of failure.

## Attacchi comuni

Un attacco informatico è una manovra attuata al fine di colpire sistemi informatici con lo scopo di renderli inutilizzabili, alterarne il comportamento oppure rubare le informazioni in essi contenute.

Per quanto riguard alo sviluppo web esistono un certo numero di buone pratiche da seguire per rendere la vita difficile ai malintenzionati che vogliono violare i nostri sistemi.

### SQL Injection

Questo è uno degli attacchi più comuni e si basa sull'inserimento di codice sql all'interno di un form. Nel caso in cui non si faccia escape sui dati inseriti si potrebbe incorrere in problemi.
Iptizziamo dia vere un codice del genere:
```
mysql_query("SELECT * FROM posts WHERE postid=$postid");
```

Se l'utente inserisse come postid il seguente codice sql: `10; DROP TABLE posts --;`, la query diventerebbe: `SELECT * FROM posts WHERE postid=10; DROP TABLE posts --`.
In questo modo l'attaccante riuscirebbe a cancellare la tabella post.

Un ottimo modo per far fronte a questo tipo di attacchi è utilizzare i prepared statement disponibili nei vari linguaggi. Un esempio in PHP, usando `PDO` potrebbe essere questo:
```
$stmt = $dbConnection->prepare('SELECT * FROM employees WHERE name = ?');
$stmt->bind_param('s', $name);
$stmt->execute();
```

## OAuth

OAuth (Open Authorization) è un protocollo aperto che permette di far sapere al _provider_ di una risorsa (ad esempio Facebook) che il _proprietario_ della risorsa (l'utente) ha dato i _permessi_ ad una _terza parte_ (ad esempio un'applicazione Facebook) di accedere alle sue _informazioni_ (ad esempio la lista degli amici).

Pensiamo alla situazione reale in cui abbiamo un account GMail e decidiamo di creare un account su Linkedin. Una volta creata l'utenza su Linkedin dovremmo andare ad inserire a mano tutti i nostri contatti che abbiamo sulla rubrica di GMail, facendo copia e incolla dei vari nomi o delle loro email.
Questo procedimento potrebbe scoraggiare l'utente a creare un account su Linkedin perchè è un'operazione lunga e noiosa.

Per questo motivo Linkedin ha avuto la brillante idea di sviluppare una funzionalità che aggiunge automaticamente tutti gli utenti presenti nella rubrica di GMail. Questa funzionalità è ottima e ci aiuta a velocizzare un processo noioso ma come possiamo comunicare questa informazione a Linkedin senza dover rivelare il nostro username e la nostra password di GMail? 

Proprio in questo momento entra in gioco OAuth. Se GMail supporta questo protocollo un'applicazione di terze parti potrebbe chiederci l'autorizzazione di accedere ai contatti in rubrica senza aver bisogno di nome utente e password.

OAuth mette a disposizione diverse funzionalità come:

 - Differenti livelli d'accesso: read-only vs read-write. Questo permette di avere una comunicazione bidirezionale. Quindi linkedin potrebbe essere autorizzato soltanto a leggere la nostra lista contatti oppure potrebbe avere la possibilità di aggiungere i nuovi collegamenti alla nostra rubrica.
 - Granularità di informazione: possiamo decidere per quali risorse garantire l'accesso (solo lista di contatti oppure anche date di nascita, nomi, email ecc ecc)
 
In parole povere OAuth è un framework in grado di fornire funzionalità di autorizzazine e autenticazione. Questo protocollo risulta essere particolarmente utile e flessibile nel mondo dei social in cui la condivisione delle informazioni deve essere flessibile, sicura e veloce al fine di garantire una buona user experience.


