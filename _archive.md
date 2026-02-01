# zero to sensors

This is a fantastic, complete spec list. Because you have the **ADS1115** (for analog sensors) and **Relays** (for the pumps), you have everything you need to build a professional-grade system rather than a "toy" one.

Here is the exact wiring guide for the components you listed.

### **The Wiring Diagram (Mental Model)**

Think of your system in two halves:

1. **The Input (3.3V side):** The Pi talks to the ADS1115 chip, and the chip talks to the Soil Sensors. This is low voltage and safe.
2. **The Output (5V side):** The Pi triggers the Relay, and the Relay handles the high power for the Pumps.

---

### **Step 1: Connect the "Translator" (ADS1115)**

The Raspberry Pi cannot read the soil sensors directly. It needs the ADS1115 to translate the analog signal into digital data over a protocol called I2C.

**Wiring ADS1115 to Raspberry Pi:**

* **VDD**  Pi 3.3V (Pin 1) *<-- Crucial: Use 3.3V so you don't fry the Pi's data lines.*
* **GND**  Pi GND (Pin 6)
* **SCL**  Pi SCL (GPIO 3, Pin 5)
* **SDA**  Pi SDA (GPIO 2, Pin 3)

**Wiring Soil Sensors to ADS1115:**

* **Sensor 1 (Smart) Signal (Yellow/Blue)**  ADS1115 **A0** pin
* **Sensor 2 (Dumb) Signal (Yellow/Blue)**  ADS1115 **A1** pin
* **Sensor Power (Red)**  Pi 3.3V (Breadboard rail)
* **Sensor Ground (Black)**  Pi GND (Breadboard rail)

> **Checkpoint:** At this stage, your Pi can "see" the soil moisture.

---

### **Step 2: Connect the "Muscles" (Relays & Pumps)**

The Pi is too weak to power a pump. It turns on a **Relay** (a switch), and the Relay connects the Pump to power.

**Wiring Relay Module to Pi:**

* **VCC**  Pi 5V (Pin 2 or 4) *<-- Relays need 5V to click.*
* **GND**  Pi GND
* **IN1 (Smart)**  Pi GPIO 17
* **IN2 (Dumb)**  Pi GPIO 27

**Wiring the Pumps (The "High" Voltage Loop):**
This is where students often get confused. Think of the Relay like a light switch on a wall. You have to cut the **positive** wire of the pump and put the relay in the middle.

1. **Power Source:** You can use the Pi's 5V rail for small pumps, but an external 4xAA battery pack is safer. If using Pi power: Connect a wire from Pi 5V to the Breadboard Power Rail.
2. **Pump Ground (Black):** Connect directly to Ground (GND).
3. **Pump Power (Red):** Connect to the **NO** (Normally Open) screw terminal on the Relay.
4. **Relay Common:** Connect the **COM** (Center) screw terminal to your **5V Power Source**.

*Result: Power waits at the COM port. When GPIO 17 goes low, the relay clicks, the gate opens, and power flows from COM to NO, turning on the pump.*

---

### **Step 3: Enable the Hardware on the Pi**

Before your Python code will work, you must tell the Raspberry Pi that it's allowed to use the I2C pins.

1. Open your terminal on the Pi.
2. Type: `sudo raspi-config`
3. Go to **Interface Options**.
4. Select **I2C** and choose **Yes** to enable it.
5. Finish and Reboot.

**Test it:**
Once rebooted, type `ls /dev/i2c*` in the terminal. If you see `/dev/i2c-1`, you are ready to run your code!

### **Updated Python Library Installation**

Based on your specs, you need these libraries to talk to that specific hardware:

```bash
# General GPIO control
sudo apt-get install python3-gpiozero

# Library for the ADS1115 chip
pip3 install adafruit-circuitpython-ads1x15

```


***

Here are the specific **Code Swaps** using the `adafruit-circuitpython-ads1x15` library (for your ADC) and `gpiozero` (for your relays).

### 1. The Setup Swap (Top of your file)

Delete your simulation constants and replace them with this hardware initialization.

```python
import time
import board
import busio
import adafruit_ads1x15.ads1115 as ADS
from adafruit_ads1x15.analog_in import AnalogIn
from gpiozero import OutputDevice

# --- Hardware Setup ---

# 1. Initialize I2C Bus and ADS1115 ADC
i2c = busio.I2C(board.SCL, board.SDA)
ads = ADS.ADS1115(i2c)

# 2. Define Sensor Channels (P0 = Smart, P1 = Dumb)
sensor_smart_channel = AnalogIn(ads, ADS.P0)
sensor_dumb_channel = AnalogIn(ads, ADS.P1)

# 3. Define Pump Relays
# Note: Relays are often "Active Low" (Low signal = ON). 
# We set active_high=False so .on() actually turns it on.
pump_smart = OutputDevice(17, active_high=False, initial_value=False)
pump_dumb = OutputDevice(27, active_high=False, initial_value=False)

# --- Calibration Constants ---
# You must test these numbers! Put a sensor in air, then in water, and record the values.
# Capacitive sensors usually read HIGH when dry and LOW when wet.
DRY_VALUE = 25000  # Example: Value in dry air
WET_VALUE = 10000  # Example: Value in cup of water

```

---

### 2. The `read_sensors` Swap

This replaces your random number generator with real physics.

```python
def read_sensors(state=None):
    """
    Reads raw voltage data from ADS1115, maps it to percentage 0-100%,
    and returns the formatted dictionary.
    """
    # 1. Read Raw Data (0 to 32767 usually)
    raw_smart = sensor_smart_channel.value
    raw_dumb = sensor_dumb_channel.value

    # 2. Map Raw Values to Percentage
    # Formula: (raw - dry) / (wet - dry) * 100
    # We use a helper function to keep it clean
    def map_to_pct(raw_val):
        if DRY_VALUE == WET_VALUE: return 0.0 # Avoid divide by zero
        pct = (raw_val - DRY_VALUE) / (WET_VALUE - DRY_VALUE) * 100
        return max(0.0, min(100.0, pct)) # Clamp between 0-100

    pct_smart = map_to_pct(raw_smart)
    pct_dumb = map_to_pct(raw_dumb)

    # Debugging print (optional, helps you see if sensors are working)
    # print(f"DEBUG: Smart Raw: {raw_smart} ({pct_smart:.1f}%) | Dumb Raw: {raw_dumb} ({pct_dumb:.1f}%)")

    # 3. Return Data (State is not needed for hardware, but kept for compatibility)
    return state, {
        'Soil_Moisture_Smart': round(pct_smart, 2),
        'Soil_Moisture_Dumb': round(pct_dumb, 2)
    }

```

---

### 3. The `run_pump` Swap

This replaces the print statements with actual relay triggers.

```python
def run_pump(system_name, state, water_tracker):
    """
    Turns on the physical relay for PUMP_DURATION seconds.
    """
    # Calculate volume based on flow rate (Calibration required for accuracy!)
    water_used = PUMP_DURATION * WATER_PER_SECOND
    
    print(f"   >>> HARDWARE: Initiating Pump {system_name} for {PUMP_DURATION}s...")

    if system_name == 'Smart':
        pump_smart.on()         # Click Relay ON
        time.sleep(PUMP_DURATION)
        pump_smart.off()        # Click Relay OFF
        
        # Update Trackers
        water_tracker['smart_total'] += water_used
        water_tracker['smart_events'] += 1

    elif system_name == 'Dumb':
        pump_dumb.on()
        time.sleep(PUMP_DURATION)
        pump_dumb.off()
        
        water_tracker['dumb_total'] += water_used
        water_tracker['dumb_events'] += 1
    
    print(f"   >>> HARDWARE: Pump {system_name} finished.")
    return water_used

```

### Critical Step: Calibration Script

Before you run the main code, you **must** find your specific `DRY_VALUE` and `WET_VALUE`, or your percentages will be wrong.

Create a separate file called `calibrate.py` and run it once:

```python
import time
import board
import busio
import adafruit_ads1x15.ads1115 as ADS
from adafruit_ads1x15.analog_in import AnalogIn

i2c = busio.I2C(board.SCL, board.SDA)
ads = ADS.ADS1115(i2c)
chan = AnalogIn(ads, ADS.P0) # Connect one sensor to pin A0

print("Reading sensor values... Press Ctrl+C to stop.")
while True:
    print(f"Raw Value: {chan.value} | Voltage: {chan.voltage:.2f}V")
    time.sleep(0.5)

```

1. **Dry:** Hold the sensor in the air. Write down the "Raw Value". This is your `DRY_VALUE`.
2. **Wet:** Dunk the sensor in a cup of water (up to the white line). Write down the "Raw Value". This is your `WET_VALUE`.

Update the constants in your main script with these numbers!

# Snap Peas

Growing **Super Sugar Snap Peas** (*Pisum sativum var. Macrocarpon*) indoors in pots requires more frequent but smaller volumes of water compared to outdoor garden beds. Indoors, air is typically drier, and pots have limited soil volume to hold moisture.

For a standard **12-inch (5-gallon) pot** containing 2–3 Super Sugar Snap plants, you should aim for approximately **1,500 mL to 2,000 mL per week**.

### Weekly Watering Breakdown (Indoor Pots)

Since you are indoors, do not pour the entire weekly amount at once. Indoor pots lack the natural drainage of the earth and are prone to root rot if they sit in "standing" water.

* **Standard Schedule:** **~500 mL** every 2–3 days.
* **During Flowering/Podding:** Increase to **~750 mL** every 2 days. This variety is "heavy-bearing" and will drop its flowers if the soil dries out even once.

---

### Critical Indoor Care Factors

Because Super Sugar Snaps are vining plants that can reach 5–6 feet, their "thirst" changes rapidly as they grow.

| Phase | Estimated mL/Week | Focus |
| --- | --- | --- |
| **Seedling (0-4")** | **500 mL** | Keep the top inch of soil "wrung-out sponge" damp. |
| **Climbing (Vines)** | **1,500 mL** | Consistent moisture to support the rapidly growing stem and tendrils. |
| **Harvest (Pods)** | **2,000+ mL** | Maximum water needed to make the "Sugar Snap" pods juicy and sweet. |

### 3 Rules for Indoor Success

1. **The "Finger Test" is Law:** Before adding your mLs, stick your finger 1 inch into the soil. If it's damp, wait. If it's dry, water. Indoor evaporation varies wildly based on your home's heating/AC.
2. **Avoid the Leaves:** Indoor air circulation is lower than outdoors. Water only at the base of the plant to prevent **Powdery Mildew**, which looks like white flour on the leaves and can kill indoor peas quickly.
3. **Temperature Alert:** Super Sugar Snaps love the "cool" part of the house. Keep them away from radiators or heat vents; if the soil gets too warm ( or ), the plant will stop producing peas regardless of how much water you give it.

**Would you like me to help you set up a lighting schedule or recommend a trellis size for your indoor pots?**



---

This is an excellent setup for a "Smart Irrigation" experiment. Comparing a **fixed timer** (Control) against an **automated threshold** (Variable) is the standard way to prove water efficiency in agricultural science.

However, your specific thresholds (**30% to 80%**) may be slightly risky for "Super Sugar Snap" peas, which are sensitive to both drought and "wet feet" (root rot).

### Analysis of Your Thresholds

* **The "Low" (30%):** This is on the edge of "danger zone" for peas. Most vegetables prefer to stay above **40% moisture**. At 30%, a pea plant’s shallow roots may already be experiencing "water stress," which can lead to flower drop.
* **The "High" (80%):** This is very high for a pot. 80% is near **Field Capacity** (where the soil is holding all the water it possibly can). In a pot with limited drainage, staying at 80% for too long can starve the roots of oxygen.

---

### Recommended "Smart" Settings

For a more successful experiment, I suggest narrowing the window slightly to keep the "Smart" group in its "Optimal Zone":

| Setting | Recommended % | Reason |
| --- | --- | --- |
| **Lower Limit (Start)** | **45%** | Keeps the plant in the "comfort zone" so it never stops growing. |
| **Upper Limit (Stop)** | **70%** | Provides plenty of water without "drowning" the roots in the bottom of the pot. |

### How to Improve the Experiment

To make your data more "scientific," consider tracking these three metrics for both groups:

1. **Water Consumption:** Keep a log of exactly how many mLs the "Smart" group uses vs. the "Timer" group. (The goal of smart irrigation is usually to produce the same crop using **less** total water).
2. **Node Counting:** Count how many "nodes" (the spots where leaves come out) each plant has. Water-stressed plants will have shorter distances between nodes.
3. **The "Wilt Watch":** If your "Smart" group hits 30% and the leaves look slightly grey or droopy, your sensor might be buried too deep or too shallow.

> **Important Note on Sensor Placement:** > Peas have shallow roots. Ensure your sensor is buried about **3–4 inches deep**. If it's at the very bottom of the pot, it will read "wet" while the plant’s roots at the top are actually bone dry.
