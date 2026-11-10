# outbnd

Gemeinsame Reiseplanung für Gruppen, Nachfolger des Malta-Planners (`larsinger20/malta-planner`, bleibt unverändert als Erinnerung). Komplett eigenständig: eigenes Repository, eigener Link, eigene Datenbank, nichts geteilt mit dem Malta-Planner. Malta kommt hier nicht vor.

**Gerade im Aufbau.** Die Seite ist noch nicht online. Etappe 1 (siehe `PLAN.md`) ist gebaut und getestet, wartet auf Lars' Abnahme und auf die Zugangsdaten des Firebase-Projekts (`const CFG` in `index.html`, solange `null` zeigt die Startseite „Noch nicht eingerichtet“).

## Aufbau (Etappe 1)
- **Eine Datei `index.html`** (HTML + CSS + ein JS-Modul), kein Framework/Build. Firebase per CDN (`firebasejs/10.12.2`): Firestore + anonyme Anmeldung. `firestore.rules` = die Regeln, die Lars in der Firebase-Konsole einträgt (bei Änderungen dort neu einfügen lassen).
- **Absicherung**: Jedes Gerät meldet sich still anonym an. Zugriff auf eine Reise hat nur ein Gerät mit `members/{geräte-id}`; das entsteht nur mit gültigem Einladungs-Schlüssel (`invites/{schlüssel}`, steht im Link `#/e/{reise}/{schlüssel}`). Inhalte liest/schreibt nur, wer zusätzlich per Name + PIN an einer Person hängt (`members.pid`). PIN-Prüfwert liegt unlesbar in `people/{pid}/priv/pin`; die Regeln vergleichen ihn (kein Server nötig, kostenloser Tarif reicht). Gleicher Name + gleiche PIN holt das Profil auf ein neues Gerät. Die PIN schützt vor Verwechslung in der Gruppe, die eigentliche Grenze ist der Einladungs-Link.
- **Daten** unter `trips/{reise}`: `{name, from, to, mood, cby, ts}` (`mood` = Schlüssel aus `MOODS`, fehlt/unbekannt = Meer, `moodOf`); `people/{pid}` `{name, col, admin, ts}`; `acts/{id}` `{title, desc, cat:'act'|'food', kind:'cook'|'rest'|'', day, time, by, ts, cm{id:{by,t,ts}}}`; `signups/{act}_{pid}` `{act, pid}`. Map `cm`: Einträge einzeln per `setDoc(..., {merge:true})`, löschen per `updateDoc` + `deleteField()`. Neue Felder/Sammlungen brauchen passende Regeln.
- **Dieses Gerät** merkt sich in `localStorage` (`outbnd.trips`) nur Reise-ID, Name, Zeitraum, Schlüssel für „Unsere Reisen“.
- **Adressen**: `#/` Start · `#/neu` Assistent · `#/e/{reise}/{schlüssel}` Beitritt · `#/r/{reise}` · `…/leute` · `…/profil` · `…/stimmung`.
- **Stimmung**: `draw()` setzt `data-m` am `<html>`; daran hängen Verlauf (`--g`) und Akzent (`--ac`/`--acb`). `main.wash` zeichnet den Verlauf im Kopf der Reise.
- Reiter-Leiste, ⋯-Menü (`menuFor` liefert die Einträge je Knopf), Lichtbrechung: Code aus der Vorlage, Leiste passt ihre Breite an die Zahl der Reiter an (`TABS`).
- `draw()` baut die Ansicht neu; wartet, solange ein Feld Fokus hat (`pending`), eigene Aktionen rufen `draw(true)`. Eingaben stehen im Zustand (`Wz`, `J`, `F`, `Pf`, `cmDraft`) und überleben Neuaufbauten.
- **Fallstricke**: CSS-Klassennamen vor dem Anlegen suchen (`.in` = „dabei“-Knopf gefüllt, Eingabefelder heißen `.fi`). Die Leiste enthält Kopien des Inhalts (Lichtbrechung): in Tests immer unter `#app` suchen.

## Testen
- Node, Java und Playwright sind in Cloud-Sitzungen da. **Regeln und Abläufe gegen den Firebase-Emulator testen** (`npm i firebase-tools firebase@10.12.2` im Scratchpad, `firebase emulators:start --only firestore,auth --project demo-outbnd`): Testkopie ersetzt den Block `FIREBASE-ANFANG … FIREBASE-ENDE` durch Emulator-Anbindung. Mehrere Geräte = mehrere Browser-Kontexte. Regeln zusätzlich direkt angreifen (Fremder, Nicht-Admin, falsche PIN).
- Zum Zeigen: Probier-Seite als Artifact, derselbe Block durch Beispieldaten im Speicher ersetzt (dort `location.hash`/`localStorage`/Teilen ersetzen, im Artifact gesperrt).
- 375 und 320 px, hell und dunkel, ohne seitliches Scrollen, ohne Konsolenfehler.

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
- **Name „outbnd“**, kleingeschrieben. Aussehen: Apple-Neutral als Grund, jede Reise mit eigener Stimmung (Verlauf im Kopf + Akzentfarbe), siehe `DESIGN.md` Abschnitt 7. Neues Icon.
- **Mehrere Reisen, streng getrennt**: Jede Reise hat eigene Mitreisende (Beitritt per Einladungs-Link, Name + PIN pro Reise), eigene Namen/Farben, eigene Vorschläge, Kosten usw. Startseite „Unsere Reisen“ zeigt die Reisen, denen man auf diesem Gerät beigetreten ist.
- **Jeder darf eine Reise anlegen**, wird dort Admin und kann weitere Admins bestimmen.
- **Setup-Assistent** (eine Frage pro Bildschirm): Name, Zeitraum, Stops, Anreise (Flug/Auto/Zug), Unterkünfte (z. B. Airbnb), welche Bereiche (Essen, Einkauf, Kosten, Karte), am Ende Link zum Teilen.
- **Ein oder mehrere Stops** (Flug + Airbnb genauso wie Roadtrip).
- **Reiter-Leiste unten** im iOS-Stil (siehe `DESIGN.md`).
- **Kein Wischen, kein Halte-Menü**, keine echte App (bleibt Web-App).
- Funktionen und bewährte Lösungen des Malta-Planners als Vorlage nutzen (dessen `CLAUDE.md` beschreibt sie ausführlich), aber unabhängig weiterentwickeln.
- Datenbank von Anfang an abgesichert (nicht offen wie beim Malta-Planner).
