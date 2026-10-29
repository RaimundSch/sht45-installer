# SHT45 mit WLAN, MQTT und Home Assistant: Firmware im Browser aufspielen

Deutsch | [English](README.en.md)

Diese Anleitung zeigt Schritt für Schritt, wie du die Firmware auf dein ESP-Board spielst. Du brauchst dafür nur einen Computer mit Browser und ein USB-Kabel. Es muss nichts installiert und nichts programmiert werden.

**Die Installationsseite:** https://raimundsch.github.io/sht45-installer/

Danach (Teil B) schließt du den Sensor an, richtest das Gerät über sein eigenes WLAN ein und bindest es in Home Assistant ein.

## Teil A: Firmware aufspielen

### Das brauchst du

- Ein Board mit **ESP32**, **ESP32-S3** (Typ N16R8) oder **ESP8266**. Andere Chips (zum Beispiel ESP32-C3, ESP32-S2, ESP32-C6) sind nicht abgedeckt.
- Ein **USB-Datenkabel**. Reine Ladekabel übertragen keine Daten, mit ihnen erscheint das Board später nicht in der Liste.
- Einen **Computer** mit dem Browser **Google Chrome** oder **Microsoft Edge**. Die Seite ist mit Chrome getestet. Handy und Tablet (Android, iPhone) gehen nicht, das nennt auch der Hersteller der Technik (ESP Web Tools).
- Eine Internetverbindung, solange du die Seite benutzt.

Welchen Chip dein Board hat, musst du **nicht** wissen. Die Seite erkennt ihn selbst und nimmt die passende Datei.

### Schritt 1: Board anschließen

1. Stecke das USB-Kabel in das Board und in den Computer.
2. Wenn ein Programm auf deinem Computer den Anschluss des Boards benutzt, schließe es. Dazu zählen serielle Monitore, Arduino IDE oder PlatformIO. Sonst meldet die Seite später „Serial port is not readable/writable. Close any other application using it and try again.“
3. Steckt noch ein anderes Board am Computer, ziehe es ab. Dann ist später klar, welcher Eintrag zu deinem Board gehört.

### Schritt 2: Installationsseite öffnen

1. Öffne in Chrome oder Edge diese Adresse: **https://raimundsch.github.io/sht45-installer/**
2. Du siehst die Seite „SHT45 installieren“ mit dem Knopf **Connect**.

Statt des Knopfes steht eine Meldung?

- „Your browser does not support installing things on ESP devices …“: Dein Browser kann das nicht. Nimm Chrome oder Edge auf einem Computer.
- „You can only install ESP devices on HTTPS websites or on the localhost.“: Du hast die Seite mit `http://` statt `https://` geöffnet. Tippe die Adresse oben genau so ein.

### Schritt 3: Anschluss auswählen

1. Klicke auf **Connect**.
2. Der Browser öffnet ein eigenes Fenster mit der Liste der seriellen Anschlüsse. Der Text des Fensters hängt von Browser und Sprache ab.
3. Wähle den Eintrag deines Boards. Er heißt meist nach dem USB-Chip, zum Beispiel „USB-SERIAL CH340 (COM3)“, „USB-Enhanced-SERIAL CH343 (COM12)“ oder „Silicon Labs CP210x USB to UART Bridge (COM9)“. Die Nummer hinter „COM“ ist bei jedem Computer anders. Ist nur ein Eintrag da, ist es deiner.
4. Bestätige mit der Schaltfläche zum Verbinden.

Die Liste ist leer?

- Probiere ein anderes USB-Kabel (es muss ein Datenkabel sein) und einen anderen USB-Anschluss am Computer.
- Manche Boards brauchen einen Treiber für ihren USB-Chip. Unter Windows öffnest du den Geräte-Manager (Rechtsklick auf Start, „Geräte-Manager“) und siehst unter „Anschlüsse (COM & LPT)“ nach, ob dein Board auftaucht. Steht es dort als unbekanntes Gerät mit Warnzeichen, fehlt der Treiber. Der Chip steht auf dem Board oder in der Beschreibung (häufig CP210x, CH340 oder CH343).
- Schließt du das Fenster, ohne etwas zu wählen, erscheint „No port selected“ mit Hinweisen: Ist das Gerät mit diesem Computer verbunden? Leuchtet die Kontrollleuchte des Boards (die meisten Boards haben eine)? Ist das USB-Kabel ein Datenkabel? Sind die Treiber für den USB-Chip da? Unter Linux muss dein Benutzer in der Gruppe `dialout` sein (Befehl `sudo usermod -a -G dialout DeinBenutzername`, danach abmelden und wieder anmelden). Danach klickst du noch einmal auf **Connect**.

### Schritt 4: Installieren

Nach der Wahl öffnet sich ein Dialog. Sein Name ist „SHT45 mit MQTT und Home Assistant“, die Texte darin sind englisch.

1. Du siehst zwei Zeilen: **Install SHT45 mit MQTT und Home Assistant** und **Logs & Console**. Klicke auf **Install …**.
2. Die Seite „**Erase device**“ fragt: „Do you want to erase the device before installing …? All data on the device will be lost.“ Darunter steht ein Kästchen **Erase device**.
   - **Neues Board oder Neustart bei null:** Setze das Häkchen. Das Board wird vollständig gelöscht und danach neu beschrieben. Bereits gespeicherte Einstellungen (WLAN, MQTT) sind weg, das Board öffnet danach sein Einrichtungs-WLAN.
   - **Nur die Firmware erneuern:** Lasse das Kästchen leer. Dann wird der Speicher nicht gelöscht. Öffnet das Board danach trotzdem sein Einrichtungs-WLAN, trage WLAN und MQTT einfach noch einmal ein (Teil B).
3. Klicke auf **Next**.
4. Die Seite „**Confirm Installation**“ fragt: „Do you want to install SHT45 mit MQTT und Home Assistant 2026-10-29?“ (die Zahl ist die Version). Steht dort „All data on the device will be erased.“, hast du das Häkchen gesetzt. Klicke auf **Install**.
5. Jetzt läuft die Installation. Du siehst nacheinander „Preparing installation“, bei gesetztem Häkchen „Erasing“, dann einen Fortschrittsbalken mit Prozentzahl. Dazu steht, wie lange es dauert: **etwa eine Minute beim ESP8266, etwa zwei Minuten bei ESP32 und ESP32-S3.**
6. **Lass die Seite während der Installation sichtbar** (Fenster nicht minimieren, nicht auf einen anderen Tab wechseln). Der Dialog weist selbst darauf hin: „Keep this page visible to prevent slow down“. Ziehe das Kabel nicht ab.
7. Am Ende steht **Installation complete!** mit einem Konfetti-Symbol. Klicke auf **Next**.

Danach zeigt der Dialog wieder die Liste vom Anfang. Schließe den Dialog. Das Board startet neu. Erscheint nach etwa einer halben Minute kein WLAN mit dem Namen `SHT45-Setup-…` (Teil B), ziehe das USB-Kabel kurz ab und stecke es wieder an.

### Wenn es nicht klappt

| Meldung oder Beobachtung | Was du tun kannst |
|---|---|
| „Failed to initialize. Try resetting your device or holding the BOOT button while clicking INSTALL.“ | Halte am Board die Taste **BOOT** (bei manchen Boards **FLASH**) gedrückt, während du auf **Install** klickst. Lasse sie los, sobald die Installation läuft. Hat das Board keine Taste, ziehe das Kabel ab, stecke es wieder an und versuche es noch einmal. |
| „Failed to connect with the device“ | Wie oben: BOOT-Taste halten und noch einmal versuchen. Anderes USB-Kabel und anderen USB-Anschluss probieren. |
| „Serial port is not readable/writable. Close any other application using it and try again.“ | Ein anderes Programm benutzt den Anschluss. Schließe es (serielle Monitore, Arduino IDE, PlatformIO, ein zweiter Browser-Tab mit der Seite) und versuche es noch einmal. |
| „Installation failed“ | Klicke auf **Back** und starte die Installation noch einmal. Hilft es nicht, setze beim zweiten Versuch das Häkchen **Erase device**. |
| „Failed to download manifest“ oder ein Fehler beim Laden der Datei | Die Internetverbindung ist unterbrochen oder die Seite ist gerade nicht erreichbar. Lade die Seite neu (F5) und versuche es noch einmal. |
| Die Installation bleibt bei einer Prozentzahl stehen | Die Seite muss sichtbar bleiben. Warte ein paar Minuten. Läuft nichts mehr, ziehe das Kabel ab, stecke es wieder an, lade die Seite neu und beginne von vorn. |

Nach einer abgebrochenen Installation startet das Board möglicherweise nicht. Beginne dann einfach von vorn und führe die Installation vollständig bis „Installation complete!“ durch.

## Teil B: Sensor anschließen und einrichten

### Das brauchst du zusätzlich

- Den SHT45 auf einer Platine, die **VDD, GND, SDA und SCL** herausführt (bei der Platine „RS01“ in dieser Reihenfolge an der Stiftleiste).
- Drei bis vier Steckkabel (Dupont).
- Ein WLAN mit **2,4 GHz**. Die Chips können kein 5-GHz-WLAN.
- Für Home Assistant: einen MQTT-Broker (zum Beispiel Mosquitto) und die MQTT-Integration. Ohne Broker zeigt das Gerät seine Werte trotzdem auf einer eigenen Webseite.

### Schritt 5: Sensor anschließen

Ziehe vorher das USB-Kabel vom Board ab.

| Sensor | Board |
|---|---|
| VDD | 3V3 (3,3 Volt) |
| GND | GND |
| SDA | SDA-Pin des Boards (Tabelle unten) |
| SCL | SCL-Pin des Boards (Tabelle unten) |

**Der Sensor verträgt höchstens 3,6 Volt. Schließe VDD niemals an 5 V an.** Die Pull-up-Widerstände (10 kΩ) für SDA und SCL sitzen auf der Sensor-Platine.

Die Nummern in der Tabelle sind GPIO-Nummern. Wie der Anschluss auf deinem Board beschriftet ist, steht in dessen Pinout.

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
| NodeMCU v3 (ESP8266) | D2 = GPIO4 | D1 = GPIO5 |
| Wemos D1 mini (ESP8266) | D2 = GPIO4 | D1 = GPIO5 |

Achtung beim NodeMCU und beim D1 mini: Auf der Platine stehen D1, D2 und so weiter. Das sind **nicht** die GPIO-Nummern. Die Tabelle nennt beide.

Dein Board steht nicht in der Tabelle? Dann wähle in Schritt 6 „Custom“ (Benutzerdefiniert) und trage selbst die zwei Pins ein, an denen du SDA und SCL angeschlossen hast. Das Portal lehnt Pins ab, die auf dem Chip für diese Aufgabe nicht taugen.

Stecke danach das USB-Kabel wieder an.

### Schritt 6: Gerät einrichten

1. Das Board öffnet beim ersten Start ein eigenes WLAN **ohne Passwort**. Es heißt `SHT45-Setup-` mit vier Zeichen am Ende, zum Beispiel `SHT45-Setup-262C`. Die vier Zeichen unterscheiden mehrere Boards voneinander.
2. Verbinde Handy oder Computer mit diesem WLAN. Oft öffnet sich die Einrichtungsseite von selbst. Wenn nicht, öffne im Browser **http://192.168.4.1**.
3. Fülle das Formular aus:
   - **Gerätename:** So heißt das Gerät in Home Assistant. Gib jedem Gerät einen eigenen Namen.
   - **Board:** Wähle dein Board. SDA, SCL und Taste werden passend eingetragen. Ist dein Board nicht dabei, wähle „Benutzerdefiniert“ und trage die Pins selbst ein.
   - **WLAN:** Netzwerkname und Passwort deines Heimnetzes.
   - **MQTT:** IP-Adresse deines Brokers, Port (meist 1883), Benutzer und Passwort. Lässt du die Server-Adresse leer, gibt es kein MQTT.
   - **Automatisch bei Home Assistant anmelden:** Haken gesetzt lassen, dann taucht das Gerät dort von selbst auf.
   - **Messintervall und Meldeintervall:** Wie oft der Sensor gelesen wird (Vorgabe 2 Sekunden) und wie oft die Werte gesendet werden (Vorgabe 30 Sekunden).
   - **Temperaturkorrektur:** Wird zur gemessenen Temperatur addiert, Vorgabe 0.
   - **Passwort für die Einstellungsseite:** Das Passwort für spätere Änderungen im Heimnetz (Benutzer `admin`). Vorgabe `SHT45`. Ändere es.
4. Tippe auf **Speichern und neu starten**. Das Gerät verbindet sich mit deinem WLAN, und die Seite zeigt die **IP-Adresse**, die es bekommen hat. Schreibe sie auf. Nach etwa zwei Minuten startet das Gerät neu und läuft im Normalbetrieb.
5. Öffne im Heimnetz `http://<IP-Adresse>`. Du siehst Temperatur, Luftfeuchte, den Zeitpunkt der letzten Messung, den Sensor, das WLAN und den MQTT-Zustand.

Klappt die Verbindung mit dem WLAN nicht, öffnet das Gerät nach etwa 20 Sekunden wieder sein Einrichtungs-WLAN. Prüfe dann Netzwerkname und Passwort.

### Schritt 7: Home Assistant

Voraussetzung ist die MQTT-Integration in Home Assistant mit deinem Broker. Mit dem Haken „Automatisch bei Home Assistant anmelden“ erscheint das Gerät unter MQTT mit zwei Sensoren, „Temperatur“ und „Luftfeuchtigkeit“. Der Name der Sensoren beginnt mit dem Gerätenamen aus dem Portal. Bricht die Verbindung ab, zeigt Home Assistant „nicht verfügbar“.

### Später etwas ändern

- **Einstellungen ändern:** Öffne im Heimnetz `http://<IP-Adresse>/einstellungen` (es gibt einen Link darauf unten auf der Werteseite). Anmeldung mit `admin` und dem Passwort aus dem Portal.
- **Einrichtungs-WLAN wieder öffnen**, zum Beispiel nach einem Router-Wechsel:
  - Strom **dreimal kurz hintereinander** einschalten und wieder abziehen, jedes Mal nach weniger als 10 Sekunden. Beim dritten Einschalten öffnet das Portal.
  - Oder, bei Boards mit Taste: Strom einschalten und die Taste (GPIO0, bei manchen Boards BOOT oder FLASH) innerhalb der ersten 10 Sekunden etwa 3 Sekunden gedrückt halten. **Drücke die Taste nicht schon beim Einschalten**, sonst startet der Chip im Aufspielmodus und das Programm läuft nicht.
- **Alles zurücksetzen:** Auf der Einstellungsseite „Alle gespeicherten Daten löschen“. Das Gerät startet danach im Einrichtungs-WLAN.

### Wenn der Sensor nicht gefunden wird

Lies die Meldungen des Boards: Das Programm schreibt beim Start, was es findet. Mit Chrome oder Edge geht das auch ohne Zusatzprogramm: Öffne die Installationsseite, klicke **Connect**, wähle den Anschluss und dann **Logs & Console**. Dort siehst du die Ausgabe. Mit **Reset Device** startest du das Board neu und siehst die Meldungen vom Start. Beende die Ansicht mit **Back**, bevor du etwas anderes mit dem Anschluss machst.

- **„Kein Sensor gefunden“:** Prüfe, ob SDA und SCL nicht vertauscht sind, ob VDD an 3V3 und GND an GND liegt und ob die Pins im Portal zum Anschluss passen. Beim NodeMCU und D1 mini sind D-Nummer und GPIO-Nummer leicht zu verwechseln.
- **MQTT „nicht verbunden (Status 4)“:** Benutzer oder Passwort des Brokers stimmen nicht. „Status 5“ heißt: nicht erlaubt.
- **Das Gerät taucht in Home Assistant nicht auf:** Ist die MQTT-Integration eingerichtet, und steht der Haken „Automatisch bei Home Assistant anmelden“? Zeigt die Werteseite „MQTT: verbunden“?
- **Die Seite im Heimnetz ist nicht erreichbar:** Dein Router kann die IP-Adresse geändert haben. Die aktuelle zeigt der Router in der Geräteliste (der Hostname beginnt mit `sht45-`).

## Stand

Die Installationsseite ist am ESP32-S3 (N16R8) mit Chrome durchgespielt, einschließlich Löschen des Speichers. Für ESP32 und ESP8266 wählt die Seite laut Dokumentation der Technik (ESP Web Tools) die passende Datei selbst, an eigenen Geräten ausprobiert ist das hier noch nicht. Die Firmware selbst läuft auf allen drei Chips auf echter Hardware.
