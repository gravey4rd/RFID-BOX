# RFID_BOX by [gravey4rd](https://github.com/gravey4rd)
A portable, standalone RFID/NFC analysis and cloning tool based on ESP32-C3 and PN532.

## Description
RFID_BOX is a compact security tool designed for testing, cloning, and analyzing 13.56MHz Mifare tags. Unlike simple cloners, this device features an SD card database, allowing you to store unlimited UIDs, edit their names, and manage them directly from the device. It also includes offensive capabilities for security auditing, such as brute-forcing sector keys and stress-testing tags.

## Features
*   **Read & Identify:** Reads UID and identifies card types (Mifare Classic 1K, Ultralight, etc.).
*   **SD Database:** Save scanned cards to an SD card with custom names.
*   **Cloning:** Write saved UIDs to "Magic" cards (Supports both **Gen1a** and **CUID** writing methods).
*   **Card Management:** Edit names or delete saved cards directly from the OLED menu.
*   **Attack Modes:**
    *   **Virus FF:** Wipes writable sectors with `0xFF` data to disrupt tag functionality.
    *   **Brute Force:** Attempts to crack sector keys using a dictionary of common default keys and saves found keys to `keys.txt`.
*   **Format Tool:** Resets a Mifare Classic card to factory state (clears data, resets keys to `FFFFFFFFFFFF`).

## Hardware Required
*   **Microcontroller:** ESP32-C3 SuperMini
*   **NFC Module:** PN532 (v3)
*   **Display:** 0.96" SSD1306 OLED (I2C)
*   **Storage:** Micro SD Card Module
*   **Input:** 3x Push Buttons (Up, Select, Down)

## Pinout Configuration
The code is configured for the **ESP32-C3 SuperMini**. Wire your components as follows:

| Component | Pin Name | ESP32-C3 Pin | Note |
| :--- | :--- | :--- | :--- |
| **SD Card** | SCK | GPIO 4 | Hardware SPI |
| | MISO | GPIO 5 | |
| | MOSI | GPIO 6 | |
| | CS | GPIO 10 | |
| **PN532** | SCK | GPIO 0 | Software SPI |
| | MISO | GPIO 1 | |
| | MOSI | GPIO 2 | |
| | CS | GPIO 7 | |
| **OLED** | SDA | GPIO 8 | I2C |
| | SCL | GPIO 9 | |
| **Buttons** | UP | GPIO 20 | Pull-up |
| | SELECT | GPIO 3 | Pull-up (RX Pin) |
| | DOWN | GPIO 21 | Pull-up (TX Pin) |

## DISCLAIMER
The source code and hardware designs provided in this repository are for **educational purposes only**. This tool is intended to be used by security researchers and hobbyists to learn about RFID technology and test their own systems.
**Do not use this tool against systems you do not own or have explicit permission to test.** The author is not responsible for any misuse.

## Installation using Arduino IDE
1.  Install **Arduino IDE**.
2.  Add ESP32 board support: Go to `File` -> `Preferences` and add this URL to `Additional Boards Manager URLs`:
    `https://raw.githubusercontent.com/espressif/arduino-esp32/gh-pages/package_esp32_index.json`
3.  Install the required libraries via **Library Manager**:
    *   `Adafruit GFX Library`
    *   `Adafruit SSD1306`
    *   `Adafruit PN532`
4.  Select Board: `ESP32-C3 Supermini` 
5.  **Important:** Ensure `USB CDC On Boot` is enabled if you want Serial debugging, though the device works standalone.
6.  Connect your ESP32-C3 and click **Upload**.

## How to Use
**Navigation:**
*   **UP Button:** Scroll Up / Go Back
*   **DOWN Button:** Scroll Down / Next Character
*   **SELECT Button:** Enter Menu / Confirm / Save

**Menu Overview:**
1.  **Kart Okuma (Read Card):** Scans a tag. Press Select to save it to the SD card.
2.  **Kayitli Kartlar (Saved Cards):** Browse your database. Select a card to **Write (Clone)**, **Edit Name**, or **Delete**.
3.  **MC Kopyalama (Write Card):** Direct write mode for the currently loaded UID.
4.  **Saldiri (Attack):**
    *   *Virus FF:* Corrupts data blocks.
    *   *Sifre Kirici (Brute Force):* Scans all sectors against a dictionary of 40+ common keys.
5.  **Kart Formatla:** Wipes the card clean.
