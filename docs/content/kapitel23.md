# Kapitel 23 – Daten im Flow: dynamische Inhalte und Ausdrücke

{{ progress(23) }}

<div class="lernziele" markdown>
<h3>Was du in diesem Kapitel lernst</h3>

- Wann ein **dynamischer Inhalt** genügt und wann du einen **Ausdruck (Expression)** brauchst
- Wie du die **Ausdrucksleiste** benutzt und wie ein Ausdruck im Feld aussieht
- Die wichtigsten **Textfunktionen** und wie du damit Dateinamen und Betreffzeilen formst
- Wie du mit **if**, **equals**, **empty** und **int** Entscheidungen und Prüfungen in ein Feld bekommst
- Warum Datumsangaben in **UTC** ankommen und wie du ein deutsches Datum daraus machst
- Wie du mit der Aktion **Verfassen (Compose)** Ausdrücke sichtbar machst und Fehler eingrenzt
</div>

---

## 23.1 Dynamische Inhalte, Ausdrücke und die Ausdrucksleiste

Wenn du in ein Feld einer Aktion klickst, öffnet sich ein Auswahlfenster mit zwei Registern. Der **dynamische Inhalt (Dynamic Content)** ist eine Liste fertiger Werte aus den vorherigen Schritten: `Betreff`, `Von`, `Name der Anlage`. Du klickst einen an, er landet als farbige Kachel im Feld. Damit kommst du überraschend weit – solange du den Wert **unverändert** übernehmen willst.

Der **Ausdruck (Expression)** ist die zweite Registerkarte. Dort gibst du in der **Ausdrucksleiste** eine Formel ein, die aus einem oder mehreren Werten einen neuen berechnet. Ausdrücke brauchst du, sobald du etwas mit dem Wert **tun** willst: kürzen, zusammensetzen, umwandeln, formatieren, prüfen.

| Aufgabe | Dynamischer Inhalt reicht | Ausdruck nötig |
|---|---|---|
| Betreff in eine Mail übernehmen | ja | – |
| Betreff in Kleinbuchstaben schreiben | – | `toLower` |
| Empfangszeit anzeigen | ja, aber als UTC-Zeitstempel | `formatDateTime` für ein lesbares Datum |
| Dateiname aus Datum und Absender | – | `concat` |
| Prüfen, ob ein Feld leer ist | – | `empty` |

Ausdrücke werden im Feld als Kachel mit dem Formelsymbol angezeigt. Steht ein Ausdruck **innerhalb** von Text, schreibt Power Automate ihn in der Form `@{...}`. Das brauchst du beim Bauen von Nachrichten ständig:

```text
Neue Anfrage von @{triggerOutputs()?['body/from']} am @{formatDateTime(utcNow(),'dd.MM.yyyy')}
```

!!! info "Merksatz"
    Ein Ausdruck ist eine **Funktion mit runden Klammern**, die einen Wert zurückgibt. Alles, was du in Power Automate mit Daten machst, ist eine Verschachtelung solcher Funktionen. Du musst keine Programmiersprache lernen, aber drei Dinge musst du beherrschen: Klammern zählen, einfache Anführungszeichen um Text setzen, Kommas zwischen Argumenten.

---

## 23.2 Datentypen und JSON: was im Feld wirklich steckt

Jeder Wert im Flow hat einen **Datentyp**. Der Typ entscheidet, welche Funktionen erlaubt sind und warum ein Ausdruck manchmal scheitert, obwohl er richtig aussieht.

| Datentyp | Wie es aussieht | Typische Quelle | Typische Falle |
|---|---|---|---|
| **Text (String)** | `Rechnung 2024-118` | Betreff, Dateiname, Formularantwort | `12` als Text lässt sich nicht addieren |
| **Zahl (Integer, Float)** | `42`, `19.99` | Anzahl, Betrag, Dateigröße | Dezimaltrennzeichen ist der **Punkt**, nicht das Komma |
| **Datum/Zeit** | `2024-11-05T14:02:31Z` | Empfangszeit, Erstellungsdatum, `utcNow` | kommt fast immer in UTC an |
| **Boolean** | `true`, `false` | Kontrollkästchen, Ergebnis von `equals` | „Ja" als Text ist **nicht** `true` |
| **Array (Liste)** | `[Anlage1, Anlage2]` | Anlagen, Listenelemente, Suchergebnisse | ein Array in ein Textfeld setzen erzeugt eine automatische Schleife (Kap. 24) |
| **Objekt** | `Absender mit Name und Adresse` | Trigger-Ausgaben, API-Antworten | einzelnes Feld muss gezielt angesprochen werden |

Objekte und Arrays kommen in einem Format namens **JSON** daher. JSON ist nur eine Schreibweise für „Feldname und Wert", verschachtelt in geschweiften Klammern für Objekte und eckigen Klammern für Listen. Du musst JSON nicht schreiben können, aber lesen. Ein einzelnes Feld sprichst du mit eckigen Klammern und Anführungszeichen an; der Schrägstrich trennt die Ebenen, das Fragezeichen bedeutet „falls vorhanden":

```text
Antwort des Triggers, vereinfacht:
  from    = anna.beispiel@firma.de
  subject = Rechnung 2024-118
  attachments = Liste mit zwei Eintraegen
      Eintrag 1: name = LV.pdf        size = 84213
      Eintrag 2: name = Signatur.png  size = 1204

Zugriff auf einzelne Werte:
@{triggerOutputs()?['body/subject']}
@{outputs('Verfassen_Dateiname')}
@{items('Fuer_jeden_Anhang')?['name']}
```

Merke dir die Regel für Schrittnamen in Ausdrücken: **Leerzeichen werden zu Unterstrichen.** Der Schritt `Für jeden Anhang` heißt im Ausdruck `Fuer_jeden_Anhang`. Genau deshalb benennst du Schritte einmal am Anfang sauber und danach nicht mehr um – bestehende Ausdrücke verweisen sonst auf einen Namen, den es nicht mehr gibt.

---

## 23.3 Text formen: die sechs Funktionen für den Alltag

| Funktion | Zweck | Beispiel | Ergebnis |
|---|---|---|---|
| `concat` | Texte aneinanderhängen | `@{concat('RE-','2024','-118')}` | `RE-2024-118` |
| `trim` | Leerzeichen am Anfang und Ende entfernen | `@{trim('  Meier  ')}` | `Meier` |
| `toLower` | alles klein schreiben | `@{toLower('Anna.Beispiel@Firma.de')}` | `anna.beispiel@firma.de` |
| `length` | Länge eines Textes oder Anzahl der Einträge einer Liste | `@{length('Rechnung')}` | `8` |
| `split` | Text an einem Trennzeichen in eine Liste zerlegen | `@{split('anna@firma.de','@')}` | Liste mit `anna` und `firma.de` |
| `replace` | Zeichenfolge ersetzen | `@{replace('Angebot/Nr 5','/','-')}` | `Angebot-Nr 5` |

Die Kombination dieser Funktionen löst die häufigste Praxisaufgabe überhaupt: einen **eindeutigen, sauberen Dateinamen** bauen. Der Anhangname allein reicht nicht, weil drei Absender ihre Datei `Anfrage.pdf` nennen (Kap. 22). Und er ist riskant, weil Schrägstrich, Doppelpunkt, Fragezeichen, Sternchen und Anführungszeichen in Datei- und Ordnernamen verboten sind – eine Betreffzeile wie `Rechnung 05/2024` bringt den Schritt `Datei erstellen` sonst zum Absturz.

!!! example "Dateiname Schritt für Schritt aufgebaut"
    Ziel ist ein Name der Form `2024-11-05_anna.beispiel_LV.pdf`.

    ```text
    Baustein 1 - Datum sortierbar:
    @{formatDateTime(utcNow(),'yyyy-MM-dd')}

    Baustein 2 - Absender klein und ohne Domain:
    @{toLower(first(split(triggerOutputs()?['body/from'],'@')))}

    Baustein 3 - Originalname ohne verbotene Zeichen:
    @{replace(items('Fuer_jeden_Anhang')?['name'],'/','-')}

    Alles zusammen im Feld Dateiname:
    @{concat(outputs('Datum'),'_',outputs('Absender'),'_',outputs('Dateiname_bereinigt'))}
    ```

    Baue solche Ausdrücke **nie** in einem Zug. Lege für jeden Baustein eine eigene `Verfassen`-Aktion mit sprechendem Namen an, prüfe sie im Testlauf und setze erst am Ende zusammen. Das ist lesbar und im Fehlerfall in Sekunden zu prüfen.

---

## 23.4 Entscheiden und prüfen: if, equals, empty, int

Nicht jede Verzweigung braucht eine eigene Aktion. Wenn nur **ein Feld** unterschiedlich gefüllt werden soll, gehört die Entscheidung in den Ausdruck. Braucht dagegen ein ganzer **Zweig** unterschiedliche Schritte, nimmst du die Aktion `Bedingung (Condition)` (Kap. 24).

```text
Ersatztext, falls der Betreff leer ist:
@{if(empty(triggerOutputs()?['body/subject']),'Ohne Betreff',triggerOutputs()?['body/subject'])}
Pruefen, ob ein Wert genau einem anderen entspricht:
@{equals(toLower(triggerOutputs()?['body/from']),'einkauf@firma.de')}
Pruefen, ob eine Liste keine Eintraege hat:
@{empty(triggerOutputs()?['body/attachments'])}
Text in eine Zahl umwandeln, um damit zu rechnen:
@{add(int('14'),7)}
```

Drei Regeln dazu ersparen dir viel Sucharbeit. Erstens: `if` hat genau **drei** Argumente – Bedingung, Wert wenn wahr, Wert wenn falsch; der erste muss ein Boolean sein, deshalb steht dort meist `equals`, `empty`, `greater` oder `contains`. Zweitens: `equals` vergleicht **exakt**, auch Groß- und Kleinschreibung, also Adressen und Kategorien immer mit `toLower` auf beiden Seiten vergleichen. Drittens: `int` erwartet einen Text aus reinen Ziffern – `int('14 Tage')` scheitert, und bei Beträgen mit deutschem Komma musst du erst `replace` anwenden.

!!! warning "Häufiges Missverständnis"
    `empty` prüft, ob etwas **keinen Inhalt** hat – bei Text, Listen und Objekten. Es prüft **nicht**, ob ein Feld existiert. Fehlt ein Feld ganz, liefert der Zugriff ohne Fragezeichen einen Fehler; mit dem Fragezeichen in `?['feldname']` bekommst du stattdessen einen leeren Wert, den `empty` dann sauber erkennt. Setze das Fragezeichen bei allen optionalen Feldern.

---

## 23.5 Datum und Zeitzone: die häufigste Fehlerquelle

Power Automate rechnet intern in **UTC**, der koordinierten Weltzeit. Deutschland liegt im Winter eine Stunde und im Sommer zwei Stunden davor. Ein Zeitstempel, der im Flow ankommt, sieht so aus: `2024-11-05T14:02:31Z`. Das `Z` am Ende bedeutet UTC – in Deutschland war es zu diesem Zeitpunkt 15:02 Uhr.

| Funktion | Zweck | Beispiel | Ergebnis |
|---|---|---|---|
| `utcNow` | aktueller Zeitpunkt in UTC | `@{utcNow()}` | `2024-11-05T14:02:31Z` |
| `formatDateTime` | Zeitstempel in ein Anzeigeformat bringen | `@{formatDateTime(utcNow(),'dd.MM.yyyy')}` | `05.11.2024` |
| `addDays` | Tage addieren oder abziehen | `@{addDays(utcNow(),14,'dd.MM.yyyy')}` | `19.11.2024` |
| `convertTimeZone` | von einer Zeitzone in eine andere rechnen | `@{convertTimeZone(utcNow(),'UTC','W. Europe Standard Time','dd.MM.yyyy HH:mm')}` | `05.11.2024 15:02` |

Die Formatzeichen musst du dir merken: `dd` Tag, `MM` Monat, `yyyy` Jahr, `HH` Stunde im 24-Stunden-Format, `mm` Minute. Groß- und Kleinschreibung ist entscheidend – `mm` sind Minuten, `MM` ist der Monat; `dd.mm.yyyy` liefert deshalb im November nicht `11`, sondern die Minutenzahl. Für Dateinamen und Sortierungen nimm `yyyy-MM-dd`, für Texte, die Menschen lesen, `dd.MM.yyyy`. Der Zeitzonenname für Deutschland lautet `W. Europe Standard Time`; er umfasst Sommer- und Winterzeit automatisch.

!!! warning "Typische Falle"
    Rechne Zeitzonen **nie** mit `addHours(utcNow(),2)` um. Das ist im Sommer richtig und im Winter eine Stunde falsch – ein Fehler, der ein halbes Jahr unentdeckt bleibt und dann Fristen und Terminmails verschiebt. Verwende immer `convertTimeZone`. Dieselbe Falle betrifft geplante Flows: Dort stellst du die Zeitzone direkt im Trigger ein (Kap. 21).

---

## 23.6 Verfassen als Debugging-Werkzeug

Die Aktion **Verfassen (Compose)** aus der Gruppe `Datenvorgang (Data Operation)` tut nur eine Sache: Sie nimmt einen Wert an und gibt ihn unverändert wieder aus. Genau das macht sie zum wichtigsten Diagnosewerkzeug für Ausdrücke, denn ihre Ausgabe steht im Ausführungsverlauf und ist damit sichtbar.

```mermaid
flowchart LR
    A([Ausdruck in Verfassen setzen]) --> B([Flow testen])
    B --> C([Ausgabe im Verlauf lesen])
    C --> D([Ausdruck korrigieren])
    D --> B
```

Typischer Einsatz: Du willst wissen, was der Trigger überhaupt liefert. Setze eine `Verfassen`-Aktion direkt nach den Trigger, trage `@{triggerOutputs()}` ein, teste und lies die Ausgabe. Danach weißt du, welche Feldnamen es wirklich gibt, statt zu raten.

Wenn ein Ausdruck einen Fehler wirft, arbeite diese Reihenfolge ab: **Fehlertext lesen** – er nennt fast immer Funktion und Problem. **Ausdruck zerlegen** – die innerste Funktion in eine eigene `Verfassen`-Aktion setzen, allein testen, dann nach außen arbeiten. **Eingaben prüfen** – der Block `Eingaben` im Verlauf zeigt, mit welchem Wert der Ausdruck tatsächlich gearbeitet hat; oft ist er leer. **Typ und Klammern prüfen** – die meisten Ausdrucksfehler sind Typfehler, und die Ausdrucksleiste färbt zusammengehörige Klammern ein.

| Fehlermeldung im Verlauf | Ursache | Abhilfe |
|---|---|---|
| `The template language function ... expects ... parameters` | falsche Anzahl Argumente | Kommas und Klammern prüfen, `if` braucht drei |
| `... cannot be converted to ...` | Typfehler | `int`, `string` oder `formatDateTime` einsetzen |
| `Property ... doesn't exist` | Feldname falsch oder Feld fehlt | mit `@{triggerOutputs()}` die echten Feldnamen ansehen, Fragezeichen setzen |
| `Unable to process template language expressions ... is null` | Wert ist leer | mit `if` und `empty` einen Ersatzwert vorsehen |

!!! tip "Verfassen statt Variable"
    Für einen berechneten Wert, der sich nicht ändert, ist `Verfassen` die richtige Wahl: kein Initialisieren, kein Zuweisen, im Verlauf sichtbar. Variablen brauchst du erst, wenn ein Wert **innerhalb** des Flows fortgeschrieben wird – etwa ein Zähler in einer Schleife. Diese Unterscheidung und ihre Fallstricke behandelt Kapitel 24.

---

## Zusammenfassung

- **Dynamische Inhalte** übernehmen Werte unverändert; sobald du etwas mit einem Wert machst, brauchst du einen **Ausdruck** aus der Ausdrucksleiste.
- Jeder Wert hat einen **Datentyp**; die meisten Ausdrucksfehler sind Typfehler, nicht Schreibfehler.
- **JSON** ist nur Feldname und Wert; ein einzelnes Feld sprichst du mit `?['feld']` an, Leerzeichen in Schrittnamen werden zu Unterstrichen.
- Mit `concat`, `trim`, `toLower`, `length`, `split` und `replace` formst du Dateinamen und Betreffzeilen – Sonderzeichen vorher ersetzen.
- Zeitstempel kommen in **UTC**; deutsche Zeiten immer mit `convertTimeZone` und `W. Europe Standard Time`, niemals durch Stundenaddition.
- **Verfassen** macht Ausdrücke im Ausführungsverlauf sichtbar; komplexe Ausdrücke von innen nach außen zerlegen und einzeln prüfen.

---

## Kurzübungen

{{ task(file="tasks/k23_01.yaml") }}

{{ task(file="tasks/k23_02.yaml") }}

{{ task(file="tasks/k23_03.yaml") }}

---

## Workshop

{{ task(file="tasks/workshop_k23.yaml") }}
