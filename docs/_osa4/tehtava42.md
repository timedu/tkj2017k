---
layout: exercise_page
title: "Tehtävä 4.2: Kurssit ja opettajat, NeDB"
exercise_template_name: W4E02.KurssitJaOpettajaNeDB
exercise_discussion_id: 75211
exercise_upload_id: 309701
---

Laadi kyselyominaisuksiltaan [tehtävän 4.1](../tehtava41) ratkaisua vastaava sovellus. Tietokannan rakenne on edelliseen tehtävään verrattuna kuitenkin hieman erilainen. Samalla [TingoDB][tingo]:n korvaa toinen sovellukseen upotettava dokumenttitietokantaratkaisu, [NeDB][NeDB].
 
[tingo]: http://www.tingodb.com
[NeDB]: https://github.com/louischatriot/nedb/blob/master/README.md


#### Mallit ja tietokanta


*Kontrollerit* käyttävät tietokantaa *malleihin* (*Kuva 1*) paketoitujen metodien kautta. 
 
~~~
  +---------------+     +---------------+
  |   Opettaja    |     |    Kurssi     |
  |   <<model>>   |     |   <<model>>   |
  +---------------+     +---------------+
  | findAll       |     | findAll       |
  | findByKey     |     | findByKey     |
  +---------------+     +---------------+
~~~
<small>Kuva 1. Sovelluksen mallit</small>


Malleja, `models/Opettaja.js` ja `models/Kurssi.js`, lukuunottamatta sovellus on rakennettu valmiiksi. Tietokanta jäsentyy tässä seuraavasti:

~~~
  +---------------+    
  |     Koulu     |   
  | <<datastore>> | 
  +---------------+     +-----------+
  | _id           |     | cuid      |
  | etunimi       |     | tunnus    |
  | sukunimi      |     | nimi      |
  | kurssit   ----|-----| laajuus   |
  +---------------+     +-----------+
~~~
<small>Kuva 2. Tietokannan rakenne</small>

Tietokanta sisältää ainoastaan yhden dokumenttikokoelman, josta NeDB:n dokumentaatio käyttää termiä *Datastore*. Kokoelmaan on talletettu sekä opettajien että kurssien tiedot siten, että opettajan tietoihin on sisällytetty opettajan pitämät kurssit. Dokumenttien määrä on sama kuin opettajien lukumäärä.

Dokumentilla (opettajalla) on järjestemän generoima `_id`. Kurssilla on *cuid*-paketilla generoitu yksilöllinen tunniste, `cuid`. 


Tiedosto `configs/db_connection.js` määrittelee tietokantayhteyden, jonka sovelluksen mallit ottavat käyttöönsä siten, että dokumenttikokoelma näkyy viitteelä `db`. Tietokanta sijaitsee projektin `database`-kansiossa nimellä `koulu`[^1]. Tietokanta datoineen muodostuu sovelluksen käynnistyksen yhteydessä, jos kansiossa ei ole em. tiedostoa[^2].

[^1]: Näkyy NetBeansin *Files*-ikkunassa.
[^2]: Päämoduulin `main.js` kutsuma `configs/db_seed.js` rakentaa tietokannan kansiossa `configs/db_seed` olevien csv-tiedostojen pohjalta.

#### Toiminnot

Sovellus toteuttaa edellisistä tehtävistä tutun sivukartan:

![sivukartta](https://www.lucidchart.com/publicSegments/view/d84f9961-ce43-4b79-bac2-7405afa830ac/image.png)

<small>Kuva 3. Sivukartta</small>

Kaikki sivuilla esitetyt luettelot ovat nousevassa aakkosjärjestyksessä. Soveluksen näkymät `kurssi_list`, `opetaja_list` ja `opettaja_detail` tuottavat sivuja, joissa esiintyy luettelomuotoista dataa. Mallien metodien tehtävänä on palauttaa tiedot valmiiksi lajiteltuna. 

Mallien tulee olla rakennettu myös niin, että ne lukevat tietokannasta vain kulloinkin tarvittavat tiedot (ei ylimääräsiä dokumentteja eikä dokumenttien attribuutteja).


#### Palautettava aineisto

**Palauta** tehtävän ratkaisuna tiedostot `models/Opettaja.js` ja `models/Kurssi.js`. Varmista ennen palautusta, että sovellus toimii tehtäväkuvauksen mukaan sovellusta ajamalla[^3]. Tehtävänpohja ei sisällä testikoodia[^4].
 
[^3]: Tarkoitus on, että toimivuus ei edellytä palautettavan moduulin muutosten lisäksi muutoksia muihin moduuleihin. 
[^4]: Testeillä varustettu pohja saatetaan julkaista myöhemmin (tässä voi käyttää Tehtävän 4.1 koodipohjan testejä). 

### Vihjeitä ja lisätietoja

Tiedot tietokankäsittelyn apuneuvoista löytyvät NeDB:n tiiviistä [dokumentaatiosta][NeDB]. Keskeisin kohta on [Finding documents][Finding-documents]. Rajapinta ei juuri poikkea TingoDB:n vastaavasta, mutta NeDB:n dokumentaatio on merkittävästi kattavampi.

[Finding-documents]: https://github.com/louischatriot/nedb/blob/master/README.md#finding-documents

Tiettävästi kurssitietoja ei saa opettajien tapaan lajiteltua tässä ao. näkymiä varten NeDB:n keinoin. Moduulista `configs/utils` löytyy lajittelufunktio `sortBy`, jota voi hyödyntää näissä tilanteissa. Sovelluksen mallit ottavat em. funktion käyttöönsä.

Myös esim. seuraavat taulukko-metodit saattavat olla hyödyllisiä tämän tehtävän ratkaisussa: [concat][concat], [find][find].

[concat]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/concat
[find]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/find

<br/>


