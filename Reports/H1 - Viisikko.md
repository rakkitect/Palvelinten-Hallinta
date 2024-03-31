# H1 - Viisikko

x) Lue ja tiivistä kolme annettua artikkelia:
- Karvinen 2023: Run Salt Command Locally. Luettavissa: https://terokarvinen.com/2021/salt-run-command-locally/
- Karvinen 2023: Create a Web Page Using Github. Luettavissa: https://terokarvinen.com/2023/create-a-web-page-using-github/
- Karvinen 2006: Raportin kirjoittaminen. Luettavissa: https://terokarvinen.com/2006/06/04/raportin-kirjoittaminen-4/

## Run Salt Command Locally:

- Salt-komennoilla voidaan hallita tietokonetta paikallisesti, mutta yleisin käyttötapa on useiden orjien hallitseminen etänä.
- Samat Salt-komennot toimivat Linux - sekä Windows-käyttöjärjestelmillä.
- Tärkeimmät tilafunktiot (state functions) ovat pkg, file, service, user ja cmd.

## Create a Web Page Using Github:

- Verkkosivun julkaiseminen Githubilla on helppoa ja aloittelijaystävällistä
- Uuteen arkistoon (repository) tarvitsee luoda README-tiedosto, muuten myöhemmin ilmenee ongelmia
  - Mahdolliset ongelmat eivät paljastu artikkelissa
- .md loppupääte muuntaa luomasi tiedoston verkkosivuksi MarkDown-merkintäkiellä

## Raportin kirjoittaminen:

- Raportissa tulee tulla selkeästi ilmi, mitä teit ja mihin tämä johti
- Mikäli joku muu seuraa raporttiasi, hänen tulisi päästä samaan lopputulokseen
- Täsmällisyys on tärkeää
- Lähteet, lähteet, lähteet
- Tekstin tulee olle helppolukuista ja selkokielistä ilman kirjoitusvirheitä

## Tehtävät

### a) Hello Windows Salt World!

Olen asentanut Windows-koneelleni Salt-minionin heidän verkkosivuiltaan: https://docs.saltproject.io/salt/install-guide/en/latest/topics/install-by-operating-system/windows.html

Todisteena kuvankaappaus tietokoneeni PowerShell komentoriviltä:

![PowerShell Administrator komentorivi](https://github.com/rakkitect/Server-Management/blob/main/Images/salt-version.png)

### b) Hello Vagrant!

Olen asentanut myös Vagrantin koneelleni. Kuvankaappauksessa olen ottanut SSH-yhteyden virtuaalikoneeseen.

![Vagrant SSH](https://github.com/rakkitect/Server-Management/blob/main/Images/vagrant.png)

### c) Linux-virtuaalikoneen luominen Vagrantilla

Aloitin uuden virtuaalikoneen luomalla tyhjän kansion Demo2 ja siirtymällä sinne komennoilla:

    mkdir demo2
    cd demo2

Loin tyhjän kansion virtuaalikonetta varten, jotta Vagrant voi luoda sinne vagrantfile-tiedoston.

Sen jälkeen alustin virtuaalikoneen komennolla:

    vagrant init bento/ubuntu-16.04

Ja käynnistin virtuaalikoneen komennolla:

    vagrant up

Jostain syystä virtuaalikoneen käynnistys ei toiminut, vastaukseksi sain virheen "timed out", valitettavasti unohdin ottaa tästä kuvankaappauksen.. Kokeilin uudelleen komennolla

    vagrant up

jonka jälkeen sain ilmoituksen että virtuaalikone on jo "varattu" (provisioned). Kokeilin ehdotettu komentoja, mutta näillä ei ollut vaikutusta tilanteeseen.

![Vagrant provision?](https://github.com/rakkitect/Server-Management/blob/main/Images/vagrant-provision.png)

Tässä vaiheessa päätin poistaa luomani kansion ja aloittaa alusta. Huomasin mahdollisen virheeni käyttäessäni pohjana raporttia vuodelta 2017, ja vaihdoin komentoni muotoon:

    vagrant init debian/bullseye64

ja tämä korjasi ongelmani.

![Onnistunut virtuaalikoneen käyttöönotto](https://github.com/rakkitect/Server-Management/blob/main/Images/vagrant-init2.png)

### d) Salt asennus Linux-virtuaalikoneelle

Aloitin Salt-minionin asennuksen päivittämällä paketin hallinnan, jonka jälkeen ajoin asennuksen:

    sudo apt-get update
    sudo apt-get -y install salt-minion

![Salt-minion asennettuna](https://github.com/rakkitect/Server-Management/blob/main/Images/salt-minion-linux.png)


### e) Viisi tärkeintä Salt tilafunktiota

#### pkg

pkg.installed varmistaa että järjestelmässä on jokin tietty sovellus asennettuna. Valitsin tätä varten Micro-tekstieditorin. Tilafunktio ajetaan komennolla:

    sudo salt-call --local -l info state.single pkg.installed micro

Koska Micro ei ollut asennettuna aiemmin, niin pkg.installed tilafunktio asensi sen. Ajaessani komennon toisen kerran, se tarkistaa onko sovellus asennettuna.

![pkg.installed demonstraatio](https://github.com/rakkitect/Server-Management/blob/main/Images/pkg_installed.png)

#### file

file.managed varmistaa että järjestelmässä on jokin tietty tiedosto. Tässä esimerkissä tarkistetaan löytyykö järjestelmästä tiedosto tmp/liirumlaarum, ja mikäli ei, niin se luodaan

    sudo salt-call --local -l info state.single file.managed /tmp/liirumlaarum

![Luotu kansio](https://github.com/rakkitect/Server-Management/blob/main/Images/file-managed.png)

#### service

service.running varmistaa että spesifioitu daemoni on käynnissä.

    sudo salt-call --local -l info state.single service.running apache2

En asentanut virtuaalikoneelleni Apache2-sovelluspakettia, joten annettu komento palasi "false" tilassa:

![Service.running](https://github.com/rakkitect/Server-Management/blob/main/Images/service-running.png)

#### user

user.present tilafunktio tarkistaa onko tietty käyttäjä järjestelmässä. Vastaavasti user.absent tarkistaa että käyttäjää EI ole järjestelmässä.

Molemmissa tilanteissa mikäli järjestelmän tila ei vastaa tilafunktion määritystä, se korjataan. Toisin sanoen user.present luo nimetyn käyttäjän mikäli tätä ei ole olemassa, ja user.absent poistaa käyttäjän mikäli tämä on olemassa.

![User.present demonstraatio](https://github.com/rakkitect/Server-Management/blob/main/Images/user-present.png)

#### cmd

cmd.run tilafunktio ajaa annetun komennon. Kokeilin komentoa

    sudo salt-call --local -l info state.single cmd.run date > /tmp/salt-run

joka tallensi "salt-run" nimiseen tiedostoon ajan komennon ajamishetkellä.

![cmd.run demonstraatio](https://github.com/rakkitect/Server-Management/blob/main/Images/cmd-run.png)
![Luotu tiedosto](https://github.com/rakkitect/Server-Management/blob/main/Images/salt-run.png)

### f) Idempotentti

Idempotentti toiminto ei muuta järjestelmää ensimmäisen kerran jälkeen. Esimerkkinä tästä on mm. käyttämäni user.present tilafunktio; kun ajoin komennon ensimmäisen kerran, se loi käyttäjän - mutta vaikka ajaisin saman komennon sata kertaa, käyttäjää ei luoda uudestaan (ellei käyttäjää poisteta välissä)

Muita vastaavia idempotenttejä tilafunktioita ovat file.managed ja pkg.installed, jotka ensimmäisen ajon jälkeen eivät muuta järjestelmän tilaa.

### g) Tiedon kerääminen koneesta grains.items komennolla

Grains, yksikössä grain, ovat tietoja järjestelmästä. Tiedot saa tulostettua ruudulle komennolla:

    sudo salt-call --local  grains.items | less

käytin "less" lisäkomentoa jotta tekstin saa paloiteltua helppolukuisempiin osiin.

Poimin kolme mielenkiintoista kohtaa komennolla:

    sudo salt-call --local grains.item ipv6 cpu_model biosreleasedate

![grains.item demonstraatio](https://github.com/rakkitect/Server-Management/blob/main/Images/grains-item.png)

- biosreleasedate
  - Virtuaalikoneessa käytetty BIOS on julkaistu 12.01.2006/01.12.2006, en ole varma mikä päivämääräformaatti on käytössä.
- cpu_model
  - Prosessoriksi on listattu AMD Ryzen 7 5800U with Radeon Graphics, eli oman tietokoneeni prosessori
- ipv6
  - ::1 on virtuaalikoneen localhost-osoite
  - fe80::a00:27ff:fe8d:c04d on virtuaalikoneen IPv6-osoite



## Lähteet:

- Karvinen 2006: Raportin kirjoittaminen. Luettavissa: https://terokarvinen.com/2006/06/04/raportin-kirjoittaminen-4/
- Karvinen 2017: Vagrant Revisited – Install & Boot New Virtual Machine in 31 seconds. Luettavissa: https://terokarvinen.com/2017/vagrant-revisited-install-boot-new-virtual-machine-in-31-seconds/?fromSearch=vagrant
- Karvinen 2023: Create a Web Page Using Github. Luettavissa: https://terokarvinen.com/2023/create-a-web-page-using-github/
- Karvinen 2023: Run Salt Command Locally. Luettavissa: https://terokarvinen.com/2021/salt-run-command-locally/
- Salt Project s.a: Glossary. Luettavissa: https://docs.saltproject.io/en/latest/glossary.html
- Wikipedia s.a. Idempotenssi. Luettavissa: https://fi.wikipedia.org/wiki/Idempotenssi
