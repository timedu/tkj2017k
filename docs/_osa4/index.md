---
layout: collection_index
permalink: /:collection/index.html
---

Kurssin osassa 3 käsiteltiin avain-arvoparitietokantoja, joiden yhteydessä fyysistä tietokantaa käsittelemä järjestelmä ei tunne arvojen rakennetta vaan arvojen tulkinnan tekee järjestelmän kautta tietokantaa käsittelevä sovellus. Osan tehtävissä tietokantaan  talletettiin [JSON][JSON]-muodossa olevia merkkijonoja, jotka esimerkkinä käytetty järjestelmä tosin kykeni muuntamaan sovelluksessa käytettäviksi JavaScript-objekteiksi.

[JSON]: http://www.json.org

Dokumenttitietokannoissa[^1] tiedot talletetaan juuri JSON-tyyppisiksi rakenteiksi, joita näiden tietokantojen yhteydessä kutsutaan dokumenteiksi. Erona avain-arvoparitietokantoihin on mm. se, että järjestelmä tunnistaa nämä rakenteet so. järjestelmällä on tiedossaan mitä attribuutteja jokin dokumentti sisältää, mikä mahdollistaa järjestelmälle toteuttaa esim. kyselyissa attribuuttien arvoihin perustuvan valinnan ja lajittelun. 

[^1]: Ks. esim. [ITKA204/Dokumenttitietokannat](https://tim.jyu.fi/view/kurssit/tktl/itka204/kurssimoniste#dokumenttitietokannat)

Tiettävästi [eniten käytetty dokumenttitietokanta][ranking] on [MongoDB][MongoDB], joka lienee samalla myös suosituin NoSQL-tietokanta. Mongon ohella tämän osan tehtävissä on esillä kaksi muuta järjestelmää, joita käytettäessä (Mongosta poiketen) tietokanta voidaan upottaa sovellukseen:  


[ranking]: http://db-engines.com/en/ranking/document+store


> [TingoDB][TingoDB] is embedded JavaScript NoSql database for Node.js and node-webkit. Its API and features designed to be upward compatible with MongoDB and its driver for Node.js. 
>
> [NeDB][NeDB]: Embedded persistent or in memory database for Node.js, nw.js, Electron and browsers, 100% JavaScript, no binary dependency. API is a subset of MongoDB's and it's plenty fast.

[TingoDB]: http://www.tingodb.com
[NeDB]: https://github.com/louischatriot/nedb/blob/master/README.md



### Tehtävät

Tehtävissä jatketaan edelleen *kurssien ja opettajien* käsittelyä. Esillä on kahdella eri tavalla jäsennetty dokumenttitietokanta, joka tehtävästä riippuen sijaitsee joko sovellukseen upotettuna tai verkon yli käytettävässä pilvipalvelussa. Tietokannan hallinta toteutetaan kussakin tehtävässä eri järjestelmällä. 

{% include exercises_list.md %}

[Tehtävässä 4.1](tehtava41) tietokanta on jäsennetty kahdeksi dokumenttikoelmaksi, jotka vastaavat relaatiotietokannan tauluja. Tietokannan rakenne on muutenkin "relaatiomainen": *Kurssi*-dokumentintissa on attribuutti, johon on talletettu kurssin opettajaa vastaavan *Opettaja*-dokumentin tietokantatunniste. Tehtävässä toteutetaan tietokantaan liittyvät kyselyt sekä opettajatietojen osalta ylläpito-operaatiot. Tietokanta on upotettu sovellukseen ja sitä käsitellään [TingoDB][TingoDB]-kirjastolla.

[Tehtävän 4.2](tehtava42) tietokanta muodostuu yhdestä dokumenttikokoelmasta siten, että *Opettaja* esiintyy varsinaisena kokoelman dokumenttina, jonka attribuutin arvona on opettajan pitämien *kurssien* tiedot. Tehtävässä toteutetaan ainoastaan tietokantaan kohdistyvat kyselyt. Edellisen tehtävän tapaan tietokanta on upotettu sovellukseen, mutta sitä käsitellään Tingon sijaan [NeDB][NeDB]-kirjastolla.

[Tehtävässä 4.3](tehtava43) tietokanta siirtyy verkon yli hyödynnettävään pilvipalveluun. Käytettävä [MongoDB][MongoDB]-tietokanta on perustettu [mLab][mLab]:in kautta [Amazon][Amazon]:in pilveen. Tietokannan rakenne vastaa tehtävää 4.1. Tässä toteutetaan dokumenttikokoelmiin kohdistuvat kyselyt. Tehtävän ratkaisu on kyselyjen osalta likimain sama kuin Tehtävässä 4.1. Tietokantayhteys kuitenkin muodostetaan uudelleen kunkin sovellukseen tulevan pyynnön yhteydessä. Valmiin tietokannan sijaan tehtävässä voi käyttää pilvipalveluun itse perustamaansa tietokantaa.

[MongoDB]: https://www.mongodb.com
[mLab]: https://mlab.com
[Amazon]: https://aws.amazon.com

<br/>