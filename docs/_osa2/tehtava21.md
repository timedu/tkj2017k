---
layout: exercise_page
title: "Tehtävä 2.1: Kurssit ja opettajat, SQL"
exercise_template_name: W2E01.KurssitJaOpettajatSQL
exercise_discussion_id: 
exercise_upload_id: 
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

Seuraavassa *listauksessa 1* on tehtäväpohjassa olevan tiedoston `kurssiController.js` koodi.

{% highlight javascript %}

const SelectOneKurssi = '                       \
SELECT                                          \
    k.*,                                        \
    o.etunimi AS opettaja_etunimi,              \
    o.sukunimi AS opettaja_sukunimi             \
FROM kurssi AS k LEFT OUTER JOIN opettaja AS o  \
    ON k.opettaja_id = o.id                     \
WHERE k.tunnus = ?';

module.exports = (app) => {

    app.get('/kurssit', function (req, res) {        
        res.render('kurssiluettelo');        
    });

    app.get('/kurssit/:tunnus', function (req, res) {
        global.db.get(SelectOneKurssi, req.params.tunnus, (err, row) => {
            res.render('kurssi', {
                kurssi: row
            });
        });
    });
    
};

{% endhighlight %}

<small>Listaus 1.</small>

Moduulista näkyy ulospäin se, joka sijoitetaan muuttujan `module.exports` arvoksi eli tässä funktio, joka määrittelee kahteen polkuun liittyvät funktiot. Nämä siis suoritetaan kun ao. polkuun tulee pyyntö. Koodissa `(app) => { ... }` voisi olla myös `function(app) { ... }`. Sovelluksessa sen käynnistyksen yhteydessä `config.js` kutsuu tämän moduulin tarjoamaa funktiota `app`-parametrilla. 


Polkuun `/kurssit/:tunnus` liittyvä funktio (*Listaus 2*) on koodissa laadittu valmiiksi. Polun osana on muuttuja, `:tunnus`, joka saadaan funktion käyttöön viittauksella  `req.params.tunnus`. 


{% highlight javascript %}

    app.get('/kurssit/:tunnus', function (req, res) {
    
        global.db.get(SelectOneKurssi, req.params.tunnus, (err, row) => {
        
            res.render('kurssi', {
                kurssi: row
            });
            
        });
        
    });


{% endhighlight %}

<small>Listaus 2.</small>


Funktio toteuttaa tietokantakyselyn (`db.get`) ja sitten, kun kyselyn palauttama tulos on käytettävissä, hahmontaa (`render`) tietokannasta luetut tiedot sivulle. Viite tietokantayhteyteen (`db`) on sovelluksessa asetettu[^1] sovelluksen globaalin objektin 
(`global`) ominaisuudeksi. 

[^1]: Tämän tekee `db_config.js`.


[Tietokantarajapinnan][node-sqlite3] [`get`][get] -metodilla on tässä kolme parametria: 1) sql-komento, 2) sql-komennon parametri, ja 3) funktio, joka suoritetaan, kun data tietokannasta on saatu. Data saadaan funktioon sen toisen parametrin kautta objektimuodossa.

[node-sqlite3]: https://github.com/mapbox/node-sqlite3
[get]: https://github.com/mapbox/node-sqlite3/wiki/API#databasegetsql-param--callback

Sql-komento on moduulissa asetettu `SelectOneKurssi`-vakion arvoksi:


{% highlight javascript %}

const SelectOneKurssi = '                       \
SELECT                                          \
    k.*,                                        \
    o.etunimi AS opettaja_etunimi,              \
    o.sukunimi AS opettaja_sukunimi             \
FROM kurssi AS k LEFT OUTER JOIN opettaja AS o  \
    ON k.opettaja_id = o.id                     \
WHERE k.tunnus = ?';

{% endhighlight %}

<small>Listaus 3.</small>


*Listauksessa 3* olevassa sql-komennossa[^2] `?` on paikka parametrille, jonka arvoksi siis asettuu tässä `req.params.tunnus`.

[^2]: SQLite:n  sql-komennot: <https://www.sqlite.org/lang.html>

Tässä käytetään rajapinnan [`get`][get] -metodia, koska kysely palauttaa taulusta ainoastaan yhden rivin. Jos kysely palautta monta riviä, on käytettävä [`all`][all]-metodia.

[all]: https://github.com/mapbox/node-sqlite3/wiki/API#databaseallsql-param--callback

<br/>


