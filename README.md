# OEKOFLXML

**Austauschformat für landwirtschaftliche Flächendaten von Öko-Betrieben aus einem 
InVeKo-System**

Version 0.1.0 — D. Betz, dietmar.betz@abcert.de

---

## Fachlicher Zweck

OEKOFLXML definiert ein standardisiertes XML-Austauschformat für Feldstücksdaten im Rahmen des
**Integrierten Verwaltungs- und Kontrollsystems (InVeKo)** der deutschen Agrarverwaltung.

Es ermöglicht den strukturierten Austausch von Feldstücken (Flurstücken), Flächennutzungen und
Geometrien zwischen Agrarverwaltungssystemen, Landwirtschaftsbetrieben und Öko-Kontrollstellen.

Der Austausch von Flächendaten erfolgt als Export aus dem Agrarverwaltungssystem im 
OEKOFLXML Format und anschließender Übertragung an den Empfänger 
Landwirtschaftzlicher Betrieb und/oder Öko-Kontrollstelle.

Das Ziel, das mit dem standardisierten Austauschformat verfolgt wird ist die Nutzung 
der Flächendaten aus dem Agrarverwaltungssystem durch die Öko-Kontrollstellen für 
die Zertifizierung nach der EU-Öko-Verordnung.

## Geltungsbereich

- Landwirtschaftliche Betriebe, die dem Öko-Kontrollverfahren unterstehen
- Betriebe mit InVeKos-Betriebsnummer (INVEKOSNR)
- Feldstücke mit FLIK-Kennung, Gesamtfläche und Geometrie (WKT MULTIPOLYGON)
- Flächennutzungen gemäß FNN-Codeliste (Nutzungscode, Bezeichnung, Fläche)
- Bundeslandspezifische Angaben: Flurstueckskennung, Streuobstbäume, Ökoflächen-Kennzeichnung

---

## Dateistruktur

| Datei | Beschreibung |
|---|---|
| `OEKOFLXML.xsd` | XML Schema Definition |
| `OEKOFLXML.xml` | Beispiel-/Template-Dokument |

---

## Datenmodell

```
Betriebe                        (1..n Betriebe)
└── Betrieb                     Name, Adresse, INVEKOSNR
    └── Feldstuecke             Liste der bewirtschafteten Feldstücke
        └── Feldstueck          FLIK, Bezeichnung, Nummer, Fläche, Geometrie
            ├── Geometrie       MULTIPOLYGON in WKT mit Koordinatensystem (srid)
            ├── Nutzungen       Flächennutzungen (optional, wiederholbar)
            │   └── Nutzung     NutzungsCode (FNN), Bezeichnung, Fläche
            ├── AnzahlBaeume    Streuobstbäume (bundeslandspezifisch, optional)
            └── Oekoflaeche     Ökoflächen-Marker im InVeKo (optional)
```

---

## Feldbeschreibung

### Betrieb

| Feld | Pflicht | Beschreibung |
|---|---|---|
| `Bezeichnung` | Ja | Name des Betriebs |
| `Strasse` | Nein | Straße |
| `Hausnummer` | Nein | Hausnummer |
| `Plz` | Nein | Postleitzahl |
| `Ort` | Nein | Ort |
| `INVEKOSNR` | Ja | InVeKo-Betriebsnummer, exakt 15 Ziffern, beginnt für DE mit 276 |

### Feldstueck

| Feld | Pflicht | Beschreibung                                                                              |
|---|---|-------------------------------------------------------------------------------------------|
| `FLIK` | Ja | Feldstück-Land-Identifikations-Kennung: `DE` + 2-Buchstaben-Bundeslandkürzel + 12 Ziffern |
| `Flurstueckskennung` | Ja | Kurzkennung des Feldstücks, i.d.R. Teil der FLIK (bundeslandspezifisch)                   |
| `Bezeichnung` | Ja | Bezeichnung des Feldstücks                                                                |
| `Nummer` | Ja | Feldstücksnummer                                                                          |
| `Gesamtflaeche` | Ja | Gesamtfläche in m² oder ha, max. 4 Nachkommastellen                                       |
| `Geometrie` | Ja | Umriss als WKT MULTIPOLYGON mit Pflichtattribut `srid`                                    |
| `Nutzungen` | Nein | Liste der Flächennutzungen (Teilstücke)                                                   |
| `AnzahlBaeume` | Nein | Anzahl Streuobstbäume (nur in einigen Bundesländern)                                      |
| `Oekoflaeche` | Nein | `true`/`false` — Ökoflächen-Kennzeichnung im InVeKoS                                      |

### Nutzung

| Feld | Pflicht | Beschreibung |
|---|---|---|
| `NutzungsCode` | Ja | FNN-Code der Nutzung |
| `NutzungBezeichnung` | Ja | Bezeichnung gemäß FNN-Codeliste |
| `Flaeche` | Ja | Teilfläche in m² oder ha, max. 4 Nachkommastellen |
| `Flurstueckskennung` | Nein | Teilflächenkennung (bundeslandspezifisch) |

---

## Geometrie

Die Geometrie wird als **WKT MULTIPOLYGON** angegeben. Das Koordinatensystem wird über das
Pflichtattribut `srid` bestimmt:

| SRID | Koordinatensystem | Verwendung |
|---|---|---|
| `25832` | UTM Zone 32N / ETRS89 | Standard in Deutschland |
| `4326` | WGS84 (Länge/Breite) | Internationaler Standard |

Beispiel:
```xml
<Geometrie srid="25832">MULTIPOLYGON(((399000.0 5700000.0, 399100.0 5700000.0, 399100.0 5700100.0, 399000.0 5700100.0, 399000.0 5700000.0)))</Geometrie>
```

---

## Technische Hinweise

- **Kodierung:** XML-Instanzdokumente müssen in **UTF-8** kodiert sein
  ```xml
  <?xml version="1.0" encoding="UTF-8"?>
  ```
- **Dezimaltrenner:** Flächenwerte verwenden den **Punkt** (`.`) als Dezimaltrenner — Komma ist nicht erlaubt
- **FLIK-Format:** `DE[A-Z]{2}[0-9]{12}` — z.B. `DEBY012345678901`
- **INVEKOSNR-Format:** 15 Ziffern — z.B. `276123456789012`

---

## Bundeslandkürzel (FLIK)

`BB` Brandenburg · `BE` Berlin · `BW` Baden-Württemberg · `BY` Bayern · `HB` Bremen ·
`HE` Hessen · `HH` Hamburg · `MV` Mecklenburg-Vorpommern · `NI` Niedersachsen ·
`NW` Nordrhein-Westfalen · `RP` Rheinland-Pfalz · `SH` Schleswig-Holstein ·
`SL` Saarland · `SN` Sachsen · `ST` Sachsen-Anhalt · `TH` Thüringen
