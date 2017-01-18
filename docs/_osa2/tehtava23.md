---
layout: exercise_page
title: "Tehtävä 2.3: Kurssit ja opettajat, CRUD"
exercise_template_name: W2E03.KurssitJaOpettajatCRUD
exercise_discussion_id: 73565
exercise_upload_id: 307539
julkaisu: 18.1.2017
kesken: 1
---

Täydennä [Tehtävän 2.2](../tehtava22) ratkaisua[^1] siten, että se sisältää toiminnot tietokannan tietojen ylläpitoa[^2] varten.   

[^1]: tietojen kyselyihin liittyen tässä on edellisiin tehtäviin verrattuna sellainen pieni muutos, että yksittäisen kurssin tiedot haetaan `id`-attribuutin perusteella `tunnus`-attribuutin sijaan 

[^2]: lisäys, muutos ja poisto

Edellisten tehtävien ([2.1](../tehtava21) ja [2.2](../tehtava22)) ratkaisujen sivukartta on seuraavanlainen:

![sivukartta](https://www.lucidchart.com/publicSegments/view/d84f9961-ce43-4b79-bac2-7405afa830ac/image.png)

<small>Kuva 1. Sivukartta: Kurssien ja opettajien *luettelot* ja *eritelyt*.</small>

Tässä sivukartta laajenee sekä kurssien että opettajien osalta siten, että kuhunkin tietojen yllöpito-operaatioon (lisäys, muutos, poisto) liittyy oma lomakesivunsa (*Kuva 2*).

![sivukartta](../img/w2e03.png)

<small>Kuva 2. Sivukartta: lisätty lomakkeet (*uusi*, *muuta* ja *poista*) tietojen ylläpitoa varten.</small>

*Luettelo*-sivulta voidaa siirtyä lomakkeelle (*uusi*), jolla voi syöttää uuden rivin tietokantaan. Muille lomakkeille (*muuta*, *poista*) voidaan siirtyä *erittely*-sivulta. Kullakin lomakkeella voi joko vahvistaa tai peruuttaa ylläpito-operaation ao. painikkeiden avulla. *Kuvassa 2* on esitetty, mille sivuille käsittely siirtyy lomakesivuita painikkeiden klikkauksen seurauksena. 

Sovelluksen lähdekoodi jäsentyy edellisen tehtävän kanssa samalla tavalla (*Kuva 3*). Näkymiä rakenteessa on kuitenkin aiempaa enemmän.

~~~
Sources
 ├── main.js
 ├── configs
 ├── controllers
 │     ├── kurssiController.js 
 │     └── opettajaController.js 
 ├── models
 └── views
     ├── kurssi.hbs
     ├── kurssi_list.hbs
     ├── kurssi_insert.hbs
     ├── kurssi_update.hbs
     ├── kurssi_delete.hbs
     ├── opettaja.hbs
     ├── opettaja_list.hbs
     ├── opettaja_insert.hbs
     ├── opettaja_update.hbs
     ├── opettaja_delete.hbs
     └── layouts
          └── main.hbs                 
~~~

<small>Kuva 3. Sovelluksen lähdekoodin struktuuri.</small>

Kontrollereita lukuunottamatta sovellus on jo rakennettu valmiiksi. Kontrollereihin sisällytetään ORM-rajapinnan kautta tapahtuva tietokantakäsittely. 

Tietokanta sijaitsee projektin `database`-kansiossa nimellä `koulu.sqlite`[^3]. Tietokanta datoineen muodostuu sovelluksen käynnistyksen yhteydessä, jos kansiosta ei löydy em. nimistä tiedostoa. Tietokanta näkyy kontrollereissa viittauksilla `global.Kurssi` ja `global.Opettaja`, jotka ovat Sequelizen [Model][model]-objekteja. 

[^3]: `databases`-kansio näkyy NetBeansin *Files*-ikkunassa, mutta ei *Projects*-ikkunassa

**Palauta** tehtävän ratkaisuna tiedostot `kurssiController.js` ja `opettajaController.js`. Varmista ennen palautusta, että tehtäväpohjassa olevat Selenium-testit menevät läpi[^4]. Sovelluksen on oltava käynnissä testejä ajettaessa.

[^4]: testit eivät tosin testaa ylläpito-operaatioita, joten niiden todentaminen ja kelpoistaminen tulee suorittaa sovellusta ajamalla


### Vihjeitä ja lisätietoja



<br/>

