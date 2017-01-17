---
layout: exercise_page
title: "Tehtävä 2.2: Kurssit ja opettajat, ORM"
exercise_template_name: W2E02.KurssitJaOpettajatORM
exercise_discussion_id: 
exercise_upload_id: 
julkaisu: 17.1.2017
kesken: 1
---

Laadi relaatiotietokantaa käsittelevä sovellus, joka käyttäytyy kuten [Tehtävän 2.1](../tehtava21) ratkaisu, mutta toteuttaa tietokanakäsittelyn [Sequelize][sequelize] -ORM:n avulla.

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

Tietokanta sijaitsee projektin `database`-kansiossa nimellä `koulu.sqlite`[^1]. Tietokanta datoineen muodostuu sovelluksen käynnistyksen yhteydessä, jos kansiosta ei löydy em. nimistä tiedostoa.

[^1]: `databases`-kansio näkyy NetBeansin *Files*-ikkunassa, mutta ei *Projects*-ikkunassa

**Palauta** tehtävän ratkaisuna tiedostot `kurssiController.js` ja `opettajaController.js`. Varmista ennen palautusta, että tehtäväpohjassa olevat Selenium-testit menevät läpi. Sovelluksen on oltava käynnissä testejä ajettaessa.

### Vihjeitä ja lisätietoja

...


{% highlight javascript %}

    var sequelize = new Sequelize('koulu', '', '', {
        dialect: 'sqlite',
        storage: DatabaseFile,
        define: {
            timestamps: false
        }
    });

{% endhighlight %}

<small>Listaus 1.</small>



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

<small>Listaus 2.</small>



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

<small>Listaus 3.</small>


{% highlight javascript %}

    Kurssi.belongsTo(Opettaja, {foreignKey: 'opettaja_id', as: 'Opettaja'});
    Opettaja.hasMany(Kurssi, {foreignKey: 'opettaja_id', as: 'Kurssit'});

{% endhighlight %}

<small>Listaus 4.</small>



<br/>


