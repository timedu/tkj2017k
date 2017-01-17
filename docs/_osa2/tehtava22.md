---
layout: exercise_page
title: "Tehtävä 2.2: Kurssit ja opettajat, ORM"
exercise_template_name: W2E02.KurssitJaOpettajatORM
exercise_discussion_id: 73564
exercise_upload_id: 307538
---

Laadi relaatiotietokantaa käsittelevä sovellus, joka käyttäytyy kuten [Tehtävän 2.1](../tehtava21) ratkaisu, mutta toteuttaa tietokanakäsittelyn [Sequelize][sequelize]-ORM:n avulla.

[sequelize]: http://www.sequelizejs.com

Sovelluksen lähdekoodi jäsentyy edellisen tehtävän kanssa samalla tavalla (*Kuva 1*). Uutena tiedostona on kuitenkin `models`-kansiossa sijaitseva, ORM-määritykset sisältävä, `mapping.js`.

~~~
Sources
 ├── main.js
 ├── configs
 ├── controllers
 │     ├── kurssiController.js 
 │     └── opettajaController.js 
 ├── models
 │     └── mappings.js 
 └── views
~~~

<small>Kuva 1. Sovelluksen lähdekoodi</small>

Kontrollereita lukuunottamatta sovellus on jo rakennettu valmiiksi. Kontrollereihin sisällytetään ORM-rajapinnan kautta tapahtuva tietokantakäsittely. 

Tietokanta sijaitsee projektin `database`-kansiossa nimellä `koulu.sqlite`[^1]. Tietokanta datoineen muodostuu sovelluksen käynnistyksen yhteydessä, jos kansiosta ei löydy em. nimistä tiedostoa. Tietokanta näkyy kontrollereissa viittauksilla `global.Kurssi` ja `global.Opettaja`, jotka ovat Sequelizen [Model][model]-objekteja. 

[^1]: `databases`-kansio näkyy NetBeansin *Files*-ikkunassa, mutta ei *Projects*-ikkunassa

**Palauta** tehtävän ratkaisuna tiedostot `kurssiController.js` ja `opettajaController.js`. Varmista ennen palautusta, että tehtäväpohjassa olevat Selenium-testit menevät läpi. Sovelluksen on oltava käynnissä testejä ajettaessa.

### Vihjeitä ja lisätietoja

Tehtäväpohjan tiedostossa `models/mappings.js` (*Listaukset 1-4*) on kuvattu sovelluksessa käytettävien objektien liittyminen tietokantaan. 

{% highlight javascript %}

    var sequelize = new Sequelize('koulu', '', '', {
        dialect: 'sqlite',
        storage: DatabaseFile,
        define: {
            timestamps: false
        }
    });

{% endhighlight %}

<small>Listaus 1. Sequelize-objekti.</small>

Ensin luodaan [Sequelize][sequelize-obj]-objekti, joka muodostaa yhteyden tietokantaan (*Listaus 1*). Objektin `define`-metodilla voidaan sitten luoda [Model][model]-objekteja (*Listaukset 2 ja 3*), jotka toimivat sovelluksessa tietokannan taulujen vastinpareina. 

[sequelize-obj]: http://docs.sequelizejs.com/en/v3/api/sequelize/
[model]: http://docs.sequelizejs.com/en/v3/api/model/


{% highlight javascript %}

    var Opettaja = sequelize.define('opettaja', {
        id: {
            type: Sequelize.DataTypes.INTEGER, 
            autoIncrement: true, 
            primaryKey: true
        },
        tunnus: Sequelize.DataTypes.STRING,
        etunimi: Sequelize.DataTypes.STRING,
        sukunimi: Sequelize.DataTypes.STRING
    }, {
        freezeTableName: true
    });


{% endhighlight %}

<small>Listaus 2. Model-objekti *Opettaja*.</small>



{% highlight javascript %}

    var Kurssi = sequelize.define('kurssi', {
        id: {
            type: Sequelize.DataTypes.INTEGER, 
            autoIncrement: true, 
            primaryKey: true
        },
        tunnus: Sequelize.DataTypes.STRING,
        nimi: Sequelize.DataTypes.STRING,
        laajuus: Sequelize.DataTypes.STRING
    }, {
        freezeTableName: true
    });

{% endhighlight %}

<small>Listaus 3. Model-objekti *Kurssi*.</small>


Tietokannan käsittely tapahtuu Model-objektin metodien avulla. Tämän tehtävän ratkaisussa käyttökelpoisia metodeja lienevät [findAll][findAll], [findById][findById] ja [findOne][findOne]. Sequelize-dokumentaation kohdissa [Models/Usage][models-usage] ja [Querying][querying] on näiden metodien käyttöön liittyviä esimerkkejä, joista muutama on poimittu alla olevaan kohtaan *Esimerkkejä*.

[findAll]: http://docs.sequelizejs.com/en/v3/api/model/#findalloptions-promisearrayinstance
[findById]: http://docs.sequelizejs.com/en/v3/api/model/#findbyidid-options-promiseinstance
[findOne]: http://docs.sequelizejs.com/en/v3/api/model/#findoneoptions-promiseinstance

[models-usage]: http://docs.sequelizejs.com/en/v3/docs/models-usage/
[querying]: http://docs.sequelizejs.com/en/v3/docs/querying/


Model-objektien väliset yhteydet kuvataan ao. metodien avulla, tässä käyttäen [belongsTo][belongsTo]- ja [hasMany][hasMany] -metodeja (*Listaus 4*).

[belongsTo]: http://docs.sequelizejs.com/en/v3/api/associations/#belongstotarget-options
[hasMany]: http://docs.sequelizejs.com/en/v3/api/associations/#hasmanytarget-options


{% highlight javascript %}

    Kurssi.belongsTo(Opettaja, {foreignKey: 'opettaja_id', as: 'Opettaja'});
    Opettaja.hasMany(Kurssi, {foreignKey: 'opettaja_id', as: 'Kurssit'});

{% endhighlight %}

<small>Listaus 4. Model-obejektien väliset assosiaatiot.</small>

*Listauksen 4* määrittely aikaansaa mm. sen, että `Kurssi`-objektin tietokantahaun tuloksena palauttama objekti sisältää metodin `getOpettaja`, ja vastaavasti `Opettaja`-objektin palauttama objekti metodin `getKurssit`.

#### Esimerkkejä

Seuraavat esimerkit on lainattu suoraan Sequelize-dokumentaation kohdasta [Models/Usage][models-usage].

{% highlight javascript %}

// search for known ids
Project.findById(123).then(function(project) {
  // project will be an instance of Project and stores the content of the table entry
  // with id 123. if such an entry is not defined you will get null
})

{% endhighlight %}


{% highlight javascript %}

// search for attributes
Project.findOne({ where: {title: 'aProject'} }).then(function(project) {
  // project will be the first entry of the Projects table with the title 'aProject' || null
})

{% endhighlight %}


{% highlight javascript %}

// find multiple entries
Project.findAll().then(function(projects) {
  // projects will be an array of all Project instances
})

{% endhighlight %}


{% highlight javascript %}

// search for specific attributes - hash usage
Project.findAll({ where: { name: 'A Project' } }).then(function(projects) {
  // projects will be an array of Project instances with the specified name
})

{% endhighlight %}


{% highlight javascript %}

Project.findAll({order: 'title DESC'})
// yields ORDER BY title DESC

{% endhighlight %}


<br/>


