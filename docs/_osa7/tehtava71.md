---
layout: exercise_page
title: "Tehtävä 7.1: Kurssit ja opettajat, OrientDB (Doc)"
exercise_template_name: W7E01.KurssitJaOpettajatOrientDBDoc
exercise_discussion_id: 
exercise_upload_id: 
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

OrientDB:n asennuspaketit löytyvät tuotteen [download][download] -sivulta, jolta voi valita esim. vaihtoehdon *ZIP file for any OS*. Tämän paketin voi purkaa mihin tahansa hakemistoon kehitysympäristössä. Luokkakoneissa on kirjoitusoikeus hakemistoon `C:\Temp`. Tietokantapalvelimen voi käynnistää `bin`-hakemistosta löytyvällä skriptillä `server.sh` (tai `server.bat`, jos operoidaan Windows-koneessa). Ensimmäisellä käynnistyskerralla skripti kysyy palvelimen pääkäyttäjälle, *root*, salasanaa, joka tallentuu tiedostoon `config/orientdb-server-config.xml`.  

[download]: http://orientdb.com/download/

Tehtävässä tarvittavan *Koulu* -tietokannan voi perustaa esim. web-pohjaisella hallintatyökalulla, joka saa selaimeen osoitteella `http://localhost:2480` palvelinohjelmiston ollessa käynnissä. Hallintatyökalun login -ikkunassa on painike *NEW DB*, jota klikkaamalla avautuu ao. ikkuna.

#### Sovellusrajapinta

Tässä käytettävä tietokannan [Node.js-ajuri][Node-ajuri] on kuvattu osana OrientDB:n muuta [dokumentaatiota][dokumentaatio].

[Node-ajuri]: http://orientdb.com/docs/last/OrientJS.html
[dokumentaatio]: http://orientdb.com/docs/last/index.html

Moduulin `db_connection.js` tarjoama `db` on objekti, joka on kuvattu kohdassa [Database API][Database-API]. Objektilla on mm. metodi [`query`][query], jolla voi välittää OrienDB:n [SQL-komennon][SQL-komento] suoritettavaksi palvelimelle. Tehtävän kaikki tietokantakäsittely voidaan toteuttaa `query`-metodia käyttäen, mutta ainakin tietojen ylläpitoon liittyvät operaatiot hoitunevat yksinkertaisemmin muita apuveuvoja käyttäen. 

[Database-API]: http://orientdb.com/docs/last/OrientJS-Database.html
[query]: http://orientdb.com/docs/last/OrientJS-Query.html
[SQL-komento]: http://orientdb.com/docs/last/Commands.html

Jos halutun tietueen tietokantatunniste (*Record ID - RID*) on tiedossa, tietue voidaan  hakea kyselyn ohella  `db` -objektin [`record`][record] -ominaisuuden  [`get`][get] -metodilla. Myös tiedon [muutoksessa][update] ja [poistossa][delete] voidaan käyttää `record` -ominaisuuden metodeja.

= to be continued =

[record]: http://orientdb.com/docs/last/OrientJS-Record.html
[get]: http://orientdb.com/docs/last/OrientJS-Record.html#getting-records
[update]: http://orientdb.com/docs/last/OrientJS-Record.html#updating-a-record
[delete]: http://orientdb.com/docs/last/OrientJS-Record.html#deleting-records

#### Pohjakoodista


{% highlight javascript %}

const FindAllKurssit = '\
SELECT @RID.substring(1) AS key, nimi \
FROM Kurssi \
ORDER BY nimi';


{% endhighlight %}



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




<br/>








