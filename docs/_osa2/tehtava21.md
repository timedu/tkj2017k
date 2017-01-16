---
layout: exercise_page
title: "Tehtävä 2.1: Kurssit ja opettajat, SQL"
exercise_template_name: W2E01.KurssitJaOpettajatSQL
exercise_discussion_id: 
exercise_upload_id: 
kesken: 1
---

Pohjakoodissa oleva tietokanta sisältää kaksi tietokantataulua: *Kurssi* ja *Opettaja*. Opettajalla voi olla useita kursseja, mutta kurssilla on enintään yksi opettaja. Laadi sovellus, jolla voidaan tarkastella tietokannan sisältämää tietoa. Sovellus käyttäytyy seuraavan sivukartan (Kuva 1) mukaan.


![sivukartta](https://www.lucidchart.com/publicSegments/view/d84f9961-ce43-4b79-bac2-7405afa830ac/image.png)

<small>Kuva 1. Sivukartta</small>

Tehtäessä pyyntö polkuun `/kurssit`, tulee esiin *Kurssit* -sivu, joka esittää luettelon tietokannan sisältämistä kursseista nimen mukaisessa aakkosjärjestyksessä. Kurssin nimi toimii samalla linkkinä, jonka klikkaus tuo esiin *Kurssi* -sivun sisältäen yksittäisen kurssin tiedot. *Kurssi*-sivulle voidaan siirtyä myös suoraan tekemällä pyyntö seuraavanlaiseen polkuun: `/kurssit/PLA-32820` (polun loppuosa on kurssin tunnus). *Kurssi*-sivu esittää myös kurssin opettajan nimen, joka toimii samalla linkkinä *Opettaja* -sivulle.

Tehtäessä pyyntö polkuun `/opettajat`, tulee esiin *Opettaja* -sivu, joka esittää luettelon tietokannan sisältämistä opettajista sukunimen mukaisessa aakkosjärjestyksessä. Opettajan nimi toimii samalla linkkinä, jonka klikkaus tuo esiin *Opettaja* -sivun sisältäen yksittäisen opettajan tiedot. *Opettaja*-sivulle voidaan siirtyä myös suoraan tekemällä pyyntö seuraavanlaiseen polkuun: `/opettajat/5` (polun loppuosa on opettajan id). *Opettaja*-sivu esittää myös aakkosjärjestyksessä olevan luettelon opettajan kursseista, jotka toimivat samalla linkkeinä *Kurssi* -sivuille.

Sovelluksen lähdekoodi jäsentyy seuraavasti:

~~~
Sources
 ├── main.js
 ├── configs
 │     ├── config.js 
 │     └── db 
 │          ├── db_config.js 
 │          ├── data 
 │          │    ├── kurssit.csv 
 │          │    └── opettajat.csv 
 │          └── schema 
 │                ├── createKurssi.sql 
 │                └── createPpettaja.sql 
 ├── controllers
 │     ├── kurssiController.js 
 │     └── opettajaController.js 
 └── views
     ├── kurssi.hbs
     ├── kurssiluettelo.hbs
     ├── opettaja.hbs
     ├── opettajaluettelo.hbs
     └── layouts
          └── main.hbs                 
~~~

<small>Kuva 2. Sovelluksen lähdekoodi</small>

Kontrollereita lukuunottamatta sovellus on jo rakennettu valmiiksi. Kontrollereihin sisällytetään tietokantakäsittely, jota pohjassa on jo aloitettu siten, että *Kurssi*-sivu esittää yksittäisen kurssin tiedot - esim. pyynnön polkuun `/kurssit/PLA-32820` pitäisi esittää *Kurssit*-sivulla  *Mobiiliohjelmointi* -kurssin tiedot.

Pohjakoodi on rakennettu niin, että sovelluksen käynnistyksen yhteydessä muodostuu muistinvarainen tietokanta, jonka taulujen rakenne on määritelty `schema` -kansiossa olevissa tiedostoissa. Tietosisältö löytyy `data` -kansion tiedostoista. `views` -kansion  näkymien koodista selviää, mitä tietoja kontrollerien tulee välittää näkymille. 

**Palauta** tehtävän ratkaisuna tiedostot `kurssiController.js` ja `opettajaController.js`. Varmista ennen palautusta, että tehtäväpohjassa olevat Selenium-testit menevät läpi. Sovelluksen on oltava käynnissä testejä ajettaessa.

### Vihjeitä ja lisätietoja

...
