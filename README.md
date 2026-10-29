# Prosjekt nettverk

# Beskrivelse av oppdraget:
I dette prosjektet så har jeg analysert, konfigurert og feilsøkt mitt lokale nettverk som jeg har på skolen:
Videre så har jeg ved bruk av packet tracer satt opp ett virtuelt bedriftsnettverk, der 2 rutere skal kommunisere med hverandre og ha flere avdelinger med DHCP IP-er koblet til hver ruter, og som også skal kommunisere med hverandre og en server med DNS.

# Kravspesifikasjon

Kravspesifikasjon:

* ServerO2 og Klient pc skal kunne kommunisere med hverandre.

* ServerO2 skal ha en fast IP-adresse.

* Klient pc skal kunne få nettverksinnstillinger fra ruter med DHCP

* Klient pc skal bruke minst én tjeneste på ServerO2

* Bedriftsnettverk i packet tracer skal ha flere LAN og Routing mellom dem.

* Klientene i packet tracer skal få riktige nettverksinnstillinger og bruke DNS og HTTP.

* Nettverkene skal testes og resultatene dokumenteres.

* Bedriftsnettverket skal være skalerbart og skal ha mulighet til å kunne bygges ut med en ny avdeling og flere klienter.

# Del 1 – Fysisk Nettverk - Fysisk topologi og utstyr

Tegne nettverk
Jeg koblet først nettverket skissert ut slik:

 <img width="945" height="559" alt="image" src="https://github.com/user-attachments/assets/b53245d7-7e56-495f-864d-eab8272ef564" />
 
Switch port 1 er koblet til port 1 på Ruter1, 

Switch port 2 er koblet til ServerO2.

Klient pc er koblet til Ruter 1 via wifi.

For å få internett på klient pc så bruker jeg en ekstra switch.

Denne bruker Ethernet porten fra veggen, videre så går en kabel til Ludvigs ruter internettport, og en annen til min.

På denne switchen så går port 1 til veggen for internett.

Port 2 går til internettport på Ruter1

Port 3 går til Internettport på Ruteren til Ludvig


# Adresseplan
* Subnett: **255.255.255.0 eller /24**
* Klient PC (statisk via Ethernet): **192.168.1.11**
* Klient PC (dynamisk via WiFi): **192.168.1.100** 
* Server: **192.168.1.10**
* Ruter og Gateway: **192.168.1.1**
  
_(DHCP-spekteret går fra 100 til 199, og endelsen på IP-adressen kan derfor variere i noe av oppgaven)_
# Kommunikasjon mellom klient og server
For å være sikker på at klient og server fikk kommunisert så pinget jeg først til server.

Ping til server fra klient:
<img width="895" height="395" alt="image" src="https://github.com/user-attachments/assets/7b4509f6-ca07-4faa-8a16-139af888c398" />

Etter dette så pinget jeg fra server til klient pc-en

Ping til klient fra server:

<img width="886" height="714" alt="image" src="https://github.com/user-attachments/assets/6263095d-f337-4d21-9016-63a65f12168f" />

Jeg pinget også med den statiske IP-en til klienten via ethernet som var 192.168.1.11 og det funket, dette er via wifi med DHCP IP.

(Request timed out var fra før jeg åpnet for echorequest innbound rules som det  står mer om feilsøkings delen.)

# DHCP

<img width="945" height="429" alt="image" src="https://github.com/user-attachments/assets/ab6e0149-1d7e-4c75-8a1c-bf95431d3326" />

Etter å ha brukt statisk IP med hjelp av Ethernet så skrudde jeg på DHCP. Ruteren er konfigurert, med DHCP.

Her så blir klient pc tildelt en IP mellom 100 og 199.

ServerO1 er gitt en manuell IP adresse. Som er: 192.168.1.10

Siden server er en domain Controller så må IP-en være statisk, eller så får ikke klient og server kommunisert, og funksjoner som GPO vil ikke fungere. i dette tilfellet er også serveren DNS serveren. En DNS bør alltid være statisk siden den fungerer som ankeret i nettverket der alle enhetene kan søke opp adressen for å få kontakt.

Klient PC er gitt en IP-adresse med fra DHCP innenfor det oppsatte spekteret. Som er:

<img width="945" height="221" alt="image" src="https://github.com/user-attachments/assets/03e13f34-3a96-4805-957d-130d3e0e6c81" />

ved å bruke

ipconfig /release 

Så kommuniserte klienten med DHCP tjenesten på ruteren, og ga klienten en ny IP-adresse. 

ipconfig /renew

Denne kommandoen gjør at klienten sender en ny forespørsel til DHCP, og spør om en ny IP-adresse.

Ved å bruke ipconfig/all så resultatet slik ut:

<img width="945" height="425" alt="image" src="https://github.com/user-attachments/assets/6db219b0-748d-4ae0-acd5-b7709fe890ab" />

IP-adressen jeg hadde tidligere ble endret til 100 istedenfor 101, da 100 var ikke leaset ut.

Gateway:

<img width="945" height="348" alt="image" src="https://github.com/user-attachments/assets/233e8d44-6f2a-4922-9dd4-6b0ce9c3e07e" />

Ruter er gitt samme IP- adresse som Gateway. Da ruteren er sluttpunktet på nettverket.

# Fildeling
For å lage en felles mappe så startet jeg med å lage en mappe på skrivebordet, videre så trykket jeg på Properties, og så Advanced sharing.

<img width="944" height="634" alt="image" src="https://github.com/user-attachments/assets/6867c4b2-15cd-41a9-9da2-cb3dc2239792" />

Deretter så la jeg til gruppen everyone, det gjorde jeg ved å åpne permissions og huket av for read and write tilgang for everyone.

_Dette er noe som ideelt sett ikke bør gjøres i et nettverk der det er flere brukere, da dette gir alle fri tilgang til å gjøre endringer, men siden det bare var meg på nettverket så funket denne løsningen._

<img width="363" height="450" alt="Bilde1" src="https://github.com/user-attachments/assets/1235f849-b266-4d85-9420-2b717eb1101e" />

Etter dette så måtte jeg ha tilgang til mappen på klient pc-en.

<img width="369" height="311" alt="image" src="https://github.com/user-attachments/assets/c7f9d589-fa2e-411a-afc8-b1589433e1fe" />

_jeg la til nettverksplasseringen via filutforsker._

<img width="930" height="769" alt="image" src="https://github.com/user-attachments/assets/bffff9f3-bbae-402b-8d54-f982ee7ac928" />

Slik så Windows-stien til mappen ut. Jeg opprettet en mappe i felles som jeg kalte bilder. I denne mappen så kunne jeg dele bilder fra server til klient som hjalp når jeg skrev dokumentasjonen.

192.168.1.10 er IP-adressen på serveren, og siden denne er satt opp statisk, så vet klient pc-en hvor den finner denne mappen hver gang den er koblet på samme nettverk. Dette er en av grunnene til at det er viktig at serveren er på en statisk IP-adresse. 

Slik så resultatet ut:

<img width="456" height="141" alt="image" src="https://github.com/user-attachments/assets/3d1e9834-77cb-49da-bcd0-016b81e769a1" />

_Nettverksmappen i Windows filutforsker._

<img width="764" height="109" alt="image" src="https://github.com/user-attachments/assets/ae31e19a-a318-4ebb-964f-2fad753065ad" />

_Bilder mappen._

<img width="764" height="109" alt="image" src="https://github.com/user-attachments/assets/c1871a65-10b2-4d64-b5d9-77b4bffaf16a" />

_Bilder lagret på server, som klient har tilgang til._

# Nmap, porter

For å sjekke portene på nettverket åpnet jeg CMD, og skrev inn denne komandoen:

nmap 192.168.1.10

<img width="945" height="719" alt="image" src="https://github.com/user-attachments/assets/f412a3f7-e326-4540-a9ff-2abe2326500d" />

_Slik så resultatet ut, her ser vi alle portene som er åpne eller lukkede dersom det er tilfellet._

**445/tcp open  microsoft-ds - SMB**

Dette er en nettverksprotokoll som gjør det mulig for enheter på et nettverk å dele filer slik som jeg har gjort med min fildelingsmappe, og skrivere over nettverket. 

**389/tcp  open  ldap**

Dette er en protokoll som søker i og henter informasjon fra en katalogtjeneste slik som Active Directory. Som sett på bildet ovenfor så er det en protokoll på port 636 som er kalt LDAPSSL. Dette er en kryptert versjon av LDAP-protokollen. 

En åpen port betyr at en tjeneste som for eksempel Microsoft DS eller LDAP lytter etter tilkoblinger. Dette vil si at fildeling via serveren er mulig og at Active Directory kan brukes gjennom nettverket.

# Wireshark

Nå skulle jeg bruke Wireshark til å lese trafikken som skjer når jeg bruker ping til server fra klient-pc, og åpner den delte mappen fra server på klient pc-en.

**Ping til server**

<img width="945" height="125" alt="image" src="https://github.com/user-attachments/assets/ce26720d-5a46-4554-abe9-0f50201d0ecf" />

Som vi ser på denne loggen, så blir først en ping request sendt fra klient pc-en (192.168.1.101) til serveren (192.168.1.10), og får ett svar i retur. Dette repeteres fire ganger siden en ping sender fire request, i mitt tilfelle så fikk jeg reply på alle, men hvis en request ikke fikk en reply så ville dette blitt loggført som en tapt pakke i CMD.

**Åpne den delte mappen fra server**

Etter ping fangsten startet jeg en ny fangst der jeg kunne se trafikken når jeg åpnet den delte mappen fra server på klienten.

<img width="945" height="253" alt="image" src="https://github.com/user-attachments/assets/aa12ecf5-a59c-4da7-a74f-d1b3043d2896" />

Her ser vi at først så spør klienten (create request) serveren om tilgang til å mappen bilder. 
Videre så svarer serveren (create response) og serveren verifiserer brukerens rettigheter, og  gir eventuelt tilgang til mappen etter å ha gått gjennom rettighetene til brukeren.

NMAP skanner Server02  hvor jeg kan se hvilke porter som er blant annet åpne og lukkede. Den kan også gi meg et innblikk i tjenester som er tilgjengelige på server. 
I fangsten hvor jeg åpnet den delte mappen så kunne jeg se protokollen SMB, som jeg valgte ut fra NMAP scanningen tidligere, her kan vi se i praksis et dypere innblikk i hvordan denne protokollen håndterer fildelingtrafikk.

Wireshark, så ser jeg pakke trafikken, der jeg kan se hvor pakkene blir sendt fra og hvor de sendes. Jeg får også muligheten til å observere noe av trafikken på klient. Generelt sett så kan det være begrenset hvor detaljert innholdet i hver individuelle pakke er da mye er kryptert.

Alt i alt så skanner NMAP portene på Server02, der kan jeg se hva som er tilgjengelig og hvilken tilstand. Wireshark derimot ser på pakketrafikken på klienten hvor man kan se på hva som skjer mellom klient og server.

**Videre utforskning**

For å forstå Wireshark enda mer så gjorde jeg en annen test, jeg lagde en mappe på klient pc-en under en Wireshark fangst. 

Først så lagde jeg mappen som vises på fangsten slik:

<img width="945" height="41" alt="image" src="https://github.com/user-attachments/assets/1afba06d-90df-44b8-9de0-c52f31862290" />

_Her så kan vi se at klient gjør en forespørsel sendt til server om å lage en ny mappe, og hvor serveren så svarer på forespørselen til klienten._

<img width="945" height="73" alt="image" src="https://github.com/user-attachments/assets/f7e6672f-68e6-443e-b0ab-903fd7c54ce7" />

Etter dette så kan jeg se under TCP protokollen. At Porten til klienten 58397  brukes mot 445 (SMB). 

Porten 58397 tilhører klienten, og er en «Ephemeral port» dette er en dynamisk port som Windows oppretter for en kortvarig kommunikasjon med porten til tjenesten på serveren i dette tilfellet SMB nevnt tidligere.

Kommunikasjonen følger samme mønster som tidligere der den starter fra klient til server. 

Her sender de pakker som:

<img width="945" height="97" alt="image" src="https://github.com/user-attachments/assets/91997335-9417-47ee-8e03-ba427f3db237" />

_Her kan vi se mer av kommunikasjonen mellom klient og server når det kommer til opprettelse av mappen._

<img width="945" height="30" alt="image" src="https://github.com/user-attachments/assets/47bb3264-8f88-425e-83de-10fb018405fa" />

Etter litt googling så ser det ut til at her, så spør klient om info om filsystemet til server mappen. Informasjon som trolig er relevant og nødvendig før det blir laget en mappe.

<img width="945" height="19" alt="image" src="https://github.com/user-attachments/assets/5780cbd6-7ac6-4283-bdd4-eb4474ded6fd" />

Lenger ned i kommunikasjonen så finner jeg  en henvendelse fra klient til server, her så blir gjort en ny forespørsel om å endre navn på den nye mappen til 123. Noe som samsvarer med det jeg gjorde fra klient pc-en.

Selv om mye av denne trafikken var vanskelig å tolke, så følte jeg at det i hvert fall ga meg et dypere innblikk i hvordan klienten kommuniserer med server i sanntid.

# Del 2 – Packet Tracer

**Oppsett og testing av LAN**

Jeg begynte på oppgaven ved å legge til alle komponentene som oppgaven trengte. Siden jeg var litt usikker på hvordan nettverket skulle se ut, så tok jeg utgangspunkt fra en tidligere oppgave der jeg lagde ett lite kontornettverk.

Som så slik ut:

<img width="945" height="521" alt="image" src="https://github.com/user-attachments/assets/a0b6ea77-713f-4f32-89ca-e81bc98a9d88" />

_Her så ser vi en switch for hver avdeling, hvor hver klient er koblet til switch. Switchene er så koblet til ruter._

Mitt bedriftsnettverk så da slik ut:

<img width="945" height="449" alt="image" src="https://github.com/user-attachments/assets/c12c741c-ff2a-4491-947d-61a3a3ad4a21" />

Jeg fulgte samme utgangspunkt som  det lille kontornettverket på Router1, jeg la bare til server nettverket. Siden Salg skulle ha sin egen avdeling, så måtte jeg også sørge for at de var på en egen ruter kalt router2 som så var koblet til router1.

På den første ruteren jeg valgte så var det ikke nok porter for alle switchene på hovedkontoret, så jeg byttet da til ruter 2911. På denne fikk jeg mulighet til å koble ruteren til alle switchene. 

Det var et nytt problem som oppstod når jeg skulle koble router 1 til router 2, det var at jeg ikke nok porter til å koble disse sammen. Da fant jeg ut at det er mulig å utvide portene på packet tracer ved å gå inn på physical

<img width="945" height="283" alt="image" src="https://github.com/user-attachments/assets/0343e57d-96d3-4960-b208-c8810e0692a8" />

Og her dro jeg inn HWC-4ESW, denne ga meg 4 flere porter. Jeg gjorde det samme på router 2 og slik så fikk jeg koblet sammen nettverket.

<img width="639" height="52" alt="image" src="https://github.com/user-attachments/assets/fc00213f-ed30-4d34-bcc0-4164220ea78a" />

Jeg konfigurerte Gateway på Router1 port 0/0 til Administrasjon IP: 192.168.10.1/24, med no shutdown på slutten, som sørger for at porten blir skrudd på. 
Videre så ga jeg en statisk IP til PC0:

<img width="945" height="114" alt="image" src="https://github.com/user-attachments/assets/7f244b2b-3967-4930-8788-1656994e7ee0" />

For å teste om PC0 fikk kommunisert med Router1 så pinget jeg Router 1 fra PC0.

<img width="684" height="317" alt="image" src="https://github.com/user-attachments/assets/14217d01-918d-4b83-941f-45ae38c92077" />

Noe som funket

Jeg ga videre en statisk IP til PC1, med IP 192.168.10.11, jeg pinget så PC0 fra PC1:

<img width="675" height="336" alt="image" src="https://github.com/user-attachments/assets/9db65b09-7537-41f8-a5b7-c8335f6a4ff2" />

Dette funket også

Jeg gjorde så det samme med support nettverket. Dette var den samme prosessen om over, men med følgende adresser:

_/24 for alle_

Router1 Gateway port 0/1: 
**192.168.20.1**

Support PC2: 
**192.168.20.10**

Support PC3: 
**192.168.20.11**

Etter at jeg hadde satt opp support, så startet jeg med å sette opp server.


Router1 Gateway port 0/2:
**192.168.30.1**

Server0 har IP: 
**192.168.30.10**

Når jeg skulle koble Router1 til Router2 så la jeg merke til at portene som jeg fikk av modulen: **HWIC-4ESW**

Ga meg 4 Ethernet porter som jeg ikke kunne konfigurere gateway på, jeg måtte derfor endre disse portene, jeg fant ut at jeg måtte bytte over til **HWIC-1GE-SFP**, med **GLC-LH-SMD** port, da denne ville la meg putte inn gateway.

Denne porten ville ikke la meg koble til en av de vanlige gigabitportene så jeg måtte koble denne til samme modul på Router2, og sørge for at de var koblet sammen.

Når jeg var ferdig med oppsettet av enhetene så nettverket slik ut:

### Administrasjon

* Nettverk: **192.168.10.0/24**
* Gateway: **192.168.10.1**
* PC0 (statisk): **192.168.10.10**
* PC1 (statisk): **192.168.10.11**

### Support

* Nettverk: **192.168.20.0/24**
* Gateway: **192.168.20.1**
* PC2 (statisk): **192.168.20.10**
* PC3 (statisk): **192.168.20.11**

### Server

* Nettverk: **192.168.30.0/24**
* Gateway: **192.168.30.1**
* Server0 (statisk): **192.168.30.10**

### Salg

* Nettverk: **192.168.40.0/24**
* Gateway: **192.168.40.1**
* PC4 (statisk): **192.168.40.10**
* PC5 (statisk): **192.168.40.11**

### Ruterne (eget nettverk)

* Nettverk: **10.0.0.0/30**
* Router 1 IP-adresse: **10.0.0.1** (for kontakt med Router2)
* Router 2 IP-adresse: **10.0.0.2** (for kontakt med Router1)

# IP-Routing

Etter oppsettet av LAN, så trengte jeg å sette opp IP-routing slik at hovedkontoret vet hvordan det skal kommunisere med salgsavdelingen.

Jeg begynte derfor på router2 å sette opp IP route via:

<img width="728" height="22" alt="image" src="https://github.com/user-attachments/assets/fe5d2f56-f6d5-43a3-a33c-bebed9218e0d" />

Jeg gjorde dette for alle nettverkene på Router1, og dette var resultatet:

<img width="834" height="238" alt="image" src="https://github.com/user-attachments/assets/d24777f8-5f3e-40fa-aa8a-cd1a610bb969" />

Her kan vi se administrasjon, support og server nettverket på Router1.

På Router1 så skrev jeg inn:

<img width="677" height="28" alt="image" src="https://github.com/user-attachments/assets/bba637f9-aba0-4b54-90f9-491f1ec1af99" />

Og slik så det ut i routing tabben i brukergrensesnittet:

<img width="886" height="133" alt="image" src="https://github.com/user-attachments/assets/ebda5334-e076-4547-a1b2-3910977d7ea4" />

Dette gjør at nettverket på hovedkontoret vet hvordan det kan kommunisere med salgsavdelingen, og motsatt.

IP-route fra Router1:

<img width="916" height="586" alt="image" src="https://github.com/user-attachments/assets/d7ad666c-1816-4b76-bf14-bddcc5e6b30f" />

_Nederst så ser vi nettverket på Router2_

IP-route fra Router2:

<img width="945" height="443" alt="image" src="https://github.com/user-attachments/assets/7ee78eb3-6bba-4f44-8689-e54ded9f51be" />

_Her ser vi til høyre for S, Administrasjon, salg og server nettverket på Router1._


Når en pc fra administrasjon nå sender en pakke til salgsavdelingen så sendes pakken videre til gateway på router1. Router1 henvender seg til tabellen (show ip route)

her så  ser vi 192.168.40.0/24, og adressen til Router2.

Siden det samme er gjort for alle nettverkene på router1.

Så vet begge ruterne hvor de skal sende trafikken, og trafikken kan gå begge veier.

**Tracert sammenligning mellom det fysiske og det virtuelle nettverket**

Tracert fra administrasjon klient(router1) til salg klient(router2):

<img width="722" height="208" alt="image" src="https://github.com/user-attachments/assets/967bffdf-697a-4cd2-a8f8-250edfec9ed0" />

_Her  så ser vi at pakken først blir sendt til administrasjon sin gateway på router1, videre så blir pakken sendt til salgsavdelingen sin router2, og fra router2 til klient._

Tracert på det fysiske nettverket:

<img width="855" height="477" alt="image" src="https://github.com/user-attachments/assets/c6eb8bd1-8836-4ca4-acfa-f9b0654af696" />

_Siden klient og server kommuniserer direkte i samme subnett, uten å gå gjennom en gateway på ruter, er det bare ett hopp._

# Oppsett av DHCP

Etter å ha satt opp IP-Routing, så begynte jeg på oppsett av DHCP.

Jeg satt opp først DHCP for Administrasjon nettverket:

<img width="816" height="102" alt="image" src="https://github.com/user-attachments/assets/19e0e9aa-d5de-4bfb-838d-a7ed48664b41" />

Her setter jeg 1-20 som excluded, dette gjør at DHCP ikke kan utdele disse adressene automatisk, de er reserverte for ting som trenger statiske adresser, som gateway og server. I tillegg er det reservert flere adresser enn det som trengs slik at om jeg skulle utvide nettverket i fremtiden med ting som trenger statiske IP-adresser så er det mulig.

<img width="402" height="47" alt="image" src="https://github.com/user-attachments/assets/c1660f0c-8a10-42e6-bbe8-4c43498fd60d" />

Her lager en pool kalt ADMIN, som gjør det mulig å konfigurere nettverket som er oppsatt med DHCP.

<img width="656" height="70" alt="image" src="https://github.com/user-attachments/assets/0c659ac3-e1a9-4e77-b09c-cf72e10aa9fd" />

I DCHP config så spesifiserte jeg nettverket til Administrasjon, og gateway, dette gjør at klientene i ADMIN poolen automatisk får IP, og gateway via DHCP.

Jeg gjorde det samme for support:

<img width="738" height="130" alt="image" src="https://github.com/user-attachments/assets/b7a6252a-cfdd-4f50-8e6e-6db764db7dc7" />

Etter å ha skrudd på DHCP så var dette de nye IP-adressen på maskinene som sett fra Router1:

<img width="864" height="155" alt="image" src="https://github.com/user-attachments/assets/7db64526-a541-4322-ba37-48870d7e1f2f" />

Ved bruk av show ip dhcp binding så ser vi hvilke IP-adresser som ruteren har leaset ut:

<img width="924" height="528" alt="image" src="https://github.com/user-attachments/assets/b49c53e8-f0ee-48c7-b20a-8a41c0fe87f4" />

Ved bruk av show ip dhcp pool så ser vi konfigureringen av IPen innenfor hver navngitte pool hvor jeg ga navnet for hvert nettverk. Ved å bruke disse kommandoene så kan vi se at både ADMIN og SUPPORT har fått utdelt IP på 2 klienter.

## Jeg sjekket hvilke klienter som fikk hva:
### Administrasjon:
* Pc0 fikk IP-en: **192.168.10.21**
* Pc1 fikk IP-en: **192.168.10.22**
### Support:
* Pc2 fikk IP-en: **192.168.20.21**
* Pc3 fikk IP-en: **192.168.20.22**

Jeg lot server nettverket være statisk da det ikke er noen klienter der, og serveren bør ikke ha en dynamisk IP.

På router 2 gjorde jeg det samme for salg:

<img width="824" height="272" alt="image" src="https://github.com/user-attachments/assets/4e75395b-3e5d-4db7-9c26-723da5d40e37" />

<img width="889" height="141" alt="image" src="https://github.com/user-attachments/assets/baa52989-ee86-4ca9-9dc5-6cf67cccb593" />

<img width="945" height="290" alt="image" src="https://github.com/user-attachments/assets/e8d0a21b-c4f2-4d93-bed2-9d4d4f25003c" />

Det ble leaset ut 2 adresser på hver klient i salg:

* Pc4 fikk IP-en: **192.168.40.21**
* Pc5 fikk IP-en: **192.168.40.22**
  
For å teste skalerbarheten på nettet i salg så la jeg til 2 nye klienter.

Ved å endre ipv4 fra statisk til DHCP så ble de 2 nye klientene automatisk tildelt en IP-adresse.

Pc6 fikk IP-en som slutter på 23 og pc 7 fikk IP-en som slutter på 24.

## Test av kommunikasjon med ping

Etter å ha konfigurert DHCP på alle nettverkene utenom server, så sjekket jeg kommunikasjonen på tvers av nettverkene.

Fra support pc3 pinget jeg Salg pc1:

<img width="691" height="291" alt="image" src="https://github.com/user-attachments/assets/85a556ca-e579-4697-b8d4-cc46f667b890" />

Fra server så pinget jeg salg pc2:

<img width="730" height="348" alt="image" src="https://github.com/user-attachments/assets/54a57288-7792-45f4-877c-7ebf71f5b5cb" />




# Oppsett av HTTP og DNS

# Simulation mode

