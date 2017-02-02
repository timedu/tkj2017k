---
layout: exercise_page
title: "Tehtävä 4.3: Kurssit ja opettajat, MongoDB"
exercise_template_name: W4E03.KurssitJaOpettajatMongoDB
exercise_discussion_id: 75212
exercise_upload_id: 309703
---

Laadi kyselyominaisuksiltaan [tehtävän 4.1](../tehtava41) ratkaisua vastaava sovellus. TingoDB:n sijaan tässä kuitenkin käytetään [pilvipalvelussa][mLab] toimivaa [MongoDB][mongo]-tietokantaa. Tietokannan rakenne on samanlainen kuin tehtävässä 4.1.

[mongo]: https://www.mongodb.com

#### Mallit ja tietokanta


Tässäkin palvelupyyntöihin vastaavat *kontrollerit* käyttävät tietokantaa *malleihin*  paketoitujen metodien kautta (*Kuva 1*). 
 
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


Malleja, `models/Opettaja.js` ja `models/Kurssi.js`, lukuunottamatta sovellus on rakennettu valmiiksi. 

Tietokanta on lähes kuin tehtävässä 4.1: 

~~~
  +----------------+     +----------------+
  |     Kurssi     |     |    Opettaja    |
  | <<collection>> |     | <<collection>> |
  +----------------+     +----------------+
  | _id            |     | _id            |
  | tunnus         |     | etunimi        |
  | nimi           |     | sukunimi       |
  | laajuus        |     +----------------+
  | opettaja_id    |
  +----------------+
~~~
<small>Kuva 2. Tietokannan rakenne</small>

*Kurssi*-dokumentissa oleva *opettaja*-dokumentin tunnus on tässä nimeltään `opettaja_id`. Dokumenttien tunnisteet ovat MongoDB:n määrittelemää muotoa. 

Tietokantayhteys määritellään tiedostossa `configs/db_connection.js` olevilla tunnuksilla:

{% highlight javascript %}

// nämä tunnisteet moodlesta
const dbuser = '';
const dbpassword = '';
const url = ``;
// 

{% endhighlight %}

<small>Listaus 1. Tietokantayhteyden tunnisteet</small>

Tietokantatunnisteiden arvot löytyvät [Moodlesta][tunnisteet]. Tunnisteilla on *Kurssit ja opettajat* -tietokannan tietojen lukuoikeus. Halutessasi voit perustaa [mLab][mLab]:in palveluun oman tietokantasi. Palvelu tarjoaa veloituksetta kokeiluja varten 500 MB "hiekkalaatikon". Käytä tässä tapauksessa omaan tietokantaasi liittyviä tunnisteita Moodlessa olevien sijaan.

[tunnisteet]: https://moodle2.tut.fi/mod/page/view.php?id=309643
[mLab]: https://mlab.com

Tehtäväpohjan tiedostossa `configs/db_seed.js` on koodi, joka tallettaa datan tietokantaan sovelluksen käynnistyksen yhteydessä, jos tunnisteiden viittaamassa tietokannassa ei ole dokumentteja `kurssit`- ja `opettajat`-kokoelmissa. Koodi on kuitenkin poiskommentoitu. Omaa tietokantaa käytettäessä sen voi aktivoida.

`configs/db_connection.js` tarjoaa moduulin käyttäjälle funktion, jolle annetaan parametriksi tietokantakäsittelyä toteuttava funktio:


{% highlight javascript %}

module.exports = (run) => {
   MongoClient.connect(url, (err, db) => {
      if (!err) {         
         // log(module.filename, "connected to mongo server");
         run(db);         
      } else {         
         log(module.filename, "mongo connection failed");
         log(module.filename, err);         
      }      
   });
};

{% endhighlight %}

<small>Listaus 2. </small>


*Listauksen 2* funktio ottaa yhteyden tietokantaan. Jos yhteydenotto onnistuu, se kutsuu sille parametrina annettua funktiota `run`, jolle se antaa parametriksi viitteen (`db`) tietokantaan. 

Sovelluksen mallit ottavat *listauksen 2* funktion käyttöönsä seuraavasti:


{% highlight javascript %}

const mongo = require('../configs/db_connection');

{% endhighlight %}

<small>Listaus 3. </small>


Käyyttöönoton jälkeen mallit voivat suorittaa tietokantakäsittelyä seuraavanlaisesti ikään kuin "mongo-kehyksessä":


{% highlight javascript %}

Opettaja.findAll = (callback) => {

   mongo((db) => {
   
      const opettajat = db.collection('opettajat');
      opettajat.find({}).sort({'sukunimi': 1}).toArray((err, docs) => {
         db.close();
         callback(docs);
      });
      
   });  
};

{% endhighlight %}

<small>Listaus 4. </small>


#### Toiminnot

Sovellus sisältää kyselyjen osalta samat toiminnot kuin [tehtävä 4.1](../tehtava41), jonka ratkaisusta tämän tehtävän ratkaisu ei juuri poikkea. Tosin tässä tulee huomioida *Listauksessa 4* esitetty periaate. 

#### Palautettava aineisto

**Palauta** tehtävän ratkaisuna tiedostot `models/Opettaja.js` ja `models/Kurssi.js`. Varmista ennen palautusta, että sovellus toimii tehtäväkuvauksen mukaan sovellusta ajamalla[^3]. Tehtävänpohja ei sisällä testikoodia[^4].
 
[^3]: Tarkoitus on, että toimivuus ei edellytä palautettavan moduulin muutosten lisäksi muutoksia muihin moduuleihin. 
[^4]: Testeillä varustettu pohja saatetaan julkaista myöhemmin. 


### Vihjeitä ja lisätietoja

Tarvittaessa mongo-ajurin käsikirjaa voi silmäillä kohdasta [Collection][Collection].

[Collection]: http://mongodb.github.io/node-mongodb-native/2.2/api/Collection.html

Jos kyselyehdossa käytetään merkkijonomuodossa olevaa tietokantatunnistetta[^5], se on muunnettava Mongon ObjectId:ksi. Tämän voi tehdä `ObjectId`-funktiolla, jonka mallit ottavat käyttöönsä. 

[^5]: Merkkijonomuodossa olevia tietokantatunnisteita esiintyy pyyntöjen polkuissa. Kontrolleri poimii nämä poluissa olevat parametrit välittäen ne malleille.


<br/>

