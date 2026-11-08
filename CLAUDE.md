# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Purpose

OEKOFLXML defines a standardized XML exchange format for agricultural field data (Feldstücke) of organic farms in the German InVeKo system (Integriertes Verwaltungs- und Kontrollsystem). It enables structured data exchange between farm management systems, authorities, and organic certification bodies (Öko-Kontrollstellen).

## XML Schema Architecture

The schema (`OEKOFLXML.xsd`) defines this hierarchy:

```
Betriebe                          # root: list of farm operations
└── Betrieb                       # one farm (name, address, INVEKOSNR)
    └── Feldstuecke               # collection of field parcels
        └── Feldstueck            # one parcel (FLIK, Flurstueckskennung, area, geometry)
            ├── Geometrie         # WKT MULTIPOLYGON with required srid attribute
            ├── Nutzungen         # land uses on this parcel (optional, repeatable)
            │   └── Nutzung       # one use: NutzungsCode (InVeKoS-NC), NutzungBezeichnung, Flaeche
            ├── AnzahlBaeume      # optional: number of fruit/orchard trees (Streuobst)
            └── Oekoflaeche       # optional boolean: parcel registered as eco-area in InVeKo
```

## Defined Simple Types

| Type | Used by | Constraint |
|---|---|---|
| `Flaechenwert` | `Gesamtflaeche`, `Flaeche` | `xs:decimal`, max. 4 Nachkommastellen, ≥ 0, Dezimaltrenner Punkt |
| `Flaecheneinheit` | `Gesamtflaeche/@Einheit`, `Flaeche/@Einheit` | Enum: `m2` oder `ha` |
| `INVEKOSNRType` | `INVEKOSNR` | Exakt 15 Ziffern (`[0-9]{15}`), beginnt für DE mit 276 |
| `FLIKType` | `FLIK` | `DE` + 2 Großbuchstaben Bundesland + 12 Ziffern, gesamt 16 Zeichen |
| `MultiPolygonWKT` | `Geometrie` | WKT MULTIPOLYGON, mehrere Ringe und Polygone, negative Koordinaten erlaubt |

## Key Schema Details

- `INVEKOSNR` und `FLIK` sind Pflichtfelder; Adressfelder (`Strasse`, `Hausnummer`, `Plz`, `Ort`) sind optional
- `Flurstueckskennung` ist in `Feldstueck` und `Nutzung` optional (`minOccurs="0"`) — bundeslandspezifisch
- `Bezeichnung` ist ein globales Element: in `Betrieb` Pflicht, in `Feldstueck` optional (`minOccurs="0"`)
- `Nummer` in `Feldstueck` ist optional (`minOccurs="0"`), da nicht in allen Agrarverwaltungssystemen vorhanden
- Konvention für fehlende optionale Werte: Element weglassen — kein leeres Element, kein `xsi:nil`
- `Geometrie/@srid` ist Pflichtattribut: `4326` (WGS84) oder `25832` (UTM Zone 32N / ETRS89, DE-Standard)
- `@Einheit` an `Gesamtflaeche` und `Flaeche` ist Pflicht, ohne Default-Wert
- `AnzahlBaeume` und `Oekoflaeche` sind bundeslandspezifisch optional
- Flächenwerte verwenden Punkt als Dezimaltrenner — Komma ist schema-invalid
- XML-Instanzdokumente müssen in UTF-8 kodiert sein
