# SDU Maintainer Tool

Ist ein Windows Tool zum hochladen der Software ([SDU-Archive](sdu-signing-tool.md)) auf den SDU-Server und zur Verwaltung/Verteilung dieser. Das Tool bedient sich der [REST API](https://github.com/SSV-embedded/SDU-API).

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
	- `name`: Produkt Name nach JavaScript [Kodierungskonventionen](https://de.wikipedia.org/wiki/Namenskonvention_(Datenverarbeitung))
	- `text`: In den Tools angezeigter Produkt name. Frei definierbar.
	- `dataType`: Art wie das Firmware Archiv zusammengepackt werden soll. Muss von der Gegenstelle, dem [SDU-Agenten](sdu-agent.md), unterstützt sein. Hier gibt es zwei Möglichkeiten:
		- `raw`: wenn die Firmware aus einer Datei besteht kann diese roh transportiert werden.
		- `tar`: wenn die Firmware aus mehreren Dateien besteht macht es Sinn diese in ein TAR Archiv zu packen.
	- `maxParts`, `minParts`: maximale bzw. minimale Anzahl der Dateien die in das Archive hinzugefügt werden können. Gilt als auswahlbegrenzung für das [Signing Tool](sdu-signing-tool.md)
- Im Programm unter **Settings** 
![MUC Settings](img/muc-settings.png)
	- SDU-Server Adresse eintragen.
	- Zertifikat für die kommunikation mit dem Sererver auswählen und dazugehöriges Passwort eintragen.
	- Die erstellte Produktliste importieren.

### Firmware verwalten
![MUC Firmware](img/muc-firmware.png)
- Firmware hochladen. Projekt auswählen, eine signierte Firmware hochladen und aktivieren
- Firmware einem/mehreren Gerät/en zuweisen durch anlegen eines **Assigments**. Der Filter ist ein Regulärer Ausdruck und wird von oben nach unten abgearbeitet.
