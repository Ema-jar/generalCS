# Code

Code e brokers vengono utilizzati per implementare una comunicazione asincrona tra micro servizi. La meccanica è abbastanza semplice: un produttore produce un messaggio che viene consumato da uno o più consumatori.

## Pub Sub

Si tratta di un'architettura di scambio di messaggi basata su un produttore che, invece di inviare direttamente un messaggio ad un consumatore, lo mette su un buffer e lascia agli eventuali consumatori il compito di leggere i pacchetto inviato.

Il filtraggio di questi messaggi da parte di un consumatore può essere basato su un topic o sul contenuto del messaggio.

Nel caso di topic il consumatore si registrerà ad un determinato flusso (topic) e consumerà tutti i messaggi da questo. Il produttore definisce i topic.

Nel caso di filtro basato sul contenuto avremmo una consegna del messaggio basata da un filtro definito dal consumatore.

I vantaggi riguardano la separazione del produttore e del consumatore. In un'architettura del genere non c'è un forte legame tra questi due moduli, questo favorisce un'architettura a micro servizi.

La scalabilità è un altro aspetto avvantaggiato da questa organizzazione. Per aumentare il numero di messaggi processati si può incrementare il numero di consumatori.

## Kafka

Kafka è uno dei più famosi broker utilizzati per scambiare messaggi tra produttore e consumatore.
I concetti su cui è basato sono i seguenti:

 - Topic: tutti i messaggi per Kafka sono organizzati in topic. Il produttore invia i messaggi in un topic e il consumatore legge i messaggi da questo topic.
 - Partizione: un topic è diviso in diverse partizioni e diverse partizioni possono essere messe su diverse macchine in modo da essere letto da un diverso consumer.
 - Broker: un broker è un nodo del cluster, quindi una macchina su cui mettiamo una partizione.

Per riassumere possiamo dire che:
 1. Un cluster Kafka è formato da uno o più broker (i server su cui gira Kafka).
 2. Ogni Broker può avere più Topic.
 3. Ogni topic è suddiviso in più partizioni.
 4. Ogni partizione può essere messa su una singola macchina in modo da permettere una lettura parallela da uno stesso topic.

Il consumatore o i consumatori che leggono i messaggi da una partizione fanno parte di un _consumer group_. 

Per scalare la nostra architettura dobbiamo aumentare il numero di partizioni all'interno di un topic e, per ogni partizione, dobbiamo assegnare un diverso consumatore.

__IMPORTANTE:__ il numero di partizioni deve essere uguale al numero di consumatori nel consumer group altrimenti un consumatore rimarrà in attesa senza consumare nessun messaggio.

Più informazioni su questo argomento possono essere trovate [qui](https://stackoverflow.com/a/57643635/2649618).

Un altro aspetto molto importante riguarda gli offset. Ogni messaggio in una partizione è identificato da un offset. L'offset ci permette di capire da quale messaggio dobbiamo riprendere la lettura.
Dopo essere stato processato un messaggio deve essere committato in modo da permettere a Kafka di avanzare di offset.

