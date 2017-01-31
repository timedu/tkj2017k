---
layout: exercise_page
title: "Tehtävä 4.1: Kurssit ja opettajat, TingoDB 1"
exercise_template_name: W4E01.KurssitJaOpettajatTingo1
exercise_discussion_id: 
exercise_upload_id: 
julkaisu: 31.1.2017
kesken: 1
---

Laadi  ulkoisilta ominaisuksiltaan [tehtävän 3.3](../../osa3/tehtava33) ratkaisua vastaava sovellus. Avain-arvoparitietokannan sijaan tässä käytetään kuitenkin [TingoDB][tingo] -dokumettitietokantaa, jonka sovellusrajapinta on yhteensopiva [MongoDB:n rajapinnan][mongo-api] kanssa[^0].

[tingo]: http://www.tingodb.com
[mongo-api]: http://mongodb.github.io/node-mongodb-native

[^0]: Tingo toteuttaa vain osan Mongon sovellusraajapinnasta.


#### Mallit ja tietokanta


*Kontrollerit* käyttävät tietokantaa *malleihin* (*Kuva 1*) paketoitujen metodien kautta. 
 
~~~
  +---------------+     +---------------+
  |   Opettaja    |     |    Kurssi     |
  |   <<model>>   |     |   <<model>>   |
  +---------------+     +---------------+
  | findAll       |     | findAll       |
  | findByKey     |     | findByKey     |
  | create        |     +---------------+s
  | update        |
  | destroy       |
  +---------------+
~~~
<small>Kuva 1. Sovelluksen mallit</small>


Sovellus on rakennettu valmiiksi lukuun ottamatta tietostoa `models/Opettaja.js`. Tietokanta rakentuu tässä seuraavasti:

~~~
  +----------------+     +----------------+
  |     Kurssi     |     |    Opettaja    |
  | <<collection>> |     | <<collection>> |
  +----------------+     +----------------+
  | _id            |     | _id            |
  | tunnus         |     | etunimi        |
  | nimi           |     | sukunimi       |
  | laajuus        |     +----------------+
  | opettaja       |
  +----------------+
~~~
<small>Kuva 2. Tietokannan rakenne</small>

Tietokanta on jäsennetty samoin kuin se tyypillisesti tehtäisiin käytettäessä relaatiotietokantaa. Tietokannassa on kaksi dokumenttikokoelmaa, jotka tässä vastaavat relaatiotietokannan tauluja. Kokoelmien dokumenteilla on järjestelmän automaattisesti generoima yksilöllinen avain, `_id`.  *Kurssi*-kokoelman dokumenteilla on `opettaja`-attribuutti, jonka sisältönä on kurssin opettajan `_id`. Järjestelmä ei mitenkään ylläpidä tätä yhteyttä (vs. relaatiotietokannan *primary key* - *foreign key* -riippuvuudet).

Tiedosto `configs/db_connection.js` määrittelee tietokantayhteyden, jonka sovelluksen mallit ottavat käyttöönsä siten, että kokoelmat näkyvät viitteillä `db.opettajat` ja `db.kurssit`. Tietokanta sijaitsee projektin `database`-kansiossa[^1], jossa kokoelmat näkyvät omina tiedostoinaan. Tietokanta datoineen muodostuu sovelluksen käynnistyksen yhteydessä, jos kansiossa ei ole kokoelmatiedostoja[^2].

[^1]: Näkyy NetBeansin *Files*-ikkunassa.
[^2]: Päämoduulin `main.js` kutsuma `configs/db_seed.js` rakentaa tietokannan kansiossa `configs/db_seed` olevien csv-tiedostojen pohjalta.

#### Toiminnot

Sovellus toteuttaa aikaisemmissa tehtävissä esillä olleet sivukartat (ks. esim. [Tehtävä 2.3](../../osa2/tehtava23)). Ylläpitotoiminnot tässä kohdistuvat tosin ainoastaan opettajan tietoihin. Opettajan *poiston* yhteydessä tulee huomioida, että poistetun opettajan `_id` ei jää *Kurssi*-dokumentteihin. Jos kurssilla ei ole opettajaa, *kurssi*-dokumentin `opettaja`-attribuuntin arvona on `0`. 

Kaikki sivuilla esitetyt luettelot ovat nousevassa aakkosjärjestyksessä. Soveluksen näkymät `kurssi_list`, `opetaja_list` ja `opettaja_detail` tuottavat sivuja, joissa esiintyy luettelomuotoista dataa. Mallien metodien tehtävänä on palauttaa tiedot valmiiksi lajiteltuna.

#### Palautettava aineisto

**Palauta** tehtävän ratkaisuna tiedosto `models/Opettaja.js`. Varmista ennen palautusta, että sovellus toimii tehtäväkuvauksen mukaan sovellusta ajamalla[^3]. Tehtävänpohja ei sisällä testikoodia[^4].
 
[^3]: Tarkoitus on, että toimivuus ei edellytä palautettavan moduulin muutosten lisäksi muutoksia muihin moduuleihin. 
[^4]: Testeillä varustettu pohja saatetaan julkaista myöhemmin. 

### Vihjeitä ja lisätietoja

...




<br/>

