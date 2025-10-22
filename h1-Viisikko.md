# Viisikko

## Tiivistelmä

## Saltin asennus Debian 13

Salt:in avulla voidaan siis hallita yhtä tai useampia Windows- ja Linux koneita. Salt ei ole saatavilla Debianin vakiovarastossa, joten joudumme asentamaan uuden apt-repositorion. Saadaksemme automaattiset päivitykset toimintaan, tarvitsee asentaa unattended-upgrades paketti, vaihtoehtoisesti päivitykset voidaan myös ajaa manuaalisesti komennoilla `sudo apt-get update; sudo apt-get dist-upgrade`. (Karvinen Salt install)  

Apt repository joka asennetaan koostuu vain kahdesta tiedostosta. **PGP-julkinen avain**, eli tällä varmistetaan että binääritiedostoon luotetaan, kun allekirjoitus vastaa tätä avainta. **Sources.list** sisältää URL-osoitteen, mistä binääritiedostot ladataan. (Karvinen Salt install)  

Asennus tiivistettynä:  
1. Asennetaan wget
2. Ladataan repositoryn tiedostot
3. Asennetaan avain ja repository
4. Asennetaan Salt
5. Testataan onnistuiko asennus
(Karvinen Salt install)

## Komentojen ajaminen lokaalisti







## Lähteet
Karvinen, T. 2025. Palvelinten Hallinta. Luettavissa: https://terokarvinen.com/palvelinten-hallinta/. Luettu: 22.10.2025  

Karvinen, T. 2025. Install Salt on Debian 13 Trixie. Luettavissa: https://terokarvinen.com/install-salt-on-debian-13-trixie/. Luettu: 22.10.2025  


