# Concorrenza

La concorrenza è l'abilità di un programma di eseguire operazioni diverse in maniera parallela.
Questo risultato può essere raggiunto dividendo la computazione su più CPU oppure su più macchine
appartenenti allo stesso network.

## Threads vs processi

Un processo è un ambiente di esecuzione fornito dal sistema operativo e, in quanto tale, ha il suo insieme
di risorse (file aperti, memoria ecc ecc).
I thread invece vivono all'interno di un processo e condividono tra loro le risorse del processo stesso.
Genericamente i thread sono utilizzati per eseguire compiti più piccoli, in cui le performance ricoprono
un ruolo fondamentale.

Entrando nel mondo Java un esempio di processo potrebbe essere la JVM in esecuzione. All'interno di
questo ambiente diversi thread possono essere creati e gestiti dalla JVM stessa.

In una qualsiasi applicazione Java esiste almeno un thread chiamato _main thread_. Quindi il numero minimo di
thread attivi in un'applicazione è 1.

È possibile accedere al thread corrente con il metodo statico `currentThread()` della classe Thread in questo 
modo: `Thread.currentThread()`.

## Thread scheduling

Bisogna sottolineare che un vero e proprio multi threading si ha quando si lavora con un sistema 
dotato di più processori. In casi come questo ogni thread ha un processore dedicato e la computazione
può davvero essere eseguita in parallelo. Nel caso si abbia un solo processore la JVM _simula_ la programmazione multithread scambiando molto velocemente il thread attivo, dando quindi solo l'illusione di essere in un sistema parallelo.

Chiaramente pur avendo più processori una computazione _totalmente_ multi thread è impossibile poichè
i thread avviati sono decine e decine, quindi avremmo bisogno di macchine con un numero di processori
molto alto. Proprio per questo su ogni processore si alternano più thread diversi gestiti da uno
scheduler. nel mondo Java la JVM è dotata di un modulo chiamato _thread scheduler_, una componente il 
cui compito è proprio quello di decidere la sequenza di thread in base ai processori disponibili.

È importante sottolineare una cosa riguardo la schedulazione dei thread: Java non forza la JVM a 
schedulare i thread in un dato ordine. Questa scelta è presa totalmente dalla JVM e quindi dipende 
dalla macchina su cui ci troviamo.

## Creazione di un thread

In Java un thread può essere creato in due modi diversi.

**Estendendo la classe Thread**
```
public class MyThread extends Thread{

    public MyThread(String name){
        super(name);
    }

    public void run(){
        System.out.println("I'm a thread!");
    }

    public static void main(String[] args) throws InterruptedException {
		MyThread myThread = new MyThread("myThread");
		myThread.start();
	}
}
```

**Implementando l'interfaccia Runnable**
```
public class MyRunnable implements Runnable {

		public void run() {
			System.out.println("I'm a thread!!");
		}

		public static void main(String[] args) throws InterruptedException {
			Thread myThread = new Thread(new MyRunnable());
			myThread.start();
		}
	}
```

Implementare l'interfaccia Runnable è la via da preferire in questo caso poichè rende il codice più
generico. In questo modo posso creare thread passando un semplice oggetto che implementa Runnable, 
questo ci evita di specificare un comportamento per ogni classe che deve essere trattata come thread.
Inoltre, evitando di estendere la classe Thread, possiamo estendere un'altra classe e conferire le proprietà
richieste usando l'interfaccia Runnable (ricordiamo che in Java l'ereditarietà multipla non esiste).

## Stati di un thread

In Java un thread si trova sempre in uno di questi possibili sei stati:

* NEW -> thread creato ma non ancora avviato
* RUNNABLE -> thread in esecuzione nella JVM
* BLOCKED -> thread bloccato
* WAITING -> thread in attesa che un secondo thread esegua una particolare azione (rischio starvation)
* TIMED_WAITING -> come per WAITING ma il thread aspetta per un dato intervallo di tempo
* TERMINATED -> thread che ha terminato la sua esecuzione

Un thread appena creato si trova nello stato di NEW, significa che il thread esiste ma non è stato ancora
chiamato il metodo `start()` su questo thread. Una volta avviato, il thread passa nello stato di RUNNABLE, 
questo significa che è in esecuzione nella JVM. 
Un thread avviato può passare nello stato BLOCKED se sta aspettando che una risorsa si liberi, è il caso in 
cui il thread vuole entrare in un metodo definito synchronized oppure quando sul thread viene invocato il 
metodo `sleep()`.

## Stop di un thread

Stoppare un thread con il metodo `stop()` non è la scelta migliore perchè potrebbe lascire molte risorse
condivise in uno stato inconsistente.
Appena invocato il metodo di stop i monitor bloccati dal thread vengono rilasciati, se alcuni degli oggetti
protetti da questi monitor erano in uno stato inconsistente questi rimangono _danneggiati_ e si presenta
il problema.
Oggetti lasciati in uno stato inconsistente creano comportamenti inaspettati, andrebbero quindi evitati.

La soluzione migliore per stoppare un thread è quella di usare una variabile che viene controllata prima
dell'esecuzione del thread stesso e messa a false quando questo vuole essere stoppato come mostrato
in questo esempio.

```
public class Foo extends Thread {
  private volatile boolean close = false;
  public void run() {
    while(!close) {
      // do work
    }
  }
  public void close() {
    close = true;
    // interrupt here if needed
  }
}
```

In questo esempio usiamo una variabile `volatile` per assicurarci che il thread che gestisce il run accedano
sempre all'ultima versione aggiornata di tale variabile. Questo ci permette di avviare un thread e di 
stopparlo da un thread differente senza problemi.

## Executor Service

Le versioni moderne di Java utilizzano uno strumento chiamato Executor Service per gestire in modo ottimale lo scheduling di più thread.
Quindi, mentre un tempo l'utilizzo di Thread e Runnable era necessaro, ai giorni nostri sarebbe meglio ricorrere a questa interfaccia.

Il service ci mette a disposizione moltissimi metodi per aggiungere un runnable, per effettuare lo shutdown del gruppo di thread o per attendere che un particolare thread venga processato.

Quindi, se un tempo il meccanismo era basato sul Thread che eseguiva un Runnable, adesso è basato su un task che viene eseguito da un executor. I task sono di due tipo: Runnable e Callable (praticamente un runnable che ritorna un valore).

## Sincronizzazione e locking

È possibile utilizzare la parola chiave `synchronize` per evitare che due thread differenti accedano lo stesso
metodo nello stesso momento.
Questo significa che se un thread sta eseguendo un metodo sincronizzato, tutti gli altri thread che devono
invocare lo stesso metodo devono aspettare che il primo abbia terminato.
Questa funzionalità permette di evitare situazioni in cui due thread accedano simultaneamente alla stessa 
risorsa effettuando modifiche o letture che la renderebbero inconsistente. 

Ogni oggetto in Java ha un monitor, cioè un lock, una sorta di chiave d'accesso all'oggetto stesso.
Genericamente il monitor di un oggetto è sbloccato, quindi nessuno ha preso questa chiave d'accesso.
La situazione cambia quando un oggetto ha dei metodi sincronizzati, in questo caso, un thread che vuole 
usare _uno qualsiasi_ di questi metodi, deve bloccare il monitor.

In altre parole, quando un thread vuole accedere ad un metodo sincronizzato deve bloccare il monitor. Questo
impedisce ad altri thread di accedere a **tutti** i metodi sincronizzati dell'oggetto. Se un thread prova
ad acquisire un monitor bloccato da un altro thread, questo viene stoppato e si mette in attesa che il monitor
venga rilasciato dal thread che lo sta usando.

Il monitor di un oggetto viene acquisito quando un thread usa un metodo sincronizzato di questo oggetto e
viene rilasciato quando l'esecuzione di tale metodo termina con un return.

Un altro modo per ottenere la sincronizzazione di una risorsa non thread safe senza usare synchronize è l'utilizzo dei Thread Local.
Un thread local è un oggetto accessibile solo all'interno del thread in cui viene istanziato, quindi una variabile inserita in un thread local è sicuramente accessibile da un solo thread.

L'uso di un thread local andrebbe evitato nel caso in cui i thread vengano gestiti con un Executor Service perchè si potrebbe incorrere in un comportamento non deterministico.


## Deadlock e starvation

Il deadlock è un problema di sincronizzazione che si verifica quando due o più processi si bloccano in attesa che un altro rilasci una risorsa.

Ipotiziamo di avere due processi P1 e P2, P1 ha un lock sulla risorsa A e sta aspettando che la risorsa B si liberi per completare il suo task, P2 ha un lock sulla risorsa B e sta aspettando che la risorsa A si liberi per terminare il suo task. I due processi restano bloccati indefinitivamente. Per evitare un deadlock si possono attuare diverse tecniche basate sul forzare ogni processo a richiedere una sola risorsa per volta oppure facendo in modo che i processi richiedano le risorse in base ad una data gerarchia.

Un problema analogo è quello della starvation, cioè la _morte per fame_ di un processo che non riesce mai ad accedere ad una risorsa condivisa. Problemi di starvation possono colpire i processi a bassa priorità in un sistema a scheduler. Questi processi non riescono mai ad essere eseguiti perchè la CPU viene sempre _rubata_ da un processo dotato di priorità maggiore.

