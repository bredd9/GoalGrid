# Goal Grid - IoT Football Data Terminal (Remote Controlled)

## General Description
The **Goal Grid** is a connected embedded system designed to track live standings, points, and team form across 7 major European football leagues (including the La Liga, Premier League, Champions League, and Romanian SuperLiga).

Unlike standard dashboard projects, this system utilizes a **dual-microcontroller architecture**:
* **The "Brain" (ESP32):** Handles Wi-Fi connectivity, HTTPS requests to the Football API, JSON parsing, and graphics rendering on a TFT display.
* **The "Controller" (Arduino UNO):** Acts as a dedicated physical remote interface, processing user inputs (Up/Down/Enter) via interrupt-based logic and transmitting commands to the ESP32 via UART serial communication.

This project demonstrates **Systems Integration** (getting two different architectures to communicate), **IoT Data Processing** (handling complex JSON on the edge), and **UI/UX Design** for embedded displays.

---

## Bill of Materials (BOM)

| Component | Quantity | Description |
| :--- | :--- | :--- |
| **ESP32 Dev Module** | 1 | Main processor for Wi-Fi & Display control ("Brain") |
| **Arduino UNO R3** | 1 | Input controller for physical buttons ("Remote") |
| **3.5" TFT SPI Display** | 1 | Display which showcases the **Goal Grid**, can be controlled using buttons |
| **Push Buttons** | 3 | Physical inputs for Menu Navigation (Up, Down, Confirm) |
| **Resistors** | as needed | Resistors for buttons & voltage dividing |
| **Voltage Divider** | 2 | 1kΩ & 2kΩ resistors (Level shifting 5V Arduino TX to 3.3V ESP32 RX) |
| **Breadboard** | 1 | For prototyping |
| **Jumper Wires** | as needed | Wires for connectivity |
| **USB Cables** | 2 | For power and programming both boards |

---

## Project Q&A 

### Q1 - What is the system boundary?
* **Inside:** The system boundary encircles the **ESP32**, the **Arduino UNO**, the **TFT Display**, and the **Physical Buttons**.
* **Outisde:** The system interacts with the "outside world" via Wi-Fi to the **Football API** server. The router and the API server are *outside* the system boundary; the device merely consumes their data.

### Q2 - Where does intelligence live?
The intelligence is distributed but primarily resides in the **ESP32**, it runs the core logic. It decides *when* to fetch data to save API credits, parses the complex JSON structure (extracting only relevant fields like 'Points' and 'Form').
The  **Arduino** handles signal conditioning (debouncing) and translates physical button presses into standardized commands for the master unit.

### Q3 - What is the hardest technical problem?
**Memory Management & JSON Parsing on the Edge.**
The JSON payload for a full league table is large (several kilobytes). Parsing this entirely into dynamic memory can cause the ESP32 to crash or lag. The challenge is implementing a stream-based parser or an efficient filter to extract *only* the Top 10-15 teams and their specific stats, discarding the rest to prevent heap overflow, while maintaining a responsive UI.

### Q4 - What is the minimum demo?
A successful minimum demo consists of:
1.  Powering on the device.
2.  The ESP32 successfully connecting to Wi-Fi.
3.  The user selecting one league via the Arduino buttons.
4.  The screen displaying a static text list of the Top 5 Teams with their current point totals, fetched live from the API.

### Q5 - Why is this not just a tutorial?
* **Complexity:** It integrates two distinct arhitectures (AVR/Arduino and Xtensa/ESP32) communicating via a custom UART protocol.
* **Customization:** It requires specific API handling for niche leagues (Romanian Liga 1) and advanced memory handling logic for the specific stats that are needed for the **Goal Grid**.

### Q6 - Do you need an ESP32?

Yes, we need the **Wi-Fi** capabilities to fetch live data from the internet, and the significantly higher **RAM** (compared to an Arduino) to handle the JSON parsing and the color display buffer. An Arduino UNO alone cannot handle HTTPS encryption or the memory requirements of the display library.

---

## Diagram

