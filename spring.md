# Spring

Qui sono contenute varie informazioni sul framework Spring.

## IoC container

Spring implementa l'Inversione di Controllo attraverso la Dependency Injection. Seguendo questo approccio gli oggetti (i beans) definiscono le loro dipendenze, cioè gli altri oggetti di cui hanno bisogno, e lasciano al container il compito di iniettare queste dipendenze al momento della creazione del bean. Questo processo è l'esatto _inverso_ della creazione delle dipendenze da parte degli oggetti che le usano, in questo modo si evita il new all'interno di una classe.

## Bean

Nel mondo di Spring compare spesso la parola Bean. Un bean è un'istanza di un oggetto gestita dal container di Spring. Immaginiamoli come una serie di oggetti creati e messi dentro il container per essere utilizzati ovunque nell'applicazione.

### BeanFactory e ApplicationContext

La BeanFactory è responsabile della gestione di tutti gli oggetti all'interno di un'applicazione Spring, quindi l'inversione di controllo viene messa in atto da questo modulo. In particolare la BeanFactory si occupa dell'istanziazione e del wiring dei vari oggetti. L'ApplicationContext estende la BeanFactory e ne aumenta le funzionalità, di fatto è la classe utilizzata maggiormente, a meno che non ci siano motivi seri per utilizzare la BeanFactory.

### Scope dei beans

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

## Hibernate

Hibernate è un ORM spesso utilizzato con Spring. [[1]](https://www.journaldev.com/3633/hibernate-interview-questions-and-answers)

I benefici di una libreria come Hibernate sono molteplici. Innanzitutto si crea un mapping tra il mondo dei database, notoriamente basato su relazioni con il mondo a oggetti presente in un contesto Java, inoltre Hibernate pone un livello tra l'applicazione e il database, in questo modo la tecnologia utilizzata per il salvataggio dei dati può essere cambiata senza che l'applicazione ne risenta.

Hibernate altro non è che un'implementazione della Java Persistance API (JPA) e può essere visto come un livello costruito sopra JDBC. 

Le funzionalità messe a disposizioni da Hibernate sono molteplici e aiutano lo sviluppatore nelle comuni operazioni di CRUD, nella creazione di query complesse o nel mapping tra oggetti java e tabelle del database.
Senza Hibernate dovremmo utilizzare JDBC, con tutti le difficoltà che questo comporta: 

 - molto boilerplate
 - gestione delle operazioni transazionali mancante
 - utilizzo di molti try/catch per gestire le SQLException, che sono catched exceptions
 - mancata possibilità di creazione delle tabelle sul db

Hibernate fa un uso massivo delle sessioni che rappresentano il mezzo di comunicazione tra l'applicazione e il database. Una sessione è un oggetto leggero che viene istanziato ogni volta che inadiamo ad interagire con un'entità mappata. La sessione non è thread safe e dovrebbe essere aperta e chiusa nel più breve tempo possibile.

Una volta aperta una sessine posso anche eseguire un'operazione transazionale come mostrato nel seguente esempio:

```
Session session = factory.openSession();
Transaction tx = null;

try {
   tx = session.beginTransaction();
   // do some work
   ...
   tx.commit();
}

catch (Exception e) {
   if (tx!=null) tx.rollback();
   e.printStackTrace(); 
} finally {
   session.close();
}
```

Possiamo notare che dentro una transazione può essere sollevata un'eccezione, in tal caso si fa rollback e la sessione viene chiusa.

## Spring data

Un altro progetto molto utilizzato all'interno dell'ecosistema Spring è spring data.

Sprig data mette a disposizione degli sviluppatori alcuni potenti mezzi per semplificare le query e le interazioni con il db. I repository ad esempio possono essere utilizzati per creare delle query basate sul nome dei metodi definiti in un'interfaccia che estende un repository, come nel seguente esempio:

```
interface UserRepository extends CrudRepository<User, Long> {

  long deleteByLastname(String lastname);

  List<User> removeByLastname(String lastname);
}

```

Ogni tipologia di repository fornisce funzionalità differenti come i CRUD oppure la possibilità di ottenere un risultato paginato o, ancora, ordinato.

I repository possono essere anche a notati con annotazioni atte a controllare la non-nullità del risultato. @NonNullable applicata ad un metodo lancia un'eccezione nel caso in cui il metodo restituisca un valore null.

Spring data fornisce allo sviluppatore l'opportunità di creare delle query utilizzando l'annotation @Query, questa annotation permette anche di eseguire le query scritte come query native.
Anche le stored procedure possono essere utilizzate grazie a Spring data, referenziando la procedura con l'annotation @Procedure.

Le query by example sono un altro mezzo molto potente per effettuare una query utilizzando come _esempio_ un oggetto dell'entità che si vuole ricercare.

Hibernate e Spring Data possono lavorare insieme semplificandosi i compiti a vicenda. Mentre Hibernate fornisce una mappatura diretta tra il db e l'applicazione sviluppata in OOP, Spring data aiuta nella creazione delle interfacce di DAO fornendo dei metodi utili per fare CRUD.
