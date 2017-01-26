---
layout: collection_index
permalink: /:collection/index.html
---

Kurssin edellisessä osassa on esillä relaatiotietokanta, joissa tiedot jäsentyvät matemaattisten relaatioiden pohjalta sarakkeiksia ja taulukoiksi. Rivien välisiä yhteyksiä muodostetaan perus- ja vierasavainsarakkeiden avulla. Ennen kuin tietokantaa voidaan käyttää, muodostetaan sen skeema so. määritellään ennalta, minkälaista tietoa tietokanta voi sisältää.

Tassä osassa siirrytään tarkastelemaan ns. NoSQL -tietokantoja[^1], joissa tietojen rakenne poikkeaa merkittävästi perinteisestä relaatiomallista. Tyypillisesti tietokannan skeemaakaan ei tarvitse määritellä ennalta ainakaan kokonaisuudessaan. Monet NoSQL -järjestälmät tukevat luonnollisella tavalla myös tietojen hajautusta.


[^1]: Jyväskylän yliopiston *Tietokannat ja tiedonhallinnan perusteet* -kurssin materiaalissa on luku, [Tietokantaparadigmat][itka204-8], joka esittelee lyhyesti eri tyyppisiä NoSQL -tietokantoja. Youtubesta löytyy esim. Martin Fowlerin pitämä esitys [Introduction to NoSQL][youtube-fowler].

[itka204-8]: https://tim.jyu.fi/view/kurssit/tktl/itka204/kurssimoniste#tietokantaparadigmat
[youtube-fowler]: https://www.youtube.com/watch?v=qI_g07C_Q5I


Nyt NoSQL -tietokannoista esillä on *avain-arvoparitietokanta*, jossa nimensä mukaisesti tiedot jäsentyvät siten kuin tietokantatyypin nimitys antaa olettaa. Yksittäistä avain-arvoparia relaatiotietokannassa voi vastata esim. taulukon yksi rivi tai vaikka kokonainen taulukko. Tietokannan hallintajärjestelmä ei tunne arvojen rakennetta vaan tulkinnan tekee tietoja käyttävä sovellus. Avain-arvoparitietokannoista esimerkkinä on [LevelDB][LevelDB], josta kerrotaan seuraavaa:

> [LevelDB][LevelDB] is a simple key/value data store built by Google, inspired by [BigTable][BigTable]. It's used in Google Chrome and many other products. LevelDB supports arbitrary byte arrays as both keys and values, singular get, put and delete operations, batched put and delete, bi-directional iterators and simple compression using the very fast Snappy algorithm.
> 
> [LevelUP][LevelUP] aims to expose the features of LevelDB in a Node.js-friendly way. All standard Buffer encoding types are supported, as is a special JSON encoding. LevelDB's iterators are exposed as a Node.js-style readable stream.

[LevelDB]: http://leveldb.org 
[LevelUP]: https://github.com/Level/levelup/blob/master/README.md
[BigTable]: https://research.google.com/archive/bigtable.html


Edellisessä osassa esillä olleen *SQLite:n* tapaan *LevelDB*-tietokanta voidaan upottaa sovellukseen siten, että erillillä tietokannan hallintajärjestelmää ei tarvita. 

{% comment %}

[Redis][redis]

[redis]: https://redis.io

<https://firebase.google.com>

{% endcomment %}

### Tehtävät

Tehtävien taustalla on [edellisen osan](../osa2) tehtävistä tuttu *Kurssit ja opettajat* -tietokanta, mikä nyt on jäsennetty eri tavoin avain-arvoparitietokannaksi. 

{% include exercises_list.md %}

[Tehtävässä 3.1](tehtava31) on kaksi erillistä tietokantaa, toinen kurseille ja toinen opettajille. [Tehtävässä 3.2](tehtava32) tiedot ovat yhdessä tietokannassa, joka sisältää avain-arvoparin kullelkin kurssille ja opettajalle sekä näiden lisäksi myös kurssi- ja opettajaluetteloille. [Tehtävässä 3.3](tehtava33) otetaan käyttöön Noden LevelDB -toteutuksen laajennus, jossa tietokannan sisältämiä avain-arvopareja voidaan ryhmitellä alirakenteiksi.


<br/>