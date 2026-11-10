# outbnd – Plan

Reiseplanung für Gruppen als Web-App: Link öffnen und los, keine Installation. Nachfolger des Malta-Planners, komplett getrennt von ihm.

## Grundsätze
- **Jede Reise ist eine eigene, abgeschlossene Welt:** eigene Mitreisende, Namen, Farben, Vorschläge, Kosten. Beitritt nur über den Einladungs-Link der Reise.
- **Jeder darf eine Reise anlegen** und ist dort Admin (kann weitere Admins bestimmen).
- Spontanes Planen bleibt der Kern: Vorschläge brauchen keinen Tag und keine Uhrzeit.
- Design nach `DESIGN.md`, Vorlage `vorlagen/glas-probe.html`.
- Datenbank von Anfang an abgesichert (nicht offen wie beim Malta-Planner).

## Was Lars einmalig einrichtet (Anleitung kommt Schritt für Schritt)
1. Neues Firebase-Projekt für outbnd (kostenlos) + Einstellungen für Anmeldung und Regeln
2. Google-Maps-Schlüssel für die outbnd-Adresse
3. GitHub Pages für das Repository einschalten (Adresse: `larsinger20.github.io/outbnd`)

## Etappen (jede für sich nutzbar, getestet, erst nach OK online)
**1. Grundgerüst** (gebaut und getestet 10.11.2026, wartet auf Abnahme)
- Seite im neuen Design: Reiter-Leiste unten, Kacheln, ⋯-Menü, hell/dunkel; Apple-Neutral, Stimmung je Reise (Verlauf + Akzentfarbe), Countdown im Kopf
- Startseite „Unsere Reisen“ (Reisen dieses Geräts), „+ Reise planen“
- Reise anlegen (erst nur Name + Zeitraum), Einladungs-Link, Beitritt mit Name + PIN
- Aktivitäten und Essen: vorschlagen, „Ich bin dabei“ / Daumen, Kommentare, live für alle

**2. Setup-Assistent**
- Eine Frage pro Bildschirm: Name, Zeitraum, Ziel/Stops, Anreise (Flug/Auto/Zug), Unterkunft, gewünschte Bereiche, Link teilen
- Reise-Info mit Countdown, Wetter, Unterkunft, Anreise (später alles änderbar)

**3. Alles Weitere vom Malta-Planner**
- Kosten teilen, Einkaufsliste, Mein Plan, Karte, „Neu“-Hinweise, Erledigt

**4. Mehrere Stops / Roadtrip**
- Stops mit Ort, Zeitraum, Unterkunft; Vorschläge optional einem Stop zuordnen
- Etappen auf der Karte, Wetter je Stop, „Heute: A → B“

**Später**
- Icon und Name auf dem Home-Bildschirm, Rückblick nach der Reise, Reise auf „Nur lesen“ stellen

## Offene Fragen
- Noch nicht in Etappe 1: PIN ändern, Reise umbenennen/Zeitraum ändern, Einladungs-Link erneuern, Mitreisende entfernen, Reise von der Startseite nehmen (Regeln erlauben es schon)
- Icon-Entwürfe (3–4 Varianten) für Lars zur Auswahl
