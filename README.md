# Sintetizator audio folosind Daisy Seed si Plugdata

Scopul proiectului a fost acela de a implementa un instrument pe o platforma embedded specializata pentru domeniul audio: Electrosmith Daisy Seed.

Pentru a facilita interactiunea cu utilizatorul, au fost adaugati diversi senzori de atingere, lumina si miscare, conectati la perifericele I2C si porturile GPIO ale microcontrollerului.

Intreaga implementare si interfatare a componentelor hardware a fost realizata in Plugdata, un mediu de dezvoltare vizual pentru aplicatii audio.

## Dezvoltare

Pentru traducerea patch-ului din PD in cod C++ pentru Daisy Seed este necesara utilizarea Heavy Compiler Collection (hvcc). Acesta permite incarcarea codului odata ce placa de dezvoltare este introdusa in modul "Flash". Bootloader-ul Daisy se ocupa de incarcarea programului in zona de memorie aleasa in procesul de dezvoltare.

Avand in vedere dimensiunile mari ale blocurilor si abstractizarilor utilizate in patch, memoria Flash de 128kB a placii nu a fost suficienta, asa ca am optata pentru utilizarea memoriei SRAM (512 kB). Pentru aceasta facilitate a fost necesara si adaugarea unui Linker care sa faca posibila incarcarea codului in zona respectiva de memorie.

Exista o lista de senzori si functii compatibile cu hvcc si Daisy Seed, alegerea instrumentelor de lucru pentru acest proiect fiind limitata de aceasta.

Senzori utilizati:
- Photorezistor
- Senzor capacitiv MPR121 
- Accelerometru ADXL335

Toti acesti senzori comunica prin diferite periferice ale placii asa ca legaturile dintre ele trebuie definite foarte clar intr-un fisier JSON care contine:
- numele senzorilor utilizati
- perifericul si pinii la care acesta este conectat
- definirea apelurilor catre acestia, care sa poata fi incluse in patch

Odata indepliniti acesti pasi, patch-ul care controleaza mictrocontroller-ul este realizat similar oricarei alte aplicatii, avand insa in vedere  apelurile specifice hvcc si limitarile de memorie a platformei embedded.

## Utilizare

Utilizatorul poate genera sunet atingand cele 6 cabluri conectate la senzorul capacitiv. Fiecare pin al sensorului are mapata o alta nota, setata in patch.

Fotorezistorul, conectat la unul din pinii GPIO ai placii permite filtrarea trece jos a sunetului. Cresterea intensitatii luminoase duce la cresterea frecventei de taiere a filtrului definit in patch. Este luata in considerare si o valoare obisnuita pentru lumina ambientala, astfel incat utilizatorul sa poata controla filtrul utilizand doar propria sursa de lumina.

Accelerometrul permite ajustarea a doi parametrii prin valorile inclinatiilor pe axele X si Y. Pentru a usura miscarea sintetizatorului pe cele doua axe, acesta a fost plasat pe o jumatate de sfera. In prezent, facilitatea se afla inca in lucru.

## Istoric

Etapa 1: Concept, documentare, achizitii hardware
Etapa 2: Testare individuala a senzorilor
Etapa finala: Integrare facilitati intr-un singur patch

## TODO

Implementare control parametrii prin miscare, utilizand accelerometrul ADXL335

## (Link-uri)
Daisy: https://daisy.audio/software/
hvcc: https://wasted-audio.github.io/hvcc/0.16.1/
Plugdata: https://plugdata.org/
Senzori si functii compatibile: https://github.com/electro-smith/pd2dsy/
