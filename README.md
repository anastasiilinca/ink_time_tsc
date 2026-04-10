[!SCHEMA_BLOC]![alt text](schema_bloc.png)

# Descriere Hardware

## 1. Prezentare generala

Sistemul hardware este construit in jurul microcontrollerului Nordic
nRF52840. Microcontrollerul controleaza comunicatia cu perifericele,
citeste date de la senzori si gestioneaza dispozitivele externe.

Blocuri hardware principale:

-   microcontroller nRF52840
-   display e-paper
-   baterie LiPo
-   incarcator baterie (BQ25180)
-   convertor DC/DC
-   fuel gauge pentru monitorizarea bateriei
-   senzor IMU (BMA421)
-   driver haptic (DRV2605)
-   butoane
-   interfata USB-C
-   interfata SWD pentru debugging

## 2. Microcontroller

Caracteristici principale:

-   CPU ARM Cortex-M4F
-   frecventa maxima: 64 MHz
-   memorie Flash: 1 MB
-   RAM: 256 KB
-	alimentare: 3.3V
-	control asupra tutror perifericelor

## 3. Sistemul de alimentare

### 3.1 Baterie LiPo

Dispozitivul este alimentat de o baterie LiPo.

Parametri tipici:

-   tensiune nominala: 3.7 V
-   tensiune maxima: 4.2 V
-   capacitate: depinde de bateria utilizata

Bateria furnizeaza energie pentru intregul sistem.

### 3.2 Incarcator baterie

Incarcarea bateriei se face folosind circuitul BQ25180.

Functii:

-   incarcare baterie LiPo
-   management energetic
-   reglare tensiune

Incarcatorul primeste alimentare de la conectorul USB-C.

### 3.3 Convertor DC/DC

Convertorul DC/DC RT6160 este utilizat pentru a genera o tensiune
stabila de 3.3 V.

Functie:

-   converteste tensiunea bateriei (3.7 - 4.2 V)
-   genereaza o iesire stabila de 3.3 V

Aceasta tensiune alimenteaza:

-   microcontrollerul
-   senzorii
-   displayul
-   alte periferice

## 4. Interfata USB

Sistemul utilizeaza un conector USB-C.

Functii:

-   intrare alimentare
-   incarcare baterie
-   comunicatie USB cu microcontrollerul

Semnale utilizate:

-   VBUS
-   D+
-   D-
-   CC1
-   CC2

Pentru protectie este utilizat un circuit de protectie ESD pe liniile
USB.

## 5. Display e-paper

Sistemul utilizeaza un display e-paper pentru afisare.

Avantaje:

-   consum foarte redus de energie
-   imaginea ramane vizibila fara alimentare
-   lizibilitate buna in lumina naturala

Comunicarea cu microcontrollerul se face prin interfata SPI.

Semnale:

-   MOSI
-   SCK
-   CS
-   DC
-   RST
-   BUSY

Driverul displayului genereaza tensiunile necesare functionarii panoului
e-paper.

## 6. Fuel gauge

Monitorizarea bateriei este realizata folosind circuitul MAX17048.

Parametri masurati:

-   tensiunea bateriei
-   nivelul de incarcare (state of charge)

Comunicarea cu microcontrollerul se face prin I2C.

Semnale:

-   SCL
-   SDA
-   ALERT

## 7. Senzor IMU

Sistemul include un senzor IMU BMA421.

Tip:

-   accelerometru pe 3 axe

Functii:

-   detectia miscarii
-   detectia orientarii
-   detectia gesturilor

Interfata de comunicatie:

I2C

Semnale:

-   SCL
-   SDA
-   INT1
-   INT2

## 8. Driver haptic

Sistemul include un driver haptic DRV2605.

Functii:

-   generarea vibratiilor
-   efecte haptice configurabile
-   controlul intensitatii vibratiei

Interfata de comunicatie:

I2C

Semnale:

-   SCL
-   SDA
-   TRIG sau EN

## 9. Butoane

Dispozitivul include mai multe butoane conectate la pini GPIO ai
microcontrollerului.

Rol:

-   interactiune cu utilizatorul
-   navigare in meniu
-   controlul functiilor sistemului

Butoanele utilizeaza rezistente pull-up sau pull-down pentru
stabilizarea nivelului logic.

## 10. Estimarea consumului de energie

Valori tipice de consum:

nRF52840:

-   sleep: aproximativ 2 - 5 uA
-   activ: aproximativ 5 - 10 mA
-   transmitere BLE: aproximativ 4.8 mA

Display e-paper:

-   refresh: aproximativ 10 - 20 mA
-   idle: aproape 0 mA

BMA421:

-   aproximativ 14 - 160 uA in functie de mod

MAX17048:

-   aproximativ 50 uA

DRV2605:

-   pana la aproximativ 100 mA in timpul vibratiei

Consum estimat in functionare normala:

MCU: aproximativ 5 mA IMU: aproximativ 0.2 mA Fuel gauge: aproximativ
0.05 mA Display idle: aproximativ 0 mA

Total: aproximativ 5.25 mA

Exemplu de calcul pentru o baterie de 500 mAh:

Durata baterie = 500 mAh / 5.25 mA Durata baterie aproximativ 95 ore

# Utilizarea pinilor microcontrollerului nRF52840

# 1. Interfata USB

Aceasta interfata este utilizata pentru comunicatia cu un calculator si pentru detectarea alimentarii prin USB.

Pinii utilizati:

- **P0.13 – D+**
- **P0.14 – D-**
- **VBUS – detectie alimentare USB**

Rolul pinilor:

- **D+ si D-** realizeaza comunicatia diferentiala USB intre microcontroller si host
- **VBUS** permite detectarea conectarii cablului USB si activarea perifericului USB

# 2. Interfata SPI pentru display e-paper

Displayul e-paper este conectat la microcontroller printr-o interfata **SPI**, deoarece aceasta permite transferuri rapide de date necesare pentru actualizarea imaginii pe ecran.

Pinii utilizati:

- **P0.15 – MOSI**
- **P0.16 – SCK**
- **P0.17 – CS**
- **P0.18 – DC**
- **P0.19 – RST**
- **P0.20 – BUSY**

Rolul fiecarui pin:

- **MOSI (Master Out Slave In)** transmite datele grafice si comenzile catre display
- **SCK** furnizeaza semnalul de clock pentru transferul SPI
- **CS (Chip Select)** selecteaza dispozitivul e-paper pe magistrala
- **DC (Data/Command)** indica daca informatia transmisa este comanda sau date
- **RST** reseteaza controlerul displayului
- **BUSY** indica daca displayul este ocupat cu actualizarea imaginii

SPI a fost ales deoarece displayurile e-paper necesita transferuri relativ mari de date atunci cand imaginea este actualizata.

# 3. Interfata I2C pentru senzori si periferice

Magistrala **I2C** este utilizata pentru conectarea mai multor componente periferice cu consum redus de energie. In acest sistem sunt conectate trei dispozitive pe aceeasi magistrala:

- IMU BMA421
- fuel gauge MAX17048
- driver haptic DRV2605

Pinii utilizati pentru magistrala I2C sunt:

- **P0.26 – SDA**
- **P0.27 – SCL**

Rolul pinilor:

- **SDA** transmite datele pe magistrala I2C
- **SCL** genereaza semnalul de clock

Avantajul folosirii I2C este posibilitatea conectarii mai multor dispozitive pe aceeasi magistrala, fiecare fiind identificat printr-o adresa unica.

# 4. Senzor IMU BMA421

Senzorul IMU BMA421 este conectat la magistrala I2C pentru transferul datelor de acceleratie.

Pe langa liniile I2C, sunt utilizate si pini de intrerupere pentru a semnaliza evenimente.

Pinii utilizati:

- **SDA – P0.26**
- **SCL – P0.27**
- **P1.10 – INT1**
- **P1.11 – INT2**

Rolul pinilor de intrerupere:

- detectia miscarii
- detectia orientarii
- notificarea microcontrollerului atunci cand apare un eveniment

Utilizarea intreruperilor permite microcontrollerului sa ramana in mod sleep si sa fie trezit doar atunci cand este necesar.

# 5. Fuel gauge MAX17048

Circuitul **MAX17048** este utilizat pentru monitorizarea bateriei.

Acesta masoara:

- tensiunea bateriei
- nivelul de incarcare (state of charge)

Pinii utilizati:

- **SDA – P0.26**
- **SCL – P0.27**
- **P0.21 – ALERT**

Rolul pinului ALERT:

- semnalizeaza niveluri critice ale bateriei
- poate genera o intrerupere catre microcontroller

# 6. Driver haptic DRV2605

Driverul haptic **DRV2605** controleaza motorul de vibratie al dispozitivului.

Configurarea si controlul se realizeaza prin magistrala I2C.

Pinii utilizati:

- **SDA – P0.26**
- **SCL – P0.27**
- **P0.22 – TRIG / EN**

Rolul pinului TRIG:

- permite declansarea directa a unui efect haptic printr-un semnal digital
- permite activarea vibratiei fara comunicatie I2C continua

# 7. Butoane

Butoanele utilizatorului sunt conectate la pini GPIO configurati ca intrari digitale.

Pinii utilizati:

- **P1.01 – Button UP**
- **P1.02 – Button DOWN**
- **P1.03 – Button SELECT**

Rolul acestora:

- interactiune cu utilizatorul
- navigarea in meniuri
- selectarea optiunilor

Pinii sunt configurati cu **rezistente pull-up interne**, iar apasarea butonului conecteaza pinul la masa.

# 8. Interfata SWD pentru programare si debugging

Pentru programarea microcontrollerului si debugging se utilizeaza interfata **SWD (Serial Wire Debug)**.

Pinii utilizati:

- **SWDIO**
- **SWDCLK**
- **RESET**
- **GND**
- **3.3V**

Rolul acestei interfete:

- programarea firmware-ului
- debugging in timp real
- acces la registrii microcontrollerului

Aceasta interfata este conectata la un header extern pentru conectarea unui debugger.

# 9. Motivul alegerii pinilor

Alegerea pinilor a fost facuta tinand cont de urmatoarele criterii:

- compatibilitatea cu functiile hardware ale microcontrollerului
- utilizarea interfetelor dedicate (USB, SPI, I2C)
- simplificarea traseelor pe PCB
- posibilitatea utilizarii intreruperilor hardware
- optimizarea consumului de energie

Aceasta organizare permite o arhitectura hardware clara, modulara si eficienta energetic.