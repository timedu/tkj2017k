---
layout: collection_index
permalink: /:collection/index.html
---

Kurssilla tutustutaan erilaisiin tietokantaratkaisuihin kehittämällä yksinkertaisia tietokantoja käsitteleviä sovelluksia. Kehitysvälineenä on [NetBeans IDE][netbeans][^1], jolla koodin kirjoittamisen ohella syntaksiohjatusti voidaan mm. hallita sovelluksen riippuvuuksia erillisistä moduuleista, ajaa testejä sekä käynnistää ja pysäyttää kehityksen alla oleva sovellus.

[^1]: IDE - Integrated Development Environment 

Laadittavat ohjelmat ovat ns. [Backend][front_and_back_ends] -sovelluksia, joita ajetaan palvelimella selaimen toimiessa niiden käyttöliittymänä. Ohjelmointikielenä on [JavaScript][js]. Koodi tulkitaan [Node:lla][node], joka sisältää myös joukon palvelinpään ohjelmointia tukevia moduuleja. Noden moduulit kuitenkin peittää [Express][express] -sovelluskehys, jonka kyljessä käytetään [Handlebars][handlebars] -sivupohjia. Tätä kalustoa luonnehditaan tuotteiden sivuistoilla seuraavasti:

> [Node.js][node] is a JavaScript runtime built on [Chrome's V8 JavaScript engine][v8]. Node.js uses an event-driven, non-blocking I/O model that makes it lightweight and efficient. Node.js' package ecosystem, [npm][npm-site], is the largest ecosystem of open source libraries in the world.
>
> [Express][express] is a minimal and flexible Node.js web application framework that provides a robust set of features for web and mobile applications.
>
> [Handlebars][handlebars] provides the power necessary to let you build semantic templates effectively with no frustration. Handlebars provides templates look like regular HTML, with embedded handlebars expressions.

Joidenkin tehtävien kohjakoodissa on testejä, joissa käytettyjä välineitä ovat [Mocha][mocha], [Selenium WebDriver][selenium], [PhantomJS][phantom] sekä Noden [Assert-moduuli][assert]. Sovellusten moduuliriippuvuuksien hallinnan taustalla toimii [npm][npm].

[mocha]: https://mochajs.org
[selenium]: http://www.seleniumhq.org/docs/03_webdriver.jsp
[phantom]: http://phantomjs.org
[assert]: https://nodejs.org/dist/latest-v6.x/docs/api/assert.html
[npm]: https://docs.npmjs.com


[netbeans]: http://netbeans.org  
[js]: https://developer.mozilla.org/en-US/docs/Web/JavaScript

[node]: https://nodejs.org 
[v8]: https://developers.google.com/v8/
[npm-site]: https://www.npmjs.com

[express]: http://expressjs.com  
[handlebars]: http://handlebarsjs.com

[front_and_back_ends]: https://en.wikipedia.org/wiki/Front_and_back_ends

### Tehtävät

Tämä osan tehtävien tarkoituksena on tutustuttaa osallistujat kurssilla käytettävään kehitystyön peruskalustoon. Rakennettavat tuotokset ovat hieman eri periaattein toteutettavia "Hello World" -sovelluksia. 

{% include exercises_list.md %}

Tehtävien 1.1 - 1.3 ratkaisut ovat yksisivuisia, jotka tervehdysten tulostamisen ohella laskevat sovelluksen eri polkuihin kohdistuvia pyyntöjä. [Tehtävä 1.1](tehtava11) toteutetaan perus-Nodella. [Tehtävässä 1.2](tehtava12) otetaan käyttöön *Express* -sovelluskehys. Samalla sovelluksen toteuttama laskenta siirretään erilliseen moduuliin. [Tehtävässä 1.3](tehtava13) selaimelle välitettävä tuotos hahmonnetaan *Handlebars* -sivupohjien avulla.

Tehtävissa 1.4 - 1.5 sovelukseen lisätään toinen sivu, joka esittää yhteenvedon eri polkuihin tulleista pyynnöistä. [Tehtävässä 1.4](tehtava14) määritellään pyyntöpolkuja, joihin tuleviin kutsuihin reagoidaan toisistaan poikkeavalla tavalla.  [Tehtävässä 1.5](tehtava15) sama sovellus jäsennetään *MVC*-arkkitehtuurimallin[^2] mukaan.

[^2]: Model - View - Controller

<br />

