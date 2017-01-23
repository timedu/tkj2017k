---
layout: exercise_page
title: "Tehtävä 3.1: Kurssit ja opettajat, LevelDB"
exercise_template_name: W3E01.KurssitJaOpettajatLevel
exercise_discussion_id: 
exercise_upload_id: 
kesken: 1
julkaisu: 24.1.2017
---

Laadi [LevelDB][LevelDB][^a]-tietokantaa käsittelevä sovellus, joka käyttäytyy [Tehtävän 2.1]({{site.baseurl}}/osa2/tehtava21) ratkaisun tapaan.

[LevelDB]: http://leveldb.org
[^a]: avain-arvoparitietokanta

Sovelluksen lähdekoodi jäsentyy seuraavasti:

~~~
Sources
 ├── main.js
 ├── configs
 │     ├── log.js 
 │     ├── db_connection.js 
 │     ├── db_seed.js 
 │     └── db_seed 
 │           ├── kurssit.csv 
 │           └── opettajat.csv  
 ├── controllers
 │     ├── kurssit.js 
 │     └── opettajat.js 
 ├── models
 │     ├── Kurssi.js 
 │     └── Opettajat.js  
 └── views
~~~

<small>Kuva 1. Sovelluksen lähdekoodi</small>

`models`-kansiossa olevia tiedostoja, `Kurssi.js` ja `Opettaja,js`, lukuunottamatta
sovellus on jo rakennettu valmiiksi. Tiedostoihin sisällytetään *LevelDB* -tietokantojen käsittely, johon liittyvä "viittauspolku" on alla olevan kuvan 2 mukainen. 

~~~
[ controllers ] --> [ models ] --> [ db_connection ] --> [ database ] 
~~~

<small>Kuva 2. Viittauspolku konrollereista tietokantaan</small>

`controllers`-kansiossa olevissa tiedostoissa on määritelty funktiot, jotka toteuttavat sovelluksen eri kutsupolkuihin tulevat pyynnöt. *Kontrolleri* toteuttaa tarvittavan tietokantakäsittelyn *mallin* avulla, minkä jälkeen *kotrolleri* tuotaa pyyntöön vasteen hahmontamalla pyyntöön liittyvän *näkymän*. 

Sovelluksen *mallin* tässä muodostaa kaksi oliota, `Kurssi` ja `Opettaja`, joihin on paketoitu metodit `Kurssi.findAll`, `Kurssi.findByKey` ja `Kurssi.findAllByOpettaja` sekä `Opettaja.findAll` ja `Opettaja.findByKey`. Metodesta `findAllByOpettaja` on tehtäväpohjassa valmiina. Muiden osalta pohjassa on rungot, jotka palauttavat vakiotietoja. *Malli* käyttää tietokantayhteyttä, joka on määritelty tiedostossa `db_connection.js`.

Sovelluksella on kaksi erillistä tietokantaa, `kurssi.level` ja `opettaja.level`, jotka sijaitsevat projektin `database`-kansiossa[^1]. Tietokannat datoineen muodostuvat sovelluksen käynnistyksen yhteydessä, jos kansiosta ei löydy em. nimisiä tiedostoja. 

[^1]: `databases`-kansio näkyy NetBeansin *Files*-ikkunassa, mutta ei *Projects*-ikkunassa

**Palauta** tehtävän ratkaisuna tiedostot `models/Kurssi.js` ja `models/Opettaja.js`. Varmista ennen palautusta, että tehtäväpohjassa olevat Selenium-testit menevät läpi. Sovelluksen on oltava käynnissä testejä ajettaessa.

### Vihjeitä ja lisätietoja



<br/>

