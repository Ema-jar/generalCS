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

## Pattern strutturali

Sono usati per risolvere problemi inerenti all'architettura delle classi

## Pattern comportamentali

Utili perchè risolvono problemi di interazione tra gli oggetti