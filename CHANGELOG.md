# Changelog

Alle wesentlichen Änderungen an diesem Projekt werden in dieser Datei dokumentiert.

Das Format basiert auf [Keep a Changelog](https://keepachangelog.com/de/1.1.0/),
und dieses Projekt hält sich an [Semantic Versioning](https://semver.org/lang/de/).

## [Unreleased]

## [0.2.0] - 2026-11-08

### Geändert
- `Feldstueck/Flurstueckskennung` ist optional (`minOccurs="0"`, vorher Pflicht), da nur einige Bundesländer zusätzlich zur FLIK Flurstückskennungen führen.
- `Feldstueck/Flurstueckskennung`: Annotation am Element ergänzt; die Kennung kann angegeben werden, sofern sie vorhanden ist.
- `Feldstueck/Bezeichnung` ist optional (`minOccurs="0"`), da Agrarverwaltungssysteme keine Feldstück-Bezeichnung erzwingen. `Betrieb/Bezeichnung` bleibt Pflicht.
- `Feldstueck/Nummer` ist optional (`minOccurs="0"`), da die Nummer nicht in allen Agrarverwaltungssystemen vorhanden ist.
- Fehlende optionale Werte werden durch Weglassen des Elements übermittelt, nicht als leeres Element oder mit `xsi:nil`.
- `Bezeichnung`: Dokumentation des globalen Elements neutral formuliert, da es von `Betrieb` und `Feldstueck` verwendet wird.
- `Nummer` in `Feldstueck`: Dokumentation ergänzt (laufende Nummer im Flächenverzeichnis, nicht zwingend vorhanden).
- `NutzungsCode`, `NutzungBezeichnung`: Dokumentation auf die offizielle Bezeichnung InVeKoS-NC bzw. InVeKoS-NC-Liste umgestellt (vorher FNN-Code bzw. FNN-Codeliste).
- `Flaechenwert`: Dokumentation präzisiert, Bezeichnung als Größe einer Fläche klargestellt.

### Behoben
- `Gesamtflaeche/@Einheit`: unzulässige Kombination `use="required"` mit `default="m2"` entfernt. Das Schema ließ sich dadurch nicht kompilieren. Die Einheit bleibt Pflicht, ohne Default-Wert.
- `Bezeichnung`: Annotation korrekt innerhalb der Elementdeklaration platziert.

## [0.1.0] - 2026-04-22

### Hinzugefügt
- `OEKOFLXML.xsd`: initiales XML-Schema für Feldstücksdaten im InVeKo-System.
- `OEKOFLXML.xml`: Beispiel- und Template-Dokument.
- `README.md`: Projektbeschreibung mit Datenmodell und Feldbeschreibungen.
- `CHANGELOG.md`: diese Datei.
- Simple Type `Flaecheneinheit`: Enum für Flächeneinheiten (`m2`, `ha`).
- Simple Type `Flaechenwert`: Dezimaltyp (`xs:decimal`) für Flächenwerte, max. 4 Nachkommastellen, ≥ 0.
- Simple Type `INVEKOSNRType`: InVeKo-Betriebsnummer, exakt 15 Ziffern.
- Simple Type `FLIKType`: FLIK-Kennung im Format `DE[A-Z]{2}[0-9]{12}`.
- Simple Type `MultiPolygonWKT`: WKT MULTIPOLYGON mit mehreren Polygonen, Löchern und negativen Koordinaten.
- Schema-weite `xs:annotation` mit fachlichem Zweck, Geltungsbereich, Version, Autor und Kodierungshinweis (`xml:lang="de"`).
- `xs:appinfo` mit maschinenlesbaren Metadaten (Version, Datum, Kodierung).
- Elementdokumentationen für alle relevanten Felder.

### Behoben
Korrekturen gegenüber dem ersten, unveröffentlichten Entwurf:
- `Gesamtflaeche`, `Flaeche`, `Geometrie`: ungültige Kombination aus `type`-Attribut und `complexType` durch `simpleContent/extension` ersetzt.
- `Gesamtflaeche`, `Flaeche`: `xs:float` durch `xs:decimal` (via `Flaechenwert`) ersetzt.
- `MultiPolygonWKT`-Pattern: negative Koordinaten, mehrere Polygone und Löcher ergänzt.
- `Feldstuecke` in `Betrieb`: `maxOccurs="unbounded"` auf `maxOccurs="1"` korrigiert.
- `Feldstuecke/@Anzahl`: Typ auf `xs:integer` eingeschränkt.
- `Nutzung`: `minOccurs`/`maxOccurs` für alle Felder explizit gesetzt; `Flurstueckskennung` optional (`minOccurs="0"`).
- `OEKOFLXML.xml`: Tippfehler `AnzahlBaume` → `AnzahlBaeume` korrigiert.
- `OEKOFLXML.xml`: `Geometrie/@srid` und WKT-Textinhalt ergänzt.
- `OEKOFLXML.xml`: `NutzungsCode` und `NutzungBezeichnung` korrigiert (waren `Code` und `Bezeichnung`).

[Unreleased]: https://github.com/dietmarbetz/OEKOFLXML/compare/v0.2.0...HEAD
[0.2.0]: https://github.com/dietmarbetz/OEKOFLXML/compare/v0.1.0...v0.2.0
[0.1.0]: https://github.com/dietmarbetz/OEKOFLXML/releases/tag/v0.1.0
