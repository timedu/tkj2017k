---
layout: exercise_page
title: "Tehtävä 3.3: Kurssit ja opettajat, Sub-levels"
exercise_template_name: W3E03.KurssitJaOpettajatSubLevels
exercise_discussion_id: 
exercise_upload_id: 
kesken: 1
julkaisu: 26.1.2017
---

Laadi  ulkoisilta ominaisuksiltaan [tehtävän 3.2](../tehtava32) ratkaisua vastaava sovellus, joka taustalla on edelliseen tehtävään verrattuna hieman erirakenteinen tietokanta. Tässä tukeudutaan Noden LevelDB rajapinnan laajennukseen, jolla tietokannan sisältö voidaan jäsentää jossain määrin relaatiotietokannan tauluja muistuttaviin osiin. Samalla hyödynnetään erillistä node-pakettia, jolla voidaan muodostaa tietokannan tiedoille yksilöllisiä tunnisteita.

Sovellukseen liittyy loki, jota varten lähdekoodi sisältää oman näkymän, `views/del_loki.` Muuten tiedostokokonaisuus vastaa  [edellistä tehtävää](../tehtava32).

*Kontrollerit* käyttävät tietokantaa *malleihin* (*Kuva 1*) paketoitujen metodien kautta. 
 
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
  | delLoki       |
  +---------------+
~~~
<small>Kuva 1. Sovelluksen mallit</small>


Sovellus on rakennettu valmiiksi lukuun ottamatta tietostoa `models/Opettaja.js`, joka sekin sisältää ratkaisussa hyödynnettävissä olevaa koodia. 

Tehtävässä tietokanta jäsentyy seuraavasti:

~~~
  +---------------+  
  |     base      |  
  +-------+-------+  
          |
          +------------------------------------------+
          |                                          |
  +-------+-------+                          +-------+-------+
  |    kurssit    |                          |    opettajat  |
  +-------+-------+                          +-------+-------+
          |                                          |
          +---------------------+                    |
          |                     |                    |
  +-------+-------+     +-------+-------+    +-------+-------+
  |<opettaja-cuid>| ... |<opettaja-cuid>|    |    del_loki   |
  +---------------+     +---------------+    +---------------+
~~~
<small>Kuva 2. Tietokannan rakenne</small>

`base` on tietokannan päätaso, jolla on kaksi alitasoa, `kurssit` ja `opettajat`, sisältäen nimitystensä mukaiset tiedot (*avain-arvoparit*). Avaimet muodostetaan tässä [`cuid`][cuid]-funktiolla, joka palauttaa yksilöllisiä tunnisteita. Arvot ovat json-muodossa olevia merkkijonoja. Osana yksittäisen kurssin *arvoa* on `opettaja`-attribuutti, joka sisältää kurssin opettajan *avaimen*.

[cuid]: https://github.com/ericelliott/cuid/blob/master/README.markdown#cuid

*Kuvassa 2* `kurssit`-tason alapuolella on otsikolla `<opettaja-cuid>` esitettyjä elementtejä, missä otsikolla viitataan opettajan *avaimeen*. Näissä elementeissä (*alitasoissa*) kurssin tiedot on talletettu opettajittain. `opettajat`-tason alapuolella oleva `del_loki` sisältää tietokannasta poistettujen opettajien *avaimet* erillistä "roskien keruuta"[^1] varten. 

[^1]: Ei toteuteta tähän ratkaisuun

**Palauta** tehtävän ratkaisuna tiedosto `models/Kurssi.js`. Varmista ennen palautusta, että sovellus toimii tehtäväkuvauksen mukaan sovellusta ajamalla. Tehtävänpohja ei sisällä testikoodia. 

### Vihjeitä ja lisätietoja


<https://github.com/dominictarr/level-sublevel/blob/master/README.md>

<https://blog.yld.io/2016/10/24/node-js-databases-an-embedded-database-using-leveldb/#usinglevelsublevel>

<br/>


