# 🌡️ Automated Smart Fan & Mini Climate Station

An Arduino-based temperature and humidity monitoring system built with the **Cytron Maker UNO**. The system reads real-time ambient climate data using a **DHT11 sensor**, displays the live status on a **16x2 I2C LCD screen**, and automatically triggers a **relay-controlled DC cooling fan** with an audible buzzer alert whenever the room temperature exceeds a set threshold (default: **28.0°C**).

---

## 🛠️ Hardware & Components Required

* **Microcontroller:** Cytron Maker UNO (or standard Arduino UNO)
* **Climate Sensor:** DHT11 Temperature & Humidity Sensor Module (KY-015 from 37-in-1 Sensor Kit)
* **Display:** 16x2 Character LCD with I2C Backpack Adapter (`PCF8574`)
* **Actuator Switch:** 5V 1-Channel Relay Module (KY-019)
* **Fan / Motor:** 3V–6V DC Brushed Motor + Fan Blade
* **External Power:** Battery Holder (for the DC motor circuit)
* **Prototyping:** Half-size Breadboard & Male-to-Male / Male-to-Female Jumper Wires
* **Cable:** Micro-USB Data Cable (for programming and 5V board power)

---

## 🔌 Pinout & Wiring Guide
<img width="3000" height="1957" alt="Automated Smart Fan" src="https://github.com/user-attachments/assets/f847d3e2-3c00-4cf3-a183-3194bbdeeabf" />
### 1. Sensor, Relay & Display (Logic Circuit — 5V)

| Component | Component Pin | Maker UNO / Breadboard Connection |
| :--- | :--- | :--- |
| **Maker UNO Power** | `5V` | Breadboard `+` (Red 5V Power Rail) |
| **Maker UNO Ground** | `GND` | Breadboard `-` (Blue Ground Rail) |
| **DHT11 Sensor** | `VCC` (`+`) | Breadboard `+` (5V Rail) |
| **DHT11 Sensor** | `GND` (`-`) | Breadboard `-` (GND Rail) |
| **DHT11 Sensor** | `S` (Data / Out) | **Digital Pin 2** |
| **Relay Module** | `VCC` (`+`) | Breadboard `+` (5V Rail) |
| **Relay Module** | `GND` (`-`) | Breadboard `-` (GND Rail) |
| **Relay Module** | `IN` (Signal) | **Digital Pin 3** |
| **16x2 I2C LCD** | `GND` | Breadboard `-` (GND Rail) |
| **16x2 I2C LCD** | `VCC` | Breadboard `+` (5V Rail) |
| **16x2 I2C LCD** | `SDA` | **Pin A4** (or dedicated `SDA` pin) |
| **16x2 I2C LCD** | `SCL` | **Pin A5** (or dedicated `SCL` pin) |
| **Onboard Piezo Buzzer** | Built-in | **Digital Pin 8** *(Hardwired on Maker UNO)* |

### 2. DC Motor & Battery Pack (High-Current Load Circuit)
> ⚠️ **Important Safety Note:** Never connect both battery `+` and `-` wires directly into the relay screw terminals, as this will short-circuit the battery when the relay activates. Wire the relay in series on the positive wire like a standard light switch:

1. **Battery Negative (`-`)** $\rightarrow$ Connect directly to **DC Motor Terminal 1**.
2. **Battery Positive (`+`)** $\rightarrow$ Connect to the **COM (Middle)** screw terminal on the Relay.
3. **Relay NO (Normally Open)** $\rightarrow$ Connect to **DC Motor Terminal 2**.

---

## 💻 Software & Library Dependencies

Before compiling and uploading the sketch in the **Arduino IDE** or **Arduino Cloud Web Editor**, install the following libraries via the **Library Manager** (`Sketch` $\rightarrow$ `Include Library` $\rightarrow$ `Manage Libraries...`):

1. **DHT sensor library** by *Adafruit* (Install along with the required **Adafruit Unified Sensor** dependency).
2. **LiquidCrystal I2C** by *Frank de Brabander*.
3. **Wire.h** *(Built-in with Arduino IDE)*.

*(Note: If you are using the Cytron Maker UNO for the first time, ensure the **CH340G USB Serial Driver** is installed on your computer).*

---

## 🚀 How It Works

1. **Startup:** Upon boot, the LCD initializes its backlight and displays a startup greeting (`Hello User / Starting up...`) for 2 seconds while the DHT11 sensor stabilizes.
2. **Climate Polling:** Every 2 seconds, the microcontroller queries the DHT11 sensor for relative humidity (`%`) and ambient temperature (`°C`).
3. **Live LCD Readout:** 
   * **Top Row:** Displays real-time temperature and humidity (e.g., `T:27.5°C H:65%`).
   * **Bottom Row:** Displays current fan status (`Fan: ON (Hot!)` or `Fan: OFF (Cool)`).
4. **Automated Fan Trigger:**
   * When temperature $\ge$ **28.0°C**, Digital Pin 3 goes `HIGH`, energizing the relay coil and closing the circuit to spin the DC motor. The Maker UNO's built-in piezo buzzer on Pin 8 emits a short `1500 Hz` alert beep when the fan first kicks on.
   * When temperature drops below **28.0°C**, Digital Pin 3 goes `LOW`, cutting power to the motor.

---

## ⚙️ Customization

You can easily adjust the trigger temperature at the top of the `.ino` sketch file:

```cpp


// Change 28.0 to your preferred trigger temperature in Celsius
const float TEMP_THRESHOLD = 28.0;<img width="3000" height="1957" alt="Automated Smart Fan" src="https://github.com/user-attachments/assets/35ffb725-2fbe-40ed-b09d-00f89783fdf1" />

---

# 🚨 Project 2: Laser Tripwire & Light Security Alarm

A fast-response optical security alarm built with the **Cytron Maker UNO** (or standard Arduino UNO). A continuous laser beam is aimed directly at a **4-pin Photoresistor (Light Sensor) Module**. As long as the beam hits the sensor, the system remains armed and silent. The instant someone crosses the path and breaks the laser beam, the microcontroller triggers an alternating dual-tone police siren on the **piezo buzzer** and flashes a **red warning LED**.

### 📸 Circuit Diagram


<img width="567" height="330" alt="Laser trip wire" src="https://github.com/user-attachments/assets/fbdb8c2c-7465-476a-a7fd-22c7d1e2224d" />


---

## 🛠️ Hardware & Components Required (Project 2)

* **Microcontroller:** Cytron Maker UNO (or standard Arduino UNO)
* **Light Sensor:** 4-Pin Photoresistor / Light Detection Sensor Module (`VCC`, `GND`, `DO`, `AO`)
* **Beam Emitter:** KY-008 650nm Laser Emitter Module (from the 37-in-1 Sensor Kit)
* **Audio Alert:** Piezo Buzzer (External 2-wire buzzer or Maker UNO built-in Pin 8 buzzer)
* **Visual Alert:** 5mm Red LED + 220Ω / 330Ω Current-Limiting Resistor (or Maker UNO built-in Pin 3 LED)
* **Prototyping:** Half-size Breadboard & Male-to-Male / Male-to-Female Jumper Wires

---

## 🔌 Pinout & Wiring Guide (Project 2)

| Component | Component Pin | Arduino UNO / Breadboard Connection |
| :--- | :--- | :--- |
| **Arduino Power** | `5V` | Breadboard `+` (Red 5V Power Rail) |
| **Arduino Ground** | `GND` | Breadboard `-` (Blue Ground Rail) |
| **4-Pin Light Sensor** | `VCC` | Breadboard `+` (5V Rail) |
| **4-Pin Light Sensor** | `GND` | Breadboard `-` (GND Rail) |
| **4-Pin Light Sensor** | `DO` (Digital Out) | **Digital Pin 2** |
| **4-Pin Light Sensor** | `AO` (Analog Out) | **Analog Pin A0** *(Optional — for Serial Monitor calibration)* |
| **Red Warning LED** | Anode (`+` / Bent Leg) | **Digital Pin 3** |
| **Red Warning LED** | Cathode (`-` / Short Leg) | Breadboard `-` (GND Rail) **via 220Ω Resistor** |
| **Laser Emitter Module** | `S` (Signal) | **Digital Pin 4** |
| **Laser Emitter Module** | `-` (`GND`) | Breadboard `-` (GND Rail) |
| **Piezo Buzzer** | Red Wire (`+` / Positive) | **Digital Pin 8** |
| **Piezo Buzzer** | Black Wire (`-` / Negative) | Breadboard `-` (GND Rail) |

---

## 🚀 How It Works

1. **System Arming:** When powered on, **Digital Pin 4** outputs `HIGH` (5V), keeping the laser beam shining continuously onto the face of the photoresistor.
2. **Beam Monitoring:**
   * **Beam Intact (Bright Light):** When the laser hits the photoresistor, the sensor module's onboard `LM393` comparator keeps the Digital Output (`DO`) pin **`LOW` (0)**. Both the buzzer and red LED stay off.
   * **Beam Broken (Darkness):** When an object or person blocks the laser beam, light levels drop sharply and the `DO` pin switches **`HIGH` (1)**.
3. **Intruder Alarm:** Once `Digital Pin 2` reads `HIGH`, the Arduino flashes **Digital Pin 3** (Red LED) and alternates **Digital Pin 8** (Piezo Buzzer) between `1200 Hz` and `1800 Hz` to create an urgent siren effect until the beam is restored.

---

## 💻 Arduino Sketch (Project 2)

```cpp
// Pin Definitions
#define SENSOR_DO_PIN 2  // Sensor Digital Output (DO) -> Pin 2
#define SENSOR_AO_PIN A0 // Sensor Analog Output (AO)  -> Pin A0
#define LED_PIN 3        // Red Warning LED            -> Pin 3
#define LASER_PIN 4      // Laser Emitter Module       -> Pin 4
#define BUZZER_PIN 8     // Piezo Buzzer               -> Pin 8

void setup() {
  Serial.begin(9600);
  
  pinMode(SENSOR_DO_PIN, INPUT);
  pinMode(LED_PIN, OUTPUT);
  pinMode(LASER_PIN, OUTPUT);
  pinMode(BUZZER_PIN, OUTPUT);

  // Turn the laser beam ON continuously
  digitalWrite(LASER_PIN, HIGH);
  digitalWrite(LED_PIN, LOW);
}

void loop() {
  int beamBrokenDigital = digitalRead(SENSOR_DO_PIN);
  int lightLevelAnalog = analogRead(SENSOR_AO_PIN);

  // Print live readings to Serial Monitor for easy calibration
  Serial.print("Light Level (AO): ");
  Serial.print(lightLevelAnalog);
  Serial.print(" | Digital State (DO): ");
  Serial.println(beamBrokenDigital);

  // HIGH = Beam is blocked (Dark) | LOW = Laser is hitting sensor (Bright)
  if (beamBrokenDigital == HIGH) {
    digitalWrite(LED_PIN, HIGH);
    tone(BUZZER_PIN, 1200);
    delay(120);
    
    digitalWrite(LED_PIN, LOW);
    tone(BUZZER_PIN, 1800);
    delay(120);
  } else {
    digitalWrite(LED_PIN, LOW);
    noTone(BUZZER_PIN);
  }

  delay(50);
}
