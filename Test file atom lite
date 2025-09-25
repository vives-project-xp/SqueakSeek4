/*
 * Arduino C++ programma voor M5Stack ATOM Lite
 * Dit programma communiceert met een Pololu 3pi robot.
 * Het haalt de robot uit de demomodus en laat hem vooruit rijden.
 */

#include <M5Atom.h> // M5Stack ATOM bibliotheek

// --- Configuratie ---
// De ATOM Lite gebruikt HardwareSerial poort 2 voor pinnen G22 (TX) en G19 (RX)
// We geven deze een duidelijke naam: Serial3pi
HardwareSerial Serial3pi(2); 
const long BAUDRATE = 115200; // Baudrate voor de 3pi serial slave

// --- 3pi Seriële Commando's (in bytes) ---
// Gebaseerd op de Pololu 3pi User's Guide, Sectie 10.a
const byte CMD_GET_SIGNATURE = 0x81;
const byte CMD_M1_FORWARD    = 0xC1;
const byte CMD_M2_FORWARD    = 0xC5;

// --- Functies ---

void exitDemoMode() {
    Serial.println("Versturen van commando om demomodus te verlaten...");
    Serial3pi.write(CMD_GET_SIGNATURE);
    delay(100); // Wacht even op een eventuele reactie

    // Lees en print de reactie van de 3pi (optioneel)
    if (Serial3pi.available()) {
        String response = Serial3pi.readString();
        Serial.print("Reactie van 3pi ontvangen: ");
        Serial.println(response);
    } else {
        Serial.println("Geen reactie van 3pi. Controleer de verbindingen.");
    }
}

void driveForward(byte speed) {
    // De snelheid moet tussen 0 en 127 zijn.
    // 'constrain' zorgt ervoor dat de waarde binnen dit bereik blijft.
    speed = constrain(speed, 0, 127);

    Serial.print("Rijd vooruit met snelheid: ");
    Serial.println(speed);

    // Stuur commando's voor beide motoren
    Serial3pi.write(CMD_M1_FORWARD);
    Serial3pi.write(speed);
    
    Serial3pi.write(CMD_M2_FORWARD);
    Serial3pi.write(speed);
}

void stopRobot() {
    Serial.println("Robot stoppen...");
    // Stuur commando's met snelheid 0 om de motoren te stoppen.
    driveForward(0);
}

// De setup() functie wordt één keer uitgevoerd wanneer de ATOM opstart.
void setup() {
    // Start de M5Atom hardware (voor LED, etc.)
    M5.begin(true, false, true); // Serial, I2C, LED
    
    // Start de seriële poort voor communicatie met de 3pi
    // ATOM Lite standaard UART pinnen: TX=G22, RX=G19
    Serial3pi.begin(BAUDRATE, SERIAL_8N1, 19, 22); 
    
    Serial.println("M5Stack ATOM Lite gestart.");
    Serial.println("Wacht 2 seconden tot de 3pi is opgestart...");
    delay(2000); // Geef de 3pi de tijd om op te starten

    // Haal de robot uit de demomodus
    exitDemoMode();
    
    delay(1000);

    // Rijd 3 seconden vooruit met een gemiddelde snelheid
    driveForward(80);
    delay(3000);

    // Stop de robot
    stopRobot();

    Serial.println("Programma voltooid.");
}

// De loop() functie wordt continu herhaald, maar we hebben hem hier niet nodig.
void loop() {
    // Laat de LED van de ATOM Lite blauw knipperen om aan te geven dat het programma is voltooid.
    M5.dis.drawpix(0, 0x0000FF); // Blauw
    delay(500);
    M5.dis.clear();
    delay(500);
}
