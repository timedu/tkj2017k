---
layout: exercise_page
title: "Tehtävä 6.2: Kurssit ja opettajat, Cassandra CRUD"
exercise_template_name: W6E02.KurssitJaOpettajatCassandraCRUD
exercise_discussion_id: 
exercise_upload_id: 
---

Täydennä [edellisen tehtävän](../tehtava61) ratkaisuasi opettajatietoihin liittyvillä ylläpitotoiminnolla, *lisäys*, *muutos* ja *poisto*. Tietokanta tässä on aivan sama kuin [Tehtävässä 6.1](../tehtava61). 


#### Mallit ja tietokanta

Opettajatietoihin liittyvään malliin on lisätty ylläpitotoimintoja varten kolme metodia, `create`, `update` ja `destroy`:

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


Malleja, `models/Opettaja.js` ja `models/Kurssi.js`, lukuunottamatta sovellus on rakennettu valmiiksi. `findAll`- ja `findByKey` -metodit voit kopioida edellisen tehtävän ratkaisustasi, joten laadittavaksi tässä jää metodit `create`, `update` ja `delete`.

Mallit ottavat moduulin `configs/db_connection.js` käyttöönsä siten, että tietokanta näkyy tunnisteessa `db`. Tietokannassa on neljä taulua *opettajat*, *opettaja_list*, *kurssit* ja *kurssi_list*, joista opettajatietojen ylläpito kohdistuu kolmeen ensin mainittuun tauluun. Tietokannan rakenne ja luonti on kuvattu [edellisen tehtävän](../tehtava61#mallit-ja-tietokanta) yhteydessä.


#### Toiminnot

Opettajatietojen ylläpito tapahtuu seuraavan sivukartan mukaan:

![sivukartta](../../osa2/img/w2e03.png)

<small>Kuva 2. Sivukartta: sisältää lomakkeet (*uusi*, *muuta* ja *poista*) tietojen ylläpitoa varten.</small>

Sivukartta voidaan tulkita samalla sovelluksen tilakaavioksi[^3]. `create`-metodin kutsu tapahtuu siirryttäessä *uusi*-tilasta *erittely*-tilaan ja `destroy`-metodin kutsu siirryttäessä *poista*-tilasta *luettelo*-tilaan. *muuta*-tilasta *erittely*-tilaan on kaksi erilaista siirtymää. `update`-metodin kutsu tapahtuu siirtymässä, jonka on pannut liikkeelle käyttäjän tekemä *Talleta*-painikkeen klikkaus.

[^3]: Tehtäväpohjassa oleva kontrolleri toteuttaa tämän tilakaavion

Tietokannan toisteisuudesta johtuen kukin ylläpito-toiminto kohdistuu yhtä useampaan tauluun. Opettajan *lisäys* tapahtuu sekä *opettajat*- että *opettaja_list*-tauluun. *Muutos* ja *poisto* kohdistuvat edellisten lisäksi myös *kurssit*-tauluun. Opettajan muutoksen yhteydessä on huomioitava, että *opettaja_list* tauluun kohdistuu *UPDATE*-operaation sijaan *DELETE*- ja *INSERT* -operaatiot[^3a].

[^3a]: Tämä johtuu siitä, että muutettava tieto on osana perusavainta.

#### Palautettava aineisto

**Palauta** tehtävän ratkaisuna tiedosto `models/Opettaja.js`. Varmista ennen palautusta, että sovellus toimii tehtäväkuvauksen mukaan, ajaen sovellusta sekä käyttäen oheistettuja Selenium-testejä[^4]. 

[^4]: Nämä testaavat sovelluksen ulospäin näkyvää toiminnallisuutta eivätkä siten paljasta sisäisiä vikoja.

### Vihjeitä ja lisätietoja

Tietokantaan kohdistuvat ylläpito-operaatiot voi suorittaa kyselyjen tapaan rajapinnan [client][class.Client] -olion [execute][execute] -metodilla, jolle annetaan parametrina [CQL][CQL]:n *INSERT*-, *UPDATE*- tai *DELETE* -komentoja. Tässä kuitenkin jokainen ylläpitotoiminto muodostuu useasta operaatiosta, jolloin operaatioiden suorittamisessa tulisi käyttää [batch][batch] -metodia, jonka avulla operaatioita voidaan niputtaa yhteen.

Tehtävän pohjakoodin moduulissa `configs/db_seed.js` tiedot tietokantaan talletetaan *batch*-metodilla. Seuraavassa on koodista poimintoja, joissa on esillä myös yksilöllisen avaimen generointi lisättävälle tiedolle.


[class.Client]: http://docs.datastax.com/en/developer/nodejs-driver/3.2/api/class.Client/
[execute]: http://docs.datastax.com/en/developer/nodejs-driver/3.2/api/class.Client/#execute
[batch]: http://docs.datastax.com/en/developer/nodejs-driver/3.2/api/class.Client/#batch
[CQL]: http://docs.datastax.com/en/cql/3.1/index.html


{% highlight javascript %}

// ...
const Uuid = require('cassandra-driver').types.Uuid;
const db = require('./db_connection');
// ...

// --
// komento, joka suoritetaan osana nippua (merkkijono);
// '?'-merkit korvataan parametreilla
// 
const INSERT_opettaja_list = "\
INSERT INTO opettaja_list (block_id, sukunimi, etunimi, key) \
VALUES (1, ?, ?, ?)";
// ...

// --
// taulukko, johon talletetaan nipussa suoritettavat operaatiot
// 
const InsertRows = [];
// ...

// --
// yksilöivän avaimen generointi 
//
const opettaja_key = Uuid.random();

// --
// alkion lisääminen taulukkoon, joka annetaan parametrina 
// batch-metodille; alkio on objekti, jonka query-attribuutissa on
// suoritettava komento ja params-attribuutissa komennolle annettavat
// parametrit
// 
InsertRows.push({
   query: INSERT_opettaja_list,
   params: [
      opettaja.sukunimi,
      opettaja.etunimi,
      opettaja_key
   ]
});
// ...

// --
// batch-metodin kutsu; parametrina komennot sisältävä taulukko 
//
db.batch(InsertRows).then( () => {
   log('rows inserted');
}).catch( err => {
   log(err);
});
// ...

{% endhighlight %}

<small>Listaus 1. Poimintoja moduulista *configs/db_seed.js*</small>


<br/>

