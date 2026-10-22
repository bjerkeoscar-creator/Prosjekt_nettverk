# Prosjekt nettverk

# Beskrivelse av oppdraget:
I dette prosjektet så har jeg analysert, konfigurert og feilsøkt mitt lokale nettverk som jeg har på skolen:
Videre så har jeg ved bruk av packet tracer satt opp ett virtuelt bedriftsnettverk, der 2 rutere kommuniserer med hverandre og videre…..

# Del 1 – Fysisk Nettverk

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
Nettverket bruker subnettet 255.255.255.0 eller /24

Klient pc statisk via Ethernet: 192.168.1.11

Klient pc dynamisk via wifi: 192.168.1.100

(DHCP spekteret går fra 100 til 199, og endelsen på IP-adressen kan derfor variere i noe av oppgaven) 

Server: 192.168.1.10 

Ruter og Gateway: 192.168.1.1

# Kommunikasjon mellom klient og server
For å være sikker på at klient og server fikk kommunisert så pinget jeg først til server.

Ping til server fra klient:
<img width="895" height="395" alt="image" src="https://github.com/user-attachments/assets/7b4509f6-ca07-4faa-8a16-139af888c398" />

Etter dette så pinget jeg fra server til klient pc-en

Ping til klient fra server:

<img width="886" height="714" alt="image" src="https://github.com/user-attachments/assets/6263095d-f337-4d21-9016-63a65f12168f" />

Jeg pinget også med den statiske IP-en til klienten via ethernet som var 192.168.1.11 og det funket, dette er via wifi med DHCP IP.

(Request timed out var fra før jeg åpnet for echorequest innbound rules som det  står mer om feilsøkings delen.)
