# outbnd

Gemeinsame Reiseplanung für Gruppen, Nachfolger des Malta-Planners (`larsinger20/malta-planner`, bleibt unverändert als Erinnerung). Komplett eigenständig: eigenes Repository, eigener Link, eigene Datenbank, nichts geteilt mit dem Malta-Planner. Malta kommt hier nicht vor.

**Gerade im Aufbau.** Die Seite ist noch nicht online.

## Design: IMMER zuerst `DESIGN.md` lesen
Grundlage für jedes Aussehen und jede Bewegung ist Apples aktuelles iOS-Design (iOS 27, Liquid Glass). Die Regeln stehen in `DESIGN.md` und gehen allem anderen vor. Bei neuen iOS-Versionen dort nachpflegen.
- `vorlagen/glas-probe.html`: abgenommene Vorlage für Reiter-Leiste (Pille/Lupe), ⋯-Menü und Lichtbrechung. Code von dort übernehmen statt neu erfinden. (Die Datei hat kein `<html>`-Gerüst, zum lokalen Ansehen eines davor setzen.)
- `PLAN.md`: Etappen und Grundsätze.
- Antworten an Lars **kompakt**: Ergebnis zuerst, nur was er wissen oder entscheiden muss.

## Über Lars
- Betreut das Projekt (GitHub `larsinger20`), **kein Programmierer**: auf Deutsch, einfach, Schritt für Schritt, ohne Fachjargon.
- Meist **iPhone (Safari, auch als Web-App vom Home-Bildschirm)**, teils Windows-PC. Mobile-first (375 px).
- Seitentexte Deutsch in der **Wir-Form**. Vorschlagskacheln nie „Karte“ nennen (Verwechslung mit der Landkarte).
- Entscheidet Design-Geschmack selbst; bei Unsicherheit kurz Varianten zeigen (z. B. als Probier-Seite fürs iPhone).

## Goldene Regeln
1. **Erst gründlich testen, dann zeigen, dann nach Lars' ausdrücklichem OK hochladen.** Nie ungefragt pushen.
2. **Nie Testdaten in die echte Datenbank.** Getestet wird nur in einer Testkopie mit Beispieldaten.
3. Vor jedem Push `git fetch` (Lars ändert manchmal direkt auf GitHub), fremde Änderungen erhalten.
4. Bestehende Daten nie brechen (alte Dokumente haben neue Felder evtl. nicht).
5. Änderungen klein halten. Commit-Nachrichten Deutsch, kurz.
6. Bei nennenswerten Änderungen diese Datei mitpflegen.

## Was entstehen soll (Stand der Planung)
- **Name „outbnd“**, kleingeschrieben. Farben wie beim Malta-Planner (Sand + Meerestürkis), neues Icon.
- **Mehrere Reisen, streng getrennt**: Jede Reise hat eigene Mitreisende (Beitritt per Einladungs-Link, Name + PIN pro Reise), eigene Namen/Farben, eigene Vorschläge, Kosten usw. Startseite „Unsere Reisen“ zeigt die Reisen, denen man auf diesem Gerät beigetreten ist.
- **Jeder darf eine Reise anlegen**, wird dort Admin und kann weitere Admins bestimmen.
- **Setup-Assistent** (eine Frage pro Bildschirm): Name, Zeitraum, Stops, Anreise (Flug/Auto/Zug), Unterkünfte (z. B. Airbnb), welche Bereiche (Essen, Einkauf, Kosten, Karte), am Ende Link zum Teilen.
- **Ein oder mehrere Stops** (Flug + Airbnb genauso wie Roadtrip).
- **Reiter-Leiste unten** im iOS-Stil (siehe `DESIGN.md`).
- **Kein Wischen, kein Halte-Menü**, keine echte App (bleibt Web-App).
- Funktionen und bewährte Lösungen des Malta-Planners als Vorlage nutzen (dessen `CLAUDE.md` beschreibt sie ausführlich), aber unabhängig weiterentwickeln.
- Datenbank von Anfang an abgesichert (nicht offen wie beim Malta-Planner).
