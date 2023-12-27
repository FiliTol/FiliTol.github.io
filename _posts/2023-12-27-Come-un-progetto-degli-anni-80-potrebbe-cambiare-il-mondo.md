---
layout: post
title: "Come un progetto dimenticato potrebbe ringiovanire Internet"
categories: Bitcoin
---

## Introduzione

Il 1983 è passato alla storia come uno dei punti di flesso per l'evoluzione delle comunicazioni, con la migrazione di [ARPANET](https://en.wikipedia.org/wiki/ARPANET) dal [NCP](https://en.wikipedia.org/wiki/Network_Control_Protocol_(ARPANET)) al protocollo [TCP/IP](https://en.wikipedia.org/wiki/Internet_protocol_suite) avvenuta il 1° gennaio.
Nel settembre dello stesso anno nasce il progetto [GNU](https://en.wikipedia.org/wiki/GNU_Project), che ha rivoluzionato profondamente l'industria del software fino ai giorni nostri, iniziando *de-facto* il [movimento free-software](https://en.wikipedia.org/wiki/Free_software_movement).

Certamente meno rumoroso rispetto agli avvenimenti citati, anche il paper [*Blind signatures for untraceable payments*](https://www.hit.bme.hu/~buttyan/courses/BMEVIHIM219/2009/Chaum.BlindSigForPayment.1982.PDF) vide la luce nello stesso anno. L'autore, il crittografo statunitense David Chaum, illustra all'interno del paper un metodo rivoluzionario per garantire la verificabilità e l'anonimato dei partecipanti ad una votazione effettuata a mezzo postale.
Nel 1989 lo stesso Chaum fonda Digicash allo scopo di fornire servizi costruiti con l'ausilio degli schemi crittografici da lui ideati.
Nonstante la genialità delle invenzioni di Chaum, Digicash dichiara bancarotta nel 1998, citando - tra gli altri - problemi di scarsa adozione da parte degli istituti di credito.

Nell'intervista [*First Monday Interviews: David Chaum*](https://firstmonday.org/ojs/index.php/fm/article/view/683/593) del 1999 lo stesso Chaum rimarca alcuni dei problemi dei sistema di pagamenti digitali dell'epoca:

> Intervistatore: *Here in Belgium for instance there is Proton, but the problem with this kind of payment system is that everything you buy with it is recorded*.
> David Chaum: *And it's also that people assume that this is not taking place. **The way these products are often promoted to the public is that they are essentially like cash.***

e ancora

> Intervistatore: *Do you think then that the Digicash technology entered the market too early - that people aren't yet used to this kind of technology?*
> David Chaum: *There were things like that. **Electronic commerce was hardly happening at the consumer level when Digicash was trying to gain momentum**. The ease with which things could be integrated into the Web has improved a lot since we were most active. The technological infrastructure was a little less optimal then for us than it is today - as far as making it easy for consumers and merchants to use it. I expect that it will get even easier.*






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


