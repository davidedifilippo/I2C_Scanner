## I2C_Scanner

A volte non si conosce l'indirizzo con cui il sensore risponde alle interrogazioni. Essendo però un codice a 7 bit, 
le combinazioni non sono infinite (solo 127 possibilità).
Si potrebbe provare allora ad interrogare a tutti gli indirizzi possibili e vedere in corrispondenza di quale indirizzo
abbiamo una risposta. Essendo il solo dispositivo collegato sul BUS I2C avremo determinato l'indirizzo del sensore 
sul BUS.

## La libreria di gestione del modulo I2C

Prima di tutto bisogna includere la libreria che permette la gestione dell'interfaccia di comunicazione seriale I2C 
del microcontrollore ATMEGA 328P.

        #include <Wire.h>
 
## Fase di setup

Per poter utilizzare il modulo di comunicazione seriale bisogna attivarlo in modo analogo alla modalità di attivazione 
del modulo di comunicazione seriale:

        Wire.begin();
        
In questo caso lasciamo la velocità al valore di default 100Kb/s dato che è la velocità minima supportata da qualsiasi 
sensore I2C in commercio.

## Loop di interrogazione 

Nel loop interrogo il sensore a tutti gli indirizzi possibili per vedere se riponde:

        Wire.beginTrasmission(address);

        bool success = true;

        success = Wire.endTransmission();

Il sensore, se risponde, invia un codice che vale 0 (false). In questo caso stampo sulla porta di comunicazione seriale 
micrcontrollore --> PC che il dispositivo è stato trovato all'indirizzo esadeciamle 0X..

        Serial.println(address, HEX);

che effettua la stampa dell'indirizzo trovato in formato esadecimale.


 
 
