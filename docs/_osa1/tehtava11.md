---
layout: exercise_page
title: "Tehtävä 1.1: Hello Node"
exercise_template_name: "W1E01.HelloNode"
exercise_discussion_id: 72799
exercise_upload_id: 300102
---

Tehtäväpohjassa on Node-sovellus[^1] , joka tuottaa selaimen ikkunaan tekstin `Hello World`, kun selain tekee pyynnön osoitteeseen `http://127.0.0.1:3000/`. Tuloste ei ole riippuvainen osoitteessa olevasta polusta. Esim. osoitteeseen `http://127.0.0.1:3000/polku` tehty pyyntö tuo selaimelle aivan saman tekstin.

[^1]: Pohjakoodi on kopioitu osoitteesta <https://nodejs.org/en/about/>. Koodin syntaksin tunnistamminen edellyttäneen Netbeansista vähintään versiota 8.2.

Muokkaa sovellusta siten, että se tuottaa selaimen ikkunaan seuraavanlaisen tekstin[^2]:

[^2]: Tulosteessa sanat on erotettu toisistaan yhdellä välilyönnillä.

~~~
Hello 3 from Node (/abc)
~~~

Edellä oleva tuloste liittyy tilanteeseen, jossa sovelluksen käynnistämisen jälkeen osoiteeseen `http://127.0.0.1:3000/abc` on tehty kolmas pyyntö. Jos tämän jälkeen tehdään ensimmäinen pyyntö osoitteeseen `http://127.0.0.1:3000/def`, tuloste selaimessa on seuraava:

~~~
Hello 1 from Node (/def)
~~~

Palattaessa aiempaan pyyntöön selaimeen saatu teksti on seuraava:

~~~
Hello 4 from Node (/abc)
~~~

**Palauta** tehtävän ratkaisuna tiedosto `main.js`. Varmista ennen palautusta, että tehtäväpohjassa olevat testit menevät läpi.


### Vihjeitä ja lisätietoja

Sovelluksen ajaminen edellyttää, että [Node.js][node] on asennettu kehitysympäristöön. Asennuspaketin eri ympäristöihin voi ladata [Downloads][node-download] -sivulta. Kun asennus on suoritettu, sovelluksia voi ajaa `node`-komennolla, esim. `node main.js`[^2a].

Sovellus voidaan käynnistää myös NetBeans IDE:ssä *Run*-valinnalla, jolloin Netbeansin suorittama käynnistyskomento ilmestyy IDE:n *Output*-ikkunaan. Sovelluksen antama vaste tulee näkyviin, kun selaimessa tehdään pyyntö osoitteeseen `http://127.0.0.1:3000/`.

[node-download]: https://nodejs.org/en/download/

[^2a]: Esimerkki olettaa, että `node` on komntojenhakupolussa ja sitä, että komento annetaan hakemistossa, jossa `main.js` sijaitsee. 


#### Pohjakoodista


Tehtäväpohjan tiedostossa `main.js` on seuraava koodi[^1]:


{% highlight javascript %}

const http = require('http');

const hostname = '127.0.0.1';
const port = 3000;

const server = http.createServer((req, res) => {
  res.statusCode = 200;
  res.setHeader('Content-Type', 'text/plain');
  res.end('Hello World\n');
});

server.listen(port, hostname, () => {
  console.log('Server running at http://${hostname}:${port}/');
});

{% endhighlight %}


Koodi ottaa käyttöön (`require`) [noden][node] [http][http] -moduulin, mudostaa sen avulla web-palvelimen (`http.createServer`) ja käynnistää sitten palvelimen (`server.listen`). 

[node]: https://nodejs.org/en/
[http]: https://nodejs.org/dist/latest-v6.x/docs/api/http.html

Palvelinta muodostettaessa koodi määrittelee funktion, jonka palvelin suorittaa, kun palvelimen kuuntelemaan porttiin tulee pyyntö:


{% highlight javascript %}

const server = http.createServer((req, res) => {
    ...
});

{% endhighlight %}


Funktion määrittelyssä voisi käyttää myös perinteisempää syntaksia:


{% highlight javascript %}

const server = http.createServer( function(req, res) {
    ...
});

{% endhighlight %}


Funktiolla on kaksi parametria: `http.IncomingMessage`-tyyppinen `req` ("request") ja `http.ServerResponse`-tyyppinen `res` ("response"). Tässä `res`-olion avulla muodostetaan pyyntöön annetava vaste - paluukoodi, palautettavan sisällön tyyppi sekä paluuviestin varsinainen sisältö:


{% highlight javascript %}

  res.statusCode = 200;
  res.setHeader('Content-Type', 'text/plain');
  res.end('Hello World\n');

{% endhighlight %}


Paluuviestin sisällön tyypin määrittelyssä voisi olla mukana myös merkistökoodaus[^3]: 

[^3]: Selaimen konsolille saattaa ilmestyä varoitus, jos tieto merkistökoodauksesta puuttuu.


{% highlight javascript %}

  res.setHeader('Content-Type', 'text/plain; charset=utf-8');

{% endhighlight %}

Pohjakoodissa ei hyödynnetä `req` -oliota, mutta se on tarpeellinen tehtävän ratkaisussa. Saattaa olla, että sen `req.url`-attribuutista löytyy juuri paluuviestin osaksi asetettavaa tietoa.  


#### Testeistä

Tehtäväpohjissa voi olla mukana kahdenlaisia testejä: yksikkötestejä ja kokonaisuutta testaavia Selenium -testejä. Testeihin liittyvät valinnat tulevat näkyviin IDE:ssä klikattaessa hiiren oikealla napilla projektia *Projects* -ikkunassa: *Test* (yksikkötestien ajo)  ja *Run Selenium Tests*. 

Testien ajo edellyttää, että kehitysympäristöön on asennettu tarvittavat ohjelmistopaketit: [Mocha][mocha], [Selenium Web-Driver][web-driver] ja [PhantomJS][phantom]. Yksikkötestien ajamiseen riittää *Mocha*, mutta Selenium-testit edellyttävät myös kahta jälkimmäistä, jolloin *Selenium* ohjaa *Phantom*-selainta, joka on yhteydessä kehitettävään sovellukseen. Paketit voi asentaa omaan kehitysympäristöön `npm`-komennolla[^4], joka tulee Node-asennuksen mukana.

[mocha]: https://mochajs.org
[web-driver]: http://www.seleniumhq.org/docs/03_webdriver.jsp
[phantom]: http://phantomjs.org

[^4]: [`npm install mocha`][npm-mocha],  [`npm install selenium-webdriver`][npm-selenium], [`npm install phantomjs-prebuilt`][npm-phantom]

[npm-mocha]: https://www.npmjs.com/package/mocha
[npm-selenium]: https://www.npmjs.com/package/selenium-webdriver
[npm-phantom]: https://www.npmjs.com/package/phantomjs-prebuilt

Jotta testien ajo Netbeansista käsin onnistuu, kehitettävällä projektilla on oltava tieto, minkä tuella projektin testit ajetaan. Tässä sekä yksikkö- että Selenium -testien *Test Provider* on *Mocha*. Määrityksen voi tehdä ao. kohdassa ikkunassa, joka tulee esiin projektivalikon *Properties*-valinnalla.

Selenium -testit edellyttävät, että testattava sovellus on käynnissä, mutta yksikkötestaus onnistuu ilman sovelluksen ajoa. Tässä tehtävässä on mukana sekä yksikkötesti että Selenium -testejä. Tosin yksikkötesti onnistuu aina ilmoittaen, että "ei sisällä yksikkötestejä". Jos Selenium-testi epäonnistuu, saatava virheilmoitus on seuraavanlainen: "TimeoutError: Wait timed out after 500ms". Tämä tarkoittaa sitä, että testi on tehnyt sovellukselle pyynnön, mutta ei ole saanut siltä puolessa sekunnissa odotettua vastetta. Testien ajon tulokset ilmestyvät NetBeansin *Test Results* -ikkunaan.

<br/>


