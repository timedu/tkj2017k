---
layout: collection_index
permalink: /:collection/index.html
kesken: 1
julkaisu: 9.2.2017
---

[JYU/ITKA204/graafitietokannat]( https://tim.jyu.fi/view/kurssit/tktl/itka204/kurssimoniste#graafitietokannat)

<http://db-engines.com/en/ranking/graph+dbms>


[Neo4j]: https://neo4j.com
[LevelGraph]: https://github.com/mcollina/levelgraph/blob/master/README.md
[GrapheneDB]: http://www.graphenedb.com
[Amazon]: https://aws.amazon.com

### Tehtävät

Tämän osan tehtävissä *Kurssit ja opettajat* on ensin jäsennetty [LevelGraph][LevelGraph] -tietokannan tukemiksi kolmikoiksi. Tietokanta on tässä ratkaisussa upotettu sovellukseen. Toisena jäsennyksenä on [Neo4j][Neo4j]:n graafit. Tuolloin tietokanta sijaitsee verkon yli käytettävässä pilvipalvelussa. 

{% include exercises_list.md %}

[Tehtävässä 5.1](tehtava51) *LevelGraph* -tietokanta muodostuu kolmikoista, joissa predikaattina esiintyy joko *on_opettaja*, *on_kurssi* tai *opettaa*. Opettajien tiedot on talletettu *on_opettaja* -kolmikkoihin ja kurssien tiedot *on_kurssi* -kolmikkoihin. *opettaa* -kolmikoilla määritellään opettajien ja kurssien väliset predikaatin mukaiset yhteydet. Tehtävässä toteutetaan ainoastaan tietokantaan kohdistuvat kyselyt. [Tehtävässä 5.2](tehtava52) ratkaisua täydennetään opettajan tietoihin kohdistuvilla ylläpito-operaatioilla, *lisäys*, *muutos* ja *poisto*.

[Tehtävän 5.3](tehtava53) *Neo4j* -tietokanta on perustettu [GrapheneDB][GrapheneDB]-palvelun kautta [Amazon][Amazon]:in pilveen. Tietokanta on edellisiin tehtäviin verrattuna laajempi sisältäen nyt myös kurssien välisiä yhteyksiä. Tässä tietokanta muodostuu solmuista ja kaarista. Opettajien tiedot on talletettu *Opettaja* -otsikolla varustettuihin solmuihin ja kurssien tiedot solmuihin, joilla on otsikko *Kurssi*. *Kurssi*- ja *Opettaja* -solmujen välillä on *OPETTAA* -otsikolla varustettuja kaaria. Kurssien väliset keskinäiset yhteydet on kuvattu tallettamalla tietokantaan *ON_ESITIETO* -kaaria. Tehtävässä toteutetaan ainoastaan kurssitietoihin kohdistuvat kyselyt. Valmiin tietokannan sijaan voi tukeutua itse perustamaansa tietokantaan.
