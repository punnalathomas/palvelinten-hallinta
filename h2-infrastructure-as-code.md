update: 29.10.2025  

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

Tämän raportin tavoitteet löytyvät Karvisen (2025) Palvelinten hallinta verkkosivulta kohdasta H2.  

## Lue ja tiivistä artikkelit

### Hello Salt Infra-as-Code

Ennen itse koodin kirjoittamista luodaan kansio /srv/ hakemistoon. `sudo mkdir -p /srv/salt/[moduulin nimi]`. Moduulin kannattaa nimetä kuvaavasti, esimerkiksi Apachen verkkopalvelinta varten nimi voisi olla `apache2`. (Karvinen 2024)  

Hakemisto /srv/salt/ on se kansio mikä jaetaan orjan koneille master koneelta ja tuosta hakemistosta löytyvä moduuli (esim apache2) sisältää kaikki Apacheen liittyvät koodit, tiedostot ja templatet. Moduulista löytyvä init.sls tiedosto toimii pääsisäänkäyntinä, eli se on ensimmäinen tiedosto joka ajetaan moduulin suorittamisen yhteydessä. (Karvinen 2024)  

Init.sls-tiedostolle voidaan antaa seuraavanlainen sisältö:  
```
/tmp/hellothomas:
  file.managed
```
Eli tässä Saltille annetaan ohjeeksi käsitellä tiedostoa `hellothomas`, joka sijaitsee /tmp/-hakemistossa. Saltin state-funktio file.managed pitää huolen, että haluttu tiedosto on olemassa. Koodiin voisi lisätä vielä lisärivin `- contents: "[sisältö tiedostoon]`, jolloin Salt varmistaisi, että tiedosto hellothomas on /tmp/-hakemistossa ja sen sisältö olisi määritellyn kaltainen. (Karvinen 2024)  

Kun moduuli on luotu, sitä voidaan ajaa lokaalisti komennolla `sudo salt-call --local state.apply [moduulin nimi]`. Tämä tulostaa komentoriville tulosteen, mistä näkee Saltin tekemät mahdolliset muutokset. Lopuksi on hyvä tarkistaa vielä eri työkalulla, että koodimme toimi halutulla tavalla, esimerkiksi `ls /tmp/hellothomas`. (Karvinen 2024)  

### Salt overview YAML

YAML on merkintäkieli. YAML renderöijä muuttaa YAML-datarakenteen Pythonin tietorakenteeksi, jota Salt käsittelee (Salt).  

Perussäännöt:  
1. Data muodostuu **avain**:**arvo** -pareista
2. Ne erotetaan kaksoipisteellä (:) ja yhdellä välilyönnillä
3. Avaimen arvo voi olla tyypiltää esimerkiksi teksti, numero, lista tai sanakirja
4. Avaimet ovat case-sensitive
5. Käytä välilyöntejä, ei tabulaattoria
6. Kommentti alkaa #

YAML kolme peruselementtiä:  

1. Scalars - avain-arvo-parit.  
```
eläin: kissa
hedelmä: omena
```

2. Lista, eli avaimen jälkeen tulee luettelo arvoista. Arvot alkavat yhdysmerkillä (-).  
```
eläin:
  - kissa
  - koira
  - leijona

hedelmä:
  - omena
  - mandariini
```

3. Sanakirja, eli kokoelma avain-arvo-pareja ja listoja.  
```
Päivällinen:
  alkupala: salaatti
  juoma: vesi
  pääruoka:
    - pihvi
    - muusi
    - kastike
  jälkiruoka:
    - suklaakakku
```  
(Salt YAML)  

### Top file















## Lähteet

Karvinen, T. 2025. Palvelinten hallinta. Luettavissa: https://terokarvinen.com/palvelinten-hallinta/#h2-infraa-koodina. Luettu: 29.10.2025  

Karvinen, T. 2024. Hello Salt Infra-as-Code. Luettavissa: https://terokarvinen.com/2024/hello-salt-infra-as-code/. Luettu: 29.10.2025  

Saltproject. Salt overview. Luettavissa: https://docs.saltproject.io/salt/user-guide/en/latest/topics/overview.html#rules-of-yaml. Luettu: 29.10.2025  





