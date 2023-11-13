Daten und Modelle als Grundlage für die Vorbereitung der Workshops

## Validierung Testdatensatz

Die Validierung des 2.4-Datensatzes mit dem Katalog benötigt eine SNAPSHOT-Version des ilivalidators.

```
java -jar "\ilivalidator\ilivalidator-1.13.4-UNSTABLE-DEV-SNAPSHOT.jar" --allObjectsAccessible --trace --modeldir ".\;https://models.interlis.ch" LWB_Landwirtschaftliche_Zonengrenzen_Testdata_ILI24.xtf LWB_Landwirtschaftliche_Zonengrenzen_Kataloge_V2_0_ILI24.xml
```

Testversion aus dem 2.4-Build ist unter \tools abgelegt.