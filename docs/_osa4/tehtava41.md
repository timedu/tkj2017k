---
layout: exercise_page
title: "Tehtävä 4.1: Kurssit ja opettajat, TingoDB"
exercise_template_name: W4E01.KurssitJaOpettajatTingo1
exercise_discussion_id: 75210
exercise_upload_id: 309699
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

Tässä käytetään tietokantaa sen kokoelmiin viittaavien tunisteiden kautta. TingoDB tarjoaa kokoelmiin liittyen esim. seuraavia metodeja: `insert`, `findOne`, `find`, `update`, `save` ja `remove`. Metodien kuvaukset löytyvät Mongon API-dokumenteista kohdasta [Collection][Collection]. 

[Collection]: http://mongodb.github.io/node-mongodb-native/2.2/api/Collection.html

#### Dokumenttien kysely

Seuraavissa *listauksissa 1 ja 3* on tehtäväpohjasta poimitut esimerkit `find` ja `findOne`-metodien käytöstä. 


{% highlight javascript %}

Kurssi.findAll = (callback) => {
   db.kurssit.find().sort({'nimi': 1}).toArray((err, docs) => {
      callback(docs);
   });
};


{% endhighlight %}

<small>Listaus 1.  Ote tiedostosta *models/Kurssi.js*</small>


*Listauksen 1* määrittelevä funktio `Kurssi.findAll` lukee tietokannasta kaikki *Kurssi*-kokoelman dokumentit käyttäen *Collection*-olion `db.kurssit` metodia `find`, joka palauttaa [Cursor][Cursor]-olion. Tämä sisältää metodin `sort`, jonka avulla data voidaan lajitella, mikä tässä tapahtuu *nimen* mukaiseen nousevaan aakkosjärjestykseen. Myös `sort` palautta *Cursor*-olion, josta löytyy myös metodi `toArray`, millä kyselyn määrittelemä data saadaan kokonaisuudessaan taulukkomuotoon. Metodin parametrina on funktio, joka suoritetaan, kun kaikki tietojen hakuun liittyvät operaatiot on suoritettu. Funktio kutsuu tässä `Kurssi.findAll`-funktiolle parametrina annettavaa funktiota `callback`, jonka määrittelee kontrolleri (*Listaus 2*). 

[Cursor]: http://mongodb.github.io/node-mongodb-native/2.2/api/Cursor.html


{% highlight javascript %}

router.get('/', (req, res) => {
   Kurssi.findAll(kurssit => 
      res.render('kurssi_list', {
         kurssit: kurssit
      });
   });
});

{% endhighlight %}

<small>Listaus 2.  Ote tiedostosta *controllers/kurssit.js*</small>


*Listauksen 3* määrittelemä funktio `Kurssi.findByKey` lukee tietokannasta yhden *Kurssi*-dokumentin sen `_id`-attribuutin perusteella. Kurssin `opettaja`-attribuutissa on kurssin opettajaan liittyvän dokumentin `_id`. Tätä käyttäen luetaan kurssin opettajan tiedot kontrollerille palautettavien tietojen oheen. 


{% highlight javascript %}

Kurssi.findByKey = (_id, callback) => {
   db.kurssit.findOne({'_id': _id}, (err, kurssi) => {
      if (kurssi.opettaja) {
         db.opettajat.findOne({'_id': kurssi.opettaja}, (err, opettaja) => {
            kurssi.opettaja = opettaja;
            callback(kurssi);
         });
      } else {
         callback(kurssi);
      }
   });
};

{% endhighlight %}

<small>Listaus 3.  Ote tiedostosta *models/Kurssi.js*</small>


`findOne`-metodeille annetaan *Listauksessa 3* kaksi parametria: kyselyyn liittyvä valintaehto sekä funktio, joka suoritetaan, kun kyselyn palauttama data on käytettävissä.

 Kontrolleri (*Listaus 4*) välittää *Listauksen 3* funktiolla haettavan kurssin tunnuksen sekä funktion, jota kutsumalla haun tulos välittyy kontrollerille ja edelleen näkymään.


{% highlight javascript %}

router.get('/:_id', (req, res) => {
   Kurssi.findByKey(req.params._id, kurssi => {
      res.render('kurssi_detail', {
         kurssi: kurssi
      });
   });
});

{% endhighlight %}

<small>Listaus 4.  Ote tiedostosta *controllers/kurssit.js*</small>


#### Dokumenttien ylläpito


Dokumentin lisäyksen kokoelmaan voi tehdä `insert`-metodilla seuraavaan tyyliin:


{% highlight javascript %}

collection.insert(doc, (err, docs) => {
    // ...
});

{% endhighlight %}

<small>Listaus 5.  *insert*-metodi</small>


`insert`-metodin toisena parametrina annettava funktion `docs`-parametrin kautta palautuu lisätty dokumentti, jossa on mukana järjestelmän generoima `_id`. `docs` on taulukko, vaikka kokoelmaan lisätään vain yksi dokumentti. 


Dokumentiin kohdistuneet muutokset tietokantaan voi tallettaa `save`-metodilla:


{% highlight javascript %}

doc._id = parseInt(doc._id);

collection.save(doc, () => {
    // ...
});

{% endhighlight %}

<small>Listaus 6.  *save*-metodi</small>


Tässä muutoksen talletuksen yhteydessä lienee kuitenkin syytä vielä varmistaa, että tietokantatunniste pysyy kokonaislukuna (*Listaus 6*).

Poiston voi suorittaa `remove`-metodilla (*Listaus 7*). Poistoon liittyvä valintaehdon voi määritellä samoin kuin kyselyjen yhteydessä (ks. *Listaus 3*).

{% highlight javascript %}

collection.remove({...}, () => {
    // ..
});

{% endhighlight %}

<small>Listaus 7.  *remove*-metodi</small>


Dokumenttijoukkoa koskevia muutoksia varten on `update` -metodi:


{% highlight javascript %}

collection.update( {...}, { $set: {'attribute': value} }, {multi: true}, 
(err, result) => {
      // ...
});

{% endhighlight %}

<small>Listaus 8.  *update*-metodi</small>

*Listauksen 8* periaate-esimerkissä metodilla on neljä parametria: valintaehto,  muutos-operaatio, optiot sekä funktio, joka suoritetaan, kun muutos tietokantaan on toteutettu. Funktion `result` -parametri palauttaa lukumäärän koskien dokumentteja, joihin muutos kohdistui. Muutoksena tässä on yhden attribuutin arvon asettaminen. 

Valintaehdon yhteydessä tulisi  huomioida, että kokonaisluku ei ole välttämättä sama kuin vastaava kokonaisluku merkkijonona (vrt. *Listaus 6*).


<br/>

