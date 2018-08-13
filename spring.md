# Spring

Qui sono contenute varie informazioni sul framework Spring.

## IoC container

Spring implementa l'Inversione di Controllo attraverso la Dependency Injection. Seguendo questo approccio gli oggetti (i beans) definiscono le loro dipendenze, cioè gli altri oggetti di cui hanno bisogno, e lasciano al container il compito di iniettare queste dipendenze al momento della creazione del bean. Questo processo è l'esatto _inverso_ della creazione delle dipendenze da parte degli oggetti che le usano, in questo modo si evita il new all'interno di una classe.

## Bean

Nel mondo di Spring compare spesso la parola Bean. Un bean è un'istanza di un oggetto gestita dal container di Spring. Immaginiamoli come una serie di oggetti creati e messi dentro il container per essere utilizzati ovunque nell'applicazione.

## BeanFactory e ApplicationContext

La BeanFactory è responsabile della gestione di tutti gli oggetti all'interno di un'applicazione Spring, quindi l'inversione di controllo viene messa in atto da questo modulo. In particolare la BeanFactory si occupa dell'istanziazione e del wiring dei vari oggetti. L'ApplicationContext estende la BeanFactory e ne aumenta le funzionalità, di fatto è la classe utilizzata maggiormente, a meno che non ci siano motivi seri per utilizzare la BeanFactory.

## Scope dei beans

I beans istanziati dal container hanno uno scope che può variare.

1. Singleton: ogni volta che chiediamo un bean al container questo ci restituisce sempre la stessa istanza.
2. Prototype: il container istanzia un nuovo bean ogni volta che lo richiediamo.
3. Request: il container istanzia un bean richiesto ad ogni richiesta HTTP.
4. Session: il container istanzia un bean richiesto ad ogni sessione HTTP.
5. Global: usato in applicazioni dotate di portlet container. Ogni portlet ha le sue sessioni, un bean global è disponibile in tutte queste sessioni.

Gli ultimi tre scope vengono definiti _web-aware_ perchè hanno un senso all'interno di un contesto web.

## Annotation comuni

 - @Component: annotazione generica per il generico componente di spring, il component equivale ad un Bean. Specializzazioni di Component sono Service e Repository.
 - @Service: rappresentano il livello di servizio, la parte dell'applicazione in cui c'è logica.
 - @Repository: rappresentano i DAO, accedono direttamente al DB.
 - @Autowired: utilizzato per iniettare un component attraverso dependency injection.
 - @Qualifier: utilizzato per miglioarare il controllo che abbiamo sulla dipendency injection. Questa annotation è usata per scegliere quale componente iniettare nel caso in cui due o più beans fossero dello stesso tipo.
 - @ComponentScan: utilizzata per definire i package in cui effettuare uno scan dei componenti che devono essere messi nel contesto.
 - @Configuration: utilizzata per indicare che la classe annotata contiene dei metodi che definiscono dei bean tramite annotazionen @Bean.
 - @Bean: è utilizzata per definire diversi dipi di Bean, lavora a livello di metodo. Quando l'applicazione viene avviata il risultato del metodo viene salvato in una BeanFactory e può essere usato in seguito.
 - @Lazy: solitamente quando in un componente ci sono dipendenze iniettate tramite autowire queste vengono create e configurate all'avvio. Se usiamo questa annotation facciamo in modo di instanziare questi oggetti sollo la prima volta che vengono richiesti.
 - @Transactional: questa annotazione deve essere prima configurata utilizzando @EnableTransactionManagement. A questo punto la classe o il metodo annotati supporteranno configurazioni come ad esempio quella di rollback o di timeout per la transazione. Importante notare che il rollback avverrà solo in caso di eccezioni unchecked.



