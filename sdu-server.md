# SDU Server

Secure Device Update Server verwaltet und stellt Firmware für die zu aktualisierende Geräte bereit. Der SDU-Server wird über eine [REST API](https://github.com/SSV-embedded/SDU-API) bedient und steht als ein Docker Image bei DockerHub (https://hub.docker.com/r/ssvembeddedde/ssv-sdu-server/) zu Verfügung.

Der Workflow für den Betrieb des Servers sieht wie folgt aus:
- SDU-Server Docker auf einem Host starten. Der Host muss eine gültige FQDN besitzen.
- SSV den FQDN des Hosts mitteilen. SSV stellt eine Lizenz aus und spielt diese in den Server. Ebenso werden Schlüssel erstellt für den Zugriff auf den Server.
- Damit ist der SDU-Server betriebsbereit.

## Server starten
### 1. SDU-Server Konfigurieren
Datenverzeichnis anlegen, in diesem Beispiel `data/`, in welchen der Container seine Daten ablegt. Die gespeicherten Daten sind die Server Konfiguration und die vom Benutzer hochgeladene Software:
```bash
mkdir -p data
```
Docker-Compose Konfiguration `docker-compose.yml` mit folgendem Inhalt anlegen:
```yml
version: '2.1'
services:
  sdu-server:
    image: ssvembeddedde/ssv-sdu-server:1.0.2
    container_name: sdu-server
    volumes:
      - ./data:/opt/sdu-server/data
    ports:
      - 9080:9080/tcp
      - 443:443/tcp
    restart: always
```
- image: `ssvembeddedde/ssv-sdu-server:<version>`, die letzte verfügbare Version wählen
- volumes: `<lokales verzeichnis>:/opt/sdu-server/data`
- ports: es müssen folgende Ports exportiert werden:
	- 9080: Konfigurationsport, für SSV reserviert
	- 443: SDU API Port

### 2. SDU-Server starten:
```bash
docker-compose up -d
```
Nach dem starten wird in dem Datenverzeichnis eine `events` Datenbank und `license` Key erstellt.
```bash
ls data/
events  license
```
Um zu testen ob der Server im internet erreichbar ist im Browser die URL `http://<FQDN>:9080/` aufrufen. Die Anfrage wird beantwortet mit:
```json
{"version":1,"error":"Endpoint does not exist"}
```

### 3. Initialer upload
Damit das Herunterladen mit Blob diff funktioniert muss initial ein NULL Blob hochgeladen werden gegen welchen die diffs erstellt werden.
Beispiel mit curl, benötigt werden die Maintaner key und crt Dateien:
```
CERT=<maintainer.crt.pem>
KEY=<maintainer.key.pem>
SERVER=<FQDN>
touch zero
curl -s -X POST --cacert ssv-server-pki.crt.pem --cert $CERT --key $KEY --data-binary @zero "https://$SERVER/v2/blob/$(sha256sum zero | cut -d' ' -f1)"
```
