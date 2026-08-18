# Kapitel 10 – Fortgeschrittenes Prompt-Design und Agenten-Instruktionen

{{ progress(10) }}

<div class="lernziele" markdown>
<h3>Was du in diesem Kapitel lernst</h3>

- Wie du eine große Aufgabe in **Teilschritte zerlegst** und damit Qualität und Prüfbarkeit gewinnst
- Wie du das Modell **schrittweise arbeiten** lässt und eine **Selbstprüfung** anforderst, die wirklich etwas bringt
- Wie du **Ausgabeformate erzwingst** – Tabelle, feste Feldstruktur, Vorlage – damit Ergebnisse weiterverarbeitbar werden
- Was **Systemprompt**, **Custom Instructions** und **Agenten-Instruktionen** sind und wie du sie schreibst
- Wie du **wiederverwendbare Vorlagen mit Platzhaltern** baust
- Wie du Prompts **testest und versionierst**, statt sie stillschweigend zu verschlimmbessern
</div>

---

## 10.1 Zerlegen statt alles auf einmal verlangen

Kapitel 9 hat einen einzelnen Auftrag scharfgestellt. Manche Aufgaben werden dadurch trotzdem nicht gut, weil sie mehrere Denkschritte enthalten, die sich gegenseitig im Weg stehen. Ein Sprachmodell erzeugt Text von vorne nach hinten (Kap. 5) – es kann nicht am Ende merken, dass die Struktur am Anfang falsch war.

| Muster | Wann sinnvoll | Beispiel |
|---|---|---|
| **Erst sammeln, dann formulieren** | Text soll vollständig werden | erst alle Beschwerdepunkte auflisten, dann antworten |
| **Erst strukturieren, dann füllen** | längere Dokumente | erst Gliederung abstimmen, dann Abschnitte schreiben |
| **Erst prüfen, dann entscheiden** | Bewertungen, Auswahl | erst Kriterien festlegen, dann Angebote danach bewerten |
| **Erst extrahieren, dann rechnen** | Zahlen im Spiel | Werte herausziehen, außerhalb rechnen (Kap. 8) |

```mermaid
flowchart LR
    A([Schritt 1 sammeln]) --> B([Zwischenergebnis pruefen])
    B --> C([Schritt 2 strukturieren])
    C --> D([Zwischenergebnis pruefen])
    D --> E([Schritt 3 ausformulieren])
```

!!! info "Merksatz"
    Der Gewinn liegt nicht nur im besseren Endergebnis, sondern in den **Zwischenergebnissen**: Du siehst, wo es schiefgeht, und korrigierst dort – statt am Ende eine plausible, aber falsche Gesamtausgabe zu bekommen. Ein Auftrag, den du am Ergebnis nicht prüfen kannst, ist zu groß. Zerlege ihn, bis jeder Schritt einzeln prüfbar ist – dieselbe Logik, nach der du später einen Ablauf in einzelne Aktionen zerlegst (Kap. 19).

---

## 10.2 Denken lassen und Selbstprüfung anfordern

Ein Modell wird zuverlässiger, wenn es die Zwischenschritte tatsächlich **ausschreibt**, statt direkt das Ergebnis zu nennen. Der Grund liegt in der Bauweise: Was schon im Text steht, geht in die nächste Vorhersage ein. Ein vorher notierter Rechenweg oder eine notierte Prüfliste beeinflusst also die Fortsetzung.

```text
Gehe in dieser Reihenfolge vor und schreibe jeden Schritt sichtbar auf:
1. Liste alle Anforderungen auf, die im Text genannt werden.
2. Ordne jeder Anforderung zu, ob unser Angebot sie erfuellt.
3. Fasse erst danach in drei Saetzen zusammen.
```

Die zweite Technik ist die **Selbstprüfung**. Sie funktioniert nur, wenn du sie konkret machst. „Prüfe deine Antwort" bringt fast nichts – geprüft wird gegen nichts. Nützlich wird es mit einer Prüfliste:

```text
Pruefe deinen Entwurf danach gegen diese Punkte und gib eine Pruefnotiz aus:
- Steht jede Zahl so im Ausgangstext, oder wurde sie ergaenzt?
- Sind alle fuenf Gliederungspunkte enthalten?
- Wurde ein Termin oder eine Zusage genannt, die nicht vorgegeben war?
Gib danach nur die korrigierte Fassung und die Pruefnotiz aus.
```

!!! warning "Grenzen der Selbstprüfung"
    Die Prüfung läuft im selben System, das den Fehler gemacht hat. Sie findet zuverlässig **formale** Abweichungen – fehlende Abschnitte, überschrittene Länge, nicht eingehaltene Struktur. Sie findet **nicht** zuverlässig, dass eine Aussage sachlich falsch ist. Eine Selbstprüfung ersetzt deine Prüfung nicht, sie reduziert nur die Anzahl der Fälle, die dich erreichen. Wirksamer ist der **Rollenwechsel im zweiten Durchgang**: Entwurf erzeugen, dann in einem neuen Chat als kritischer Reviewer prüfen lassen (Kap. 8) – ohne den eigenen Entwurfsverlauf im Kontext fällt die Kritik deutlich schärfer aus.

---

## 10.3 Ausgabeformate erzwingen

Solange du Ergebnisse selbst liest, reicht schöner Fließtext. Sobald etwas weiterverarbeitet wird – in eine Liste, eine Tabelle, einen Ablauf – brauchst du ein **festes Format**.

**Tabelle mit definierten Spalten:**

```text
Gib das Ergebnis als Markdown-Tabelle mit genau diesen Spalten aus:
Anliegen | Kategorie | Dringlichkeit | Zustaendig | Fehlende Angabe
Erlaubt in Kategorie: Reklamation, Anfrage, Bestellung, Sonstiges.
Erlaubt in Dringlichkeit: hoch, mittel, niedrig.
Keine Erklaerung vor oder nach der Tabelle.
```

**Feste Feldstruktur** – die Grundlage dafür, dass ein Ablauf die Antwort maschinell auseinandernehmen kann (Kap. 30):

```text
Antworte ausschliesslich in diesem Format, ein Feld pro Zeile:
Kategorie: <eine von Reklamation, Anfrage, Bestellung, Sonstiges>
Dringlichkeit: <hoch, mittel oder niedrig>
Kundennummer: <Nummer aus dem Text oder UNBEKANNT>
Kurzfassung: <ein Satz, maximal 20 Woerter>
```

**Vorlage mit Leerstellen** – wenn ein Dokument einer festen Hausform folgen muss:

```text
Fuelle die Vorlage aus, aendere ihre Struktur nicht und trage
FEHLT ein, wo eine Angabe fehlt.
Betreff:
Anlass:
Sachstand:
Naechster Schritt:
```

Für die Weiterverarbeitung gilt: Je enger das Format, desto weniger Nacharbeit. Bei Freitext gibst du Länge und Ton vor, bei Tabellen Spalten und erlaubte Werte, bei Feldstrukturen zusätzlich das Verbot jeglichen Begleittextes – sonst steht vor deiner Struktur ein freundlicher Einleitungssatz, der jede maschinelle Auswertung stört.

!!! tip "Erlaubte Werte statt freier Formulierung"
    Der wirksamste Handgriff bei Formaten ist die **Werteliste**: „Verwende ausschließlich hoch, mittel, niedrig." Ohne sie bekommst du beim ersten Durchlauf „hoch", beim zweiten „sehr dringend" und beim dritten „Priorität A" – und jede nachgelagerte Auswertung bricht (Kap. 29).

---

## 10.4 Systemprompt, Custom Instructions und Agenten-Instruktionen

Bisher ging es um Aufträge für einen Einzelfall. Für wiederkehrende Arbeit setzt du Vorgaben eine Ebene höher an – sie gelten dann für **alle** Gespräche.

| Ebene | Wo eingestellt | Reichweite |
|---|---|---|
| **Systemprompt** | vom Anbieter gesetzt, für dich unsichtbar | das gesamte Produkt |
| **Custom Instructions** | in deinem Konto, etwa in ChatGPT | alle deine Chats |
| **Projekt- oder Raum-Instruktion** | im Projekt oder Arbeitsraum (Kap. 7) | alle Chats dieses Projekts |
| **Agenten-Instruktion** | im selbst gebauten Agenten, etwa Copilot Studio (Kap. 32) | jeder Aufruf des Agenten |
| **Prompt** | im Chatfenster | ein einzelner Fall |

Eine gute **Agenten-Instruktion** beantwortet fünf Fragen und ist deutlich strenger formuliert als ein Chat-Prompt, weil niemand daneben sitzt und nachbessert:

```text
Rolle: Du bist Auskunftsassistent fuer Reisekostenfragen der Beschaeftigten.
Aufgabe: Beantworte Fragen zu Reisekosten ausschliesslich auf Basis der
hinterlegten Richtlinie und nenne immer den Abschnitt als Fundstelle.
Umgangston: sachlich, kurz, in du-Form, maximal 120 Woerter.
Grenzen:
- Deckt die Richtlinie den Fall nicht ab, sage das und verweise an die Personalabteilung.
- Genehmige nichts und sage keine Erstattung zu.
- Nenne keine Betraege, die nicht in der Richtlinie stehen.
Bei Unklarheit: Stelle genau eine Rueckfrage, bevor du antwortest.
```

!!! example "Der Unterschied in einem Satz"
    Ein Prompt sagt, was **jetzt** zu tun ist. Eine Instruktion sagt, wie sich das System **immer** verhält – einschließlich der Fälle, an die du gerade nicht denkst. Deshalb ist der wichtigste Teil jeder Instruktion der Abschnitt **Grenzen**: Was tust du nicht, und was tust du bei Unsicherheit? Und weil Instruktionen dauerhaft wirken, wirken auch ihre Fehler dauerhaft – eine unglückliche Formulierung in den Custom Instructions verzerrt wochenlang jede Antwort, ohne dass du die Ursache im Chat siehst. Sieh sie regelmäßig durch und halte sie kurz.

---

## 10.5 Vorlagen mit Platzhaltern

Aus einem bewährten Prompt wird eine Vorlage, indem du die veränderlichen Stellen als **Platzhalter in eckigen Klammern** markierst. Die eckige Klammer ist bewusst gewählt: Sie fällt auf, wenn du sie zu ersetzen vergisst.

```text
Du bist [Rolle, z. B. Sachbearbeiterin im Einkauf].
Aufgabe: Formuliere [Textart, z. B. eine Absage] an [Empfaenger].
Kontext: [Sachverhalt in Stichpunkten]
Format: maximal [Anzahl] Woerter, [mit oder ohne Betreffzeile]. Ton: [Tonvorgabe].
Einschraenkungen: Nutze ausschliesslich die Angaben im Kontext.
Fehlende Angaben markierst du mit FEHLT. [weitere Einschraenkung]
```

Solche Vorlagen sind die Vorstufe zu allem, was danach kommt. Dieselbe Struktur findest du wieder, wenn ein Ablauf einen Prompt mit Werten aus einem Formular oder einer Mail befüllt (Kap. 29, Kap. 30) oder wenn du einen Agenten mit fester Instruktion in Copilot Studio baust (Kap. 32). Der Unterschied ist nur, wer die Platzhalter füllt: heute du von Hand, später der Ablauf automatisch. Willst du eine Vorlage im Team teilen, gehören drei Angaben dazu – wofür sie gedacht ist, wie ein gutes Ergebnis aussieht und worauf zu prüfen ist. Ohne sie wird die Vorlage falsch eingesetzt und dann als „funktioniert nicht" verworfen.

---

## 10.6 Prompts testen und versionieren

Prompts werden gerne „verbessert", bis sie schlechter sind als vorher – weil niemand vergleicht. Behandle einen produktiv genutzten Prompt wie ein kleines Arbeitsmittel und lege **Testfälle** fest. Drei bis fünf reichen, aber sie müssen die Bandbreite abdecken:

```text
Testfall 1 Normalfall:  vollstaendige Angaben, klarer Sachverhalt
Testfall 2 Luecke:      eine wichtige Angabe fehlt absichtlich
Testfall 3 Sonderfall:  zwei Anliegen in einem Text
Testfall 4 Grenzfall:   Anliegen, das der Prompt bewusst ablehnen soll
```

Der **Lückenfall** ist der wichtigste: Er zeigt, ob dein Prompt Fehlendes markiert oder erfindet. Der **Grenzfall** zeigt, ob die Einschränkungen halten.

**Versionieren.** Notiere zu jedem Prompt eine Version, das Datum und in einem Satz, was du geändert hast und warum. Wenn eine Änderung ein Ergebnis verschlechtert, kannst du zurück.

```text
v1  Erstfassung
v2  Einschraenkung ergaenzt: kein Liefertermin nennen
v3  Ausgabeformat auf feste Felder umgestellt, fuer Ablauf noetig
v4  Werteliste fuer Dringlichkeit ergaenzt, Freitext war zu bunt
```

!!! warning "Vor jeder Änderung an einem laufenden Prompt"
    Ändere nie mehrere Dinge gleichzeitig und teste immer gegen dieselben Testfälle. Sonst weißt du hinterher nicht, welche Änderung gewirkt hat. Diese Disziplin brauchst du spätestens, wenn ein Prompt in einem automatisierten Ablauf steckt und niemand mehr sieht, was er ausgibt (Kap. 28, Kap. 37).

Damit ist der Werkzeugkasten aus Block 2 komplett. Ab Kapitel 11 wendest du ihn in Microsoft 365 an.

---

## Zusammenfassung

- **Zerlegen** macht Aufgaben prüfbar: erst sammeln, dann strukturieren, dann formulieren – mit Kontrolle an den Zwischenständen.
- Sichtbar ausgeschriebene Zwischenschritte verbessern das Ergebnis; eine **Selbstprüfung** wirkt nur mit konkreter Prüfliste und ersetzt deine Prüfung nicht.
- **Ausgabeformate** – Tabelle, feste Feldstruktur, Vorlage – machen Ergebnisse weiterverarbeitbar; entscheidend sind vorgegebene **erlaubte Werte**.
- **Custom Instructions** und **Agenten-Instruktionen** gelten dauerhaft; ihr wichtigster Teil sind die Grenzen und das Verhalten bei Unsicherheit.
- **Vorlagen mit Platzhaltern** in eckigen Klammern sind die Vorstufe zu Prompts, die später von Abläufen befüllt werden.
- Produktive Prompts werden gegen feste **Testfälle** geprüft und **versioniert** – besonders Lücken- und Grenzfall.

---

## Kurzübungen

{{ task(file="tasks/k10_01.yaml") }}

{{ task(file="tasks/k10_02.yaml") }}

{{ task(file="tasks/k10_03.yaml") }}

---

## Workshop

{{ task(file="tasks/workshop_k10.yaml") }}
