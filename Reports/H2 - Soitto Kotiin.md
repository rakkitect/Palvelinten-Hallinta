# H2 - Soitto Kotiin

## Tehtävä x) Lue ja tiivistä

### Karvinen 2021: [Two Machine Virtual Network With Debian 11 Bullseye and Vagrant](https://terokarvinen.com/2021/two-machine-virtual-network-with-debian-11-bullseye-and-vagrant/)
- Kahden tai useamman koneen virtuaalinen verkkoympäristö on helppo pystyttää käyttäen Vagrantia
- Vagrant voidaan helposti asentaa Debian- tai Ubuntu-käyttöjärjestelmille paketinhallinnan avulla, tai Windows- ja Mac-käyttöjärjestelmille [Vagrantin verkkosivuilta](https://developer.hashicorp.com/vagrant/install)
- Kaikki verkkoympäristöön haluttavat koneet konfiguroidaan saman Vagrantfile-tiedoston avulla

### Karvinen 2018: [Salt Quickstart – Salt Stack Master and Slave on Ubuntu Linux](https://terokarvinen.com/2018/salt-quickstart-salt-stack-master-and-slave-on-ubuntu-linux/?fromSearch=salt%20quickstart%20salt%20stack%20master%20and%20slave%20on%20ubuntu%20linux)
- Saltin avulla pystytään hallinnoimaan jopa tuhansia koneita etänä
- Saltista on kaksi versiota; orja (slave) ja herra (master)
  - Salt-Master tarvitsee asentaa vain yhdelle koneelle, mutta jokainen hallittava kone tarvitsee Salt-Minionin
- Kun orjakoneet on hyväksytty ja yhdistetty herra-koneeseen, herra-koneelta pystyy ajamaan samat komennot kaikkiin koneisiin kerralla

### Karvinen 2014: [Hello Salt Infra-as-Code](https://terokarvinen.com/2024/hello-salt-infra-as-code/)
- On hyvä käytäntö, että jokaiselle loogiselle kokonaisuudelle, eli moduulille, on oma kansionsa, esim: /srv/salt/apache2
- Moduulikansio sisältää kaikki tarvittavat tiedostot ja mallipohjat
- Saltin konfiguraatiot kirjoitetaan .sls tiedostoihin ja sijoitetaan moduulikansioon, esim: /srv/salt/apache2/init.sls

## Tehtävä a) Kahden virtuaalikoneen asennus samaan verkkoon

Suoritin tämän tehtävän pöytäkoneellani, jolla on käytössä Windows 11. Olen jo aiemmin asentanut Vagrantin [täältä](https://developer.hashicorp.com/vagrant/install).

Loin tyhjän kansion tehtävää varten, jonne loin Vagrantfilen käyttäen Notepad++ sovellusta. Mielestäni tämä on Windowsilla helpoin tapa, Debianilla tai Ubuntulla käyttäisin komentorivin kautta Nanoa tai Microa.
Vagrantfilen sisältö on lainattu muokaten [Tero Karviselta](https://terokarvinen.com/2021/two-machine-virtual-network-with-debian-11-bullseye-and-vagrant/)

![Vagrantfile](https://github.com/rakkitect/Server-Management/blob/main/Images/vagrantfile.png)

Luotuani Vagrantfilen, ajoin komennon

    vagrant up

joka luo tiedostossa määritellyt koneet 'kone1' ja 'kone2'.

Tämän jälkeen avasin toisen PowerShell-ikkunan administrator-oikeuksilla, ja ajoin näissä kahdessa ruudussa omat komentonsa:

    vagrant ssh kone1

ja

    vagrant ssh kone2

Kun sain koneisiin yhteyden, pingasin molemmilla koneilla toisiaan komennoilla:

    ping -c 2 192.168.88.102

ja

    ping -c 2 192.168.88.101

Komennon osa "-c 2" rajoitti pingien määrän kahteen, sillä jostain syystä koneet halusivat jatkaa pingaamista loputtomiin?

![Ping ja pong](https://github.com/rakkitect/Server-Management/blob/main/Images/ping.png)

## Tehtävä b) Saltin herra-orja arkkitehtuuri verkon yli

Asensin edellisessä tehtävässä luomilleni Vagrant koneille salt-masterin kone1:lle ja salt-minionin kone2:lle.

Asennukset onnistuivat komennoilla:

    sudo apt-get update
    sudo apt-get -y install salt-master

ja

    sudo apt-get update
    sudo apt-get -y install salt-minion

Orjakone tarvitsee tiedon herrakoneen sijainnista verkkoavaruudessa, ja tein tämän lisäämällä herrakoneen IP-osoitteen orjakoneen konfiguraatiotiedostoon komennolla

    sudoedit /etc/salt/minion

Herrakoneen IP-osoite lisätään "master" riville. Kaikki rivit ovat pois käytöstä kommentteina, joten poistin rivin alustas "#" merkin, jonka jälkeen oma rivini näytti tältä:

![Minion konfiguraatio](https://github.com/rakkitect/Server-Management/blob/main/Images/minion_config.png)

Konfiguraatiotiedoston muokkauksen jälkeen palvelu piti käynnistää uudelleen komennolla:

    sudo systemctl restart salt-minion.service

Hyväksyin orjan yhteydenoton herrakoneella komennolla:

    sudo salt-key -A
    Y

![Master Accept](https://github.com/rakkitect/Server-Management/blob/main/Images/master_accept.png)

Nyt kone1 on kone2 herra, ja pystyn ajamaan komentoja "etänä".

## Tehtävä c) Shell-komennon ajaminen orjalla

Ajoin orjakoneella shell-komennon:

    $vagrant@kone1 sudo salt '*' cmd.run 'ls -l /home'

Komento listasi orjakoneen /home-hakemiston sisällön.

![Shell-komento](https://github.com/rakkitect/Server-Management/blob/main/Images/shell_komento.png)

## Tehtävä d) Useiden idempotenttejen komentojen ajaminen master-slave yhteyden ylitse

Ajamani idempotentit komennot:

    sudo salt '*' state.single pkg.installed nginx

Komento tarkastaa onko nginx-paketti asennettu orja-koneille. Mikäli ei, se asennetaan, ja jos on niin mitään ei tapahdu.

    sudo salt '*' state.single file.touch /etc/example.conf

Komento yrittää luoda tiedoston nimeltä example.conf hakemistoon /etc. Jos tiedosto on jo olemassa, mitään ei tapahdu, mutta jos sitä ei ole olemassa, se luodaan.

    sudo salt '*' state.single user.present john_doe

Komento tarkistaa onko käyttäjä john_doe olemassa.

    sudo salt '*' state.single file.directory /opt/esimerkki_hakemisto/

Komento varmistaa että hakemisto /opt/esimerkki_hakemisto/ on olemassa.

## Tehtävä e) Teknisten tietojen keruu orjista

Teknisten tietojen keruu orjakoneista onnistuu komennolla:

    sudo salt '*' grains.item cpu_model cpuarch kernelversion

Mikäli orjakoneita olisi useampi kuin yksi, niin tiedot jaoteltaisiin koneittain.

![Grains.item](https://github.com/rakkitect/Server-Management/blob/main/Images/grains_item.png)

## Tehtävä f) Infraa koodina

Jotta voin tehdä infraa koodina, loin uuden hakemiston hello-moduulille:

    sudo mkdir -p /srv/salt/hello/
    cd /srv/salt/hello/

Tähän kansioon loin uuden init.sls konfiguraatio-tiedoston

    sudo nano init.sls

![Infraa koodina](https://github.com/rakkitect/Server-Management/blob/main/Images/infraa_koodina_init.png)

Tämä tila luo example.txt tiedoston orjakoneen koti-hakemistoon.

Konfiguraatio-tiedoston luomisen jälkeen ajoin tilan orjakoneelle:

    sudo salt '*' state.apply hello

![Tilan ajo](https://github.com/rakkitect/Server-Management/blob/main/Images/tilan_ajo.png)

Kuten kuvasta näkyy, ajon jälkeinen tila on "Succeeded: 1 (changed=1). Varmistan tiedoston olemassaolon vielä komennolla:

    sudo salt '*' cmd.run 'ls -l /home/vagrant/'

![Tarkistus](https://github.com/rakkitect/Server-Management/blob/main/Images/tarkistus.png)

# Lähteet:

- Karvinen 2021. Two Machine Virtual Network With Debian 11 Bullseye and Vagrant. Luettavissa: https://terokarvinen.com/2021/two-machine-virtual-network-with-debian-11-bullseye-and-vagrant/. Luettu: 07.04.2024
- Karvinen 2018. Salt Quickstart – Salt Stack Master and Slave on Ubuntu Linux. Luettavissa: https://terokarvinen.com/2018/salt-quickstart-salt-stack-master-and-slave-on-ubuntu-linux/?fromSearch=salt%20quickstart%20salt%20stack%20master%20and%20slave%20on%20ubuntu%20linux. Luettu: 07.04.2024
- Karvinen 2014. Hello Salt Infra-as-Code. Luettavissa: https://terokarvinen.com/2024/hello-salt-infra-as-code/. Luettu: 07.04.2024
