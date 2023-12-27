---
layout: post
title: "Ecash, ovvero un progetto dimenticato che potrebbe rivoluzionare i pagamenti digitali"
categories: Bitcoin
---

## Introduzione

Il 1983 è passato alla storia come uno dei punti di flesso per l'evoluzione delle comunicazioni, con la migrazione di [ARPANET](https://en.wikipedia.org/wiki/ARPANET) dal [NCP](https://en.wikipedia.org/wiki/Network_Control_Protocol_(ARPANET)) al protocollo [TCP/IP](https://en.wikipedia.org/wiki/Internet_protocol_suite) avvenuta il 1° gennaio.
Nel settembre dello stesso anno nasce il progetto [GNU](https://en.wikipedia.org/wiki/GNU_Project), che ha rivoluzionato profondamente l'industria del software fino ai giorni nostri, iniziando *de-facto* il [movimento free-software](https://en.wikipedia.org/wiki/Free_software_movement).

Certamente meno rumoroso rispetto agli avvenimenti citati, anche il paper [*Blind signatures for untraceable payments*](https://www.hit.bme.hu/~buttyan/courses/BMEVIHIM219/2009/Chaum.BlindSigForPayment.1982.PDF) vide la luce nello stesso anno. L'autore, il crittografo statunitense David Chaum, illustra all'interno del paper un metodo rivoluzionario per garantire la verificabilità e l'anonimato dei partecipanti ad una votazione effettuata a mezzo postale.
Nel 1989 lo stesso Chaum fonda Digicash allo scopo di fornire servizi costruiti con l'ausilio degli schemi crittografici da lui ideati.
Nonstante la genialità delle invenzioni di Chaum, Digicash dichiara bancarotta nel 1998, citando - tra gli altri - problemi di scarsa adozione da parte degli istituti di credito.

Nell'intervista [*First Monday Interviews: David Chaum*](https://firstmonday.org/ojs/index.php/fm/article/view/683/593) del 1999 lo stesso Chaum rimarca alcuni dei problemi dei sistema di pagamenti digitali dell'epoca:

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

Il sistema teorizzato da David Chaum è stato migliorato nel corso degli anni, tuttavia non esistono implementazioni di ecash chaumiano che siano in uso ad oggi. Le motivazioni possibili sono molteplici, per lo più:

1. **Pressioni normative**. Un sistema costruito sulla proposta di ecash chaumiano è un sistema innatamente anonimo, dove l'emittente dei token ecash non ha possibilità di tracciare, monitorare o correlare pagamenti tra utenti. Ad oggi le normative per Anti-riciclaggio e Anti-terrorismo non permettono questo tipo di approccio;

2. **Brevetti**. Alcune tecnologie inizialmente proposte per la creazione di un simile sistema sono state poi brevettate, richiedendo quindi un notevole sforzo per aggirare i brevetti;

3. **Cold start problem**. Un sistema di ecash simile si può considerare più difficile da proporre sul mercato poichè utenti disinteressati alla privacy online non sono di certo incentivati ad utilizzarlo;

4. **Problemi di scalabilità**. Il funzionamento di un sistema come quello teorizzato da Chaum necessita di una sostanziale capacità di throughput, di certo non ottenibile con hardware di qualche decennio fa.

### Funzionamento generale di un sistema ecash

Il sistema proposto da David Chaum per i pagamenti digitali è generalizzabile utilizzando il seguente schema:

- Una Banca (B), emittente dei token ecash e custode del collaterale a garanzia della massa monetaria di ecash in circolazione;
- utente Alice (A), un individuo interessato ad ottenere degli ecash.

Per utilizzare dell'ecash, Alice deve recarsi presso la banca e prelevare una certa somma di ecash a fronte di un corrispettivo in altra valuta. Per assicurare l'anonimato nell'erogazione dei token ecash, il tutto avviene nel rispetto della seguente procedura:

- Alice ripone un foglio di carta bianco all'interno di una busta speciale (ad esempio una busta di [carta carbone](https://it.wikipedia.org/wiki/Carta_carbone));
- Alice si reca presso la Banca e richiede di prelevare una somma X in token ecash versando questa somma in Euro presso lo sportello;
- La Banca riceve il versamento di €X e appone sulla busta di Alice una firma specifica e valida per l'importo €X;
- Alice torna a casa, apre la busta ed estrae il foglio di carta. Poichè la busta è in *carta carbone*, la firma valida per €X effettuata dalla Banca è penetrata all'interno della busta e risulta ora apposta anche sul foglio di carta, ora di fatto un token ecash;
- Alice può ora scambiare liberamente il foglio di carta come un token ecash di valore €X. Chiunque riceva da Alice questo token può verificare presso la Banca che la firma riposta sul token sia corrispondente ad una firma valida per un prelievo di €X.

Il sistema illustrato garantisce che non vi sia una tracciabilità del token ecash emesso dalla Banca, poichè la Banca stessa non ha mai avuto visibilità sul foglio di carta ma solo sulla busta che lo conteneva. Il token in circolazione può quindi essere riportato presso la Banca e riscattato per un valore di €X in qualsiasi momento, senza collegare l'identità di Alice all'identità del prelevante.

La firma apposta dalla Banca sulla busta è una **blind signature**, una firma che la Banca appone senza essere a conoscenza di cosa effettivamente stia firmando. Il token assume un valore corrispondente alla tipologia specifica di firma riposta sulla busta dalla Banca, che corrisponde a quanto versato da Alice allo sportello. Così facendo Alice non ha modo di falsificare poi il contenuto della busta ed è obbligata a scambiare il token al valore nominale garantito dalla firma stessa.

Quanto illustrato ha una rilevanza marginale in un contesto di transazioni economiche effettuate a mezzo contante. Nonostante ciò la procedura risulta di interesse sostanziale là dove gli scambi economici avvengono principalmente a mezzo digitale, con un conseguente rischio per la privacy degli utenti, oltre che il sistematico tracciamento degli utenti da parte degli intermediari e terze parti fidate.

### Proposta per una ecash di banca centrale

Ad onor del vero, esiste un recente working paper che propone l'implementazione di un sistema simile. Lo stesso David Chaum - con la collaborazione di Christian Grothoff e Thomas Moser - ha infatti pubblicato nel marzo del 2021 il working paper [How to issue a central bank digital currency](https://www.snb.ch/en/publications/research/working-papers/2021/working_paper_2021_03), nel quale viene proposto un sistema di ecash implementabile da una Banca Centrale per garantire la privacy degli utenti ed il contestuale rispetto delle regolamentazioni in materia di intermediari finanziari.

Nel novembre 2023 la Bank for International Settlements ha annunciato la creazione del [Progetto Tourbillon](https://www.bis.org/publ/othp80.htm), il quale propone un'implementazione di ecash chaumiano a sostituzione del sistema a Central Bank Digital Currency (CBDC). Il progetto sembra garantire maggiore privacy negli scambi, mantenendo anonima l'identità del pagante ma conservando la possibilità di monitorare e tracciare le transazioni del ricevente ai fini fiscali, di antiriciclaggio ed anti-terrorismo.
La proposta del Progetto Tourbillon è certamente rivoluzionaria rispetto al dibattito classico in materia di CBDC, rimangono alcuni dubbi circa il livello di privacy effettiva garantita dallo strumento che sicuramente verranno chiariti quando e se l'utilizzo effettivo del sistema prenderà piede.

## Cashu, ecash chaumiano costruito su Bitcoin

### Il protocollo Bitcoin in breve

Il whitepaper di Bitcoin, pubblicato da Satoshi Nakamoto il 31 ottobre 2008, illustra un sistema di pagamento digitale senza intermediari, che risolve il problema della doppia spesa con una geniale applicazione di varie tecnologie tra cui [Reusable Proof Of Work](https://nakamotoinstitute.org/finney/rpow/index.html) ideato da Hal Finney, [b-money](http://www.weidai.com/bmoney.txt) proposto da Wei Dai ed il sistema [Hashcash](http://www.hashcash.org/papers/hashcash.pdf) teorizzato da Adam Back.

#### Proprietà

Le caratteristiche tecnologiche principali del protocollo Bitcoin lo rendono:

- **Immutabile**, poichè la cronologia e l'ordinamento delle transazioni incluse nel blocchi convergono verso un unico status grazie allo schema di incentivi economici;
- **Neutrale**, poichè pressioni esogene al protocollo non possono modificarne il funzionamento;
- **Senza intermediari**, poichè le transazioni non sono intermediate da alcuna controparte;
- **Distribuito**, poichè ogni componente del network ha la possibilità di eseguire un client che applica le regole del protocollo anche in situazioni di risorse limitate (bassa latenza, impossibilità nell'accedere ad hardware di ultima generazione etc.).

> Il protocollo Bitcoin è di fatto un sistema di comunicazione distribuito che converge ad una cronologia di transazioni univoca e non conflittuale, realizzando un token scarso utilizzato come remunerazione per i partecipanti al protocollo.

#### Scalabilità 

La natura del protocollo e la necessità di decentralizzazione dello stesso costituiscono un serio limite alla scalabilità del protocollo in termini di **throughput** transattivo. 
In linea generale la soluzione di lungo termine a questo limite tecnologico risiede nell'utilizzo della blockchain come livello di settlement, con il contestuale spostamento della massa transattiva verso a protocolli e soluzioni off-chain.

Questa elaborazione della struttura protocollare crea la necessità di disegnare protocolli alternativi off-chain idonei e nel rispetto del seguente trade-off generale:

##### 1) Soluzione custodial

L'utente si affida ad una piattaforma che funge da intermediario per le transazioni che avvengono internamente alla piattaforma, oltre che da custode dei fondi. Questo tipo di soluzione ripristina il rischio di controparte e di intermediario che il protocollo Bitcoin si propone di eliminare, a beneficio di una maggiore scalabilità il cui limite è definito dall'infrastruttura tecnologica della piattaforma;

In questo caso, le problematiche che l'utente deve affrontare sono le seguenti:

1. **rischio di controparte**, cioè il rischio che la controparte non adempia ai propri obblighi (ad esempio, la piattaforma sceglie di non rimborsare i propri utenti, la banca blocca i prelievi etc);
2. **problemi di privacy**, ovvero la controparte ha a disposizione tutta la cronologia di transazioni dell'utente.


##### 2) Soluzione non custodial

L'utente mantiene la sovranità sui propri fondi, ereditando le responsabilità di corretta custodia e diligenza richieste dal protocollo base, oltre che spesso aumentando la complessità e le conoscenze richieste.

Ogni proposta tecnologica avanzata fino ad oggi si mantiene all'interno dello spettro di possibilità costituito dal precedente trade-off: soluzioni come le piattaforme Exchange sono infatti un perfetto esempio di soluzione di tipo 1, mentre proposte come Lightning Network o il concetto generale di sidechain si possono considerare alla stregua di 'variazioni' della soluzione di tipo 2.

### Cashu

> Cashu is a free and open-source Chaumian ecash system built for Bitcoin. Cashu offers near-perfect privacy for users of custodial Bitcoin applications. [cashu.space](https://cashu.space/)

Cashu è un protocollo che punta a risolvere il problema della privacy in un contesto di piattaforma custodial, costituendo un'ottima soluzione per utenti non avvezzi a soluzioni self-hosted che vogliono però tutelare maggiormente la propria privacy.

### Come funziona

Di seguito uno schema del funzionamento di Cashu:

- Bob (mint)
    - k private key of mint (one for each amount)
    - K public key of mint
    - Q promise (blinded signature)

- Alice (user)
    - x random string (secret message), corresponds to point Y on curve
    - r private key (blinding factor)
    - T blinded message
    - Z proof (unblinded signature)

![](assets/img/cashu_scheme.png)



### Implementazioni e riferimenti



### Alternative a Cashu: Fedi


## Fonti

- 
- 
- 
- 
- [Chaumian eCash in Bitcoin - Cashu & Fedimint - Adam Gibson aka waxwing](https://www.youtube.com/watch?v=VwMzNE1D3so)




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


