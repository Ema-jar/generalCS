# Java

## JVM
La Java Virtual Machine è il componente della piattaforma Java che esegue i programmi
tradotti in bytecode durante la fase di compilazione.

L'architettura della JVM è basata su sei componenti principali:

* Un set di istruzioni dedicate ai bytecode
* Un gruppo di registri
* Uno Stack
* Un'area detta heap su cui agisce il garbage collector
* Un'area per la memorizzazione dei metodi

La JVM non è indipendente dalla piattaforma, quindi esiste una JVM per Windows, una per
Linux, OSX e via dicendo. L'aspetto fondamentale è che il codice compilato e trasformato in
bytecode può essere eseguito usando una qualsiasi macchina virtuale.
Quindi la JVM sta alla base della portabilità del linguaggio Java, un software scritto in Java
può essere eseguito su qualsiasi macchina, a patto di avere una JVM compatibile con quella macchina.

## Classi e oggetti
Java è un linguaggio di programmazione a oggetti quindi i vari concetti espressi all'interno del
software sono mappati in oggetti in grado di interagire tra loro.

La definizione dei tipi di dati in un contesto OOP è delegata alle classi. Una classe 
può essere vista come uno stampo da cui è possibile ricavare un oggetto in seguito ad una procedura
chiamata instanziazione.

Gli oggetti, dal canto loro, altro non sono che la concretizzazione di una classe.

Le classi definiscono al loro interno un insieme di attributi e di metodi che hanno il compito 
di descrivere la classe stessa.
Ipotiziamo ad esempio di voler definire una classe Persona che avrà il compito di rappresentare 
degli esseri umani. Una possibile implementazione di tale classe è la seguente.

```
public class Persona{

	private String name;
	private String lastname;
	
	public void sayYourName(){
		System.out.println(name);
	}

}
```

La classe Persona definisce una categoria, tutti gli oggetti di tipo persona, seppur diversi tra loro,
saranno caratterizzati dagli attributi di tale categoria e avranno i metodi definiti nella classe.
In parole povere, ogni Persona avrà un nome, un cognome e potrà usare il metodo sayYourName.

Un oggetto viene creato a partire da una classe in questo modo:

```
Persona p = new Persona();
```

Dietro questa, apparentemente, semplice operazione si nascondono tre diverse azioni estremamente importanti:

1. Creazione dell'oggetto Persona all'interno dell'heap tramite la parola chiave `new`
2. Creazione del riferimento ad un oggetto di tipo Persona
3. Unione del riferimento all'oggeto utilizzando l'operatore di uguaglianza `=`

Da questo momento l'oggetto esiste nell'heap e rimarrà presente in questo spazio di memoria
fintanto che esisterà un riferimento verso di esso. Quando non ci sarà più nessun riferimento associato
a questa istanza allora il garbage collector potrà eliminarlo.

## Modificatori

In Java classi, variabili e metodi possono essere dotati di modificatori d'accesso:

* public
* protected
* package
* private 

Queste parole chiavi vengono utilizzate per definire lo scope dell'elemento a cui fanno riferimento.
Public indica che l'elemento è accessibile pubblicamente, ovunque all'interno del programma.
Private significa che l'elemento è accessibile solo all'interno di una classe.
Protected permette l'accesso dell'elemento solo da parte delle sottoclassi, indipendentemente dal package 
di appartenenza.
Package indica che l'elemento è accessibile all'interno di un dato package.
La seguente tabella è da tenere a mente:

 Modifier | Class | Package            | Subclass                | World
|-------- | ----- | ------------------ | ----------------------- | -----
public    |  yes  |        yes         |           yes           |  yes 
protected |  yes  |        yes         |           yes           |  no 
package   |  yes  |        yes         |           no            |  no 
private   |  yes  |        no          |           no            |  no 


## Final & Static

Oltre ai modificatori precedentemente descritti, variabili, metodi e classi possono essere 
definiti usando le parole chiave `final` oppure `static`.

Una variabile definita come final è una variabile che, una volta inizializzata, non potrà
più cambiare valore.
Quindi il seguente codice genera un errore al momento della compilazione perché si sta provando a settare
il valore della variabile `name` definita come final e già inizializzata.

```
public class Persona{

	private String name = "Emanuele";
	
	public void setName(String newName){
		this.name = newName;
	}
}
```

Un metodo final è un metodo che non può essere ridefinito da una sottoclasse.

Una classe final è una classe che non può essere estesa da nessuna altra classe. `java.lang.String`
è un esempio di classe final, infatti in Java il seguente codice genera un errore al momento
della compilazione.

```
public class customString extends String{
	//do something
}
```

Anche `static` è una parola chiave utilizzata nel mondo Java in associazione con una variabile o un 
metodo.

Una variabile statica è una variabile che appartiene alla classe e non all'oggetto. Questo significa che
tutti gli oggetti instanziati da quella classe faranno riferimento a questa variabile condividendola.
Questa è la principale differenza tra variabili statiche e variabili d'istanza: abbiamo una variabile statica
_per ogni classe_ mentre abbiamo una variabile d'istanza _per ogni oggetto_ di quella classe.
Le variabili statiche sono inizializzate quando la classe è caricata.

> Una classe viene caricata quando la JVM decide che è arrivato il momento di caricarla, genericamente
> questo accade quando qualcuno prova a creare una nuova istanza della classe oppure quando ne
> viene utilizzato un metodo statico.

Un metodo statico è un metodo il cui comportamento non dipende da una specifica istanza di una classe. 
Pensiamo ad esempio al metodo `sqrt(double d)` della classe `java.lang.Math`. Questo metodno eleva al quadrato un valore 
passato come input e, per effettuare questa operazione, non ha bisogno di nessuna variabile di istanza
della classe Math, il metodo agisce solo sull'argomento.
Se per effettuare un'operazione del genere avessimo bisogno in istanziare ogni volta un oggetto della 
classe Math, questo ci porterebbe ad avere moltissimi oggetti inutili nel nostro heap.
Java previene questo inutile spreco di spazio utilizzando metodi statici. Un metodo può essere utilizzato
senza che ci sia bisogno di un effettivo oggetto, con un conseguente risparmio di memoria.
Solitamente i metodi static vengono utilizzati per fornire delle funzionalità generiche come, appunto, 
quelle fornite dalla classe Math.

Per invocare un metodo statico bisogna specificare a quale classe è associato tale metodo con la seguente
sintassi:

`Math.sqrt(12.3)`

Ovviamente all'interno di un metodo statico non si possono utilizzare variabili appartenenti alla classe
che contiene il metodo. Questo perchè, come detto prima, un metodo statico è accessibile senza che ci sia
un oggetto nell'heap. Il seguente codice quindi genera un errore in fase di compilazione.

```
public class Person{
	private String name;

	public static void printName(){
		System.out.println("The name is: " + name);
	}
}
```

Il metodo `printName()` è usato chiamandolo su una _classe_ e non su un _riferimento_ ad uno specifico oggetto. 
Quindi il metodo statico non sa _quale_ variabile d'istanza utilizzare per ottenere il valore di `name`.
Stesso identico discorso per i metodi: un metodo statico non può richiamare al suo interno un metodo _non_
statico.

## Classi statiche

In Java non è possibile definire una classe _top-level_ come statica. Al massimo è possibile definire la 
classe come final, fornire un costruttore privato e implementare solo metodi statici al suo interno.
Questa è esattamente l'idea seguita da molte classi di utilità, come, appunto, la classe Math.

Nel caso di una classe interna possiamo invece utilizzare la parola chiave static, come mostrato di 
seguito.

```
public class A{
	public static class B{
	}
}
```
In questo modo la classe statica interna può essere istanziata senza un'istanza di quella esterna, 
nel classico modo: `B b = new B();`.

Nel caso in cui _B_ non fosse dichiarata statica potremmo istanziarla solo attraverso un'istanza
di _A_, in questo modo: 
`B b = a.new B();`


## Java 8

La versione 1.8 della JDK introduce molte novità e porta Java su un piano totalmente differente introducendo
concetti vicini alla programmazione funzionale.

### Lambda e interfacce funzionali

Le lambda sono funzioni anonime utilizzabili seguendo questo tipo di dichiarazione:

`(parametri della funzione) -> {corpo della funzione}`

Volendo concretizzare il precedente pseudocodice possiamo scrivere questa funzione:

`(String myString) -> System.out.println(myString)`

Le lambda sono innanzitutto funzioni, quindi non devono essere per forza associate ad un oggetto, sono
anonime, quindi possono essere dichiarate senza un nome che le identifichi e, soprattutto, possono
essere passate come parametro di una metodo.

Le lambda sono fortemente legate ad un'altro concetto introdotto da Java 8, quello delle interfacce 
funzionali.
Un'interfaccia funzionale altro non è che un'interfaccia che definisce un singolo metodo. Un classico
esempio di interfaccia funzionale è `Runnable`, largamente utilizzata per definire un thread.

Runnable definisce un singolo metodo `run()`, proprio per questo può essere sostituita da una lambda come 
mostrato di seguito.

*JDK 1.7*

```
Runnable task = new Runnable(){
	@Override
	public void run(){
		System.out.println("I'm running!!!");
	}
};

Thread t = new Thread(task);
t.start();
```

*JDK 1.8*

```
Runnable task = () -> { System.out.println("I'm running!!!"); }

Thread t = new Thread(task);
t.start();
```

Come si può notare il codice scritto diminuisce di molto.

### Streams

La stream API nasce con lo scopo di rendere l'iterazione sulle collectionpiù facile e veloce.
Uno stream, una volta legato ad una collection, fornisce diversi metodi che possono essere eseguiti
sui membri della collection stessa.

Per comprendere meglio questa nuova API proviamo a calcolare la somma dei punti di un insieme di tasks
aperti.

```
final long totalPointsOfOpenTasks = tasks
    .stream()
    .filter( task -> task.getStatus() == Status.OPEN )
    .mapToInt( Task::getPoints )
    .sum();
```

Vediamo cosa succede passo dopo passo:

1. agganciamo uno stream all'array dei tasks
2. filtriamo tutti i task aperti tramite il metodo `filter`
3. mappiamo ogni task aperto sul suo punteggio tramite `mapToInt`
4. sommiamo i punteggi con `sum`

Le operazioni sugli stream si dividono in _intermedie_ e _finali_. Le operazioni intermedie ritornano un 
nuovo stream, quelle finali attraversano lo stream per produrre un risultato finale. 
Filter è un'intermedia, sum è una finale.

> Le operazioni intermedie sono _lazy_, quindi non attraversano mai lo stream. 
> Filter ad esempio non attraversa lo stream effettuando un filtro ma crea un nuovo stream che, 
> una volta attraversato, conterrà solo gli elementi specificati dal predicato.
> Le operazioni finali invece, attraversano lo stream producendo un risultato o un side effect.

 









