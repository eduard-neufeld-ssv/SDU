# SDU Agent

SDU-Agent ist ein speziell für den jeweiligen Updateprozess, eine Komponente/Mashine/Anlage, erstelltes Programm. Der Agent wird im Gateway vom [GUC](sdu-guc.md) (Gateway Update Client) ausgeführt/bedient und bietet dazu eine spezielle Schnittstelle (ADU-Agent API). Es können mehrere Agenten im System installiert werden.

## SDU-Agent API
Ein SDU-Agent hat folgende Aufgaben:

* Auskunft geben über die aktuell installierte Version
* Eine neue Version installieren

Die Kommunikation erfolgt durch Aufrufparameter, Standard Input/Output/Error und Exit Code des Programms.

### Aktuelle Version abfragen

Die Abfrage erlaubt dem SDU-Gateway-Update-Client herauszufinden, welches Produkt der Agent bedient, welche Version installiert ist und ggf. wo ein Abbild der aktuell installierten Version zu finden ist. Letzteres wird benötigt, um Updates per Differenzen zu komprimieren.

* **Aufruf**: `/path/to/agent info`
* **STDOUT**:
```js
{
	"product": "rmg941",
	"version": "3.2.5", // Optional (nicht angegeben -> auf jeden Fall updaten)
	"path": "/dev/mmcblk0p3", // Optional
	"sha256": "ba7816bf8f01cfea414140de5dae2223b00361a396177a9cb410ff61f20015ad" // Optional
}
```
* **STDERR**: Fehlermeldungen für den Fall, dass der Exit-Code nicht 0 ist.
* **Exit**:
   - `0`: OK
   - `1`: Busy
   - `2`: WrongConfiguration

### Neue Version schreiben

Die Abfrage erlaubt dem SDU-Gateway-Update-Client ein neues Update zu installieren. Dies wird passieren, wenn die Version, die der `info`-Befehl zurück liefert sich von der Version, die der SDU-Server vorsieht, unterscheidet.

* **Aufruf**: `/path/to/agent install [version] [sha256]`
   - `[version]`: Versions-String des zu installierenden Updates. Der Agent kann hieran bereits entscheiden, ob das Update akzeptiert wird. Bspw. können darüber Downgrades verhindert werden, wenn die Firmware damit nicht umgehen kann.
   - `[sha256]`: Der SHA256-Hash des Updates. Der Agent muss prüfen, ob die empfangenen Daten tatsächlich diesen Hash bilden. Um einen vollständigen Download zum Prüfen des Hashes zu verhindern (bei großen Update ist das bspw. gar nicht möglich), wird die Prüf-Aufgabe nicht vom Gateway-Update-Client erledigt.
* **STDIN**: Datenstrom des neuen Images
* **STDERR**: Statusmeldungen über den aktuellen Schritt
* **STDOUT**: Aktionen, die dem GUC mitgeteilt werden können
   - `REBOOT`: Neustart des Gateways nach dem erfolgreichen Update
* **Exit**:
   - 0: Done
   - 1: Failed
   - 2: RejectWrongVersion
   - 3: RejectWrongSHA256
