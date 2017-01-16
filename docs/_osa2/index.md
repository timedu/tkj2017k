---
layout: collection_index
permalink: /:collection/index.html
---

Kurssin tässä osassa siirrytään tietokantakäsittelyyn. Esillä on relaatiotietokannan käyttö sovellusohjelmissa - ensin SQL:n avulla ja sitten ORM-rajapinnan[^1] kautta. ORM:ää käytettäessä tietokannan tiedot näkyvät sovellusohjelmassa tyypillisinä ohjelmointikielen objekteina. 

Relaatiotietokantana tässä on [SQLite][sqlite] ja sen SQL-pohjaisena perusrajapintana [node-sqlite3][node-sqlite3]. ORM:n toteuttaa [Sequelize][sequelize]. Tuotteiden sivustoilla teknologioita luonnehditaan seuraavasti:

[node-sqlite3]: https://github.com/mapbox/node-sqlite3

> [SQLite][sqlite] is an in-process library that implements a self-contained, serverless, zero-configuration, transactional SQL database engine. The code for SQLite is in the public domain and is thus free for use for any purpose, commercial or private. SQLite is the most widely deployed database in the world with more applications than we can count, including several high-profile projects.
> 
> [Sequelize][sequelize] is a promise-based ORM for Node.js and io.js. It supports the dialects PostgreSQL, MySQL, MariaDB, [SQLite][sqlite] and MSSQL and features solid transaction support, relations, read replication and more.

[sqlite]: https://www.sqlite.org
[sequelize]: http://www.sequelizejs.com

[^1]: ORM - Object Reltional Mapping

### Tehtävät

Osaan sisältyy kolme tehtävää, jotka kaikki perustuvat samaan *Kurssit ja opettajat*-tietokantaan

{% include exercises_list.md %}

[Tehtävässä 2.1](tehtava21) laaditaan neljä sivua käsittävä sovellus, jolla voidaan tarkastella tietokannan tietoja sekä luetteloina että erittelyinä. Tietokannan käsittely tapahtuu tässä SQL-rajapinnan kautta. [Tehtävässä 2.2](tehtava22) toteutetaan edellisen kanssa samanlainen sovellus kuitenkin niin, että tietokantarajapintana käytetään ORM:ää. [Tehtävässä 2.3](tehtava23) edellistä ratkaisua täydennetään siten, että kyselyjen lisäksi sovelluksella voi toteuttaa myös muita tietokanta-operaatiota[^2].

[^2]: CRUD - Create, Retrieve, Update, Delete 

<br/>

