# Proiect-TSC

![msedge_Ucl1mEEFf3](https://github.com/user-attachments/assets/82342bde-491b-4181-9ff9-e894b8f3599f)


[bom-table.md](https://github.com/user-attachments/files/19619067/bom-table.md)

Descrierea Funcționalității Hardware
Modulul Microcontroller
Proiectul este bazat pe modulul ESP32-C6-WROOM-1-N8, un microcontroller de ultimă generație care include WiFi 6 și Bluetooth 5. Acest modul oferă performanțe excelente și un consum de energie redus, fiind ideal pentru aplicații IoT.
Senzori

BME680 - un senzor integrat de mediu produs de Bosch Sensortec care măsoară:

Temperatura (între -40°C și +85°C)
Umiditatea relativă (între 0% și 100%)
Presiunea barometrică (între 300hPa și 1100hPa)
Calitatea aerului (prin măsurarea compușilor organici volatili)

Senzorul se conectează la ESP32-C6 prin interfața I2C, utilizând pinii SDA și SCL.
DS3231SN - un Real Time Clock (RTC) de înaltă precizie:

Menține data și ora precisă
Include compensare de temperatură
Interfațare prin I2C
Backup cu baterie pentru păstrarea timpului când alimentarea principală este întreruptă



Sistemul de Alimentare

Încărcător de baterie Li-Ion/Li-Polymer:

Controller dedicat MCP73831T-2ACI-OT
LED indicator pentru starea încărcării
Circuite de protecție pentru baterie


Management Energie:

Regulator LDO XC6220A331MR_G pentru alimentare stabilă 3.3V
Detector de tensiune BD5229G-TR pentru monitorizarea nivelului bateriei
Monitor de baterie MAX17048G_T10 pentru măsurarea precisă a nivelului de încărcare
Circuite de protecție PTC (PFMF.050.2) pentru protecție la supracurent


Convertoare DC-DC:

Inductori de putere (744043680, WE-TPC-4828) pentru conversia eficientă a tensiunii
Diode Schottky (SD0805S020S1R0, MBR0530T3G) pentru circuitele de comutație



Stocare
Sistemul include memoria flash W25Q512JVEIQ de 512Mbit (64MB) conectată prin interfața SPI, oferind:

Spațiu amplu pentru logging de date
Viteză mare de citire/scriere (până la 104 MHz în mod SPI)
Suport pentru modul Quad SPI pentru transfer de date accelerat
Durată de retenție a datelor de peste 20 de ani

Conectivitate

USB Type-C:

Conector USB4110-GF-A pentru încărcare și comunicare
Circuit de protecție USBLC6-2SC6Y pentru protecție ESD
Suport pentru comunicare serială prin UART


Interfață FPC:

Conector FH34SRJ-24S-0.5SH(99) cu 24 de pini
Permite conectarea modulelor de expansiune sau a ecranelor
Accesează multiple interfețe (I2C, SPI, GPIO)


Conectivitate Wireless:

WiFi 6 integrat în modulul ESP32-C6
Bluetooth 5.0 pentru conectivitate la dispozitive mobile
Conector pentru antenă externă 112A-TAAR-R03 pentru îmbunătățirea recepției



Interfața Utilizator

Butoane Tactile:

Buton de boot (KMS231GPLFS) pentru intrarea în modul de programare
Buton de reset pentru repornirea sistemului
Butoane adiționale configurabile de utilizator


LED-uri de Status:

LED pentru indicarea stării de încărcare
LED pentru indicarea activității sistemului
LED-uri configurabile pentru notificări personalizate
[pinout-table.md](https://github.com/user-attachments/files/19619070/pinout-table.md)
| Pin ESP32-C6 | Funcție | Componentă Conectată | Motivația Alegerii |
|--------------|---------|----------------------|---------------------|
| GPIO0 | Boot | Buton Boot (KMS231GPLFS) | Pin dedicat pentru modul boot |
| GPIO1 | TX | USB-UART Bridge | Funcție standard UART |
| GPIO2 | RX | USB-UART Bridge | Funcție standard UART |
| GPIO3 | SDA | Senzor BME680, RTC DS3231 | I2C Data Line - partajat între dispozitive I2C |
| GPIO4 | SCL | Senzor BME680, RTC DS3231 | I2C Clock Line - partajat între dispozitive I2C |
| GPIO5 | MOSI | Memorie Flash W25Q512JVEIQ | SPI Master Out Slave In |
| GPIO6 | MISO | Memorie Flash W25Q512JVEIQ | SPI Master In Slave Out |
| GPIO7 | SCK | Memorie Flash W25Q512JVEIQ | SPI Clock |
| GPIO8 | CS | Memorie Flash W25Q512JVEIQ | SPI Chip Select |
| GPIO9 | BAT_MON_SCL | Monitor Baterie MAX17048G_T10 | I2C Clock dedicat pentru monitorul bateriei |
| GPIO10 | BAT_MON_SDA | Monitor Baterie MAX17048G_T10 | I2C Data Line dedicat pentru monitorul bateriei |
| GPIO11 | CHRG_EN | Controller Încărcare MCP73831T | Control activare încărcare |
| GPIO12 | CHRG_STAT | Controller Încărcare MCP73831T | Citire status încărcare |
| GPIO13 | LED_CTRL | LED status sistem | Control indicator general de stare |
| GPIO14 | USER_BTN | Buton configurabil | Intrare pentru interacțiune utilizator |
| GPIO15 | PWR_CTRL | Tranzistor DMG2305UX_7 | Control alimentare pentru subsisteme |
| GPIO16 | ALERT | RTC DS3231, Monitor Baterie | Semnal de alertă/întrerupere |
| GPIO17-20 | FPC_GPIO[1-4] | Conector FPC | GPIO pentru extensii viitoare |
| GPIO21 | FPC_SPI_CS | Conector FPC | SPI Chip Select pentru dispozitive externe |
| GPIO22 | FPC_SPI_CLK | Conector FPC | SPI Clock pentru dispozitive externe |
| GPIO23 | FPC_SPI_MOSI | Conector FPC | SPI MOSI pentru dispozitive externe |
| GPIO24 | FPC_SPI_MISO | Conector FPC | SPI MISO pentru dispozitive externe |

Calcule de Consum Energetic
Consumul în diferite moduri de operare:

Mod Activ (Toate funcțiile active):

ESP32-C6: ~60mA @ 3.3V
BME680: ~3.7mA în mod activ
DS3231: ~0.15mA
Memorie Flash: ~15mA în operație de scriere
Monitor baterie: ~0.5mA
Circuite adiționale: ~5mA
Total mod activ: ~84.35mA @ 3.3V = 278.35mW


Mod Normal (Funcționare tipică):

ESP32-C6: ~35mA (WiFi activ, procesor la viteză medie)
BME680: ~1mA (măsurători periodice)
DS3231: ~0.15mA
Memorie Flash: ~1mA (operații ocazionale)
Circuite adiționale: ~3mA
Total mod normal: ~40.15mA @ 3.3V = 132.5mW


Mod Sleep (Economisire energie):

ESP32-C6: ~10μA (deep sleep)
BME680: ~0.15μA (sleep)
DS3231: ~0.15mA (activ pentru menținerea timpului)
Circuite adiționale: ~50μA
Total mod sleep: ~0.21mA @ 3.3V = 0.693mW



Durata de viață a bateriei:
Considerând o baterie Li-Po de 1200mAh:

În mod activ: ~14.2 ore
În mod normal: ~29.9 ore
Cu ciclu de funcționare optimizat (10% mod normal, 90% mod sleep): ~8-10 zile

Specificații de Comunicație

Interfața I2C:

Frecvență: 400kHz (Fast Mode)
Adrese dispozitive:

BME680: 0x76 (sau 0x77)
DS3231: 0x68
MAX17048G: 0x36




Interfața SPI:

Frecvență: până la 80MHz pentru memorie flash
Mode: 0 (CPOL=0, CPHA=0)
Comunicare Full-Duplex
Suport pentru transfer rapid de date în mod Quad SPI (pentru memoria flash)


USB:

USB 2.0 Full Speed (12Mbps)
Configurat ca dispozitiv CDC (Communications Device Class)
Permite programare și debugging prin convertor USB-UART



Design PCB și Considerații Mecanice
Dimensiuni PCB:

Dimensiuni totale: 80mm x 40mm
Grosime PCB: 1.6mm
Straturi: 4 (Top, GND, Power, Bottom)

Considerații Termice:

Componentele cu disipare mare de căldură (regulatoare, încărcător baterie) plasate cu zone de disipare adecvate
Vias termice sub ESP32-C6 pentru disiparea căldurii

Layout Componente:

Secțiune RF (antenă WiFi/BT) izolată și optimizată pentru performanță
Separarea circuitelor digitale de cele analogice pentru minimizarea interferențelor
Componente sensibile la zgomot (senzori) plasate departe de surse de interferență

Integrarea ESP32-C6 oferă capabilități WiFi 6 și Bluetooth de ultimă generație
Senzorul BME680 permite monitorizarea completă a parametrilor de mediu
Sistemul de management al energiei optimizat pentru durată lungă de funcționare
Conectorul FPC permite extensibilitate și adăugarea de module viitoare

Perspective de dezvoltare:

Adăugarea unui ecran E-Ink pentru afișare cu consum ultra-redus
Implementarea unui încărcător solar pentru autonomie completă
Dezvoltarea de module de expansiune specifice pentru diverse aplicații
Optimizarea și mai pronunțată a consumului energetic pentru funcționare pe termen lung

Limitări actuale:

Dimensiunile pot fi reduse în versiunile viitoare
Costuri de producție pot fi optimizate prin reducerea numărului de componente
Protecție IP limitată fără carcasă dedicată

Concluzie
Proiectul actual reprezintă o platformă de dezvoltare IoT completă, bazată pe ESP32-C6, cu capabilități extinse de senzori, managementul energiei, și opțiuni de conectivitate. Design-ul hardware a fost realizat cu accent pe eficiență energetică, flexibilitate și fiabilitate, permițând o gamă largă de aplicații de la monitorizarea mediului până la controlul automatizărilor.
