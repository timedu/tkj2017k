---
layout: exercise_page
title: "Tehtävä 5.2: Kurssit ja opettajat, LevelGraph CRUD"
exercise_template_name: W5E02.KurssitJaOpettajatLevelGraphCRUD
exercise_discussion_id: 
exercise_upload_id: 
---

Täydennä [edellisen tehtävän](../tehtava51) ratkaisuasi opettatietoihin liittyvillä ylläpitotoiminnolla, *lisäys*, *muutos* ja *poisto*. Tietokanta tässä on pientä nyanssia lukuun ottamatta samarakenteinen.


#### Mallit ja tietokanta

Opettajatietoihin liittyvään malliin on lisätty ylläpitotoimintoja varten kolme metodia, `create`, `update` ja `destroy`:

~~~
  +---------------+     +---------------+
  |   Opettaja    |     |    Kurssi     |
  |   <<model>>   |     |   <<model>>   |
  +---------------+     +---------------+
  | findAll       |     | findAll       |
  | findByKey     |     | findByKey     |
  | create        |     +---------------+
  | update        |
  | destroy       |
  +---------------+
~~~
<small>Kuva 1. Sovelluksen mallit</small>


Malleja, `models/Opettaja.js` ja `models/Kurssi.js`, lukuunottamatta sovellus on rakennettu valmiiksi. `findAll`- ja `findByKey` -metodit voit kopioida edellisen tehtävän ratkaisustasi, joten laadittavaksi tässä jää metodit `create`, `update` ja `delete`.

Mallit ottavat moduulin `configs/db_connection.js` käyttöönsä siten, että tietokanta näkyy tunnisteissa `db.level` ja `db.graph`, josta ensimmäinen tarjoaa tietokantaan [LevelUp][LevelUp] -rajapinnan[^1] ja jälkimmäinen [LevelGraph][LevelGraph] -rajapinnan. 


[LevelUp]: https://github.com/Level/levelup/blob/master/README.md
[LevelGraph]: https://github.com/mcollina/levelgraph/blob/master/README.md

[^1]: Rajapintaa käytetty [Osan 3](../../osa3) tehtävissä.

Tietokannan rakenne poikkeaa [Tehtävän 5.1](../tehtava51) vastaavasta siten, että tässä *on_opettaja*- ja *on_kurssi* -kolmikkojen (ks. [edellisen tehtävän](../tehtava51) *Listaukset 1 ja 2*) `object`-attribuutin arvona on JavaScript -objektin sijaan vastaava *JSON*-merkkijono[^2]. Pohjakoodissa olevan `normalize`-funktion toteutuksessa on huomioitu tämä ero.

[^2]: Ilmeisesti (?) tehtävässä 5.1 *js*-objektien tallettaminen graafiin onnistuu, koska talletuksen yhteydessä tietokanta on tyhjä 

#### Toiminnot

Sovelluksen käyttäjän näkökulmasta opettajatietojen ylläpito tapahtuu seuraavan sivukartan mukaan:

![sivukartta](../../osa2/img/w2e03.png)

<small>Kuva 2. Sivukartta: sisältää lomakkeet (*uusi*, *muuta* ja *poista*) tietojen ylläpitoa varten.</small>

Sivukartta voidaan tulkita samalla sovelluksen tilakaavioksi[^3]. `create`-metodin kutsu tapahtuu siirryttäessä *uusi*-tilasta *erittely*-tilaan ja `destroy`-metodin kutsu siirryttäessä *poista*-tilasta *luettelo*-tilaan. *muuta*-tilasta *erittely*-tilaan on kaksi erilaista siirtymää. `update`-metodin kutsu tapahtuu siirtymässä, jonka on pannut liikkeelle käyttäjän tekemä *Talleta*-painikkeen klikkaus.

[^3]: Tehtäväpohjassa oleva kontrolleri toteuttaa tämän tilakaavion


Opettajan *lisäys* tapahtuu tallettamalla tietokantaan *on_opettaja*-kolmikko (ks. [edellisen tehtävän](../tehtava51) *Listaus 1*). Talletuksen yhteydessä on huomioitava, että kolmikon `object`-attribuutin arvoksi tulee *JSON*-merkkijono.

*Muutos* edellyttää kahta operaatiota, koska kolmikkoihin ei liity erillistä muutos-metodia. Muutettava opettaja on poistettava ja sen tilalle on talletettava uusi. Tämän operaatioparin tulisi olla jakamaton. 


*Poistettaessa* opettajaa on tietokannasta poistettava ao. *on_opettaja* -kolmikon lisäksi myös kaikki opettajaan liittyvät *opettaa* -kolmikot (ks. [edellisen tehtävän](../tehtava51) *Listaus 3*). Näidenkin operaatioiden tulisi olla jakamattomia.


#### Palautettava aineisto

**Palauta** tehtävän ratkaisuna tiedosto `models/Opettaja.js`. Varmista ennen palautusta, että sovellus toimii tehtäväkuvauksen mukaan, ajaen sovellusta sekä käyttäen oheistettuja Selenium-testejä[^4]. 

[^4]: Nämä testaavat sovelluksen ulospäin näkyvää toiminnallisuutta eivätkä siten paljasta sisäisiä vikoja.

### Vihjeitä ja lisätietoja


#### Sovellusrajapinnoista

Kolmikon *lisäyksen* tietokantaan voidaan tehdä [LevelGraph][LevelGraph] -sovellusrajapinnan [`put`][put]-metodilla ja *poiston* [`del`][del]-metodilla. *put* -metodin tapaan *del* kohdistuu vain yhteen kolmikkoon.

[LevelUp][LevelUp]-rajapinta tarjoaa metodin [`batch`][batch], jota käyttäen erillsiä operaatioita voidaan niputtaa yhteen[^5] [^6]. *batch* -metodille annetaan parametrina taulukko suoritettavista ylläpito-operaatioista. *LevelGraph* -kolmikkoon kohdistuvan *put/del* -operaation saa *batch* -metodille parametrina annettavaan muotoon rajapinnan [`generateBatch`][generateBatch] -metodilla.


[put]: https://github.com/mcollina/levelgraph#get-and-put
[del]: https://github.com/mcollina/levelgraph#deleting
[batch]: https://github.com/Level/levelup/blob/master/README.md#batch
[generateBatch]: https://github.com/mcollina/levelgraph#generate-batch-operations

[^5]: Tässä oletetaan, että *batch* muodostaa jakamattoman ("kaikki tai ei mitään") operaatiokokonaisuuden.

[^6]: Esim. [Tehtävän 3.2](../../osa3/tehtava32) ratkaisu edellytti *batch*-metodin käyttöä. 


#### Muista apuneuvoista

Seuraavat apuneuvot saattavat olla tarpeellisia tehtävän ratkaisussa:

* JavaScript-objektin voi muuntaa JSON-merkkijonoksi funktiolla [`JSON.stringify`][JSON.stringify]. 
* Taulukoita voi liittää taulukon yhteen [`concat`][concat] -metodilla.


[JSON.stringify]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify
[concat]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/concat


<br/>
