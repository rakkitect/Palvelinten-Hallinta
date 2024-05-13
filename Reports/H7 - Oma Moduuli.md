# H7 - Oma Moduuli

Tässä raportissa käyn läpi Tero Karvisen kurssille [Palvelinten Hallinta](https://terokarvinen.com/2024/configuration-management-2024-spring/) lopputehtäväksi tekemäni moduulin.

Tehtävänanto: https://terokarvinen.com/2024/configuration-management-2024-spring/#h7-oma-miniprojekti

Moduuli on rakennettu ja toiminta tarkistettu Debian/Bullseye64 virtuaalikoneella.

Tiedostan että moduuli voisi olla laajempi ja käyttäjäystävällisempi, mutta aikataulujen takia jouduin pitämään tämän suppeana.

## Tekijä ja lisenssi

Osku Heikkinen

GNU General Public License, versio 3

## Tarkoitus

Moduulin ideana on asentaa kirjaston yhteiskäyttökoneelle halutut sovellukset:

### Yleisesti tarvittavat:

- LibreOffice
- Firefox
- Thunderbird

### Media:

- GIMP
- Audacity
- VLC Media Player

Listatut sovellukset poimin näistä artikkeleista:
- https://itsfoss.com/essential-linux-applications/
- https://www.techradar.com/best/best-linux-apps.

## Moduulin rakentaminen

Loin kansion `/srv/salt/installer`, ja sinne tiedoston `init.sls`:

    required:
      pkg.installed:
        - pkgs:
          - firefox-esr
          - libreoffice
          - gimp
          - audacity
          - vlc

Seuraavaksi loin `top.sls`-tiedoston `/srv/salt/`-kansioon:

    base:
      '*':
        - installer

Pidin Katri Laulajaisen (Laulajainen, K. 2018) tekemästä komennosta joka ajaa `top.sls`-tiedoston:

    $ cat run.sh
    salt-call --local state.highstate --file-root /srv/salt

Komento ajetaan komennolla `sudo bash run.sh`, hakemistossa jossa se on.

## Asennus

Asennukseen tarvittavat tiedostot löytyvät [GitHubistani](https://github.com/rakkitect/oma-moduli.git).

Kun Git repo on kloonattu koneelle, `/srv/salt`-kansio kopioidaan omaksi kansiokseen komennolla `sudo cp -r ~/oma-moduli/srv/salt /srv/salt/`.

Tämän jälkeen palataan `oma-moduli`-hakemistoon, ja ajetaan `sudo bash run.sh`.

LibreOffice on isohko paketti, joten asennuksessa kestää jonkin aikaa (~1-2 min)

Lopputuloksena on, että aiemmin listatut ohjelmat ovat asentuneet.

![lopputulos](https://github.com/rakkitect/Server-Management/blob/main/Images/lopputulos.png)

# Lähteet:

Karvinen, T. 2024. Infra as Code - Palvelinten Hallinta 2024. Luettavissa: https://terokarvinen.com/2024/configuration-management-2024-spring/
Karvinen, T. 2023. Salt Vagrant - automatically provision one master and two slaves. Luettavissa: https://terokarvinen.com/2023/salt-vagrant/?fromSearch=salt
Das, A. 2024. Must Have Essential Applications for Desktop Linux Users. Luettavissa: https://itsfoss.com/essential-linux-applications/
Techradar. 2024. Best Linux app of 2024. Luettavissa: https://www.techradar.com/best/best-linux-apps
Laulajainen, K. 2018. Palvelinten hallinta h6 8.5.2018 – Oma miniprojekti Saltilla. Luettavissa: https://katrilaulajainen.wordpress.com/2018/05/10/palvelinten-hallinta-h6-8-5-2018-oma-miniprojekti-saltilla/
