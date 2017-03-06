---
layout: exercise_page
title: "Tehtävä 2.3: Kurssit ja opettajat, CRUD"
exercise_template_name: W2E03.KurssitJaOpettajatCRUD
exercise_discussion_id: 73565
exercise_upload_id: 307539
---


> Tehtävälle on käytettävissä myös toinen, arkkitehtuuriltaan hieman erilainen, pohjakoodi, jonka voi halutessaan ottaa oman ratkaisun lähtökohdaksi (ks. sivun lopussa kohta [Tehtävän pohjakoodi, versio 170121](#tehtvn-pohjakoodi-versio-170121)).



Täydennä [Tehtävän 2.2](../tehtava22) ratkaisua[^1] siten, että se sisältää toiminnot tietokannan tietojen ylläpitoa[^2] varten.   

[^1]: tietojen kyselyihin liittyen tässä on edellisiin tehtäviin verrattuna sellainen pieni muutos, että yksittäisen kurssin tiedot haetaan `id`-attribuutin perusteella `tunnus`-attribuutin sijaan 

[^2]: lisäys, muutos ja poisto

Edellisten tehtävien ([2.1](../tehtava21) ja [2.2](../tehtava22)) ratkaisujen sivukartta on seuraavanlainen:


![sivukartta](https://www.lucidchart.com/publicSegments/view/d84f9961-ce43-4b79-bac2-7405afa830ac/image.png)

<small>Kuva 1. Sivukartta: Kurssien ja opettajien *luettelot* ja *eritelyt*.</small>


Tässä sivukartta laajenee sekä kurssien että opettajien osalta siten, että kuhunkin tietojen ylläpito-operaatioon (lisäys, muutos, poisto) liittyy oma lomakesivunsa (*Kuva 2*).


![sivukartta](../img/w2e03.png)

<small>Kuva 2. Sivukartta: sisältää lomakkeet (*uusi*, *muuta* ja *poista*) tietojen ylläpitoa varten.</small>


*Luettelo*-sivulta voidaa siirtyä lomakkeelle (*uusi*), jolla voi syöttää uuden rivin tietokantaan. Muille lomakkeille (*muuta*, *poista*) voidaan siirtyä *erittely*-sivulta. Kullakin lomakkeella voi joko vahvistaa tai peruuttaa ylläpito-operaation ao. painikkeiden avulla. *Kuvassa 2* on esitetty, mille sivuille käsittely siirtyy lomakesivuita painikkeiden klikkauksen seurauksena. 

Sovelluksen lähdekoodi jäsentyy edellisen tehtävän kanssa samalla tavalla (*Kuva 3*). Näkymiä rakenteessa on kuitenkin aiempaa enemmän.

~~~
Sources
 ├── main.js
 ├── configs
 ├── controllers
 │     ├── kurssiController.js 
 │     └── opettajaController.js 
 ├── models
 └── views
     ├── kurssi.hbs
     ├── kurssi_list.hbs
     ├── kurssi_insert.hbs
     ├── kurssi_update.hbs
     ├── kurssi_delete.hbs
     ├── opettaja.hbs
     ├── opettaja_list.hbs
     ├── opettaja_insert.hbs
     ├── opettaja_update.hbs
     ├── opettaja_delete.hbs
     └── layouts
          └── main.hbs                 
~~~

<small>Kuva 3. Sovelluksen lähdekoodin struktuuri.</small>


Kontrollereita lukuunottamatta sovellus on jo rakennettu valmiiksi. Kontrollereihin[^2a] sisällytetään ORM-rajapinnan kautta tapahtuva tietokantakäsittely, johon sisältyy  kyselyjen lisäksi nyt tietokannan tilaa muuttavat operaatiot. Kyselyt voi kopioida edellisen tehtävän ratkaisusta[^1].

[^2a]: muutamaan pyyntöön liittyvät funktiot ovat pohjassa valmiina

Tietokanta sijaitsee projektin `database`-kansiossa nimellä `koulu.sqlite`[^3]. Tietokanta datoineen muodostuu sovelluksen käynnistyksen yhteydessä, jos kansiosta ei löydy em. nimistä tiedostoa. Tietokanta näkyy kontrollereissa viittauksilla `global.Kurssi` ja `global.Opettaja`, jotka ovat Sequelizen [Model][model]-objekteja. 

[model]: http://docs.sequelizejs.com/en/v3/api/model/

[^3]: `databases`-kansio näkyy NetBeansin *Files*-ikkunassa, mutta ei *Projects*-ikkunassa

**Palauta** tehtävän ratkaisuna tiedostot `kurssiController.js` ja `opettajaController.js`. Varmista ennen palautusta, että tehtäväpohjassa olevat Selenium-testit menevät läpi. Sovelluksen on oltava käynnissä testejä ajettaessa ja tietokannan tulee olla alkutilassaan.


### Vihjeitä ja lisätietoja

Sovellukset kontrollerit rakentuvat seuraavasti:

{% highlight javascript %}

module.exports = (app) => {
    // insert
    app.get('/kurssit/insert', (req, res) => {});
    app.post('/kurssit/insert', (req, res) => {});
    // update
    app.get('/kurssit/:id/update', (req, res) => {});
    app.post('/kurssit/update', (req, res) => {});
    // delete
    app.get('/kurssit/:id/delete', (req, res) => {});
    app.post('/kurssit/delete', (req, res) => {});
    // select
    app.get('/kurssit', function (req, res) {});
    app.get('/kurssit/:id', function (req, res) {});
};

{% endhighlight %}

<small>Listaus 1. Sovelluksen lähdekoodin struktuuri.</small>

Kuhinkin ylläpito-operaatioon liittyy kaksi polkua, joista toiseen tulee pyyntö `GET`-metodilla ja toiseen `POST`-metodilla. *GET*-pyyntöön tuotetaan vasteeksi  ylläpito-operaatioon liittyvä lomake-sivu. *POST*-pyyntöön reagoidaan toteuttamalla  tietokantaoperaatio ja ohjaamalla käsittely sitten seuraavalle sivulle (ks. *Kuvan 2* sivukartta).

Alla olevassa *Listauksessa 2* on esitetty metodi, joka tuottaa vasteeksi sivun (*Listaus 3*) sisältäen lomakkeen, jonka avulla voidaan ylläpitää kurssin tietoja.

{% highlight javascript %}

app.get('/kurssit/:id/update', (req, res) => {

   global.Kurssi.findById(req.params.id).then((kurssi) => {

      if (!kurssi) {
         res.render('kurssi');
         return;
      }

      global.Opettaja.findAll({order: 'sukunimi'}).then((opettajat) => {

         opettajat.forEach((opettaja) => {
            opettaja.selected = opettaja.id === kurssi.opettaja_id ? 'selected' : '';
         });
         
         res.render('kurssi_update', {
            kurssi: kurssi,
            opettajat: opettajat
         });
         
      });

   });
});

{% endhighlight %}

<small>Listaus 2. Ote tiedostosta *kurssiKontroller.js*.</small>


Polussa on mukana pyyntöön liittyvä parametri. Polku voi olla esim. `/kurssit/10/update`, jossa `10` on muutoksen kohteena olevan kurssin `id`-attribuutin arvo tietokannassa. Polussa olevaan `id`-parametriin päästään käsiksi viittauksella `req.params.id`, joka perusteella tietokannasta haetaa rivi käyttäen `findById` -metodia. Jos riviä ei jostakin syystä löydy, hahmonnetaan näkymä `kurssi.hbs` ilman dataa, ja päätetään funktion suoritus tähän.

Jos tietokannasta saadaan muutoksen kohteena oleva kurssi, luettaan tietokannasta `findAll`-metodilla kaikki opettajat lomakkeella olevaa `select`-elementtiä varten (ks. *Listaus 3*). Jotta lomakkeella olisi oikea opettaja valittuna, kullekin opettajalle määritellään tässä `selected`-ominaisuus, jonka arvo määrää aktiivisen valinnan (*Listaus 3*). Tämän jälkeen hahmonnetaan näkymä `kurssi_update.hbs` (*Listaus 3*) so. tuotetaan pyynnön vasteeksi muutoslomakkeen sisältävä sivu.

{% highlight html %}
{% raw %}


<h2>Päivitä kurssi</h2>
<form method="POST" action="/kurssit/update">
   <div>
      Tunnus: 
      <input name="tunnus" value="{{kurssi.tunnus}}"/>
   </div>
   <div>
      Nimi: 
      <input name="nimi" value="{{kurssi.nimi}}"/>
   </div>
   <div>
      Laajuus: 
      <input name="laajuus" value="{{kurssi.laajuus}}"/>
   </div>
   <div>
      Opettaja: 
      <select name="opettaja_id">
         <option></option>
         {{#each opettajat}}
         <option value="{{id}}" {{selected}}>{{sukunimi}}, {{etunimi}}</option>
         {{/each}}
      </select>
   </div>
   <br/>
   <input type="hidden" name="id" value="{{kurssi.id}}"/>
   <input type="submit" name="_update" value="Talleta"/>
   <input type="submit" name="_cancel" value="Peruuta"/>
</form>

{% endraw %}
{% endhighlight %}

<small>Listaus 3. Näkymä *kurssi_insert.hbs*.</small>

Lomakeen (*Listaus 3*) *submit*-napin klikkaus aikaansaa `POST`-metodilla polkuun `/kurssit/update` pyynnön, joka käsitellään *Listauksen 4*  koodilla.

{% highlight javascript %}

app.post('/kurssit/update', (req, res) => {

   if (req.body._cancel) {
      res.redirect('/kurssit/' + req.body.id);
      return;
   }

   global.Kurssi.findById(req.body.id).then((kurssi) => {

      if (!req.body.opettaja_id.length)
         req.body.opettaja_id = null;

      kurssi.update(req.body).then((kurssi) => {
         res.redirect('/kurssit/' + kurssi.id);
      });
   });

});

{% endhighlight %}

<small>Listaus 4. Ote tiedostosta *kurssiKontroller.js*.</small>

Lomakkeen lähettämä data löytyy `req.body`-olion[^5] attribuuteista. Käsittelyn alussa selvitetään, onko käyttäjä lähettänyt lomakkeen klikkaamalla *Peruuta*-painiketta. Jos näin on, palautetaan käsittely kurssitietojen erittely -sivulle (ks. *Kuva 2*). Muussa tapauksessa luetaan muutettava kurssi tietokannasta.

[^5]: jäsennyksen toteuttaa `body-parser`-moduuli, jonka käyttöönotto on määritelty  tiedostossa `configs/config.js`

Lomakeelta saatavan `opettaja_id`-tiedon osalta on tässä tehtävä pieni "säätö" ennen tietojen muutoksen toteuttamista: mahdollisen tyhjän merkkijonon vaihtaminen arvoon `null`. 

Muutos tietokantaan voidaan toteuttaa [`update`][update]-metodilla, jonka suorituksen jälkeen käsittely ohjataan kurssitietojen erittely -sivulle.


[`update`][update]-metodin lisäksi tehtävän ratkaisussa tarvittaneen [`create`][create]- ja [`destroy`][destroy] -metodeja. Näiden käytöstä löytyy esimerkkejä *Sequelize*-dokumentaation kohdasta [Instances][instances].

[update]: http://docs.sequelizejs.com/en/v3/api/instance/#updateupdates-options-promisethis
[create]: http://docs.sequelizejs.com/en/v3/api/model/#createvalues-options-promiseinstance
[destroy]: http://docs.sequelizejs.com/en/v3/api/instance/#destroyoptions-promiseundefined

[instances]: http://docs.sequelizejs.com/en/v3/docs/instances/

#### Muutoksia

170121

* uusi projektipohja (sivun alussa olevan linkin kohdetta ei kuitenkaan ole muutettu);  ks. tarkemmin alla kohdassa "Tehtävän pohjakoodi, versio 170121


170119

* uusi projektipohja (kattavammat testit; korjauksia näkymiin)

#### Tehtävän pohjakoodi, versio 170121

[Tehtäväpohjan tähän versioon][uusi_pohja] on tehty tiettyjä arkkitehtuuritason muutoksia. Tehävän ratkaisun voi laatia tähän [uuteen pohjaan][uusi_pohja] tai siihen, jonka voi ladata sivun alussa olevasta linkistä. Uutta pohjaa käytettäessä tehtävän **ratkaisuna palautetaan** zip-arkistoituna sovelluksen lähdekoodin `controllers`-kansio.

[uusi_pohja]: /tkj2017k/pohjat/W2E03.KurssitJaOpettajatCRUD.v2.zip


Lähdekoodi rakentuu seuraavasti:

~~~
Sources
 ├── main.js
 ├── configs
 │     ├── log.js 
 │     ├── db_connection.js 
 │     ├── db_seed.js 
 │     └── db_seed 
 │           ├── kurssit.csv 
 │           └── opettajat.csv  
 ├── controllers
 │     ├── kurssit.js 
 │     ├── opettajat.js 
 │     ├── kurssit 
 │     │    ├── kurssit_insert.js  
 │     │    ├── kurssit_remove.js 
 │     │    ├── kurssit_select.js  
 │     │    └── kurssit_update.js   
 │     └── opettajat 
 │          ├── opettajat_insert.js  
 │          ├── opettajat_remove.js 
 │          ├── opettajat_select.js  
 │          └── opettajat_update.js   
 ├── models
 │     ├── Kurssi.js 
 │     └── Opettajat.js  
 └── views
~~~

<small>Kuva 4. Sovelluksen lähdekoodin struktuuri (versio 170121).</small>


Kuten aiemmin niin myös tässä tehtävänä on täydentää sovelluksen kontrollerien funktioiden runkoja. Tässä konrollerit on jatettu koodirivimäärältään pienempiin moduuleihin. Täydennettävät tiedostot löytyvät `kurssit`- ja `opettajat`-kansioista.

Sovellus rakentuu nyt niin, että tiedostot `kurssit.js`  ja `opettajat.js` määrittelevät polut, joihin sovellus reagoi hakemistoisaa `kurssit` ja `opettajat` olevien tiedostojen koodilla. Määritellyt polut kytketään sovellukseen päämoduulissa `main.js`.

Käsiteltävä tietokanta tässä luodaan ORM:n avulla. Pohjan edellisessä versiossa olevan `models/mappins.js` -tiedoston sisältö on jaettu tiedostoihin `models/Kurssi.js`, `models/Opettaja.js` ja `configs/db_connection.js`.


<br/>

