# H6 - Benchmark

## Tehtävä x) Lue ja tiivistä

### Salt Project - Windows Package Manager

- Salt tarjoaa paketinhallintajärjestelmän joka toimii samalla periaatteella kuin Linuxilla `yum` ja `apt`
- Paketin määritelmätiedosto sisältää:
  - paketin koko nimen
  - paketin version
  - paketin latauspaikan
  - komentorivin määritykset hiljaiseen asennukseen ja asennuksen poistoon
  - Windowsin tehtävien ajoituksen käyttömahdollisuuden
- Paketinhallinta tapahtuu GitHub:in kautta
- Paketinhallinta otetaan käyttöön kopioimalla Git repo, jonka jälkeen tietokanta pitää päivittää komennolla `pkg.refresh_db`. Tämän jälkeen `pkg.install`-komentoa voidaan käyttää lokaalisti Windowsilla tai herra-koneen välityksellä Windows-orjille.
- Tärkeitä komentoja:
  - `pkg.list_pkgs` = Listaa koneelle asennetut paketit. Huomasin että tämä listaa myös paketinhallintajärjestelmän ulkopuoliset sovellukset, mm. Steamin kautta asennetut pelit.
  - `pkg.list_available` = Listaa saatavilla olevat versiot määritetystä paketista
  - `pkg.install` = Asentaa määritetyn paketin
  - `pkg.remove` = Poistaa määritetyn paketin

## Tehtävä a) Asenna Windowsiin ohjelmia Saltin pkg.installed-funktiolla

Suoritin paketinhallintajärjestelmän alustuksen koneelleni komennoilla `salt-call --local winrepo.update_git_repos` ja `salt-call --local pkg.refresh_db`.

![alustus](https://github.com/rakkitect/Server-Management/blob/main/Images/pkg_manager_alustus.png)

Tämän jälkeen asensin paketit **Firefox_x64**, **Gimp** ja **Putty**. Gimp vaati install wizardin käyttöä.

![asennus](https://github.com/rakkitect/Server-Management/blob/main/Images/pkg_manager_install.png)

## Tehtävä b) Keskitetyn hallinnan projektien esittely

### https://katrilaulajainen.wordpress.com/2018/05/10/palvelinten-hallinta-h6-8-5-2018-oma-miniprojekti-saltilla/

- Tarkoitus: Asettaa yrityksen koneelle oletuskotisivun, sekä asentaa Gimp-kuvanmuokkaustyökalun. Lisäksi asentaa ja konfiguroi palomuurin ja ssh-yhteyden etätöitä varten.
- Lisenssi: Tarkistin tämän projektin GitHub:ista, ja käytetty lisenssi on GNU General Public License v3.0. 
- Tekijä ja vuosi: Katri Laulajainen, 2018.
- Riippuvuudet: Moduuli on tehty Linux-käyttöjärjestelmälle, muita riippuvuuksia en tunnista.
- Kiinnostavaa: Lopputulos on mielestäni hyödyllinen.
- Kysymyksiä ja huomioitavaa: Ottaen huomioon kuinka suurelle osalle potentiaalisista käyttäjistä Windows-käyttöjärjestelmä saattaisi olla mukavempi käyttää, olisin itse suunnannut moduulin ehkä sille käyttöjärjestelmälle. Tietenkään en tiedä juuri tämän yrityksen taustaa, että käyttävätkö he Linuxia työkoneillaan.

### https://janni.tech.blog/2020/05/19/h7-oma-moduuli/

- Tarkoitus: LAMP:in asennus Master-koneelle ja kahdelle orjalle.
- Lisenssi: Ei mainittu
- Tekijä ja vuosi: Janni Ojala, 2020.
- Riippuvuudet: Ubuntu-käyttöjärjestelmä ja CentOS-käyttöjärjestelmä.
- Huomioitavaa: Tekijä ei saanut moduuliansa toimimaan.

### https://tonivapalo.com/posts/palvelintenhallinta/phvt7/

- Tarkoitus: Python Flask- ja React kehitysympäristöjen automatisointi Vagrant-virtuaalikoneisiin
- Lisenssi: Tarkistin tämän projektin GitHub:ista, ja käytetty lisenssi on GNU General Public License v3.0.
- Tekijä ja vuosi: Toni Vapalo, 2021.
- Riippuvuudet: VirtualBox ja Vagrant Debian 10.
- Huomioitavaa: Tekijä ei saanut moduuliansa täysin toimintaan, tiedosti itsekkin että projekti oli kunnianhimoinen.

### https://sampohautala.wordpress.com/2018/12/09/ph-h7-oma-moduli/

- Tarkoitus: Front-end kehitysympäristön asennus Linux-koneelle, sekä kuvanmuokkaus ja 3D-mallinnusohjelmien asennus Windows-koneelle.
- Lisenssi: Tarkistin tämän projektin GitHub:ista, ja käytetty lisenssi on GNU General Public License v3.0.
- Tekijä ja vuosi: Sampo Hautala, 2018.
- Riippuvuudet: Linux- ja Windows-käyttöjärjestelmät.
- Kiinnostavaa: Projekti oli laaja, hyödyllinen ja onnistunut.

## Tehtävä c) Testbench. Aja toisen tekemä tila.

Valitsin tehtävää varten Katri Laulajaisen yrityskoneen konfiguraatio-moduulin. Tarkistin lähdekoodin GitHubissa, ja ladattavat sovellukset tulevat paketinhallinnasta. En huomannut koodissa mitään epäilyttävää, mutta suoritan asennuksen ja testin silti Vagrant-virtuaalikoneella.

Loin uuden Vagrant koneen Debian/Bullseye64-käyttöjärjestelmällä. Käynnistettyäni virtuaalikoneen päivitin paketinhallinnan, asensin Git:in ja kopioin moduulin Git repon kotihakemistooni komennolla `git clone https://github.com/KatriL/tulikettu.git`.

![Repon kopiointi](https://github.com/rakkitect/Server-Management/blob/main/Images/moduulin_lataus.png)

Seurasin moduulin tekijän ohjeita, ja ajoin komennon `sudo bash high.sh`. Huomasin heti yhden ongelman: Moduuli ei asenna ufw-pakettia, jolloin osa moduulin tiloista ei toimi. Lisäksi 'firefox' pakettia ei löytynyt. Korjasin nämä ongelmat itse lisäämällä tilan `init.sls`-tiedostoon asennettavan paketin ufw, ja muokkasin asennettavan paketin "firefox" muotoon "firefox-esr". Lisäksi muokkasin ID:n "/etc/firefox/syspref.js" muotoon "/etc/firefox-esr/syspref.js", ja tämän jälkeen tila toimi niinkuin piti.


Keltaisella korostettu muutetut kohdat.

![Muokattu init.sls](https://github.com/rakkitect/Server-Management/blob/main/Images/moduuli_init.png)

![Ajettu tila](https://github.com/rakkitect/Server-Management/blob/main/Images/moduuli_ajettu_tila.png)

Vaikka tila toimi muuten niinkuin piti, niin nämä kaksi virhettä ovat suuria ja tekevät tilasta toimimattoman. En tiedä johtuuko ongelmat mahdollisesti eri käyttöjärjestelmistä.

## Tehtävä d) Viisi ideaa omille moduuleille.

- Moduuli joka asentaa nettikahvilan koneille yleisimmät tarvittavat sovellukset (windows)
- Moduuli joka asentaa palvelupisteelle tarvittavat sovellukset (windows)
- Moduuli joka asentaa kotikoneeseen halutut ohjelmat (windows)
- Moduuli joka asentaa palvelinympäristön (linux)
- Moduuli joka alustaa pienen yrityksen käyttäjien koneet käyttökuntoisiksi

# Lähteet:

- Hautala, S. 2018. Ph h7 - Oma moduli. Luettavissa: https://sampohautala.wordpress.com/2018/12/09/ph-h7-oma-moduli/. Luettu: 06.05.2024
- Laulajainen, K. 2018. Palvelinten hallinta h6 8.5.2018 – Oma miniprojekti Saltilla. Luettavissa: https://katrilaulajainen.wordpress.com/2018/05/10/palvelinten-hallinta-h6-8-5-2018-oma-miniprojekti-saltilla/. Luettu: 06.05.2024
- Ojala, J. 2020. h7 Oma Moduuli. Luettavissa: https://janni.tech.blog/2020/05/19/h7-oma-moduuli/. Luettu: 06.05.2024
- Salt Project s.a. Windows Package Manager. Luettavissa: https://docs.saltproject.io/en/latest/topics/windows/windows-package-manager.html#usage. Luettu: 06.05.2024
- Vapalo, T. 2021. Oma moduli. Luettavissa: https://tonivapalo.com/posts/palvelintenhallinta/phvt7/. Luettu: 06.05.2024
