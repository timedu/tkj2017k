---
layout: exercise_page
title: "Tehtävä 6.2: Kurssit ja opettajat, Cassandra CRUD"
exercise_template_name: W6E02.KurssitJaOpettajatCassandraCRUD
exercise_discussion_id: 
exercise_upload_id: 
julkaisu: 15.2.2017
kesken: 1
---

Täydennä [edellisen tehtävän](../tehtava61) ratkaisuasi opettatietoihin liittyvillä ylläpitotoiminnolla, *lisäys*, *muutos* ja *poisto*. Tietokanta tässä on aivan sama kuin [Tehtävässä 6.1](../tehtava61). 


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

Mallit ottavat moduulin `configs/db_connection.js` käyttöönsä siten, että tietokanta näkyy tunnisteessa `db`. 

... to be continued ...

#### Toiminnot

Sovelluksen käyttäjän näkökulmasta opettajatietojen ylläpito tapahtuu seuraavan sivukartan mukaan:

![sivukartta](../../osa2/img/w2e03.png)

<small>Kuva 2. Sivukartta: sisältää lomakkeet (*uusi*, *muuta* ja *poista*) tietojen ylläpitoa varten.</small>

Sivukartta voidaan tulkita samalla sovelluksen tilakaavioksi[^3]. `create`-metodin kutsu tapahtuu siirryttäessä *uusi*-tilasta *erittely*-tilaan ja `destroy`-metodin kutsu siirryttäessä *poista*-tilasta *luettelo*-tilaan. *muuta*-tilasta *erittely*-tilaan on kaksi erilaista siirtymää. `update`-metodin kutsu tapahtuu siirtymässä, jonka on pannut liikkeelle käyttäjän tekemä *Talleta*-painikkeen klikkaus.

[^3]: Tehtäväpohjassa oleva kontrolleri toteuttaa tämän tilakaavion

... to be continued ...


#### Palautettava aineisto

**Palauta** tehtävän ratkaisuna tiedosto `models/Opettaja.js`. Varmista ennen palautusta, että sovellus toimii tehtäväkuvauksen mukaan, ajaen sovellusta sekä käyttäen oheistettuja Selenium-testejä[^4]. 

[^4]: Nämä testaavat sovelluksen ulospäin näkyvää toiminnallisuutta eivätkä siten paljasta sisäisiä vikoja.

### Vihjeitä ja lisätietoja


... to be continued ...

<br/>

