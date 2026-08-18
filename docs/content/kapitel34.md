# Kapitel 34 – Ideenwerkstatt: Use Cases finden, bewerten, priorisieren

{{ progress(34) }}

<div class="lernziele" markdown>
<h3>Was du in diesem Kapitel lernst</h3>

- Sieben Suchmuster, mit denen du **Automatisierungsideen systematisch findest** statt sie zu erraten
- Ein **Bewertungsraster** mit sechs Dimensionen und einer konkreten Punkteskala
- Wie eine Bewertung praktisch aussieht – an vier Ideen **durchgerechnet**
- Wie du aus der **Aufwand-Nutzen-Matrix** eine begründete Startreihenfolge ableitest
- Den **Use-Case-Steckbrief** als kopierbare Vorlage
- Warum ein **Pilot** dem Großprojekt überlegen ist und wie du den Erfolg tatsächlich misst
</div>

---

## 34.1 Ideen systematisch finden

Die übliche Ideensammlung beginnt mit der Frage „Was könnten wir mit KI machen?" – und endet mit einer Wunschliste, die niemand umsetzt. Sie fragt nach der Lösung, bevor das Problem benannt ist. Die bessere Frage lautet: **Wo tut es weh?**

Sieben Suchmuster führen zuverlässig zu brauchbaren Kandidaten:

| Suchmuster | Leitfrage | Typischer Fund |
|---|---|---|
| **Zeitfresser** | Was dauert regelmäßig länger, als es sollte? | Belege abtippen, Berichte zusammenstellen |
| **Medienbrüche** | Wo wechseln Daten das Format oder das System? | Mailanhang wird von Hand in eine Liste übertragen |
| **Doppelerfassung** | Wo wird dasselbe zweimal eingegeben? | Auftragsdaten in Fachsystem und Excel |
| **Warteschlangen** | Wo liegt etwas herum und wartet auf jemanden? | Freigaben, Rückmeldungen, Prüfungen |
| **Wiederkehrende Rückfragen** | Welche Frage wird jede Woche neu gestellt? | Wo finde ich, wie beantrage ich, wer ist zuständig |
| **Fehlerquellen** | Wo passieren regelmäßig dieselben Fehler? | Zahlendreher, vergessene Felder, falscher Empfänger |
| **Tag-im-Leben-Analyse** | Was tut eine Person an einem normalen Tag wirklich? | die unsichtbaren Kleinaufgaben zwischen den großen |

Die letzte Methode ist die ergiebigste und die unbequemste: Eine Person notiert einen Arbeitstag lang, was sie tut, in Blöcken von etwa fünfzehn Minuten. Nichts wird beschönigt, auch nicht die dreimal am Tag wiederholte Suche nach derselben Datei. Das Ergebnis überrascht regelmäßig – die größten Zeitfresser stehen fast nie in Prozessbeschreibungen, weil sie niemand für erwähnenswert hält.

Beim Aufschreiben gilt eine strenge Regel: **Frage nach Tätigkeiten, nicht nach Themen.** „Buchhaltung" ist keine Idee. „Eingangsrechnungen prüfen und kontieren" ist eine. Formuliere jede Idee als Tätigkeit mit einem Objekt, also Verb plus Gegenstand. Wer das nicht kann, hat noch keine Idee, sondern ein Themengebiet – und Themengebiete lassen sich weder bewerten noch umsetzen (Kap. 17).

!!! warning "Häufiges Missverständnis: die KI-Brille"
    Wer mit der Frage „Wo können wir KI einsetzen?" loszieht, findet KI-Anwendungen – auch dort, wo eine Bedingung oder eine bessere Excel-Vorlage genügt hätte. Sammle deshalb erst die Schmerzpunkte, und entscheide **danach**, ob der Fall überhaupt KI braucht (Kap. 29). Ein guter Ideenvorrat enthält immer auch Fälle, die man ohne KI löst.

---

## 34.2 Das Bewertungsraster

Ideen zu sammeln ist leicht, sie zu vergleichen ist die eigentliche Arbeit. Dafür bewertest du jede Idee in sechs Dimensionen mit **1 bis 5 Punkten**. Entscheidend ist die Richtung: **Hohe Punktzahl heißt immer günstig.** Bei Aufwand und Risiko bedeutet das, dass wenig Aufwand und geringes Risiko hohe Punkte bekommen.

| Dimension | 1 Punkt | 3 Punkte | 5 Punkte |
|---|---|---|---|
| **Nutzen** | spart kaum spürbar Zeit oder Ärger | spürbare Entlastung für ein Team | deutliche Entlastung oder erkennbar bessere Qualität |
| **Aufwand** | Wochen, externe Hilfe nötig | einige Tage | in ein bis zwei Tagen baubar |
| **Risiko** | rechtlich heikel, Fehler wirken nach außen | Fehler intern korrigierbar | Fehler folgenlos, leicht rückgängig |
| **Datenverfügbarkeit** | Daten fehlen oder liegen auf Papier | Daten vorhanden, aber uneinheitlich | Daten digital, strukturiert, zugänglich |
| **Akzeptanz** | Betroffene lehnen ab oder fürchten um ihre Arbeit | gemischt, Überzeugungsarbeit nötig | Betroffene wünschen es sich ausdrücklich |
| **Wiederholhäufigkeit** | einige Male im Jahr | wöchentlich | täglich oder mehrmals täglich |

Zusätzlich gilt eine **Ausschlussregel**: Eine Idee mit 1 Punkt beim Risiko fällt aus der Bewertung heraus, unabhängig von ihrer Gesamtpunktzahl. Rechtlich heikle Vorhaben werden nicht durch hohen Nutzen erlaubt. Das betrifft in der Praxis vor allem Personalentscheidungen, Bonitätseinschätzungen und alles, was Menschen bewertet (Kap. 4 und 14).

!!! info "Merksatz"
    Die unterschätzte Dimension ist die **Datenverfügbarkeit**. Sie ist keine gleichberechtigte Dimension unter sechs, sondern eine Voraussetzung: Eine Idee mit hohem Nutzen und 2 Punkten bei den Daten ist kein guter Startkandidat, sondern ein Datenprojekt mit angehängter Automatisierung. Das darf man machen – aber man sollte wissen, dass man es tut.

---

## 34.3 Vier Ideen, durchgerechnet

!!! example "Bewertung bei der Bergmüller Haustechnik GmbH"
    Ausgangslage: 120 Beschäftigte, Innendienst mit sechs Personen, Buchhaltung mit drei, keine eigene IT-Entwicklung. Vier Ideen aus der Sammelrunde:

    | Idee | Nutzen | Aufwand | Risiko | Daten | Akzeptanz | Häufigkeit | Summe |
    |---|---|---|---|---|---|---|---|
    | A Eingangsrechnungen vorerfassen | 5 | 2 | 3 | 4 | 4 | 5 | **23** |
    | B Serviceanfragen vorsortieren | 4 | 4 | 4 | 5 | 4 | 5 | **26** |
    | C Bewerbungen vorbewerten | 3 | 3 | 1 | 3 | 2 | 2 | **14** |
    | D Wochenbericht aus Projektnotizen | 3 | 5 | 5 | 2 | 4 | 4 | **23** |

    **Idee C** fällt sofort heraus: 1 Punkt beim Risiko löst die Ausschlussregel aus. Die Vorbewertung von Bewerbungen berührt Diskriminierungsrisiken und gehört im EU AI Act zu den besonders streng regulierten Anwendungen. Zusätzlich ist die Akzeptanz mit 2 Punkten die niedrigste im Feld – ein Projekt, das gegen den Widerstand der Betroffenen gestartet wird, scheitert selten an der Technik.

    **Idee B** führt mit 26 Punkten, und zwar ohne Ausreißer: kein Wert unter 4. Genau das macht sie zum idealen Startkandidaten.

    **Idee A und D** liegen mit je 23 Punkten gleichauf – aber das Gleichstandsergebnis täuscht. A hat den mit Abstand höchsten Nutzen und den höchsten Aufwand. D ist billig zu bauen, scheitert aber wahrscheinlich an der Datenverfügbarkeit: Projektnotizen liegen verstreut in Mails, Notizbüchern und Chatverläufen. Bei gleicher Punktzahl entscheidet deshalb die Frage, welcher niedrige Einzelwert **auflösbar** ist. Hoher Aufwand ist eine Frage der Zeitplanung. Fehlende Daten sind ein eigenes Vorprojekt.

    **Startreihenfolge:** B, dann A, dann D. C wird nicht weiterverfolgt und die Ablehnung schriftlich begründet – damit die Idee nicht in drei Monaten unbewertet wieder auftaucht.

---

## 34.4 Aufwand-Nutzen-Matrix und Startreihenfolge

Die Punktsumme ordnet, aber sie erklärt nicht. Für die Kommunikation mit Entscheidenden ist die **Aufwand-Nutzen-Matrix** wirksamer: zwei Achsen, vier Felder, jede Idee ein Punkt darin.

| | geringer Aufwand | hoher Aufwand |
|---|---|---|
| **hoher Nutzen** | sofort starten – hier liegt der Einstieg | planen und in Etappen bauen |
| **geringer Nutzen** | nebenbei mitnehmen, wenn Zeit ist | nicht machen und Ablehnung begründen |

Für unser Beispiel: Idee B landet oben links und ist damit der Startkandidat. Idee A landet oben rechts – hoher Nutzen, hoher Aufwand – und wird in Etappen geplant, etwa zuerst nur für die fünf größten Lieferanten. Idee D landet unten links und wird mitgenommen, sobald die Notizen ohnehin vereinheitlicht werden.

```mermaid
flowchart LR
    A([Ideen sammeln]) --> B([In sechs Dimensionen bewerten])
    B --> C([Ausschlussregel Risiko pruefen])
    C --> D([In die Matrix eintragen])
    D --> E([Startreihenfolge begruenden])
```

Die Startreihenfolge folgt aber nicht allein der Punktzahl. Drei weitere Überlegungen verschieben sie regelmäßig:

**Sichtbarkeit.** Das erste Vorhaben sollte für andere spürbar sein. Eine Verbesserung, die niemand bemerkt, erzeugt keine Bereitschaft für das zweite.

**Umkehrbarkeit.** Beginne dort, wo ein Fehlschlag folgenlos bleibt. Der erste Versuch ist der, bei dem du am wenigsten weißt.

**Verfügbarkeit der Beteiligten.** Ein Vorhaben, dessen Fachkenntnis bei einer Person liegt, die sechs Wochen nicht ansprechbar ist, ist unabhängig von der Punktzahl kein Startkandidat.

!!! warning "Typische Falle: das Prestigeprojekt zuerst"
    Der häufigste Fehler ist, mit dem größten Vorhaben zu beginnen, weil es den sichtbarsten Effekt verspricht. Groß bedeutet aber fast immer: viele Beteiligte, viele Sonderfälle, lange Bauzeit, spätes Ergebnis. Ein gescheitertes erstes Vorhaben verbrennt die Bereitschaft für alle folgenden – und zwar für Jahre. Beginne klein, liefere schnell, und nimm das große Vorhaben als zweites.

---

## 34.5 Der Use-Case-Steckbrief

Sobald eine Idee weiterverfolgt wird, bekommt sie einen Steckbrief. Er passt auf eine Seite und zwingt zu Klarheit an genau den Stellen, an denen sonst Nebel bleibt. Kopiere diese Vorlage und fülle sie je Idee aus:

```text
USE-CASE-STECKBRIEF

Kurztitel            Verb plus Gegenstand, hoechstens acht Woerter
Bereich              Abteilung oder Team
Verantwortlich       Name der Person, die das Ergebnis vertritt

Problem heute        Was laeuft schlecht, in zwei bis drei Saetzen
Betroffene           Wer macht es heute, wie viele Personen
Haeufigkeit          Wie oft faellt es an, mit Zahl
Zeitaufwand heute    Minuten je Fall und Summe pro Woche

Ausloeser            Was startet den Ablauf
Schritte heute       Nummerierte Liste, hoechstens zehn Schritte
Beteiligte Systeme   Wo liegen die Daten heute

Zielbild             Wie soll es laufen, in drei bis fuenf Saetzen
KI-Anteil            Welcher Teilschritt und welche Einsatzstelle
Deterministischer Teil Was macht der Ablauf verlaesslich
Entscheidungspunkt   Wer entscheidet was und wann
Rueckfallweg         Was passiert, wenn die KI unsicher ist

Bewertung            Nutzen, Aufwand, Risiko, Daten, Akzeptanz,
                     Haeufigkeit, jeweils 1 bis 5, plus Summe
Ausserhalb           Was ausdruecklich nicht dazugehoert
Risiken              Was schiefgehen kann, und die Gegenmassnahme
Rechtliche Punkte    Personenbezug, Aufbewahrung, Mitbestimmung

Erfolgskriterium     Eine messbare Aussage mit Zahl und Frist
Messung vorher       Was wird wann gemessen, bevor gebaut wird
Messung nachher      Was wird wann gemessen, nachdem es laeuft
```

Zwei Felder werden regelmäßig zu schnell abgehakt. **Außerhalb** ist das wirksamste Mittel gegen ausufernden Umfang: Wer aufschreibt, was nicht dazugehört, kann später darauf zeigen. Und **Messung vorher** ist die Bedingung dafür, dass du den Erfolg überhaupt belegen kannst – nachträglich lässt sich der Ausgangszustand nicht mehr erheben.

---

## 34.6 Pilot statt Großprojekt, und richtig messen

Ein **Pilot** ist keine verkleinerte Version des Vorhabens, sondern ein bewusst begrenzter Ausschnitt, der eine Frage beantwortet: Trägt die Idee? Begrenzt wird auf einer von drei Achsen – ein Team statt aller, ein Dokumenttyp statt aller, vier Wochen statt dauerhaft. Zwei Achsen gleichzeitig zu begrenzen ist meist zu viel; dann misst man nichts mehr Aussagekräftiges.

Die Erfolgsmessung entscheidet darüber, ob aus dem Piloten etwas wird. Sie braucht drei Dinge: eine Zahl, einen Zeitpunkt davor und einen Zeitpunkt danach.

| Statt dieser Aussage | Diese Aussage |
|---|---|
| „Spart viel Zeit" | „Bearbeitungszeit je Anfrage von 7 auf unter 3 Minuten, gemessen an 40 Fällen" |
| „Weniger Fehler" | „Anteil unvollständiger Vorgänge von 12 auf unter 5 Prozent in vier Wochen" |
| „Kommt gut an" | „Mindestens 8 von 10 Beteiligten wollen nach vier Wochen weitermachen" |
| „Läuft stabil" | „Höchstens 2 Fehlerabbrüche pro Woche, alle innerhalb eines Tages behoben" |

!!! warning "Ohne Vorher-Messung keine Erfolgsaussage"
    Der häufigste Fehler ist, erst nach dem Bauen zu messen. Dann bleibt nur die Schätzung „gefühlt schneller", und die überzeugt niemanden, der Ressourcen freigeben soll. Miss **vor** dem Bauen, eine Woche lang, mit einer einfachen Strichliste: wie viele Fälle, wie viele Minuten je Fall, wie viele Fehler. Der Aufwand dafür beträgt wenige Minuten am Tag, und ohne diese Zahlen ist jede spätere Nutzenaussage angreifbar.

Am Ende dieses Blocks solltest du eine **bewertete Ideenliste** haben – nicht eine Idee, sondern mehrere, mit Punkten, Reihenfolge und Begründung. Aus dieser Liste wählst du in Kap. 35 das Vorhaben aus, das du in der Projektarbeit umsetzt. Je ehrlicher du hier bewertest, desto weniger Überraschungen erlebst du dort. Und die Ideen, die du jetzt begründet ablehnst, sind kein Verlust: Eine dokumentierte Absage ist genauso wertvoll wie ein Startkandidat, weil sie verhindert, dass dieselbe Diskussion in einem halben Jahr von vorn beginnt.

---

## Zusammenfassung

- Ideen findest du über **sieben Suchmuster** – am ergiebigsten ist die Tag-im-Leben-Analyse; formuliere jede Idee als Tätigkeit, nicht als Thema.
- Das **Bewertungsraster** hat sechs Dimensionen mit 1 bis 5 Punkten, hohe Punktzahl heißt immer günstig, und ein Risikowert von 1 schließt aus.
- Die **Datenverfügbarkeit** ist keine gleichrangige Dimension, sondern eine Voraussetzung; ein niedriger Wert bedeutet ein vorgelagertes Datenprojekt.
- Die **Aufwand-Nutzen-Matrix** übersetzt die Punkte in vier Handlungsempfehlungen; Sichtbarkeit, Umkehrbarkeit und Verfügbarkeit verschieben die Reihenfolge zusätzlich.
- Der **Use-Case-Steckbrief** zwingt zur Klarheit – besonders bei „Außerhalb", „Rückfallweg" und „Messung vorher".
- Ein **Pilot** begrenzt auf genau einer Achse, und ohne **Vorher-Messung** gibt es keine belegbare Erfolgsaussage.

---

## Kurzübungen

{{ task(file="tasks/k34_01.yaml") }}

{{ task(file="tasks/k34_02.yaml") }}

{{ task(file="tasks/k34_03.yaml") }}

---

## Workshop

{{ task(file="tasks/workshop_k34.yaml") }}
