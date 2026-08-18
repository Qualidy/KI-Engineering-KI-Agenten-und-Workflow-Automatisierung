# Kapitel 37 – Umsetzung: bauen, iterieren, testen

{{ progress(37) }}

<div class="lernziele" markdown>
<h3>Was du in diesem Kapitel lernst</h3>

- Warum du zuerst ein **Skelett** baust, das durchläuft, und erst danach die Fachlogik ergänzt
- Wie du **Testdaten** anlegst, statt an Echtdaten zu üben
- Wie **Kurzzyklen** aus Bauen, Testen und Korrigieren funktionieren – und wie lang ein Zyklus sein darf
- Wie du Fehler **systematisch eingrenzt**, statt zu raten
- Wie du den **KI-Anteil kalibrierst**: Prompt schärfen, Ausgabeformat erzwingen, Schwellenwerte setzen, Stichproben auswerten
- Wie du die **Abnahme** gegen deine Testfälle durchführst und mit Blockern umgehst, ohne das Projekt zu verlieren
</div>

---

## 37.1 Erst das Skelett, dann der Ausbau

Der häufigste Fehler in der Bauphase: den Flow von vorne nach hinten fertig konfigurieren – Trigger, dann Schritt eins vollständig, dann Schritt zwei vollständig. Nach zwei Stunden gibt es viele perfekte Einstellungen und keinen einzigen erfolgreichen Durchlauf.

Der bessere Weg ist ein **Skelett**: Trigger, alle geplanten Aktionen in der richtigen Reihenfolge, aber mit festen Platzhalterwerten statt echter Logik. Kein KI-Aufruf, keine Bedingungen, keine Fehlerbehandlung. Ziel ist ein einziger Durchlauf, der bis zum letzten Schritt kommt.

```mermaid
flowchart LR
    A([Skelett laeuft durch]) --> B([Datenfluss echt machen])
    B --> C([KI-Stelle einbauen])
    C --> D([Verzweigung und Freigabe])
    D --> E([Fehlerpfade ergaenzen])
```

Diese Reihenfolge ist kein Geschmacksurteil. Sie sorgt dafür, dass du jeden neuen Fehler **einem** Schritt zuordnen kannst, weil vorher alles lief.

!!! info "Merksatz"
    Ein Flow, der falsche Ergebnisse liefert, ist weiter als ein Flow, der nicht startet. Sorge zuerst dafür, dass Daten fließen – Richtigkeit kommt im zweiten Schritt.

!!! tip "Speichern und Namen geben"
    Benenne jede Aktion sofort sprechend („Rechnungsdaten auslesen" statt „Text mit GPT erstellen 2") und speichere nach jedem funktionierenden Zwischenstand. Wenn ein Werkzeug Versionen oder Kopien erlaubt, lege vor jedem größeren Umbau eine Kopie an. Das ersetzt kein Versionsmanagement (Kap. 28), rettet aber Nachmittage.

---

## 37.2 Testdaten statt Echtdaten

Mit Echtdaten zu üben ist bequem und aus drei Gründen falsch: Du erzeugst echte Nebenwirkungen (Mails an echte Kunden, Einträge in produktive Listen), du verarbeitest personenbezogene Daten ohne Notwendigkeit (Kap. 4), und du hast trotzdem nicht die Fälle, die du brauchst – Grenzfälle kommen im Alltag selten und kaum auf Zuruf.

| Bestandteil einer Testumgebung | Konkret |
|---|---|
| **Eigene Zielablage** | SharePoint-Liste oder Excel-Datei mit dem Zusatz „Test" im Namen |
| **Eigener Eingangskanal** | Testpostfach, eigenes Formular oder ein Ordner, in den du Dateien legst |
| **Testdatensatz** | 8 bis 15 vorbereitete Fälle, abgeleitet aus deinen Testfällen (Kap. 36) |
| **Eigene Empfänger** | alle Benachrichtigungen zunächst an dich selbst |
| **Erkennbare Markierung** | Betreff und Einträge beginnen mit TEST, damit nichts verwechselt wird |

Wenn du Echtdaten als Vorlage brauchst, anonymisiere sie: Namen ersetzen, Mailadressen auf deine eigene ändern, Beträge und Nummern verfremden. Behalte die **Struktur** bei – gerade die schrägen Formate sind der Grund, warum echte Fälle lehrreich sind.

!!! warning "Typische Falle"
    Benachrichtigungsschritte, die beim Testen noch an echte Empfänger gehen, sind der klassische peinliche Moment: dreißig Testmails an eine Führungskraft, weil ein Schleifenlauf falsch konfiguriert war. Setze **jeden** Empfänger auf dich selbst und tausche sie erst beim Rollout aus (Kap. 38).

---

## 37.3 Kurzzyklen: bauen, testen, korrigieren

Arbeite in Zyklen von **einer Änderung**. Ein Zyklus besteht aus: eine Sache ändern, speichern, mit einem Testfall ausführen, Ergebnis im Ausführungsverlauf ansehen, Ergebnis notieren. Wer drei Änderungen gleichzeitig macht und dann testet, weiß bei einem Fehler nicht, welche davon schuld ist.

Halte die Ergebnisse mit: ein knappes Protokoll reicht, aber es muss existieren. Es ist später die Grundlage für den Testnachweis in der Dokumentation (Kap. 39).

```text
BAU- UND TESTPROTOKOLL

| Datum/Zeit | Geaenderter Schritt | Testfall Nr | Ergebnis | Naechster Schritt |
|------------|---------------------|-------------|----------|-------------------|
| [Zeit] | [Aktion oder Prompt] | [Nr] | ok / Fehler: [Meldung] | [was du als naechstes aenderst] |
| [Zeit] | [Aktion oder Prompt] | [Nr] | ok / Fehler: [Meldung] | [Aenderung] |

Offene Punkte:
  - [Punkt] , entdeckt am [Datum] , Prioritaet: [hoch / mittel / niedrig]
Geparkt fuer spaeter:
  - [Punkt] , weil [Grund]
```

!!! example "Ein Zyklus in der Praxis"
    Ausgangslage: Das Skelett läuft, der Listeneintrag wird erzeugt, aber das Datumsfeld bleibt leer.

    1. **Eine Änderung:** in der Aktion „Element erstellen" das Datumsfeld mit dem Wert aus dem Trigger belegen.
    2. **Ausführen** mit Testfall 1.
    3. **Verlauf ansehen:** Fehler „Die Zeichenfolge ist kein gültiges Datum".
    4. **Notieren** und daraus eine Hypothese bilden: Das Formular liefert das Datum als Text im Format Tag.Monat.Jahr.
    5. **Nächster Zyklus:** eine `Verfassen`-Aktion vor dem Eintrag, die das Datum umformt (Kap. 23), erneut ausführen.

    Zwei Zyklen, zwei Minuten, ein gelöstes Problem. Wer stattdessen gleich Datumsformat, Zeitzone und Feldzuordnung zusammen ändert, sucht danach eine halbe Stunde.

---

## 37.4 Fehler systematisch eingrenzen

Fehlersuche ist ein Verfahren, kein Talent. Vier Techniken genügen für fast alles, was in einem Flow schiefgeht (Grundlagen zum Ausführungsverlauf in Kap. 28).

| Technik | Vorgehen | Wofür |
|---|---|---|
| **Halbieren** | die hintere Hälfte des Flows abschalten und prüfen, ob die vordere sauber liefert | wenn unklar ist, wo der Fehler entsteht |
| **Isolieren** | den verdächtigen Schritt in einem separaten Testflow allein ausführen | bei KI-Aufrufen, HTTP-Aktionen, Ausdrücken |
| **Mitschreiben** | Zwischenergebnisse über `Verfassen` sichtbar machen und im Verlauf lesen | wenn Daten anders aussehen als erwartet |
| **Vereinfachen** | den Ausdruck auf den einfachsten Fall reduzieren und dann stückweise aufbauen | bei verschachtelten Ausdrücken und Formaten |

Und eine Frage vorweg, die viel Zeit spart: **Hat es vorher funktioniert?** Wenn ja, lautet die erste Hypothese immer „meine letzte Änderung" – nicht „das Werkzeug hat ein Problem".

!!! warning "Häufiges Missverständnis"
    Eine Fehlermeldung, die nach Technik klingt, wird oft ungelesen als „unlösbar" abgetan. Tatsächlich stehen im Ausführungsverlauf meist die Eingabe und die Ausgabe jedes Schrittes im Klartext. Lies zuerst die **Eingabe** des fehlgeschlagenen Schrittes: In den meisten Fällen ist dort schon sichtbar, dass ein Wert leer, doppelt oder im falschen Format ankommt.

---

## 37.5 Den KI-Anteil kalibrieren

Der KI-Baustein ist der Teil deines Flows, der nicht durch Konfiguration richtig wird, sondern durch **Kalibrierung**. Vier Hebel, in dieser Reihenfolge.

**Erstens: Ausgabeformat erzwingen.** Ein Flow kann keinen Fließtext weiterverarbeiten. Verlange genau die Felder, die du brauchst, in einer festen Reihenfolge, ohne Einleitung und ohne Erklärung.

```text
Du ordnest eingehende Anfragen einer Kategorie zu.
Erlaubte Kategorien: Vertrieb, Support, Rechnung, Sonstiges.
Antworte in genau drei Zeilen, ohne Einleitung:
Kategorie: [eine der erlaubten Kategorien]
Dringlichkeit: hoch oder normal
Zusammenfassung: [maximal 20 Woerter]
Wenn der Text keine Zuordnung erlaubt, schreibe als Kategorie: Unklar.
Text der Anfrage:
[Text]
```

**Zweitens: Prompt schärfen.** Ergänze eine erlaubte Werteliste, ein Beispiel für den schwierigen Fall und eine ausdrückliche Erlaubnis, „Unklar" zu antworten. Diese Erlaubnis ist wichtiger, als sie klingt: Ein Modell, das keine Ausweichmöglichkeit hat, erfindet lieber eine Kategorie (Kap. 5).

**Drittens: Schwellenwerte anpassen.** Wo dein Baustein einen Konfidenzwert liefert (etwa bei Dokumentenverarbeitung, Kap. 31), legst du fest, ab wann automatisch weitergearbeitet wird und ab wann ein Mensch prüft. Beginne streng und lockere erst, wenn deine Stichprobe es rechtfertigt.

**Viertens: Stichproben auswerten.** Lass 10 bis 20 Fälle durchlaufen und zähle nach.

| Größe | Wie du sie erhebst | Was sie dir sagt |
|---|---|---|
| **Trefferquote** | Anzahl fachlich korrekter Ergebnisse geteilt durch Anzahl Fälle | ob der Baustein gut genug für den Zweck ist |
| **Formattreue** | Anzahl Antworten im verlangten Format geteilt durch Anzahl Fälle | ob der Flow die Antwort verlässlich weiterverarbeiten kann |
| **Fehlerrichtung** | bei welchen Fällen falsch geurteilt wird | ob eine Kategorie fehlt oder ein Prompt-Teil unklar ist |
| **Unklar-Anteil** | Anzahl Ausweichantworten | ob die Schwelle zu streng oder der Prompt zu eng ist |

!!! info "Die Fehlerrichtung ist wichtiger als die Quote"
    17 von 20 richtig klingt gleich, ist es aber nicht. Werden nur harmlose Fälle falsch einsortiert und gehen alle kritischen zur Prüfung, ist das ein brauchbarer Zustand. Wenn umgekehrt die drei Fehler genau die dringenden Fälle betreffen, ist der Baustein trotz 85 Prozent nicht einsetzbar. Schau immer nachrechnend hin, **welche** Fälle danebengehen.

---

## 37.6 Abnahme, Blocker und Zeitmanagement

Die **Abnahme** ist ein einzelner, sauber durchgeführter Durchgang: alle Testfälle aus Kapitel 36 der Reihe nach ausführen, Ergebnis eintragen, keine Änderung während des Durchgangs. Änderst du zwischendrin, beginnt der Durchgang neu – sonst ist der Nachweis wertlos.

Ein Testfall gilt als bestanden, wenn das erwartete Ergebnis eintritt, nicht wenn „ungefähr das Richtige" passiert. Nicht bestandene Fälle bekommen einen Eintrag: Ursache, Entscheidung (korrigieren oder als Grenze dokumentieren), und bei Bedarf einen Vermerk in den bekannten Grenzen (Kap. 39).

| Blocker | Reduktion statt Aufgeben |
|---|---|
| Connector oder Rechte fehlen | Zwischenschritt über Liste oder Datei, manueller Import einmal täglich |
| KI-Ergebnisse zu unzuverlässig | Aufgabe verkleinern: statt vier Kategorien nur „dringend / nicht dringend", Rest zur Prüfung |
| Ein Schritt ist technisch zu aufwendig | diesen Schritt als manuelle Aufgabe im Flow anlegen, statt ihn zu automatisieren |
| Zeit reicht nicht für alle Zweige | einen Zweig als Nicht-Ziel dokumentieren und den Hauptzweig fertig machen |
| Fehler bleibt ungelöst | Fehlerpfad ergänzen, der den Fall sichtbar an einen Menschen gibt, und Ursache als offenen Punkt dokumentieren |

!!! tip "Das Zeitbudget schützen"
    Setze dir eine feste Grenze pro Problem – etwa 45 Minuten. Ist es dann nicht gelöst, wandert es in die Liste „geparkt" und du machst mit dem nächsten Meilenstein weiter. Ein fertiger, dokumentierter Flow mit zwei bekannten Grenzen ist ein Ergebnis. Ein nicht laufender Flow mit einem perfekt gelösten Detailproblem ist keines.

!!! warning "Der Reflex, kurz vor Schluss noch etwas einzubauen"
    Wenn der Flow läuft, wirkt jede kleine Ergänzung verlockend. Genau in dieser Phase entstehen die Fehler, die niemand mehr findet, weil danach nicht mehr getestet wird. Regel: **Nach der Abnahme keine Änderung ohne erneuten Testdurchgang.** Zusatzwünsche gehören in die nächsten Schritte (Kap. 40).

---

## Zusammenfassung

- Erst ein **Skelett**, das durchläuft, dann Datenfluss, KI-Stelle, Verzweigungen und Fehlerpfade.
- **Testdaten und Testablagen** statt Echtdaten – alle Benachrichtigungen zunächst an dich selbst.
- **Kurzzyklen** mit genau einer Änderung pro Zyklus, mitgeschrieben im Bau- und Testprotokoll.
- Fehler eingrenzen durch **Halbieren, Isolieren, Mitschreiben, Vereinfachen** – und immer zuerst die Eingabe des fehlgeschlagenen Schrittes lesen.
- Den KI-Anteil **kalibrieren**: Format erzwingen, Prompt schärfen, Schwellenwerte setzen, Stichprobe auswerten und dabei auf die Fehlerrichtung achten.
- **Abnahme** in einem Durchgang ohne Änderungen; bei Blockern den Umfang reduzieren, statt das Projekt zu verlieren.

---

## Kurzübungen

{{ task(file="tasks/k37_01.yaml") }}

{{ task(file="tasks/k37_02.yaml") }}

{{ task(file="tasks/k37_03.yaml") }}

---

## Workshop

{{ task(file="tasks/workshop_k37.yaml") }}
