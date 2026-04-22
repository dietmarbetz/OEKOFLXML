# Changelog

Alle wesentlichen Änderungen an diesem Projekt werden in dieser Datei dokumentiert.
Format basiert auf [Keep a Changelog](https://keepachangelog.com/de/1.0.0/).

---

## [0.1.0] — 2026-04-22

### Hinzugefügt
- `OEKOFLXML.xsd` — initiales XML-Schema für Feldstücksdaten im InVeKo-System
- `OEKOFLXML.xml` — Beispiel-/Template-Dokument
- `README.md` — Projektbeschreibung mit Datenmodell und Feldbeschreibungen
- `CHANGELOG.md` — diese Datei

### Schema-Typen
- `Flaecheneinheit` — Enum für Flächeneinheiten (`m2`, `ha`)
- `Flaechenwert` — Dezimaltyp für Flächenwerte, max. 4 Nachkommastellen, ≥ 0
- `INVEKOSNRType` — InVeKo-Betriebsnummer, exakt 15 Ziffern
- `FLIKType` — FLIK-Kennung, Format `DE[A-Z]{2}[0-9]{12}`
- `MultiPolygonWKT` — WKT MULTIPOLYGON mit Unterstützung für Löcher und negative Koordinaten

### Schema-Korrekturen (gegenüber erstem Entwurf)
- `Gesamtflaeche`, `Flaeche`, `Geometrie`: ungültige Kombination `type`-Attribut + `complexType` behoben (`simpleContent/extension`)
- `Gesamtflaeche`, `Flaeche`: `xs:float` durch `xs:decimal` (via `Flaechenwert`) ersetzt
- `MultiPolygonWKT`-Pattern: negative Koordinaten, mehrere Polygone und Löcher ergänzt
- `Feldstuecke` in `Betrieb`: `maxOccurs="unbounded"` → `maxOccurs="1"`
- `Feldstuecke/@Anzahl`: Typ auf `xs:integer` eingeschränkt
- `Nutzung`: `minOccurs`/`maxOccurs` für alle Felder explizit gesetzt; `Flurstueckskennung` optional (`minOccurs="0"`)
- Tippfehler `AnzahlBaume` → `AnzahlBaeume` in `OEKOFLXML.xml` korrigiert
- `Geometrie/@srid` und WKT-Textinhalt in `OEKOFLXML.xml` ergänzt
- `NutzungsCode`, `NutzungBezeichnung` in `OEKOFLXML.xml` korrigiert (waren `Code`, `Bezeichnung`)

### Dokumentation
- Schema-weite `xs:annotation` mit fachlichem Zweck, Geltungsbereich, Version, Autor, Kodierungshinweis
- `xs:appinfo` mit maschinenlesbaren Metadaten (Version, Datum, Kodierung)
- `xml:lang="de"` auf Schema-Annotation gesetzt
- Elementdokumentationen für alle relevanten Felder ergänzt

---

## [Unreleased]

### Geändert
- `Flaechenwert`: Dokumentation präzisiert — Bezeichnung als Größe einer Fläche klargestellt
- `Bezeichnung`: Dokumentation ergänzt — Unternehmensbezeichnung des Betriebs; Annotation korrekt innerhalb der Elementdeklaration platziert
- `Nummer` in `Feldstueck`: Dokumentation ergänzt — laufende Nummer im Flächenverzeichnis
