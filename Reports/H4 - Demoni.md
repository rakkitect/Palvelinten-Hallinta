# H4 - Demoni

## Tehtävä x) Lue ja tiivistä

### Tero Karvinen: Salt Vagrant - automatically provision one master and two slaves

- .sls-tiedostot luodaan kansioon srv/salt/
- .sls-tiedostoilla määritetään mitä salt-tilat tekevät
- top.sls tiedosto määrittää mitä tiloja ajetaan millekkin orjalle
  - kun top.sls on määritetty, se ajetaan komennolla ````sudo salt '*' state.apply````
    - tällöin sinun ei tarvitse erikseen nimetä kaikkia ajettavia moduuleja
   
### Salt Project: Rules of YAML, YAML simple structure, Lists and dictionaries - YAML block structures

- Saltissa käytetään YAML-merkintäkieltä
- Tärkeitä YAML-sääntöjä:
  - Tabulaattoria ei pidä käyttää, kaksi välilyöntiä = sisennys
  - Kirjainkoolla on väliä
  - Key: value parit merkitään ": ", eli kaksoipiste ja välilyönti
 - YAML:ssa on kolme perus elementtiä
   - Skalaarit (scalars)
     -````key: value```` pari missä ````value```` voi olla numero, merkkijono tai boolean
   - Listat (lists)
     - ````key:```` jonka jälkeen on rivinvaihto, ja uudessa sisennyksessä ````- value````
     - jokainen kohta merkitään väliviivalla ja välilyönnillä ("- ")
   - Sanakirjoja (dictionaries)
     - kokoelma ````key: value````-pareja ja listoja

### Tero Karvinen: Pkg-File-Service – Control Daemons with Salt – Change SSH Server Port

- Pkg-File-Service kombinaatiolla pystyt samalla tilalla asentamaan orja-koneille ohjelman, ja varmistaa sen toiminnan
  - pkg.installed asentaa sovelluksen paketinhallinnasta
  - file.managed kopioi konfiguraatio-tiedoston herra-koneelta orja-koneelle
  - service.running varmistaa että sovellus on käynnissä
 
### Salt ohjeet tilafunktioille pkg, file ja service

#### pkg
pkg-tilafunktiolla voi hallita pakettejen asennusta, poistoa ja päivitystä
- pkg.installed varmistaa että paketti on asennettu, ja että se on oikea versio mikäli haluttu versio on tarkennettu.
- pkg.purged varmistaa että pakettia ei ole asennettu. Mikäli paketti löytyy asennettuna, se poistetaan mukaanlukien mahdolliset konfiguraatiotiedostot
- pkgs-avainta käytetään jos haluaa yhdellä tilalla määrittää useamman paketin listana

#### file
Käsittelee tiedostoja, hakemistoja ja symboolisia linkkejä
- file.managed mahdollistaa tiedoston lataamisen herra-koneelta
  - tiedostoa voidaan säilöä herra-koneella, lokaalisti orja-koneella tai HTTP/FTP-palvelimella
- file.absent varmistaa että tiedostoa tai hakemistoa ei ole olemassa
- file.symlink luo symboolisen linkin (symlink).
  - Jos samanniminen symlink on jo olemassa, tila palaa epätotena. Tämä voidaan ennaltaehkäistä käyttämällä ````backupname: ```` 

#### service
Sisältää systemd-ympäristössä määriteltyjen palveluiden hallitsemista helpottavia tiedostoja ja ohjeita
- service.running varmistaa että palvelu on käytössä
- service.dead varmistaa että palvelu ei ole käytössä, pysäyttämällä sen mikäli se on päällä
- service.enabled tila asettaa palvelun käynnistymään automaattisesti järjestelmän käynnistyessä

## Tehtävä a) Hello SLS! Tee Hei maailma -tila kirjoittamalla se tekstitiedostoon, esim /srv/salt/hello/init.sls.

Loin kansion .sls tiedostoa varten komennolla ````sudo mkdir -p /srv/salt/hello/````. ````-p````-parametrin lisäämällä komento luo myös parent-kansion /salt/.

Loin hello-hakemistoon init.sls kansion komennolla ````sudoedit init.sls```` ja kirjoitin sisällöksi:

    /tmp/helloworld
      file.managed

Tämän jälkeen ajoin salt-tilan komennolla ````sudo salt-call --local state.apply hello````. Ohessa kuva prosessista ja lopputuloksesta.

![Tehtävä a](https://github.com/rakkitect/Server-Management/blob/main/Images/teht%C3%A4v%C3%A4_a.png)


## Tehtävä b) Tee top.sls niin, että useita valitsemiasi tiloja ajetaan automaattisesti

Tätä tehtävää varten loin muutamia tiloja.

![salt_tilat](https://github.com/rakkitect/Server-Management/blob/main/Images/salt_tilat.png)

Luotuani tilat, lisäsin ````/srv/salt/````-hakemistoon "top.sls"-tiedoston:

    base:
      '*':
        - hello
        - favorites
        - users

Sen jälkeen ajoin kaikki tilat komennolla ````sudo salt-call --local state.apply````, ja lopputulos oli tämä:

![salt_top](https://github.com/rakkitect/Server-Management/blob/main/Images/salt_top.png)

## Tehtävä c) Asenna Apache käsin ja luo salt-tila

Paketin asennus menee näin:

    sudo apt-get update
    sudo apt-get -y install apache2

Loin kotihakemistoon kansion ````mkdir DemoniTestisivu````, ja loin sinne index.html-tiedoston. Tehtävän lopuksi tähän tiedostoon kirjoittamani tekstin olisi tarkoitus tulla näkyviin ruudulle ````curl http://localhost```` komennolla.

Menin kansioon ````etc/apache2/sites-available/````, missä oli kaksi default-tiedostoa, **000-default.conf** ja **default-ssl.conf**. Poistin nämä molemmat, ja loin oman .conf-tiedostoni nimeltään DemoniTestisivu.conf

    <VirtualHost *:80>
    ServerName www.demonitestisivu.com
    ServerAlias demonitestisivu.com
    DocumentRoot /home/vagrant/DemoniTestisivu
      <Directory /home/vagrant/DemoniTestisivu>
          Require all granted
      </Directory>
    </VirtualHost>

![DemoniTestisivun conf](https://github.com/rakkitect/Server-Management/blob/main/Images/demoni_testi_conf.png)

Tämän jälkeen siirryin hakemistoon ````/etc/apache2/sites-enabled/````, johon loin uuden symbolisen linkin äsken luomaamme .conf-tiedostoon komennolla ````sudo ln -s /etc/apache2/sites-available/DemoniTestisivu.conf DemoniTestisivu.conf````. Lisäksi poistin linkin aiemmin poistettuun **000-default.conf**-tiedostoon. Komennossa "ln" tarkoittaa linkin luontia, ja "-s" tekee siitä symbolisen linkin. Symbolinen linkki on verrattavissa Windowsin pikakuvakkeeseen, eli se vain viittaa alkuperäiseen tiedostoon tai kansioon.

Tehtyäni nämä toimenpiteet, käynnistin Apachen uudelleen komennolla ````sudo systemctl restart apache2````, ja varmistin että kaikki toimii niinkuin pitää:

![Demoni toimii](https://github.com/rakkitect/Server-Management/blob/main/Images/demoni_toimii.png)

Nyt kun kaikki toimii manuaalisen asennuksen jälkeen, tämä voidaan automatisoida Saltin avulla.

Loin init.sls tiedoston kansioon ````srv/salt/apache/````, ja konfiguroin sen seuraavasti:

    apache2:
      pkg.installed
      
    /etc/apache2/sites-available/DemoniTestisivu.conf:
      file.managed:
        - source: "salt://apache/DemoniTestisivu.conf"
        
    /etc/apache2/sites-enabled/osku.conf:
      file.symlink:
        - target: "/etc/apache2/sites-available/DemoniTestisivu.conf"
        
    /etc/apache/sites-enabled/000-default.conf:
      file.absent
      
    apache2.service:
      service.running

Tila toimii seuraavasti:
- apache2 varmistaa että paketti apache2 on asennettu
- /etc/apache2/sites-available/DemoniTestisivu.conf: varmistaa että "DemoniTestisivu.conf" löytyy kyseisestä kansiosta
- /etc/apache2/sites-enabled/osku.conf: varmistaa että symbolinen linkki on olemassa
- /etc/apache/sites-enabled/000-default.conf: varmistaa/poistaa **000-default.conf**-tiedoston
- apache2.service varmistaa että demoni on käynnissä

Seuraavaksi koitin ajaa juuri luomani salt-tilan komennolla ````sudo salt-call --local state.apply apache````, ja luulin kaiken olevan kunnossa kunnes sain virheen:

![Demoni salt virhe](https://github.com/rakkitect/Server-Management/blob/main/Images/demoni_salt_virhe.png)

Hetken pohdittuani, tajusin että minun tarvitsee kopioida **DemoniTestisivu.conf**-tiedosto myös ````/srv/salt/apache/````-hakemistoon, jotta salt sitä voisi käyttää. Melko loogista :)
Kopioituani tiedoston kansioon, kaikki toimi kuten piti

![Demoni_salt_toimii](https://github.com/rakkitect/Server-Management/blob/main/Images/demoni_salt_toimii.png)

Siinä oli Apachen asennus manuaalisesti ja automatisoituna

## Tehtävä d) SSHouto. Lisää uusi portti, jossa SSHd kuuntelee

Tässä tehtävässä en ollut täysin varma mitä olen tekemässä, joten seurasin Tero Karvisen ohjetta: [Pkg-File-Service – Control Daemons with Salt – Change SSH Server Port](https://terokarvinen.com/2018/04/03/pkg-file-service-control-daemons-with-salt-change-ssh-server-port/?fromSearch=karvinen%20salt%20ssh).

Seuraten ohjetta, loin tiedoston **sshd.sls** ````/srv/salt/```` kansioon:

    openssh-server:
      pkg.installed
      
    /etc/ssh/sshd_config:
      file.managed:
        - source: salt://sshd_config
        
    sshd:
      service.running:
        - watch:
          - file: /etc/ssh/sshd_config

Tila toimii näin:
- openssh-server varmistaa että "openssh-server"-paketti on asennettuna
- /etc/ssh/sshd_config varmistaa että salt-masterilla oleva sshd_config tiedosto löytyy orja-järjestelmästä
- sshd pitää sshd_server demonin käynnissä, ja käynnistää sen uudelleen mikäli sshd_config-tiedostoa muutetaan masterilla

Edellisestä tehtävästä oppineena loin **sshd_config**-tiedoston ````/srv/salt/````-kansioon:

![ssh config](https://github.com/rakkitect/Server-Management/blob/main/Images/ssh_config.png)

Lisäsin Karvisen pohjaan portin 22, koska suoritan tehtäviä Vagrant-koneella ja yhteyteni koneeseen on tuossa portissa. Lisäsin myös portin 1234 testimielessä.

Ajoin tilan komennolla ````sudo salt-call --local state.apply sshd```` ja korjattuani muutamat typot mitä huomasin virheiden pohjalta, kaikki lopulta toimii:

![sshd_toimii](https://github.com/rakkitect/Server-Management/blob/main/Images/sshd_toimii.png)

# Lähteet

- Karvinen, Tero 2018. Pkg-File-Service – Control Daemons with Salt – Change SSH Server Port. Luettavissa: https://terokarvinen.com/2018/04/03/pkg-file-service-control-daemons-with-salt-change-ssh-server-port/?fromSearch=karvinen%20salt%20ssh. Luettu: 18.04.2024
- Karvinen, Tero 2023. Salt Vagrant - automatically provision one master and two slaves. Luettavissa: https://terokarvinen.com/2023/salt-vagrant/#infra-as-code---your-wishes-as-a-text-file. Luettu: 18.04.2024
- Salt Project s.a. Salt overview - Rules of YAML. Luettavissa: https://docs.saltproject.io/salt/user-guide/en/latest/topics/overview.html#rules-of-yaml. Luettu: 18.04.2024
