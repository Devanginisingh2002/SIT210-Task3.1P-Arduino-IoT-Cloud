# SIT210-Task3.1P-Arduino-IoT-Cloud

/*
 *  Student Name: Devangini Singh
 *  Student Id: 2010993042
 *  
 *  Set up the platform for the sensor update which is a temperature sensor.
 */

#include "DHT.h"

#define DHTPIN 7 // Digital pin attached to the DHT sensor

//#define DHTTYPE DHT11 // DHT 11
#define DHTTYPE DHT22 // DHT 22 (AM2302), AM2321
//#define DHTTYPE DHT21 // DHT 21 (AM2301)


// Connect pin 1 (on the left) of the sensor to +5V
// NOTE: If using a board with 3.3V logic like an Arduino Due connect pin 1
// to 3.3V instead of 5V!
// Connect pin 2 of the sensor to whatever your DHTPIN is
// Connect pin 4 (on the right) of the sensor to GROUND
// Connect a 10K resistor from pin 2 (data) to pin 1 (power) of the sensor


// Initialize DHT sensor.
// Note that older versions of this library took an optional third parameter to
// tweak the timings for faster processors. This parameter is no longer needed
// as the current DHT reading algorithm adjusts itself to work on faster procs.
DHT dht(DHTPIN, DHTTYPE);

#include "thingProperties.h"

void setup() {
// Initialize serial and wait for port to open:
  Serial.begin(9600);

 // Serial.println(F("DHTxx test!"));
  pinMode(LED_BUILTIN, OUTPUT);

  dht.begin();

// This delay gives the chance to wait for a Serial Monitor without blocking if none is found
delay(1500);


// Defined in thingProperties.h
initProperties();



// Connect to Arduino IoT Cloud
ArduinoCloud.begin(ArduinoIoTPreferredConnection);

/*
The following function allows you to obtain more information
related to the state of network and IoT Cloud connection and errors
the higher number the more granular information you’ll get.
The default is 0 (only errors).
Maximum is 4
*/
setDebugMessageLevel(2);
ArduinoCloud.printDebugInfo();
}


void loop() {
  
  ArduinoCloud.update();
  delay(2000);
  temp = dht.readTemperature();
  Serial.print("  Temperature: ");
  Serial.print(temp);
  Serial.println("°C ");
  
}
void onLedChange() {
  // Do something
  digitalWrite(LED_BUILTIN, led);
  
}

