# MobiRoute Dresden – Datenpakete

Dieses Repository enthält nur **Daten**: die nächtlich gebauten Fahrplanpakete der Android-App
MobiRoute Dresden (ÖPNV + MOBIbike + zu Fuß, Routenplanung direkt auf dem Handy). Kein Code, keine
Schlüssel.

Jede Nacht entsteht ein Release `data-<datum>-<uhrzeit>` mit

| Datei | Inhalt |
|---|---|
| `manifest.json` | Version, Gültigkeit (14 Tage), SHA-256 und Größe der Dateien, Prüfergebnisse, Linien „unter Vorbehalt“ |
| `manifest.json.sig` | Ed25519-Signatur des Manifests (Base64) |
| `timetable.bin.gz` | Fahrplan Großraum Dresden für 14 Tage (Halt-Folgen, Zeitprofile, Fahrten, Betriebskalender) |
| `static.bin.gz` | Halte, Fußwege zwischen Halten, Rückgabepunkte, Rad-Matrix der MOBIbike-Stationen, Höhenraster, Elbufer, Brücken, Rückgabezonen |

Die App lädt `https://github.com/SirTine/dvb-bike-planer-data/releases/latest/download/manifest.json`,
prüft Signatur und Prüfsummen und lädt nur geänderte Dateien (im WLAN). Ein Paket wird nur
veröffentlicht, wenn alle Prüfungen bestanden sind: Plausibilität gegen die Vorversion, Gegenprobe der
Soll-Zeiten mit den VVO-Abfahrtsmonitoren und ein Abgleich des Planers mit einem unabhängigen
Connection-Scan auf 100 Zufallsanfragen.

Format: „MRP1“ – Abschnittstabelle mit ausgerichteten Little-Endian-Arrays, direkt memory-mapped lesbar.

## Quellen und Lizenzen

- **Fahrplandaten: [gtfs.de](https://gtfs.de) (DELFI e.V.), [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/deed.de)** –
  Feeds `nv_free` und `rv_free`, zugeschnitten auf den Großraum Dresden (lat 50,93–51,17, lng 13,55–14,00),
  14 Tage ab Bauzeit; Fernverkehr und Anruf-Linien-Taxis entfernt, gleiche Fahrten zusammengeführt,
  Fußwege und Rad-Matrix vorberechnet.
- MOBIbike-Stationen und Rückgabezonen: nextbike GmbH (öffentliche Live-Daten), Stand der Bauzeit.
- Elbe und Brücken: © [OpenStreetMap](https://www.openstreetmap.org/copyright)-Mitwirkende, ODbL.
- Höhen: Copernicus DEM GLO-90 über die Open-Meteo Elevation API.

Die Daten sind ohne Gewähr. Fahrplandaten können vom tatsächlichen Angebot abweichen (z. B. Ferien- und
Baufahrpläne); die App prüft deshalb Abfahrten vor der Anzeige bei der VVO-Auskunft.
