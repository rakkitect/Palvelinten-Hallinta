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

![Salt Windowsilla]

## Tehtävä b) Tiedonkeruu Windows-koneesta grains.items-toiminnolla




# Lähteet

- Karvinen, T 2018. Control Windows with Salt. Luettavissa: https://terokarvinen.com/2018/control-windows-with-salt/. Luettu: 26.04.2024
