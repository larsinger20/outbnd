# outbnd – Design-Grundlage (gilt IMMER)

Wunsch von Lars: Jede Entscheidung zu Aussehen und Bewegung richtet sich nach Apples aktuellem iOS-Design (iOS 27, „Liquid Glass“). Bei Unsicherheit: „Wie macht es eine Apple-App?“ – und das nachbauen, nicht etwas Eigenes erfinden.
Stand: November 2026. Quellen: Apple Human Interface Guidelines (HIG), WWDC25 „Meet Liquid Glass“, SwiftUI-Doku `Spring`, Berichte zu iOS 27 (WWDC26, Beta-Tests).

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

## 6. Reiter-Leiste
- **Unten**, schwebend, Glas, Inhalt scrollt darunter durch. Immer sichtbar (nicht beim Scrollen verkleinern, das ist nur für Apps mit Mini-Player gedacht).
- Symbol + kurzer Name (ein Wort, wenn möglich), gefüllte Symbole bevorzugt für den gewählten Reiter.
- Nur Navigation, keine Aktionen in der Leiste. Wenige Reiter (bei uns 4).
- Hinweis-Punkt/Zahl nur für wirklich Neues.
- Am oberen/unteren Rand: **Scroll-Kante** (Inhalt wird unter der Leiste unscharf/blass, damit die Leiste lesbar bleibt). Nicht als Deko.

## 7. Sonstiges
- Tippflächen mind. 44 px.
- Systemschrift (SF Pro auf dem iPhone).
- Hell und dunkel immer beide gestalten, ebenso „Transparenz reduzieren“ (Glas wird dann fest).
- Dialoge/Nachfragen wachsen ebenfalls aus dem Knopf, der sie auslöst.
- Alles auf dem echten iPhone prüfen (Safari und Home-Bildschirm-App).
