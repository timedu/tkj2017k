---
layout: exercise_page
title: "Tehtävä 6.1: Kurssit ja opettajat, Cassandra"
exercise_template_name: W6E01.KurssitJaOpettajatCassandra
exercise_discussion_id: 
exercise_upload_id: 
---

Laadi ulkoisilta ominaisuksiltaan edellisten tehtävien ratkaisujen kaltainen sovellus, jolla voidaan tarkastella *kurssi- ja opettajatietoja* sekä *yhteenveto-* että *erittelymuotoisten* näkymien kautta. Taustalla oleva tietokantaratkaisu on nyt [Bitnamin virtuaalikoneessa][cassandra-vm] ajettava [Cassandra][Cassandra].

[cassandra-vm]: https://bitnami.com/stack/cassandra/virtual-machine
[Cassandra]: http://cassandra.apache.org

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


Malleja, `models/Opettaja.js` ja `models/Kurssi.js`, lukuunottamatta sovellus on rakennettu valmiiksi. Mallit ottavat moduulin `configs/db_connection.js` käyttöönsä siten, että tietokanta näkyy tunnisteessa `db`.

Tietokanta rakentuu siten, että kutakin kyselyä varten on oma tietokantataulunsa,  *opettaja_list*, *opettajat*, *kurssi_list* ja *kurssit*. Seuraavissa *Listauksissa 1-6* on esitetty luontikomennot *tauluille* ja niiden viittaamille *tyypeille*[^1].

[^1]: Listaukset on poimittu moduulista `configs/db_seed.js`.


{% highlight javascript %}

const CREATE_TABLE_opettaja_list = '        \
CREATE TABLE IF NOT EXISTS opettaja_list (  \
  block_id  int,                            \
  key       uuid,                           \
  sukunimi  text,                           \
  etunimi   text,                           \
PRIMARY KEY ( (block_id), sukunimi, etunimi, key) )';

{% endhighlight %}

<small>Listaus 1. *opettaja_list*-taulu</small>


Taulussa `opettaja_list` olevan partitio-avaimen `block_id` arvo on kaikkien taulun rivien osalta sama so. rivit talletetaan yhteen partitioon (*Listaus 1*).


{% highlight javascript %}

const CREATE_TABLE_opettajat = '        \
CREATE TABLE IF NOT EXISTS opettajat (  \
  key       uuid,                       \
  sukunimi  text STATIC,                \
  etunimi   text STATIC,                \
  kurssi    frozen<kurssi>,             \
PRIMARY KEY ((key), kurssi) )';

{% endhighlight %}

<small>Listaus 2. *opettajat*-taulu</small>



{% highlight javascript %}

const CREATE_TYPE_kurssi = '        \
CREATE TYPE IF NOT EXISTS kurssi (  \
  nimi  text,                       \
  key   uuid                        \
)';

{% endhighlight %}

<small>Listaus 3. *kurssi*-tyyppi</small>


*Listauksen 2* esittämässä `opettajat` -taulun perustamiskomennossa  viitataan tyyppiin `kurssi`, jonka määrittely on *Listauksessa 3*.


{% highlight javascript %}

const CREATE_TABLE_kurssi_list = '        \
CREATE TABLE IF NOT EXISTS kurssi_list (  \
  block_id  int,                          \
  key       uuid,                         \
  nimi      text,                         \
PRIMARY KEY ( (block_id), nimi, key) )';

{% endhighlight %}

<small>Listaus 4. *kurssi_list*-taulu</small>


*Listauksen 4* esittämän `kurssi_list` -taulun rivit talletetaan yhteen partitioon taulun `opettaja_list` (*Listaus 1*) tapaan.


{% highlight javascript %}

const CREATE_TABLE_kurssit = '        \
CREATE TABLE IF NOT EXISTS kurssit (  \
  key     uuid,                       \
  tunnus  text,                       \
  nimi    text,                       \
  laajuus text,                       \
  opettaja frozen<opettaja>,          \
PRIMARY KEY (key) )';

{% endhighlight %}

<small>Listaus 5. *kurssit*-taulu</small>


{% highlight javascript %}

const CREATE_TYPE_opettaja = '        \
CREATE TYPE IF NOT EXISTS opettaja (  \
  key       uuid,                     \
  sukunimi  text,                     \
  etunimi   text                      \
)';

{% endhighlight %}

<small>Listaus 6. *opettaja*-tyyppi</small>


*Listauksen 5* esittämässä `kurssit` -taulun perustamiskomennossa  viitataan tyyppiin `opettaja`, jonka määrittely on *Listauksessa 6*.

Tehtäväpohjan moduulissa `configs/db_seed.js` on koodi, joka muodostaa tietokannan datoineen sovelluksen jokaisen käynnistyskerran yhteydessä. Jos tietokanta on jo muodostettu, em. moduulissa voi vaihtaa tietyt kommenttimerkit toiseen paikkaan[^2], jolloin moduuli ei luo tietokantaa uudelleen.

[^2]: Moduuliin on kirjattu tästä ohje

Cassandra:n on oltava käynnissä sovellusta käytettäessä. Bitnamista ladattavissa oleva [linux-virtuaalikone][cassandra-vm] käynnistää automaatisesti Cassandran, johon liittyvät tunnistetiedot ovat näkyvissä koneen *login*-ikkunassa. Moduuliin `configs/db_connection.js` kirjattujen tunnistetietojen tulee olla ne, jotka näkyvät virtuaalikoneessa. Koneeseen ei tarvitse kirjautua.

#### Toiminnot

Sovellus toteuttaa edellisistä tehtävistä tutun sivukartan:

![sivukartta](https://www.lucidchart.com/publicSegments/view/d84f9961-ce43-4b79-bac2-7405afa830ac/image.png)

<small>Kuva 2. Sivukartta</small>

Tietokantataulut ja mallien metodikokonaisuus vastaavat sivukarttaa (*Kuva 2*) - neljä taulua, neljä metodia, neljä sivua. Kaikki sivuilla esitetyt luettelot ovat nousevassa aakkosjärjestyksessä. 

Kussakin metodissa on ainoastaan yksi *CQL*:n *SELECT*-komento. Mikään komennoista ei sisällä *ORDER BY* -lausetta. 


#### Palautettava aineisto

**Palauta** tehtävän ratkaisuna tiedostot `models/Opettaja.js` ja `models/Kurssi.js`. Varmista ennen palautusta, että sovellus toimii tehtäväkuvauksen mukaan sovellusta ajamalla sekä käyttäen oheistettuja Selenium-testejä.


### Vihjeitä ja lisätietoja


#### Virtuaalikoneesta

Luokkakoneisiin on asennettu  *VMWare Workstation* - ja *VMWare Player* -ohjelmistot, joilla voi ajaa virtuaalikoneita. Bitnamin kone on [ova][ova]-formaatissa, jonka virtualisointiohjelmisto osaa purkaa käyttämäänsä muotoon. Lataa virtuaalikone käyttöä varten luokkakoneen `C:\Temp`-hakemistoon, jonne kaikilla on kirjoitusoikeus.

[ova]: https://en.wikipedia.org/wiki/Open_Virtualization_Format

Virtualisointiohjelmiston voi latata halutessaan myös omaan kehitysympäristöön. [VMWare Workstation Player][player]:ia voi käyttää vapaasti, kunhan käyttö on ei-kaupallista.
Binamin konetta voi ajaa myös [VirtualBox][VirtualBox]:lla, jonka voi niin ikään ladata veloituksetta. 

[VirtualBox]: https://www.virtualbox.org
[player]: http://www.vmware.com/products/player/playerpro-evaluation.html

Cassandran voi latata myös [sen omalta][cassandra-download] tai [DataStax:in][datastax-download] sivustolta, mutta Bitnamin virtuaalikoneen käyttöönotto lienee näitä yksinkertaisempaa.  

[cassandra-download]: http://cassandra.apache.org/download/
[datastax-download]: https://academy.datastax.com/planet-cassandra/cassandra


#### Tietokannan rakenteesta

[DataStax][dataModeling]:n sivustolla Cassandran tietomallista kerrotaan mm. seuraavaa:

> * A table stores data based on a primary key, which consists of a partition key and optional clustering columns.
* A partition key defines the node on which the data is stored.
* A clustering column defines the order of data stored in a row.
* A primary key is used to access the data in the table.

[dataModeling]: http://docs.datastax.com/en/landing_page/doc/landing_page/dataModeling.html

Edellisen lainauksen perusteella taulun riviksi (*row*) voidaan kutsua sitä, minkä taulussa yksilöi partitioavain (*partition key*). Parempi termi tälle saattaisi olla *partitio*, koska tietokantaan kohdistuva kysely saattaa palauttaa useita rivejä *rivejä* yhdestä partitiosta. Seuraavassa on vielä uudelleen *Listauksessa 1* esitetty komento.

{% highlight sql %}

CREATE TABLE IF NOT EXISTS opettaja_list (
  block_id  int,
  key       uuid,
  sukunimi  text,
  etunimi   text,
PRIMARY KEY ((block_id), sukunimi, etunimi, key) )

{% endhighlight %}

<small>Listaus 7. *opettaja_list*-taulu</small>

Cassandra-tietokannan suunnittelun lähtökohtana on tyypilisesti se, että kyselyn edellyttämä data toimitetaan mahdollisimman nopeasti. Tällöin tiedot ovat yleestä yhdessä taulussa ja juuri kyselyn edellyttämässä järjestyksessä. *Listauksen 7* taulu on muodostettu tämän periaatteen mukaan - taulu tuottaan datan näkymään, joka esittää aakkosjärjestyksessä olevan opettajaluettelon. 

Taulun data on talletettu yhteen partitioavaimen *block_id* (arvo: 1) määrittelemään partitioon so. tarvittava data kokonaisuudessaan pystytään paikantamaan *block_id:n* perusteella. Komennossa oleva perusavain muodostuu myös muista osista, *sukunimi*, *etunimi* ja *key*, joka määrittelevät rivien järjestyksen partitiossa ja toisaalta varmistavat sen, että perusavain yksilöi rivin. Täten Cassandran perusavaimella on huomattavasti moninaisempi merkitys kuin perusavaimella relaatiotietokannassa.

Seuraavassa on jo *Listauksessa 2* esitetty taulu, josta tuotetaan data yksittäisen opettajan tiedot esittävään näkymään:


{% highlight sql %}

CREATE TABLE IF NOT EXISTS opettajat (
  key       uuid,
  sukunimi  text STATIC,
  etunimi   text STATIC,
  kurssi    frozen<kurssi>,
PRIMARY KEY ((key), kurssi) )

{% endhighlight %}

<small>Listaus 8. *opettajat*-taulu</small>

*Listausten 7 ja 8* perusteella voidaan todeta, että sama data on talletettu yhtä useampaan tauluun - taulujen rakenne perustuu kyselyjen määräämään tarpeeseen: opettajan erittely-näkymä esittää yksittäisen opettajan tiedot mukaan lukien opettajan kurssit nimen mukaisessa aakkosjäjestyksessä.

Kunkin opettajan tiedot kursseineen on talletettu omaan, *key*-attribuutin määräämään, partitioon. Partition sisältämät opettajan pitämät kurssit on järjestetty kurssin nimen mukaiseen aakkosjärjestykseen[^3], koska *kurssi* on perusavaimessa partitioavaimen jälkeen. *Listauksessa 8* esiityvä `STATIC`-määre tarkoittaa sitä, että attribuutti talletetaan partitioon ainoastaan yhteen kertaan (esim. tässä *sukunimeä* ei toisteta jokaisen *kurssin* rinnalla).
 
[^3]: *kurssi* on *nimi*- ja *key*-attribuutit sisältävä tyyppi, jossa *nimi* on ensimmäisenä (*Listaus 3*)

#### Sovellusrajapinnasta

Cassandra-tietokannan sovellusrajapinta on kuvattu [rajapinnan käsikirjassa][Driver]. Tässä rajapinnasta käytetään sen [client][class.Client]-oliota, jonka sovelluksen mallit ottavat käyttöönsä tunnuksella `db`. Kaikki tarvittavat kyselyt voidaan toteuttaa olion [`execute`][execute] -metodilla[^4], mutta yhdessä kyselyssä toteutus saattaa muodostua yksinkertaisemmaksi [`eachRow`][each-row]-metodia käyttämällä. Metodeille annetaan perametrina [CQL][CQL]-kielisiä komentoja, jotka tässä eivät mitenkään eroa SQL:stä.


[Driver]: http://docs.datastax.com/en/developer/nodejs-driver/3.2/
[class.Client]: http://docs.datastax.com/en/developer/nodejs-driver/3.2/api/class.Client/
[execute]: http://docs.datastax.com/en/developer/nodejs-driver/3.2/api/class.Client/#execute
[each-row]: http://docs.datastax.com/en/developer/nodejs-driver/3.2/api/class.Client/#each-row
[CQL]: http://docs.datastax.com/en/cql/3.1/index.html


[^4]: Callback-based API; node.js ei välttämättä vielä tue toista (Promise-based API, using async/await)


<br/>


