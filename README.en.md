# SHT45 with WiFi, MQTT and Home Assistant: install the firmware from your browser

[Deutsch](README.md) | English

This guide shows step by step how to put the firmware on your ESP board. All you need is a computer with a browser and a USB cable. Nothing has to be installed or programmed.

**The installation page:** https://raimundsch.github.io/sht45-installer/

After that (part B) you connect the sensor, set up the device through its own WiFi network and add it to Home Assistant.

## Part A: install the firmware

### What you need

- A board with an **ESP32**, **ESP32-S3** (type N16R8), **ESP32-C6** or **ESP8266**. Other chips (for example ESP32-C3 or ESP32-S2) are not covered.
- A **USB data cable**. Charge-only cables do not carry data, with them the board will not show up in the list later.
- A **computer** with **Google Chrome** or **Microsoft Edge**. The page has been tested with Chrome. Phones and tablets (Android, iPhone) do not work, as the maker of the technology (ESP Web Tools) also states.
- An internet connection while you use the page.

You do **not** need to know which chip your board has. The page detects it and picks the right file.

### Step 1: connect the board

1. Plug the USB cable into the board and into the computer.
2. If a program on your computer uses the board's port, close it. This includes serial monitors, Arduino IDE or PlatformIO. Otherwise the page will later say "Serial port is not readable/writable. Close any other application using it and try again."
3. If another board is plugged into the computer, unplug it. Then it is clear later which entry belongs to your board.

### Step 2: open the installation page

1. In Chrome or Edge, open this address: **https://raimundsch.github.io/sht45-installer/**
2. You see the page "SHT45 installieren" with the button **Connect**.

A message instead of the button?

- "Your browser does not support installing things on ESP devices …": Your browser cannot do this. Use Chrome or Edge on a computer.
- "You can only install ESP devices on HTTPS websites or on the localhost.": You opened the page with `http://` instead of `https://`. Type the address above exactly.

### Step 3: choose the port

1. Click **Connect**.
2. The browser opens its own window with the list of serial ports. The text of that window depends on the browser and its language.
3. Choose the entry of your board. It is usually named after the USB chip, for example "USB-SERIAL CH340 (COM3)", "USB-Enhanced-SERIAL CH343 (COM12)" or "Silicon Labs CP210x USB to UART Bridge (COM9)". The number after "COM" is different on every computer. If there is only one entry, it is yours.
4. Confirm with the button for connecting.

The list is empty?

- Try another USB cable (it must be a data cable) and another USB port on the computer.
- Some boards need a driver for their USB chip. On Windows, open Device Manager (right-click Start, "Device Manager") and look under "Ports (COM & LPT)" to see whether your board appears. If it is listed as an unknown device with a warning sign, the driver is missing. The chip is printed on the board or named in its description (often CP210x, CH340 or CH343).
- If you close the window without choosing anything, "No port selected" appears with hints: Is the device connected to this computer? Does the board's indicator light come on (most boards have one)? Is the USB cable a data cable? Are the drivers for the USB chip installed? On Linux your user must be in the group `dialout` (command `sudo usermod -a -G dialout YourUserName`, then log out and in again). Then click **Connect** again.

### Step 4: install

After you choose, a dialog opens. Its name is "SHT45 mit MQTT und Home Assistant", the texts in it are in English.

1. You see two rows: **Install SHT45 mit MQTT und Home Assistant** and **Logs & Console**. Click **Install …**.
2. The page "**Erase device**" asks: "Do you want to erase the device before installing …? All data on the device will be lost." Below it is a checkbox **Erase device**.
   - **New board or a fresh start:** Tick the box. The board is erased completely and then written again. Settings that were already saved (WiFi, MQTT) are gone, and the board opens its setup network afterwards.
   - **Only renewing the firmware:** Leave the box empty. The memory is then not erased. If the board opens its setup network anyway, simply enter WiFi and MQTT again (part B).
3. Click **Next**.
4. The page "**Confirm Installation**" asks: "Do you want to install SHT45 mit MQTT und Home Assistant 2026-10-29?" (the number is the version). If it says "All data on the device will be erased.", you ticked the box. Click **Install**.
5. The installation runs now. You see "Preparing installation" first, "Erasing" if you ticked the box, then a progress bar with a percentage. The dialog also says how long it takes: **about a minute on the ESP8266, about two minutes on ESP32, ESP32-S3 and ESP32-C6.**
6. **Keep the page visible while it installs** (do not minimize the window, do not switch to another tab). The dialog says so itself: "Keep this page visible to prevent slow down". Do not unplug the cable.
7. At the end it says **Installation complete!** with a confetti symbol. Click **Next**.

The dialog then shows the list from the start again. Close the dialog. The board restarts. If no WiFi network named `SHT45-Setup-…` (part B) appears after about half a minute, unplug the USB cable briefly and plug it in again.

### If it does not work

| Message or observation | What you can do |
|---|---|
| "Failed to initialize. Try resetting your device or holding the BOOT button while clicking INSTALL." | Hold the **BOOT** button on the board (**FLASH** on some boards) while you click **Install**. Release it as soon as the installation runs. If the board has no button, unplug the cable, plug it in again and try once more. |
| "Failed to connect with the device" | As above: hold the BOOT button and try again. Try another USB cable and another USB port. |
| "Serial port is not readable/writable. Close any other application using it and try again." | Another program is using the port. Close it (serial monitors, Arduino IDE, PlatformIO, a second browser tab with the page) and try again. |
| "Installation failed" | Click **Back** and start the installation again. If that does not help, tick **Erase device** on the second attempt. |
| "Failed to download manifest" or an error while loading the file | The internet connection is interrupted or the page is not reachable right now. Reload the page (F5) and try again. |
| The installation stops at a percentage | The page has to stay visible. Wait a few minutes. If nothing moves, unplug the cable, plug it in again, reload the page and start over. |

After an interrupted installation the board may not start. Then simply start over and run the installation completely up to "Installation complete!".

## Part B: connect the sensor and set it up

### What else you need

- The SHT45 on a board that breaks out **VDD, GND, SDA and SCL** (on the "RS01" board in this order on the pin header).
- Three to four jumper wires (Dupont).
- A **2.4 GHz** WiFi network. These chips cannot use 5 GHz.
- For Home Assistant: an MQTT broker (for example Mosquitto) and the MQTT integration. Without a broker the device still shows its readings on its own web page.

### Step 5: connect the sensor

Unplug the USB cable from the board first.

| Sensor | Board |
|---|---|
| VDD | 3V3 (3.3 volts) |
| GND | GND |
| SDA | the board's SDA pin (table below) |
| SCL | the board's SCL pin (table below) |

**The sensor tolerates 3.6 volts at most. Never connect VDD to 5 V.** The pull-up resistors (10 kΩ) for SDA and SCL are on the sensor board.

The numbers in the table are GPIO numbers. How the pin is labelled on your board is shown in its pinout.

| Board | SDA | SCL |
|---|---|---|
| ESP32 DevKit C1 | GPIO21 | GPIO22 |
| Adafruit Feather ESP32 V2 | GPIO22 | GPIO20 |
| Adafruit ItsyBitsy ESP32 | GPIO15 | GPIO27 |
| Adafruit QT Py ESP32 (STEMMA QT) | GPIO22 | GPIO19 |
| Olimex ESP32-EVB (UEXT) | GPIO13 | GPIO16 |
| Wireless-Tag WT32-ETH01 | GPIO33 | GPIO32 |
| M5Stack Core (Basic), Port A | GPIO21 | GPIO22 |
| M5Stack Core2, Port A | GPIO32 | GPIO33 |
| WEMOS LOLIN D32 Pro | GPIO21 | GPIO22 |
| ESP32-S3 N16R8 | GPIO8 | GPIO9 |
| DFRobot Beetle ESP32-C6 | GPIO19 | GPIO20 |
| ESP32-C6 DevKitC-1 | GPIO23 | GPIO22 |
| NodeMCU v3 (ESP8266) | D2 = GPIO4 | D1 = GPIO5 |
| Wemos D1 mini (ESP8266) | D2 = GPIO4 | D1 = GPIO5 |

Careful with the NodeMCU and the D1 mini: the board is labelled D1, D2 and so on. These are **not** GPIO numbers. The table gives both.

Your board is not in the table? Then choose "Custom" in step 6 and enter the two pins to which you connected SDA and SCL. The portal rejects pins that cannot be used for this on the chip.

Then plug the USB cable in again.

### Step 6: set up the device

1. On first start the board opens its own WiFi network **without a password**. It is called `SHT45-Setup-` with four characters at the end, for example `SHT45-Setup-262C`. The four characters tell several boards apart.
2. Connect your phone or computer to this network. The setup page often opens by itself. If not, open **http://192.168.4.1** in a browser.
3. Fill in the form:
   - **Device name:** The name of the device in Home Assistant. Give every device its own name.
   - **Board:** Choose your board. SDA, SCL and button are filled in accordingly. If your board is not listed, choose "Custom" and enter the pins yourself.
   - **WiFi:** Network name and password of your home network.
   - **MQTT:** IP address of your broker, port (usually 1883), user and password. If you leave the server address empty, there is no MQTT.
   - **Register automatically with Home Assistant:** Leave it ticked and the device shows up there by itself.
   - **Measuring interval and reporting interval:** How often the sensor is read (default 2 seconds) and how often the values are sent (default 30 seconds).
   - **Temperature correction:** Added to the measured temperature, default 0.
   - **Password for the settings page:** The password for later changes in your home network (user `admin`). Default `SHT45`. Change it.
4. Tap **Save and restart**. The device connects to your WiFi, and the page shows the **IP address** it received. Write it down. After about two minutes the device restarts and runs normally.
5. In your home network, open `http://<IP address>`. You see temperature, humidity, the time of the last measurement, the sensor, the WiFi and the MQTT state.

If the WiFi connection fails, the device opens its setup network again after about 20 seconds. Then check the network name and password.

### Step 7: Home Assistant

You need the MQTT integration in Home Assistant, connected to your broker. With "Register automatically with Home Assistant" ticked, the device appears under MQTT with two sensors, "Temperatur" and "Luftfeuchtigkeit" (these names are German). The sensor names start with the device name from the portal. If the connection drops, Home Assistant shows "unavailable".

### Changing something later

- **Change settings:** In your home network, open `http://<IP address>/einstellungen` (there is a link to it at the bottom of the readings page). Log in with `admin` and the password from the portal.
- **Open the setup network again**, for example after replacing the router:
  - Switch the power on and unplug it again **three times in a row**, each time after less than 10 seconds. At the third power-on the portal opens.
  - Or, on boards with a button: switch the power on and hold the button (GPIO0, called BOOT or FLASH on some boards) for about 3 seconds within the first 10 seconds. **Do not hold the button while switching on**, otherwise the chip starts in flashing mode and the program does not run.
- **Reset everything:** On the settings page, "Delete all saved data". The device then restarts in the setup network.

### If the sensor is not found

Read the board's messages: At start the program writes what it finds. With Chrome or Edge you can do this without any extra program: open the installation page, click **Connect**, choose the port and then **Logs & Console**. The output appears there. With **Reset Device** you restart the board and see the messages from the start. Leave the view with **Back** before you do anything else with the port.

- **"Kein Sensor gefunden" (no sensor found):** Check that SDA and SCL are not swapped, that VDD is on 3V3 and GND on GND, and that the pins in the portal match your wiring. On the NodeMCU and D1 mini the D number and the GPIO number are easy to mix up.
- **MQTT "nicht verbunden (Status 4)" (not connected):** The broker's user or password is wrong. "Status 5" means not allowed.
- **The device does not show up in Home Assistant:** Is the MQTT integration set up, and is "Register automatically with Home Assistant" ticked? Does the readings page show MQTT as connected?
- **The page in your home network is not reachable:** Your router may have changed the IP address. The router's device list shows the current one (the host name starts with `sht45-`).

## Status

The installation page has been run through completely on the ESP32-S3 (N16R8) with Chrome, including erasing the memory. For ESP32, ESP32-C6 and ESP8266 the page picks the matching file itself according to the documentation of the technology (ESP Web Tools); this has not yet been tried on our own devices. The firmware itself runs on real hardware on all four chips, on the ESP32-C6 with the DFRobot Beetle (sensor on GPIO19 and GPIO20). The installation page file for the C6 has not yet been flashed to a device. Still open on the C6: once, in the setup window on a notebook, the display of the IP address did not appear, on a phone it did. The cause is not clear. If you do not see an IP address, look in your router's device list (the name starts with `sht45-`).
