# Wurm Online Skill Collector
## Allgemein
Das Projekt ist ein einfaches Programm, welches die Spieler Skills aus Wurm Online in eine Google Docs Tablelle schreibt. 
Aktuell muss die Tabelle auf das Projekt angepasst sein.

## Anwendung
### Einrichtung
Das Programm ist ein Single-File-Executable, weshalb diese nicht installiert werden muss.
Die `WurmSkillCollector.exe` muss heruntergeladen und in den Installationsordner von Wurm Online verschoben werden.
Es ist empfehlenswert sich eine Verknüpfung auf den z.B. Desktop zu legen.

Wenn Wurm über Steam installiert wurde, befindet sich der Ordner unter `C:\Program Files\Steam\steamapps\common\Wurm Online` oder 
in Steam Rechtsklick -> Verwalten -> Lokale Dateien durchsuchen.

In Wurm Online muss die Option `Dump Skills To File` aktiviert sein. Diese wird nach dem Beenden des Spiels geschrieben.

### Ausführung
Um einen Charakter in die GoogleDocs Tabelle zu übertragen muss der Name vorher in der Tabelle existieren.

Im Ordner muss die Datei ausgeführt werden, Windows sollte dabei keine Rechte anfordern. Daraufhin wird versucht den aktuellsten Log zu finden und in die Tabelle zu schreiben. 
Das Programm läuft nur einmal und muss bei erneutem schreiben neu ausgeführt werden.

## Entwicklung

- Um das Python Script auszuführen muss die `config.ini.sample` kopiert und zu `config.ini` umbenannt werden. 
- Die sheet_id von dem GoogleDocs muss eingetragen werden (diese kann aus der Url kopiert werden). 
- Es wird ein Google Developer Account benötigt um das schreiben auf Docs zu ermöglichen.
  - unter https://console.developers.google.com sollte ein neues Projekt erstellt werden
  - unter https://console.developers.google.com/apis/library muss der Dienst Google Sheets API muss aktiviert werden.
  - unter https://console.developers.google.com/apis/credentials muss ein Service-Worker (Dienstkonto) erstellt werden.
  Es muss ein Key erstellt werden und die Json Datei runtergeladen werden. Diese muss in dem Projekt unter `service_account.json` 
  gespeichert werden.
  - Die E-Mail des Service-Workers muss in der Google Tabelle als Person freigegeben werden. Ein öffentlicher Link ist nicht ausreichent.

Um das Programm in Python zu testen muss der `players_path` überschrieben werden.
Das Programm kann nun ausgeführt werden.

Um das Projekt zu builden wird pyinstaller (https://pyinstaller.readthedocs.io) genutzt.
Der Erstellungbefehl ist:
```
pyinstaller -n WurmSkillCollector --onefile --add-data "service_account.json;." --add-data "config.ini;." main.py
```

## Fehler

#### Google
Das Programm nutzt einen Service-Worker welcher auf 100 Lesevorgänge pro 100 Sekunden begrenzt ist. Da 2 Mal gelesen werden muss, 
können nur 50 Spieler in 100 Sekunden die Tabelle beschreiben. Das Tageskontigent ist unlimitiert, versucht es einfach später erneut.
Sollte es öfters zu Problemen kommen, sprecht mich an.


```
Verbinde zu GoogleDocs...
{'code': 429, 'message': "Quota exceeded for quota group 'ReadGroup' and limit 'Read requests per user per 100 seconds' of service 'sheets.googleapis.com' for consumer 'project_number:'.", 'status': 'RESOURCE_EXHAUSTED', 'details': [{'@type': 'type.googleapis.com/google.rpc.Help', 'links': [{'description': 'Google developer console API key'}]}]}
```

#### Fehler
Sollte ein Fehler auftreten, nutzt bitte den Issue-Tracker von Github.