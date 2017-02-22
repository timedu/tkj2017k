---
layout: collection_index
permalink: /:collection/index.html
---

NoSQL -tietokannoille esitetään tyypillisesti neljää päätyyppiä, jotka kaikki ovat olleet esillä osissa 3-6. Kurssi [osassa 4](../osa4) käsiteltiin dokumenttitietokantoja (esim. [MongoDB][MongoDB]) ja [osassa5](../osa5) graafitietokantoja (esim. [Neo4j][Neo4j]). Tässä osassa esimerkkinä oleva tietokanta, [OrientDB][OrientDB], on samalla sekä dokumenttitietokanta että graafitietokanta. Dokumenttien välille voidaan muodostaa linkkejä myös ilman graafien käyttöä.

[MongoDB]: https://www.mongodb.com
[Neo4j]: https://neo4j.com
[OrientDB]: http://orientdb.com

OrientDB-tietokannan tietueet perustuvat ennalta määriteltyihin luokkiin (Class), joille voidaan määritellä ominaisuuksia ja rajoitteita. Luokat voivat muodostaa perintähierarkkian. Näin OrientDB:llä on oliopiirteitä sen lisäksi että se toteuttaa dokumentti- ja graafityyppiset tietomallit. Luokkien, ominaisuuksien ja rajoitteiden määrittelyjen kautta tietokannalle muodostuu skeema, jonka ei kuitenkaan tarvitse olla kattava so. tietue voi sisältää ominaisuuksia, joita ei ole määritelty skeemassa. Tietokantaa käsitellään käyttäen SQL -kieltä, joka huomioi OrientDB:n moninaiset tietojen jäsennysmallit.

### Tehtävät

Opintojakson tähän osaan sisältyy kaksi ohjelmointitehtävää, joista ensimmäisessä tietokanta perustuu dokumenttimalliin. Toisessa tehtävässä tukeudutaan OrientDB:n  graafiominaisuuksiin. Kolmannen tehtävän muodostaa 12 monivalintatehtävää, joiden taustalla on joukko OrientDB:n ominaisuuksia esitteleviä lyhyitä videoita.

{% include exercises_list.md %}

[Tehtävässä 7.1](tehtava71) käytettävään *Kurssit ja opettajat* -dokumenttitietokantaan kohdistetaan sekä kyselyjä että ylläpito-operaatioita. Tietueiden väliset yhteydet on toteutettu linkki-ominaisuuksien avulla. Tietokanta jäsentyy melko pitkälle relaatiotietokannan tapaan. [Tehtävän 7.2](tehtava72) *Kurssit ja opettajat* -tietokanta rakentuu graafitietokannan elementeistä, solmuista ja kaarista. Kurssien ja opettajien välisten kaarien lisäksi tietokannassa on kurssien välisiä esitietovaatimuuksiin liittyviä kaaria. Tietokantaan kohdistetaan ainoastaan kyselyjä. [Tehtävän 7.3](tehtava73) videoaineisto on osa [Udemy][Udemy] -alustalle toteutettua [OrientDB - Getting Started][OrientDB-Udemy] -verkkokurssia, jota voi opiskella veloituksetta. 

[Udemy]: https://www.udemy.com/
[OrientDB-Udemy]: https://www.udemy.com/orientdb-getting-started/


