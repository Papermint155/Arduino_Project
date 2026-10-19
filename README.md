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
