# SDU Maintainer Tool

Ist ein Windows Tool zum hochladen und verwalten der Firmware auf dem SDU-Server. Das Tool bedient sich der [REST API](sdu-api.md). Ebenso ist es möglich [SDU-Archiv](sdu-signing-tool.md) Dateien (Signierte Firmware) auf den SDU-Server zu laden.

## Benutzung
### Installation
- Tool herunterladen und entpacken. [SDU Maintainer Update Client-1.0.1-win.zip](https://hidrive.ionos.com/lnk/ow0jOvyX)
- Die Anwendung `SDU Maintainer Update Client.exe` starten
### Konfiguration
- Produktliste erstellen. Beispiel Datei mit einem Produkt:  
`my-products.json`:
	``` json
	{
		"products": [
			{
				"name": "dummy",
				"text": "My Dummy Device",
				"dataType": "raw",
				"maxParts": 1,
				"minParts": 1
			}
		]
	}
	```
	- `name`: Produkt Name
	- `text`: Frei definierter Produkt anzeige name
	- `dataType`: Art wie das Firmware Archiv zusammengepackt werden soll. Muss von der Gegenstelle, den Agenten, unterstützt sein.
		- `raw`: wenn die Firmware eine Datei/Image ist. Wird vom `rmgab` Agenten unterstützt.
		- `tar`: wenn die Firmware aus mehreren Dateien besteht.
	- `maxParts`, `minParts`: maximale bzw. minimale Anzahl der Dateien die in das Archive hinzugefügt werden können.
- Im Programm unter **Settings** 
![MUC Settings](img/muc-settings.png)
	- SDU-Server Adresse eintragen.
	- Zertifikat für die kommunikation mit dem Sererver auswählen und dazugehöriges Passwort eintragen.
	- Die erstellte Produktliste importieren.

### Firmware verwalten
![MUC Firmware](img/muc-firmware.png)
- Firmware hochladen. Projekt auswählen, eine signierte Firmware hochladen und aktivieren
- Firmware einem/mehreren Gerät/en zuweisen durch anlegen eines **Assigments**. Der Filter ist ein Regulärer Ausdruck.
