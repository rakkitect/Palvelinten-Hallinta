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

## Tehtävä a) Hello SLS


# Lähteet

- Karvinen, Tero 2018. Pkg-File-Service – Control Daemons with Salt – Change SSH Server Port. Luettavissa: https://terokarvinen.com/2018/04/03/pkg-file-service-control-daemons-with-salt-change-ssh-server-port/?fromSearch=karvinen%20salt%20ssh. Luettu: 18.04.2024
- Karvinen, Tero 2023. Salt Vagrant - automatically provision one master and two slaves. Luettavissa: https://terokarvinen.com/2023/salt-vagrant/#infra-as-code---your-wishes-as-a-text-file. Luettu: 18.04.2024
- Salt Project s.a. Salt overview - Rules of YAML. Luettavissa: https://docs.saltproject.io/salt/user-guide/en/latest/topics/overview.html#rules-of-yaml. Luettu: 18.04.2024
