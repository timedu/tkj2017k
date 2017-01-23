---
layout: site_page
title: Kurssin konteksti
permalink: /konteksti/index.html 
---

Ohjelmistotuotanto[^0] on tietotekniikkaa ja johtamista integroiva oppiaine, johon sisältyy olennaisina osina *ohjelmistokehitys* ja *ohjelmistojohtaminen*. *Tietokantajärjestelmät* on opintojakso, joka käsittelee *ohjelmistokehitystä* tähtäimenä tuottaa tietokantoihin perustuvia tietojärjestelmiä. Kurssi antaa valmiuksia käytännön ohjelmistotyöhön tehtäviin, joita rekrytointi-ilmoituksissa  kuvataan termein *database-/backend developer*, *database designer* ja *software architect*.

[^0]: Ohjelmistotuotanto: *"Asiakkaan kohtuulliset odotukset täyttävien tietokoneohjelmien tuottaminen siten, että kehityskustannukset ja aikataulu ovat ennustettavissa riittävällä tarkkuudella"* (Haikala & Mikkonen, 2011)

Seuraavassa kuvataan lyhyesti ensin, miten *Tietokantajärjestelmät* liittyy TTY Porin ohjelmistotekniikan opintoihin ja sitten, mitä ohjelmistotuotannon osa-alueita kurssi sivuaa.

#### *Tietokantajärjestelmät* osana ohjelmistotekniikan opintoja

*Tietokantajärjestelmät* on yksi TTY Porin viidestä tiettyyn tekniseen ympäristöön painottuvasta *ohjelmistokehityksen* opintojaksosta. Tämä *kaaviossa 1* keskellä esitetty kokonaisuus lähestyy teollista ohjelmistotuotantoa siten, että esillä olevat järjestelmät koostuvat tyypillisesti useista komponenteista samalla, kun tarkastelun alla on varsinaisen toteutustyön ohella muut ohjelmistokehityksen keskeiset prosessit.

~~~

  Ohjelmointitekniikka   ⎹                          ⎹     
  Olio-ohjelmointi       ⎹  Mobiiliohjelmointi      ⎹  
  Tietorakenteet         ⎹  Sulautetut järjestelmät ⎹ 
  ⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼ ⎹  Tietokantajärjestelmät  ⎹  Ohjelmistoprojekti
  Tiedonhall. ja tietok. ⎹  Web-palvelinohjelmointi ⎹
  Web-ohjelmointi        ⎹  Web-selainohjelmointi   ⎹ 
  Käyttäjäkesk. suun.    ⎹                          ⎹  
  
~~~
*Kaavio 1.*

Kurssi odottaa osallistujaltaan ohjelmointitaitoja, joita geneerisellä tasolla TTY Porissa tarjoaa yllä olevassa kaaviossa vasemmalla ylhäällä oleva kurssikokonaisuus. *Olio-ohjelmointi* -kurssin tarjoamat tiedot ovat kokonaisuudesta tämän kurssin kannalta keskeisimpiä. Ohjelmointitaitojen lisäksi toivotaan perustietoja *tietokannoista* sekä *web-tekniikoista*, jotka ovat esillä *kaavion 1* vasemmassa alareunassa esitetyillä kahdella kurssilla. *Tietokantajärjestelmät* puolestaan tarjoaa tekniset valmiudet tuottaa *Ohjelmistoprojekti* -opintojakson puitteissa tietokantapohjainen tietojärjestelmä.


#### *Tietokantajärjestelmät* ja ohjelmistotuotannon prosessit

Ohjelmistotuotannosta on esitetty erilaisia prosessimalleja, joista eräs löytyy lähteestä *Haikala & Mikkonen (2011)*[^1]. Malliin viitaten tämä kurssi käsittelee pääosin alla olevassa kaaviossa esitettyjä prosesseja:

~~~
  ⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼
  Suunnittelu │ Ohjelmointi │ Testaus │ Käyttöönotto
  ⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼
  Tuotteen- ja versionhallinta
  ⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼
~~~
*Kaavio 2.*

Kurssin ytimenä on joukko tehtäviä, joiden kuvauksissa on tyypillisesti esitetty ongelmaan liittyvän sekä tietokannan että sovelluksen arkkitehtuuri- ja yksityiskohtaisen tason designit (*suunnittelu*). Tehtävä suoritetaan muuntamalla nämä designit toimivaksi järjestelmäksi (*ohjelmointi*), johon kohdistetaan tehtäväpohjassa mukana olevat automatisoidut yksikkö- ja systeemitason testit (*testaus*). Tehtäväpohjat ladataan versionhallinnan toteuttavasta pilvipalvelusta. Osana kutakin tehtäväpohjaa on mukana määritely, joka esittää kehitettävän sovelluksen riippuvuudet ulkopuolisista moduuleista. Riippuvuudet ladataan ohjelmallisesti osaksi kehitettävää sovellusta (*tuotteen- ja versionhallinta*). Osa tehtävien ratkaisuista toimitetaan ajettavaksi julkisessa pilvipalvelussa (*käyttöönotto*).

*Haikalan ja Mikkosen (2011)*[^1] malli esittää ohjelmistoprojektille seuraavanlaisen prosessirakenteen, jonka osana on edellä (*Kaavio 2*) kuvatut ohjelmistokehityksen  prosessit:

~~~
  ⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼
  Projektinhallinta
  ⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼
  Määrittely │ Suunnittelu │ Ohjelmointi │ Testaus │ Käyttöönotto
  ⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼
  Tuotteen- ja versionhallinta
  ⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼
  Laadunvarmistus
  ⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼
  Dokumentointi
  ⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼
  Vaatimustenhallinta
  ⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼  
~~~
*Kaavio 3.*

Kurssi sivuaa jonkin verran myös muita *Kaaviossa 3* esitettyjä prosesseja.


~~~
  ⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼
  Liiketoiminta ja johtaminen
  ⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼
      Laadunhallintajärjestelmä
      ⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼
          Hankkeiden hallinta
          ⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼
              Ohjelmistoprojekti
              ⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼
              Ohjelmistoprojekti
              ⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼
               ...
  ⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼⎼
~~~
*Kaavio 4.*


<br/>

[^1]: *Haikala, Mikkonen(2011). Ohjelmistotuotannon käytännöt. s.29.*


