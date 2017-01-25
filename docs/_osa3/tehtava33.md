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


Sovellus on rakennettu valmiiksi lukuunottamatta tietostoa `models/Opettaja.js`, joka sekin sisältää ratkaisussa hyödynnettävissä olevaa koodia. 



**Palauta** tehtävän ratkaisuna tiedosto `models/Kurssi.js`. Varmista ennen palautusta, että sovellus toimii tehtäväkuvauksen mukaan sovellusta ajamalla. Tehtävänpohja ei sisällä testikoodia. 

### Vihjeitä ja lisätietoja


<https://github.com/dominictarr/level-sublevel/blob/master/README.md>

<https://blog.yld.io/2016/10/24/node-js-databases-an-embedded-database-using-leveldb/#usinglevelsublevel>




