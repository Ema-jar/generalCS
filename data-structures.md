# Strutture dati

In questa sezione verranno descritte le più comuni strutture dati utilizzabili, sottolineando i tempi
di inserimento, rimozione e modifica.

### Set, list and map

Tra le strutture dati più comuni vale la penadistinguere tre macrocategorie:

* i set
* le liste
* le mappe

Ogni struttura dati ha delle caratteristiche particolari in base all'ordine degli elementi contenuti,
alle modalità d'accesso, alle operazioni di inserimento e rimozione e alla presenza o meno di
duplicati.

I *Set* vengono utilizzati per rappresentare un insieme, non ammettono duplicati e l'ordine degli
elementi non viene garantito. I set non permettono un accesso posizionale ai loro elementi proprio 
a causa della mancanza di un ordinamento interno. 
All'interno del mondo Java `Set` è un'interfaccia che estende `Collection`.

> Esempi di Set sono i TreeSet e gli HashSet.


Le *List* vengono utilizzate per rappresentare una sequenza di elementi (duplicati o no) che seguono 
un dato ordine. Uno dei vantaggi delle liste sta nella possiblità di accedere agli elementi in maniera 
posizionale, questa caratteristica è data proprio dall'ordinamento interno della lista.
Nel mondo Java `List` è un'interfaccia che estende `Collection`.

> Esempi di List sono gli ArrayList e le LinkedList.

Le *Map* sono delle strutture dati che differiscono da quelle precedentemente descritte. La principale
differenza sta nel fatto che le mappe contengono coppie chiave-valore e non singoli oggetti.
All'interno di una mappa le chiavi non possono essere duplicate, inoltre questa struttura non 
mantiene un ordinamento interno, quindi non è possibile accedere agli elementi in maniera posizionale come
per le liste.
Un altro aspetto caratteristico delle mappe è che, nel mondo Java, `Map` è un'interfaccia che
non implementa `Collection`, quindi le mappe non sono parte della _Collection API_.

> Esempi di Map sono le HashMap e i TreeMap.

La scelta di una struttura dati rispetto ad un'altra dipende da molti fattori, come ad esempio:

* la velocità di inserimento
* la velocità di ricerca
* la velocità di rimozione

Un altro aspetto da considerare nello scegliere una struttura è l'utilizzo che se ne dovrà fare.
Se volessimo raggruppare una lista di colori un set sarebbe una scelta perfetta poichè non ammette
duplicati. 
Se volessimo creare una rubrica telefonica le coppie (nome, numero di telefono) sarebbero perfettamente
rappresentate da un'hash map.

### LinkedList vs ArrayList

`ArrayList` e `LinkedList` sono due implementazioni differenti della stessa interfaccia `List`: una LinkedList 
è implementata come una lista di oggetti, ognuno avente un riferimento al successivo e uno al 
precedente, un'ArrayList è invece implementato su un array.

Un elemento in una LinkedList non può essere indirizzato in tempo costante poichè una lista del genere non è
posizionale. Ne consegue che l'inserimento o la rimozione hanno un tempo di esecuzione di O(n) nel caso
peggiore.
L'inserimento in prima posizione è invece molto vantaggioso e può essere eseguito in tempo costante.

Discorso differente per un'ArrayList, questa struttura è basata su un array, quindi le operazioni su di essa
rispettano le stesse regole.
Inserire un elemento provoca lo shift di tutti i seguenti, questo ha un costo di O(n) nel caso in 
cui l'elemento sia inserito in prima posizione.
D'altro canto è possibile estrarre un'elemento contenuto in una specifica posizione in tempo costante 
utilizzando semplicemente l'indice dell'elemento, come per un array.

In genere possiamo dire che vale la pena utilizzare una LinkedList quando vogliamo una struttura dati dinamica, 
capace di performare bene a fronte di molti inserimenti e rimozioni, il metodo `add(E elem)` infatti viene 
eseguito in tempo costante grazie ad un puntatore sulla coda.

Un'ArrayList risulta essere la scelta migliore quando il quantitativo di dati inseriti nella struttura non
cambia di frequente e, soprattutto, quando abbiamo bisogno di accedere ad alcuni valori in base alla loro
posizione.