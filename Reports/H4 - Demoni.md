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

# Lähteet

- Karvinen, Tero 2018. Pkg-File-Service – Control Daemons with Salt – Change SSH Server Port. Luettavissa: https://terokarvinen.com/2018/04/03/pkg-file-service-control-daemons-with-salt-change-ssh-server-port/?fromSearch=karvinen%20salt%20ssh. Luettu: 18.04.2024
- Karvinen, Tero 2023. Salt Vagrant - automatically provision one master and two slaves. Luettavissa: https://terokarvinen.com/2023/salt-vagrant/#infra-as-code---your-wishes-as-a-text-file. Luettu: 18.04.2024
- Salt Project s.a. Salt overview - Rules of YAML. Luettavissa: https://docs.saltproject.io/salt/user-guide/en/latest/topics/overview.html#rules-of-yaml. Luettu: 18.04.2024
