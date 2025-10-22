# h1 Sniff

#### Oma Host kokoonpanoni:

| Komponentti | Kuvaus | Lisätiedot |
| :---        |    :----:   |          ---: |
| Emolevy | MSI B550-A PRO | ATX, AM4 |
| Prosessori   | AMD Ryzen 9 5900X | 12-Core 3.70 GHz |
| RAM   | G.Skill  Ripjaws V |  32GB (4x8GB) DDR4 3200MHz  |
| Näytönohjain   | Sapphire PULSE AMD Radeon RX 7900 GRE        | 16GB     |
| Kovalevy   | Kingston 1TB        | A2000 NVMe PCIe SSD M.2      |
| Kovalevy   | Crucial 512GB        | MX100 SSD     |
| Kovalevy   | Crucial 256GB        | MX100 SSD     |
| Virtalähde   | Asus 750W TUF Gaming Gold        | ATX 80 Plus      |
| Kotelo   | Phanteks Enthoo Pro       |  Full Tower      |

Käyttöjärjestelmä: Windows 11 Pro 25H2

Oracle VirtualBox 7.1.12

kali-linux-2025.3-virtualbox-amd64

## x) Lue ja tiivistä. (Tässä x-alakohdassa ei tarvitse tehdä testejä tietokoneella, vain lukeminen tai kuunteleminen ja tiivistelmä riittää. Tiivistämiseen riittää muutama ranskalainen viiva.)

[Karvinen 2025: Wireshark - Getting Started](https://terokarvinen.com/wireshark-getting-started/)

- Wiresharkin asentaminen:
```bash
sudo apt-get install wireshark
```
- Non-superkäyttäjille annetaan mahdollisuus kaapata paketteja "Yes", jotta ei tarvitse käyttää isoilla käyttöoikeuksilla.
- Lisätään oma käyttäjä Wiresharkin sallimiin käyttäjiin:
```bash
sudo adduser tero wireshark #<-- korvaa "tero" omalla käyttäjätunnuksella
```
- Tämän jälkeen uudelleenkirjautuminen.
- Komennolla "wireshark" käynnistetään Wiresharkin graafinen käyttöliittymä.
- Aluksi valitaan verkkointerface, jota tutkitaan. Voi myös valita "any", jolloin kaikkien interfacejen liikenne kaapataan.
- Hain evä symbolilla käynnistetään ja pysäytetään verkkoliikenteen kaappaus.
- Kaappauksia voidaan tallentaa .pcap tiedostomuotoon, ja avata myöhempää tarkastelua varten.
- 

[Karvinen 2025: Network Interface Names on Linux](https://terokarvinen.com/network-interface-linux/)




## Otsikko




## Otsikko




##




##


### Lähteet

https://terokarvinen.com/wireshark-getting-started/

https://terokarvinen.com/network-interface-linux/

---

Tätä dokumenttia saa kopioida ja muokata GNU General Public License (versio 2 tai uudempi) mukaisesti. http://www.gnu.org/licenses/gpl.html

Pohjana Tero Karvinen & Lari-Iso Anttila 2025: Verkkoon tunkeutuminen ja tiedustelu

Kirjoittanut: <em>Santeri Vauramo</em> 2025
