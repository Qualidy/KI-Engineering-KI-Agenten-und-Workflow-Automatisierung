# Kapitel 13 – KI-gestützte Datenarbeit in Excel und auf dem Weg zu Power BI

{{ progress(13) }}

<div class="lernziele" markdown>
<h3>Was du in diesem Kapitel lernst</h3>

- Warum **Datenqualität** die Voraussetzung und nicht das Ergebnis der KI-Nutzung ist
- Wie du **Formeln erklären und erzeugen** lässt, ohne dich blind darauf zu verlassen
- Wie du **abgeleitete Spalten** und Kategorien aus vorhandenen Daten gewinnst
- Wie du **Auffälligkeiten** in Daten suchst und echte Sonderfälle von Fehlern trennst
- Wie du unstrukturierten **Text in Tabellenform** bringst
- Wo die harte Grenze beim **exakten Rechnen** liegt, wie die **Gegenprobe** aussieht und wann der Wechsel zu **Power BI** sinnvoll wird
</div>

---

## 13.1 Ohne saubere Tabelle passiert nichts

Copilot in Excel erreichst du über `Excel → Registerkarte Start → Copilot`. Der häufigste erste Satz lautet dort: „Um zu beginnen, formatiere den Bereich als Tabelle." Das ist keine Schikane, sondern die Bedingung dafür, dass überhaupt sinnvoll gearbeitet werden kann. Der Klickpfad zum Umbau lautet `Excel → Bereich markieren → Registerkarte Einfügen → Tabelle`, den Namen vergibst du über `Registerkarte Tabellenentwurf → Tabellenname`. Verwertbar ist eine Tabelle, wenn sie fünf Bedingungen erfüllt:

| Bedingung | Warum sie zählt | Häufiger Verstoß |
|---|---|---|
| Genau eine Kopfzeile mit sprechenden Überschriften | Ohne Namen kein Bezug auf Spalten | Überschrift über zwei Zeilen verteilt |
| Ein Datensatz pro Zeile, keine Zwischensummen | Zwischensummen werden mitgezählt | Summenzeile mitten in den Daten |
| Keine verbundenen Zellen | Zellverbund zerstört den Spaltenbezug | Optisch schöne Kopfbereiche |
| Einheitliche Datentypen je Spalte | Text in Zahlenspalten blockiert Rechnen | `1.250 EUR` statt `1250` |
| Keine leeren Zeilen oder Spalten | Bereich bricht dort ab | Leerspalte zur Optik |

!!! warning "Typische Falle: Zahlen, die keine Zahlen sind"
    Der teuerste Datenqualitätsfehler ist die Zahl im Textformat. Sie sieht in der Zelle korrekt aus, wird aber links ausgerichtet und beim Rechnen ignoriert. Eine Summe über 200 Zeilen kann dadurch stillschweigend um Zehntausende zu niedrig ausfallen. Prüfe verdächtige Spalten mit einer Formel wie `=ANZAHL(Auftraege2026[Betrag])` und vergleiche das Ergebnis mit der Anzahl der Zeilen. Weichen die Werte ab, stehen dort Texte.

```mermaid
flowchart LR
    A([Rohdaten im Blatt]) --> B([Als Tabelle formatieren])
    B --> C([Datentypen pruefen])
    C --> D([Copilot beauftragen])
    D --> E([Ergebnis gegenpruefen])
```

---

## 13.2 Formeln erklären und erzeugen lassen

Zwei Richtungen sind im Alltag wertvoll – und die erste wird meist unterschätzt: **eine geerbte Formel verstehen.** Du übernimmst eine Datei und findest darin ein Ungeheuer. Statt zu raten, lässt du es erklären:

```text
Rolle: Du bist Excel-Trainerin und erklaerst fuer Menschen ohne Formelkenntnis.
Aufgabe: Erklaere die folgende Formel Schritt fuer Schritt.
Formel:
=WENNFEHLER(INDEX(Preise[Netto];VERGLEICH([@Artikelnummer];Preise[Artikelnummer];0));"kein Preis")
Format: Erst was die Formel liefert, dann eine Tabelle mit den Spalten
Formelteil und Bedeutung, dann zwei Faelle mit unerwartetem Ergebnis.
Einschraenkung: Keine Verbesserungsvorschlaege, nur Erklaerung.
```

Die zweite Richtung ist das **Erzeugen**. Hier entscheidet die Genauigkeit der Beschreibung über das Ergebnis: Nenne Spaltennamen, Sonderfälle und das gewünschte Verhalten bei fehlenden Werten.

```text
Aufgabe: Erzeuge eine Excel-Formel fuer eine neue Spalte Lieferstatus in der
Tabelle Auftraege2026.
Vorhandene Spalten: Auftragsnummer, Kunde, Zusagedatum, Lieferdatum, Menge
Regeln:
- Lieferdatum leer und Zusagedatum in der Vergangenheit: ueberfaellig
- Lieferdatum leer und Zusagedatum in der Zukunft: offen
- Lieferdatum kleiner oder gleich Zusagedatum: puenktlich
- Lieferdatum groesser als Zusagedatum: verspaetet
Format: Nur die Formel, danach eine Zeile Erklaerung je Regel.
Einschraenkung: Nutze strukturierte Verweise mit Spaltennamen, keine
Zellbezuege wie C2.
```

!!! tip "Die Drei-Fälle-Gegenprobe"
    Prüfe jede erzeugte Formel an drei selbst gewählten Zeilen: einer klar normalen, einer Grenzfallzeile (Lieferdatum genau gleich Zusagedatum) und einer Zeile mit leeren Werten. Wenn alle drei stimmen, ist die Formel meistens brauchbar. Diese Prüfung dauert zwei Minuten und ersetzt jedes Vertrauen.

---

## 13.3 Abgeleitete Spalten und Kategorien

Viele Auswertungen scheitern nicht am Rechnen, sondern daran, dass die passende Spalte fehlt: Region zur Postleitzahl, Größenklasse zur Menge, Kategorie zum Freitext. Genau hier ist KI stark, weil sie auch **unscharfe** Zuordnungen bewältigt. Ein kleiner, erfundener Datensatz als Arbeitsgrundlage:

```text
Auftragsnummer;Kunde;PLZ;Artikelbezeichnung;Menge;Netto
A-1001;Musterhandel GmbH;40213;Metallregal 200x100 verzinkt;12;1044,00
A-1002;Nordlicht Bueromoebel;20095;Rollcontainer 3 Schuebe weiss;40;2360,00
A-1003;Musterhandel GmbH;40213;Sonderanfertigung Ablage Eiche;2;890,00
A-1004;Talwerk Montage;04109;Sonderanfertigung Werkbank Buche;1;1420,00
```

```text
Aufgabe: Ergaenze fuer den folgenden Datensatz drei abgeleitete Spalten.
Spalte Warengruppe: Metallregal, Rollcontainer oder Sonderanfertigung,
abgeleitet aus der Artikelbezeichnung.
Spalte Region: abgeleitet aus der ersten Stelle der PLZ als PLZ-Zone 0 bis 9.
Spalte Stueckpreis: Netto geteilt durch Menge, auf zwei Stellen gerundet.
Format: Vollstaendige Tabelle im gleichen Trennzeichenformat, danach eine Liste
der unsicheren Zeilen mit Begruendung.
Einschraenkung: Keine Zeile weglassen, keine Zeile umsortieren. Wenn eine
Zuordnung nicht eindeutig ist, schreibe unklar statt zu raten.
```

Der entscheidende Teil steht in der letzten Zeile. Ohne die Anweisung `unklar statt raten` bekommst du für jede Zeile eine Zuordnung – auch für die, bei denen es keine gute gibt. Und du erkennst nicht, welche das waren.

!!! info "Merksatz zur Arbeitsteilung"
    Nutze KI für die **Zuordnungslogik** und Excel für die **Rechnung**. Lass dir die Regel „Metallregal erkennt man an der Bezeichnung" von der KI bauen und die Division `Netto / Menge` von Excel ausführen. Wer es umgekehrt macht, kombiniert die Schwäche beider Werkzeuge.

---

## 13.4 Auffälligkeiten finden – und richtig einordnen

Copilot in Excel kann eine Tabelle durchsehen und benennen, was aus dem Rahmen fällt. Der Nutzen liegt im Finden, nicht im Bewerten.

```text
Aufgabe: Untersuche die Tabelle Auftraege2026 auf Auffaelligkeiten.
Format: Tabelle mit den Spalten Auffaelligkeit, betroffene Zeilen, moegliche
Ursache, empfohlene Pruefung.
Pruefe mindestens:
- Fehlende Werte je Spalte mit Anzahl
- Werte, die deutlich vom Rest der Spalte abweichen
- Moegliche Dubletten und uneinheitliche Schreibweisen desselben Kunden
Einschraenkung: Nichts korrigieren und nichts loeschen. Nur benennen und die
Zeilennummer angeben.
```

Dann kommt der Teil, den nur du erledigen kannst:

| Fund | Könnte Fehler sein | Könnte richtig sein |
|---|---|---|
| Stückpreis 1420 Euro bei Menge 1 | Komma verrutscht | Sonderanfertigung ist tatsächlich teurer |
| Kunde zweimal in anderer Schreibweise | Dublette | zwei rechtlich getrennte Standorte |
| Netto leer | Vergessen | Gutschrift, Wert wird später ergänzt |

!!! warning "Häufiges Missverständnis"
    Ein von der KI gemeldeter „Ausreißer" ist eine **statistische Beobachtung**, keine Fehlerfeststellung. Wer Ausreißer ungeprüft bereinigt, entfernt systematisch die interessantesten Fälle – Sonderaufträge, Großabrufe, Reklamationen. Behandle jeden Fund als Prüfauftrag, nicht als Korrekturanweisung (Kap. 8).

---

## 13.5 Text in Tabellenform bringen

Eine unterschätzte Stärke: aus Freitext eine strukturierte Tabelle machen – Mailanfragen, Protokollnotizen, Bestelltexte, Formularantworten.

```text
Aufgabe: Wandle die folgenden Bestellhinweise in eine Tabelle um.
Spalten: Kunde, Artikel, Menge, Wunschtermin, Besonderheit
Quelltext:
Frau Berger von Musterhandel braucht 12 Metallregale 200 mal 100 verzinkt bis
zum 20. Maerz, Lieferung nur vormittags moeglich.
Nordlicht Bueromoebel ruft 40 Rollcontainer weiss ab, Termin offen.
Talwerk Montage moechte eine Werkbank Buche als Sonderanfertigung, so schnell
wie moeglich, Hoehe 90 Zentimeter.
Format: Tabelle mit Trennzeichen Semikolon, eine Zeile je Bestellung.
Einschraenkung: Fehlende Angaben als leeres Feld lassen, nicht schaetzen.
Formuliere so schnell wie moeglich nicht in ein Datum um.
```

Die letzte Einschränkung ist der Kern. Ohne sie wird aus „so schnell wie möglich" mit hoher Wahrscheinlichkeit ein konkretes Datum – und aus einem Wunsch eine Zusage. Aus einem ungenauen Text darf keine genaue Tabelle entstehen; die Ungenauigkeit muss sichtbar bleiben.

!!! example "Der Weg in die Datei"
    Das Ergebnis kopierst du in Excel und trennst es über `Excel → Registerkarte Daten → Text in Spalten → Getrennt → Semikolon`. Danach `Registerkarte Einfügen → Tabelle` und Datentypen prüfen. Für regelmäßig wiederkehrende Fälle ist genau dieser Ablauf später ein Kandidat für Automatisierung (Kap. 29 und Kap. 31).

---

## 13.6 Die Rechengrenze und der Weg zu Power BI

Ein Sprachmodell erzeugt Text, indem es das jeweils wahrscheinlichste nächste Stück berechnet. Beim exakten Rechnen ist das die falsche Methode (Kap. 5). Zwei Fälle lassen sich von außen kaum unterscheiden: Entweder führt das Werkzeug intern eine echte Berechnung im Tabellenblatt aus – dann stimmt das Ergebnis. Oder es formuliert eine plausible Zahl – dann stimmt sie manchmal. Deshalb gilt eine einfache Regel: **Zahlen aus einem Chatfenster sind Hypothesen.** Lass die Rechnung in der Tabelle ausführen und prüfe gegen.

| Prüfung | Konkretes Vorgehen |
|---|---|
| Summenprobe | Ergebnis mit `=SUMME(...)` oder `=TEILERGEBNIS(...)` selbst nachrechnen |
| Zeilenprobe | Anzahl Zeilen im Ergebnis mit `=ANZAHL2(...)` gegen das Original prüfen |
| Extremwertprobe | größten und kleinsten Wert prüfen, dort fallen Fehler zuerst auf |
| Stichprobe | drei zufällige Zeilen von Hand nachrechnen, Größenordnung vergleichen |

Wenn du merkst, dass du dieselbe Auswertung monatlich neu baust, aus mehreren Quellen zusammenkopierst oder die Datei durch Formeln unbeherrschbar geworden ist, ist der Zeitpunkt für **Power BI** gekommen – ein Werkzeug, das Daten aus Quellen anbindet, sie dauerhaft aufbereitet und in Berichte überführt.

| Kriterium | Excel mit Copilot | Power BI |
|---|---|---|
| Anlass | einmalige oder unregelmäßige Frage | wiederkehrender Bericht |
| Datenmenge | bis einige zehntausend Zeilen | deutlich größer |
| Quellen | eine Datei | mehrere Systeme verbunden |
| Aktualisierung | manuell | geplant und automatisch |

Der Wechsel zu Power BI ändert dabei das Werkzeug, nicht die Verantwortung. Auch ein automatisch aktualisierter Bericht kann eine falsche Kennzahl konsistent falsch anzeigen – dann täglich und für alle sichtbar. Die Gegenprobe wird beim Umstieg also nicht weniger wichtig, sondern wichtiger.

---

## Zusammenfassung

- Datenarbeit mit KI beginnt bei der **Tabellenform**: eine Kopfzeile, ein Datensatz pro Zeile, keine verbundenen Zellen, einheitliche Datentypen.
- Formeln **erklären lassen** ist oft wertvoller als sie erzeugen zu lassen – jede erzeugte Formel braucht die **Drei-Fälle-Gegenprobe**. Bei **abgeleiteten Spalten** macht die Anweisung `unklar statt raten` Unsicherheit sichtbar.
- Gemeldete **Auffälligkeiten** sind Prüfaufträge, keine Fehlerfeststellungen – Sonderfälle und Fehler sehen gleich aus. Aus ungenauem **Text** darf keine scheingenaue Tabelle werden.
- Zahlen aus dem Chatfenster sind **Hypothesen**: Summen-, Zeilen-, Extremwert- und Stichprobenprüfung gehören dazu.
- Wiederkehrende Berichte gehören nach **Power BI** – die Prüfpflicht wandert mit.

---

## Kurzübungen

{{ task(file="tasks/k13_01.yaml") }}

{{ task(file="tasks/k13_02.yaml") }}

{{ task(file="tasks/k13_03.yaml") }}

---

## Workshop

{{ task(file="tasks/workshop_k13.yaml") }}
