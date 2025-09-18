# 🔌 ESP32 Pin Configuration Table

| Component                  | Pin on Module         | ESP32 Pin Used   | Notes / Working |
|----------------------------|-----------------------|------------------|-----------------|
| **ACS712 Current Sensor**  | OUT                   | GPIO35 (ADC1_CH7)| Measures current in fence wire. Baseline ~1.65V, shifts on tapping. |
|                            | VCC                   | 3.3V             |                 |
|                            | GND                   | GND              |                 |
| **Voltage Divider**        | Divider Node          | GPIO34 (ADC1_CH6)| Scales 12V fence voltage → ~2.9V (safe ADC). |
|                            | Top Resistor (33kΩ)   | Fence +12V       |                 |
|                            | Bottom Resistor (10kΩ)| GND              |                 |
| **Tamper Switch**          | Pin 1                 | 3.3V             | Push button for box tamper detection. |
|                            | Pin 2                 | GPIO32           | Configured as `INPUT_PULLDOWN`. |
| **Manual Reset Button**    | Pin 1                 | 3.3V             | Push button for alarm reset. |
|                            | Pin 2                 | GPIO33           | Configured as `INPUT_PULLDOWN`. |
| **Buzzer + Driver (2N2222)**| Base (via 1kΩ R)     | GPIO27           | GPIO27 HIGH → buzzer ON. |
|                            | Collector             | Buzzer −         |                 |
|                            | Emitter               | GND              |                 |
|                            | Buzzer +              | 3.3V             | Powered from 3.3V rail. |
| **LoRa SX1278 (Ra-02)**    | MOSI                  | GPIO23           | SPI interface. |
|                            | MISO                  | GPIO19           |                 |
|                            | SCK                   | GPIO18           |                 |
|                            | NSS/CS                | GPIO5            | Chip select. |
|                            | RESET                 | GPIO14           | Module reset. |
|                            | DIO0                  | GPIO2            | Interrupt pin. |
|                            | VCC                   | 3.3V             |                 |
|                            | GND                   | GND              |                 |
| **GPS NEO-6M**             | TX                    | GPIO16 (RX2)     | UART2. |
|                            | RX                    | GPIO17 (TX2)     | UART2. |
|                            | VCC                   | 3.3V / 5V        | Depends on module version. |
|                            | GND                   | GND              |                 |
| **Power Supply (LM2596)**  | VIN+                  | Battery +12V     | Input from 3S Li-ion pack. |
|                            | VIN−                  | Battery GND      |                 |
|                            | VOUT+                 | 3.3V Rail        | Powers ESP32 & modules. |
|                            | VOUT−                 | GND Rail         | Common ground. |
