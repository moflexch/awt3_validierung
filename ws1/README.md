## Dokumentation zum Workshop 1: _Datenqualität und INTERLIS: ilivalidator nutzen und verstehen_

### 1. Herunterladen der Verzeichnisstruktur:
Quelle: [Release auf Github](https://github.com/moflexch/awt3_validierung/releases/download/ws1ready/ws_1.zip)

**Inhalt der ZIP-Datei**
* ilivalidator (Entwicklerversion)
* Verzeichnis `model` mit dem vorgestellten Workshop-Modell
* Verzeichnis `data` mit den Transferdateien (.xtf) und den Katalogdaten (.xml)

-> Sichtung der Dateien (ili, xtf)

### 2. Test: Ist Java installiert?
`java -version`

_Output (Beispiel, alles > 1.6 ist i.O.)_
```
openjdk version "1.8.0_282"
OpenJDK Runtime Environment (AdoptOpenJDK)(build 1.8.0_282-b08)
OpenJDK 64-Bit Server VM (AdoptOpenJDK)(build 25.282-b08, mixed mode)
```

### 3. Test: Läuft ilivalidator?
`java -jar ilivalidator-1.13.4-dev\ilivalidator-1.13.4-dev.jar --version`

_Output_
```
ilivalidator, Version 1.13.4-UNSTABLE-DEV-SNAPSHOT-8b7398404cf08aa917bb688444649179deb7b36b
Developed by Eisenhut Informatik AG, CH-3400 Burgdorf
```

### 4. ilivalidator mittels grafischer Benutzeroberfläche starten (nicht Bestandteil des Workshops)
`java -jar ilivalidator-1.13.4-dev\ilivalidator-1.13.4-dev.jar` <- Start ohne Optionen; hat selben Effekt wie Doppelklick auf Dateiname im Explorer.

### 5. Validierungsversuch 1 (gescheitert)
ilivalidator aufrufen unter Angabe der zu prüfenden Datei (im Verzeichnis \\data):

`java -jar ilivalidator-1.13.4-dev\ilivalidator-1.13.4-dev.jar data\LWB_Landwirtschaftliche_Zonengrenzen_Testdata_ILI24.xtf`

---

HINWEIS:
Wenn Dateipfade Leerschläge enthalten, so müssen sie in Hochkommas (') oder Anführungszeichen (") gesetzt werden!

---

Ausgabe endet mit folgendem Fehler:

```
Error: LWB_Landwirtschaftliche_Zonengrenzen_V2_0_ILI24: model(s) not found
```

#### Interpretation:
Das Workshop-Modell wird nicht gefunden.

### 6. Validierungsversuch 2 (gescheitert)
ilivalidator aufrufen unter Angabe der zu prüfenden Datei und des lokalen Verzeichnisses, wo das Modell gespeichert ist (\\model):

`java -jar ilivalidator-1.13.4-dev\ilivalidator-1.13.4-dev.jar --modeldir model data\LWB_Landwirtschaftliche_Zonengrenzen_Testdata_ILI24.xtf`

Ausgabe endet mit folgendem Fehler:

```
Error: GeometryCHLV95_V2: model(s) not found
```

#### Interpretation:
Das Workshop-Modell wird nun gefunden, jedoch ist das Modell `GeometryCHLV95_V2`, welches ja im Workshop-Modell importiert wird, unauffindbar. Dieses ist jedoch in Repository des Bundes unter http://models.geo.admin.ch abgelegt.

### 7. Validierungsversuch 3 (erfolgreich)
ilivalidator aufrufen unter Angabe der zu prüfenden Datei, des lokalen Modellverzeichnisses _und_ des Repositorys des Bundes:

`java -jar ilivalidator-1.13.4-dev\ilivalidator-1.13.4-dev.jar --modeldir "model;https://models.geo.admin.ch" data\LWB_Landwirtschaftliche_Zonengrenzen_Testdata_ILI24.xtf`

Ausgabe endet mit folgender Meldung:

```
Info:      98 objects in CLASS LWB_Landwirtschaftliche_Zonengrenzen_V2_0_ILI24.Zonengrenzen.LZ_Flaeche
Info:       3 objects in CLASS LWB_Landwirtschaftliche_Zonengrenzen_V2_0_ILI24.Zonengrenzen.MeliorationsProjekt
Info: ...validation done
Info: End date 2023-11-22 13:49 validation took 00h:00m:07s.0267ms
```

#### Interpretation:
Die übergebene Datei wurde erfolgreich gegen das zugehörige Modell geprüft; die Datei ist also modellkonform.

## Umgang mit fehlerhaften Daten

### 8. Fehlende Werte
ilivalidator aufrufen und fehlerhafte Transferdatei `Zonengrenzen_Testdata_A.xtf` übergeben:

Ausgabe:

```
Info:      98 objects in CLASS LWB_Landwirtschaftliche_Zonengrenzen_V2_0_ILI24.Zonengrenzen.LZ_Flaeche
Info:       3 objects in CLASS LWB_Landwirtschaftliche_Zonengrenzen_V2_0_ILI24.Zonengrenzen.MeliorationsProjekt
Info: ...validation failed
```

Weiter oben im Ausgabefenster erscheinen folgende Fehlermeldungen:

```
Error: line 80999: LWB_Landwirtschaftliche_Zonengrenzen_V2_0_ILI24.Zonengrenzen.MeliorationsProjekt: tid a49ac986-82f8-11ee-b962-0242ac120002: Attribute ProjektNummer requires a value
Error: line 81004: LWB_Landwirtschaftliche_Zonengrenzen_V2_0_ILI24.Zonengrenzen.MeliorationsProjekt: tid 9170955e-82fb-11ee-b962-0242ac120002: Attribute ProjektNummer requires a value
```

#### Fehleranalyse:
Öffnen der Transferdatei und Suche nach den beanstandeten TID:

```xml
<LWB_Landwirtschaftliche_Zonengrenzen_V2_0_ILI24:MeliorationsProjekt ili:tid="a49ac986-82f8-11ee-b962-0242ac120002">
	<LWB_Landwirtschaftliche_Zonengrenzen_V2_0_ILI24:Bezeichnung>Melioration Vorderberg Süd</LWB_Landwirtschaftliche_Zonengrenzen_V2_0_ILI24:Bezeichnung>
	<LWB_Landwirtschaftliche_Zonengrenzen_V2_0_ILI24:Status>abgeschlossen</LWB_Landwirtschaftliche_Zonengrenzen_V2_0_ILI24:Status>
	<LWB_Landwirtschaftliche_Zonengrenzen_V2_0_ILI24:Eigenschaften>Teilmelioration</LWB_Landwirtschaftliche_Zonengrenzen_V2_0_ILI24:Eigenschaften>
</LWB_Landwirtschaftliche_Zonengrenzen_V2_0_ILI24:MeliorationsProjekt>
```

sowie

```xml
<LWB_Landwirtschaftliche_Zonengrenzen_V2_0_ILI24:MeliorationsProjekt ili:tid="9170955e-82fb-11ee-b962-0242ac120002">
	<LWB_Landwirtschaftliche_Zonengrenzen_V2_0_ILI24:ProjektNummer></LWB_Landwirtschaftliche_Zonengrenzen_V2_0_ILI24:ProjektNummer>
	<LWB_Landwirtschaftliche_Zonengrenzen_V2_0_ILI24:Bezeichnung>Melioration Oberberg Ost</LWB_Landwirtschaftliche_Zonengrenzen_V2_0_ILI24:Bezeichnung>
	<LWB_Landwirtschaftliche_Zonengrenzen_V2_0_ILI24:Status>abgeschlossen</LWB_Landwirtschaftliche_Zonengrenzen_V2_0_ILI24:Status>
	<LWB_Landwirtschaftliche_Zonengrenzen_V2_0_ILI24:Eigenschaften>Gesamtmelioration</LWB_Landwirtschaftliche_Zonengrenzen_V2_0_ILI24:Eigenschaften>
	<LWB_Landwirtschaftliche_Zonengrenzen_V2_0_ILI24:Eigenschaften>SBB Neubaustrecke</LWB_Landwirtschaftliche_Zonengrenzen_V2_0_ILI24:Eigenschaften>
	<LWB_Landwirtschaftliche_Zonengrenzen_V2_0_ILI24:Eigenschaften>NSM 322</LWB_Landwirtschaftliche_Zonengrenzen_V2_0_ILI24:Eigenschaften>
</LWB_Landwirtschaftliche_Zonengrenzen_V2_0_ILI24:MeliorationsProjekt>
```

Die zugehörige Attributdefinition im INTERLIS-Modell lautet (Klasse _MeliorationsProjekt_):

```python
ProjektNummer : MANDATORY ProjektID;
```

#### Interpretation:
Gemäss Datenmodell muss jedes Objekt der Klasse _MeliorationsProjekt_ obligatorisch (`MANDATORY`) ein Attribut `ProjektNummer` besitzen. Beim ersten bemängelten Objekt (tid="a49a...") fehlt das XML-Element "ProjektNummer" komplett. Beim zweiten bemängelten Objekt (tid="9170...") ist das XML-Element leer.

### 9. Falsche Werte
ilivalidator aufrufen und fehlerhafte Transferdatei `Zonengrenzen_Testdata_B.xtf` übergeben:

Ausgabe:

```
Error: line 12: <Modellname>.<Topicname>.LZ_Flaeche: tid a450d6b0-2c7d-4286-91d8-b2bc292c886b: date value <2015-12-32> is not in range in attribute Aenderungsdatum
Error: line 80992: <Modellname>.<Topicname>.MeliorationsProjekt: tid fa3ed182-82eb-11ee-b962-0242ac120002: value lauffend is not a member of the enumeration in attribute Status
```

#### Fehleranalyse:
Öffnen der Transferdatei und Suche nach den beanstandeten TID:

```xml
<LWB_Landwirtschaftliche_Zonengrenzen_V2_0_ILI24:LZ_Flaeche ili:tid="a450d6b0-2c7d-4286-91d8-b2bc292c886b">
	<LWB_Landwirtschaftliche_Zonengrenzen_V2_0_ILI24:rProjekt ili:ref="9170955e-82fb-11ee-b962-0242ac120002"/>
	<LWB_Landwirtschaftliche_Zonengrenzen_V2_0_ILI24:Flaeche> ...
	</LWB_Landwirtschaftliche_Zonengrenzen_V2_0_ILI24:Flaeche>
	<LWB_Landwirtschaftliche_Zonengrenzen_V2_0_ILI24:Aenderungsdatum>2015-12-32</LWB_Landwirtschaftliche_Zonengrenzen_V2_0_ILI24:Aenderungsdatum>
	<LWB_Landwirtschaftliche_Zonengrenzen_V2_0_ILI24:Typ ili:ref="41"/>
</LWB_Landwirtschaftliche_Zonengrenzen_V2_0_ILI24:LZ_Flaeche>
```

sowie

```xml
<LWB_Landwirtschaftliche_Zonengrenzen_V2_0_ILI24:MeliorationsProjekt ili:tid="fa3ed182-82eb-11ee-b962-0242ac120002">
	<LWB_Landwirtschaftliche_Zonengrenzen_V2_0_ILI24:ProjektNummer>2022-AG</LWB_Landwirtschaftliche_Zonengrenzen_V2_0_ILI24:ProjektNummer>
	<LWB_Landwirtschaftliche_Zonengrenzen_V2_0_ILI24:Bezeichnung>Melioration Hinterberg Nord</LWB_Landwirtschaftliche_Zonengrenzen_V2_0_ILI24:Bezeichnung>
	<LWB_Landwirtschaftliche_Zonengrenzen_V2_0_ILI24:Status>lauffend</LWB_Landwirtschaftliche_Zonengrenzen_V2_0_ILI24:Status>
	<LWB_Landwirtschaftliche_Zonengrenzen_V2_0_ILI24:Eigenschaften>Gesamtmelioration</LWB_Landwirtschaftliche_Zonengrenzen_V2_0_ILI24:Eigenschaften>
	<LWB_Landwirtschaftliche_Zonengrenzen_V2_0_ILI24:Eigenschaften>Los 4 J4</LWB_Landwirtschaftliche_Zonengrenzen_V2_0_ILI24:Eigenschaften>
</LWB_Landwirtschaftliche_Zonengrenzen_V2_0_ILI24:MeliorationsProjekt>
```

Die zugehörige Attributdefinition im INTERLIS-Modell lautet (Klassen _LZ\_Flaeche_ und _MeliorationsProjekt_):

```python
CLASS LZ_Flaeche =
  ...
  Aenderungsdatum : MANDATORY INTERLIS.XMLDate;
  ...
END LZ_Flaeche;

CLASS MeliorationsProjekt =
  ...
  Status : MANDATORY (geplant,laufend,abgeschlossen);
  ...
END MeliorationsProjekt;
```

#### Interpretation:
Gemäss Datenmodell muss jedes Objekt der Klasse _LZ\_Flaeche_ ein Attribut `Aenderungsdatum` besitzen, welches ein gültiges Datum (`INTERLIS.XMLDate`) enthält. Das Datum beim ersten bemängelten Objekt (tid="a450...") ist ungültig (32. Dezember).
Beim zweiten bemängelten Objekt (tid="fa3e...") gehört der Wert im Attribut `Status` (_lauffend_) nicht zu den zulässigen Werten (falsche Rechtschreibung).

### 10. Doppelte Werte
ilivalidator aufrufen und fehlerhafte Transferdatei `Zonengrenzen_Testdata_C.xtf` übergeben:

Ausgabe:

```
Error: line 80992: <Modellname>.<Topicname>.MeliorationsProjekt: tid fa3ed182-82eb-11ee-b962-0242ac120002: Unique constraint <Modellname>.<Topicname>.MeliorationsProjekt.Constraint1 is violated! Values 2022-AG already exist in Object: a49ac986-82f8-11ee-b962-0242ac120002
```

#### Fehleranalyse:
Öffnen der Transferdatei und Suche nach den beanstandeten TID:

```xml
<LWB_Landwirtschaftliche_Zonengrenzen_V2_0_ILI24:MeliorationsProjekt ili:tid="fa3ed182-82eb-11ee-b962-0242ac120002">
	<LWB_Landwirtschaftliche_Zonengrenzen_V2_0_ILI24:ProjektNummer>2022-AG</LWB_Landwirtschaftliche_Zonengrenzen_V2_0_ILI24:ProjektNummer>
	<LWB_Landwirtschaftliche_Zonengrenzen_V2_0_ILI24:Bezeichnung>Melioration Hinterberg Nord</LWB_Landwirtschaftliche_Zonengrenzen_V2_0_ILI24:Bezeichnung>
	<LWB_Landwirtschaftliche_Zonengrenzen_V2_0_ILI24:Status>laufend</LWB_Landwirtschaftliche_Zonengrenzen_V2_0_ILI24:Status>
	<LWB_Landwirtschaftliche_Zonengrenzen_V2_0_ILI24:Eigenschaften>Gesamtmelioration</LWB_Landwirtschaftliche_Zonengrenzen_V2_0_ILI24:Eigenschaften>
	<LWB_Landwirtschaftliche_Zonengrenzen_V2_0_ILI24:Eigenschaften>Los 4 J4</LWB_Landwirtschaftliche_Zonengrenzen_V2_0_ILI24:Eigenschaften>
</LWB_Landwirtschaftliche_Zonengrenzen_V2_0_ILI24:MeliorationsProjekt>
<LWB_Landwirtschaftliche_Zonengrenzen_V2_0_ILI24:MeliorationsProjekt ili:tid="a49ac986-82f8-11ee-b962-0242ac120002">
	<LWB_Landwirtschaftliche_Zonengrenzen_V2_0_ILI24:ProjektNummer>2022-AG</LWB_Landwirtschaftliche_Zonengrenzen_V2_0_ILI24:ProjektNummer>
	<LWB_Landwirtschaftliche_Zonengrenzen_V2_0_ILI24:Bezeichnung>Melioration Vorderberg Süd</LWB_Landwirtschaftliche_Zonengrenzen_V2_0_ILI24:Bezeichnung>
	<LWB_Landwirtschaftliche_Zonengrenzen_V2_0_ILI24:Status>abgeschlossen</LWB_Landwirtschaftliche_Zonengrenzen_V2_0_ILI24:Status>
	<LWB_Landwirtschaftliche_Zonengrenzen_V2_0_ILI24:Eigenschaften>Teilmelioration</LWB_Landwirtschaftliche_Zonengrenzen_V2_0_ILI24:Eigenschaften>
</LWB_Landwirtschaftliche_Zonengrenzen_V2_0_ILI24:MeliorationsProjekt>
```

Die zugehörige Beschreibung im INTERLIS-Modell lautet (Klasse _MeliorationsProjekt_):

```python
CLASS MeliorationsProjekt =
  ProjektNummer : MANDATORY ProjektID;
  Bezeichnung : TEXT*200;
  Status : MANDATORY (geplant,laufend,abgeschlossen);
  Eigenschaften : BAG {0..3} OF TEXT*50;
  UNIQUE 
    ProjektNummer;
END MeliorationsProjekt;
```

#### Interpretation:
Gemäss Datenmodell darf ein Wert im Attribut `ProjektNummer` nur bei genau einem Objekt vorkommen (`UNIQUE ProjektNummer`). Der Wert _2022-AG_ erscheint jedoch in beiden bemängelten Objekten im Attribut `ProjektNummer`.

### 11. Fehlende Beziehung (association)
ilivalidator aufrufen und fehlerhafte Transferdatei `Zonengrenzen_Testdata_D.xtf` übergeben:

Ausgabe:

```
Error: line 2957: <Modellname>.<Topicname>.LZ_Flaeche: tid 388ef3d5-1721-402b-b5ff-274f2ac60edd: No object found with OID fa3ed182-82eb-11ee-b962-0242ac120001 in basket LWB_Landwirtschaftliche_Zonengrenzen_V2_0_ILI24.Zonengrenzen.
...
Error: line 80992: <Modellname>.<Topicname>.MeliorationsProjekt: tid fa3ed182-82eb-11ee-b962-0242ac120002: rZone should associate 2 to * target objects (instead of 1)
```

#### Fehleranalyse:
Öffnen der Transferdatei und Suche nach den beanstandeten TID:

```xml
<LWB_Landwirtschaftliche_Zonengrenzen_V2_0_ILI24:LZ_Flaeche ili:tid="388ef3d5-1721-402b-b5ff-274f2ac60edd">
	<LWB_Landwirtschaftliche_Zonengrenzen_V2_0_ILI24:rProjekt ili:ref="fa3ed182-82eb-11ee-b962-0242ac120001"/>
	<LWB_Landwirtschaftliche_Zonengrenzen_V2_0_ILI24:Flaeche> ...
	</LWB_Landwirtschaftliche_Zonengrenzen_V2_0_ILI24:Flaeche>
	<LWB_Landwirtschaftliche_Zonengrenzen_V2_0_ILI24:Aenderungsdatum>2015-12-03</LWB_Landwirtschaftliche_Zonengrenzen_V2_0_ILI24:Aenderungsdatum>
	<LWB_Landwirtschaftliche_Zonengrenzen_V2_0_ILI24:Typ ili:ref="41"/>
</LWB_Landwirtschaftliche_Zonengrenzen_V2_0_ILI24:LZ_Flaeche>
```

sowie

```xml
<LWB_Landwirtschaftliche_Zonengrenzen_V2_0_ILI24:MeliorationsProjekt ili:tid="fa3ed182-82eb-11ee-b962-0242ac120002">
	<LWB_Landwirtschaftliche_Zonengrenzen_V2_0_ILI24:ProjektNummer>2022-AG</LWB_Landwirtschaftliche_Zonengrenzen_V2_0_ILI24:ProjektNummer>
	<LWB_Landwirtschaftliche_Zonengrenzen_V2_0_ILI24:Bezeichnung>Melioration Hinterberg Nord</LWB_Landwirtschaftliche_Zonengrenzen_V2_0_ILI24:Bezeichnung>
	<LWB_Landwirtschaftliche_Zonengrenzen_V2_0_ILI24:Status>laufend</LWB_Landwirtschaftliche_Zonengrenzen_V2_0_ILI24:Status>
	<LWB_Landwirtschaftliche_Zonengrenzen_V2_0_ILI24:Eigenschaften>Gesamtmelioration</LWB_Landwirtschaftliche_Zonengrenzen_V2_0_ILI24:Eigenschaften>
	<LWB_Landwirtschaftliche_Zonengrenzen_V2_0_ILI24:Eigenschaften>Los 4 J4</LWB_Landwirtschaftliche_Zonengrenzen_V2_0_ILI24:Eigenschaften>
</LWB_Landwirtschaftliche_Zonengrenzen_V2_0_ILI24:MeliorationsProjekt>
```

Die zugehörige Beziehung im INTERLIS-Modell lautet:

```python
  ASSOCIATION LZFlaecheProjekt = 
    rZone -- {2..*} LZ_Flaeche;   
    rProjekt -- {0..1} MeliorationsProjekt;
  END LZFlaecheProjekt;
```

#### Interpretation:
Gemäss Datenmodell kann ein Objekt der Klasse _LZ\_Flaeche_ eine Beziehung zu einem Objekt der Klasse _MeliorationsProjekt_ besitzen (Rolle _rProjekt -- {0..1} MeliorationsProjekt_). In der Transferdatei wird dies durch eine Referenz auf die TID eines anderen Objektes realisiert. Im obigen (ersten) Ausschnitt aus der Transferdatei wird jedoch bei dieser Referenz (rProjekt) eine TID angegeben, welche im Datensatz (Basket) nicht vorkommt - es wird kein Objekt gefunden.

Gemäss Datenmodell muss ein Objekt der Klasse _MeliorationsProjekt_ zu mindestens 2 Objekten der Klasse _LZ\_Flaeche_ eine Beziehung haben (Rolle _rZone -- {2..*} LZ\_Flaeche_). Eine Suche in der Transferdatei nach der TID ("fa3ed182...") ergibt 2 Treffer (davon jedoch nur 1 als Referenz). 

### 12. Fehlerhafte Geometrie
ilivalidator aufrufen und fehlerhafte Transferdatei `Zonengrenzen_Testdata_E.xtf` übergeben:

Ausgabe:

```
Error: <Modellname>.<Topicname>.LZ_Flaeche.Flaeche: dangle tid 554f6492-7995-47cb-aa90-77a88ac7dcf9
Error: no polygon
Info: AREA topology of attribute Flaeche not validated, validation of SURFACE topology failed in attribute Flaeche
```

#### Fehleranalyse
Öffnen der Transferdatei und Suche nach den beanstandeten TID:

```xml
<LWB_Landwirtschaftliche_Zonengrenzen_V2_0_ILI24:LZ_Flaeche ili:tid="554f6492-7995-47cb-aa90-77a88ac7dcf9">
	<LWB_Landwirtschaftliche_Zonengrenzen_V2_0_ILI24:rProjekt ili:ref="9170955e-82fb-11ee-b962-0242ac120002"/>
	<LWB_Landwirtschaftliche_Zonengrenzen_V2_0_ILI24:Flaeche>
		<geom:surface>
			<geom:exterior>
				<geom:polyline>
					<geom:coord>
						<geom:c1>2712054.361</geom:c1>
						<geom:c2>1278220.478</geom:c2>
					</geom:coord>
					<geom:coord>
          ...
					</geom:coord>
					<geom:coord>
						<geom:c1>2712031.608</geom:c1>
						<geom:c2>1278265.826</geom:c2>
					</geom:coord>
				</geom:polyline>
			</geom:exterior>
		</geom:surface>
	</LWB_Landwirtschaftliche_Zonengrenzen_V2_0_ILI24:Flaeche>
	<LWB_Landwirtschaftliche_Zonengrenzen_V2_0_ILI24:Aenderungsdatum>2015-12-03</LWB_Landwirtschaftliche_Zonengrenzen_V2_0_ILI24:Aenderungsdatum>
	<LWB_Landwirtschaftliche_Zonengrenzen_V2_0_ILI24:Typ ili:ref="41"/>
</LWB_Landwirtschaftliche_Zonengrenzen_V2_0_ILI24:LZ_Flaeche>
```

#### Interpretation:
In INTERLIS werden Flächen durch Polygone gebildet. In einem Polygon müssen Startpunkt und Endpunkt identisch sein. In obigen Beispiel ist dies nicht der Fall, was als Fehler zurückgemeldet wird.

## Verwenden von Protokolldateien (Logs)

### 13. Einfaches Protokollieren (log)
ilivalidator aufrufen und fehlerhafte Transferdatei `Zonengrenzen_Testdata_F.xtf` übergeben:

Ausgabe:
Sehr viele Zeilen, es wird unüberschaubar...

ilivalidator aufrufen unter Angabe einer Logdatei:

`java -jar ilivalidator-1.13.4-dev\ilivalidator-1.13.4-dev.jar --log Zonengrenzen_Testdata_F.log --modeldir "model;https://models.geo.admin.ch" data\Zonengrenzen_Testdata_F.xtf`

Ausgabe:

```
Error: line 12: <Modellname>.<Topicname>.LZ_Flaeche: tid a450d6b0-2c7d-4286-91d8-b2bc292c886b: date value <22-11-2023> is not in range in attribute Aenderungsdatum
Info: see <Zonengrenzen_Testdata_E.log> for more validation results
```

Alle Fehlermeldungen werden nun in eine Datei geschrieben (log). Falls man Daten als xtf weitergeben muss, so schickt man idealerweise diese Log-Datei mit, damit der/die Empfänger:in beurteilen kann, welche Qualität die gelieferten Daten haben.

### 14. Strukturiertes Protokollieren (xtf)

### Problematik:
Bei der Protokolldatei handelt es sich um eine unstrukturierte Textdatei. Eine automatisierte Prozessierung der Fehlermeldungen ist umständlich. Schön wäre, wenn man eine strukturierte Ausgabe der Fehler hätte...

Was eignet sich besser als INTERLIS, um Daten strukturiert zu beschreiben?! :wink:

### Lösung:
Das Modell [IliVErrors](https://models.interlis.ch/tools/IliVErrors.ili) beschreibt die Fehlerausgabe des ilivalidators. Es enthält nur 1 Klasse:

```python
MODEL IliVErrors
  AT "mailto:ceis@localhost"
  VERSION "2016-06-10" =

  TOPIC ErrorLog =

    CLASS Error =
       Message: MANDATORY TEXT*255; !! Sprachspezifische Fehlermeldungen inkl. aufgeloeste Platzhalter/Attributwerte aber ohne Tid, TechId, UserId, usw.
       Type: MANDATORY (
               Error,
               Warning, !! Fehler, die der Benutzer als akzeptabel konfiguriert hat
               Info, !! Info, z.B. Software-Version
               DetailInfo !! Wie Info, aber weniger wichtig
             );
       ObjTag: TEXT*255; !! Qualifizierter Topic- oder Class-Name worauf sich die TID bezieht, z.B. "Beispiel1.Bodenbedeckung.Gebaeude" ; Bei Programmfehlern nicht vorhanden.
       Tid: TEXT*40; !! Bei Check auf der DB evtl. nicht vorhanden
       TechId: TEXT*40; !! Der Software interne Schluessel
       UserId: TEXT*255; !! Text mit dem fachlichen Schluessel (falls bekannt), z.B. "NBIdent 255, Nummer 30"
       IliQName: TEXT*255; !! Qualifizierter Topic-, Class-, Constraint-, Rollen- oder Attribut-Name, z.B. "Beispiel1.Bodenbedeckung.Gebaeude.Art" ; Bei Programmfehlern nicht vorhanden.
       DataSource: TEXT*255; !! z.B. Dateiname oder DB-Name
       Line: 1 .. 1000000000; !! Zeilennummer; Bei Check auf der DB evtl. noch nicht vorhanden
       Geometry : COORD 2480000.00 .. 2850000.00, 1060000.00 .. 1320000.00; !! die Fehlerkoordinate oder Koordinate im/beim Objekt
       TechDetails : MTEXT; !! evtl. technische Details, z.B. ein Stacktrace
    END Error;

  END ErrorLog;

END IliVErrors.
```

### Anwendung:
ilivalidator aufrufen unter Verwendung einer Logdatei im xft-Format:

`java -jar ilivalidator-1.13.4-dev\ilivalidator-1.13.4-dev.jar --log Zonengrenzen_Testdata_F.log --xtflog Zonengrenzen_Testdata_F_log.xtf --modeldir "model;https://models.geo.admin.ch" data\Zonengrenzen_Testdata_F.xtf`

Anschliessend lässt sich die strukturierte Log-Datei weiter bearbeiten (z. B. importieren in eine Datenbank (ili2pg, ili2ora usw.), Umwandlung in ein gpkg (ili2gpkg) oder Visualisierung in QGIS mittels PlugIn "XTFLog-Checker"):

![XTFLog Screenshot](/ws1/resources/xtflog-checker.png)

## Gezielte Datenvalidierung
Bei unübersichtlichen Protokoll-Dateien mit vielen tausend Fehlermeldungen wäre es hilfreich, gewisse Meldungen auszublenden oder sogar zu präzisieren. Um diese Anforderung zu erfüllen wurde die Konfigurationsdatei geschaffen:

### 15. Erstellen einer Konfigurationsdatei
Die Konfigurationsdatei ist eine Textdatei, in die zusätzliche Anweisungen geschrieben werden, welche an den ilivalidator weitergegeben werden. Der Name der Konfigurationsdatei kann beliebig sein; häufig wird die Datei jedoch _config.ini_ (oder historisch: _config.toml_) genannt.

Neue Textdatei _config.ini_ mit folgendem Inhalt erstellen:
```python
["PARAMETER"]
validation="off"
```

ilivalidator aufrufen und ihm neben den anderen Optionen den Pfad zum config.ini übergeben. Geprüft wird wiederum die Transferdatei `Zonengrenzen_Testdata_F.xtf`:

`java -jar ilivalidator-1.13.4-dev\ilivalidator-1.13.4-dev.jar --config config.ini --log Zonengrenzen_Testdata_F.log --modeldir "model;https://models.geo.admin.ch" data\Zonengrenzen_Testdata_F.xtf`

### Fehleranalyse
Es gibt keine Fehler. Die Validierung wurde ausgeschaltet, das Logfile erscheint so, als wenn alles fehlerfrei wäre.

### 16. Gezieltes Unterdrücken der Datentyp-Validierung
Die Transferdatei `Zonengrenzen_Testdata_F.xtf` hatte sehr viele Fehler, welche das Attribut _LZ\_Flaeche.Aenderungsdatum_ betrafen. Und zwar wurde dort der Datentyp bemängelt:

```
Error: ... date value <22-11-2023> is not in range in attribute Aenderungsdatum
```

Wir können nun gezielt diesen Fehler unterdrücken, um die Logdatei kleiner und damit übersichtlicher zu machen:

_config.ini_ anpassen (bestehenden Inhalt löschen):
```python
["LWB_Landwirtschaftliche_Zonengrenzen_V2_0_ILI24.Zonengrenzen.LZ_Flaeche.Aenderungsdatum"]
type="off"
```

Das betreffende Attribut wird unter Angabe des vollen Namens (auch _qualifizierter Name_ genannt) angegeben: also in der Form \<Modellname\>.\<Topicname\>.\<Klassenname\>.\<Attributname\>. Danach wird angegeben, welche Eigenschaft konfiguriert (_type_ für Datentyp) und auf welchen Status (_off_) die Prüfung gesetzt werden soll.

Ausgabe:

```
Info: <Modellname>.<Topicname>.LZ_Flaeche.Aenderungsdatum not validated, validation configuration type=off in attribute Aenderungsdatum
```

Damit wird das Logfile auf einen Schlag um 98 Zeilen kürzer, da der Datumsfehler in allen 98 LZ\_Flaeche-Objekten auftritt und nun nicht mehr ausgegeben wird.

Ein weiterer Fehler betraf das Attribut _MelioratonsProjekt.Bezeichnung_: Die Zeichenlänge dieses Attributes ist auf 200 Zeichen begrenzt. Auch hier handelt es sich wieder um eine Datentyp-Prüfung. Die Konfiguration wird wie folgt ergänzt:

```python
["LWB_Landwirtschaftliche_Zonengrenzen_V2_0_ILI24.Zonengrenzen.LZ_Flaeche.Aenderungsdatum"]
type="off"

# Prüfung der Bezeichnungslänge deaktivieren
["LWB_Landwirtschaftliche_Zonengrenzen_V2_0_ILI24.Zonengrenzen.MeliorationsProjekt.Bezeichnung"]
type="off"
```

Mittels # zu Beginn einer Zeile wird ein Kommentar markiert. In der Ausgabe erscheint nun bezüglich der überlangen Bezeichnung kein Fehler mehr.

**Aufgabe**

Konfiguration so anpassen, dass auch beim Attribut _MelioratonsProjekt.Eigenschaften_ kein Fehler mehr ausgegeben wird, wenn die erlaubte Anzahl Zeichen überschritten wird.

### 17. Gezieltes Unterdrücken der Multiplizitäts-Validierung
Beim Attribut _MelioratonsProjekt.Eigenschaften_ taucht eine Fehlermeldung auf, die nicht auf oben beschriebene Art unterdrückt werden kann:

```
Error: ... Attribute Eigenschaften has wrong number of values
```

Hierbei geht es also nicht um die Werte selber, sondern um deren Anzahl. Im Modell steht:

```python
Eigenschaften : BAG {0..3} OF TEXT*50;
```

Es dürfen somit maximal 3 Eigenschaften-Werte pro Objekt vorkommen. Hierbei handelt es sich also nicht um die Validierung des Datentyps, sondern der Multiplizität (multiplicity).

Die Konfiguration wird wie folgt ergänzt:

```python
["LWB_Landwirtschaftliche_Zonengrenzen_V2_0_ILI24.Zonengrenzen.LZ_Flaeche.Aenderungsdatum"]
type="off"

# bei Überschreitung der Bezeichnunslänge warnen
["LWB_Landwirtschaftliche_Zonengrenzen_V2_0_ILI24.Zonengrenzen.MeliorationsProjekt.Bezeichnung"]
type="off"

["LWB_Landwirtschaftliche_Zonengrenzen_V2_0_ILI24.Zonengrenzen.MeliorationsProjekt.Eigenschaften"]
type="off"
multiplicity="off"
```

Beim Attribut _MeliorationsProjekt.Eigenschaften_ wird nun also weder der Typ noch die Multiplizität geprüft. Damit verschwindet auch diese Fehlermeldung.

### 18. Gezieltes Unterdrücken der Prüfung von Beziehungen
Im Logfile ist weiterhin folgender Fehler vorhanden (siehe auch Ziffer 11 Fehlende Beziehung):

```
Error: ... No object found with OID fa3ed182...
```

Wie in Ziffer 11 beschrieben, geht es hierbei um die Referenz auf ein _MeliorationsProjekt_-Objekt, das es nicht gibt. Die Beziehung zwischen den Klassen wird über die Association und insbesondere deren Rollen modelliert:

```python
ASSOCIATION LZFlaecheProjekt = 
  rZone -- {2..*} LZ_Flaeche;   
  rProjekt -- {0..1} MeliorationsProjekt;
END LZFlaecheProjekt;
```

Wenn man also aus Sicht der Klasse _LZ\_Flaeche_ die Beziehung zur Klasse _MeliorationsProjekt_ von der Validierung ausschliessen will, so muss dazu die Rolle _rProjekt_ deaktiviert werden.

Die Konfiguration wird wie folgt ergänzt:

```python
["LWB_Landwirtschaftliche_Zonengrenzen_V2_0_ILI24.Zonengrenzen.LZ_Flaeche.Aenderungsdatum"]
type="off"

# bei Überschreitung der Bezeichnunslänge warnen
["LWB_Landwirtschaftliche_Zonengrenzen_V2_0_ILI24.Zonengrenzen.MeliorationsProjekt.Bezeichnung"]
type="off"

["LWB_Landwirtschaftliche_Zonengrenzen_V2_0_ILI24.Zonengrenzen.MeliorationsProjekt.Eigenschaften"]
type="off"
multiplicity="off"

["LWB_Landwirtschaftliche_Zonengrenzen_V2_0_ILI24.Zonengrenzen.LZFlaecheProjekt.rProjekt"]
target="off"
```

Im Falle von Beziehungen wird der qualifizierte Rollenname (hier _rProjekt_) adressiert und mittels der Eigenschaft _target_ die Validierung der Referenz auf ein vorhandenes Objekt ausgeschaltet. Auch diese Fehlermeldung taucht nun nicht mehr im Logfile auf.

### 19. Gezieltes Unterdrücken der Prüfung von Konsistenzbedingungen (constraints)
INTERLIS 2.4 kennt [5 Konsistenzbedingungen](https://geostandards-ch.github.io/doc_refhb24/#_konsistenzbedingungen). Diese werden im Detail im Workshop 2 behandelt. Im Workshop-Modell ist jedoch in der Klasse _MeliorationsProjekt_ eine Eindeutigkeitsforderung (_uniqueness constraint_) modelliert:

```python
  ...
  UNIQUE 
    ProjektNummer;
  ...
```

Die Sprache INTERLIS erlaubt es, dass Constraints mit einem Namen bezeichnet werden. Dies ist aber in diesem Fall nicht geschehen. Deshalb werden Constraints pro Klasse aufsteigend durchnummeriert: Constraint1, Constraint2, Constraint3 usw.

Wie man der Fehlermeldung entnehmen kann, handelt es sich hierbei um den Constraint1, der verletzt wird:

```
Error: ... Unique constraint <Modellname>.<Topicname>.MeliorationsProjekt.Constraint1 is violated! Values 2022-AG already exist in Object: a49ac986...
```

Mit dieser Information lässt sich die Konfigurationsdatei um folgenden Eintrag ergänzen:

```python
["LWB_Landwirtschaftliche_Zonengrenzen_V2_0_ILI24.Zonengrenzen.LZ_Flaeche.Aenderungsdatum"]
type="off"

# bei Überschreitung der Bezeichnunslänge warnen
["LWB_Landwirtschaftliche_Zonengrenzen_V2_0_ILI24.Zonengrenzen.MeliorationsProjekt.Bezeichnung"]
type="off"

["LWB_Landwirtschaftliche_Zonengrenzen_V2_0_ILI24.Zonengrenzen.MeliorationsProjekt.Eigenschaften"]
type="off"
multiplicity="off"

["LWB_Landwirtschaftliche_Zonengrenzen_V2_0_ILI24.Zonengrenzen.LZFlaecheProjekt.rProjekt"]
target="off"

["LWB_Landwirtschaftliche_Zonengrenzen_V2_0_ILI24.Zonengrenzen.MeliorationsProjekt.Constraint1"]
check="off"
```

Dies hat zur Folge, dass der Constraint1 deaktiviert wird. Damit hat man eine (vermeintlich) fehlerfreie Transferdatei.

### 20. Ergänzen der Fehlermeldung für Konsistenzbedingungen mir eigenem Text
Um Fehlermeldungen für Endnutzer aussagekräftiger zu machen, besteht die Möglichkeit, bei verletzten Konsistenzbedingungen eine auf den Anwendungsfall zugeschnittene Fehlermeldung auszugeben. Dazu wird der letzte Eintrag der Konfigurationsdatei wie folgt abgeändert:

```python
["LWB_Landwirtschaftliche_Zonengrenzen_V2_0_ILI24.Zonengrenzen.MeliorationsProjekt.Constraint1"]
check="warning"
msg="Der Wert <{ProjektNummer}> im Attribut ProjektNummer kommt mehrfach vor."
```

Die Prüfung wird nun nicht mehr ausgeschaltet (check="off") sondern bei einem Fehler wird eine Warnung ausgegeben (check="warning"). In der zurückgegebenen Mitteilung kann ein Platzhalter für einen Attributwert des fehlerhaften Objektes angegeben werden ({ProjektNummer}).

Damit ändert die Standardfehlermeldung von

```
Error: ... Unique constraint <Modellname>.<Topicname>.MeliorationsProjekt.Constraint1 is violated! Values 2022-AG already exist in Object: a49ac986...
```

auf

```
Warning: line 80992: ... Der Wert <2022-AG> im Attribut ProjektNummer kommt mehrfach vor.
```

---

Hinweis: Validierungen, die Warnungen enthalten, gelten als fehlerfrei (_...validation done_).

---

