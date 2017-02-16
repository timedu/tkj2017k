---
layout: collection_index
permalink: /:collection/index.html
---

Sarakeperhetietokannoista[^1] tiettävästi [eniten käytetty][ranking] on [Apache Cassandra][cassandra], joka toimii tämän osan esimerkkijärjestelmänä. Cassandran sivustolla tuotteesta mainitaan mm. seuraavaa:

[^1]: Ks. esim. [JYU/ITKA204/sarakeperhetietokannat]( https://tim.jyu.fi/view/kurssit/tktl/itka204/kurssimoniste#sarakeperhetietokannat)

[ranking]: http://db-engines.com/en/ranking/wide+column+store
[cassandra]: http://cassandra.apache.org


> The Apache Cassandra database is the right choice when you need scalability and high availability without compromising performance. Linear scalability and proven fault-tolerance on commodity hardware or cloud infrastructure make it the perfect platform for mission-critical data.

Järjestemän käyttö muistuttaa huomattavasti relaatiotietokannan käsittelyä, koska käytettävän *CQL*-kielen komennot ovat usein täysin samoja kuin vastaavat *SQL*-komennot. Tietokannan muodostamisperiaate kuitenkin poikkeaa huomattavasti perinteisesti relaatiotietokantojen yhteydessä noudatetusta käytännöstä.

Relaatiotietokantojen taulut tyypillisesti normalisoidaan siten, että tietojen ylläpito kohdistuu yleensä ainoastaan yhteen tietokokonaisuuteen. Tällöin kyselytarpeet toteutetaan monen taulun liitoksilla. *Cassandran* yhteydessä tilanne on päinvastainen. Järjestelmä ei sisällä lainkaan liitos-operaatiota. Tietokanta de-normalisoidaan siten, että kulloinkin tarvittavat tiedot voidaan hakea ainoastaan yhdestä taulusta.

Relaatiotietokannassa avain yksilöi taulun rivin. Avain on sellainen, että avaimesta ei voi poistaa yhtään osaa ilman, että samalla menetetään yksilöivyys. Relaatiotietokannan avaimen valinnalla ei myöskään suoraan oteta kantaa tiedon talletusrakenteeseen. Nämäkin asiat ovat *Cassandran* yhteydessä hieman eri tavalla. Avain yksilöi tiedon, mutta useinkaan se ei ole minimalistinen[^2]. Avain myös osaltaan määrittelee tietojen sijoitusta ja järjestystä tietovälineellä. 

[^2]: Avaimessa on yksilöivyyden näkökulmasta turhia osia. 

### Tehtävät

Kurssin tämän osan tehtävistä kaksi ensimmäistä ovat ohjelmointitehtäviä, jotka perustuvat Cassandra-tietokannaksi jäsennettyyn *Kurssit ja opettajat*-aineistoon. Kolmannen tehtävän muodostaa 12 monivalintatehtävää, joiden lähtökohtana on joukko Cassandra:a esitteleviä lyhyitä videoita.

{% include exercises_list.md %}

Tehtävät [6.1](tehtava61) ja [6.2](tehtava62) ovat Cassandra -vastineita [edellisen osan](../osa5) Tehtäville [5.1](../osa5/tehtava51) ja [5.2](../osa5/tehtava52): sovellukseen laaditaan ensin kyselyt ja sitten sitä täydennetään opettajatietojen ylläpito-operaatioilla. [Tehtävän 6.3](tehtava63) videoaineisto on osa Datastaxin[^3] [Data Modeling][data-modeling] -verkkokurssia, jota voi opiskella veloituksetta. 

[^3]: [Datastax](http://www.datastax.com/company) on firma, joka tarjoaa kaupallisia Cassandraan perustuvia tuotteita ja palveluja.

[data-modeling]: https://academy.datastax.com/resources/ds220-data-modeling

<br/>

