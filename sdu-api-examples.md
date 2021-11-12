# SDU API Beispiele

Beispiele mit Bash, Openssl und Curl.
* Informationen:
	- [SDU Manifest Datei](sdu-manifest.md) 
	- [SDU REST API](sdu-api.md)
* Voraussetzung:
	- Firmware BLOB Datei welche hochgeladen werden soll: `myfirmware.tar`
	- Maintainer Key zum signieren und hochladen: `mykey.pfx`
	- SDU-Server CA Zertifikat zum hochladen: [ssv-server-pki.crt.pem](https://github.com/eduard-neufeld-ssv/SDU/ssv-server-pki.crt.pem)
	- SDU-Server Adresse: `https://ssvdev-sdu0.ssv-service.de`

Key und Zertifikat wird vorher aus PFX extrahiert da OpenSSL nicht mit PFX signieren kann.
```bash
openssl pkcs12 -in mykey.pfx -nodes -nocerts -out mykey.key
openssl pkcs12 -in mykey.pfx -clcerts -nokeys -out mykey.crt
```
Umgebungswariablen setzen um folgende Befehle komfortabler zu benutzen
```bash
PATHCA=ssv-server-pki.crt.pem
PATHCRT=mykey.crt
PATHKEY=mykey.key
SDUSERVER=https://ssvdev-sdu0.ssv-service.de
```

## Produkte und Versionen anzeigen
- Produkte anzeigen (das Tool jq dient nur zur formatierung der Ausgabe):
```bash
curl -s --cacert $PATHCA --cert $PATHCRT --key $PATHKEY $SDUSERVER/v2/product | jq
```
- Versionen eines Produkts Namens `dummy` anzeigen:
```bash
PRODUCT=dummy
curl -s --cacert $PATHCA --cert $PATHCRT --key $PATHKEY $SDUSERVER/v2/product/$PRODUCT/version | jq
```

## Blob und Manifest hochladen
* Manifest datei erstellen
```bash
BLOB=myfirmware.tar
PRODUCT=dummy
VERSION=1.2.3
SHA256=$(sha256sum $BLOB | cut -d' ' -f1)
cat > $BLOB.manifest <<-EOF
{
    "product": "$PRODUCT",
    "version": "$VERSION",
    "sha256": "$SHA256",
    "comment": ""
}
EOF
```

* Manifest Datei signieren.
```bash
openssl cms -sign -signer $PATHCRT -inkey $PATHKEY <$BLOB.manifest >$BLOB.manifest.cms
```

* Blob hochladen
```bash
curl -s -X POST --cacert $PATHCA --cert $PATHCRT --key $PATHKEY --data-binary @$BLOB $SDUSERVER/v2/blob/$SHA256
```

* Manifest hochladen
```bash
curl -s -X POST --cacert $PATHCA --cert $PATHCRT --key $PATHKEY --data-binary @$BLOB.manifest.cms -H "Content-Type: application/cms" $SDUSERVER/v2/product/$PRODUCT/version/$VERSION/manifest
```
