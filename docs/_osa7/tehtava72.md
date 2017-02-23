---
layout: exercise_page
title: "Tehtävä 7.2: Kurssit ja opettajat, OrientDB (Graph)"
exercise_template_name: W7E02.KurssitJaOpettajatOrientDBGraph
exercise_discussion_id: 77220
exercise_upload_id: 313009
---

Laadi sovellus, jolla voidaan tarkastella *kurssi- ja opettajatietoja* sekä *yhteenveto-* että *erittelymuotoisten* näkymien kautta. [Tähtävän 5.3](../../osa5/tehtava53) tapaan  *Kurssi*-sivu esittää kurssien välisiä esitieto-riippuvuuksia. Kuten [edellisessä tehtävässä](../tehtava71) tietokantaratkaisuna on tässä [OrientDB][OrientDB], josta nyt hyödynnetään sen graafiominaisuuksia.

[OrientDB]: http://orientdb.com

#### Mallit ja tietokanta

Tietokantaa käsitellään moduuleissa `models/Kurssi.js` ja `models/Opettaja.js` määriteltyjen `findAll`- ja `findByKey` -metodien kautta. Tehtävä ratkaistaan täydentämällä nämä moduulit. Muilta osin sovellus on pohjakoodissa valmiina.

Tietokanta muodostuu *Kurssi*- ja *Opettaja* -solmuista (`Vertex`) sekä *Opettaa*- ja *OnEsitieto* -kaarista (`Edge`). Nämä kaikki ovat tietokannan luokkia (`Class`), joita voidaan käsitellä SQL:n avulla vastaavasti kuin relaatiotietokannassa käsitellään tauluja SQL-komennoilla. *Kurssi* ja *Opettaja* on periytetty *V* -luokasta ("vertex"), ja *Opettaa* ja *OnEsitieto* luokasta *E* ("edge").

Tietueiden väliset yhteydet muodostuvat kaarien avulla. Kaari-luokka on verrattavissa relaatiotietokannan ns. välitauluun, jolla kuvataan tietojen välisiä n:m -yheyksiä. Kaarella on *in*- ja *out*-ominaisuudet, jotka ovat linkkejä kaareen liittyviin solmuihin. Esim. tilanteessa "*opettaja opettaa kurssia*", *opettaa* -kaaren *in*-linkki osoittaa *kurssiin* ja *out* -linkki *opettajaan*:

~~~~

  [ Opettaja ] -(out)----( Opettaa )-----(in)-> [ Kurssi ]

~~~~
<small>Kuva 1. </small>

Tietojen talletuksen yhteydessä *Opettaja* -tietueille muodostuu ominaisuudet *sukunimi* ja *etunimi*, ja *Kurssi* -tietueille ominaisuudet *tunnus*, *nimi* ja *laajuus*. Tietueilla ei ole [edellisen tehtävän tietokannassa](../tehtava71/#mallit-ja-tietokanta) esiintyneitä *Link* / *Linklist* -tyyppisiä *opettaja* / *kurssit* -ominaisuuksia, koska tässä yhteydet on toteutettu kaarien avulla. 

*Opettaa* -tietueilla on *tyyppi* -ominaisuus, jolla esitetään onko esitieto pakollinen (p) vai suositeltava (s).

Pohjakoodin moduuli `config/db_seed.js` luo tietokantaan luokat ja tietueet sovelluksen käynnistyksen yhteydessä, jos tietokanta ei sisällä jotakin em. luokista. Tietokanta kuitenkin tulee perustaa [edellisen tehtävän](../tehtava71/#orientdbn-asennus-ja-tietokannan-perustaminen) tapaan tietokannan 
hallintatyökalulla. Tietokannan tulee olla nimellä `KouluGraph`. 


#### Toiminnot

Toimintoihin liittyvä sivukartta löytyy esim. [tehtävän 6.1](../../osa6/tehtava61/#toiminnot) kuvauksessa. Mallien toteuttama tietokantakäsittely tapahtuu siirryttäessä sivulta toiselle. [Tehtävän 5.3](../../osa5/tehtava53) kuvauksessa on esitetty kurssien esitieto-riippuvuuksiin liittyvä sovelluksen ulkoinen käyttäytyminen [^1].

[^1]: [Tehtävän 5.3](../../osa5/tehtava53) kuvauksen alussa on esitietoriippuvuuksia esittävä kuva, jossa sama kurssi esiintyy sekä *Välittömät esitiedot* - että *Muut esitiedot* -luettelossa. Tässä tehtävässä *Välittömät esitiedot* -luettelossa oleva kurssi ei ole mukana enää *Muut esitiedot* -luettelossa.

#### Palautettava aineisto

**Palauta** tehtävän ratkaisuna tiedostot `models/Opettaja.js` ja `models/Kurssi.js`. Varmista ennen palautusta, että sovellus toimii tehtäväkuvauksen mukaan sovellusta ajamalla. Pohjakoodi ei sisällä testejä.

### Vihjeitä ja lisätietoja

Tehtävässä on täydennettävänä neljä metodia, joista `findAll` -metodit voi kopioida suoraan [edellisen tehtävän](../tehtava71) ratkaisusta. Ehdotukset `models/Opettaja.js` -moduulin `findByKey`- metodiin liittyviksi SQL-komennoiksi on osana pohjakoodia, mutta näitä käyttävä koodi tulee laatia metodin sisällöksi. Moduulissa `models/Kurssi.js` tilanne on päinvastoin: `findByKey` -metodi on kutakuinkin valmis, mutta sen viittaamat SQL-komennot on jätetty laadittavaksi.

Pohjakoodi ehdottaa, että tiedot opettajan erittelysivulle tuotetaan kahdella SQL-komennolla (*Listaukset 1 ja 2*): opettajan perustiedot haetaan *Opettaja* -luokan (solmu) kautta ja opettajan kurssit *Opettaa* -luokkaan (kaari) kohdistuvalla kyselyllä.


{% highlight javascript %}

const FindOpettajaByKey = '\
SELECT \
   @RID.substring(1) AS key, \
   sukunimi, \
   etunimi \
FROM Opettaja \
WHERE @RID = :rid';

{% endhighlight %}

<small>Listaus 1. </small>


{% highlight javascript %}

const FindKurssitUsingEdge = '\
SELECT \
  in.nimi AS nimi,    \
  in.@RID.substring(1) AS key \
FROM Opettaa \
WHERE out = :opettaja_rid \
ORDER BY nimi ';

{% endhighlight %}

<small>Listaus 2. </small>

*Listauksessa 2* ominaisuus *out* on linkki, joka osoittaa *Opettaja* -tietueeseen ja *in* -ominaisuus *Kurssi* -tietueeseen osoittava linkki. 

*Listauksessa 3* on moduulista `models/Kurssi.js` löytyvä `findByKey` -metodi, joka tuottaa tiedot kurssin erittelysivulle. Myös tässä ratkaisu perustuu yhtä useamman kyselyn käyttöön. Metodin toteutuksen rakenneperiaatetta on kuvattu [tehtävässä 5.3](../../osa5/tehtava53/#tehtvn-toteutuksesta). 


{% highlight javascript %}


Kurssi.findByKey = (kurssi_key, callback) => {

   // nämä rivit pois-kommentoidaan
   callback({});
   return;
   //

   db.record.get('#' + kurssi_key).then(kurssi => {
   
      Promise.all([
      
         db.query(FindOpettajaUsingEdge, {
            params: {kurssi_rid: kurssi['@rid']}
         }),
         db.query(FindValittomatEsitiedot, {
            params: {kurssi_rid: kurssi['@rid']}
         }),
         db.query(FindEsitietoKursseille, {
            params: {kurssi_rid: kurssi['@rid']}
         }),
         db.query(TraverseMuutEsitiedot, {
            params: {kurssi_rid: kurssi['@rid']}
         })
         
      ]).then(resArr => {
      
         kurssi.opettaja = resArr[0][0];
         kurssi.valittomat_esitiedot = resArr[1];
         kurssi.esitieto_kursseille = resArr[2];
         kurssi.muut_esitiedot = resArr[3];
         
         callback(kurssi);
         
      }).catch(error => {
         log(error);
         callback({});
      });
   }).catch(error => {
      log(error);
      callback({});
   });
};

{% endhighlight %}

<small>Listaus 3. </small>


*Listaus 3* viittaa neljään merkkijonojen sisältönä olevaan SQL-komentoon: *FindOpettajaUsingEdge*, *FindValittomatEsitiedot*, *FindEsitietoKursseille* ja *TraverseMuutEsitiedot*. Näistä kolme ensimmäistä voidaan toteuttaa samalla periaatteella kaareen kohdistuvalla kyselyllä kuin *Listauksessa 2* oleva komento. 

*TraverseMuutEsitiedot* tuottaa kurssin epäsuorat esitietovaatumukset. (Esim. *Kuvassa 2* kurssi *Ohjelmiointitekniikka* on kurssin *Tietorakenteet* epäsuora esitieto.) Tämänkaltaiset ongelmat voidaan voidaan ratkaista [`Traverse`][Traverse] -komennolla.

[Traverse]: http://orientdb.com/docs/last/SQL-Traverse.html

~~~~

[Ohjelmiointitekniikka]-->[Olio-ohjelmointi]-->[Tietorakenteet]

~~~~
 
<small>Kuva 2. </small>


Seuraavassa on eräs `TRAVERSE`-komentoa soveltava kysely, joka poimii kavereita kaveri-polun varrelta tiettyltä etäisyydeltä. `WHERE` poistaa tuloksesta henkilön (`#10:1234`), josta polun seuraaminen lähti liikkeelle.

{% highlight sql %}

SELECT 
FROM (TRAVERSE out("Friend") FROM #10:1234 MAXDEPTH 4) 
WHERE $depth >= 1


{% endhighlight %}

<small>Listaus 4. </small>


<br/>



