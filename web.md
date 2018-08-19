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


## CORS

Spesso un'applicaizone web effettua richieste verso domini esterni per ottenere risorse come pagine web, immagini o font. I server web devono quindi gestire delle _security policies_ per processare queste richieste con sicurezza. 

Le _same-origin policies_ sono estremamente restrittive, da una parte evitano che codice Javascript possa fare richieste verso domini differenti, dall'altra parte permettono ai nostri applicativi di effettuare richieste solo verso i server su cui l'applicativo risiede.

Le Cross-Origin Resource Sharing (CORS) allegeriscono il vincolo precedentemente descritto e permettono al codice Javascript di eseguire chiamate verso server differenti.

La seguente richiesta contiene molte informazioni interessanti:
```
GET /greeting/ HTTP/1.1
User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10_8_5) AppleWebKit/536.30.1 (KHTML, like Gecko) Version/6.0.5 Safari/536.30.1
Accept: application/json, text/plain, */*
Referer: http://foo.client.com/
Origin: http://foo.client.com
```

La prima riga ci mostra la tipologia di richiesta fatta dal client, in questo caso un GET.
La seconda riga riporta le informazioni relative al browser da cui la richiesta proviene, in questo caso Safari.
La terza riga ci dice che il client accetta una risposta in json o in plain text.
L'ultima riga contiene un'informazione importante, l'_Origin_ della chiamata. Il server si baserà su questo valore per permettere o negare una risposta al client. Nel caso in cui il server accetti la chiamata, nella risposta fornita sarà presente il campo `Access-Control-Allow-Origin: http://foo.client.com`. In questo caso il server accetta tutte le richieste provenienti da http://foo.client.com. Alcune volte il valore del campo Access-Control-Allow-Origin può essere "*", in questo caso si intende che il server accetta richieste da tutti i client. Quest'ultima non è da considerarsi una best practice, a meno che la risorsa non sia pubblica e consumabile indistintamente da tutti.

Quando la richiesta può avere effetti delicati sui dati, ad esempio nel caso di una DELETE, vale la pena accertarsi che il server possa gestire questo tipo di interazione senza problemi. Per questo motivo viene inviata una richiesta preliminare chiamata _preflight_ utilizzando il verbo `OPTION`.

La seguente richiesta descrive i parametri di una OPTION:
```
OPTIONS /resource/12345
User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10_8_5) AppleWebKit/536.30.1 (KHTML, like Gecko) Version/6.0.5 Safari/536.30.1
Access-Control-Request-Method: DELETE
Access-Control-Request-Headers: origin, x-requested-with, accept
Origin: http://foo.client.com
```

Il client http://foo.client.com **vorrebbe** fare una delete sulla risorsa 12345, quindi manda un preflight per sapere se la richiesta è gestibile dal server. Notare che il preflight **non è** una richiesta di DELETE, la delete non è stata ancora inviata. Immaginiamo questa OPTION come un voler dire: _caro server, vorrei fare una delete su questa risorsa, come la vedi? Puoi gestirla senza problemi?_

In caso positivo il server risponde con una risposta del genere:
```
HTTP/1.1 200 OK
Date: Wed, 20 Nov 2013 19:36:00 GMT
Server: Apache-Coyote/1.1
Content-Length: 0
Connection: keep-alive
Access-Control-Allow-Origin: http://foo.client.com
Access-Control-Allow-Methods: POST, GET, OPTIONS, DELETE
Access-Control-Max-Age: 86400
```

La prima riga indica l'esito positivo della richiesta, la seconda la data in cui la richiesta è stata fatta,
la terza contiene informazioni sul server poi abbiamo la lunghezza del contenuto inviato al client e il keep-alive che informa il client della volontà di mantenere aperta la connessione.
Le ultime tre righe sono importanti perchè autorizzano l'origin a procedere, elencano i metodi supportati dal server (tra cui la delete che il client vuole fare) e assicura al client che questo preflight sarà valido per 86400 secondi, un giorno. 