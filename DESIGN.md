# outbndr. – Design-Grundlage (gilt IMMER)

Wunsch von Lars: Aussehen und Bewegung so nah wie möglich an Apples aktuellem iOS-Design (iOS 27, „Liquid Glass“). Hoher Anspruch: nichts ungeprüft übernehmen, alles hinterfragen.
Stand: 10.11.2026. **Vorlage mit allen Mustern zum Abschauen: `vorlagen/glas-probe.html`** (Probier-Seite, von Lars auf dem iPhone abgenommen; online: https://claude.ai/artifact/WYuRH8EcEk9p1NLY51npaB).

## 0. Apple-Check (vor JEDER Änderung)
1. **Gibt es das so in einer Apple-App?** Welche App, welcher Bereich? Nur Gleiches vergleichen: Ein ⋯ in Safaris Bedienleiste verhält sich anders als ein ⋯ in einem Eintrag (Musik-App). Falsches Vorbild = lange falsche Richtung (ist passiert).
2. **Messen statt schätzen:** Lars um eine Bildschirmaufnahme oder ein Foto der Apple-App bitten und Bild für Bild ausmessen (Maße, Farben, Zeiten). Aufnahmen haben 60 oder 120 Bilder/s. Standfotos zeigen Details, die im Video verwischen (z. B. Aufbau der Lupe).
3. **Auf dem iPhone abnehmen lassen:** Das Testprogramm ist kein Safari. Erst Lars' Aufnahme vom eigenen iPhone zeigt, wie es wirklich aussieht und ob es flüssig läuft.
4. Schrift nur in Apple-Größen (Abschnitt 6), Inhalt ohne Glas, Bedienung mit Glas (Abschnitt 1).
5. Kontrast messen (Ziel 4,5), hell und dunkel. Helle Werte sind bisher abgeleitet, nicht gemessen (alle Aufnahmen waren dunkel).
6. Abweichungen bewusst entscheiden, Lars sagen, in Abschnitt 9 eintragen.

## 1. Zwei Ebenen
- **Inhalt** (Kacheln, Listen, Texte): kein Glas, feste Flächen.
- **Bedienung** (Reiter-Leiste, Menüs, Dialoge, Hinweise): Glas, schwebt über dem Inhalt. Sparsam, kein Glas auf Glas.
- Bedienelemente im Inhalt (⋯) sind in Ruhe schlicht und werden erst beim Berühren zu Glas.

## 2. Glas im Browser: was geht und was nicht
- Apples fertiges Glas gibt es nur in echten Apps. **Echte App ist ausgeschlossen** (kein Entwicklerkonto; „Link öffnen und los“ soll bleiben). SwiftUI-Anleitungen und WebGL-Tricks (brauchen Inhalt im 3D-Bild) helfen nicht.
- Safari kann den Hintergrund nur weichzeichnen/einfärben, nicht verbiegen.
- **Lichtbrechung („Variante A“, von Lars gewählt):** Eine Kopie des Seiteninhalts (`main.cloneNode`) liegt in schmalen Randstreifen des Glas-Elements und wird dort auf 40 % gestaucht (nur `transform`). Die Streifen laufen per Maske weich nach innen aus, darüber liegt eine leichte Weichzeichnung (`backdrop-filter: blur(2px)`), damit kein doppelt lesbarer Text entsteht. Mitlaufen beim Scrollen über `animation-timeline: scroll()` (ohne Verzögerung), sonst per Scroll-Ereignis. Kopie nach Inhaltsänderungen neu aufbauen. Ein echter Verzerrungs-Filter über die ganze Fläche (Variante B) war Lars zu durchsichtig.
- Neutrales Glas: keine Farbverstärkung, die die Leiste bunt macht. Dunkel etwa `rgba(38,38,40,.4)` + `blur(7px) saturate(170%)`, feine helle Kante nur oben und unten, kein Ring rundherum, unter der Leiste keine sichtbare Scroll-Kante.

## 3. Reiter-Leiste (gemessen an der Uhr-App, iPhone 420 pt breit)
- Unten, schwebend. Höhe 64, seitlich 14, 20 vom unteren Bildschirmrand (im Sicherheitsbereich), volle Kapselform. Immer sichtbar, schrumpft nicht beim Scrollen.
- **Alle Symbole gefüllt**; gewählt ändert nur die Farbe (Akzent). Namen 10 pt halbfett. Symbol ca. 23–25.
- Gewählter Reiter in Ruhe: flache, **fast schwarze Pille** (dunkel) bzw. leicht graue (hell), Abstand 5 zum Leistenrand, ohne Rand.
- **Lupe** (beim Berühren): klares Glas, ca. 1,3-fach so groß wie die Pille (ca. 72 hoch, 120 breit), ragt oben, unten und am Leistenende **über die Leiste hinaus**. Feine helle Kante oben/unten, Glanz in der Fläche, Regenbogen-Säume dezent. **Kein Rahmen:** keine harte Innenkante, kein dunkler Ring; aber auch nicht so weich, dass nur ein Schleier bleibt. Inhalt darin in Akzentfarbe, nur ca. 1,12-fach vergrößert (lange Namen stoßen sonst an). Am linken/rechten Rand wird der Inhalt in die Breite gezogen; die unverzerrte Mitte endet genau an den Randstreifen (sonst doppelte „Fetzen“). Was unter der Lupe liegt, wird ausgeschnitten.
- **Bewegung:** Wechsel sehr schnell (60–80 ms). Die Pille hat zwei Kanten: die vordere springt sofort zum Ziel, die hintere folgt ca. 45 ms später und federt leicht nach; die Lupe zieht sich kurz über beide Reiter. Kurzes Tippen: Lupe hebt sich nur halb. Halten/Ziehen: Lupe ganz groß, folgt dem Finger direkt, Leiste ca. 1 % kleiner. Loslassen: Lupe fällt in ca. 150 ms zur Pille zusammen. Nichts darf zucken oder nachwackeln („hektisch“ war ein Kritikpunkt).

## 4. ⋯-Menü (gemessen an der Musik-App: ⋯ in einem Eintrag)
- ⋯ in Ruhe: nur drei Punkte. Beim Berühren kurz Glas. Antippen oder Halten öffnet; beim Halten Finger auf Eintrag ziehen und loslassen = wählen.
- **Öffnen:** startet als runder, schon ca. 45 % großer Fleck am Knopf, nach ca. 0,1 s fast volle Größe, Breite ca. 2 % über, Höhe läuft ruhig aus. Einträge von Anfang an da (klein, unscharf), werden scharf. Kein starkes Wabbeln.
- **Schließen:** ca. 0,1 s: schrumpft etwas zum Knopf, verschwimmt, weg. Danach erscheinen die Punkte wieder.
- Breite 250, Ecken 30, Zeilen 48 hoch, Schrift 17, Symbole links in einer Spalte, Trennlinie zwischen Gruppen links/rechts eingerückt, Löschen in Apple-Rot unten in eigener Gruppe.
- Glas wie die Leiste, mit Lichtbrechung am Rand (Abschnitt 2). Lars will es eher gläsern; dafür ist Rot über orangem Hintergrund schwächer lesbar (roter Text hat deshalb einen Schein).
- Offen: Seite scrollt nicht, Tipp daneben schließt nur.

## 5. Bewegung allgemein
- Federn statt fester Zeiten, unterbrechbar (neuer Tipp lenkt weich um). „Bewegung reduzieren“ beachten.
- Apple ist schneller und ruhiger als gedacht: erst messen. Apples SwiftUI-Voreinstellungen als Anhalt: smooth (0,5 s, kein Nachfedern), snappy (leicht), bouncy (Bounce 0,3). Umrechnung: Steifheit = (2π/Dauer)², Dämpfungsverhältnis = 1 − Bounce.
- Nur `transform`/`opacity` animieren; Weichzeichnen während großer Bewegungen ruckelt auf dem iPhone.

- **Seiten wie in iOS** (Wunsch Lars 12.11.2026: „aus einem Guss“): Tiefer gehen (Start → Reise → Unterseite, Assistent weiter) schiebt die neue Seite von rechts herein, die alte weicht ein Stück nach links und wird blasser; Zurück umgekehrt. Reiter wechseln sofort ohne Bewegung (wie iOS; Überblenden sah doppelt aus). Zurück landet an derselben Scroll-Stelle; wischt Safari selbst zurück, keine zweite Bewegung. Technik: View Transitions (`navTo('push'|'pop'|'tab')`, Tiefe per `depth()`), bewegt werden nur Bildschirmfotos; ohne Unterstützung oder bei „Bewegung reduzieren“ einfach ohne.
- **Eine Kurve für alles**: `cubic-bezier(.32,.72,0,1)` (iOS: schnell los, weich auslaufend, Variable `--ios`, im Code `MO`). Dauern: Seiten 0,42 s, Aufklappen 0,30–0,46 s je nach Höhe, zu 70 % davon, Umschalter 0,42 s, Inhalt blendet 0,22 s ein.
- **Oberer Rand beim Scrollen** (iOS 26): Inhalt unter Uhr und Akku blendet weich in den Grund aus (`.sedge`, nur Farbverlauf: Unschärfe mit Maske ergab in Safari eine harte Kante); ist der große Titel weggescrollt, steht er klein in der Mitte (`#ntitle`, `topTitle`).
- **Lichtbrechung der Reiter-Leiste** wird erst neu berechnet, wenn keine Bewegung läuft (`refrSoon` wartet auf `slides`/`navBusy` und die gleitende Pille `raf`), sonst ruckeln Bewegungen. Bei Reiter- oder Umschalter-Wechsel blenden die Ränder sofort aus (`refrHide`, `.refr.rgo`) und mit dem neuen Inhalt wieder ein; vorher zeigten sie bis zu 0,8 s die alte Seite und sprangen dann um (Video Lars 12.11.2026).
- **Reise-Info** ist eine eigene Seite (`#/r/{id}/info`), auf der Reiseseite nur eine Zeile mit Pfeil wie in den Einstellungen (Wetter-Symbol, Temperatur, Unterkunft, Anreise). Riesige Aufklapp-Kacheln vermeiden.
- **Wetter wie die Wetter-App**: oben jetzt (Symbol, große Temperatur, Beschreibung, H/T), darunter eine Zeile je Tag (Wochentag bzw. „Heute“ und darunter klein das Datum, Symbol mit Regen-% ab 30 %, Tiefst, Balken mit Temperatur-Verlauf über die Spanne aller gezeigten Tage, Punkt = jetzt, Höchst). Liegen unsere Tage am Ziel in der 16-Tage-Vorhersage, zeigen wir genau diese („Unsere Tage dort“), sonst die nächsten 7 Tage mit Hinweis.
- **Unterkunft und Anreise** als gruppierte Listen (`.glist`/`.gl2`) wie in den Einstellungen; Links als eigene Zeilen mit Pfeil.
- **Eine Bewegungssprache** (Wunsch Lars 12.11.2026: Gleiches muss sich gleich anfühlen; im Code `MO`, `openAnim`, `shut`, `foldRow`, `closeForm`, `segSwitch`):
  - Aufklapp-Zeilen (Kommentare, Einkaufsliste, Erledigt, Archiv, Vorschläge, Kosten-Übersichten) und Formulare (Vorschlag, Ausgabe, Reservierung, Stopp, Fahrt): Höhe + Deckkraft mit der iOS-Kurve (siehe oben). Gilt für Abbrechen und Speichern gleich. Der Pfeil dreht sich im selben Takt.
  - Umschalter (Reise-Info, Essen): Perle gleitet 0,42 s mit Feder von der alten Stelle, der Inhalt darunter blendet in 0,22 s ein.
  - Während einer Bewegung warten Live-Daten anderer (`slides`), damit nichts abreißt.
- **Filter** sind Umschalter (Segmented Control) wie in iOS, keine losen Pillen; erst ab 5 Einträgen eine wischbare Pillen-Zeile. Ändert sich eine Liste (Filter), gleiten die Kacheln an ihre neue Stelle, nichts springt.
- **Antippen statt Berühren** bei Auswahllisten (Ortsvorschläge): Übernommen wird erst beim Loslassen ohne Bewegung und ohne Scrollen (Lars: Scrollen übernahm sonst einen Vorschlag).

## 6. Schrift
- `-apple-system, BlinkMacSystemFont, system-ui, "Segoe UI", sans-serif`. Keine eigenen Buchstabenabstände, keine gesperrten Großbuchstaben.
- **Titel, Knöpfe und Countdown in Apples runder Schrift** (`ui-rounded`, Variable `--rnd`; Entscheidung Lars 10.11.2026). Fließtext, Infozeilen, Menüs und Reiter bleiben in der Standardschrift. Windows hat keine runde Schrift, dort Segoe UI.
- Nur Apples Größen: 34 fett (Seitentitel) · 28/22/20 (Titel, Kachel-Titel 20 halbfett) · 17 (Text, Listen, Menüs) · 16 · 15 (Infozeile, Namen, Knöpfe halbfett) · 13 (Hinweise) · 12/11 · 10 halbfett (Reiter).

## 7. Inhalte
- Kacheln ohne Rahmen, Radius 22, heben sich durch die Fläche ab.
- Knöpfe: Kapseln, normal grau getönt mit Akzentschrift, gewählt gefüllt. **Keine Emojis in Knöpfen**, nur Linien-Symbole. Emojis nur in Inhalten, die Leute schreiben.
- **Grund: Apple-Neutral** (Entscheidung Lars 10.11.2026, löst Sand + Meerestürkis ab). Apples Systemgrau: Grund `#f2f2f7` / dunkel `#000`, Kacheln `#fff` / `#1c1c1e`, Text Schwarz/Weiß, Trennlinien `#e0e0e5` / `#38383a`, Füllung `rgba(118,118,128,.12/.24)`. Nebentext `#636366` / `#98989f` (bewusst dunkler als Apples Nebentext, der auf Weiß nur ca. 3,5 erreicht).
- **Stimmung je Reise** (feste Auswahl, `MOODS` in `index.html`: Meer, Insel, Sonne, Wüste, Natur, Berge, Winter, Stadt, Frühling, Nacht; beim Anlegen gewählt, Admin ändert über ⋯). Sie bestimmt zwei Dinge:
  - **Kopf mit Verlauf über die Seite:** Der Verlauf blendet unter Safaris Balken weich ein (oben 120 px Übergang aus dem Grund, sanft beginnend und auslaufend – ein gleichmäßiger Übergang zeigt an seinem Ende eine sichtbare Linie; der Balken selbst bleibt neutral, Safari färbt ihn nach dem Seitengrund und verändert dabei den Ton), füllt den oberen Bildschirm und läuft unten weich in den Grund aus. Darauf weiß: Countdown-Kapsel (dunkles Glas), Titel, Zeitraum, Kreise der Mitreisenden. Die Verläufe sind bewusst kräftig statt pastellig: Ihr hellster Punkt ist so gewählt, dass weiße Schrift dort mind. 3,9 (Titel) erreicht. Eine Abdunklung über dem Verlauf wurde probiert und verworfen (zu trüb, Lars). Der Knopf direkt darunter liegt auf Kachel-Fläche.
  - **Akzentfarbe** für gefüllte Knöpfe, Knopfschrift, Links und den gewählten Reiter: Apples Systemfarbe in der **kontraststarken Fassung** (Tabelle „Increased contrast“ der Richtlinie, Stand 9.6.2025), weil Weiß auf Apples Standardfarben nur 2,2–2,3 erreicht (Türkis, Orange, Grün). Gemessen: Weiß auf Knopf 4,5–6,1; Akzent auf dunkler Kachel 7,8–10,5. Startseite: Meer.
- Neue Stimmung = Verlauf + Apple-Farbe (hell und dunkel) + Kontrast messen. Keine frei mischbaren Farben (Lesbarkeit nicht garantierbar).
- Apples Leitlinie dazu (Richtlinie „Color“ und WWDC 2025/2026): Farbe gehört in den Inhalt, nicht in die Leisten; Glas bleibt neutral und nimmt die Farbe darunter auf; Akzent sparsam und mit Bedeutung.

- **Umschalter** (Reise-Info, Essen): Kapsel mit gleitender Perle (`.seg2`/`.ind2`, Position per `translate`, Feder-Kurve), wie der abgenommene Umschalter der Vorlage. Auf der Karte als Glas (`.mseg`), weil sie über dem Inhalt schwebt.
- **Schalter** (Bereiche): iOS-Schalter 51×31, an = Systemgrün.
- **Karte**: Vollbild, wächst per `transform: scale` aus dem Karte-Knopf, Inhalt weicht auf 94 % zurück; nur transform/opacity. Schließen-Knopf und Filter schweben als Glas.
- Knöpfe, die auf dem Verlauf im Kopf liegen, haben den Kachel-Grund (sonst zu blass).

- **Laden** (Wunsch Lars 11.11.2026): Startbildschirm mit Schriftzug „outbndr.“, der Punkt hüpft leise (steht schon im HTML, erscheint also vor dem Programm; nach 6 s „Einen Moment, wir packen noch.“, nach 15 s „Die Verbindung ist gerade langsam.“ + Neu laden). Beim Öffnen einer Reise: Name, Zeitraum und Stimmung sofort aus der gemerkten Liste, der Rest als Platzhalter in Form der echten Kacheln mit wanderndem Glanz (nur transform, bei „Bewegung reduzieren“ ohne). Nach 8 s Hinweis „Dauert etwas länger“ mit „Nochmal“, nach 20 s „Keine Verbindung“. Wetter lädt als Platzhalter-Kacheln, Karte zeigt „Karte lädt …“. Offline: Glas-Hinweis oben, Änderungen gehen raus, sobald wieder Netz da ist.

## 8. Bedienung
- **Kein Wischen, kein Halte-Menü auf Kacheln** (Entscheidung Lars: in einer Web-App schwer zu bedienen und nicht intuitiv). Alles läuft sichtbar über ⋯.
- Tippflächen mind. 44.

## 9. Bewusste Abweichungen
- Web-App statt echter App; Lichtbrechung nur über die Inhalts-Kopie am Rand.
- Schriftzug „outbndr.“ mit eigenem engen Buchstabenabstand (Logo).
- Windows: Segoe UI statt SF Pro.
- Menü etwas durchsichtiger als in der Musik-App (Wunsch Lars).
- Nebentext dunkler als bei Apple; Akzentfarben in Apples kontraststarker Fassung statt Standard (Lesbarkeit, Abschnitt 7).
- Verläufe der Stimmungen sind frei gestaltet (dafür gibt es keine Apple-Werte).
