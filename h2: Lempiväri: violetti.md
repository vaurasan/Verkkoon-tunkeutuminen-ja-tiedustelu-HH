# h2: Lempiväri: violetti

#### Oma host kokoonpanoni:

| Komponentti | Kuvaus | Lisätiedot |
| :---        |    :----:   |          ---: |
| Emolevy | MSI B550-A PRO | ATX, AM4 |
| Prosessori   | AMD Ryzen 9 5900X | 12-Core 3.70 GHz |
| RAM   | G.Skill  Ripjaws V |  32GB (4x8GB) DDR4 3200MHz  |
| Näytönohjain   | Sapphire PULSE AMD Radeon RX 7900 GRE        | 16GB     |
| Kovalevy   | Kingston 1TB        | A2000 NVMe PCIe SSD M.2      |
| Kovalevy   | Crucial 512GB        | MX100 SSD     |
| Kovalevy   | Crucial 256GB        | MX100 SSD     |
| Virtalähde   | Asus 750W TUF       | ATX 80 Plus      |
| Kotelo   | Phanteks Enthoo Pro       |  Full Tower      |

Käyttöjärjestelmä: Windows 11 Pro 25H2

Oracle VirtualBox 7.1.12

kali-linux-2025.3-virtualbox-amd64

### [https://terokarvinen.com/verkkoon-tunkeutuminen-ja-tiedustelu/#h2-lempivari-violetti](https://terokarvinen.com/verkkoon-tunkeutuminen-ja-tiedustelu/#h2-lempivari-violetti)

## x) Lue ja vastaa lyhyesti kysymyksiin. Tässä alakohdassa x ei tällä kertaa tarvitse lukea artikkeleita kokonaan, ei tarvitse tiivistää niitä, eikä tehdä testejä koneella.

### - Selitä tuskan pyramidin idea 1-2 virkkeellä. Bianco 2013: [Pyramid of Pain](https://detect-respond.blogspot.com/2013/03/the-pyramid-of-pain.html). (Katso eritoten pyramidin kuvaa.)

Tuskan pyramidi kertoo visuaalisesti:

- Hyökkääjän toiminnan havaitsemisessa käytettävien ilmaisimien välisestä suhteesta.
- Kuinka paljon mikäkin vastatoimi aiheuttaa hyökkääjälle päänvaivaa.

### - Selitä timanttimallin (Diamond Model) idea 1-2 virkkeellä. Tekijä esittelee sen aika juhlallisesti, voit myös etsiä yksinkertaisempia artikkeleita [hakukoneella](https://duckduckgo.com/?t=ftsa&q=diamond+model+attacker+capability+infrastructure&ia=web) tai kelata suoraan timantin kuvaan. Caltagirone et al 2013: [Diamond Model](https://www.threatintel.academy/wp-content/uploads/2020/07/diamond-model.pdf)

Tässä [webasha.com](https://www.webasha.com/blog/what-is-the-diamond-model-in-cybersecurity-a-beginner-friendly-guide-with-real-world-examples-and-analysis#sec4) sivulla oli aika hyvin selitetty asiat selkokielellä.

- Kuinka **Adversary**, eli hyökkääjä käyttää **Capability**ä, eli tapaa tai työkalua puolustajan infrastruktuuria vastaan.
- Vastapuolella **Infrastructure**, eli puolustajan käytössä olevat järjestelmät ja työkalut, sekä **Victim**, esimerkiksi: yritys, käyttäjä, palvelin, tai laite, joka on hyökkäyksen kohteena.

## a) Apache log. Asenna Apache-weppipalvelin paikalliselle virtuaalikoneellesi. Surffaa palvelimellesi salaamattomalla HTTP-yhteydellä, http://localhost . Etsi omaa sivulataustasi vastaava lokirivi. Analysoi yksi tällainen lokirivi, eli selitä sen kaikki kohdat. (Jos Apache ei ole kovin tuttu, voit tätä tehtävää varten vain asentaa sen ja testata oletusweppisivulla. Eli ei tarvitse tehdä omia kotisvuja tms.)

Vanhasta muistista ja osin tämän tehtävän Karvisen (2025) ohjeesta asensin apache2 web-palvelimen ja käynnistin sen, jonka jälkeen menin seuraamaan palvelimen lokia ja navigoin localhost osoitteeseen selaimella.

```bash
sudo apt-get update
sudo apt-get install apache2
sudo systemctl start apache2
/var/log/apache2/
pwd
sudo tail -F /var/log/apache2/access.log
```
Lokin lukemiseen löysin hyvän oloisen lähteen [Sumo logic](https://www.sumologic.com/blog/apache-access-log), sekä tietenkin [Apachen oma](https://httpd.apache.org/docs/2.4/logs.html) lokien ohje.

![201](/kuvat/201.png)

- **127.0.0.1** = clientin IP osoite, joka on tehnyt pyynnön palvelimelle.
- **-** = ensimmäinen viiva on epäluotettava clientin määrittämä RFC 1413 identiteetti, johon ei tulisi luottaa muuta kuin tarkkaan hallituissa verkoissa. Apache ei yritä määrittää tätä, ellei *IdentityCheck* ole asetettu **On**-tilaan.
- **-** = toisen viivan tilalla olisi käyttäjän nimi, jos dokumentti olisi salasalasuojattu. Tässä tapauksessa pelkkä viiva riittää.
- Päivämäärä ja aika, jolloin pyyntö on vastaanotettu.
- **"GET / HTTP/1.1"** = pyynnön tyyppi ja mahdollisesti mihin resurssiin pyyntö kohdistuu.
- **200** = status koodi, jonka palvelin palauttaa clientille. 2:lla alkava koodi = onnistunut. 3:lla alkava = uudelleenohjaus. 4:llä alkava = clientin aiheuttama virhe. 5:llä alkava = palvelimesta johtuva virhe.
- **3383** = palautetun objektin koko ilman headereita. Jos mitään ei palautettu clientille, tässä on "-".
- **"-"** = tämä viiva jäi hieman mysteeriksi. Sumo logic:n ohjeessa mainittiin, että tässä kohdassa voisi lukea source URL, eli tässä tapauksessa **"http://localhost/"**.
- Viimeisenä User Agent, joka kertoo clientin selaimen yksityiskohtia. 

## b) Nmapped. Porttiskannaa oma weppipalvelimesi käyttäen localhost-osoitetta ja 'nmap -A' päällä. Selitä tulokset. (Pelkkä http-portti 80/tcp riittää)

Irroitan varmuuden vuoksi viruaalisen kaaplelin virtuaalikoneesta tehtävän ajaksi. **Network settings** -> **Cable connected**-kohdasta täppä pois.

Nmap:n käyttö on minulle uutta, joten otan [nmap.org](https://nmap.org/book/man-port-scanning-basics.html):n port scanning basics ohjeet käyttöön. Ohjelman sisäisessä ohjeessa tosin kerrotaan jo, että "-A" = Enable OS detection.

Kokeilen skannata localhost osoitetta:
```bash
nmap -v -A 127.0.0.1 #Ohjeessa oli "-v" = "increase verbosity level"
```

![202](/kuvat/202.png)

Tulkitsen tuloksia tätä hyödyntäen: [https://www.geeksforgeeks.org/ethical-hacking/nmap-scanning-results/](https://www.geeksforgeeks.org/ethical-hacking/nmap-scanning-results/). Ei ehkä ihan täydellinen ohje, neuvotaan filteröimään portit 1-1000 komennolla "sudo nmap -sS -Pn 1-1000 10.143.85.1", mutta kuvassa näkyy, "Failed to resolve "1-1000". Nmap.org:sta löytyy yksityiskohtaisempia tietoja [https://nmap.org/book/osdetect-usage.html](https://nmap.org/book/osdetect-usage.html).

- Aloitusaika.
- 157 scriptiä käytössä.
- 1000 porttia SYN stealth skannattu
- Avoin portti löytynyt 80/TCP, palvelu: http, Apache httpd 2.4.65 Debianille.
- Tuetut http metodit: HEAD, GET, POST, OPTIONS.
- Linux kernel 5.0-6.2 löytynyt
- **Network Distance: 0 hops**: OS detection päällä näemme kaikki matkalla olevat reitittimet. Localhostia skannatessa 0.
- **TCP Sequence prediction**: Difficulty 259 = TCP spoofing hyökkäyksen vaikeusaste. Ilmeisen vaikea, koska: (Good luck!).
- **IP ID sequence generation**: All zeros: = Tämä näytetään ainoastaan *verbose*-tilassa. Kertoo ID-kentän generointitavan, jota voisi hyödyntää joissain hyökkäyksissä.

##


##




##


### Lähteet

https://terokarvinen.com/verkkoon-tunkeutuminen-ja-tiedustelu/#h2-lempivari-violetti

https://detect-respond.blogspot.com/2013/03/the-pyramid-of-pain.html

https://duckduckgo.com/?t=ftsa&q=diamond+model+attacker+capability+infrastructure&ia=web

https://www.threatintel.academy/wp-content/uploads/2020/07/diamond-model.pdf

https://www.webasha.com/blog/what-is-the-diamond-model-in-cybersecurity-a-beginner-friendly-guide-with-real-world-examples-and-analysis#sec4

https://www.sumologic.com/blog/apache-access-log

https://httpd.apache.org/docs/2.4/logs.html

https://www.geeksforgeeks.org/ethical-hacking/nmap-scanning-results/

https://nmap.org/book/osdetect-usage.html

---

Tätä dokumenttia saa kopioida ja muokata GNU General Public License (versio 2 tai uudempi) mukaisesti. [http://www.gnu.org/licenses/gpl.html](http://www.gnu.org/licenses/gpl.html)

Pohjana Tero Karvinen & Lari-Iso Anttila 2025: [Verkkoon tunkeutuminen ja tiedustelu](https://terokarvinen.com/verkkoon-tunkeutuminen-ja-tiedustelu/)

Kirjoittanut: <em>Santeri Vauramo</em> 2025
