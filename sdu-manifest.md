# Manifest

Das Manifest enthält alle Meta-Informationen für ein bereitgestelltes Update. 
Es wird vom Signing Tool (SK) erzeugt und vom Gateway Update Client (GUC) 
ausgewertet und die entsprechenden Informationen an den SDU Agenten weitergereicht.

Das Manifest ist eine S/MIME-signierte JSON-Datei. Das Signieren muss von einem **Maintainer** erfolgen.

## Inhalt des Manifests
Body S/MIME-Datei:

```js
{
	"product": "rmg941", // Produktname
	"version": "1.2.4", // Versionsname
	"sha256": "e3b0c44298fc1c149afbf4c8996fb92427ae41e4649b934ca495991b7852b855", // SHA256 der Daten
	"comment": "" // Optional: Kommentar zur Version
	// Produkt-spezifische Felder können bei Bedarf ergänzt werden
}
```
