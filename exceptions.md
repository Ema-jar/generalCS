# Eccezioni

Le eccezioni sono utilizzate per bloccare il normale flusso di un programma a causa di un evento _eccezionale_.

Il seguente codice mostra un utilizzo totalement eerrato del concetto di eccezione e, di conseguenza, andrebbe evitato:
```
try{
    int i = 0;
    while(true){
        range[i++].doSomething();
    }
} catch(ArrayIndexOutOfBound e){
    // catch vuoto...
} 
```

Come si può notare si vuole ciclare su un array utilizzando un'eccezione per determinare il raggiungimento dell'ultimo elemento.
Il precedente approccio è errato per i seguenti motivi:

1. le eccezioni sono pensate per descrivere eventi effettivamente eccezionali, quindi non dovrebbero mai essere usate per controllare il flusso di esecuzione di un programma
2. l'ìutilizzo di un blocco `try/catch` blocca la JVM dall'effettuare molte ottimizzazioni 

## Eccezioni checked vs unchecked

Le eccezioni lanciate implementano l'interfaccia `Throwable` e sono di tre tipi: `checked exception`, `runtime exception` e `error`.

Le prime descrivono una condizione dalla quale ci si aspetta una ripresa del programma a seguito della gestione della stessa. Utilizzando queste eccezioni forziamo lo sviluppatore ad utilizzare un blocco cartc oppure a dichiarare che il metodo lancia un'eccezione del genere e lasciare che questa si propaghi. Il classico esempio è la `FileNotFoundException` che indica un errore relativo all'accesso ad un file non più esistente. In questo caso l'errore potrebbe non dipendere dallo sviluppatore (il file potrebbe essere stato rimosso da altri), quindi è necessario gestire l'eccezione.

Le runtime exceptions descrivono degli errori commessi dagli sviluppatori e, solitamente, riguardano la violazione delle precondizioni di un metodo. Un esempio classico è la `NullPointerException` che indica un errore commesso dallo sviluppatore che sta tentando di accedere ad un metodo di un oggetto avente valore null. Queste eccezioni possono essere evitate effettuando dei controlli prima di interagire con un oggetto o con un metodo. Anche se non esplicitamente richiesto dalla JLS, le eccezioni di tipo unchecked dovrebbero estendere sempre `RuntimeException`, direttamente o indirettamente. 

Gli errori infine sono solitamente utilizzati dalla JVM per indicare problemi relativi alla mancanza di risorse o altre condizioni che impediscono il proseguimento del flusso, è quindi buona norma non implementare nessuna sottoclasse di `Error`.

## Traduzione di eccezione

È fondamentale che l'eccezione lanciata sia appropriata al livello di astrazione in cui viene sollevata.
In parole povere un'eccezione lanciata a basso livello dovrebbe essere _tradotta_ mano a mano che sale fino ad arrivare ai livelli più alti.

Un esempio di questo approccio può essere trovato nella classe AbstractSequentialList, che implementa l'interfaccia List.

```
public E get(int index){
    ListIterator<E> i = listIterator(index);

    try{
        return i.next();
    } catch(NoSuchElementException e){
        throw new IndexOutOfBoundException("Index: " + index);
    }
} 
```

IN questo caso la `NoSuchElementException` indica che non ci sono più elementi, quindi che la lista è stata completamente scansita. Questa eccezione viene tradotta in una `IndexOutOfBoundException`, più esplicativa in relazione al contesto del metodo get chiamato.

È anche possibile possibile _wrappare_ l'eccezione lanciata a livello più basso all'interno di quella lanciata a livello più alto. In questo modo lo stack trace viene mantenuto.

```
try{
    //...
} catch(LowerLevelException cause){
    throw new HigherLevelException(cause);
}
``` 

## Atomicità

Quando viene lanciata un'eccezione di tipo checked ci si aspetta che lo sviluppatore gestisca la problematica, è quindi utile che l'oggetto che ha scatenato il problema rimanga in uno stato consistente, come era prima dell'eccezione.

Per ottenere questo risultato possono essere seguiti diversi approcci:

1. Utilizzare oggetti immutabili, in questo modo l'atomicità è assicurata senza sforzi.
2. Effettuare il controllo che potrebbe sollevare l'eccezione _prima_ di eseguire l'operazione.
3. Effettuare le operazioni su una copia temporanea dell'oggetto, che verrà sostituita con l'originale solo quando l'operazione sarà stata eseguita senza problemi.

## Ignorare ecezioni

A volte capita di vedere codice del genere:
```
try{
    //...
} catch(SomeException e){
    // do nothing :( 
}
```

Ignorare un'eccezione è una pratica da evitare sempre. Anche quando non c'è nulla da fare all'interno di un blocco catch, vale comunque la pena loggare delle informazioni che segnalino allo sviluppatore un comportamento anomalo.
Ignorare un'eccezine è come sentire l'allarme antincendio e spegnerlo senza fare nulla, il risultato potrebbe essere disastroso e il nostro programma potrebbe fallire in un altro punto a causa dell'eccezione non gestita, rendendo difficilissimo il corretto debug.
