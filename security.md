# Sicurezza

Alla base della sicurezza informatica c'è il il processo continuo di garantire confidenzialità, integrità e disponibilità sui vari livelli di un sistema. Questa viene chiamata la Triade CIA.

L'**integrità** di un sistema riguarda tutti quegli aspetti che assicurano che un dato sia reale, accurato, consistente e non modificabile da coloro che non ne hanno permesso. In parole povere il principio di integrità assicura che un dato venga cambiato solo da coloro che hanno un'_autorizzazione_ per cambiarlo.

La _responsabilità_ (Accountability) di una modifica è un concetto fortemente legato all'integrità del dato. UN sistema deve essere in grado di sapere chi ha modificato cosa.

La _non negabilità_ (non-repudiation) è la capacità del sistema di generare log e informazioni in seguito alle modifiche effettuate in modo da rendere innegabile l'azione di un utente.

L'integrità può essere assicurata con l'aiuto di funzioni di hashing. Passando un dato all'interno di una funzione di hash possiamo capire immediatamente se è stata effettuata una modifica. Un classico utilizzo dell'hashing viene fatto durante il download degli aggiornamenti di un programma. L'hash di tale aggiornamento viene confrontata con quella originale per capire se qualche ente malevolo ha effettuato delle modiiche durante il download.

La **confidenzialità** di un sistema riguarda la capacità di mostrare i dati solo alle entità autorizzate a vederli.

Alla base di questo aspetto si trova il concetto di _autenticazione_. Un'entità deve essere nota al sistema prima di intervenire sui dati in esso contenuti.

Subito dopo l'autenticazione è la volta dell'_autorizzazione_. Un'entità autenticata potrebbe non avere diritto di interagire con una risorsa a causa di una mancata autorizzazione.

La crittografia entra in gioco quando i dati devono essere messi al sicuro da sguardi indiscreti. Utilizzare tecniche del genere permette di nascondere il contenuto dei dati a tutti coloro che non posseggono la chiave giusta per leggerli.

La **disponibilità** assicura che un sistema rimanga accessibile agli utenti autorizzati quano questi ne hanno bisogno.
Gli attacchi di tipo DoS o DDoS vanno a colpire proprio questo aspetto.

Il _disaster recovery_ è importantissimo per garantire la disponibilità di un sistema. I gestori devono essere in grado di ricostruire il contenuto di un sistema in caso di disastro umano o naturale.

Un altro aspetto fondamentale riguarda il _failover_, cioè la capacità di attivare una seconda istanza del sistema qualora la prima non dovesse essere disponibile.

La _resilienza_ è l'ultimo aspetto fondamentale. Durante un attacco il sistema deve essere in grado di reagire scalando oppure evitando dei single point of failure.



