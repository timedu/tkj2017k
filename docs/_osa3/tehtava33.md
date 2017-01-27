---
layout: exercise_page
title: "Tehtävä 3.3: Kurssit ja opettajat, Sub-levels"
exercise_template_name: W3E03.KurssitJaOpettajatSubLevels
exercise_discussion_id: 74591
exercise_upload_id: 308965
---

Laadi  ulkoisilta ominaisuksiltaan [tehtävän 3.2](../tehtava32) ratkaisua vastaava sovellus, joka taustalla on edelliseen tehtävään verrattuna hieman erirakenteinen tietokanta. Tässä tukeudutaan Noden LevelDB rajapinnan laajennukseen, jolla tietokannan sisältö voidaan jäsentää jossain määrin relaatiotietokannan tauluja muistuttaviin osiin. Samalla hyödynnetään erillistä node-pakettia, jolla voidaan muodostaa tietokannan tiedoille yksilöllisiä tunnisteita.

Sovellukseen liittyy loki, jota varten lähdekoodi sisältää oman näkymän, `views/del_loki.` Muuten tiedostokokonaisuus vastaa  [edellistä tehtävää](../tehtava32).

#### Mallit ja tietokanta


*Kontrollerit* käyttävät tietokantaa *malleihin* (*Kuva 1*) paketoitujen metodien kautta. 
 
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
  | delLoki       |
  +---------------+
~~~
<small>Kuva 1. Sovelluksen mallit</small>


Sovellus on rakennettu valmiiksi lukuun ottamatta tietostoa `models/Opettaja.js`, joka sekin sisältää ratkaisussa hyödynnettävissä olevaa koodia. 

Tehtävässä tietokanta jäsentyy seuraavasti:

~~~
  +---------------+  
  |     base      |  
  +-------+-------+  
          |
          +------------------------------------------+
          |                                          |
  +-------+-------+                          +-------+-------+
  |    kurssit    |                          |    opettajat  |
  +-------+-------+                          +-------+-------+
          |                                          |
          +---------------------+                    |
          |                     |                    |
  +-------+-------+     +-------+-------+    +-------+-------+
  |<opettaja-cuid>| ... |<opettaja-cuid>|    |    del_loki   |
  +---------------+     +---------------+    +---------------+
~~~
<small>Kuva 2. Tietokannan rakenne</small>

`base` on tietokannan päätaso, jolla on kaksi alitasoa, `kurssit` ja `opettajat`, sisältäen nimitystensä mukaiset tiedot (*avain-arvoparit*). Avaimet muodostetaan tässä [`cuid`][cuid]-funktiolla, joka palauttaa yksilöllisiä tunnisteita. Arvot ovat json-muodossa olevia merkkijonoja. Osana yksittäisen kurssin *arvoa* on `opettaja`-attribuutti, joka sisältää kurssin opettajan *avaimen*.

[cuid]: https://github.com/ericelliott/cuid/blob/master/README.markdown#cuid

*Kuvassa 2* `kurssit`-tason alapuolella on otsikolla `<opettaja-cuid>` esitettyjä elementtejä, missä otsikolla viitataan opettajan *avaimeen*. Näissä elementeissä (*alitasoissa*) kurssin tiedot on talletettu opettajittain. `opettajat`-tason alapuolella oleva `del_loki` sisältää tietokannasta poistettujen opettajien *avaimet* erillistä "roskien keruuta"[^1] varten. 

[^1]: Ei toteuteta tähän ratkaisuun

#### Toiminnot

Sovellus toteuttaa kyselyjen osalta jo aiempiin tehtäviin liittyvän sivukartan (*Kuva 3*), joka muodostuu kahdesta luettelo- ja kahdesta erittelysivusta. *Kurssit*-sivu esittää tietokannasta (*Kuva 2*) `kurssit`-tasolta luettuja tietoja ja *Opettajat*-sivu `opettajat`-tasolta luettuja tietoja. Kurssin erittelysivu, *Kurssi*, esittää `kurssit`-tasolta luettujen tietojen lisäksi opettajan nimen, joka on luettu `opettajat`-tasolta. Opettajan erittelysivu, *Opettaja*, esittää `opettajat`-tasolta luettujen tietojen lisäksi opettajan pitämät kyrssit tasolta, joka on nimetty opettajan *avaimen* mukaan (`<opettaja-cuid>`). 

![sivukartta](https://www.lucidchart.com/publicSegments/view/d84f9961-ce43-4b79-bac2-7405afa830ac/image.png)

<small>Kuva 3. Sivukartta liittyen sovelluksen kyselytoimintoinhin</small>

Ratkaisuun sisältyy myös toiminnot opettaja-tietojen ylläpitoa varten (*lisäys*, *muutos* ja *poisto*). *Lisäys* ja *muutos* kohdistuvat ainoastaan tietokannan `opettajat`-tasolle. Tietokannan toisteisuudesta johtuen opettaja *poisto* kohdistuu myös muihin tasoihin: `kurssit`-tasolla esiintyy opettajan *avain* ja kullakin opettajalla on oma opettajan kurssit sisältävä "`<opettaja-cuid>`"-tasonsa. *Poisto* kuitenkin toteutetaan tässä niin, että poistettaessa ao. tieto `opettajat`-tasolta, talletetaan poistetun  opettajan *avain* tietokannan `del_loki`-tasolle (ei siis tehdä muutoksia tasolle `kurssit` eikä ao. `<opettaja-cuid>`-tasolle).

#### Palautettava aineisto

**Palauta** tehtävän ratkaisuna tiedosto `models/Opettaja.js`. Varmista ennen palautusta, että sovellus toimii tehtäväkuvauksen mukaan sovellusta ajamalla. Tehtävänpohja ei sisällä testikoodia. 

### Vihjeitä ja lisätietoja

Tietokannan alitasojen kautta on käytettävissä sama rajapinta kuin käytettäessä ilman alitasoja olevaa LevelDB-tietokantaa (ks. [Readme][level-sublevel]). `batch`-metodia käytetään kuitenkin päätason kautta. *Pedro Teixeiran* kirjoitus, [Node.js Databases: An Embedded Database Using LevelDB][teixeira], sisältää  esimerkkejä myös [alitasojen käytöstä][teixeira-sublevel].

[level-sublevel]: https://github.com/dominictarr/level-sublevel/blob/master/README.md
[teixeira]: https://blog.yld.io/2016/10/24/node-js-databases-an-embedded-database-using-leveldb
[teixeira-sublevel]: https://blog.yld.io/2016/10/24/node-js-databases-an-embedded-database-using-leveldb/#usinglevelsublevel

#### Sovelluksen osien liittyminen toisiinsa

Tähän mennessä esillä olleiden tehtävien tapaan tässä laadittava ratkaisu on web-palvelinsovellus, joka käynnistymisen jälkeen jää kuutelemaan sovellukseen kohdistuvia palvelupyntöjä. Sovellus käynnitetään kuten aiempienkin tehtävien ratkaisut: `node main.js`. Tosin käynnistys kehitysympäristössä on tehty IDE:n kautta.

Tehtävän pohjakoodissa kursseja koskevat toiminnot ovat valmiina. Seuraavissa listauksissa on näihin toimintoihin liittyen pohjakoodista otteita, joiden avulla tuodaan vielä esiin sovelluksen eri moduulien littyminen toisiinsa. 

Tässä lähdetään liikkeelle moduulista `kurssit.js` (*Listaus 1.*), jossa määritellään polkuja ja niihin liittyviä funktioita, joka suoritetaan pyynnön kohdistuessa polkuun.


{% highlight javascript %}

const Kurssi = require('../models/Kurssi');

const router = require('express').Router();
module.exports = router;

// ...
router.get('/:key', (req, res) => {

   Kurssi.findByKey(req.params.key, kurssi => {
   
      res.render('kurssi_detail', {
         kurssi: kurssi
      });
   });   
});

{% endhighlight %}

<small>Listaus 1.  Kontrolleri (controllers/kurssit.js)</small>


Polkujen määritelyä varten luodaan [`router`][router]-olio, joka julkaistaan moduulin käyttäjille, tässä `main.js` (*Listaus 2*), jossa *router*  kytketään polkuun `/kurssit`. Kytkennän jälkeen *routeriin* määritelty polku `/:key` näkyy ulospäin polkuna `/kurssit/:key`.

[router]: http://expressjs.com/en/4x/api.html#router

Pyyntöön vastaavalla funktiolla on kaksi parametria, joista ensimmäinen on [`Request`][req]-olio ja toinen [`Response`][req]-olio. Funktio suorittaa tietokantahaun, jonka jälkeen hahmontaa haun tuloksen näkymään so. muodostaa html-sivun, joka välitetään pyynnön lähettäjälle.

[req]: http://expressjs.com/en/4x/api.html#req
[res]: http://expressjs.com/en/4x/api.html#res

Tietokantahaku suoritetaan `Kurssi`-olion `findByKey`-metodilla. Olio on määritelty moduulissa `Kurssi.js` (*Listaus 4*), jonka tämä moduuli ottaa käyttöönsä `require`-funktiolla. Metodikutsulle annetaan parametriksi haettavan kurssin *avain*, joka on osana pyynnnön polkua. Parametri poimitaan polusta *Request*-olion avulla viittauksella `req.params.key`. 

`findByKey`-metodi toimii tässä niin, että se kutsuu sille toisena parametrina annettua funktiota. 

...


{% highlight javascript %}

// ...
const Express = require('express');
const app = Express();
// ...
app.use('/kurssit', require('./controllers/kurssit'));
// ...

{% endhighlight %}

<small>Listaus 2. Päämoduuli (main.js)</small>



{% highlight html %}
{% raw %}

...
{{#with kurssi}}
<div>
    Tunnus: <span>{{tunnus}}</span>
</div>
...
<div>
    Opettaja:     
    {{#with opettaja}}{{#if sukunimi}}
        <a href="/opettajat/{{key}}">
            {{sukunimi}}, {{etunimi}}
        </a>
    {{/if}}{{/with}}    
</div>
{{/with}}


{% endraw %}
{% endhighlight %}

<small>Listaus 3. Näkymä (views/kurssi_detail.hbs)</small>




{% highlight javascript %}

const db = require('../configs/db_connection');

const Kurssi = {};
module.exports = Kurssi;
// ...
Kurssi.findByKey = (key, callback) => {

   db.kurssit.get(key, (err, kurssi) => {

      kurssi.key = key;

      db.opettajat.get(kurssi.opettaja, (err, opettaja) => {

         kurssi.opettaja = opettaja;         
         callback(kurssi);
      });
   });
}; 

{% endhighlight %}

<small>Listaus 4. Tietokannan käsittely rajapinnan kautta (models/Kurssi.js)</small>



{% highlight javascript %}

const Level = require('level');
const SubLevel = require('level-sublevel');

const Database = './database/koulu.level';
const levelDB = Level(Database, {valueEncoding: 'json'});
const subLevelDB = SubLevel(levelDB);

const opettajat = subLevelDB.sublevel('opettajat'); 
const kurssit = subLevelDB.sublevel('kurssit'); 

module.exports = {
  base:  subLevelDB,
  opettajat: opettajat,
  kurssit: kurssit
};


{% endhighlight %}

<small>Listaus 5. Viiteet tietokantaan (configs/db_connection.js)</small>






{% highlight javascript %}


{% endhighlight %}

<small>Listaus n. </small>







<br/>


