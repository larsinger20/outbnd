# outbnd – Design-Grundlage (gilt IMMER)

Wunsch von Lars: Jede Entscheidung zu Aussehen und Bewegung richtet sich nach Apples aktuellem iOS-Design (iOS 27, „Liquid Glass“). Bei Unsicherheit: „Wie macht es eine Apple-App?“ – und das nachbauen, nicht etwas Eigenes erfinden.
Stand: November 2026. Quellen: Apple Human Interface Guidelines (HIG), WWDC25 „Meet Liquid Glass“, SwiftUI-Doku `Spring`, Berichte zu iOS 27 (WWDC26, Beta-Tests).

## 0. Apple-Check (vor JEDER Änderung, nichts ungeprüft übernehmen)
Alles hinterfragen, auch eigene frühere Entscheidungen und Wünsche, die nach „Apple“ klingen. Vor jeder Änderung beantworten:
1. **Gibt es das so in einer Apple-App?** Welche App, welcher Bereich (Bedienleiste oder Inhalt)? Wenn nein: nicht erfinden, sondern Apples Muster suchen.
2. **Schrift:** nur Apple-Größen (Tabelle in Abschnitt 8), keine eigenen Buchstabenabstände.
3. **Ebene:** Inhalt ohne Glas, Bedienung mit Glas (Abschnitt 1). Vergleiche nur mit Gleichem (ein ⋯ in Safaris Leiste ist nicht dasselbe wie ein ⋯ in einer Kachel).
4. **Bewegung:** Apple-Feder (Abschnitt 4), unterbrechbar, „Bewegung reduzieren“ beachten.
5. **Lesbarkeit:** Kontrast messen (mind. 4,5, auch über bunten Bildern), hell und dunkel.
6. **Gesten:** Was man bei Apple antippen, halten oder wischen kann, muss bei uns genauso gehen (Abschnitt 10).
7. **Bei Abweichung:** bewusst entscheiden, Lars sagen und in Abschnitt 11 eintragen.
Vorbilder für Abläufe am besten per Bildschirmaufnahme vom iPhone Bild für Bild nachmessen (Aufnahmen haben 60 Bilder/s).

## 1. Zwei Ebenen
- **Inhalt** (Kacheln, Listen, Texte): kein Glas. Feste Flächen, Farbe gehört in den Inhalt.
- **Bedienung** (Reiter-Leiste, schwebende Knöpfe, Umschalter, Menüs, Dialoge, Hinweise): Liquid Glass, schwebt über dem Inhalt.
- Glas sparsam einsetzen. Kein Glas auf Glas stapeln.
- Ausnahme: Ein Bedienelement im Inhalt (z. B. ⋯, Schieberegler) wird **beim Berühren** kurz zu Glas („hebt sich in Glas“) und ist in Ruhe unauffällig.

## 2. Das Glas
- **Normales Glas** (Standard): macht den Hintergrund unscharf und passt die Helligkeit an, damit Text lesbar bleibt. Für Leisten, Menüs, Dialoge.
- **Klares Glas**: sehr durchsichtig, nur über Fotos/Bildern, dann mit Abdunkelung dahinter (ca. 35 % bei hellem Hintergrund).
- **Linse statt Milchglas**: Apples Glas bricht das Licht am Rand. Auf Webseiten in Safari nicht echt möglich → mit Lichtkante andeuten (oben links heller Rand, unten rechts schwächer), nicht nur weichzeichnen.
- **iOS 27**: Glas mit **klareren Rändern statt starker Schatten**, insgesamt etwas weniger durchsichtig, im Dunkelmodus heller. Gewählter Reiter im Dunkelmodus: Hintergrund **dunkler** statt heller.
- **Größe ändert das Material**: Je größer ein Glas-Element (z. B. Menü), desto „dicker“: tieferer, weicherer Schatten, mehr Lesbarkeit. Große Flächen wechseln nicht zwischen hell und dunkel, kleine (Leisten) dürfen.
- **Farbe/Tönung** nur für die wichtigste Aktion, nie alles einfärben.
- Ecken **konzentrisch**: Innere Rundungen passen in äußere (innerer Radius = äußerer Radius − Abstand). iOS 27 rundet etwas weniger übertrieben.

## 3. Reaktion auf Berührung
- Glas reagiert **sofort** beim Aufsetzen des Fingers (nicht erst beim Loslassen): wölbt sich leicht auf, „wie Gel“.
- **Leuchten von innen**, beginnend unter dem Finger, breitet sich über das Element aus (und dezent auf Glas daneben).
- Reiter-Leiste: Finger halten und ziehen → die Auswahl hebt sich als Lupe und folgt dem Finger, beim Loslassen schnappt sie ein.
- Bewegung immer **unterbrechbar**: Neue Berührung mitten in einer Animation → weich umlenken, Schwung behalten. Nie warten lassen.

## 4. Bewegung (Federn statt fester Zeiten)
Apples Feder-Voreinstellungen (SwiftUI) als feste Werte:
| Name | Wofür | Werte |
|---|---|---|
| smooth | Ein-/Ausblenden, Seitenwechsel | 0,5 s, Bounce 0 |
| snappy | häufige kleine Dinge, Standard | 0,5 s, Bounce ca. 0,15 |
| bouncy | Glas-Perle, Aufploppen von Menüs | 0,5 s, Bounce 0,3 (Steifheit 158, Dämpfung 17,6) |
| interactive | folgt dem Finger | ca. 0,15 s, Dämpfung 0,86 |
- Umrechnung: Steifheit = (2π / Dauer)², Dämpfungsverhältnis = 1 − Bounce.
- iOS 27: „snappier“, also eher knackig als lang. Bounce über 0,3 wirkt verspielt → vermeiden.
- Häufige Aktionen: wenig Bewegung. „Bewegung reduzieren“ in den Einstellungen immer beachten (dann nur Überblenden).

## 5. Menüs (⋯)
- **Das Menü wächst aus dem Knopf** („die Blase ploppt auf und zeigt ihren Inhalt“): gleiche Stelle, gleiche Glasfläche, kein separates Fenster daneben. Schließen: zurück in den Knopf.
- Hintergrund wird **nicht abgedunkelt** und bleibt sichtbar.
- Antippen daneben schließt nur (die Berührung löst sonst nichts aus, Seite scrollt nicht).
- Finger auf ⋯ halten → Menü öffnet, Finger auf einen Eintrag ziehen und loslassen = auswählen (Markierung folgt dem Finger).
- Einträge: Verb, kurz, wichtigste oben, verwandte gruppiert (Trennlinie/Abstand), Löschen rot und unten. Symbole **für alle Einträge einer Gruppe oder für keinen**, als Linien-Symbole in einer Spalte.
- Nicht verfügbare Einträge blass zeigen statt verstecken. „…“ am Ende, wenn danach noch Eingaben kommen.
- **Ablauf wie Safari (nachgemessen):** Knopf hellt beim Antippen auf → wird zum runden, klaren Glas-Tropfen → wächst schräg zum Menü (nach ca. 0,25 s fast fertig), Breite und Höhe federn getrennt (wobbelt wie Gel), Einträge erst vergrößert und unscharf wie durch eine Lupe, dann scharf. Schließen: Einträge verschwimmen, Menü wird rund, schrumpft zum Tropfen mit Spitze zum Knopf, der Knopf taucht schon auf, während der Rest in ihn hineinfließt, ist kurz etwas zu groß und federt zurück.
- Menü-Glas recht durchsichtig (Farben dahinter leuchten durch), mit Lichtkante; Schrift mit leichtem Schein (Vibrancy), Löschen in kräftigem Apple-Rot. Kontrast über bunten Bildern messen.

## 6. Reiter-Leiste
- **Unten**, schwebend, Glas, Inhalt scrollt darunter durch. Immer sichtbar (nicht beim Scrollen verkleinern, das ist nur für Apps mit Mini-Player gedacht).
- Symbol + kurzer Name (ein Wort, wenn möglich), gefüllte Symbole bevorzugt für den gewählten Reiter.
- Nur Navigation, keine Aktionen in der Leiste. Wenige Reiter (bei uns 4).
- Hinweis-Punkt/Zahl nur für wirklich Neues.
- Am oberen/unteren Rand: **Scroll-Kante** (Inhalt wird unter der Leiste unscharf/blass, damit die Leiste lesbar bleibt). Nicht als Deko.

## 7. Sonstiges
- Tippflächen mind. 44 px.
- Hell und dunkel immer beide gestalten, ebenso „Transparenz reduzieren“ (Glas wird dann fest).
- Dialoge/Nachfragen wachsen ebenfalls aus dem Knopf, der sie auslöst.
- Alles auf dem echten iPhone prüfen (Safari und Home-Bildschirm-App).

## 8. Schrift
- Systemschrift: `-apple-system, BlinkMacSystemFont, system-ui, "Segoe UI", sans-serif`. Auf Apple-Geräten SF Pro (wechselt selbst zwischen Text- und Display-Schnitt), auf Windows Segoe UI (SF Pro darf dort nicht eingebunden werden).
- **Keine eigenen Buchstabenabstände** (`letter-spacing`): SF Pro bringt sie je nach Größe selbst mit.
- **Keine Großbuchstaben-Überschriften mit Sperrung** (seit iOS 26 normal geschrieben, fett).
- Nur Apples Größen (Dynamic Type, Standardgröße):
| Stil | Größe | Gewicht | Wofür bei uns |
|---|---|---|---|
| Large Title | 34 | fett | Seitentitel |
| Title 1 / 2 / 3 | 28 / 22 / 20 | normal bzw. halbfett | Abschnitte, Kachel-Titel (20 halbfett) |
| Headline | 17 | halbfett | Hervorhebung |
| Body | 17 | normal | Listen, Menüs, Fließtext |
| Callout | 16 | normal | selten |
| Subheadline | 15 | normal | Infozeile, Namen, Untertitel, Knöpfe (halbfett) |
| Footnote | 13 | normal | Hinweise, Umschalter |
| Caption 1 / 2 | 12 / 11 | normal | Bildbeschriftung, Avatare |
| Reiter-Namen | 10 | mittel | Reiter-Leiste |

## 9. Inhalte
- **Kacheln:** ohne Rahmen, heben sich durch die Fläche ab (weiß auf Sand bzw. dunkelgrau), Radius 22, kein Schatten.
- **Knöpfe:** Kapseln. Normal = grau getönt mit Akzentschrift, gewählt/wichtig = gefüllt in Akzentfarbe. **Keine Emojis in Knöpfen**, nur Symbole (Linien-Symbole wie SF Symbols). Emojis nur in Inhalten, die Leute selbst schreiben.
- **⋯ in einer Kachel:** in Ruhe nur die drei Punkte (wie Musik/Podcasts), beim Berühren hebt es sich als Glas-Knopf, daraus wächst das Menü.
- Reiter-Leiste: gewählter Reiter mit **gefülltem** Symbol.

## 10. Gesten (was Apple-Nutzer erwarten)
- **Gedrückt halten auf einem Eintrag** → Kontextmenü: Eintrag schrumpft beim Halten leicht, hebt sich dann an, alles andere wird unscharf und leicht abgedunkelt, Menü erscheint darunter (auf der Seite, wo der Finger ist). Passt es nicht, wird der Eintrag verkleinert/verschoben. Finger halten und auf einen Eintrag ziehen = auswählen. Am PC: Rechtsklick.
- **Nach links wischen** (iOS 27: bei allen Einträgen in Listen) → Aktionen dahinter (z. B. Erledigt, Löschen). Wischen nach oben/unten scrollt weiter normal; Scrollen schließt offene Aktionen. Löschen fragt trotzdem nach.
- Tippflächen und Gesten nie nur per Wischen erreichbar machen: alles auch über ⋯ erreichbar.

## 11. Bewusste Abweichungen (mit Grund)
- Schriftzug „outbnd“: eigener enger Buchstabenabstand (Logo, kein Text).
- Windows: Segoe UI statt SF Pro (Lizenz).
- Wisch-Aktionen als runde Farbknöpfe mit Namen darunter: Apples genaue iOS-27-Form noch auf dem iPhone vergleichen.
- Echte Lichtbrechung am Glasrand ist im Browser nicht möglich, nur angedeutet.

## Referenz
Probier-Seite mit allen Mustern (Reiter-Leiste, ⋯-Menü, Halten, Wischen): https://claude.ai/artifact/WYuRH8EcEk9p1NLY51npaB
