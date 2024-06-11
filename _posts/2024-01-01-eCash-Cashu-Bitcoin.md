---
layout: post
title: "eCash, Bitcoin e crittografia per degli intermediari meno fidati (ma più affidabili)"
categories: Bitcoin
lang: it
---

Quanto descritto nel seguente articolo è disponibile anche <a href="https://www.filippotol.in/material/federated_ecash.pdf" target="_blank">sottoforma di slide</a>.

## Introduzione

Il 1983 è passato alla storia come uno dei punti di flesso per l'evoluzione delle telecomunicazioni, con la migrazione di <a href="https://en.wikipedia.org/wiki/ARPANET" target="_blank">ARPANET</a> dal <a href="https://en.wikipedia.org/wiki/Network_Control_Protocol_(ARPANET)" target="_blank">NCP</a> al protocollo <a href="https://en.wikipedia.org/wiki/Internet_protocol_suite" target="_blank">TCP/IP</a> avvenuta il 1° gennaio.
Nel settembre dello stesso anno prende vita il progetto <a href="https://en.wikipedia.org/wiki/GNU_Project" target="_blank">GNU</a>, destinato a rivoluzionare l'industria del <a href="https://en.wikipedia.org/wiki/Free_software_movement" target="_blank">free-software</a>.

Certamente meno rumoroso rispetto agli avvenimenti citati, anche il paper <a href="https://www.hit.bme.hu/~buttyan/courses/BMEVIHIM219/2009/Chaum.BlindSigForPayment.1982.PDF" target="_blank">Blind Signatures for Untraceable Payments</a> vide la luce nello stesso anno. L'autore, il crittografo statunitense David Chaum, illustra all'interno del paper un metodo rivoluzionario per garantire la verificabilità e l'anonimato dei partecipanti ad una votazione effettuata a mezzo postale.

Nel 1989 lo stesso Chaum fonda Digicash allo scopo di fornire servizi costruiti con l'ausilio degli schemi crittografici da lui ideati.
Nonstante la genialità delle invenzioni di Chaum, Digicash dichiara bancarotta nel 1998, citando - tra gli altri - problemi di scarsa adozione da parte degli istituti di credito.

Nell'intervista <a href="https://firstmonday.org/ojs/index.php/fm/article/view/683/593" target="_blank">First Monday Interviews: David Chaum</a> del 1999 lo stesso Chaum rimarca alcuni dei problemi dei sistema di pagamenti digitali dell'epoca:

> *Intervistatore*: Here in Belgium for instance there is Proton, but the problem with this kind of payment system is that everything you buy with it is recorded.

> *David Chaum*: And it's also that people assume that this is not taking place. **The way these products are often promoted to the public is that they are essentially like cash.**

ma anche ...

> *Intervistatore*: Do you think then that the Digicash technology entered the market too early - that people aren't yet used to this kind of technology?

> *David Chaum*: There were things like that. **Electronic commerce was hardly happening at the consumer level when Digicash was trying to gain momentum**. The ease with which things could be integrated into the Web has improved a lot since we were most active. The technological infrastructure was a little less optimal then for us than it is today - as far as making it easy for consumers and merchants to use it. I expect that it will get even easier.

e ancora ...

> *Intervistatore*: **Will there be an anonymous and secure electronic cash system soon, or is there a general trend towards existing systems which just carry the credit card principle on the net? As I said before, there is often a strong interest in tracking the users' behaviour**, knowing what they are buying, where they are buying it and being also able to connect this information to targeted advertisement.

> *David Chaum*: I think you have both kind of forces. **The thing about the interest in having consumer controlled privacy protection is that most people (most consumers) aren't that aware of it**, and it's not really a viable option today, but I believe that it could dominate if it is both made readily available in an easy to use manner and awareness of it is created.

## Firme cieche, banche e messaggi segreti

### Problemi generali di un sistema ecash

Il sistema teorizzato da David Chaum ha subito aggiornamenti e miglioramenti nel corso degli anni, tuttavia ad oggi non esistono implementazioni di eCash chaumiano che siano diffusamente in uso. 

Le motivazioni possibili sono molteplici, per lo più legate a...

1. **... pressioni normative**. Un sistema costruito secondo i meccanismi definiti da Chaum è un sistema innatamente anonimo, nel quale l'emittente dei token eCash non ha possibilità di tracciare o monitorare i pagamenti degli utenti. Ad oggi le normative per anti-riciclaggio e anti-terrorismo - come anche la disciplina tributaria - non permettono questo tipo di approccio (per lo meno nella stragrande maggioranza delle giurisdizioni);

2. **... brevetti**. Molte tecnologie e standard crittografici sono stati brevettati dagli inventori, tra cui anche molti algoritmi ideati da Chaum. La presenza di brevetti potrebbe aver rallentato lo sviluppo ed implementazione di sistemi basati su eCash;

3. **.. cold start problem**. Un sistema di eCash permette agli utenti di godere di anonimato e privacy quasi perfetta. Tuttavia esso risulta difficile da proporre sul mercato poichè la maggior parte degli utenti si professano apertamente disinteressati alla privacy online e dei pagamenti. Senza un'iniziale massa critica di utenti, nessuno è incentivato ad utilizzare un nuovo sistema di pagamento, il che rende difficile il **bootstrapping** di un progetto simile;

4. **... problemi di scalabilità**. Il processo di creazione, circolazione ed estinzione di nuovi token eCash necessita di una sostanziale capacità di <a href="https://www.techtarget.com/searchnetworking/definition/throughput" target="_blank">throughput</a>, difficilmente ottenibile con hardware e software di qualche decennio fa.

### Funzionamento generale di un sistema ecash

Il sistema proposto da David Chaum per i pagamenti digitali è generalizzabile secondo il seguente schema:

1. **Partecipanti**:
    - Bob (B) - di fatto una banca (mint) - che è emittente dei token eCash e custode del collaterale a garanzia della massa monetaria di eCash in circolazione;
    - Alice (A), un individuo interessato ad ottenere degli eCash.

2. **Procedura**: 
    - per utilizzare eCash, Alice deve recarsi presso Bob e prelevare una certa somma di eCash a fronte di un corrispettivo in altra valuta. Inizialmente Alice ripone un foglio di carta bianco all'interno di una busta speciale (ad esempio una busta di <a href="https://it.wikipedia.org/wiki/Carta_carbone)" target="_blank">carta carbone</a>;
    - Alice si reca presso Bob e richiede di prelevare una somma X in token eCash versando questa somma in euro presso lo sportello;
    - Bob riceve il versamento di €X e appone sulla busta di Alice una firma specifica e valida per l'importo €X;
    - Alice, lasciato lo sportello, apre la busta ed estrae il foglio di carta. Poichè la busta è in *carta carbone*, la firma valida per €X effettuata da Bob è penetrata all'interno della busta e risulta ora apposta anche sul foglio di carta, ora di fatto un token eCash;
    - Alice può ora scambiare liberamente il foglio di carta come un token eCash di valore €X. Chiunque riceva da Alice questo token può verificare presso Bob che la firma riposta sul token sia corrispondente ad una firma valida per un prelievo di €X.

Il sistema illustrato garantisce che le transazioni in a cui il token eCash emesso da Bob prende parte non siano tracciabili, poichè la Bob stesso non ha mai avuto visibilità sul foglio di carta (token eCash) ma solo sulla busta che lo conteneva. Il token in circolazione può quindi essere riportato presso Bob e riscattato per un valore di €X in qualsiasi momento, senza collegare l'identità di Alice all'identità del prelevante.

La firma apposta da Bob sulla busta è una **blind signature**, una firma che Bob appone senza essere a conoscenza di cosa effettivamente stia firmando. Il token assume un valore corrispondente alla tipologia specifica di firma riposta sulla busta da Bob, che corrisponde a quanto versato da Alice allo sportello. Così facendo Alice non ha modo di falsificare poi il contenuto della busta ed è obbligata a scambiare il token al valore nominale garantito dalla firma stessa.

Quanto illustrato, sebbene abbia una rilevanza marginale in un contesto di transazioni economiche effettuate a mezzo contante, risulta di interesse sostanziale là dove gli scambi economici avvengono principalmente a mezzo digitale.

### Proposta per una ecash di banca centrale

Nonostante la mancanza di servizi che implementano meccanismi di eCash chaumiano, nel 2021 è stato pubblicato un working paper (<a href="https://www.snb.ch/en/publications/research/working-papers/2021/working_paper_2021_03" target="_blank">*How to issue a central bank digital currency*</a>) nel quale gli autori propongono un sistema di eCash attivabile da una Banca Centrale per garantire la privacy degli utenti ed il contestuale rispetto delle regolamentazioni in materia di intermediari finanziari.
Seguendo le indicazioni degli autori di questo paper, nel novembre 2023 la Bank for International Settlements ha annunciato la creazione del <a href="https://www.bis.org/publ/othp80.htm" target="_blank">Progetto Tourbillon</a>, il quale propone un'implementazione di eCash chaumiano all'interno di un sistema a Central Bank Digital Currency (CBDC). 
Il progetto sembra garantire maggiore privacy negli scambi, mantenendo anonima l'identità del pagante ma conservando la possibilità di monitorare e tracciare le transazioni del ricevente ai fini fiscali, di antiriciclaggio ed anti-terrorismo.
La proposta del Progetto Tourbillon è certamente rivoluzionaria rispetto al dibattito classico in materia di CBDC, rimangono alcuni dubbi circa il livello di privacy effettiva garantita dallo strumento che sicuramente verranno chiariti quando e se l'utilizzo effettivo del sistema prenderà piede.

## Cashu, eCash chaumiano costruito su Bitcoin

A mio giudizio, le implementazioni più interessanti di un sistema ad eCash chaumiano sono Cashu e Fedi, due giovanissime proposte di scalabilità per il protocollo Bitcoin. Le seguenti sezioni dell'articolo approfondiscono il protocollo Cashu. Si rimanda ad uno dei successivi articoli per un analisi approfondita sulla proposta Fedi.

Al fine di introdurre le particolarità ed il funzionamento di Cashu, si procede con l'illustrazione delle caratteristiche e trade-off principali del protocollo Bitcoin ad oggi.

### Il protocollo Bitcoin in breve

<a href="https://bitcoin.org/bitcoin.pdf" target="_blank">Il whitepaper di Bitcoin</a>, pubblicato da Satoshi Nakamoto il 31 ottobre 2008, illustra un sistema di pagamento digitale senza intermediari che risolve il problema della doppia spesa con una geniale applicazione di varie tecnologie tra cui <a href="https://nakamotoinstitute.org/finney/rpow/index.html" target="_blank">Reusable Proof of Work</a> ideato da Hal Finney ed il sistema <a href="http://www.hashcash.org/papers/hashcash.pdf" target="_blank">Hashcash</a> teorizzato da Adam Back.

#### Proprietà del protocollo

Le caratteristiche tecnologiche principali del protocollo Bitcoin lo rendono:

- **Immutabile**, poichè la cronologia e l'ordinamento delle transazioni incluse nel blocchi convergono verso un unico status grazie allo schema di incentivi economici;
- **Neutrale**, poichè protocollo puramente tecnologico;
- **Senza intermediari**, poichè le transazioni non sono intermediate da alcuna controparte;
- **Distribuito**, poichè ogni componente del network ha la possibilità di eseguire un client che applica le regole del protocollo anche in situazioni di risorse limitate (bassa latenza, impossibilità nell'accedere ad hardware di ultima generazione ecc.).

> Il protocollo Bitcoin è di fatto un sistema di comunicazione distribuito che converge ad una cronologia di transazioni univoca e non conflittuale, realizzando un token scarso utilizzato come remunerazione per i partecipanti al protocollo.

#### Problema della scalabilità

La natura del protocollo e la necessità di decentralizzazione dello stesso costituiscono un serio limite alla scalabilità in termini di **throughput** transattivo. 
In linea generale la soluzione di lungo termine a questo limite tecnologico risiede nell'utilizzo della blockchain come livello di settlement, con il contestuale spostamento della massa transattiva verso a protocolli e soluzioni off-chain.

Questa elaborazione della struttura protocollare crea la necessità di disegnare protocolli alternativi off-chain idonei e nel rispetto del seguente trade-off generale:

##### Estremità 1: Soluzione custodial

L'utente si affida ad una piattaforma che funge da intermediario per le transazioni che avvengono internamente alla piattaforma, oltre che da custode dei fondi. Questo tipo di soluzione ripristina il rischio di controparte che il protocollo Bitcoin si propone di eliminare, a beneficio di una maggiore scalabilità il cui limite è definito dall'infrastruttura tecnologica della piattaforma.

Utilizzando una soluzione custodial, le problematiche che l'utente deve affrontare sono le seguenti:

1. **rischio di controparte**, cioè il rischio che la piattaforma non adempia ai propri obblighi (ad esempio, la piattaforma sceglie di non rimborsare i propri utenti, la banca blocca i prelievi ecc.);
2. **problemi di privacy**, ovvero la piattaforma ha a disposizione tutta la cronologia di transazioni dell'utente.


##### Estremità 2: Soluzione non custodial

L'utente mantiene la sovranità sui propri fondi, ereditando le responsabilità di corretta custodia e diligenza richieste dal protocollo base, oltre che spesso aumentando la complessità e le conoscenze richieste.

Ogni proposta tecnologica avanzata fino ad oggi si mantiene all'interno dello spettro di possibilità costituito dal precedente trade-off: soluzioni come le piattaforme Exchange sono infatti un perfetto esempio di soluzione di tipo 1, mentre proposte come Lightning Network o il concetto generale di sidechain si possono considerare alla stregua di 'variazioni' della soluzione di tipo 2.

<img src="/assets/img/custodiality.png" alt="Trade-off custodial vs self-custodial" width="600" height="400">

### Cashu

> Cashu is a free and open-source Chaumian ecash system built for Bitcoin. Cashu offers near-perfect privacy for users of custodial Bitcoin applications. <a href="https://cashu.space/" target="_blank">(cashu.space)</a>

Cashu è un protocollo che punta a risolvere il problema della privacy in un contesto di piattaforma custodial, costituendo un'ottima soluzione per utenti non avvezzi a soluzioni self-custodial che vogliono però tutelare maggiormente la propria privacy.

### Come funziona

Il funzionamento generale di cashu è molto simile a quanto descritto con riferimento al paper di David Chaum, con alcune aggiunte riguardanti:

- modalità di **blinding**, ovvero come Bob (mint) rimane 'cieco' con riferimento al messaggio all'interno della busta;
- modalità di **unblinding**, ovvero come Alice riesce a 'scartare' la busta;
- modalità di **verifica**, ovvero come Bob verifica la legittimità del token eCash e, di conseguenza, come la controparte Carol (con cui Alice ha scambiato il token) si assicura della legittimità dei fondi ricevuti.

Di seguito uno schema approfondito del funzionamento di Cashu. Tra le risorse aggiuntive a fine articolo è disponibile lo schema completo per la procedura (Diffie-Helmann Key Exchange) utilizzato da Cashu.

- Bob (mint)
    - `k` chiave privata della mint 
    - `K` chiave pubblica della mint

- Alice (user)
    - `x` stringa casuale, di fatto il messaggio segreto. Da `x` viene matematicamente derivato il punto `Y` sulla curva ellittica
    - `r` chiave privata di Alice (fattore blinding)

<img src="/assets/img/cashu_scheme.png" alt="Ecash creation scheme" width="800" height="800">

#### Osservazioni

Dall'analisi dello schema emergono ulteriori dettagli rispetto allo schema generico illustrato in precedenza.

1. Per assicurarsi della legittimità dei fondi ricevuti, Carol è obbligata a richiedere la verifica di Bob ad ogni transazione ricevuta, da cui la necessità che Bob sia sempre online e raggiungibile;
2. Nel momento in cui Bob riceve `(x, C)` egli è in grado di osservare il segreto `x` che quindi **non è più segreto** poichè sia Bob, sia Alice, sia Carol ne sono a conoscenza. 

Per ovviare al problema emerso al punto 2, Carol richiede dei nuovi ecash token usando lo stesso meccanismo con cui Alice riscatta i token da Bob. Il valore richiesto da Carol è esattamente corrispondente a quanto ricevuto da Alice. In questo modo:

- Carol genera un nuovo `x`, un nuovo `Y` ed un nuovo `B_`;
- Carol richiede a Bob dei nuovi ecash in cambio del token `(x, C)` appena ricevuto da Alice;
- il token `(x, C)` esistente viene *bruciato* da Bob;
- Bob restituisce a Carol un nuovo `C_` calcolato con il nuovo `B_` ricevuto da Carol;
- Carol calcola `C` per il nuovo token.

Questa procedura assicura l'anonimità del token ad ogni trasferimento.

### Vantaggi e svantaggi di Cashu

#### Vantaggi

Il sistema ecash implementato in Cashu ha numerosissime proprietà e qualità, tra cui:

- perfetta privacy degli utenti che effettuano transazioni grazie alle firme cieche;
- una Mint può essere hostata su qualsiasi hardware che ha accesso ad internet e riesce a utilizzare una propria **founding source** come un nodo lightning, wallet onchain o un wallet di terza parte;
- è possibile costruire schemi più elaborati dove il token `(x, C)` viene criptato;
- il token `(x, C)` è effettivamente il denaro, che quindi viene conservato come strumento al portatore dagli utenti e non custodito presso la Mint. Ciò contribuisce a diminuisce il rischio di controparte, anche se la Mint è di fatto il garante del valore del token ecash e responsabile della conservazione del collaterale;
- Le transazioni in ecash possono avvenire anche tra utenti che possiedono ecash di mint diverse, è necessario però che:
    - gli utenti si fidino della nuova mint di cui hanno ricevuto i fondi;
    - oppure che gli utenti effettuino uno swap tra mint, di fatto facendo un settlement Lightning Network tra le due mint.
- interoperabilità con Lightning Network.

#### Svantaggi

Cashu soffre chiaramente delle problematiche classiche dei sistemi basati su controparti custodial, quali:

- Gli ecash non vengono creati seguendo il modello a UTXO. In Cashu esistono 'monete' di valore nominale pari a 2<sup>n</sup> con `n` intero maggiore di 0(dunque monete da 1, 2, 4, 8, 16, 32, ..), ciò aumenta la privacy delle transazioni ma limita potenzialmente il throughput della mint;
- La mint è responsabile della conservazione del collaterale. Se il collaterale a garanzia degli ecash è detenuto in bitcoin su Lightning Network, la mint diventa di fatto un hot-wallet;
- La mint deve essere sempre online poichè ad ogni transazione gli utenti devono presentare alla mint il token `(x, C)` ricevuto e ricevere dei nuovi ecash;
- Per un numero di utenti sufficientemente grande, la mint potrebbe incorrere in problemi di scalabilità;
- Come tutti i servizi custodial, la mint incorre in possibili rischi normativi a dipendere dalla legislazione in cui risiede.

### Implementazioni e riferimenti

Il protocollo Cashu conta ad oggi 8 implementazioni di wallet (disponibili <a href="https://docs.cashu.space/wallets" target="_blank">qui</a>) e 5 implementazioni di mint (guarda <a href="https://docs.cashu.space/mints" target="_blank">qui</a>), effettivamente utilizzabili ad oggi con fondi mainnet (anche se rischiosi a causa di bug comprensibili vista la giovane età del protocollo).

## Conclusione

Lo schema di blinded signature e di eCash proposto da David Chaum nel 1983 potrebbe ricoprire un ruolo fondamentale per l'avanzamento dello sviluppo del protocollo Bitcoin, generando nuovi modelli di business e tutelando la privacy degli utenti. 

Nonostante i decenni, la soluzione proposta da Chaum si riconferma una tecnologia elegante e semplice per ovviare ai noti problemi di sistemi basati su controparti fidate, forse l'ultima chance per creare sistemi di pagamento digitali liberi ed a servizio degli utenti, svincolati dalla retorica della 'prevenzione attraverso la sorveglianza'.

## Fonti

- <a href="https://www.hit.bme.hu/~buttyan/courses/BMEVIHIM219/2009/Chaum.BlindSigForPayment.1982.PDF" target="_blank">Blinded signatures for Untraceable Paymants - Chaum, D, (1983)</a>
- <a href="https://firstmonday.org/ojs/index.php/fm/article/view/683/593" target="_blank">First Monday Interviews: David Chaum (1999)</a>
- <a href="https://www.snb.ch/en/publications/research/working-papers/2021/working_paper_2021_03" target="_blank">How to issue a central bank digital currency - Chaum D., Grothoff C., Moser T. (2021)</a>
- <a href="https://www.bis.org/" target="_blank">Bank for International Settlements [website]</a>
- <a href="https://bitcoin.org/bitcoin.pdf" target="_blank">Bitcoin: A Peer-to-Peer Electronic Cash System - Nakamoto, S. (2008)</a>
- <a href="https://docs.cashu.space/" target="_blank">Cashu documentation [website]</a>
- <a href="https://www.youtube.com/watch?v=VwMzNE1D3so" target="_blank">Chaumian eCash in Bitcoin - Cashu & Fedimint - Adam Gibson aka waxwing [youtube]</a>

## Risorse aggiuntive

Per chiarire ulteriormente la procedura di minting e verifica del token utilizzata in Cashu allego uno schema riassuntivo contenente le formule estese.

<object data="/assets/docs/scheme.pdf" width="600" height="800" type='application/pdf'/>

<div class="post-categories">
  {% if post %}
    {% assign categories = post.categories %}
  {% else %}
    {% assign categories = page.categories %}
  {% endif %}
  {% for category in categories %}
  <a href="{{site.baseurl}}/categories/#{{category|slugize}}">{{category}}</a>
  {% unless forloop.last %}&nbsp;{% endunless %}
  {% endfor %}
</div>


