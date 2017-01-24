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

