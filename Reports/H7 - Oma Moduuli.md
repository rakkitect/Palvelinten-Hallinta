# H7 - Oma Moduuli

Tässä raportissa käyn läpi Tero Karvisen kurssille [Palvelinten Hallinta](https://terokarvinen.com/2024/configuration-management-2024-spring/) lopputehtäväksi tekemäni moduulin.

Moduuli on rakennettu ja toiminta tarkistettu Debian/Bullseye64 virtuaalikoneella.

## Tekijä ja lisenssi

Osku Heikkinen

GNU General Public License, versio 3

## Tarkoitus

Moduulin ideana on hallita kymmentä yhteiskäyttö-konetta yhdeltä master-koneelta, ja asentaa niille halutut sovellukset:

### Yleisesti tarvittavat:

- LibreOffice
- Firefox
- Thunderbird

### Media:

- GIMP
- Audacity
- VLC Media Player

Listatut sovellukset poimin näistä artikkeleista: https://itsfoss.com/essential-linux-applications/ ja https://www.techradar.com/best/best-linux-apps.

## Virtuaaliympäristö

Loin projektia varten uuden virtuaaliympäristön Vagrantia käyttäen. Vagrantfile-tiedoston loin mukaillen Tero Karvisen ohjetta (Karvinen, T. 2023)

    # -*- mode: ruby -*-
    # vi: set ft=ruby :
    # Copyright 2014-2023 Tero Karvinen http://TeroKarvinen.com
    
    $minion = <<MINION
    sudo apt-get update
    sudo apt-get -qy install salt-minion
    echo "master: 192.168.12.3">/etc/salt/minion
    sudo service salt-minion restart
    echo "See also: https://terokarvinen.com/2023/salt-vagrant/"
    MINION
    
    $master = <<MASTER
    sudo apt-get update
    sudo apt-get -qy install salt-master
    echo "See also: https://terokarvinen.com/2023/salt-vagrant/"
    MASTER
    
    Vagrant.configure("2") do |config|
    	config.vm.box = "debian/bullseye64"
    
    	config.vm.define "kone1" do |kone1|
    		kone1.vm.provision :shell, inline: $minion
    		kone1.vm.network "private_network", ip: "192.168.12.100"
    		kone1.vm.hostname = "kone1"
    	end
    
    	config.vm.define "kone2" do |kone2|
    		kone2.vm.provision :shell, inline: $minion
    		kone2.vm.network "private_network", ip: "192.168.12.101"
    		kone2.vm.hostname = "kone2"
    	end
    
      config.vm.define "kone3" do |kone3|
    		kone3.vm.provision :shell, inline: $minion
    		kone3.vm.network "private_network", ip: "192.168.12.102"
    		kone3.vm.hostname = "kone3"
    	end
    
    	config.vm.define "kone4" do |kone4|
    		kone4.vm.provision :shell, inline: $minion
    		kone4.vm.network "private_network", ip: "192.168.12.103"
    		kone4.vm.hostname = "kone4"
    	end
    
      config.vm.define "kone5" do |kone5|
    		kone5.vm.provision :shell, inline: $minion
    		kone5.vm.network "private_network", ip: "192.168.12.104"
    		kone5.vm.hostname = "kone5"
    	end
    
    	config.vm.define "kone6" do |kone6|
    		kone6.vm.provision :shell, inline: $minion
    		kone6.vm.network "private_network", ip: "192.168.12.105"
    		kone6.vm.hostname = "kone6"
    	end
    
      config.vm.define "kone7" do |kone7|
    		kone7.vm.provision :shell, inline: $minion
    		kone7.vm.network "private_network", ip: "192.168.12.106"
    		kone7.vm.hostname = "kone7"
    	end
    
    	config.vm.define "kone8" do |kone8|
    		kone8.vm.provision :shell, inline: $minion
    		kone8.vm.network "private_network", ip: "192.168.12.107"
    		kone8.vm.hostname = "kone8"
    	end
    
      config.vm.define "kone9" do |kone9|
    		kone9.vm.provision :shell, inline: $minion
    		kone9.vm.network "private_network", ip: "192.168.12.108"
    		kone9.vm.hostname = "kone9"
    	end
    
    	config.vm.define "kone10" do |kone10|
    		kone10.vm.provision :shell, inline: $minion
    		kone10.vm.network "private_network", ip: "192.168.12.109"
    		kone10.vm.hostname = "kone10"
    	end
    
    	config.vm.define "tmaster", primary: true do |tmaster|
    		tmaster.vm.provision :shell, inline: $master
    		tmaster.vm.network "private_network", ip: "192.168.12.3"
    		tmaster.vm.hostname = "tmaster"
    	end
    end

## "Herra-Orja"-arkkitehtuurin toiminnan varmistaminen

Hyväksyin orjat herra-koneella komennolla `sudo salt-key -A` ja `Y`

Varmistaakseni arkkitehtuurin toiminnan ajoin komennon `sudo salt '*' test.ping` ja kaikki koneet vastasivat "True".

Tämän jälkeen loin ensimmäisen salt-tilan jolla tuplavarmistan kaiken toimivan:

    $ sudo mkdir -p /srv/salt/hello
    
    $ sudoedit /srv/salt/hello/init.sls
    tmp/test_salt:
      file.managed

Seuraavaksi ajoin tilan kaikille koneille komennolla `sudo salt '*' state.apply hello`

![herra-orja-arkkitehtuuri]

## Moduulin rakentaminen



# Lähteet:

https://terokarvinen.com/2024/configuration-management-2024-spring/
https://terokarvinen.com/2023/salt-vagrant/?fromSearch=salt
https://itsfoss.com/essential-linux-applications/
