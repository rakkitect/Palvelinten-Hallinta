# H1 - Toimiva versio

## Tehtävä x) Lue ja tiivistä

### Chacon and Straub 2014: Pro Git, 2ed: 1.3 Getting Started - What is Git?

- Git:illä ja muiden versionhallinta työkalujen suurin ero on se, että muut työkalut käsittelevät säilöttyä dataa tiedostoina ja listana muutoksia mitä niihin on tehty. Git sen sijaan käsittelee dataa "tilannekuvina" käsiteltävästä datasta siinä tilanteessa missä muutokset on tehty.
- Git mahdollistaa varastoissa työskentelyn lokaalisti ilman verkkoyhteyttä. Tämä vaatii tietty sen että varasto on ladattu tietokoneellesi jossain vaiheessa.
- Mikäli tekemäsi muutokset on siirretty Git:iin commit viestillä, siirrettyä dataa on melkein mahdotonta kadottaa lopullisesti
- Git:issä on kolme pääasiallista tilaa:
  - Modified = Tiedostoja on muutettu, muttei siirretty tietokantaan
  - Staged = Muutokset on merkattu siirtoa varten
  - Committed = Muutokset on siirretty tietokantaan
 
### git add . && git commit; git pull && git push

- git add = Komento siirtää muutokset "staged"-tilaan. Mahdollistaa tilannekuvan esivalmistelun ennen commit-viestiä
- git commit = Siirtää tilannekuvan projektin/tiedoston nykytilasta tietokantaan
- git pull = Lataa haaran ulkoisesta varastosta, ja yhdistää sen vastaavaan lokaaliin varastoon. Tämä on automaattinen versio komennosta "git fetch", missä ladattuja muutoksia ei yhdistetä automaattisesti
- git push = Puskee lokaalin haaran ulkoiseen varastoon, siirtäen tekemäsi muutokset sinne

Lähde: https://www.atlassian.com/git/glossary#commands

### Varaston terokarvinen/suolax/ historia, eli loki ja muutokset

Muutokset järjestyksessä:
  - Initial commit, eli varaston luonti
  - README.md muutoksia
  - Kansion /srv/salt/hello luonti, ja init.sls-tiedoston lisäys
  - Makefile-tiedoston lisäys
  - Kansion /srv/salt/favourites luonti, ja init.sls-tiedoston lisäys
  - Makefile-tiedoston muokkaus
  - top.sls luonti /srv/salt kansioon
  - Lisää Makefile-tiedoston muokkausta
 
## Tehtävä a) Varaston luonti GitHub:iin

- Luodaksesi varaston tarvitset GitHub-tilin 
- Varaston saa luotua "Repositories"-välilehdellä, klikkaamalla vihreää "New"-ikonia
![CreateNew](https://github.com/rakkitect/Server-Management/blob/main/Images/new_repository.png)
- Varastolle pitää antaa nimi.
- Saat valita onko varastosi julkinen vai yksityinen. Yksityinen on näkyvillä vain itsellesi, ja muille jotka saavat työskennellä varastossasi. Tätä tehtävää varten valitsin julkisen.
- Voit luoda automaattisen README.md tiedoston, mikä on suositeltavaa.
- Tero Karvinen suosittelee artikkelissaan GNU General Public License version 3 lisenssiä, joten käytän sitä.

Tekemäni valinnat varastoa tehdessä:

![Tehtävän varasto](https://github.com/rakkitect/Server-Management/blob/main/Images/summer_repository.png)

Luotu varasto:

![Luotu varasto](https://github.com/rakkitect/Server-Management/blob/main/Images/valmis_varasto.png)

## Tehtävä b) Varaston kloonaus lokaaliksi

Valmistelen kloonauksen luomalla itselleni ssh-avaimen komennolla ````ssh-keygen````, jonka jälkeen painoin kolme kertaa **Enter** (tämä vahvistaa tallennuskansion, ja asettaa tyhjän salauksen)

Kopioin luomani ssh-avaimen komennolla ````clip < ~/.ssh/id_ed25519.pub```` ja lisäsin sen GitHub:in avain-listalle. Tämä tapahtuu varaston asetuksissa: Settings -> Deploy keys -> Add deploy key

Lisäksi asetin Git Bash:iin sähköpostin ja käyttäjänimen komennoilla ````git config --global user.email "heikkinenosku94@gmail.com```` ja ````git config --global user.name "Osku Heikkinen"````

Varsinainen kloonaus tapahtui komennolla ````git clone git@github.com:rakkitect/fried-opossum-in-the-summer.git````, jonka jälkeen minulta kysyttiin luotanko isäntään github.com. Vahvistin tämän, jonka jälkeen pystyin siirtyä ladattuun kansioon komennolla ````cd fried-opossum-in-the-summer````. Siirryttyäni kansioon, suoritin komennon ````git add . && git commit; git pull && git push````.

Komentorivillä loin kansion summer_file komennolla ````mkdir summer_file````, ja lisäsin sinne .md tiedoston komennolla ````nano summer.md````. Tehtyäni nämä muutokset ajoin komennon  ````git add . && git commit; git pull && git push````, jonka jälkeen minun piti kirjoittaa commit-viesti. Kirjoitettuani sen muutokset menivät läpi, ja tiedosto näkyy webbiliittymässä.

Komennot:

![Commandline POV](https://github.com/rakkitect/Server-Management/blob/main/Images/dolly.png)

Webbiliittymän näkymä luodusta tiedostosta:

![Webbiliittymä POV](https://github.com/rakkitect/Server-Management/blob/main/Images/web_dolly.png)


## Tehtävä c) Tyhmä muutos gittiin, ja niiden tuhoaminen

Tyhmänä muutoksena ajattelin lisenssin ja README.md-tiedoston poistamista. Nämä toteutin komennoilla ````rm LICENSE```` ja ````rm README.md````. Jäljelle jäi vain luomani kansio.
Tämän jälkeen ajoin komennon ````git reset --hard````, joka ilmeisesti palauttaa varaston viimeisimpään "tilannekuvaan", mitätöiden poistamisen.

![Tyhmät muutokset ja resetointi](https://github.com/rakkitect/Server-Management/blob/main/Images/git_reset.png)

## Tehtävä d) Tarkastele ja selitä varastosi lokia.

![Varaston loki](https://github.com/rakkitect/Server-Management/blob/main/Images/varaston_loki.png)

Ensimmäinen committi vaikuttaa olevan lisenssin lisäys ja /dev/null kansion poistaminen. Nämä ovat ilmeisesti jotain mikä tapahtuu automaattisesti varastoa luodessa, ja commit-viesti onkin "Initial commit". Myöhemmin historiassa lisenssin jälkeen näkyy myös README.md-tiedoston lisäys. Tämän commitin author on oma GitHub-tunnukseni, koska varasto on tietty luotu webbiliittymässä.

Toisessa commitissa näkyy taas /dev/null kansion poistaminen, mikä on erikoista mutta en keksi tälle itse mitään selitystä. Lisäksi siinä on kansion ja tekstitiedoston luominen.
Tämän commitin author on Git Bashissa lisätty sähköposti ja käyttäjänimi.

## Tehtävä e) Salt-tilojen ajaminen omasta varastosta

Tätä tehtävää en oikein ymmärtänyt. Jostain syystä en pystynyt sisäistämään kuinka salt-tiloja pystyy ajamaan omasta varastosta, ja henk.koht syistä tehtävän teko jäi viimetinkaan, josta syystä en pystynyt ennen palautusaikaa syventymään tähän enempää.
Aion kuitenkin arvioidessani muiden tehtäviä seurata heidän esimerkkiään ja ehkä tällä uudella ymmärryksellä toteuttaa tehtävän.


# Lähteet

Atlassian s.a. Git Glossary - Commands. Luettavissa: https://www.atlassian.com/git/glossary#commands. Luettu: 12.04.2024
Chacon and Straub 2014. Pro Git, 2ed: 1.3 Getting Started - What is Git? Luettavissa: https://git-scm.com/book/en/v2/Getting-Started-What-is-Git%3F. Luettu: 12.04.2024
GitHub Docs s.a. Generating a new SSH key and adding it to the ssh-agent. Luettavissa: https://docs.github.com/en/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent. Luettu: 14.04.2024
Tero Karvinen 2023. Create a Web Page Using Github. Luettavissa: https://terokarvinen.com/2023/create-a-web-page-using-github/. Luettu: 14.04.2024
