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

