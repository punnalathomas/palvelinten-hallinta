# Viisikko

## Tiivistelmä

Kysymys: Kuinka Terraform toimii saltstackin kanssa? Käytetäänkö niitä yhdessä kun rakennetaan ja ylläpidetään infrastrukuuria?
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

Miksi Salt ei löydy Debianin vakiovarastosta, vaikka se on yleisesti käytetty?  

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

Minkälaisissa tilanteissa työelämässä komentoja ajetaan lokaalisti yhdellä koneella?  

## Quickstart to Salt

Kuinka Saltilla siis hallitaan useita koneita? Useiden koneiden hallintaan tarvitaan yksi ***Master***-palvelin, joka ohjaa muita ***Slave***-koneita. Hienous piileekin siinä, että vain Master tarvitsee julkisen IP-osoitteen, Slavet (minionit) voivat olla missä tahansa, koska yhteys toimii NAT:in ja palomuurien läpi. Master-palvelimen palomuuriin tulisi puhkaista reikä porteille 4505/tcp ja 4506/tcp, jotta yhteys toimii. Käytännössä Salt Master asennetaan yhdelle koneelle ja tämän jälkeen Salt Minion asennetaan ohjattaville koneille. Minion-koneille kerrotaan myös missä IP-osoitteessa Master sijaitsee. Tämän jälkeen minion-daemonia potkaistaan ja Master hyväksyy Minionin-avaimen, jolloin yhteys on valmis käyttöön. (Karvinen 2018)  

Itseäni kiinnostaa tietää lisää siitä kuinka Master käytännössä löytää Minionit vaikka ne voivat olla "piilossa" siltä verkon yli.  

## Raportin kirjoittaminen

Karvisen (2006) mukaan raportin tulisi olla täsmällinen, helppolukuinen ja toistettava. Olen itse täysin samaa mieltä näistä. Erityisesti raportin tulisi olla helposti toistettavissa, myös henkilöille joilla ei olisi paljon kokemusta aiheesta. Raporttiin tulisi kiinnittää huomiota myös siitä näkökulmasta, että se myös kuvastaa itse tekijää. Hyvin tehty raportti näyttää muille, että olet vakavasti otettava tekijä ja herättää tietynlaisen luottamuksen lukijassa.  

## Debianin asennus

Asensin uuden Linux virtuaalikoneen Karvisen (2021) ohjeilla. Asennuksen yhteydessä latasin seuraavat ohjelmat: bash-completion, ufw, micro, tree ja curl. Asensin myös Guest Additions jolloin sain käyttöön leikepöydän ja paremman resoluution.  

![kuva1](./Pictures/kuva1.png)  

## Saltin asennus

Aloitin asentamalla **wget**:in komennoilla `sudo apt-get update` ja `sudo apt-get install wget`. Wgetin avulla voidaan ladata tiedostoja verkosta (Gnu 2020).  

Ensin tarkastellaan että paketit tulevat oikeasti salt projektilta. Tein kansion saltrepo komennolla `mkdir saltrepo/` ja siirryin sinne komennolla `cd saltrepo/`. Tämän jälkeen latasin tiedostot Karvisen (2025) ohjeiden mukaan komennoilla:  
```
$ wget https://packages.broadcom.com/artifactory/api/security/keypair/SaltProjectKey/public
$ wget https://github.com/saltstack/salt-install-guide/releases/latest/download/salt.sources
```

Seuraavaksi käytin komentoa `less salt.sources` ja sain seuraavanlaisen sisällön komentoriville:  

![kuva2](./Pictures/kuva2.png)  

Sisältö näyttäisi olevan virallisista lähteistä.  

Käytin komentoa `gpg --show-key --with-fingerprint public ` ja halusin tarkistaa, että fingerprint on oikea, mutta en löytänyt enään sivua mistä voisin tarkistaa onko avain oikea. Luotan tällä kertaa siihen, että se on, mutta tuotantoympäristössä tämä ei kävisi.  

![kuva3](./Pictures/kuva3.png)  

Seuraavaksi lisäsin avaimen luotettuihin projekteihin komennoilla:  
```
$ sudo cp public /etc/apt/keyrings/salt-archive-keyring.pgp
$ sudo cp salt.sources /etc/apt/sources.list.d/
```
Tämän jälkeen asensin saltin komennoilla:  
```
$ sudo apt-get update
$ sudo apt-get install salt-minion salt-master
```
Ensimmäinen asennuksen yritys jumitti koneen täysin ja jouduin ottamaan virran kokonaan irti siitä. Tästä lähdetäänkin katsomaan kuinka edetään seuraavaksi. Hyödynsin ChatGPT:tä ja ajoin uudelleen käynnistämisen jälkeen komennon `sudo dpkg --configure -a` tämän pitäisi viimeiställä jumittunut asennus ja se näytti toimivan. Testasin asennusta komennolla `salt --version` ja voidaan huomata että toimii!  

![kuva4](./Pictures/kuva4.png)  

Testasin vielä asennusta Karvisen (2025) ohjeella käyttämällä komentoa `sudo salt-call --local state.single file.managed /tmp/thomas
	file /tmp/hellothomas created` ja se näytti toimivan hyvin, eli tehtiin uusi file hellothomas, koska sitä ei ollut vielä olemassa.  

![kuva5](./Pictures/kuva5.png) 

Asennuksen yhteydessä koneelle on luotu salt-minion, jonka toiminta voidaan varmistaa komennolla `sudo systemctl status salt-minion`.  

![kuva6](./Pictures/kuva6.png)  

Kuvasta voidaan huomata, että minion on toiminnassa. Error viestit johtuvat siitä ettei minion saa yhteyttä salt-masteriin.  

## Tärkeimmät state-funktiot









## Lähteet
GNU. 2020. GNU wget. Luettavissa: https://www.gnu.org/software/wget/. Luettu: 24.10.2025  

Karvinen, T. 2025. Palvelinten Hallinta. Luettavissa: https://terokarvinen.com/palvelinten-hallinta/. Luettu: 22.10.2025  

Karvinen, T. 2025. Install Salt on Debian 13 Trixie. Luettavissa: https://terokarvinen.com/install-salt-on-debian-13-trixie/. Luettu: 22.10.2025  

Karvinen, T. 2021. Run Salt Command Locally. Luettavissa: https://terokarvinen.com/2021/salt-run-command-locally/. Luettu: 22.10.2025  

Karvinen, T. 2021. Install Debian on Virtualbox - Updated 2024. Luettavissa: https://terokarvinen.com/2021/install-debian-on-virtualbox/. Luettu: 23.10.2025  

Karvinen, T. 2018. Salt Quickstart – Salt Stack Master and Slave on Ubuntu Linux. Luettavissa: https://terokarvinen.com/2018/03/28/salt-quickstart-salt-stack-master-and-slave-on-ubuntu-linux/. Luettu: 22.10.2025  

Karvinen, T. 2006. Raportin kirjoittaminen. Luettavissa: https://terokarvinen.com/2006/06/04/raportin-kirjoittaminen-4/. Luettu: 22.10.2025  


