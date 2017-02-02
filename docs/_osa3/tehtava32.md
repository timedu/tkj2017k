---
layout: exercise_page
title: "Tehtävä 3.2: Kurssit ja opettajat, CRUD"
exercise_template_name: W3E02.KurssitJaOpettajatLevelCRUD
exercise_discussion_id: 74590
exercise_upload_id: 308964
---

Täydennä [tehtävän 3.1](../tehtava31) ratkaisua ominaisuuksiltaan siten, että sovelluksella voi opettaja- ja kurssitietojen tarkastelun ohella ylläpitää (*lisäys*, *muutos*, *poisto*) opettajatietoja. Tässä käsiteltävän tietokannan rakenne on myös hieman erilainen.

Sovelluksen lähdekoodi jäsentyy [edellisen tehtävän](../tehtava31) mukaisesti. Tosin näkymiä (`views`) on uusien ominaisuuksien myötä enemmän: `opettaja_insert.hbs`, `opettaja_update.hbs`ja `opettaja_delete.hbs`. 

Opettajatietojen ylläpitoon liittyen `controllers/opettajat.js` reagoi nyt `get`-pyynnöillä myös polkuihin 

* `/opettajat/insert`
* `/opettajat/:key/update` 
* `/opettajat/:key/delete` 

sekä `post`-pyynnöillä polkuihin 

* `/opettajat/insert`
* `/opettajat/update` 
* `/opettajat/delete`


Opettajan *mallissa* (*Kuva 1*) `models/Opettaja.js` on kolme lisämetodia: `create`, `update` ja `destroy`. 

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


Tietostoa `models/Opettaja.js` lukuunottamatta sovellus on rakennettu valmiiksi. Myös tiedoston sisältämän `update`-metodin voi jättää tilaan, jossa se on tehtävän pohjakoodissa.

[Edellisen tehtävän](../tehtava31) kaksi erillistä tietokantaa on yhdistetty yhdeksi `database`-kansiossa olevaksi tietokannaksi `koulu.level`. Tietokanta sisältää toisteisuutta siten, että kukin kyselyistä voidaan toteuttaa yksittäisellä rajapinnan `get`-metodilla. Toisaalta toisteisuus monimutkaistaa tietojen ylläpitoa.

Esim. *avaimella* `ks` tietokannasta löytyy seuraava `json`-muotoinen *arvo*:


{% highlight json %}

{
  "tunnus":"ks","sukunimi":"Käkilä","etunimi":"Simo",
  "kurssit":[
    {"tunnus":"PLA-32610","nimi":"Tietokantajärjestelmät"},
    {"tunnus":"PLA-32811","nimi":"Web-ohjelmointi"},
    {"tunnus":"PLA-32831","nimi":"Web-selainohjelmointi"},
    {"tunnus":"PLA-32841","nimi":"Web-palvelinohjelmointi"}
  ]
}

{% endhighlight %}

<small>Listaus 1. Opettaja tietokannassa (esimerkki)</small>


Opettajaan liittyvä *arvo* sisältää opettajan perustietojen lisäksi myös opettajan kurssien tunnukset ja nimet (*Listaus 1*). Avaimet `aa`-`zz`on tietokannassa varattu opettajille.

*Avaimella* `PLA-32610` tietokannasta löytyy seuraava `json`-muotoinen *arvo*:


{% highlight json %}

{
  "tunnus":"PLA-32610",
  "nimi":"Tietokantajärjestelmät",
  "laajuus":4,
  "opettaja":{"tunnus":"ks","sukunimi":"Käkilä","etunimi":"Simo"}
}

{% endhighlight %}

<small>Listaus 2. Kurssi tietokannassa (esimerkki)</small>


Kurssiin liittyvä *arvo* sisältää kurssin perustietojen lisäksi myös kurssin  opettajan tiedot (*Listaus 2*). Avaimet `PLA-00000`-`PLA-99999`on tietokannassa varattu opettajille.


Yksittäisten kurssi- ja opettaja-tietojen lisäksi tietokanta sisältää kurssien ja opettajien luettelot avaimilla `Kurssit` ja `Opettajat` (*Listaukset 3 ja 4*).


{% highlight json %}

[
  {"tunnus":"PLA-31100","nimi":"Ohjelmointitekniikka"},
  {"tunnus":"PLA-32100","nimi":"Olio-ohjelmointi"},
  {"tunnus":"PLA-32200","nimi":"Tietorakenteet"},
  ...
  {"tunnus":"PLA-33600","nimi":"Ohjelmistoprojekti"}
]

{% endhighlight %}

<small>Listaus 3. Kurssiluettelo tietokannassa avaimella *Kurssit*</small>


{% highlight json %}

{
  "ap":{"tunnus":"ap","sukunimi":"Ahtola","etunimi":"Pertti"},
  "jv":{"tunnus":"jv","sukunimi":"Jukola","etunimi":"Leevi"},
  "ks":{"tunnus":"ks","sukunimi":"Käkilä","etunimi":"Simo"},
  "ms":{"tunnus":"ms","sukunimi":"Mikama","etunimi":"Santtu"},
  "rn":{"tunnus":"rn","sukunimi":"Rämeenperä","etunimi":"Niilo"},
  "vk":{"tunnus":"vk","sukunimi":"Veto","etunimi":"Karri"}
}

{% endhighlight %}

<small>Listaus 4. Opettajaluettelo tietokannassa avaimella *Opettajat*</small>


**Palauta** tehtävän ratkaisuna tiedosto `models/Opettaja.js`. Varmista ennen palautusta, että tehtäväpohjassa olevat Selenium-testit menevät läpi. Sovelluksen on oltava käynnissä testejä ajettaessa ja tietokannan alkutilassaan.

### Vihjeitä ja lisätietoja

Sekä tiedon *lisäys* että *muutos* tapahtuu Noden [LevelDB-rajapinnan][levelup] metodilla [`put`][put]. *Poisto* voidan suorittaa metodilla [`del`][del]. Ylläpito-operaatioita  suoritetaan nipussa [`batch`][batch]-metodilla. (*Listaukset 5-7*)

[levelup]: https://github.com/Level/levelup/blob/master/README.md

[put]: https://github.com/Level/levelup#put
[del]: https://github.com/Level/levelup#del
[batch]: https://github.com/Level/levelup#batch



{% highlight javascript %}

db.put(key, value, function (err) {
  // ...
})

{% endhighlight %}

<small>Listaus 5. *put*-metodi</small>


{% highlight javascript %}

db.del(key, function (err) {
  // ...
});

{% endhighlight %}

<small>Listaus 6. *del*-metodi</small>


{% highlight javascript %}

var ops = [
    { type: 'del', key: 'ks' }
  , { type: 'put', key: 'ps', value: 'Simo Pasanen' }
  , { type: 'put', key: 'PLA-99100', value: 'Johdatus konkatenointiin' }
];

db.batch(ops, function (err) {
  // ...
});

{% endhighlight %}

<small>Listaus 7. *batch*-metodi</small>


Seuraavassa (*Listaus 8*) on koodi, joka toteuttaa sovelluksessa opettaja-tietojen  muutoksen (`models/Opettaja.js`). Tietokannassa on toisteisuutta, so. sama tieto on talletettu useaan paikkaan tietokannassa, ja siten tarvittava koodi muodostuu suhteellisen pitkäksi. Esim. opettajan sukunimi on talletettu yksittäisen opettajatiedon lisäksi opettajaluetteloon sekä jokaiselle opettajan kurssille. 

Muutamalla rivillä listauksessa suoritetaan "oliosijoitus" käyttäen metodia [`Object.assign`][assign].

[assign]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/assign

{% highlight javascript %}


// funktion ensimmäisenä parametrina on opettaja-olio, joka sisältää
// muuttuneet opettajatiedot

Opettaja.update = (newVal, callback) => {

   // taulukko, johon asetetaan nipussa suoritettavat
   // päivitysoperaatiot

   const ops = []; 

   // luetaan opetajaluettelo 
   // (ks. tiedon muoto: Listaus 4)

   Opettaja.findAll((opettajat) => {

      // sijoitetaan muuttunut tieto luettelon ao. kohtaan

      opettajat[newVal.tunnus] = Object.assign({}, newVal);

      // asetetaan taulukkoon operaatio, joka tallettaa
      // tietokantaan muuttuneen opettajaluettelon

      ops.push({
         key: 'Opettajat',
         value: opettajat
      });

      // luetaan muutettava opettaja-tieto tietokannasta

      Opettaja.findByKey(newVal.tunnus, (opettaja) => {
      
         // asetetaan taulukkoon operaatio, joka tallettaa
         // tietokantaan muuttuneen opettajatiedon
      
         ops.push({
            key: opettaja.tunnus,
            value: Object.assign(opettaja, newVal)
         });
    
         // luetaan tietokannasta kaikki kurssit 

         db.createValueStream({

            gte: 'PLA-00000',
            lte: 'PLA-99999',
            valueEncoding: 'json'

         }).on('data', (kurssi) => {
         
            // tutkitaan onko luettu kurssi opettajan kurssi 

            if (kurssi.opettaja.tunnus === newVal.tunnus) {

               // muutetaan kurssin yhteyteen talletetun opettajan
               // tietoja 

               kurssi.opettaja = Object.assign(kurssi.opettaja, opettaja);

               // asetetaan taulukkoon operaatio, joka tallettaa
               // tietokantaan muuttuneen kurssitiedon
                
               ops.push({
                  key: kurssi.tunnus,
                  value: kurssi
                });                              
            }

         }).on('end', () => {

            // kun kaikki kurssit on käyty läpi, suoritetaan ops-
            // taulukkoon kirjatut päivitysoperaatiot
            
            db.batch(ops, {valueEncoding: 'json'}, () => {
            
               // kun päivitysoperaatiot on suoritettu, kutsutaan
               // parametrina saatua funktiota 
            
               callback && callback(opettaja);               
               
            });

         });

      });

   });

};


{% endhighlight %}

<small>Listaus 8. Sovelluksessa oleva funktio [^1], joka tekee muutoksen (redundanttisiin) opettaja-tietoihin</small>

[^1]: Tehtäväpohjassa kurssitietojen päivitysoperaatioita ops-taulukkoon keräävä kohta saattaa olla erheellisesti ao. if-rakenteen ulkopuolella. Tämä aikaansaa sen, että joukko päivityksiä suoritetaan turhaan (kurssille asetetaan arvo, joka on jo tietokannassa).


#### Opettajan lisäys tietokantaan

Tehtäväpohjassa on lisäyksen toteuttamista varten seuraavanlainen funktiorunko tiedostossa `models/Opettaja.js`:


{% highlight javascript %}

Opettaja.create = (opettaja, callback) => {

   callback({});

//   opettaja.tunnus = 'zy' + Math.round((1000000 * Math.random()));
   
};

{% endhighlight %}

<small>Listaus 9. </small>


*Listauksessa 9* on kommentoituna koodi, jolla voidaan muodostaa *melkein* yksilöllisiä tunnisteita tietokantaan lisättäville opettajille niin, että tunnisteet osuvat tehtävässä opettajien tunnisteille varattuun haaruukkaan ('aa.'..'zz'). 


Funktiota `Opettaja.create` käyttää tiedostossa  `controllers/opettajat.js` määritelty kontrolleri:


{% highlight javascript %}

router.post('/insert', (req, res) => {

  // ...

   Opettaja.create(req.body, (opettaja) => {
      res.redirect('/opettajat/' + opettaja.tunnus);
   });

});


{% endhighlight %}

<small>Listaus 10. </small>


*Listauksen 10* koodi välittää lisäyksen toteuttavalle funktiolle `Opettaja.create` kaksi parametria, joista ensimmäinen on lomakkeelta saatu opettaja, esim.

{% highlight json %}

{
  "etunimi": "Bartholomeus",
  "sukunimi": "Simpsonius"
}

{% endhighlight %}

<small>Listaus 11. </small>


Lisäyksen suoritettuaan `Opettaja.create` kutsuu *Listauksessa 10* toisena parametrina olevaa funktiota, joka ohjaa käsittelyn (esim.) osoitteeseen `/opettajat/zy123456`, missä `zy123456` on lisätylle opettajalle muodostettu tietokantatunnus. 


Lisätty opettaja välittyy kontrollerille seuraavanlaisessa muodossa:

{% highlight json %}

{
  "tunnus": "zy123456",
  "etunimi": "Bartholomeus",
  "sukunimi": "Simpsonius",
  "kurssit": []
}

{% endhighlight %}

<small>Listaus 11. </small>

`Opettaja.create` tallettaa *Listauksen 11* kaltaisen objektin tietokantaan avaimella `zy123456`, mutta tietokannan toisteisuudesta johtuen funktion on myös päivitettävä tietokannassa avaimella `Opettajat` olevaa opettajaluetteloa (ks. *Listaus 4*).

Lisäyksen jälkeen *avameilla* `Opettajat` oleva *arvo* tietokannassa on seuraavanlainen:


{% highlight json %}

{
  ...
  "rn":{"tunnus":"rn","sukunimi":"Rämeenperä","etunimi":"Niilo"},
  "vk":{"tunnus":"vk","sukunimi":"Veto","etunimi":"Karri"},
  "zy123456":{"tunnus":"zy123456","sukunimi":"Simpsonius","etunimi":"Bartholomeus"}
}

{% endhighlight %}

<small>Listaus 12. </small>

Tietokannasta on lisäyksen yhteydessä siis ensin luettava *avaimella* `Opettajat` oleva *arvo*, muutettava arvoa ja sitten kirjoitettava muutettu *arvo* tietokantaan.

Arvon muutoksen voi toteuttaa seuraavanlaisella sijoituslauseella olettaen, että objekti `opettajat` sisältää *Listauksen 4* kaltaisen opettajaluettelon:
 

{% highlight javascript %}

opettajat['zy123456'] = {
  "tunnus": "zy123456",
  "etunimi": "Bartholomeus",
  "sukunimi": "Simpsonius",
}

{% endhighlight %}

<small>Listaus 13. </small>


Tietokantaan suoritetaan tässä kaksi kirjoitusoperaatiota: arvot avaimilla `zy123456` ja `Opettajat`. Nämä tulisi niputtaa yhteen so. suorittaa kirjoitusoperaatiot tietokantarajapinnan `batch`-metodilla. *Listauksessa 8* on esimerkki `batch`-metodin käytöstä.


#### Muutoksia

* 170201: kohtaan *Vihjeitä ja lisätietoja* lisätty `Opettaja.create`-funktion toteuttamiseen liittyviä vinkkejä

<br/>







