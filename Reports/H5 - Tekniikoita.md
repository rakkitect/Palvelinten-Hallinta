# H5 - Tekniikoita

## Tehtävä x) Lue ja tiivistä

### Control Windows with Salt

- Salt-Masterin pitää olla aina vähintään sama tai uudempi versio kuin Salt-Minionin (luulen että tämä pätee myös Linuxilla, ei pelkästään Windowsilla)
- Artikkelin mukaan Saltin uudemmat versiot toimivat Windowsilla paremmin, mutta artikkeli on ~6 vuotta vanha, joten en tiedä pitääkö enää paikkaansa
- Windowsille Salt pitää ladata Salt Projectin verkkosivuilta
- Ymmärtääkseni Salt-komennot toimivat samalla tavalla Windowsilla kuin Linuxilla, ````sudo```` ei tietenkään toimi, vaan PowerShell/Command Prompt pitää käynnistää administrator-oikeuksilla
- Windowsille pystyy asentamaan Salt-Masterin kautta **Chocolatey Gallery**-nimisen paketinhallinnan, jolloin pystyt asentamaan komentorivin avulla paketteja apt-get:in tavoin.
  - esim. ````sudo salt kone-nimi chocolatey.install classic-shell````

## Tehtävä a) Asenna Salt Windowsille

Olen asentanut Saltin Windows-koneelleni jo kurssin alussa täältä: [docs.saltproject.io](https://docs.saltproject.io/salt/install-guide/en/latest/topics/install-by-operating-system/windows.html)

![Salt Windowsilla](https://github.com/rakkitect/Server-Management/blob/main/Images/salt_windows.png)

## Tehtävä b) Tiedonkeruu Windows-koneesta grains.item-komennolla

````salt-call --local grains.items```` tulostaa kaikki paikallisen koneen "jyvät", eli ymmärtääkseni tietoa komponenteista ja tietokoneen arkkitehtuurista. Listattaville asioille on varmasti jokin looginen selitys.
````salt-call --local grains.item```` komentoa käyttämällä pystyy tulostamaan haluamat tietonsa.

Käytin tässä tehtävässä komentoa ````salt-call --local grains.item cpu_model motherboard cpuarch```` joka tulosti seuraavat tiedot:

![grains](https://github.com/rakkitect/Server-Management/blob/main/Images/grains.png)

- cpu_model: Valmistaja on Intel, prosessorin malli on i5-9600K, ja prosessorin kellotaajuus on 3.70GHz
- cpuarch: Prosessorin arkkitehtuuri on AMD64, mikä on yleisin suoritinarkkitehtuuri
- motherboard: Tuloste näyttää emolevyn mallin ja sarjanumeron eri riveillä.

## Tehtävä c) Saltin file-toiminto Windowsilla

Kokeilin file-toimintoa luomalla uuden tiedoston OneDriven kurssikansiooni. Käytin komentoa ````salt-call --local state.single file.managed 'C:\Users\oskuh\OneDrive - Haaga-Helia Oy Ab\Kurssit\Palvelinten Hallinta\H5\tehtava_C.txt'````.

![Windows File](https://github.com/rakkitect/Server-Management/blob/main/Images/windows_file.png)

## Tehtävä d) Find-komento Debianilla

Tätä tehtävää varten jouduin käyttämään ChatGPT:tä selvittääkseni millä komennolla pystyn tulostamaan viimeisimmäksi muokatut tiedostot /etc/-hakemistosta ja kotihakemistostani. Luulen että tämän komennon käyttö käytiin luennolla läpi, mutta en päässyt juuri tälle luennolle.

Käytin komentoa ````sudo find /etc/ ~/ -type f -printf '%TY-%Tm-%Td %TT %p\n' | sort -r | head -n 25````.

- Ajoin komennon sudolla koska ilman sitä joidenkin kansioiden sisältö ei näkynyt
- ````/etc ~/```` osoittavat mihin kansioihin haluan kohdentaa find-komennon
- ````-type f```` täsmentää että haun kohdistuvan tiedostoihin
- ````printf```` määrittää kuinka haluan tulostaa löytyneet tiedostot
  - ````%TY-%Tm-%Td %TT```` määrittää päivämäärän ja kellonajan tulostusmuodon
  - ````%p```` tulostaa polun tiedostoon
  - ````\n```` lisää rivinvaihdon jokaisen tulostetun rivin jälkeen
- ````sort -r```` putkittaa tuloksen "sort"-ohjelmaan, joka järjestää tulokset ajallisesti
- ````head -n 25```` rajaa tulosteen 25 riviin

![Linux find](https://github.com/rakkitect/Server-Management/blob/main/Images/linux_find.png)

## Tehtävä e) Tee Salt-tila, joka asentaa järjestelmään uuden komennon

Loin `foo.sh`-skriptin hakemistoon /srv/salt/:

    #!/usr/bin/bash
    echo moi
    exit

Jonka jälkeen loin `init.sls`-tiedoston hakemistoon `/srv/salt/script`

    /usr/local/bin/foo.sh:
      file.managed:
        - source: salt://foo.sh

Tilan ajo:

![skripti salt-tila](https://github.com/rakkitect/Server-Management/blob/main/Images/scripti_salt_state.png)

Komennon ajo:

![skripti ja käyttäjä](https://github.com/rakkitect/Server-Management/blob/main/Images/scriba_ja_jane.png)

Jälkikäteen poistin 

# Lähteet

- Karvinen, T 2018. Control Windows with Salt. Luettavissa: https://terokarvinen.com/2018/control-windows-with-salt/. Luettu: 26.04.2024
