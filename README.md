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

* Bedriftsnettverket skal være skalerbart og skal ha mulighet til å kunne utvides med flere klienter.

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

Nå skulle jeg aktivere HTTP og DNS

Jeg sørget til å begynne med at HTTP var skrudd på, og skrudde så på DNS.

<img width="945" height="252" alt="image" src="https://github.com/user-attachments/assets/665c15d1-be5b-4a11-aee2-c57d7d374f95" />

Her lagde jeg intranett.fjordtek.local som navnet på domenet, og koblet den til  Server IP-en: 192.168.30.10.

I server IP configurator så la jeg til Server DNS:

<img width="945" height="252" alt="image" src="https://github.com/user-attachments/assets/f3a62941-df6a-4ca8-864e-03c02d9a3c7a" />

Her lagde jeg intranett.fjordtek.local som navnet på domenet, og koblet den til  Server IP-en: 192.168.30.10.

I server IP configurator så la jeg til Server DNS:

<img width="945" height="200" alt="image" src="https://github.com/user-attachments/assets/e0a3be2c-ee0f-4047-b961-c73f1a436190" />

Jeg pinget domene fra Server:

<img width="675" height="347" alt="image" src="https://github.com/user-attachments/assets/d2f91df2-a0da-4005-937d-8bd3c08a427c" />

Dette funket.

Jeg prøvde å gjøre det samme fra administrasjon klient.

<img width="945" height="281" alt="image" src="https://github.com/user-attachments/assets/b592722e-13e8-488c-ba4c-76e5c33c9ff3" />

Å pinge serveren direkte funket, men klienten kunne ikke finne domenet.

Jeg måtte legge til en DNS server, men DHCP blokkerte meg fra å gjøre dette manuelt.
Etter litt research så fant jeg ut at jeg måtte gjøre dette med bruk av kommandoen:

dns-server 192.168.30.10

jeg måtte så gjøre dette:

<img width="738" height="131" alt="image" src="https://github.com/user-attachments/assets/9b996eef-d8ca-4ac7-b2de-3e550d330e05" />

For hver pool/avdeling.

Etter dette så måtte jeg ipconfig /release og ipconfig /renew, for å få en ny lease, ettersom at jeg endret innstillinger på DHCP og disse endringene tar ikke effekt før en ny lease har blitt gjort.

Når jeg pinget fra klient på administrasjon nå så funket ping til domenet:

<img width="655" height="292" alt="image" src="https://github.com/user-attachments/assets/1866ec66-307a-4e3e-87be-b5352574cf73" />

Etter å ha gjort dette på alle klientene så var oppsettet av DNS ferdig.

# Simulation mode

Etter å ha satt opp DNS, så testet jeg nettverket med simulation mode.

Først testet jeg kommunikasjonen mellom klienter i samme LAN.

Jeg fikk ikke opp ARP i event list, så jeg måtte først bruke ARP -D i command prompt på klienten jeg skulle sende fra. Dette slettet så ARP cache slik at jeg kunne vise ARP på simulation panelet:

<img width="600" height="498" alt="image" src="https://github.com/user-attachments/assets/4d80f9a5-7c00-494f-b9f7-a811f23e6936" />

_Dette viser en «Simple PDU» (noe som tilsvarer en ping) fra Admin PC0 til Admin PC1_

Her ser vi først ARP der Admin PC0 spør etter MAC-adressen til Admin PC1 da den må koble sammen IP-en til PC1 med MAC-adressen på maskinen.

Etter  dette så går ARP gjennom switchen til Admin PC1 den spør også ruteren da dette blir kringkastet gjennom hele nettverket.

Admin PC1 svarer, og forteller PC0 at den har IP-adressen som PC0 spør etter og sender så videre MAC-adressen tilbake til PC0, etter at PC0 mottar dette så blir dette lagret i cachen på PC0.

Noe som jeg kan se med: **ARP -A**

Dette gjør at hvis en pakke blir sendt senere så er denne MAC-adressen sin kobling til IP-adressen lagret og PC0 trenger ikke å sende en ny ARP.

Hvor lenge maskinen holder på en ARP cache varierer på ulike operativ systemer, men cachen blir slettet når kommunikasjonen er inaktiv over en lengre periode.

Etter at ARP prosessen er ferdig så kan pingen begynne:

<img width="583" height="158" alt="image" src="https://github.com/user-attachments/assets/aededb99-e541-47fc-b51f-dce415895b67" />

_Her ser vi pingen går fra PC0 gjennom switchen til PC1, dette er en request til PC1, etter dette så sender PC1 en reply tilbake til PC0 gjennom switchen._

Etter at jeg testet kommunikasjonen mellom Admin PC0 og Admin PC2, så testet jeg kommunikasjonen mellom Administrasjon og Salg.

Når jeg sendte en «simple PDU» fra Admin PC-0 til Salg PC, så la jeg merke til at pakken ble failed flere ganger, etter ett par forsøk så funket det, men da ble det bare ICMP som var vist i eventlisten.

<img width="617" height="586" alt="image" src="https://github.com/user-attachments/assets/ddd27a03-66f4-4963-9e2e-8fde96d9c718" />

Jeg sjekket PC0, med ARP -A, der kunne jeg se at IP og MAC-adresse koblingen til Salg PC4 har blitt cachet:

<img width="694" height="117" alt="image" src="https://github.com/user-attachments/assets/63f91674-967d-4a44-8dcd-ea2aa399c401" />

Jeg gjorde en ny PDU denne gang til Salg PC5 da denne MAC-adressen ikke er cachet.
Den ble failed på første forsøk og jeg så dette:

<img width="606" height="414" alt="image" src="https://github.com/user-attachments/assets/603d82f6-6dac-4fbf-91c8-19f8ac4ab3ec" />

På andre forsøk så var PDUen successful og  jeg så dette:

<img width="614" height="625" alt="image" src="https://github.com/user-attachments/assets/9d6cbdc3-7dee-425d-a4cb-a18fab83da1f" />

_Her ser vi at trafikken går fra Admin PC0  til admin switch videre så sendes pakken gjennom Router1 sin gateway, til Router2. fra Router2 så vet ikke pakken hvilken port Salg PC7 er på så den går gjennom alle portene. 
Når den finner riktig klient så blir pakken mottatt, Salg pc7 sender reply pakke tilbake denne går gjennom gateway på Router2, og videre til Admin Switch og tilbake til  Admin PC0._

Etter litt googling så lærte jeg at den første pakken blir droppet når den kommer til Router2 da Router2 må cache ARPen, og den dropper da pakken for å gjøre dette.

På det andre forsøket så blir det vellykket da den har fått cachet ARPen, og dermed vet den hvilken MAC-adresse som skal ha pakken.

Åpning av intranettsiden:

<img width="945" height="291" alt="image" src="https://github.com/user-attachments/assets/148658fa-5d12-4109-b56d-2a36b8015ac0" />

_Her ser vi CISCO sin index.html som er lagret på serveren._

<img width="945" height="370" alt="image" src="https://github.com/user-attachments/assets/d5c28aa5-97a9-4d1e-87f0-5a11a8176b3b" />

Jeg endret denne HTML-filen på serveren, og ga den navnet Fjordtek for å matche domenet. Som vi ser på URL-en så er ikke dette domenet sikret med HTTPS, den er derfor utsatt når det kommer til Wireshark fangster, som kan lese trafikken på nettstedet.

# Telnet

Etter at jeg var ferdig med simulation mode, så gjorde jeg oppsett av telnet på router1, hvor målet er å koble seg på Router1 fra Router2

Slik så oppsettet på router1 ut:

<img width="722" height="278" alt="image" src="https://github.com/user-attachments/assets/2b37aef6-e55e-4949-b2f6-ac54205cb9a7" />

Line vty 0 4, _(betyr at enheten tillater 5 samtidige tilkoblinger)_

Videre så ga jeg passordet **12345**

Jeg skrev så inn login, som skrur på login funksjonen.

Etter dette så skrev jeg transport input telnet, med denne kommandoen forteller jeg ruteren at  telnet protokollen kan brukes remote.

Videre så gikk jeg på Router 2, her skrev jeg inn passordet som er **12345**

<img width="358" height="153" alt="image" src="https://github.com/user-attachments/assets/c9061a1f-8673-4189-abed-676d4091f92d" />

_Her ser vi telnet som spør etter passordet_

Når jeg så kom inn etter å ha tastet passordet, så skrev jeg kommandoen: **show ip interface brief**

<img width="942" height="228" alt="image" src="https://github.com/user-attachments/assets/196bc113-435f-4592-853d-4604a458a99a" />

_Dette lar meg gjøre konfigureringer på Router1 fra Router2, på dette bildet så kan jeg se nettverkene på Router1 fra Router2_

Protokollen Telnet er ikke et godt valg for moderne administrasjon fordi den er ukryptert som vil si at data som passord, og konfigureringer du gjør på ruteren som bruker telnet ikke er kryptert, denne dataen kan komme på avveie og det utgjør en sikkerhetsrisiko.
I moderne nettverk så brukes protokollen SSH, denne protokollen er mye sikrere siden den krypterer slik data.


# Akseptansetest – QA

## ServerO2 og Klient pc skal kunne kommunisere med hverandre.
### Test som blir gjennomført: 

For å teste dette så pinger jeg server fra klient pc.

### Forventet resultat:

Jeg forventer at pingen blir velykket, og at det ikke blir noen pakker som går tapt.

### Resultat:

**Bestått**

<img width="844" height="375" alt="image" src="https://github.com/user-attachments/assets/fa497935-855b-4d7b-b38a-81b0803c4852" />



## SERVER01 skal ha fast IP-adresse.
### Test som blir gjennomført

Jeg sjekker IP-en på Server PCen om den er satt inn manuelt.

### Forventet resultat

Jeg forventer at dette er tilfellet.

### Resultat:
**Bestått**

<img width="625" height="711" alt="image" src="https://github.com/user-attachments/assets/0d62d67a-5af9-46e9-a643-66c72e287da3" />


## CLIENT01 skal kunne få nettverksinnstillinger fra ruteren med DHCP.

### Test som blir gjennomført

Jeg kjører ipconfig /all på CMD.

### Forventet resultat

Jeg forventer at ipconfig /all forteller meg at DHCP er konfigurert.

### Resultat:

**Bestått**

<img width="945" height="447" alt="image" src="https://github.com/user-attachments/assets/2c0ba2f4-ff3e-4fb4-9c73-7fa373a5e6f7" />

Her ser jeg DHCP enabled tydelig, men også at en lease har blitt «obtained» da jeg koblet pc-en til nettverket idag, dette forteller meg at IP-en har blitt tildelt via DHCP


## CLIENT01 skal kunne bruke minst én tjeneste på SERVER01.

### Test som blir gjennomført

Jeg sjekker at fildelingen på servero2 funker. Jeg lager derfor en tekstfil kalt serverO2, jeg gjør det samme på klient pcen som jeg kaller klient. Og sjekker mappen etterpå, hvor jeg ser etter ServerO2 på klient-pcen, og klient på serverO2

### Forventet resultat

Jeg forventer at begge får tilgang til filene som har blitt lagt inn.

### Resultat:

**Bestått**

Fra klient så ser jeg serverO2 .txt laget på server.

<img width="945" height="616" alt="image" src="https://github.com/user-attachments/assets/df0e5067-da0f-4cb2-8983-4144b8ce1145" />

Fra serverO2 så ser jeg klient.txt laget på klient-pc-en.

<img width="945" height="533" alt="image" src="https://github.com/user-attachments/assets/37ffaf18-43c4-4bc6-8219-6e43389f91c7" />

## Bedriftsnettverket i Packet Tracer skal ha flere LAN og routing mellom dem.

### Test som blir gjennomført

Her gjør jeg flere kommandoer, jeg bruker show IP router på begge ruterene for å vise routingen. 

Videre så viser jeg en liste der jeg har sendt pakker mellom forskjellige enheter i varierte nettverk for å vise at de kommuniserer med hverandre 

### Forventet resultat

Jeg forventer at hver ruter med show ip route viser hvilken nettverk som er koblet til, og at pakkene blir sendt.

### Resultat:

**Bestått**

Flere LAN:

<img width="945" height="409" alt="image" src="https://github.com/user-attachments/assets/271641f1-ff99-4ab1-90d8-d705ae83b3f5" />

Kommunikasjonen fungerer:

<img width="647" height="273" alt="image" src="https://github.com/user-attachments/assets/9cca1ae1-fb38-4101-95b9-aad9d90ee874" />

Routingen mellom dem:

På Router1:

<img width="870" height="500" alt="image" src="https://github.com/user-attachments/assets/a386bba0-6030-4644-821e-8e6968c8a664" />

_Nederst på bildet kan vi se routingen er satt opp, den viser salgsavdelingen sitt nettverk._

På Router2:

<img width="895" height="472" alt="image" src="https://github.com/user-attachments/assets/a84b5ccf-c1ac-4cf8-ac88-66644325c62c" />

_Her ser vi ved siden av S, Administrasjon, support og Server er satt opp._

## Klientene i Packet Tracer skal få riktige nettverksinnstillinger og kunne bruke DNS og HTTP.

### Test som blir gjennomført

Jeg bruker show IP DHCP binding, for å vise IP-adressene klientene har blitt utdelt.
Jeg viser også at domenet kan nås med web browser fra klient.


### Forventet resultat

Jeg forventer at de blir utdelt riktige IP – adresser, og at domenet fungerer.

### Resultat:

**Bestått**

Klienter er utdelt riktige IP adresser fra DHCP på router1:

<img width="856" height="178" alt="image" src="https://github.com/user-attachments/assets/fa1fed71-11c9-440d-a9d5-24e7a40286b0" />

_Her ser vi at klientene på hovedkontoret har fått utdelt IP via DHCP._

Klientene er utdelt riktige IP adresser fra DHCP på Router2:

<img width="922" height="169" alt="image" src="https://github.com/user-attachments/assets/b2f538c7-4e53-4861-a7dd-bdd314bcc28c" />

_Her ser vi at de 4 klientene på salg har fått utdelt IP via DHCP._

Jeg gikk på en av klient pc-ene på hovedkontoret på web browser skrev jeg in domenet til server som er server ip: 192.168.30.10 og koblet til navnet infranett.fjordtek.local

<img width="945" height="742" alt="image" src="https://github.com/user-attachments/assets/e3a9b7ad-6904-4cb4-9b2a-a93454315374" />

_Som funket._

## Nettverkene skal testes og resultatene dokumenteres.

### Test som blir gjennomført

Jeg sender en PDU pakke fra klient pcer til ruter på hver avdeling.
Videre så sender jeg en pdu pakke fra en klient på hvert nettverk til et annet nettverk.

Til slutt viser jeg at routingen fungerer ved å sende en pdu fra hovedkontor til salg og motsatt.

### Forventet resultat

Jeg forventer at alle pakkene er feilfrie, og at en eventuell feil ville hendt på grunn av at ruteren dropper pakken for å skape en ARP forbindelse. 

### Resultat:

**Bestått**

Ping fra klienter på hovedkontoret til Router1:


<img width="506" height="216" alt="image" src="https://github.com/user-attachments/assets/e287989b-8cd6-482f-bd22-85660a553391" />

_Alle pakkene nådde målet._

Ping fra klienter på Salg til Router2:

<img width="506" height="183" alt="image" src="https://github.com/user-attachments/assets/c3a4a20a-7c76-4555-b978-20cc039ba931" />

_Alle pakkene nådde målet._

Ping fra klienter på ett nettverk til et annet nettverk:

<img width="603" height="142" alt="image" src="https://github.com/user-attachments/assets/5f90bdf7-b9a3-4c48-bb0b-0b551003988c" />

Ping fra klienter på salgskontoret til hovedkontoret:


<img width="603" height="245" alt="image" src="https://github.com/user-attachments/assets/343f63d2-a0c4-47ea-84e4-179d665f2bb8" />

_Som jeg antok så kunne det oppstå en pakke som feilet, dette er antageligvis pga ARP forbindelse som ble prioritert._

Og motsatt:

<img width="608" height="222" alt="image" src="https://github.com/user-attachments/assets/4d6f5533-ecea-4846-b857-880a9a3e8a20" />

## Bedriftsnettverket skal være skalerbart og skal ha mulighet til å kunne utvides med flere klienter.

### Test som blir gjennomført

Jeg legger til 2 nye klienter på Administrasjon og support på hovedkontoret.

### Forventet resultat

Jeg forventer at det går fint, jeg legger til de 2 nye klientene, og det er minimalt med oppsett der jeg bare trenger å sørge for at DHCP blir leaset.

### Resultat:

**Bestått**

<img width="847" height="233" alt="image" src="https://github.com/user-attachments/assets/3ce5733a-ad99-499a-841c-76b1508fbcf0" />

Det gikk fint de 2 nye klientene fikk tildelt en IP-adresse og automatisk satt opp en default gateway og DNS.

De 2 nye klientene fikk kommunisert med hverandre:


<img width="542" height="131" alt="image" src="https://github.com/user-attachments/assets/add9df5e-a638-4731-a901-fa598b6ab3ba" />

# Feilsøking

Problemer som oppstod underveis, og løsninger:

## Fysiske nettverket

Det første problemet som oppstod var at Windows server brukte en auto konfigurert IP, selv om jeg hadde tatt på IP manuelt.

På serveren så fant jeg ut at min manuelle IP adresse på server ble satt som duplicate, og den auto konfigurerte IP-adressen jeg hadde med et uhell tatt på ble satt som preferred.

For å fikse dette så fulgte jeg løsningen jeg fant [her](https://help.webtechpro.com/hc/en-us/articles/8067928508307-Autoconfiguration-IP-Duplicate-IP-address-issue-solution
)

Jeg gikk på registry editor og fulgte denne adressen **HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\ Services\Tcpip\Parameters\Interfaces\ServerenSinAdapter** 

Her lagde jeg verdien **IPAutoconfigurationEnabled** og skrev 0. Dette skrudde av IP auto konfigurasjonen, da denne verdien hvis den ikke ble vist var 1.

Etter dette så restartet jeg Server pc-en.

Jeg gjorde så en ipconfig, og som det står i løsningen så var den autokonfigurerte IP-en nå fjernet, men min manuelt satte in IP-adresse var fortsatt satt som en duplicate.

For å fikse dette måtte jeg gå tilbake til registry editor, jeg fulgte denne adressen da:

**HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\Tcpip\Parameters**

Her skrev jeg inn verdien **ArpRetryCount** , og skrev 0.

Når jeg restartet pcen igjen nå så var serveren sin ip adresse min manuelle IP, og jeg fikk tilbake kontakten med serveren.

## Packet tracer

Ett problem som oppstod når jeg skulle gjøre en power cycle i packet tracer på enhetene mine i packet tracer var at alle konfigureringene jeg hadde gjort på ruterne ble slettet fordi de bare var lagret i RAM. Dette gjorde at ting som Routing og oppsett av IP adresser ble slettet.

Jeg hadde ikke gjort noen store endringer når jeg power cyclet så jeg hadde muligheten til å lukke programmet uten å save for å åpne det igjen.

For å forhindre at dette skjedde neste gang jeg skulle gjøre en power cycle, så eksporterte jeg running config på hver ruter. Videre så loadet jeg dem så på oppstart, når jeg nå resatte nettverket med en power cycle så ble det riktig konfigurert.

# Oppsummering og refleksjon

# Kilder

[Windows server brukte en auto konfigurert IP](https://help.webtechpro.com/hc/en-us/articles/8067928508307-Autoconfiguration-IP-Duplicate-IP-address-issue-solution
)

[Informasjon om ARP](https://www.youtube.com/watch?v=cn8Zxh9bPio)

Kilder jeg brukte for info på om telnet.

[Telnet Geeks](https://www.geeksforgeeks.org/computer-networks/difference-ssh-telnet/)

[Telnet IT Exams](https://itexamanswers.net/configure-router-vty-password-on-cisco-router-switch.html)

Har også brukt KI, for noe forklaring på diverse kommandoer for å hjelpe meg forstå, framfor å vise.

