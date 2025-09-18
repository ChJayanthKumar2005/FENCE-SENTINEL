//#FenceSentinel#//


#include <LoRa.h>
#include <TinyGPS++.h>
#include <HardwareSerial.h>
const int PIN_ACS = 35;     
const int PIN_DIV = 34;      
const int PIN_TAMPER = 32;  
const int PIN_RESET = 33;     
const int PIN_BUZZER = 27;   
#define LORA_SS   5
#define LORA_RST  14
#define LORA_DIO0 2
HardwareSerial GPSserial(2);
TinyGPSPlus gps;
const int GPS_RX = 16; 
const int GPS_TX = 17; 
int I_baseline = 0;
int V_baseline = 0;
const int I_THRESHOLD = 80;     
const int V_THRESHOLD_DROP = 50; 
const float DIV_RATIO = (float)10.0 / (33.0 + 10.0); 
const float ADC_REF = 3.3;
const int ADC_MAX = 4095;
bool alertActive = false;
void setup() {
  Serial.begin(115200);
  pinMode(PIN_BUZZER, OUTPUT);
  digitalWrite(PIN_BUZZER, LOW);
  pinMode(PIN_TAMPER, INPUT_PULLDOWN);
  pinMode(PIN_RESET, INPUT_PULLDOWN);
  long isum = 0, vsum = 0;
  for (int i = 0; i < 100; i++) {
    isum += analogRead(PIN_ACS);
    vsum += analogRead(PIN_DIV);
    delay(10);
  }
  I_baseline = isum / 100;
  V_baseline = vsum / 100;

  Serial.println("=== Fence Monitor (Offline Version) ===");
  Serial.printf("Baselines: I=%d, V=%d\n", I_baseline, V_baseline);
  GPSserial.begin(9600, SERIAL_8N1, GPS_RX, GPS_TX);
  Serial.println("Starting LoRa...");
  LoRa.setPins(LORA_SS, LORA_RST, LORA_DIO0);
  if (!LoRa.begin(433E6)) {   // Change to 868E6/915E6 if needed
    Serial.println("LoRa init failed!");
    while (1);
  }
  Serial.println("LoRa init OK.");
}

void loop() {
  int iread = analogRead(PIN_ACS);
  int vread = analogRead(PIN_DIV);
  bool tamper = digitalRead(PIN_TAMPER);
  bool resetBtn = digitalRead(PIN_RESET);
  float vout = (vread * ADC_REF) / ADC_MAX;
  float fence_voltage = vout / DIV_RATIO;
  bool i_trigger = (iread - I_baseline) > I_THRESHOLD;
  bool v_trigger = (V_baseline - vread) > V_THRESHOLD_DROP;
  while (GPSserial.available() > 0) {
    gps.encode(GPSserial.read());
  }
  if (resetBtn && alertActive) {
    Serial.println("✅ Reset button pressed, clearing alarm.");
    digitalWrite(PIN_BUZZER, LOW);
    alertActive = false;
  }
  int fenceStatus = 0; // 0=Normal, 1=Tap, 2=Tamper
  if (tamper) {
    fenceStatus = 2;
  } else if (i_trigger || v_trigger) {
    fenceStatus = 1;
  }
  if (fenceStatus > 0 && !alertActive) {
    String reason = (fenceStatus == 2) ? "Tamper detected" : "Tap detected";
    String packet = reason;
    if (gps.location.isValid()) {
      packet += " | Lat:";
      packet += String(gps.location.lat(), 6);
      packet += " Lng:";
      packet += String(gps.location.lng(), 6);
    } else {
      packet += " | GPS:NA";
    }
    packet += " | V=" + String(fence_voltage, 2) + "V";
    packet += " | Iraw=" + String(iread);
    Serial.println("⚠️ ALERT: " + packet);
    LoRa.beginPacket();
    LoRa.print(packet);
    LoRa.endPacket();
    digitalWrite(PIN_BUZZER, HIGH);
    alertActive = true;
  } else {
    if (!alertActive) {
      digitalWrite(PIN_BUZZER, LOW);
    }
  }

  delay(200);
}

