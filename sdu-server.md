# SDU Server

Secure Device Update Server verwaltet und stellt Firmware für die zu aktualisierende Geräte bereit. Der SDU-Server wird über eine [REST API](sdu-api.md) bedient und steht als ein Docker Image zu verfügung.
Der Workflow für den Betrieb des Servers sieht wie folgt aus:
- SDU-Server Docker auf einem Host starten. Der Host muss eine gültige FQDN besitzen.
- SSV den FQDN des Hosts mitteilen. SSV stellt eine Lizenz aus und spielt diese in den Server. Ebenso werden Schlüssel erstellt für den Zugriff auf den Server.
- Damit ist der SDU-Server betriebsbereit.

## Server starten
1) Docker-Compose Datei anlegen:  
	`docker-compose.yml`:
	``` yaml
	version: '2.1'
	services:
	sdu-server:
		image: ssvembeddedde/ssv-sdu-server:1.0.1
		container_name: sdu-server
		volumes:
		- ./data:/opt/sdu-server/data
		ports:
		- 9080:9080/tcp
		- 443:443/tcp
		restart: always
	```
2) SDU-Server starten:
	``` bash
	docker-compose up
	```
	Um zu testen ob der Server im internet erreichbar ist im Browser die URL `http://<FQDN>:9080/` aufrufen. Die Anfrage wird beantwortet mit:
	``` json
	{"version":1,"error":"Endpoint does not exist"}
	```
3) Email mit der Bitte um aktivierung und der FQDN an SSV senden.  
[support@ssv-embedded.de](mailto:support@ssv-embedded.de)