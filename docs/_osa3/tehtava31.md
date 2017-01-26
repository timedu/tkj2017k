---
layout: exercise_page
title: "Tehtävä 3.1: Kurssit ja opettajat, LevelDB"
exercise_template_name: W3E01.KurssitJaOpettajatLevel
exercise_discussion_id: 74589
exercise_upload_id: 308962
---

Laadi [LevelDB][LevelDB][^1]-tietokantaa käsittelevä sovellus, joka käyttäytyy [Tehtävän 2.1]({{site.baseurl}}/osa2/tehtava21) ratkaisun tapaan.

[LevelDB]: http://leveldb.org
[^1]: avain-arvoparitietokanta

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

`controllers`-kansiossa olevissa tiedostoissa on määritelty funktiot, jotka toteuttavat sovelluksen eri kutsupolkuihin tulevat pyynnöt. *Kontrolleri* toteuttaa tarvittavan tietokantakäsittelyn *mallin* avulla, minkä jälkeen *kotrolleri* tuottaa pyyntöön vasteen hahmontamalla pyyntöön liittyvän *näkymän*. 

Sovelluksen *mallin* tässä muodostaa kaksi oliota, `Kurssi` ja `Opettaja`, joihin on paketoitu metodit `Kurssi.findAll`, `Kurssi.findByKey` ja `Kurssi.findAllByOpettaja` sekä `Opettaja.findAll` ja `Opettaja.findByKey`. Metodesta `findAllByOpettaja` on tehtäväpohjassa valmiina. Muiden osalta pohjassa on rungot, jotka palauttavat vakiotietoja. *Malli* käyttää tietokantayhteyttä, joka on määritelty tiedostossa `db_connection.js`.

Sovelluksella on kaksi erillistä tietokantaa, `kurssi.level` ja `opettaja.level`, jotka sijaitsevat projektin `database`-kansiossa[^2]. Tietokannat datoineen muodostuvat sovelluksen käynnistyksen yhteydessä, jos kansiosta ei löydy em. nimisiä tiedostoja. Tietokantojen rakenne vastaa pohjakoodiin oheistettuja csv-tiedostoja (`configs/db_seed/*.csv`) siten, että yksitäisen tiedon (avain-arvopari) *avaimena* on tunnus ja *arvona* json-muotoon koodattu merkkijono kaikista kentistä - esim. *avain*: `PLA-32610` ja siihen liittyvä *arvo*:  `{ "tunnus": "PLA-32610", "nimi": "Tietokantajärjestelmät", "laajuus": "4", "opettaja": "ks" }`[^3].   

[^2]: `databases`-kansio näkyy NetBeansin *Files*-ikkunassa, mutta ei *Projects*-ikkunassa

[^3]: Tiedot on tässä jäsennetty siten kuin se tyypillisesti tehtäisiin relaatiotietokantojen yhteydessä. Ratkaisu ei muodostuisi kovin tehokkaaksi, jos  tietojen määrä kasvaa kovin suureksi. 

**Palauta** tehtävän ratkaisuna tiedostot `models/Kurssi.js` ja `models/Opettaja.js`. Varmista ennen palautusta, että tehtäväpohjassa olevat Selenium-testit menevät läpi. Sovelluksen on oltava käynnissä testejä ajettaessa.

### Vihjeitä ja lisätietoja

Noden LevelDB-rajapinta, [LevelUp][levelup], tarjoaa joukon metodeja, joilla voidaan *lisätä*, *poistaa*, *muuttaa* ja *kysellä* tietokannan avain-arvopareja. Tämän tehtävän ratkaisussa tietokantaan kohdistetaan ainoastaan *kyselyjä*.  

Yksittäisen arvon avaimen perusteella voi hakea tietokannasta [`get`][get] -metodilla ja tietokannan kaikki arvot [`createValueStream`][createValueStream] -metodilla. 


[levelup]: https://github.com/Level/levelup/blob/master/README.md
[get]: https://github.com/Level/levelup/blob/master/README.md#get
[createValueStream]:  https://github.com/Level/levelup/blob/master/README.md#createValueStream


{% highlight javascript %}

db.get(key, function (err, value) {
  // ... 
});

{% endhighlight %}

<small>Listaus 1. *get*-metodi</small>


Pelkistetyssä muodossa `get`-metodin kutsussa on kaksi parametria  - ensimmäisenä *avain* ja toisena funktio, joka suoritetaan, kun *arvo* tietokannasta on saatu. Jos *arvo* on tietokannassa [JSON][JSON]-muodossa oleva merkkijono, sen saa JavaScript -objektina ilmaisemalla koodaus metodikutsun yhteydessä:


{% highlight javascript %}

db.get(key, {valueEncoding: 'json'}, function (err, value) {
  // ... 
});

{% endhighlight %}

<small>Listaus 2. JSON-koodatun arvon luku *get*-metodilla</small>


[JSON]: http://www.json.org


Luettaessa tietokannasta kaikki arvot `createValueStream`-metodilla, luvun etenemistä voi seurata erilaisten tapahtumakäsittelijöiden avulla: 


{% highlight javascript %}

db.createValueStream()
  .on('data', function (data) {
    // ...
  })
 .on('end', function () {
    // ...
  });

{% endhighlight %}

<small>Listaus 3. *createValueStream*-metodi</small>


*Listauksessa 3* on määritelty käsittelijät `data`- ja `end`-tapahtumiin. `data`-tapahtuma laukeaa aina, kun tietokannasta on luettu arvo, ja `end` silloin, kun kaikki arvot on luettu. 

Seuraavassa on tehtävän pohjakoodista (`models/Kurssi.js`) poimittu metodi, joka hakee tietokannasta kaikki tietyn opettajan kurssit:  


{% highlight javascript %}

Kurssi.findAllByOpettaja = (opettajaTunnus, callback) => {

   const opettajanKurssit = [];

   db.kurssi.createValueStream({
      valueEncoding: 'json'
   }).on('data', (kurssi) => {
      if (kurssi.opettaja === opettajaTunnus) {
         opettajanKurssit.push(kurssi);
      }
   }).on('end', () => {
      callback(opettajanKurssit);
   });

};

{% endhighlight %}

<small>Listaus 4. Esimerkki *createValueStream*-metodin käytöstä rakennettavassa sovelluksessa</small>


Muuttujassa `db.kurssi` on viite *Kurssit* -tietokantaan. `createValueStream`-metodi lukee kaikki tietokannan sisältämät kurssit. `data`-tapahtumaan reagoidaan asettamalla luettu `kurssi` `opettajanKurssit`-taulukkoon, jos kurssin tiedoissa on ao. opettajan tunnus. `end`-tapahtumaan reagoidaan kutsumalla parametrina saatua `callback`-funktiota, jolle annetaan opettajan kurssit sisältävä taulukko.


*Listauksen 5* sisältämä koodi (`controllers/opettajat.js`) käyttää *listauksessa 4* määriteltyä metodia `Kurssi.findAllByOpettaja`.


{% highlight javascript %}

router.get('/:key', (req, res) => {

   Opettaja.findByKey(req.params.key, (opettaja) => {

      Kurssi.findAllByOpettaja(opettaja.tunnus, (kurssit) => {
         
         opettaja.kurssit = kurssit;
         
         res.render('opettaja_detail', {
            opettaja: opettaja
         });

      });
   });

});

{% endhighlight %}

<small>Listaus 5. Laaditun *Kurssi.findAllByOpettaja*-metodin hyödyntäminen kontrollerissa</small>


`findAllByOpettaja` -metodin kutsun toisena parametrina on funktio, jota metodi kutsuu, kun tarvittava data on saatu tietokannasta. Tässä tietokannasta luetut `kurssit` asetataan `opettaja`-olion ominaisuudeksi, jonka jälkeen opettaja kursseineen hahmonnetaan näkymällä `views/opettaja_detail.hbs`.




<br/>

