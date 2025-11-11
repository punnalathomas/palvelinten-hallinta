update: 11.11.2025  

# Lokaali tietokone ja käyttöjärjestelmä
**GPU:** Nvidia RTX 2070  
**Processor:** Intel Core i9-9900K 3.60 Ghz    
**RAM:** 16.0 GB  
**OS:**  Windows 11 Home  

# Virtuaali palvelin
**Template:** Debian GNU/Linux 13 (Trixie)  
**CPU:** 2 core  
**Storage:** 60 GB  

# Pkg-file-service

## Tiivistelmä

Tämän raportin tavoitteet löytyvät Karvisen (2025) Palvelinten hallinta verkkosivulta kohdasta H4.

## Lue ja tiivistä artikkelit

Perinteinen lähestymistapa demonien hallintaan Saltilla:  
1. Asennetaan paketti #pkg.installed
2. Muokataan konfiguraatiota #file.managed
3. Potkaistaan demonia #service.running, lisätään myös watch:, joka varmistaa, että asetustiedoston muuttuessa demoni käynnistetään uudelleen
4. Ajetaan tilaa minioneille #sudo salt '*' state.apply [moduulin nimi]
(Karvinen 2018)

# Tehtävät

Päätin tehdä tehtävät ilman Vagranttia tällä kertaa, eli kirjaudun aiemmin luodulle virtuaalikoneelle ja kokeilen kaikki asetukset lokaalisti.  

## SSHouto

### Käsin tehty

Tarkistin aluksi onko SSH asennettu virtuaalikoneelle ja tyhjää näytti. Tämän jälkeen `sudo apt-get update` -> `sudo apt-get install ssh` -> sudo systemctl status ssh`.   

![kuva53](./Pictures/kuva53.png)  

Seuraavaksi siirryin hakemistossa kohtaan /etc/ssh/ ja avasin sshd_configin microssa komennolla `micro /etc/ssh/sshd_config`. Kävin muuttamassa asetustiedostoista päälle portit 8888 ja 22.  

![kuva54](./Pictures/kuva54.png)  

Koska asetuksia muutettiin on syytä potkaista demonia eli `sudo systemctl restart ssh`.

Tämän jälkeen kirjauduin sisään omalle käyttäjälleni ssh yhteydellä, eli komento `ssh -p 8888 thomas@localhost` ja sisällä ollaan!  

![kuva55](./Pictures/kuva55.png)  

Ennen asennuksen poistoa luon kansion ssh:lle polkuun /srv/salt/ ja kopioin muokkaamani sshd_config tiedoston sinne. `sudo mkdir -p /srv/salt/ssh` ja `sudo cp /etc/ssh/sshd_config /srv/salt/ssh/sshd_config`.  

![kuva56](./Pictures/kuva56.png)  

Tämän jälkeen poistetaan äsken asennettu ssh palvelin komennolla `sudo apt remove --purge openssh-server -y`.  

![kuva57](./Pictures/kuva57.png)  

### Automatisointi




# Lähteet

Karvinen, T. 2025. Palvelinten hallinta. Luettavissa: https://terokarvinen.com/palvelinten-hallinta/#h4-pkg-file-service. Luettu: 11.11.2025  

Karvinen, T. 2018. Pkg-File-Service – Control Daemons with Salt – Change SSH Server Port. Luettavissa: https://terokarvinen.com/2018/04/03/pkg-file-service-control-daemons-with-salt-change-ssh-server-port/?fromSearch=karvinen%20salt%20ssh. Luettu: 11.11.2025  

