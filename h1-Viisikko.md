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

Normaalisti Salttia käytetään hallitsemaan useita koneita verkon yli, mutta voimme myös testata Saltin käyttöä lokaalisti komentorivillämme. Tarkoituksena on siis harjoitella paikallisesti yhdellä koneella. (Karvinen Salt commands)  

Tärkeimmät state-funktiot:
1. pkg = hallitsee paketteja
2. file = hallitsee tiedostoja
3. service = hallitsee palveluita
4. user = hallitsee käyttäjiä
5. cmd = ajaa yksittäisiä komentoja

Saltin avulla voidaan siis määrittää tietty tila (state), esimerkiksi mitä koneella pitäisi olla, kuten tietty paketti asennettuna. Komento `sudo salt-call --local -l info state.single pkg.installed tree
` asentaa tree-ohjelma, jos sitä ei vielä ole. (Karvinen Salt commands)  

## Quickstart to Salt







## Lähteet
Karvinen, T. 2025. Palvelinten Hallinta. Luettavissa: https://terokarvinen.com/palvelinten-hallinta/. Luettu: 22.10.2025  

Karvinen, T. 2025. Install Salt on Debian 13 Trixie. Luettavissa: https://terokarvinen.com/install-salt-on-debian-13-trixie/. Luettu: 22.10.2025  

Karvinen, T. 2021. Run Salt Command Locally. Luettavissa: https://terokarvinen.com/2021/salt-run-command-locally/. Luettu: 22.10.2025  

Karvinen, T. 2018. Salt Quickstart – Salt Stack Master and Slave on Ubuntu Linux. Luettavissa: https://terokarvinen.com/2018/03/28/salt-quickstart-salt-stack-master-and-slave-on-ubuntu-linux/. Luettu: 22.10.2025  


