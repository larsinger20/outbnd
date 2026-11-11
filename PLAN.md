# outbndr. – Plan

Reiseplanung für Gruppen als Web-App: Link öffnen und los, keine Installation. Nachfolger des Malta-Planners, komplett getrennt von ihm.

## Grundsätze
- **Jede Reise ist eine eigene, abgeschlossene Welt:** eigene Mitreisende, Namen, Farben, Vorschläge, Kosten. Beitritt nur über den Einladungs-Link der Reise.
- **Jeder darf eine Reise anlegen** und ist dort Admin (kann weitere Admins bestimmen).
- Spontanes Planen bleibt der Kern: Vorschläge brauchen keinen Tag und keine Uhrzeit.
- Design nach `DESIGN.md`, Vorlage `vorlagen/glas-probe.html`.
- Datenbank von Anfang an abgesichert (nicht offen wie beim Malta-Planner).

## Was Lars einmalig einrichtet (Anleitung kommt Schritt für Schritt)
1. Neues Firebase-Projekt für outbndr (kostenlos) + Einstellungen für Anmeldung und Regeln
2. ~~Google-Maps-Schlüssel~~ nicht mehr nötig (Karte und Ortssuche über OpenStreetMap)
3. GitHub Pages für das Repository einschalten (erledigt; Adresse seit der Umbenennung: `larsinger20.github.io/outbndr./`)

## Etappen (jede für sich nutzbar, getestet, erst nach OK online)
**1. Grundgerüst** (fertig 10.11.2026)
- Seite im neuen Design: Reiter-Leiste unten, Kacheln, ⋯-Menü, hell/dunkel; Apple-Neutral, Stimmung je Reise (Verlauf + Akzentfarbe), Countdown im Kopf
- Startseite „Unsere Reisen“ (Reisen dieses Geräts), „+ Reise planen“
- Reise anlegen (erst nur Name + Zeitraum), Einladungs-Link, Beitritt mit Name + PIN
- Aktivitäten und Essen: vorschlagen, „Ich bin dabei“ / Daumen, Kommentare, live für alle

**2. Setup-Assistent** (fertig 11.11.2026)
- Eine Frage pro Bildschirm: Name, Zeitraum, Ziel/Stops, Anreise (Flug/Auto/Zug), Unterkunft, gewünschte Bereiche, Link teilen
- Reise-Info mit Countdown, Wetter, Unterkunft, Anreise (später alles änderbar)

**3. Alles Weitere vom Malta-Planner** (fertig 11.11.2026; Karte mit OpenStreetMap statt Google, kein Schlüssel nötig)
- Kosten teilen, Einkaufsliste, Mein Plan, Karte, „Neu“-Hinweise, Erledigt

**4. Mehrere Stops / Roadtrip** (fertig 11.11.2026)
- Stops mit Ort, Zeitraum, Unterkunft; Vorschläge optional einem Stop zuordnen
- Etappen auf der Karte, Wetter je Stop, „Heute: A → B“

**Später**
- Icon und Name auf dem Home-Bildschirm, Rückblick nach der Reise, Reise auf „Nur lesen“ stellen

## Offene Fragen
- Icon-Entwürfe (3–4 Varianten) für Lars zur Auswahl
