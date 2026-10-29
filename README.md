# SHT45 installieren

Webseite zum Aufspielen der Firmware aus dem Browser (ESP Web Tools). Enthält nur die Seite und die drei fertigen Dateien aus dem Projekt `sht45-esp-mqtt`. Anleitung, Anschluss und Quellcode stehen dort.

- `index.html`: die Seite mit dem Installationsknopf
- `manifest.json`: ordnet jedem Chip (ESP32, ESP32-S3, ESP8266) seine Datei zu
- `firmware/`: je Chip eine Datei ab Adresse 0, dazu `SHA256SUMS.txt`

Die Seite funktioniert nur, wenn sie über `https://` ausgeliefert wird (oder lokal über `localhost`).
