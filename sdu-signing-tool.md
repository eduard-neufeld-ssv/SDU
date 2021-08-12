# SDU Signing Tool

Ist ein Windows Tool zum verpacken und signieren der Firmware in einen SDU-Archive mit der Endung `*.sdu`. Dieses Archive wird von dem [SDU Maintainer Tool](sdu-maintaner-tool.md) benutzt.

## Benutzung
### Installation
- Tool herunterladen und entpacken. [SDU Signing Tool-3.0.0-win.zip](https://hidrive.ionos.com/lnk/EwUjOrRm)
- Die Anwendung `SDU Signing Tool.exe` starten

### Konfiguration
![SDU Signing Tool](img/sdu-signing-tool.png)
- Unter **Product** die [Produktliste](sdu-maintainer-tool.md) importieren.
- Zertifikat für die Signierung auswählen und dazugehöriges Passwort eintragen.
- Verzeichnis auswählen wo das erstellte SDU-Archive abgelegt werden soll.

### SDU-Archive erstellen
- Produkt auswählen.
- Version vergeben, optional ein Kommentar vergeben.
- Firmware Datei in das Archive hinzufügen mit **Add File**.
- Archive erstellen und signieren mit **Create & Sign**.
