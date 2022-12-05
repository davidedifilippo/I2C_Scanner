## I2C_Scanner

A volte non si conosce l'indirizzo con cui il sensore risponde alle interrogazioni. Essendo però un codice a 7 bit, 
le combinazioni non sono infinite (solo 127 possibilità).
Si potrebbe provare allora ad interrogare il sensore a tutti gli indirizzi a 7 bit possibili e vedere in corrispondenza 
di quale indirizzo abbiamo una risposta. Essendo il solo dispositivo collegato sul BUS I2C avremo determinato l'indirizzo del sensore 
sul BUS.

## La libreria di gestione del modulo I2C

Prima di tutto bisogna includere la libreria che permette la gestione dell'interfaccia di comunicazione seriale I2C 
del microcontrollore ATMEGA 328P.

        #include <Wire.h>
        
        int flag = 1; // diventa falso se trovo il dispositivo (logica negativa)
 
## Fase di setup

Per poter utilizzare il modulo di comunicazione seriale bisogna attivarlo in modo analogo alla modalità di attivazione 
del modulo di comunicazione seriale:

        Wire.begin();
        
In questo caso lasciamo la velocità al valore di default 100Kb/s dato che è la velocità minima supportata da qualsiasi 
sensore I2C in commercio.

## Loop di interrogazione 

 
Nel loop interrogo il sensore a tutti gli indirizzi possibili per vedere se riponde:

        Wire.beginTrasmission(address);
        
con address che assume ad ogni ciclo un valore maggiore partendo da 8 fino al massimo 127. Questa istruzione inserisce l'indirizzo nel buffer di trasmissione del modulo I2C (dimensione 32 Byte) ma non invia nulla. Per svuotare il buffer e inviare l'indirizzo sulla linea SDA bisogna invocare:

        flag = Wire.endTransmission();

Il sensore, se risponde, invia il codice intero 0. In questo caso devo stampo sulla porta di comunicazione seriale 
microcontrollore --> PC che il dispositivo è stato trovato all'indirizzo esadeciamle 0X..

        Serial.println(address, HEX);

che effettua la stampa dell'indirizzo trovato in formato esadecimale. Poi setto un flag booleano che impedisce di andare avanti nella ricerca.

        bool trovato = false;
        
Se al termine della scansione non ho trovato nulla stampo "Dispositivo non trovato, controllare le connessioni e resettare". Poi si blocca l'esecuzione del loop.

 
 
