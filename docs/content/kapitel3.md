# Kapitel 3 – Chancen, Risiken und ethische Verantwortung

{{ progress(3) }}

<div class="lernziele" markdown>
<h3>Was du in diesem Kapitel lernst</h3>

- Die drei Nutzenarten **Zeit**, **Qualität** und **Skalierung** – und woran du sie unterscheidest
- Warum ein rechnerischer Zeitgewinn in der Praxis oft **verpufft**
- Die zentralen Risiken: **Halluzination**, **Bias**, **Kontrollverlust**, **Kompetenzverlust**, **Anbieterabhängigkeit**
- Was **Automatisierungsverzerrung** ist und warum sie dich persönlich betrifft
- Ethik als Alltagspraxis: **Transparenz**, **Fairness**, **Verantwortung**, **Mensch in der Entscheidungsschleife**
- Wie du mit Sorgen im Team umgehst, ohne sie kleinzureden oder zu bestätigen
</div>

---

## 3.1 Drei Arten von Nutzen

„KI spart Zeit" ist die häufigste und die schwächste Begründung für ein Projekt. Sie ist schwach, weil sie den Nutzen auf eine Größe verkürzt, die sich schwer messen lässt und die oft gar nicht der wichtigste Effekt ist. Trenne stattdessen drei Nutzenarten:

**Zeitnutzen** entsteht, wenn eine Tätigkeit schneller erledigt ist: Ein Antwortentwurf steht in zwanzig Sekunden statt in zehn Minuten. Zeitnutzen ist der offensichtlichste und der unzuverlässigste – er verschwindet, sobald die Prüfung des Ergebnisses so lange dauert wie die ursprüngliche Arbeit.

**Qualitätsnutzen** entsteht, wenn das Ergebnis besser oder gleichmäßiger wird: Alle Angebotstexte folgen derselben Struktur, keine Reklamation bleibt unbeantwortet, jede Besprechung hat ein Protokoll. Dieser Nutzen ist häufig der wertvollste, weil er nicht von der Tagesform abhängt.

**Skalierungsnutzen** entsteht, wenn etwas überhaupt erst möglich wird: Alle 400 offenen Rückmeldungen werden ausgewertet, nicht nur die letzten 30. Vorher hätte niemand die Zeit gehabt – es wäre gar nicht gemacht worden.

| Nutzenart | Woran du sie erkennst | Belegbar durch | Typische Täuschung |
|---|---|---|---|
| Zeit | dieselbe Aufgabe geht schneller | Vorher-Nachher-Messung an echten Fällen | Prüfzeit wird nicht mitgerechnet |
| Qualität | Ergebnisse werden gleichmäßiger und vollständiger | Fehler- und Rückfragequote | „fühlt sich besser an" ohne Kennzahl |
| Skalierung | etwas wird gemacht, was vorher entfiel | Menge der bearbeiteten Fälle | Menge wird mit Wert verwechselt |

!!! info "Merksatz"
    Der belastbarste Nutzen ist selten der Zeitgewinn. Er liegt meist in **Gleichmäßigkeit** und in **Dingen, die vorher liegen blieben**. Wer nur mit Zeitersparnis argumentiert, verliert die Diskussion beim ersten kritischen Nachrechnen.

---

## 3.2 Wann der Nutzen verpufft

Rechne den Nutzen immer über den **gesamten** Weg, nicht über den KI-Schritt. Ein Beispiel, das in dieser Form ständig vorkommt:

!!! example "Die Rechnung, die nicht aufgeht"
    Eine Sachbearbeiterin beantwortet täglich 20 Kundenanfragen, je 8 Minuten – 160 Minuten. Mit KI-Entwurf braucht sie pro Antwort noch 3 Minuten – 60 Minuten. Ersparnis auf dem Papier: 100 Minuten pro Tag.

    Jetzt der vollständige Weg:

    ```text
    Entwurf lesen und pruefen           20 x 2 Minuten  = 40 Minuten
    Bei 4 von 20 Faellen nachschaerfen   4 x 4 Minuten  = 16 Minuten
    Bei 2 Faellen Entwurf verwerfen und
    von Hand schreiben                   2 x 8 Minuten  = 16 Minuten
    Ergebnis: 60 + 40 + 16 + 16          = 132 Minuten
    ```

    Die echte Ersparnis liegt bei 28 Minuten, nicht bei 100. Das ist immer noch ein guter Wert – aber ein völlig anderes Versprechen. Und wenn die Fälle komplexer sind, kippt die Rechnung ins Negative.

Drei Ursachen lassen den Nutzen regelmäßig verschwinden:

- **Prüfaufwand:** Ein Ergebnis, dem man nicht trauen kann, muss vollständig gelesen werden. Bei kurzen Texten ist Prüfen fast so teuer wie Schreiben.
- **Verlagerter Aufwand:** Was in der Fachabteilung eingespart wird, fällt bei der IT als Wartung oder bei der Führungskraft als Freigabe wieder an.
- **Nacharbeit:** Ein plausibel klingendes, aber falsches Ergebnis kostet mehr als ein leeres Blatt, weil der Fehler erst gefunden werden muss.

Der ehrliche Nutzentest ist deshalb einfach: Miss an **zehn echten Fällen** aus deinem Alltag, inklusive Prüfen und Nacharbeit, und vergleiche mit der bisherigen Bearbeitung. Zehn Fälle genügen für eine belastbare Größenordnung – und sie schützen vor der Begeisterung, die nach dem einen gelungenen Beispiel entsteht. Die systematische Bewertung von Anwendungsfällen vertiefst du in Kapitel 34.

---

## 3.3 Die fünf großen Risiken

| Risiko | Was passiert | Wo es besonders wehtut | Erste Gegenmaßnahme |
|---|---|---|---|
| Halluzination | erfundene Inhalte, überzeugend formuliert | Zahlen, Zitate, Paragrafen, interne Regeln | Quellenpflicht, Nachprüfen, keine Fakten aus dem Modell |
| Bias | systematische Benachteiligung von Gruppen | Bewerbung, Bewertung, Auswahl, Kreditwürdigkeit | keine Personalentscheidung durch KI, Ergebnisse gruppenweise prüfen |
| Kontrollverlust | niemand weiß, warum etwas passiert ist | mehrstufige Agenten ohne Protokoll | Protokollierung, Freigabepunkte, Abschaltbarkeit |
| Kompetenzverlust | Fähigkeiten verkümmern durch Nichtgebrauch | Formulieren, Rechnen, Fachprüfung | bewusst Aufgaben ohne KI erledigen, Vier-Augen-Prinzip |
| Anbieterabhängigkeit | Preise, Funktionen, Verfügbarkeit ändern sich fremdbestimmt | Prozesse, die ohne das Werkzeug stillstehen | Rückfallebene, Export von Prompts und Daten |

**Halluzination** ist keine Fehlfunktion, sondern die Kehrseite der Funktionsweise: Ein Sprachmodell erzeugt wahrscheinliche Fortsetzungen, nicht geprüfte Wahrheiten (Details in Kapitel 5). Deshalb ist die Fehlerform so gefährlich – sie kommt nie als Fehlermeldung, sondern immer als flüssiger, selbstsicherer Satz.

**Bias**, auf Deutsch **Verzerrung**, bezeichnet eine systematische Schieflage im Ergebnis. Sie entsteht, weil Trainingsdaten die Welt so abbilden, wie sie war, samt aller Ungleichheiten. Ein System, das aus vergangenen Einstellungsentscheidungen lernt, lernt auch die Muster mit, die dort falsch waren. Genau deshalb behandelt der EU AI Act die Auswahl von Beschäftigten als Hochrisiko-Anwendung (Kapitel 4).

**Kontrollverlust** ist das Risiko, das mit der Autonomie wächst. Bei einem regelbasierten Ablauf kannst du jeden Schritt nachlesen. Bei einem Agenten, der seinen Weg selbst wählt, brauchst du eine Protokollierung, die festhält, **was** er getan hat und **womit** – sonst ist eine Fehlersuche unmöglich (Kapitel 33).

**Kompetenzverlust** wird meist unterschätzt, weil er langsam eintritt. Wer zwei Jahre lang keinen Text mehr selbst formuliert, verliert die Fähigkeit, einen schlechten Entwurf als schlecht zu erkennen. Damit fällt genau die Kontrolle weg, auf der die gesamte Absicherung beruht.

**Anbieterabhängigkeit** entsteht leise. Ein Prozess, der auf einem einzigen Werkzeug aufsetzt, ist dessen Preis- und Funktionsentscheidungen ausgeliefert. Die Frage lautet nicht „was, wenn der Anbieter verschwindet", sondern „was passiert am Montag, wenn das Werkzeug drei Stunden nicht erreichbar ist".

!!! warning "Typische Falle"
    Die Risiken treten nicht einzeln auf, sondern verstärken sich. Eine halluzinierte Zahl in einem Bericht, die niemand prüft, weil man dem Werkzeug inzwischen vertraut, und die in einem Ablauf ohne Protokoll entstanden ist – das ist keine Verkettung unglücklicher Umstände, sondern der Normalfall bei fehlenden Kontrollpunkten.

---

## 3.4 Automatisierungsverzerrung: das Risiko, das dich betrifft

**Automatisierungsverzerrung** beschreibt die menschliche Neigung, Vorschlägen einer Maschine mehr zu vertrauen als eigenen Beobachtungen. Sie ist gut untersucht und tritt umso stärker auf, je zuverlässiger das System in der Vergangenheit war.

Das Muster ist immer dasselbe: In den ersten Wochen wird jedes Ergebnis genau geprüft. Weil fast alles stimmt, sinkt die Prüftiefe. Nach einigen Monaten ist die Freigabe ein Klick. Der Kontrollpunkt existiert dann nur noch auf dem Papier – und genau in dieser Phase richtet der erste unbemerkte Fehler den größten Schaden an.

```mermaid
flowchart LR
    A([Neues Werkzeug]) --> B([Gruendliche Pruefung])
    B --> C([Ergebnisse stimmen meist])
    C --> D([Pruefung wird oberflaechlich])
    D --> E([Fehler faellt nicht mehr auf])
```

Gegen diese Verzerrung hilft kein guter Vorsatz, sondern nur ein Aufbau, der Prüfung erzwingt:

- **Stichproben mit festem Anteil:** Zum Beispiel jeder zehnte Fall wird vollständig gegengelesen, unabhängig davon, wie gut es gerade läuft.
- **Prüfung an der richtigen Stelle:** Nicht „ist der Text schön", sondern „stimmen Betrag, Datum, Name und Zusage".
- **Sichtbare Unsicherheit:** Wenn ein KI-Schritt einen Konfidenzwert liefert, gehören Fälle unterhalb einer Schwelle in eine Prüfschleife statt in den Automatikpfad (Kapitel 31).
- **Fehler zählen:** Wer nicht mitschreibt, wie oft nachgearbeitet werden musste, merkt eine Verschlechterung des Werkzeugs nicht.

!!! warning "Häufiges Missverständnis"
    „Der Mensch prüft ja" ist keine Schutzmaßnahme, solange nicht festgelegt ist, **was** geprüft wird, **wie oft** und **wer** es verantwortet. Ein Freigabeknopf ohne Prüfauftrag erzeugt nur die Illusion von Kontrolle – und verschiebt die Verantwortung auf jemanden, der sie nicht wahrnehmen kann.

---

## 3.5 Ethik praktisch: vier Pflichten im Alltag

Ethik wird in diesem Kurs nicht philosophisch behandelt. Es geht um vier Fragen, die vor jedem Einsatz beantwortbar sein müssen.

**Transparenz.** Wissen die Betroffenen, dass KI beteiligt ist? Bei Kunden ist die Antwort im Zweifel Ja – nicht weil jede E-Mail einen Hinweis braucht, sondern weil verdeckter Einsatz Vertrauen zerstört, sobald er auffällt. Konkret heißt das: Ein Chatbot auf der Website muss sich als solcher erkennbar machen. Ein von KI entworfener, von einem Menschen geprüfter und verantworteter Brief muss es nicht. Die rechtlichen Transparenzpflichten dazu findest du in Kapitel 4.

**Fairness.** Werden Menschen durch den Einsatz systematisch unterschiedlich behandelt? Diese Frage stellt sich überall, wo über Personen entschieden wird – Bewerbung, Beurteilung, Mahnung, Kulanz. Prüfe nicht Einzelfälle, sondern Muster: Fallen bestimmte Gruppen häufiger durch?

**Verantwortung.** Wer haftet für das Ergebnis? Die Antwort ist immer ein Mensch mit Namen, niemals „das System". Ein Ablauf ohne benannte Verantwortliche ist nicht fertig, sondern unfertig (Kapitel 38).

**Mensch in der Entscheidungsschleife.** Auf Englisch **Human in the Loop** – das Prinzip, dass ein Mensch vor der wirksamen Entscheidung eingreifen kann. Wichtig ist die Unterscheidung dreier Aufbauten:

| Aufbau | Bedeutung | Geeignet für |
|---|---|---|
| Mensch entscheidet, KI schlägt vor | nichts wird ohne Zustimmung wirksam | Personalfragen, Kundenzusagen, Geld, Recht |
| Mensch prüft nachträglich | wirkt sofort, wird stichprobenweise kontrolliert | Ablage, Kategorisierung, interne Hinweise |
| Kein Mensch beteiligt | vollautomatisch | nur bei harmlosen, umkehrbaren Schritten |

!!! info "Die Frage, die alles entscheidet"
    Nicht „darf KI das", sondern: **Was passiert, wenn das Ergebnis falsch ist – wie schnell fällt es auf und wie teuer ist die Korrektur?** Ist der Fehler sichtbar und reversibel, darf mehr automatisch laufen. Ist er unsichtbar oder unumkehrbar, entscheidet ein Mensch vorher.

---

## 3.6 Sorgen im Team ernst nehmen

Wenn du in deinem Bereich Automatisierung einführst, wirst du auf Widerstand treffen. Er ist meistens kein Missverständnis, sondern eine berechtigte Frage in ungünstiger Formulierung.

| Was gesagt wird | Was gemeint ist | Was hilft |
|---|---|---|
| „Das ersetzt doch nur unsere Arbeit." | Wird meine Stelle noch gebraucht? | benennen, welche Tätigkeit entfällt und welche bleibt; nicht pauschal beruhigen |
| „Bei uns ist jeder Fall ein Sonderfall." | Meine Erfahrung wird nicht anerkannt. | Häufigkeiten gemeinsam auszählen; Sonderfälle bewusst ausnehmen |
| „Ich soll das jetzt auch noch kontrollieren." | Mehr Verantwortung, keine Entlastung. | Prüfumfang begrenzen und Zeit dafür einplanen |
| „Und wenn das Ding Mist macht, bin ich schuld." | Haftungsfrage ist ungeklärt. | Verantwortlichkeiten schriftlich festhalten |

Drei Dinge tragen mehr als jede Präsentation: **Ehrlichkeit** darüber, was sich ändert; **Beteiligung** der Menschen, die die Aufgabe heute machen, weil sie die Sonderfälle kennen; und ein **kleiner Anfang** an einer unstrittigen Aufgabe statt eines Prestigeprojekts.

Ein Versprechen solltest du dabei nie geben: „Es wird sich nichts ändern." Das ist nicht wahr, und der Vertrauensverlust bei der ersten Änderung ist größer als der Gewinn durch die Beruhigung. Sage stattdessen konkret, welche Tätigkeit wegfällt, welche neu dazukommt und wer die Freigabe behält.

---

## Zusammenfassung

- Nutzen zerfällt in **Zeit**, **Qualität** und **Skalierung**; die belastbaren Argumente liegen meist bei den letzten beiden.
- Rechne Nutzen über den **gesamten** Weg inklusive **Prüfaufwand** und **Nacharbeit** – an zehn echten Fällen, nicht an einem Vorzeigebeispiel.
- Die fünf großen Risiken sind **Halluzination**, **Bias**, **Kontrollverlust**, **Kompetenzverlust** und **Anbieterabhängigkeit**; sie verstärken sich gegenseitig.
- **Automatisierungsverzerrung** entwertet Kontrollpunkte im Laufe der Zeit – dagegen helfen nur feste Stichproben, klare Prüfpunkte und gezählte Fehler.
- Ethik im Alltag heißt **Transparenz**, **Fairness**, benannte **Verantwortung** und ein passend gewählter **Mensch-in-der-Schleife**-Aufbau.
- Sorgen im Team sind Sachfragen: ehrlich benennen, was sich ändert, Betroffene beteiligen, klein anfangen.

---

## Kurzübungen

{{ task(file="tasks/k03_01.yaml") }}

{{ task(file="tasks/k03_02.yaml") }}

{{ task(file="tasks/k03_03.yaml") }}

---

## Workshop

{{ task(file="tasks/workshop_k03.yaml") }}
