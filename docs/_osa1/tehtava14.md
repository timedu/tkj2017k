---
layout: exercise_page
title: "Tehtävä 1.4: Hello Routing"
exercise_template_name: W1E04.HelloRouting
exercise_discussion_id: 72803
exercise_upload_id: 300105
---

Muokkaa [edellisen tehtävän](../tehtava13) ratkaisuasi siten, että tehtäessä pyyntö osoitteen juureen (`http://127.0.0.1:3000/`) sovellus esittää yhteenvedon eri polkuihin tehdyistä pyynnöistä. Kun pyyntö kohdistuu polkuun, vaste on samankaltainen kuin edellisissä tehtävissä, esim.

~~~
Hello 3 from Routing (/abc)
~~~

Pyyntöjen yhteenveto näkyy selaimessa seuraavasti:

~~~
Request Summary

/abc: 3
/def: 2
~~~

Vaste on html-muodossa seuraavanlainen:

{% highlight  html %}

<p>Request Summary</p>
<div>
  <span>/abc: 3</span><br/>
  <span>/def: 2</span><br/>
</div>

{% endhighlight %}

Edellä esitetty liittyy siis tilanteeseen, jossa on tehty ennen yhteenvetopyyntoä kolme pyyntöä osoitteeseen `http://127.0.0.1:3000/abc` ja kaksi pyyntöä osoitteeseen `http://127.0.0.1:3000/def`. 

Sovelluksen lähdekoodi jäsentyy projektissa seuraavasti:

~~~
├── counter.js
├── main.js
└── views
    ├── hello.hbs
    ├── summary.hbs
    └── layouts
        └── main.hbs        
~~~

`counter.js` ja `views/layouts/main.hbs` ovat projektipohjassa valmiina. Tiedoston `views/hello.hbs` voi kopioda sellaisenaan [edellisen tehtävän](../tehtava13) ratkaisusta. Tiedostoista `main.js` ja `views/summary.hbs` pohja sisältää suhteellisen pitkälle viedyt rungot. 

Pohjassa oleva *counter* -moduuli tarjoaa seuraavat kolme metodia.

* `addRequest`: lisää polun pyyntöjen määrää yhdellä
* `getRequests`: palauttaa polun pyyntöjen määrän
* `getRequestsAll`: palauttaa objektin, joka sisältää kaikkien polkujen pyyntöjen määrät

**Palauta** tehtävän ratkaisuna tiedostot `main.js` ja `summary.hbs`. Varmista ennen palautusta, että tehtäväpohjassa olevat testit menevät läpi. Selenium-testit testaavat sovellusta kokonaisuutena. Yksikkötestaus testaa counter-moduulia, vaikka se ei sisällykään palautettavaan aineistoon. Selenium-testien osalta on huomioitava, että **sovellus on käynnistettävä uudelleen** ennen testien ajoa.

### Vihjeitä ja lisätietoja

Toteuta reititys kahdella [Express][express] -kehyksen `get`-metodin kutsulla. Sivustolla on tietoa reitityksestä mm. sivuilla [Basic routing][basic-routing] ja  [Routing][routing].

[express]: http://expressjs.com
[basic-routing]: http://expressjs.com/en/starter/basic-routing.html
[routing]: http://expressjs.com/en/guide/routing.html

[Handlebars][handlebars] -sivupohjat voivat sisältää html-koodin ja muuttujien lisäksi ns. helppereitä, joiden avulla on toteutettu paketissa valmiina olevat ohjausrakenteet. Tehtävän ratkaisua tässä tukenee [each][each-helper] -helpperi.

[handlebars]: http://handlebarsjs.com
[each-helper]: http://handlebarsjs.com/builtin_helpers.html#iteration



