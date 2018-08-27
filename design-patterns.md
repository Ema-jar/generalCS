# Design Patterns

Nel corso degli anni molti sviluppatori si sono trovato a dover risolvere problemi generali quindi hanno sviluppato una serie di soluzioni concettuali semplicemente riutilizzabili, queste soluzioni vengono chiamati design pattern.

Possiamo quindi dire che i design pattern sono delle soluzioni generali a problemi ricorrenti in ambito informatico. 

Il testo di riferimento per lo studio dei pattern è _Design Pattern: Elementi per il riuso di software ad oggetti_ scritto dalla Gang of Four.

I pattern si dividono in tre categorie:

 - creazionali
 - strutturali
 - comportamentali

## Pattern creazionali

Sono dei pattern che risolvono problemi relativi all'istanziazione degli oggetti

### Builder

Il builder è un pattern creazionale utilizzato per creare un oggetto _step-by-step_. Spesso infatti accade che non tutti i parametri di un oggetto debbano essere settati alla sua istanziazione. Ipotizioamo ad esempio di avere una classe `User` che ha diversi campi, alcuni obbligatori, `firstName`, `lastName`, altri non obbligatori, `phone`, `email`.

Un oggetto del genere potrebbe essere creato fornendo un costruttore di default che obblighi l'utente a settare nome e cognome e poi altri costruttori _telescopici_.
```
public class User{
    private String firstName;
    private String lastName;
    private String phone;
    private String email;

    public User(String firstName, String lastName){
        // ...
    }

    public User(String firstName, String lastName, String phone){
        // ...
    }

    public User(String firstName, String lastName, String email){
        // ...
    }

    public User(String firstName, String lastName, String phone, String email){
        // ...
    }
}
```

Si può facilmente intuire che un approccio del genere funziona ma richiede molte combinazioni di costruttori, soprattutto nel caso in cui ci siano molti parametri.

Un secondo approccio potrebbe essere basato sull'utilizzo dei metodi _setter_ per settare uno o più parametri non obbligatori. Questo secondo approccio è sicuramente meno verbono ma ha due controindicazioni, in primis rende l'oggetto mutabile introducendo i setter, inoltre ci sarà un istante in cui l'oggetto è in uno stato inconsistente, come mostrato nel seguente codice:
```
User myUser = new User("John", "Doe");

// momento in cui l'oggetto è in uno stato inconsistente

myUser.setPhone("+36123456");
myUser.setEmail("john.doe@gmail.com");
```

Il builder risolve questi due problemi nel seguente modo:
```
public class User{
    private String firstName;
    private String lastName;

    private String email = "";
    private String phone = "";

    public static class Builder{
      private String firstName;
      private String lastName;

      private String email;
      private String phone;

      public Builder(String firstName, String lastName){
        this.firstName = firstName;
        this.lastName = lastName;
      }

      public Builder setPhone(String phone){
        this.phone = phone;
        return this;
      }
      
      public Builder setEmail(String email){
        this.email = email;
        return this;
      }

      public User build(){
        return new User(this);
      }
    }

    private User(Builder builder){
      this.firstName = builder.firstName;
      this.lastName = builder.lastName;
      this.phone = builder.phone;
      this.email = builder.email;
    }
  }
```

Come possiamo notare la classe statica interna viene utilizzata solo per costruire l'oggetto. I parametri obbligatori sono passati nel costruttore, mentre gli altri hanno un valore iniziale.
Il metodo `build()` richiama il costruttore esterno, che è privato, quindi non può essere richiamato dal fuori.
Usando un builder l'oggetto viene creato a nostro piacimento, settando tutti i campi che ci servono e, soprattutto, in un colpo solo.

Lo svantaggio dell'utilizzo del builder riguarda il fatto di dover costruire una classe che crea l'oggetto, metodo un po' verboso se non ce ne è davvero bisogno.

### Singleton

Spesso capita di dover utilizzare oggetti molto pesanti da istanziare come ad esempio le connesisoni verso le basi di dati. Creare un oggetto del genere ogni volta che ce ne è bisogno potrebbe portare degli abbassamenti dal punto di vista delle performance, in casi come questo entra in gioco il singleton [[1](https://stackoverflow.com/questions/137975/what-is-so-bad-about-singletons)].

Un singleton assicura che ci sia una sola istanza di un dato oggetto all'interno della JVM su cui gira la nostra applicazione.

Il codice utilizzato per creare un singleton è il seguente:
```
public class HeavyClass{
    private static HeavyClass istance = null;

    private HeavyClass(){
        // private constructor
    }

    public static synchronized HeavyClass getIstance(){
        if(istance == null){
            istance = new HeavyClass();
        }

        return istance;
    }
}
```

Il costruttore privato evita che la classe venga istanziata dall'esterno. L'unico modo di accedere ad un oggetto del genere è chiamar eil metodo statico `getIstance()`, il quale ritorna una singola istanza della classe, creandola se non esiste.
Il `syncronized` assicura che in un'applicazione multi thread non vengano generati più singleton.

Spesso il singleton viene considerato un antipattern perchè a volte viene utilizzato come sostituto di una variabile globale all'interno della nostra applicazione. Altra problematica del singleton riguarda la sua testabilità, aspetto questo che tocca tutte le classi con metodi statici che, al loro interno, istanziano nuovi oggetti.


## Pattern strutturali

Sono usati per risolvere problemi inerenti all'architettura delle classi

### Facade

Il facade pattern è utilizzato solitamente per fornire un livello di semplificazione, utilizzabile per nascondere operazioni complesse sottostanti.
In questo modo i client possono utilizzare la classe di facade per eseguire molteplici operazioni, tutte raggruppate in un unico metodo.

Ipotizziamo di voler progettare un applicativo che avvia un computer inizializzando tutte le sue componenti: il processore, la ram e l'hard disk.
L'inizializzazione di ogni componente è un processo complesso e difficile necessario all'avvio della macchina. Se non ci fosse una cacciata tutte le classi che devono avviare un computer dovrebbero eseguire tutte le operazioni necessarie generando molto codice duplicato.

La seguente organizzazione permette di evitare questo problema:
```
/* Parti complesse */

class CPU {
    public void freeze() { ... }
    public void jump(long position) { ... }
    public void execute() { ... }
}

class Memory {
    public void load(long position, byte[] data) { ... }
}

class HardDrive {
    public byte[] read(long lba, int size) { ... }
}

/* Facade */

class ComputerFacade {
    private CPU processor;
    private Memory ram;
    private HardDrive hd;

    public ComputerFacade() {
        this.processor = new CPU();
        this.ram = new Memory();
        this.hd = new HardDrive();
    }

    public void start() {
        processor.freeze();
        ram.load(BOOT_ADDRESS, hd.read(BOOT_SECTOR, SECTOR_SIZE));
        processor.jump(BOOT_ADDRESS);
        processor.execute();
    }
}

class Client {
    public static void main(String[] args) {
        ComputerFacade computer = new ComputerFacade();
        computer.start();
    }
}
```

Il metodo di _facciata_ chiamato `start()` può essere invocato dal client che deve avviare il computer e si occupa ti eseguire tutti i processi necessari all'avvio.
Se un nuovo processo si ritenesse necessario basterebbe inserirlo nel metodo start senza che il client ne venga modificato.

Il facade è un ottimo pattern da utilizzare durante il refactoring di un'applicazione monolitica. SI può infatti partire dallo spezzare l'applicazione in tante sotto parti richiamate in un metodo di facciata e iniziare a disaccoppiare le componenti.

## Pattern comportamentali

Utili perchè risolvono problemi di interazione tra gli oggetti