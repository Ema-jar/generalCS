# Concorrenza

La concorrenza è l'abilità di un programma di eseguire operazioni diverse in maniera parallela.
Questo risultato può essere raggiunto dividendo la computazione su più CPU oppure su più macchine
appartenenti allo stesso network.

## Threads vs processi
Un processo è un ambiente di esecuzione fornito dal sistema operativo e, in quanto tale, ha il suo insieme
di risorse (file aperti, memoria ecc ecc).
I thread invece vivono all'interno di un processo e ondividono tra loro le risorse del processo stesso.
Genericamente i thread sono utilizzati per eseguire compiti più piccoli, in cui le performance ricoprono
un ruolo fondamentale.

Entrando nel mondo Java un esempio di processo potrebbe essere la JVM in esecuzione. All'interno di
questo ambiente diversi thread possono essere creati e gestiti dalla JVM stessa.

In una qualsiasi applicazione Java esiste almeno un thread chiamato _main thread_. Quindi il numero minimo di
thread attivi in un'applicazione è 1.

È possibile accedere al thread corrente con il metodo statico `currentThread()` della classe Thread in questo 
modo: `Thread.currentThread()`.

## Stati di un thread

In Java un thread si trova sempre in uno di questi possibili sei stati:

* NEW -> thread creato ma non ancora avviato
* RUNNABLE -> thread in esecuzione nella JVM
* BLOCKED -> thread bloccato
* WAITING -> thread in attesa che un secondo thread esegua una particolare azione (rischio starvation)
* TIMED_WAITING -> come per WAITING ma il thread aspetta per un dato intervallo di tempo
* TERMINATED -> thread che ha terminato la sua esecuzione

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

## Stop di un thread


## Synchronize 


