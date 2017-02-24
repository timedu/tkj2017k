---
layout: exercise_page
title: "Tehtävä 7.1: Kurssit ja opettajat, OrientDB (Doc)"
exercise_template_name: W7E01.KurssitJaOpettajatOrientDBDoc
exercise_discussion_id: 77219
exercise_upload_id: 312819
---

Laadi sovellus, jolla voidaan tarkastella *kurssi- ja opettajatietoja* sekä ylläpitää opettajan tietoja (*lisäys*, *muutos* ja *poisto*). Ratkaisu toimii ulkoisesti samoin kuin tehtävissä [6.1](../../osa6/tehtava61) ja [6.2](../../osa6/tehtava62) määritelty sovellus. Tietokantaratkaisuna tässä on [OrientDB][OrientDB], joka asennetaan omaan kehitysympäristöön.
 
[OrientDB]: http://orientdb.com
 
 
 
#### Mallit ja tietokanta


Edellisten tehtävien tapaan  palvelupyyntöihin vastaavat *kontrollerit* käyttävät tietokantaa *malleihin*  paketoitujen metodien kautta (*Kuva 1*). Näitä metodeja lukuunottamatta sovellus on tehtävän pohjakoodissa valmiina.

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

Mallit ottavat moduulin `configs/db_connection.js` käyttöönsä siten, että tietokanta näkyy tunnisteessa `db`. Pohjassa oleva moduuli `configs/db_seed.js` rakentaa tietokannan sisällön uudelleen aina sovelluksen käynnistyksen yhteydessä, mutta se ei luo tässä käytettävää *Koulu*- tietokantaa, joka on perustettava tietokannan hallintavälineistön avulla (ks. kohta [vihjeitä ja lisätietoja](#vihjeit-ja-listietoja)). 

 `db_seed.js` muodostaa tietokantaan kaksi luokkaa (`Class`), *Kurssi* ja *Opettaja*. *Kurssi* -luokan tietueilla (`Record`) on *opettaja* -oninaisuus (`Property`), joka on *opettaja* -tietueeseen osoittava linkki (`Link`). *Opettaja* -luokan tietueilla on *kurssit* -ominaisuus, joka on *Kurssit* -luokan tietueisiin osoittavien linkkien luettelo (`Linklist`). Muita ominaisuuksia ei luokille ole määritely - ne muodostuvat tietojen talletuksen yhteydessä [^1].  Alustuksen myötä syntyvässä tietokannassa on samat 15 kurssia ja 6 opettajaa kuin edellisissäkin tehtävissä. 

[^1]: Opettaja: sukunimi, etunimi; Kurssi: tunnus, nimi, laajuus

#### Toiminnot

Toimintoihin liittyvät sivukartat on esitetty esim. tehtävien [6.1](../../osa6/tehtava61/#toiminnot) ja [6.2](../../osa6/tehtava62/#toiminnot) kuvauksissa. Sovelluksen mallien toteuttama tietokantakäsittely tapahtuu siirryttäessä sivulta toiselle.

#### Palautettava aineisto

**Palauta** tehtävän ratkaisuna tiedostot `models/Opettaja.js` ja `models/Kurssi.js`. Varmista ennen palautusta, että sovellus toimii tehtäväkuvauksen mukaan sovellusta ajamalla sekä käyttäen oheistettuja Selenium-testejä.


### Vihjeitä ja lisätietoja

#### OrientDB:n asennus ja tietokannan perustaminen

OrientDB:n asennuspaketit löytyvät tuotteen [download][download] -sivulta, jolta voi valita esim. vaihtoehdon *ZIP file for any OS*. Tämän paketin voi purkaa mihin tahansa hakemistoon kehitysympäristössä.  Tietokantapalvelimen voi käynnistää `bin`-hakemistosta löytyvällä skriptillä `server.sh` (tai `server.bat`, jos operoidaan Windows-koneessa). 

[download]: http://orientdb.com/download/

Ensimmäisellä käynnistyskerralla skripti kysyy palvelimen pääkäyttäjälle, *root*, salasanaa, joka tallentuu tiedostoon `config/orientdb-server-config.xml`. Konfiguraatiotiedostossa pääkäyttäjän salasana ei ole luettevassa muodossa. Jos salasana unohtuu, voi toimia dokumentaatiosta löytyvän ohjeen mukaan: [Restoring the Server's User root][reset-root]. 

[reset-root]: http://orientdb.com/docs/2.2/Server-Security.html#restoring-the-servers-user-root

Luokkakoneissa on kirjoitusoikeus hakemistoon `C:\Temp`, jonne OrientDB:n voisi periaatteessa asentaa. Ympäristö lienee kuitenkin sellainen, että palvelinprosessin käynnistäminen ei onnistu. Siten osan 7 tehtävissä on tukeuduttava omassa laitteessa toimivaan kehitysympäristöön.[^jelastic]

[^jelastic]: OrientDB:n saa veloituksetta sivun [OrientDB in the Cloud](http://orientdb.com/cloud/) kautta kahdeksi viikoksi koekäyttöön  [Jelastic](https://jelastic.com):in pilvipalveluun. Oletusarvoisesti järjestelmästä asentuu versio 1.7.4. Veloitukseton kokeilu lienee kuitenkin rajoitettu siten, että tietokantaa ei voi käyttää palvelun ulkopuolelta so. myös tietokantaa käyttävä sovellus tulisi asentaa palveluun. Tietokannan käyttöön liittyvän [julkisen ip-osoitteen](https://docs.jelastic.com/remote-access-mysql) tiettävästi saisi vasta rahallista korvausta vastaan.

Tehtävässä tarvittavan *Koulu* -tietokannan voi perustaa esim. web-pohjaisella hallintatyökalulla, joka saa selaimeen osoitteella <http://localhost:2480> palvelinohjelmiston ollessa käynnissä. Hallintatyökalun login -ikkunassa on painike *NEW DB*, jota klikkaamalla avautuu ao. ikkuna.

#### Sovellusrajapinta

Tässä käytettävä tietokannan [Node.js-ajuri][Node-ajuri] on kuvattu osana OrientDB:n muuta [dokumentaatiota][dokumentaatio].

[Node-ajuri]: http://orientdb.com/docs/last/OrientJS.html
[dokumentaatio]: http://orientdb.com/docs/last/index.html

Moduulin `db_connection.js` tarjoama `db` on objekti, joka on kuvattu kohdassa [Database API][Database-API]. Objektilla on mm. metodi [`query`][query], jolla voi välittää OrienDB:n [SQL-komennon][SQL-komento] suoritettavaksi palvelimelle. Metodia käytettäessä tietokantakäsittelyn voi rakentaa seuraavaan tyyliin:

[Database-API]: http://orientdb.com/docs/last/OrientJS-Database.html
[query]: http://orientdb.com/docs/last/OrientJS-Query.html
[SQL-komento]: http://orientdb.com/docs/last/Commands.html


{% highlight javascript %}

   const SQLKomento = '...'; 

   db.query(SQLKomento).then(result => {
      // ...
      callback(...);
   }).catch(error => {
      log(error);
      callback(...);
   });

{% endhighlight %}

<small>Listaus 1. </small>

Ehdotukset sovelluksen kyselyt toteuttavista SQL-komennoista löytyvät  tehtävän pohjakoodista. Sovelluksen kaikki tietokantakäsittely voidaan toteuttaa `query`-metodia käyttäen, mutta ainakin tietojen ylläpitoon liittyvät operaatiot hoitunevat yksinkertaisemmin muita apuveuvoja käyttäen.

Jos halutun tietueen tietokantatunniste (*Record ID - RID*) on tiedossa, tietue voidaan  hakea kyselyn ohella  `db` -objektin [`record`][record] -ominaisuuden  [`get`][get] -metodilla. Myös tiedon [muutoksessa][update] ja [poistossa][delete] voidaan käyttää `record` -ominaisuuden metodeja. Uusi tietue voidaan lisätä tietokannan luokkaa sovelluksessa edustavan olion [`create`][create] -metodilla. Em. olio saadaan käyttöön `db`-objektin `class`-ominaisuuden [`get`][get-class]-metodilla.


[record]: http://orientdb.com/docs/last/OrientJS-Record.html
[get]: http://orientdb.com/docs/last/OrientJS-Record.html#getting-records
[update]: http://orientdb.com/docs/last/OrientJS-Record.html#updating-a-record
[delete]: http://orientdb.com/docs/last/OrientJS-Record.html#deleting-records
[create]: http://orientdb.com/docs/last/OrientJS-Class-Records.html#creating-records
[get-class]: http://orientdb.com/docs/last/OrientJS-Class-Classes.html#getting-classes

#### Pohjakoodista

OrientDB:n tietueiden tietokantatunniste, [Record ID][RID] (RID), muodostuu klusteritunnisteesta ja tietueen sijainnista klusterissa. RID esitetään merkkijonomuodossa (esim.) seuraavasti: `#19:159`. Navigointi kehitettävässä sovelluksessa perustuu tietokantatunnisteiden käyttöön pyyntöjen poluissa. `#`-merkki osana polkua on kuitenkin ongelmallinen, ja siten esiintyessään polussa tietokantatunniste on muotoa (esim.) `19:159`. 

[RID]: http://orientdb.com/docs/last/Tutorial-Record-ID.html 

Seuraavassa on pohjakoodissa oleva SQL-komento, jota voi hyödyntää muodostettaessa ao. näkymään aakkosjärjestyksessä oleva luettelo kaikista oppikursseista.

{% highlight javascript %}

const FindAllKurssit = '\
SELECT @RID.substring(1) AS key, nimi \
FROM Kurssi \
ORDER BY nimi';

{% endhighlight %}


<small>Listaus 2. </small>

Näkymä odottaa kunkin kurssin osalta kahta tietoa, *nimi* ja *key*, joista *nimi* esitetään sivulla ja *key* on osana kurssin erittelysivulle osoittavaa linkkiä.  *key* on merkkijonomuodossa oleva tietokantatunniste, jonka alusta on poistettu `#`-merkki. *Listauksen 2* SQL-komento muodostaa halutun muotoisen *key*-tiedon hyödyntäen OrientDB:stä löytyvää [`substring`][substring] -metodia.

[substring]: http://orientdb.com/docs/last/SQL-Methods.html#substring


Alla olevan *Listauksen 3* SQL-komentoa voi käyttää tuotettaessa tiedot kurssin erittelynäkymään. 

{% highlight javascript %}

const FindKurssiByKey = '\
SELECT \
  @RID.substring(1) AS key, \
  tunnus,   \
  nimi,     \
  laajuus,  \
  opettaja.sukunimi AS opettaja_sukunimi,    \
  opettaja.etunimi  AS opettaja_etunimi,     \
  opettaja.@RID.substring(1) AS opettaja_key \
FROM Kurssi \
WHERE @RID = :rid';


{% endhighlight %}

<small>Listaus 3. </small>

*Listauksen 3* SQL-komennon *WHERE* -osassa esiintyy parametri, jolle voidaan antaa arvo siten kuin dokumentaatiosta löytyvä [esimerkki][using-parameters] ohjaa. Parametrin arvo on merkkijonomuodossa oleva tietokantatunnus so. sen ensimmäisenä merkkinä tulee olla `#`.

[using-parameters]: http://orientdb.com/docs/last/OrientJS-Query.html#using-parameters

SQL-komento palauttaa sekä *Kurssi*- että *Opettaja*-luokan tietueiden tietoja. Perinteisen relaatiotietokannan yhteydessä vastaava tilanne ratkaistaisiin liitoksen avulla. Liitoksen sijaan tässä hyödynnetään *Kurssi*-luokan tietuille määriteltyä *Link* -tyyppistä *opettaja*-ominaisuutta[^2].  

[^2]: Tehtävän pohjakoodin moduulista `db_seed.js` löytyy SQL-komento, jolla *Kurssi*-tietueiden *opettaja*-ominaisuuksille on muodostettu arvot.

*Listauksen 3* esittämän komennon palauttama data ei ole aivan sitä muotoa, jota näkymä odottaa, joten saatua tulosta on hieman muokattava. Komento palautta tiedot seuraavasti:

~~~
[{
  key: ...,
  tunnus: ...,
  nimi: ...,
  laajuus: ...,
  opettaja_sukunimi: ...,
  opettaja_etunimi: ...,
  opettaja_key: ...
}]
~~~

<small>Listaus 4. </small>

Näkymä kuitenkin edellyttää seuraavaa rakennetta:

{% highlight javascript %}

{
  key: ...,
  tunnus: ...,
  nimi: ...,
  laajuus: ...,
  opettaja: {
      sukunimi: ...,
      etunimi: ...,
      key: ...
  }
}

{% endhighlight %}

<small>Listaus 5. </small>


Seuraavassa *Listauksessa 6* on komento, joka hakee tietokannasta opettajan erittelysivulla esitetävät tiedot.

{% highlight javascript %}

const FindOpettajaByKey = '\
SELECT \
   @RID.substring(1) AS key, \
   sukunimi, \
   etunimi, \
   kurssit.@RID AS kurssi_rid, \
   kurssit.nimi AS kurssi_nimi \
FROM Opettaja \
WHERE @RID = :rid';


{% endhighlight %}

<small>Listaus 6. </small>

Myös *Listauksen 6* komennon tuottamaa tulosta on muokattava näkymää varten. Tilanne on kurssin vastaavaan verrattuna vielä hivenen monimutkaisempi, koska opettajalla voi olla useita kursseja. SQL-komento tuottaa tiedot seuraavanlaisessa muodossa:

~~~
[{
  key: ...,
  sukunimi: ...,
  etunimi: ...,
  kurssi_rid:  [ ... ]
  kurssi_nimi: [ ... ]
}]
~~~

<small>Listaus 7. </small>

Näkymälle tulisi tiedot välittää kuitenkin toisenlaisessa muodossa:

{% highlight javascript %}

{
  key: ...,
  sukunimi: ...,
  etunimi: ...,
  kurssit: [
      {
        key: ..., 
        nimi: ...
      }, 
      ... 
  ]
}


{% endhighlight %}

<small>Listaus 8. </small>

Muokkauksen yhteydessä tulee myös muuntaan "*rid*"  "*key:ksi*" so. poistaa `#`-merkki tietokantatunnisteen edestä, koska jostakin syystä tällaisessa tilanteessa se ei onnistune SQL-komennon suorituksen yhteydessä. Muunnoksessa saatetaan tarvita JavaScript-metodeja [`toString`][toString]  ja [`substr`][substr]. 

[toString]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/toString
[substr]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/substr

#### Päivityksiä

* 170224: Täsmennyksiä kohtaan [OrientDB:n asennus ja tietokannan perustaminen](#orientdbn-asennus-ja-tietokannan-perustaminen)

<br/>








