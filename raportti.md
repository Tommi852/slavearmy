# Raportti orja koneiden teosta

Käytin muuten samaa metodia, kuin kotitehtävä kolmosessa, mutta muokkasin Vagrantfilen koodia hieman, sekä vaihdoin käytettävän distron hieman kevyempään. Vaihdoin myös master palvelimen omaan netissä olevaani palvelimeen.
Sallin myös master palvelimella automaattisen sertifikaattien hyväksynnän lisäämällä /etc/puppet/puppet.conf tiedostoon rivin
```
 [master]
autosign = true
```

Vagrant tiedostossa jouduin laajentamaan automaattisesti korjattavia portteja, sillä ne loppuivat kesken omalla koneellani. Lisäsin myös jokaiseen rauta koneeseen oman satunnaisen numero kirjain yhdistelmän, jotta master ei sekoita samannimisiä tietokoneita keskenään. 

Uusi Vagrantfile näyttää tältä:
```
Vagrant.configure(2) do |config|
  config.vm.box = "ubuntu/trusty64"
  config.vm.provision "shell", path: "slaver"
  config.vm.usable_port_range = (2200..20000)

  r = rand(36**10).to_s(36)

  (1..100).each do |i|
    config.vm.define "slave#{i}" do |slave|
	slave.vm.hostname = "Slave#{i}-#{r}"

    slave.vm.provider "virtualbox" do |vb|
       vb.memory = 180
       vb.linked_clone = true
    end
    end
  end
end
```

Loin myös starter scriptin:
```
#!/bin/bash
cp -r slavearmy/. /home/.
sudo apt-get update
sudo apt-get -y install vagrant virtualbox
vagrant up
```

Jonka laitoin muistitikun juureen. Laitoin muistitikulle myös vagrantfilen ja slaver scriptin kansioon slavearmy, jolloin starter scripti kopioi ne koneelle ja aloittaa samantien, jolloin voin siirtyä seuraavan koneen asennukseen samantien.
Tein muistitikusta myös suoraan ramiin boottavan kuten kotitehtävä kolmosessa, jolloin pystyin koulussa olevilla koneilla ottamaan tikun irti heti scriptin alkamisen jälkeen ja aloittamaan seuraavan koneen orjuutuksen.

Aloitin armeijan luonnin omalta koneelta, jolle laitoin tulemaan 75 virtuaalista orjaa.

Tämän jälkeen menin koululle ja aloin pyörittämään virtuaalikoneita parilla kymmenellä koneella. Jokainen kone pyöritti noin 27-34 virtuaalista konetta.
![Orjia](kuvat/kuva1.jpg)
![Niin paljon orjia](kuvat/kuva2.jpg)


**Loppujen lopuksi sain yhteensä 639 virtuaalista orjaa.**

Logit ja lista sertifikaateista löytyy täältä: https://github.com/Tommi852/slavearmy/tree/master/logit

