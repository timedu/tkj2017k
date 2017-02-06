---
layout: exercise_page
title: "Tehtävä 5.1: Kurssit ja opettajat, LevelGraph"
exercise_template_name: W5E01.KurssitJaOpettajatLevelGraph
exercise_discussion_id: 
exercise_upload_id: 
---

Laadi ulkoisilta ominaisuksiltaan edellisten tehtävien ratkaisujen kaltainen sovellus, jolla voidaan tarkastella *kurssi- ja opettajatietoja* sekä *yhteenveto-* että *erittelymuotoisten* näkymien kautta. Taustalla oleva tietokantaratkaisu on *triplestore* -tyyppinen [LevelGraph][LevelGraph], joka rakentuu kurssin [osassa 3](../../osa3) esillä olleen [LevelUp][LevelUp] -avain-arvoparitietokannan päälle.  

[LevelGraph]: https://github.com/mcollina/levelgraph/blob/master/README.md
[LevelUp]: https://github.com/Level/levelup/blob/master/README.md

#### Mallit ja tietokanta

Edellisten tehtävien tapaan  palvelupyyntöihin vastaavat *kontrollerit* käyttävät tietokantaa *malleihin*  paketoitujen metodien kautta (*Kuva 1*). 
 
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


Malleja, `models/Opettaja.js` ja `models/Kurssi.js`, lukuunottamatta sovellus on rakennettu valmiiksi. Mallit ottavat moduulin `configs/db_connection.js` käyttöönsä siten, että tietokanta näkyy tunnisteessa `db.graph`.


Tietokanta rakentuu  *subject-predicate-object* -kolmikoista, joita tietokannassa on kolmenlaisia (käyttäen luokitteluperusteena prediaatteja). Seuraavissa *Listauksissa 1-3* on esimerkki kustakin kolmikkotyypistä.


{% highlight json %}

{ 
  "subject": "ciytxo6qr0005fu1k6hzpolnb",
  "predicate": "on_opettaja",
  "object": { 
      "sukunimi": "Käkilä", 
      "etunimi": "Simo" 
  } 
}

{% endhighlight %}

<small>Listaus 1. Esimerkki *on_opettaja*-kolmikosta</small>



{% highlight json %}

{ 
  "subject": "ciytxo6qr000efu1kdj1plost",
  "predicate": "on_kurssi",
  "object": { 
      "tunnus": "PLA-32831",
      "nimi": "Web-selainohjelmointi",
      "laajuus": "4" 
  } 
}


{% endhighlight %}

<small>Listaus 2. Esimerkki *on_kurssi*-kolmikosta</small>



{% highlight json %}

{
  "subject": "ciytxo6qr0005fu1k6hzpolnb",
  "predicate": "opettaa",
  "object": "ciytxo6qr000efu1kdj1plost" 
}


{% endhighlight %}

<small>Listaus 3. Esimerkki *opettaa*-kolmikosta</small>

Tietokantaa hallinnoiva järjestelmä tunnistaa kolmikkojen attribuutit `subject`, `predicate` ja `object`, mutta ei niiden sisäistä rakennetta. *on_opettaja*- ja *on_kurssi*-kolmikkojen (*Listaukset 1 ja 2*) `subject`-attribuutin arvot on generoitu jo aikaisemmissa tehtävissä esillä olleella `cuid`-funktiolla.


Tietokanta on verrattavissa relaatiotietokantaan siten, että *on_opettaja*- ja *on_kurssi*-kolmikot vastaavat *opettaja*- ja *kurssi*-taulujen rivejä. *opettaa*-kolmikoilla taas esitetään tietojen välisiä yhteyksiä, jotka relatiotietokannassa kuvataan yhteyden tyypistä riippuen joko lisäsarakkeilla tai lisätauluilla. 

*opettaa* -kolmikon (*Listaus 3*) avulla kerrotaan tässä "*opettaja opettaa kurssia*" -totuuksia siten, että kolmikon `subject` -attribuutin arvona on tilanteeseen liityvän *on_opettaja*-kolmikon `subject`-attribuutin arvo, ja  *opettaa* kolmikon `object` -attribuutin arvona on tilanteeseen liityvän *on_kurssi*-kolmikon `subject`-attribuutin arvo

Tehtäväpohjan tiedostossa `configs/db_seed.js` on koodi, joka muodostaa tietokannan datoineen sovelluksen käynnistyksen yhteydessä, jos tunnisteen (`db.graph`) viittaamassa tietokannassa ei ole kolmikkoja. Koodi tulostaa muodostamansa tietokannan sisällön konsolille (NetBeansin *Output*-ikkunaan).


#### Toiminnot

Sovellus toteuttaa edellisistä tehtävistä tutun sivukartan:

![sivukartta](https://www.lucidchart.com/publicSegments/view/d84f9961-ce43-4b79-bac2-7405afa830ac/image.png)

<small>Kuva 2. Sivukartta</small>

Mallien metodikokonaisuus vastaa sivukarttaa (*Kuva 1*). Kaikki sivuilla esitetyt luettelot ovat nousevassa aakkosjärjestyksessä.


#### Palautettava aineisto

**Palauta** tehtävän ratkaisuna tiedostot `models/Opettaja.js` ja `models/Kurssi.js`. Varmista ennen palautusta, että sovellus toimii tehtäväkuvauksen mukaan sovellusta ajamalla sekä käyttäen oheistettuja Selenium-testejä.


### Vihjeitä ja lisätietoja

#### Sovellusrajapinta

*LevelGraph*:in sovellusrajapinta on kuvattu järjestelmän [tiiviissä käsikirjassa][LevelGraph]. Tehtävän ratkaisussa tarvittaneen [get][get]- ja [search][search] -metodeja. Seuraavat *listaukset 4 ja 5* on kopioitu em. materiaalista.

[get]: https://github.com/mcollina/levelgraph#get-and-put
[search]: https://github.com/mcollina/levelgraph#search-without-streams


{% highlight javascript %}

db.get({ subject: "a" }, (err, list) => {
  console.log(list);
});


{% endhighlight %}

<small>Listaus 4. Esimerkki *get*-metodista</small>


*get*-metodin parametriksi annetaan valintahdon määrittelevä objekti, jossa attribuutteina voivat esiintyä `subject`, `predicate` ja `object`. *Listauksen 4* koodi palauttaa taulukossa `list` kaikki kolmikot, jolla `subject`-attribuutin arvo on `a`.


*search*-metodilla voi seurata monesta kolmikosta muodostuvaa polkua ja samalla poimia polun varrelta kolmikkojen attribuuttien arvoja:


{% highlight javascript %}

db.search([{
    subject: "matteo",
    predicate: "friend",
    object: db.v("x")
  }, {
    subject: db.v("x"),
    predicate: "friend",
    object: db.v("y")
  }, {
    subject: db.v("y"),
    predicate: "friend",
    object: "davide"
  }], (err, results) => {
    // this will print "[{ x: 'daniele', y: 'marco' }]"
    console.log(results);
  });

{% endhighlight %}

<small>Listaus 5. Esimerkki *search*-metodista</small>


*Listauksen 5* metodikutsu antaa vastauksen kysymykseen, kenen kahden henkilön kautta  kulkee  "kaveruus" *matteon* ja *daviden* välillä. Suppea esimerkkiaineisto antaa vastaukseksi vain yhden polun: *daniele* ja *marco*.



#### Tehtävän ratkaisua tukevat apufunktiot

Tehtävän pohjakoodin `configs`-kansiossa on apufunktioita sisältävä moduuli `utils.js`.  Sovelluksen mallit ottavat moduulin kayttöönsä siten, että mahdollisesti tarvitavat funktiot näkyvät tunnisteilla `sortBy` ja `normalize`. 

Jo aiemmissa tehtävissä esillä ollut `sortBy` lajitelelee oliotaulukon olion tietyn ominaisuuden mukaiseen aakkosjärjestykseen. `normalize` muokkaa taulukon alkioita seuraavanlaisiksi:


{% highlight json %}

{ 
  "key": "ciytxo6qr0005fu1k6hzpolnb",
  "sukunimi": "Käkilä", 
  "etunimi": "Simo" 
}

{% endhighlight %}

<small>Listaus 6. </small>

Munnoksessa *Listauksen 6* olion lähtökohtana on ollut *Listauksen 1* olio.
