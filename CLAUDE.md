# outbndr.

Gemeinsame Reiseplanung für Gruppen (Repository `larsinger20/outbndr.`, früher `outbnd`; lokaler Ordner und Sitzungs-Zugriff können noch `outbnd` heißen, GitHub leitet um), Nachfolger des Malta-Planners (`larsinger20/malta-planner`, bleibt unverändert als Erinnerung). Komplett eigenständig: eigenes Repository, eigener Link, eigene Datenbank, nichts geteilt mit dem Malta-Planner. Malta kommt hier nicht vor.

**Stand 11.11.2026:** Etappen 1–4 aus `PLAN.md` sind gebaut, getestet und online unter `https://larsinger20.github.io/outbndr./` (Repository `larsinger20/outbndr.` mit Punkt, von Lars am 11.11.2026 umbenannt; ohne Punkt gibt es „nicht gefunden“; Firebase-Projekt `outbnd-947bc`, `const CFG` in `index.html`). Neue Regeln aus `firestore.rules` muss Lars nach jeder Regeländerung in der Firebase-Konsole einfügen (bis dahin: Beitritt klappt trotzdem, Reise anlegen zeigt einen Hinweis).

## Aufbau
- **Eine Datei `index.html`** (HTML + CSS + ein JS-Modul), kein Framework/Build. Firebase per CDN (`firebasejs/10.12.2`): Firestore + anonyme Anmeldung. Ohne Schlüssel: Ortssuche Photon (`photon.komoot.io`, OpenStreetMap), Wetter Open-Meteo, Karte Leaflet 1.9.4 (cdnjs, erst beim Öffnen geladen) mit CARTO-Kacheln (hell `voyager`, dunkel `dark_all`). Kein Google-Schlüssel nötig.
- **Absicherung** (`firestore.rules`): Jedes Gerät meldet sich still anonym an. Zugriff auf eine Reise hat nur ein Gerät mit `members/{geräte-id}`; das entsteht nur mit gültigem Einladungs-Schlüssel (`invites/{schlüssel}`, im Link `#/e/{reise}/{schlüssel}`). Inhalte liest/schreibt nur, wer zusätzlich per Name + PIN an einer Person hängt (`members.pid`). PIN-Prüfwert unlesbar in `people/{pid}/priv/pin`; die Regeln vergleichen ihn. Gleicher Name + gleiche PIN holt das Profil auf ein neues Gerät; Profil übernehmen nur, solange der Link des Geräts gilt (entfernte Personen kommen nach „Neuer Link“ nicht zurück). Alles hängt daran, dass es die Reise gibt (`isDevice` prüft `exists(trip)`): Löschen der Reise sperrt sofort alle aus.
- **Daten** unter `trips/{reise}`: `{name, from, to, mood, cby, ts, areas{food,cost,plan,map}, stops[{id,name,lat,lng,from,to,stay{name,link,addr,lat,lng,note}}], travel[{id,kind:'flug'|'auto'|'zug'|'bus'|'faehre',title,from,to,date,time,note,who[]}], del?}` (fehlende Felder = Altbestand: `moodOf`, `areas()` mit allem an, `stops()`); `people/{pid}` `{name, col, admin, ts, nbase, sn{id:true}, cmr{actId:ts}}`; `acts/{id}` `{title, desc, cat:'act'|'food', kind:'cook'|'rest'|'', day, time, stop, place{name,lat,lng,addr}, res{t,p}, done, dby, shop{id:{t,by,ts,ok,arch}}, noshop, by, ts, cm{id:{by,t,ts}}}`; `signups/{act}_{pid}` `{act, pid}`; `costs/{id}` `{title, amt (Cent, ganze Zahl), by, for[], ts, cby, dt?, kind?:'pay', ets?, eby?}`; `lists/gen` `{shop{}}` (allgemeine Einkaufsliste). Maps (`cm`, `shop`, `sn`, `cmr`) nur einzeln per Feldpfad schreiben/löschen (`deleteField()`), nie ganz überschreiben. Neue Felder/Sammlungen brauchen passende Regeln (`tripOk`, `personOk` mit `hasOnly`!). Andere als Ersteller/Admin dürfen an Vorschlägen nur `cm`, `shop`, `noshop`, `done`, `dby` ändern.
- **Dieses Gerät** merkt sich in `localStorage` `outbnd.trips` (alter Name bleibt, sonst verlieren Geräte ihre Liste) nur Reise-ID, Name, Zeitraum, Stimmung, Schlüssel; `outbndr.wx.*` Wetter (24 h, nach 30 Min. auffrischen), `outbndr.tip1` Tipp gesehen.
- **Adressen**: `#/` Start · `#/neu` Assistent (`WSTEPS`: Name, Zeitraum, Ziel/Stopps, Unterkunft, Anreise, Bereiche, Stimmung, Wer bist du, Link) · `#/e/{reise}/{schlüssel}` Beitritt · `#/r/{reise}` Reiter · `…/leute` · `…/profil` (Name, Farbe, PIN ändern) · `…/stimmung` · `…/reise` (Admin: bearbeiten, Link erneuern, löschen; alle: auf diesem Gerät entfernen).
- **Reiter** je nach Bereichen (`setTabs`/`buildBar`): Aktivitäten immer, Essen (Umschalter Vorschläge | Einkaufsliste), Kosten, Mein Plan. Karte-Knopf neben „vorschlagen“ (Vollbild `#mapv`, wächst aus dem Knopf, Zurück-Geste schließt, Filter Alle/Aktivitäten/Essen/Meine, Pins Unterkunft/Ziel + Orte, Mini-Kachel mit „Zum Vorschlag“ und Route).
- **Funktionen wie im Malta-Planner** (dort ausführlich beschrieben, hier nachgebaut): Reise-Info als Aufklapp-Zeile mit Glas-Umschalter Wetter · Unterkunft · Anreise (`infoHTML`); Countdown im Kopf, unterwegs „Tag 3 von 8 · Split“ bzw. „Heute: Split → Hvar“; Vorschläge mit Ort (Ortssuche, `placeHTML`/`pickPlace`, Auswahl schon bei `pointerdown`), Stopp (Filterzeile bei mehreren Stopps), Reservierung, Kommentare („N neu“), Erledigt (alle dürfen, Aufklapp-Zeile „Erledigt · N“), Favorit beim Essen nur bei alleiniger Führung; „Neu“ (≥ halb sichtbar 0,8 s → `people.sn`, Pille bis Reiterwechsel, Punkt am Reiter); Einkaufsliste je Kochvorschlag + allgemein, Rezept-Vorschläge (`RECIPES`, Malta-Zeilen entfernt), Archiv, Eintragen auch beim Verlassen des Feldes; Kosten mit Stand, wenigen Überweisungen in fester Reihenfolge (`balances`/`settle`), Abhaken per Kreis, „Wer steht wo?“, Bearbeiten (Ersteller, Zahler, Admin), „Geändert“; Mein Plan nach Tagen, Tipp springt zur Kachel (`goToCard`); Update-Hinweis (ETag, alle 15 Min.).
- **Icon**: `outbndr-180.png` (Home-Bildschirm), `outbndr-32.png` (Tab), `outbndr-512.png` + `manifest.webmanifest` (Name „outbndr.“). Entwurf A „o. auf Meer“ (vorläufig, Lars wählt noch zwischen A/B/C). Neue Icons = neue Dateinamen (iPhone hält alte fest).
- **Rückblick** nach dem Ende der Reise (`recapHTML`): Aktivitäten, Essen, gemeinsam bezahlt, Mitreisende; Kopf „Schön war es“.
- **Admin**: Reise bearbeiten (Name, Zeitraum, Ziele mit Zeitraum/Unterkunft/Adresse, Fahrten mit „Wer fährt mit?“, Bereiche), Stimmung, Admins bestimmen, Mitreisende entfernen (bietet danach neuen Link an), Link erneuern, Reise für alle löschen (zuerst `del` setzen → alle sehen sofort „gibt es nicht mehr“).
- **Stimmung**: `draw()` setzt `data-m` am `<html>`; daran hängen Verlauf (`--g`) und Akzent (`--ac`/`--acb`). `main.wash` zeichnet den Verlauf im Kopf der Reise; Knöpfe auf dem Verlauf haben Kachel-Grund (`main.wash .add/.mapb/.btn.g`).
- Reiter-Leiste, ⋯-Menü (`menuFor` liefert die Einträge je Knopf), Lichtbrechung: Code aus der Vorlage.
- `draw()` baut die Ansicht neu; wartet, solange ein Feld Fokus hat (`pending`), eigene Aktionen rufen `draw(true)`. Eingaben stehen im Zustand (`Wz`, `J`, `F`, `Pf`, `Cf`, `Ed`, `Rv`, `cmDraft`, `shopDraft`; Felder mit `data-o` + `data-f`, `objFor`) und überleben Neuaufbauten.
- **Leiste oben (Uhr/Akku) in Safari**: Safari färbt sie und die Leiste unten nach dem Grund von `html`/`body` (ab iOS 26 nicht mehr nach `theme-color`) und verändert den Ton dabei selbst. Deshalb bleibt der Grund immer neutral, und der Verlauf blendet oben weich ein (`main.wash::before`, Maske mit sanftem Anfang und Ende). Färben des Balkens wurde probiert und verworfen; Vergleich in `vorlagen/kopf-probe.html`, Lars wählte A.
- `.lq` setzt `position:relative`: fest stehende Elemente mit `.lq` brauchen eine eigene Regel danach (`.nb.lq{position:fixed}`).
- **Fallstricke**: CSS-Klassennamen vor dem Anlegen suchen (`.in` = „dabei“-Knopf gefüllt, Eingabefelder heißen `.fi`, `.chv` Pfeil). Die Leiste enthält Kopien des Inhalts (Lichtbrechung): in Tests immer unter `#app` suchen. Löschen in Stapeln: jedes Dokument braucht eine Lösch-Regel, sonst scheitert der ganze Stapel.

## Testen
- Großer Ablauf-Test (`e2e2.js`, 61 Prüfungen, 3–4 Geräte) und Regel-Tests (`rules.test.mjs`, `rules2.test.mjs`) liegen nur im Scratchpad; bei neuer Sitzung nachbauen. Ortssuche, Wetter und Karten-Kacheln im Test per `context.route` mit Beispielantworten.
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
- **Name „outbndr.“** (seit 10.11.2026, vorher „outbnd“, den Namen gab es schon): Kunstname nach englisch „outbound“ (out = hinaus, bound = unterwegs nach, -er = jemand, der das tut): jemand, der rausgeht, unterwegs ist und Neues entdeckt. Kleingeschrieben, der Punkt gehört zum Namen (im Logo in Akzentfarbe). In Fließtexten „outbndr“ ohne Punkt, damit kein doppelter Satzpunkt entsteht. Technische Namen bleiben alt: Firebase-Projekt `outbnd-947bc` (unveränderbar), Speicherschlüssel `outbnd.trips` (sonst verlieren Geräte ihre Reiseliste). Aussehen: Apple-Neutral als Grund, jede Reise mit eigener Stimmung (Verlauf im Kopf + Akzentfarbe), siehe `DESIGN.md` Abschnitt 7. Neues Icon.
- **Mehrere Reisen, streng getrennt**: Jede Reise hat eigene Mitreisende (Beitritt per Einladungs-Link, Name + PIN pro Reise), eigene Namen/Farben, eigene Vorschläge, Kosten usw. Startseite „Unsere Reisen“ zeigt die Reisen, denen man auf diesem Gerät beigetreten ist.
- **Jeder darf eine Reise anlegen**, wird dort Admin und kann weitere Admins bestimmen.
- **Setup-Assistent** (eine Frage pro Bildschirm): Name, Zeitraum, Stops, Anreise (Flug/Auto/Zug), Unterkünfte (z. B. Airbnb), welche Bereiche (Essen, Einkauf, Kosten, Karte), am Ende Link zum Teilen.
- **Ein oder mehrere Stops** (Flug + Airbnb genauso wie Roadtrip).
- **Reiter-Leiste unten** im iOS-Stil (siehe `DESIGN.md`).
- **Kein Wischen, kein Halte-Menü**, keine echte App (bleibt Web-App).
- Funktionen und bewährte Lösungen des Malta-Planners als Vorlage nutzen (dessen `CLAUDE.md` beschreibt sie ausführlich), aber unabhängig weiterentwickeln.
- Datenbank von Anfang an abgesichert (nicht offen wie beim Malta-Planner).
