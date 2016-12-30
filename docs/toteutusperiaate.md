---
layout: site_page
permalink: /toteutus/index.html 
---


## Toteutus- ja suoritusperiaate

#### Opiskeluperiaate

Kurssilla oppiminen tapahtuu ongelmakeskeisesti itseopiskeluna ohjauksen tukemana (OIO).  *Ongelmakeskeisyydellä* tässä tarkoitetaan sitä, että tehtävät määrittelevät opiskeltavan aineksen. Tehtävät ratkaistaan ja niiden edellyttämät taustatiedot opiskellaan *itsenäisesti*. Kurssille on määritelty taustatiedot tarjoava kirjallinen materiaali, joka *ohjaa* tehtävien ratkaisussa. Kuhunkin teemaan liittyy johdantoteksti, jossa on viitteitä taustamateriaaliin. Jokaisen tehtävän yhteydessä on viiteet taustamateriaalin vastaaviin esimerkkeihin ja mahdollisesti myös ongelman ratkaisua tukevaan käsikirja-aineistoon. Tehtäviin saa tukea kontaktitunneilla ja Moodlen välityksellä.

#### Kurssiin liittyvät sivustot

Kurssimateriaali on kokonaisuudessaan webissä. Tehtäväkuvaukset ja niihin liittyvät johdanto-osat löytyvät [tältä sivustolta]({{site.baseurl}}/). Opintojakson toteutuksen runko on [TTY:n Moodlessa](https://moodle2.tut.fi/course/view.php?id=9776), jonne mm. palautetaan tehtävien ratkaisut, ja jonka työkaluilla on toteutettu kurssin [uutiskanava][uutiset] ja [keskustelu-foorumi][keskustelu]. Moodle-sivustolta on linkkejä tälle sivustolle ja päinvastoin.

[uutiset]: https://moodle2.tut.fi/mod/forum/view.php?id=273906 "Uutiset"
[keskustelu]: https://moodle2.tut.fi/mod/forum/view.php?id=281885 "Keskustelu"

Moodlessa on esillä myös kurssin aikataulutus: milloin kunkin osan materiaali on käytettävissä, milloin teemaa käsitellään kontaktitunneilla, ja milloin tehtävien ratkaisut tulee viimeistään palauttaa.

Sivustoilta on useita viittauksia Helsingin yliopiston [Web-selainohjelmointi -kurssimateriaalin]({{site.baseurl}}/weso/)[^1] ao. kohtiin. Ensijaisena käsikirjastona toimii [Mozillan kehittäjäsivusto](https://developer.mozilla.org/fi/). [Lukemistoja]({{site.baseurl}}/lukemistoja/)-sivulta löytyy linkkejä myös muuhun kurssin aihepiiriä käsittelevään materiaaliin. 

[^1]: Aineistosta on käytössä kopio, joka ei alkuperäisestä poiketen kysy TMC-tunnusta sivun latauksen yhteydessä. TMC (Test My Code) on Helsingin yliopiston ohjelmointikursseilla käytössä oleva testauspalvelin. 

#### Tehtävät

Kurssiin sisältyy sen teemojen mukaiset seitsemän tehtäväsarjaa, jotka ovat  arvosanan määräytymisen näkökulmasta keskenään samanarvoisia, vaikka tehtävien lukumäärä ja sarjan vaatima työmäärä saattaa vaihdella. Tehtävät ovat yksilötöitä.

Tehtäväkuvaukset löytyvät tältä sivustolta varustettuna linkeillä teoriaan ja vastaaviin esimerkkeihin. Tehtävät ratkaistaan tyypillisesti täydentämällä sovellusrunkoa, joka sisältää usein myös testikoodin. Ratkaisuna laadittava tiedosto palautetaan Moodleen. Tehtäväkuvauksen yhteydessä on linkit sovellusrunkoon sekä Moodlen palautusluukkuun. 

Tehtävien palautuksissa käytetään Moodlen VPL -työkalua (Virtual Programming Lab), jonka kautta koodia voi myös editoida. Tarkoitus kuitenkin on, että ohjelmakoodi palautetaan työkalun kautta luettavissa olevana tiedostona. VPL osaa myös tunnistaa plagiaatit.

Sovellusrungot ovat [NetBeans][netbeans] -projekteja, mutta tehtävien ratkaisu ei välttämättä edellytä NetBeansin käyttöä. Palautettava aineisto on tyypillisesti yksittäinen tekstitiedosto.

Esimerkkinä seuraavassa on kurssin ensimmäisen tehtävän sovellusrungon rakenne[^2]:

~~~
[W1E01.PuhelinmuistioA]
   [nbproject  ]
   [public_html]
      [img ]
      [js  ]
         app.js
         puhelinmuistio.js
      [spec]
      SpecRunner.html
      index.html    
~~~

Kaaviossa hakasuluissa olevat tekstit viittaavat hakemistoihin. Esimerkissä palautettava koodi laaditaan tiedostoon `puhelinmuistio.js`. Rungossa on mukana ratkaisua tukemassa palautettavaa koodia hyödyntävä käyttöliittymä, `index.html` ja `app.js`. Muu `public_html`-hakemistossa oleva aineisto liittyy ratkaisun testaamiseen. Testitapaukset sijaitsevat `spec`-hakemistossa ja niitä voi ajaa selaismessa tiedostolla `SpecRunner.html`. Jos ratkaisussa ei käytä NetBeansia, sen käyttämä, rungossa mukana oleva hakemisto, `nbproject` on tarpeeton.

[^2]: Runko on esitetty siten kuin se näkyy NetBeansin *Files* -ikkunassa. *Projects* -ikkunassa näkymä on hieman toisenlainen.

#### Kontaktitunnit

Kontaktitunneilla (8x3h) tapahtuu teemaan ja tehtäviin johdattelua läsnä olevien opiskelijoiden esittämisen toiveiden ohjaamana. Tuntien aikana saa myös tukea tehtävien ratkaisuille sekä opettajalta että useimmiten myös toisilta opiskelijoilta. Kurssi on aikataulutettu siten, että kukin teema on esillä kahdella kontaktiopetuskerralla ja niin, että kullakin kontaktikerralla on esillä kaksi eri teemaa (lukuun ottamatta ensimmäistä ja viimeistä kertaa, jotka keskittyvät ainoastaan yhteen teemaan). Kontaktitunneille ei ole velvoitetta osallistua. Tukea saa myös esim. Moodle-keskustelun kautta. 

#### Tentit

Kurssiin kuuluu perustentti ja kaksi rästitenttiä. Tentit suoritetaan lähtökohtaisesti atk-luokassa tenttiä varten rakennetuissa  virtuaalikoneissa, joihin on asennettu kehitysympäristö ja tukidokumentaatiota. Tenttien osalta siirrytään mahdollisuuksien mukaan EXAM-järjestelmän käyttöön. Internet-verkon palvelut eivät ole käytettävissä tenttien yhteydessä.

#### Arvostelu

Arvosana perustuu suurelta osin ratkaistujen tehtävien määrään. Tentin avulla lähinnä varmistetaan opiskelijan oma panos tehtävien laatimisessa. 

_Suorituskriteerit_

- vähintään n. 50% tehtävistä ja
- vähintään n. 50% suoritus tentistä

_Arvosana (0-5)_

- n. 50 % tehtävistä: 1
- n. 90 % tehtävistä: 4
- n. 90 % suoritus tentistä: +1


#### Kehitysvälineistö

Tehtävien laatimisen tukena käytetään [NetBeans][netbeans]-kehitysympäristöä, jonka voi ladata veloituksetta tuotteen sivustolta. Tämän kurssin tarpeisiin riittää "HTML5/JavaScript" -paketti, jossa on mukana kevyt kehitystyön tarpeisiin riittävä web-palvelin. Tehtävät voi kyllä ratkaista muullakin kalustolla. NetBeans (tai jokin muu vastaava IDE) kuitenkin ohjaa ratkaisun laatimista esim. ilmoittamalla syntaksivirheistä ja listaamalla vaihtoehtoja olion metodikutsuja kirjoitettaessa.

Sovellusten käyttöliittymänä toimii [Firefox][firefox] -selain, jonka perusasennuksessa (ilman liitännäisiä) on mukana erilaisia sovelluskehittäjän apuvälineitä (esim. [web-konsoli][console] ja [debuggeri][debugger]). Muitakin selaimia voi käyttää, mutta palautettujen ratkaisujen läpikäynti tapahtuu Firefoxilla.

NetBeans ja Firefox löytyvät myös luokkakoneista.

[netbeans]: http://netbeans.org
[firefox]: https://www.mozilla.org/fi/firefox/new/
[console]: https://developer.mozilla.org/en-US/docs/Tools/Web_Console "Web Console" 
[debugger]: https://developer.mozilla.org/en-US/docs/Tools/Debugger "Debugger"

