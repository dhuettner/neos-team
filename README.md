# Waterproof.Team

Team- und Personenverwaltung für **Neos CMS 9**. Personen sind Dokument-Knoten und werden zentral gepflegt; Übersichten und Ansprechpartner-Blöcke greifen darauf zu, ohne dass Daten doppelt entstehen. Abteilungen laufen über [Sitegeist.Taxonomy](https://github.com/sitegeist/Sitegeist.Taxonomy), Listen über [Flowpack.Listable](https://github.com/Flowpack/Flowpack.Listable).

Das Paket liefert **Struktur, kein Design**: Markup mit Tailwind-Klassen, keine eigene CSS-Datei und kein eigener Build.

## Installation

```bash
composer require waterproof/neos-team
./flow neos.flow:package:rescan
./flow flow:cache:flush --force
./flow resource:publish
```

Die drei Befehle nach dem Require sind unter DDEV Pflicht: Das Composer-Plugin schreibt den Paket-Cache nach `Data/Temporary/`, Flow liest ihn aus `/tmp/Flow/`. Ohne `package:rescan` bleibt das Paket unsichtbar.

### Tailwind einrichten — sonst bleibt alles ungestylt

Die Fusion-Dateien liegen außerhalb deines Site-Packages, Tailwind findet Klassen aber nur in Dateien aus dem `content`-Glob. Ergänze in deiner `tailwind.config.js`:

```js
content: [
    './Resources/Private/**/*.{fusion,html,js}',
    './NodeTypes/**/*.{yaml,fusion}',
    '../../Packages/Application/Waterproof.Team/Resources/Private/**/*.fusion',
],
```

Ohne diesen Eintrag ist das Paket funktionsfähig, aber ohne Layout.

## Node-Typen

| Node-Typ | Backend-Label | Zweck |
|---|---|---|
| `Waterproof.Team:Document.Person` | Person | Einzelne Person mit Funktion, Porträt, Kontaktdaten, optionaler Detailseite |
| `Waterproof.Team:Document.TeamIndex` | Teamübersicht | Elternseite, listet die Personen darunter |
| `Waterproof.Team:Content.TeamGrid` | Team-Übersicht | Raster, optional auf Abteilungen eingegrenzt |
| `Waterproof.Team:Content.TeamHighlight` | Ansprechpartner | Einzelne Person auf beliebiger Seite |

## Detailseiten

Jede Person trägt die Eigenschaft `hasDetailPage`, standardmäßig aus:

- **aus:** Kacheln verlinken nicht, der direkte Aufruf antwortet im Frontend mit 404. Im Backend bleibt die Seite bearbeitbar, damit sie vorbereitet werden kann.
- **an:** Die Person bekommt eine reguläre Seite mit eigenem Inhaltsbereich.

## Spaltenwahl

`Document.TeamIndex` und `Content.TeamGrid` besitzen die Eigenschaft `columns` mit den Werten `cols1` bis `cols4` (Standard `cols3`). Die Klassen stehen in `Component/PersonCard.fusion` ausgeschrieben; zusammengesetzte Klassennamen wären für den Tailwind-Scanner unsichtbar.

## Eigenes Design einsetzen

| Prototyp | Zuständig für |
|---|---|
| `Waterproof.Team:Document.Person.Short` | Kartendarstellung in Listen |
| `Waterproof.Team:Content.PersonBody` | Detailseite |
| `Waterproof.Team:Content.PersonNotFound` | Ausgabe bei abgeschalteter Detailseite |
| `Waterproof.Team:Component.PersonCollection` | Raster um die Karten |
| `Waterproof.Team:Component.GridClass` | Zuordnung Spaltenwert zu Klassen |

## Lizenz

MIT.
