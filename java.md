# Java

### JVM
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

### Classi e oggetti
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

### Modificatori

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

 









