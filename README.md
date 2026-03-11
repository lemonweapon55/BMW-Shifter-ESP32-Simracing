# BMW-Shifter-ESP32-Simracing
ESP32 based interface to convert a BMW electronic shifter (9_291_527-01) into a Bluetooth controller for Sim Racing.

I built this because I wanted a realistic sequential/auto shifter for my sim rig without spending $500+ on high-end gear. Real BMW electronic shifters are 
everywhere in junkyards, they feel/work amazing, and with an ESP32, its possible to make it talk to a PC over Bluetooth.

This project handles the CAN bus communication and the BMW checksum (CRC) logic needed to keep the shifter "alive" and translating those movements into gamepad buttons. 

It’s still a work in progress, so expect some bugs, but the core shifting logic is solid. Feel free to mod the code or add new shifter IDs!

---
## ⚠️ Personal Use Only
I'm sharing this for the community to use and improve. Build it for your own rig, but **don't start a shop and mass-sell kits using the code.** Check the LICENSE file for the "no-profit" legal details.

## Step-by-Step Setup (Tutorial)

### 1. Get the Hardware
You’ll need these basic parts:
* **ESP32 DevModule**
* **MCP2515 CAN Bus Module** (HW-184_120ohm-closed)
* **BMW Electronic Shifter** (Tested on [9_291_527-01] and probably works on more in this Gen)
* **12V Power Supply 2/3A** (For the shifter LEDs and solenoids)
* **Resistors 1K and 2K**
* **Jumper wires**
(Important notice-Make sure to keep the electrical noise around the CAN H/L Low or zero because it can
affect the shifters performance.It can be done by keeping distance with the power wires and Can)

### 2. Wiring it up
Follow this pin map to connect the ESP32 to the CAN module:
* **CS** -> GPIO 5
* **INT** -> [MCP2515_INT] -- [1K Resistor] -- [ESP32 GPIO 4] -- [2K Resistor] -- [GND] (VOLTAGE DIVIDER METHOD-THESE STEPS ARE CRUCIAL TO NOT FRY YOUR ESP)(REPEAT THe SAME STEP WITH MISO) 
* **SCK** -> GPIO 18
* **MOSI** -> GPIO 23
* **MISO** -> [MCP2515_MISO] -- [1K Resistor] -- [ESP32 GPIO 19] -- [2K Resistor] -- [GND]
* **VCC** -> 5V/VIN
* **GND** -> GND

-BMW shifter pin map
* **Pin 3 (Red)** -> CAN Low (Connect to MCP2515 **L**)
* **Pin 4 (Blue/Red)** -> CAN High (Connect to MCP2515 **H**)
* **Pin 7 (Green/Red)** -> +12V Switched Power
* **Pin 8 (Brown)** -> Ground (Connect to **12V Negative** AND **ESP32 GND**)
* **Pin 10 (Red/Blue)** -> +12V Permanent Power

(Warning-If the grounds aren't connected to all ground channels its not going to work- And the wire colours can vary depending on your version or plug)

### 3. Install the Libraries
Open your Arduino IDE and install these:
1. **ESP32-BLE-Gamepad** (by lemmingDev)
2. **mcp_can** (by coryjfowler)

### 4. Flash and Play
* Open the `.ino` file from this repo.
* Select **ESP32 DEV MODULE** as your board.
* Hit Upload.
* On your PC, search for a Bluetooth device named **"BMW Sim Shifter"** and pair it.

---
## 5 Mapping
The shifter shows up as a standard controller. 
* **Button 1**: Downshift
* **Button 2**: Upshift
* **Button 3**: Mode Toggle
* **Button 4**: Park Button

---
### 💬 Questions?
If you get stuck on the wiring or the code doesn't seem to talk to your specific shifter, just open an **Issue** here on GitHub or message me. I'm happy to help you get your rig up and running!

### ☕ Support the Project
This project is based on an open-source Arduino Uno sketch that I’ve ported over to the ESP32. I spent a lot of time rewriting the logic for Bluetooth support, decoding the CAN messages, and figuring out the hardware protections.

If this project saved you some serious cash on a sim shifter and you want to show some love, you can support me here:

* **Tikkie (NL):** []
* **Buy Me a Coffee (Global):** []

Im definitly doing more of these projects.
Every bit helps me get more junkyard parts to decode for the community!

