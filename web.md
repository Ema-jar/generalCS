# Web

In questa sezione sono inserite tutte le informazioni comune sul funzionamento del web.

## Dall'url alla risorsa

Cosa succede quando digitiamo un url all'interno del notro browser e poi premiamo invio?

I passi che portano dalla digitazione di un url all'effettivo ottenimento della risorsa sono diversi, eccoli elencati:

1. Inseriamo www.google.it nel nostro browser
2. Il browser cerca nella sua cache se c'è un record DNS associato all'url digitato. Se non lo trova nella cache del browser loc erca all'interno della cache del SO, se non lo trova nemmeno li procede con la cache del router e infine si rivolge alla cache del fornitore di servizi (ISP) di riferimento. Questa ricerca ha lo scopo di trovare l'indirizzo IP associato all'ur digitato. Ricordiamo che ad ogni sito web è associato un IP
3. Se l'url non è presente in nessuna cache viene effettuata una ricerca che si propaga da un DNS server all'altro al fine di trovare l'IP richiesto
4. Una volta trovato l'IP il browser può iniziare una connessione TCP con il server che contiene la risorsa cercata. Una connesisone TCP è iniziata con un procedimento basato sullo scambio di tre pacchetti chiamato _three way handshake_
5. A questo punto la connesisone è stata stabilita e il client può effettuare una chiamata HTTP verso il server
6. Il server, ricevuta la richiesta, può inviare una risposta HTTP

## Cosa è HTTP

HTTP (Hipertext Transfer Protocol) è un protocollo su cui vengono basate le comunicazioni via web. Nasce nel 1991 dal genio di Tim Berners-Lee e definisce una serie di informazioni e procedure grazie alle quali un client può richiedere una risorsa ad un server.

Un'evoluzione di HTTP è HTTPS (HTTP Secure). HTTPS nasce con lo scopo di garantire una maggiore sicurezza grazie a comunicazioni che fanno uso di crittografia per evitare attacchi di tipo _man-in-the-middle_.