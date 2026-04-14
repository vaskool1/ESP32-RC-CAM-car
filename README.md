# 🚗 ESP32 RC Car — Bluetooth & Wi-Fi Dual Mode

A custom-built RC car powered by two ESP32-CAM modules, controllable via a **PlayStation controller (Bluetooth)** or a **web browser (Wi-Fi)**. Features a servo for steering and a brushless motor + ESC for drive.

> ⚠️ **Windows only.** The tools required for this build are Windows-exclusive. Only proceed on another OS if you are an experienced user.

---

## 📺 Build Video

**Watch this first before doing anything else:**
👉 [YouTube Build Guide](https://www.youtube.com/watch?v=ulTM1uV1Bvg)

Follow the video guide until you reach the Arduino section — from that point, use this README instead.

---

## 🛒 Parts List

| Part | Notes |
|------|-------|
| 2× ESP32-CAM (AI Thinker) | One for main control, one for web view |
| RC car chassis | As used in the build video |
| Servo motor | For steering (brown = GND, orange = signal) |
| Brushless motor + DUMBO RC 30A ESC | For drive |
| LiPo battery pack | Compatible with your ESC |
| PS4 / DualShock 4 controller | For Bluetooth mode |
| Toggle switch | For switching between Bluetooth and Wi-Fi modes |
| 2× LEDs *(optional)* | For underglow lighting |
| Jumper wires | For all connections |
| Solder + soldering iron | For pigtail connections |
| Hot glue gun | To insulate and prevent shorts |
| USB cable | For flashing ESP32-CAMs via Arduino IDE |
| Windows PC | Required for SixaxisPairTool and SPIFFS uploader |

---

## ⚡ Wiring

### Servo (Steering)
- **Brown wire** → GND
- **Orange wire** → IO13

### ESC (Drive Motor)
- **Ground wire** → GND
- **Positive wire** → IO12
- Connect ESC positive and negative terminals to the battery
- Route the new ESC positive output → **5V rail on ESP32**

### Second ESP32 (Web View)
- Solder a pigtail onto the positive and ground wires from the ESC (**separately — do not join them**)
- Pigtail positive → **5V** of second ESP32-CAM
- Pigtail ground → **GND** of second ESP32-CAM

### Mode Switch
- One end → **IO14**
- Other end → **GND** (use the ESP32's own ground)
- Mount the switch on the **right side of the car**, next to the batteries

### Underglow LEDs *(optional)*
Wire two LEDs in series to prevent voltage damage:
1. LED 1 positive → **5V line**
2. LED 1 negative → LED 2 positive
3. LED 2 negative → **GND** (battery GND or ESP32-CAM connection GND)

> 🔒 Use **plenty of hot glue** over solder joints to prevent shorts.

---

## 💾 Software & Libraries

Install **Arduino IDE**, then add the following:

| Library | Version / Source |
|---------|-----------------|
| Espressif ESP32 boards | **v2.0.14 exactly** — newer versions break SPIFFS |
| PS4 Controller | Tools → Manage Libraries → search "PS4 Controller" |
| SPIFFS uploader | Install via the linked tutorial |

> ⚠️ You **must** use Espressif **v2.0.14**. The latest version is incompatible with the SPIFFS upload method used here.

---

## 📤 Uploading the Code

### Arduino IDE Board Settings
- **Board:** ESP32-CAM AI Thinker
- **Port:** Whichever COM port your ESP32-CAM is on

### Steps

1. **Main ESP32 (most connectors):**
   - Open the main sketch (servo, ESC control, and switch logic)
   - Set your Wi-Fi password in the file — it defaults to `password123`
   - Upload the sketch
   - Open **Tools → Sketch Data Upload** to flash the SPIFFS data to the same chip
   - Open the **Serial Monitor** (top right) and **copy the MAC address** that appears

2. **Second ESP32:**
   - Upload the smaller web-view package (for browser-based monitoring and control)

---

## 🎮 Pairing the PS4 Controller

1. Download **SixaxisPairTool** — use only the green **"Download for Windows"** button on the official page
2. Plug your PS4 controller in via USB
3. Paste the MAC address you copied from the Serial Monitor into the text box
4. Click **"Update"**
5. Confirm the "Current Master" field now shows your new MAC address

---

## 🕹️ How to Use

| Mode | How to Activate |
|------|----------------|
| **Bluetooth mode** | Flip the toggle switch to the Bluetooth position and connect your PS4 controller |
| **Wi-Fi mode** | Flip the toggle to Wi-Fi, turn off mobile data, connect to the car's Wi-Fi network, then navigate to `http://192.168.4.1` in your browser |

> 📡 **Note:** Wi-Fi mode will always have more latency than Bluetooth mode due to hardware limitations. This is expected and cannot be fixed.

---

## ⚙️ Code Notes

- Wi-Fi password is set in the main file (default: `password123`)
- ESP32 clock speed is set to **10 MHz** to improve Wi-Fi mode servo response
- SPIFFS is used to serve the HTML web interface from the ESP32's flash storage

---

## 🧩 Challenges & Lessons Learned

This was a genuinely difficult project. A few notable hurdles:

- **No Python server allowed** — the school environment blocked running a local Python server for Bluetooth commands, which led to the two-ESP32 architecture as a workaround.
- **Espressif version conflict** — the latest Espressif board package breaks SPIFFS. Version 2.0.14 is required. This took significant time to diagnose.
- **Servo lag in Wi-Fi mode** — servo commands over Wi-Fi were delayed or unreliable. Lowering the CPU clock to 10 MHz helped, but Wi-Fi mode is inherently slower than Bluetooth. No software fix exists.
- **Limited prior art** — this combination of features hadn't been fully documented by anyone else, so most problems had to be solved from scratch.

> This project is not beginner-friendly and costs more than buying a comparable off-the-shelf camera car. That said, it's a great learning experience in embedded systems, C++, JavaScript, and wireless protocols.

---

## 📄 License

Do whatever you want with this. Just have fun building it. 🚀
