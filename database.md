# Database

I database sono strutture che contengono e aggregano e connettono dati di diverse tipologie utilizzando tabelle, documenti o altri metodi.
Lo scopo dei database è quello di facilitare l'accesso a tali dati fornendo dei metodi di query.

## Database relazionali

### Chiave primaria

Nei database relazionali i dati sono contenuti all'interno di tabelle dette, appunto, _relazioni_.
Ogni tabella dovrebbe contenere una particolare colonna (o insieme di colonne) chiamata _chiave primaria_, la chiave primaria identifica univocamente una _tupla_ della tabella e deve rispettare alcune regole:

 - Una chiave primaria non può essere null
 - Una chiave primaria deve essere fornita quando la tupla viene inserita
 - Una chiave primaria deve essere più piccola possibile e deve contenere il minimo set di informazioni necessarie a renderla primaria
 - Una chiave primaria non dovrebbe essere mai cambiata perchè potrebbe essere coinvolta in altre relazioni

Una tabella dovrebbe _sempre_ avere una chiave primaria. La chiave primaria viene specificata in fase di creazione della tabella, all'interno del comando `CREATE`, una buona tecnica è fare in modo che la chiave primaria aumenti ad ogni inserimento, usando la parola chiave `AUTO_INCREMENT`.

### Chiave esterna

Una chiave esterna è una chiave che identifica univocamente una tupla appartenente ad un'altra tabella. Succede spesso che una tabella contenga più chiavi esterne come nel seguente esempio.

**Studente**         

| Id| Nome      | 
| - |---------- | 
| 1 | Marco     |
| 2 | Carlo     |
| 3 | Francesco |


**Voto**         

| Studente | Corso     | Voto | 
| -------- |---------- | ---- |
| 2        | MATH01    | A-   |
| 3        | CS101     | B+   |
| 1        | PRG32     | F    |

**Corso**         

| Id     | Nome           | 
| ------ |--------------- | 
| MATH01 | Matematica     |
| CS101  | Informatica    |
| PRG32  | Programmazione |

Possiamo notare che la tabella del voto contiene riferimento alle altre due tabelle e questo riferimento è basato proprio sulle colonne Id dello studente e del corso che, nelle rispettive tabelle, sono chiavi primarie.
Quindi Studente e Corso, all'interno della tabella Voto sono due chiavi esterne.

Possiamo dire che una chiave esterna è una colonna di una tabella che fa riferimento alla chiave primaria di un'altra tabella.

Ovviamente una chiave esterna può essere duplicata, cosa che spesso accade. Inoltre una chiave esterna può essere `null` mentre una chiave primaria non può. Una chiave esterna null indica che nessun riferimento è presente per quella tupla nella tabella padre. Questo comportamento, seppur previsto, non è considerato una best practice e può essere evitato imponendo un vincolo.

Creando un vincolo saremo obbligati ad inserire valori di chiavi esterne solo se questi sono presenti nella tabella padre, in questo modo non potremmo avere chiavi esterne null. Questo vincolo è chiamato _integrità referenziale_. Se violiamo un vincolo di integrità referenziale avremo un errore, in questo modo il collegamento tra tabella padre e tabella figlia è obbligatorio.
Un altro vantaggio dei vincoli riguarda l'eliminazione. Se elimino una riga ontenente una chiave primaria referenziata da una chiave esterna otterrò un errore. Per risolverlo dovrò _prima_ rimuovere la riga che referenzia la chiave primaria nella tabella figlia e _poi_ rimuovere la riga nella tabella padre.


### Operatori aggregati, group by e having

Gli operatori aggregati sono utilizzati per effettuare operazioni matematiche comuni su un set di valori identificati da una colonna. 
Gli operatori aggregati più comuni sono:

- AVG - media dei valori della colonna
- MIN - valore minimo della colonna
- MAX - valore massimo della colonna
- COUNT - conteggio delle righe non NULL della colonna
- SUM - somma dei valori della colonna

Abbiamo detto che un operatore aggregato lavora sulla colonna su cui viene richiamato. Ipotizziamo di avere una tabella che contiene tutte le vendite fatte dai nostri dipendenti per ogni componente prodotta:

**Sales**
|id|name     |sales|product  |
|--|---------|-----|---------|
|1 |emanuele |2    |ram      |
|2 |francesco|1    |cpu      |
|3 |antonio  |2    |hard disk|
|4 |francesco|4    |ram      |

Se eseguiamo la query:
```
SELECT SUM(sales)
FROM Sales
```

Otterremo il valore 9 dato dalla somma di _tutti i valori_ della colonna sales.

Ovviamente ci farebbe molto comodo poter effettuare una partizione del risultato per capire, ad esempio, quante unità di _ram_ sono state vendute in totale oppure quante unità sono state vendute da un dato dipendente perché vogliamo assegnare un premio produzione. Per fare questo ci viene in soccorso un altro operatore: il _group by_.

Il `GROUP BY` è un operatore che ci permette di suddividere una tabella raggruppandone le tuple che hanno lo stesso valore per una data colonna.

In questo modo se volessimo ottenere tutte le vendite fatte per ogni componente avremmo una query del genere:
```
SELECT product, SUM(sales)
FROM Sales
GROUP BY product
```

Il risultato sarà:

|product  |COUNT(sales)|
|---------|------------|
|ram      |6           |
|cpu      |1           |
|hard disk|2           |

Quindi la tabella viene prima divisa raggruppando le righe che hanno lo stesso valore espresso nel GROUP BY, poi l'operazione aggregata viene eseguita su queste porzioni, il risultato finale viene ricomposto in un'unica tabella, come mostrato precedentemente.

Ovviamente potremmo aver bisogno di estrarre solo la somma di tutte le vendite fatte per _un singolo_ componente, ad esempio le ram. In questo caso potremmo essere tentati di unilizzare l'operatore `WHERE` in una query del genere:
```
SELECT product, SUM(sales)
FROM Sales
GROUP BY product
WHERE product = 'ram'
```

Questo è un errore. Il campo product è il campo su cui è stata fatta la group by, quindi non possiamo usare il were in questi casi, l'operatore da utilizzare è `HAVING`, come mostrato di seguito:
esempio le ram. In questo caso potremmo essere tentati di unilizzare l'operatore `WHERE` in una query del genere:
```
SELECT product, SUM(sales)
FROM Sales
GROUP BY product
HAVING product = 'ram'
```

Il risultato sarà:

|product  |COUNT(sales)|
|---------|------------|
|ram      |6           |

### Join

I join ci permettono di incrociare le nostre tabelle e creare delle query su relazioni più ampie [[1](https://stackoverflow.com/questions/17759687/cross-join-vs-inner-join-in-sql-server-2008)].
Il concetto di join è fortemente legato a quello di prodotto cartesiano, consideriamo le seguenti due tabelle:

**Girl**
|girl_id|girl |toy_id|
|-------|-----|------|
|1      |Jane |3     |
|2      |Sally|4     |
|3      |Cindy|1     |

**Toy**
|toy_id|toy         |
|------|------------|
|1     |hula hop    |
|2     |balsa gilder|
|3     |toy soldier |
|4     |harmonica   |
|5     |cards       |

Il join più semplice è il `CROSS JOIN` e rappresenta il prodotto cartesiano tra i due insiemi, quindi come risultato avremo una tabella in cui ogni riga di _girl_ è associata a tutte le righe di _toy_. La query sarà una cosa del genere:
```
SELECT t.toy_id, g.girl_id
FROM toy AS t CROSS JOIN girl AS g
```

Il secondo join utilizzato è chiamato `INNER JOIN` ed è identico ad un cross join ma specifica una condizione su cui basare il join. In base a quanto detto la precedente query diventa:
```
SELECT t.toy_id, g.girl_id
FROM toy AS t INNER JOIN girl AS g ON t.toy_id = g.toy_id
```

Il risultato di questa query ci darà tutti i giocattoli posseduti dalle bambine presenti nel nostro db. Volendo ottenere tutti i giocattoli non associati ad una bambina basterà effettuare una query in cui avremo `ON t.toy_id <> g.toy_id`.
Un join che basa la query sull'uguaglianza è chiamato _equi join_, mentr uno che basa la query sulla non uguaglianza è chiamato _non-equi join_.

Il terzo tipo di join considerato è il `LEFT OUTER JOIN`. Un join di questo tipo confronta la tabella di _sinistra_ con quella di _destra_ riportando una riga per ogni riga della tabella di sinistra che ha un match sulla condizione specificata. Nel caso in cui la riga considerata non abbia nessun match nella tabella di destra, apparirà un null.

Per comprendere meglio utiliziamo le precedenti tabelle con una piccola modifica, la tabella toy diventerà:

**Toy**
|toy_id|toy         |
|------|------------|
|1     |hula hop    |
|2     |balsa gilder|
|4     |harmonica   |

Possiamo subito notare che il balsa gilder non è posseduto da nessuna bambina. Proviamo ad eseguire la seguente query:
```
SELECT t.toy, g.girl
FROM toy AS t LEFT OUTER JOIN girl AS g ON t.toy_id = g.toy_id
```

La query effettuerà questi passaggi:

 - considera la prima riga della tabella toy e cerca nella tabella girl una riga che abbia toy_id = 1, la trova, corrisponde a Cindy
 - considera la seconda riga della tabella toy e cerca nella tabella girl una riga con toy_id = 2, non la trova
 - considera la terza riga della tabella toy e cerca nella tabella girl una riga con toy_id = 4, la trova, corrisponde a Sally

 Il risultato sarà quindi una tabella così formata:

 |toy         |girl |
 |------------|-----|
 |hula hop    |Cindy|
 |balsa gilder|NULL |
 |harmonica   |Sally|

 Il balsa gilder non è associato a nessuna bambina, quindi la colonna girl è settata a NULL per quel giocattolo. Come possiamo vedere un join di questo tipo è comodo quando vogliamo trovare gli elementi della tabella di sinistra non associati con nessun elemento della tabella di destra.

 Ovviamente la tabella generata avrà un numero di righe almeno pari al numero di righe della tabella di sinistra e i potenziali null saranno presenti solo nella colonna presa dalla tabella di destra.

 Il `RIGHT OUTER JOIN` fa la stessa identica cosa del precedente solo che ha le tabelle invertite.


### Viste

Una vista è un oggetto definito da una query su cui è possibile effettuare delle ricerche.
Una vista può essere creata con il comando `CREATE VIEW` come nel seguente esempio:
```
CREATE VIEW PopularBooks AS
SELECT ISBN, Title, Author, PublishDate
FROM Books
WHERE IsPopular = 1
```

Una volta creata la vista questa può essere utilizzata come fosse una tabella del db. Il vantaggio di utilizzare una vista sta nella semplificazione che si ha su un db contenente molte tabelle. In questi casi la navigazione può risultare complicata, soprattutto se non si ha una buona dimestichezza con i join, una vista semplifica il tutto.
Bisogna considerare anche che una vista occupa poco spazio, quindi può essere una scelta vantaggiosa.

Gli svantaggi delle viste riguardano principalmente le performance. Ogni volta che facciamo riferimento ad una vista, la query che la genera viene lanciata di nuovo, inoltre la modifica ad una vista non è banale. Spesso le viste sono di sola lettura e abilitare altre operazioni potrebbe essere un procedimento difficile.

### Indicizzazione

Il processo di indicizzazine viene eseguito su una colonna di una tabella del db [[2](https://stackoverflow.com/questions/1108/how-does-database-indexing-work)]. Questo processo crea una seconda colonna contenente riferimenti a tutti i campi della prima. Su questa seconda colonna _d'appoggio_ possiamo effettuare un'ordinamento e, di conseguenza, effettuare una ricerca utilizzando una ricerca binaria.

Quindi un'indice è una struttura dati che semplifica le ricerche effettuate su una data colonna al fine di evitare quelle che sono chiamate _full table scan_. Solitamente le strutture dati utilizzate sono i b-tree con cui è possibile eseguire operazioni di insert, delete e update in tempo logaritmico.

L'indicizzazione diventa fondamentale quando una tabella cresce di dimensioni e le query su queta diventano lente  a causa di una ricerca sequenziale.

Ovviamente gli svantaggi degli indici riguardano tutte quelle query che non sono basate su un'uguaglianza. Ipotizioamo di voler trovare tutti i dipendenti che hanno meno di 40 anni, in questo caso un indice non aiuta.
Bisogna anche tener conto che ogni volta che facciamo un update o una insert sulla tabella indicizzata l'indice deve essere aggiornato per contenere il nuovo valore, inoltre all'aumentare della grandezza della tabella aumenta anche la dimensione dell'indice, questo può essere un problema a livello di spazio. 
