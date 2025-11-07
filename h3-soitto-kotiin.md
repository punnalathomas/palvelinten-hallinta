update: 7.11.2025  

# Lokaali tietokone ja käyttöjärjestelmä
**GPU:** Nvidia RTX 2070  
**Processor:** Intel Core i9-9900K 3.60 Ghz    
**RAM:** 16.0 GB  
**OS:**  Windows 11 Home  

# Virtuaali palvelin
**Template:** Debian GNU/Linux 13 (Trixie)  
**CPU:** 2 core  
**Storage:** 60 GB  

# Infrastructure as code

## Tiivistelmä

Tämän raportin tavoitteet löytyvät Karvisen (2025) Palvelinten hallinta verkkosivulta kohdasta H3.   

## Lue ja tiivistä artikkelit

### Kahden koneen virtuaalinen verkko

Vagrantin avulla voidaan luoda helposti uusia virtuaalikoneita ilman graafista käyttöliittymää. Koneille kirjaudutaan SSH-yhteydellä. Debianissa Vagrant ja Virtualbox asennetaan komennolla `sudo apt-get install vagrant virtualbox` ja Vagrant ohjaa Virtualboxia tässä tapauksessa. (Karvinen 2021)  

Asennuksen jälkeen luodaan uusi kansio projektille komennolla `mkdir twohost/` ja tänne kansioon luodaan Vagrantfile, johon annetaan ohjeet virtuaalikoneiden asennukseen. (Karvinen 2021)  

Koneet käynnnistetään komennolla `vagrant up` ja kirjautuminen sisään tapahtuu komennolla `vagrant ssh [koneen nimi]`. `vagrant exit` kirjautuu koneelta ulos ja `vagrant destroy` tuhoaa virtuaalikoneet. Kun koneet on käynnistetty, on hyvä tarkistaa, että ne voivat pingata toisiaan ja ovat yhteydessä internettiin. (Karvinen 2021)  

### Salt Quickstart

Tämä artikkeli on käyty läpi jo aikaisemmassa tehtävässä h1 viisikko. Kopioin aikaisemmin kirjoittamani tekstin tähän alle:  

Kuinka Saltilla siis hallitaan useita koneita? Useiden koneiden hallintaan tarvitaan yksi Master-palvelin, joka ohjaa muita Slave-koneita. Hienous piileekin siinä, että vain Master tarvitsee julkisen IP-osoitteen, Slavet (minionit) voivat olla missä tahansa, koska yhteys toimii NAT:in ja palomuurien läpi.  

Master-palvelimen palomuuriin tulisi puhkaista reikä porteille 4505/tcp ja 4506/tcp, jotta yhteys toimii. Käytännössä Salt Master asennetaan yhdelle koneelle ja tämän jälkeen Salt Minion asennetaan ohjattaville koneille. Minion-koneille kerrotaan myös missä IP-osoitteessa Master sijaitsee. Tämän jälkeen minion-daemonia potkaistaan ja Master hyväksyy Minionin-avaimen, jolloin yhteys on valmis käyttöön. (Karvinen 2018)  

### Salt Vagrant

Infra as Code, eli meidän tapauksessa: kirjoitetaan tekstitiedostoon koodi, mikä kertoo, minkälaisia virtuaalikoneita haluamme pystyttää ja millä asetuksilla. Top-fileen voidaan määrittää mitkä tilat ajetaan komennolla `sudo salt '*' state.apply`, eli ei tarvitse kirjoittaa jokaista moduulia erikseen, mitkä sijaitsevat top.sls -tiedostossa. (Karvinen 2023)  

## Tehtävät

### Hello Vagrant!

Aloitin tehtävän etsimällä tietoa kuinka saan asennettua Vagrantin Windowsille. Löysin Vagrantin viralliselta sivulta ladattavan tiedoston, eli valitaan täältä: https://developer.hashicorp.com/vagrant/downloads -> Windows Binary download -> AMD64. 

Tiedoston lataamisen jälkeen avasin installerin. Asennus oli erittäin yksinkertainen ja se ei tarjonnut mitään valintoja asennukseen. Asennuksen jälkeen oli aika käynnistää järjestelmä uudelleen.


## Lähteet

Hashicorp. 2025. Install Vagrant. Luettavissa: https://developer.hashicorp.com/vagrant/downloads. Luettu: 7.11.2025  

Karvinen, T. 2025. Palvelinten hallinta. Luettavissa: https://terokarvinen.com/palvelinten-hallinta/#h3-soitto-kotiin. Luettu: 7.11.2025  

Karvinen, T. 2023. Salt Vagrant - automatically provision one master and two slaves. Luettavissa: https://terokarvinen.com/2023/salt-vagrant/#infra-as-code---your-wishes-as-a-text-file. Luettu: 7.11.2025  

Karvinen, T. 2021. Two Machine Virtual Network With Debian 11 Bullseye and Vagrant. Luettavissa: https://terokarvinen.com/2021/two-machine-virtual-network-with-debian-11-bullseye-and-vagrant/. Luettu: 7.11.2025  

Karvinen, T. 2018. Salt Quickstart – Salt Stack Master and Slave on Ubuntu Linux. Luettavissa: https://terokarvinen.com/2018/salt-quickstart-salt-stack-master-and-slave-on-ubuntu-linux/?fromSearch=salt%20quickstart%20salt%20stack%20master%20and%20slave%20on%20ubuntu%20linux. Luettu: 7.11.2025  



