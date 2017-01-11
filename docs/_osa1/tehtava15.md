---
layout: exercise_page
title: "Tehtävä 1.5: Hello MVC"
exercise_template_name: W1E05.HelloMVC
exercise_discussion_id: 72804
exercise_upload_id: 300106
---

Laadi sovellus, joka toimiin ulospäin samoin[^1] [edellisen tehtävän](../tehtava14) ratkaisun kanssa. Sovellusten lähdekoodien jäsennykset kuitenkin poikkeavat toisistaan. Tässä tehtävässä sovellus rakentuu erään tyyppisen MVC-mallin mukaan (Model-View-Controller):

[^1]: Erona on kuitenkin se, että tässä Hello-sivulle tulostuu teksti *MVC* tekstin *Routing* sijaan.

~~~
                (request)  
                    ▾
[ Model ] <-- [ Controller ] --> [ View ]
~~~

*Kontrolleri* ottaa vastaan pyynnön, käsittelee[^2] pyyntöön liittyvän datan *mallin* avulla ja muodostaa sitten vasteen pyyntöön *näkymän* tuella.

[^2]: Ylläpitää (lisäys, muutos, poisto) ja/tai lukee

Edellisen tehtävän lähdekoodi jäsentyi seuraavasti:

~~~
Sources
 ├── counter.js
 ├── main.js
 └── views
     ├── hello.hbs
     ├── summary.hbs
     └── layouts
         └── main.hbs        
~~~

Tämän tehtävän ratkaisussa jäsennys on seuraavanlainen:

~~~
Sources
 ├── main.js
 ├── configs
 │     └── config.js 
 ├── controllers
 │     └── requestController.js 
 ├── models
 │     └── requestCounter.js 
 └── views
     ├── hello.hbs
     ├── summary.hbs
     └── layouts
         └── main.hbs        
~~~

Ratkaisun päämoduuli `main.js` on suhteellisen pelkistetty:

{% highlight javascript %}

const Controllers = './controllers/requestController';
const Configs = './configs/config';

const app = require('express')();
require(Controllers)(app);
require(Configs)(app).run();

{% endhighlight %}

Päämoduulin lisäksi tehtäväpohjassa on valmiina mukana layout `main.hbs`. Muiden tiedostojen osalta pohjassa on mukana rungot, joihin sisällön voi kopioida edellisen tehtävän ratkaisusta. Tiedostot `requestCounter.js`[^3], `hello.hbs`[^4] ja `summary.hbs`[^5] ovat käypiä kutakuinkin sellaisenaan. `requestController.js` sisältää reitytykseen liittyvät kaksi `get`-metodin kutsua.  `config.js` ottaa käyttöön *handlebars*-paketin ja suorittaa siihen liittyvät asetukset sekä palauttaa `run`-metodin sisältämän olion. Moduuleihin `requestController.js` ja `config.js` tarvittava koodi löytyy edellisen tehtävän ratkaisusi päämoduulista `main.js`.


[^3]: Edellisessä tehtävässä nimellä `counter.js`
[^4]: Tietojen välitys näkymälle `hello.hbs` muuttujissa `url` ja `count`
[^5]: Tietojen välitys näkymälle `summary.hbs` muuttujassa `requests`

**Palauta** tehtävän ratkaisuna tiedostot `config.js` ja `requestController.js`. Varmista ennen palautusta, että tehtäväpohjassa olevat testit menevät läpi. Selenium-testit testaavat sovellusta kokonaisuutena. Yksikkötestaus testaa *requestCounter*-moduulia, vaikka se ei sisällykään palautettavaan aineistoon. Selenium-testien osalta on huomioitava, että **sovellus on käynnistettävä uudelleen** ennen testien ajoa.

<br/>

