# So baut ihr die Lektion mit einem LLM nach

Diese Lektion ist im Dialog mit einem LLM entstanden. Hier steht der Weg dorthin als Folge von vier Prompts, die aufeinander aufbauen, plus Prompts zum Anpassen.

## Vorweg: drei Dinge, die ihr wissen solltet

**1. Rechnet mit Sicherheitsfiltern.** Eine realistische Nachbildung eines ClickFix-Köders liegt nah am echten Angriff. Die Sicherheitsfilter mancher Modelle können deshalb anspringen und die Ausgabe abbrechen, auch wenn der Zweck klar eine Schulung ist. Uns ist das in automatisierten Testläufen passiert. Darum beginnt jeder Prompt mit dem Schulungszweck und den Sicherheitsregeln, und die Lektion entsteht in Schritten statt in einem großen Block. Bricht ein Modell trotzdem ab, versucht es mit einem anderen Werkzeug oder baut nur einen Schritt auf einmal. Als Ausgangspunkt könnt ihr auch einfach unsere fertige HTML-Datei nehmen und mit den Prompts unten anpassen. Das ist der schnellste und sicherste Weg.

**2. Kein Prompt erzeugt zweimal dieselbe Datei.** Ihr bekommt eine Lektion mit gleichem Aufbau, gleichem Ablauf und gleichen Sicherheitsregeln. Den letzten Schliff holt ihr mit Folgeprompts. Genau so ist auch das Original entstanden.

**3. Werkzeug.** Am besten ein leistungsstarkes Modell in einer Umgebung, in der es Dateien anlegen und im Browser testen kann. In einem normalen Chat geht es auch: Die Datei wird lang, gegebenenfalls mit „Schreib weiter, ab der Stelle …“ nachhelfen und danach jeweils die ganze aktuelle Datei wieder mitgeben.

Eckige Klammern `[…]` durch eure Werte ersetzen.

---

## Prompt 1: Zweck, Regeln und Rahmen der Lektion

```text
Ich bin für Informationssicherheit in [unserer Organisation] verantwortlich und baue eine Security-Awareness-Schulung. Ziel ist, unsere Mitarbeitenden vor dem Angriff „ClickFix“ und seiner Variante „FileFix“ zu schützen. Dabei bringen Webseiten Nutzer dazu, selbst einen Befehl einzufügen und auszuführen. Die Mitarbeitenden sollen den Ablauf in einer harmlosen Übungsumgebung einmal erleben, damit sie ihn im Ernstfall wiedererkennen.

Bitte baue die Lektion als EINE eigenständige HTML-Datei mit eingebettetem CSS und Vanilla-JavaScript, ohne Frameworks. Wir bauen sie in mehreren Schritten. In diesem Schritt: Rahmen, Kapitel und Wissenscheck. Für die beiden Simulationen (Kapitel 3 und 4) nur Platzhalter, die bauen wir in den nächsten Schritten.

SICHERHEITSREGELN, gelten für alle Schritte:
1. Die Seite schreibt nie in die echte Zwischenablage. Kein navigator.clipboard, kein document.execCommand. Alles „Kopierte“ ist nur ein Zustand in der Simulation.
2. Die Datei enthält keinen echten Befehl, auch nicht als Text. Der „versteckte Befehl“ ist immer ein sichtbarer Platzhalter: „▒▒▒ versteckter Befehl · in dieser Simulation entfernt ▒▒▒“.
3. Alle Webseiten, Domains, Marken und Personen sind erfunden. Eigene, schlichte Icons als Inline-SVG, keine Emojis, keine fremden Logos.
4. Keine externen Skripte, keine Tracker. Einzige externe Ressource: die Schrift „Exo 2“ von Google Fonts, mit Fallback.
5. Dauerhaft sichtbar ein Etikett „Simulation · nichts wird ausgeführt“.

ZIELGRUPPE UND SPRACHE: Mitarbeitende ohne IT-Hintergrund an Windows-Arbeitsplätzen. Deutsch, Anrede „Sie“, kurze Sätze. Deutsche Anführungszeichen „…“. Keine langen Gedankenstriche (—). Dauer etwa 8 Minuten.

GESTALTUNG: seriös und hochwertig, viel Weißraum, Karten mit feinen Rändern, dezente Verläufe. Oben im CSS ein Branding-Block mit Variablen: --brand [#272859], --brand-2 [#56579A], --accent [#5DC5EA]. Logo als <img> in der Kopfzeile: [Platzhalter „Ihr Logo“]. Responsiv: keine horizontale Scrollleiste, auf schmalen Bildschirmen alles untereinander.

AUFBAU DER LERN-APP:
- Kopfzeile: Logo, „Awareness-Lektion · ClickFix erkennen“, rechts das Etikett aus Regel 5.
- Fortschrittsleiste mit 7 Kapitelnamen (erledigt, aktuell, offen). Erreichte Kapitel sind anklickbar.
- Feste Navigationsleiste unten: „Zurück“, „Kapitel X von 7“, Weiter-Knopf mit passender Beschriftung. In Kapitel 3, 4 und 6 ist „Weiter“ gesperrt, bis die Aufgabe einmal erledigt ist. Danach bleibt es freigeschaltet, auch wenn die Simulation wiederholt wird. Solange Kapitel 3 und 4 nur Platzhalter sind, bleibt „Weiter“ dort offen.

KAPITEL:
1. Einstieg: Überschrift „ClickFix: Wenn die Webseite Sie bittet, selbst etwas auszuführen“, kurzer Einleitungstext, zwei Kennzahl-Karten ([eure Kennzahl mit Quelle, z. B. aus dem Microsoft Digital Defense Report 2025] und „0 Anhänge, Downloads oder Sicherheitslücken nötig“), eine dunkle Karte mit drei Lernzielen sowie Dauer, Anzahl Simulationen und Fragen.
2. So funktioniert es: drei Schritte nebeneinander (Eine Prüfung, die scheitert / Heimlich kopiert / Sie drücken nur Tasten). Darunter vier Varianten: Ausführen-Dialog, Terminal, Explorer-Adressleiste (FileFix, hervorgehoben, weil am unterschätztesten), macOS. Darunter Einfallswege als Chips: E-Mail-Link, gekaperte echte Website, Suchergebnis oder Werbung, gefälschte Browser-Erweiterung, Anleitungsvideos in sozialen Medien.
3. Simulation 1: Platzhalter.
4. Simulation 2: Platzhalter.
5. Auflösung: dunkles Banner mit der Kernregel „Keine echte Webseite verlangt, dass Sie Tasten drücken und etwas einfügen.“ Darunter vier Karten: Woran Sie es erkennen / Warum der Text harmlos aussieht (Befehl steht vorne, das Feld zeigt nur den Tarntext am Ende) / So reagieren Sie richtig (nichts einfügen, Tab schließen, IT melden) / Wenn es schon passiert ist (sofort IT informieren, Gerät vom Netz, nicht selbst aufräumen).
6. Wissenscheck: vier Multiple-Choice-Fragen. Nach jeder Antwort richtig/falsch markieren, Erklärung zeigen, Frage sperren. Nach vier Antworten ist das Kapitel erledigt, egal wie viele richtig waren.
   - Eine Seite verlangt nach einem Captcha, Win+R zu drücken, einzufügen und Enter zu drücken. Was tun Sie? (Richtig: nichts einfügen, Tab schließen, IT informieren.)
   - Warum wirkte der eingefügte Text harmlos? (Richtig: Der Befehl steht vorne, das Feld zeigt nur den Tarntext am Ende.)
   - Wo kann ein eingefügter Befehl ausgeführt werden? (Richtig: Ausführen-Dialog, Terminal und Explorer-Adressleiste.)
   - Sie haben es schon ausgeführt. Was ist richtig? (Richtig: sofort IT informieren, Gerät vom Netz.)
7. Abschluss: „Lektion abgeschlossen“ mit Anzahl der auf Anhieb richtigen Antworten, drei Karten (Die Regel / Der Trick / Die Reaktion), darunter „Über diese Lektion“ mit [Urheber] und Quellen.

LERNPLATTFORM: SCORM-1.2-Anbindung, nur aktiv, wenn ein LMS die API bereitstellt (window.API in Eltern-Frames suchen). Start: lesson_status „incomplete“. Nach dem Wissenscheck: „completed“ und score.raw 0 bis 100 (25 je richtiger Antwort). Keine Bestehensgrenze.

Gib die vollständige Datei aus.
```

---

## Prompt 2: Der Übungs-Desktop und Simulation 1

```text
Jetzt bauen wir Kapitel 3, die erste Übung. Zur Erinnerung: Es ist eine Schulungssimulation, alle Sicherheitsregeln aus dem ersten Schritt gelten weiter, insbesondere kein Zugriff auf die Zwischenablage und nur der sichtbare Platzhalter statt eines Befehls.

DARSTELLUNG DES ÜBUNGS-DESKTOPS (wird in Kapitel 4 wiederverwendet):
- Ein nachgebauter Windows-11-Desktop mit fester virtueller Größe 1024×640, per CSS transform auf die Containerbreite skaliert, in einem dunklen Monitorrahmen. Unten links am Rahmen ein kleines Etikett „Übungsumgebung“.
- Wiedererkennbar und glaubwürdig, damit der Transfer in den Alltag gelingt: Schrift „Segoe UI“ mit Systemfallback. Taskleiste unten mit Start-Symbol (vier blaue Quadrate), Suchfeld, Browser, Explorer-Ordner, Mail; Infobereich mit echter Uhrzeit und Datum. Browser maximiert mit Tab, Adressfeld und üblichen Symbolen. Fenster mit runden Ecken, weichem Schatten und den drei Fenster-Schaltflächen.

STEUERUNG: Die Teilnehmenden handeln selbst, direkt im simulierten Bildschirm.
- Keine Knöpfe im Aufgabenfeld, die Aktionen auslösen.
- Ein pulsierender Ring in der Akzentfarbe markiert das Element, mit dem es weitergeht.
- Rechts neben dem Bildschirm ein Aufgabenfeld: oben „Sie steuern direkt in der Simulation. Der blaue Ring zeigt, wo es weitergeht.“, dann „Ihre Aufgabe“, die Handlungsanweisung in einem umrandeten Kasten, Hinweise (blau = Info, rot = Warnsignal), eine Schrittliste mit Status, ganz unten „Simulation neu starten“.
- Die Windows-Taste fängt das Betriebssystem ab. Erkläre das in einem Satz und nutze die Alternative, die Windows selbst bietet: Rechtsklick (oder Klick) auf das Start-Symbol öffnet das Schnellmenü (Win+X) mit den üblichen Einträgen, nur „Ausführen“ ist aktiv.
- Tastenkürzel seitenweit abfangen, solange das Kapitel offen ist: Strg/⌘+V zum Einfügen, Pos1 zum Aufdecken, Enter zum Bestätigen. Die Eingabefelder sind keine echten Eingabefelder, sondern nicht editierbare Elemente, deren Inhalt das Skript setzt. Immer auch per Maus möglich: Rechtsklick ins Feld öffnet ein Kontextmenü, in dem nur „Einfügen“ aktiv ist; „OK“ bestätigt.
- Inaktive Elemente („Abbrechen“, Fenster-X, Herunterladen usw.) tun nichts.
- Ereignisse per Delegation am Desktop-Container, damit „Simulation neu starten“ einfach den Desktop neu aufbauen kann.

ABLAUF SIMULATION 1 (Titel „Sie möchten einem Meeting beitreten“):
1. Eine erfundene Meeting-Vorschauseite: Kamera-Vorschau mit Initialen, Meeting-Name „Q3-Update Team“, Namensfeld, „Jetzt teilnehmen“.
2. Danach ein Dialog „Kurze Sicherheitsüberprüfung“ mit einem Prüf-Kästchen der erfundenen Marke „SecureCheck“.
3. Klick aufs Kästchen: kurzer Ladekreis, dann „Verifizierung fehlgeschlagen“ mit drei nummerierten Schritten und gezeichneten Tasten (⊞ + R, Strg + V, Enter) und dem Hinweis, ein Code sei kopiert worden. Im Aufgabenfeld ein Warnsignal: Sie haben nichts kopiert.
4. Über das Start-Symbol „Ausführen“ öffnen: Ausführen-Dialog unten links mit „Öffnen:“-Feld, OK, Abbrechen, Durchsuchen.
5. Einfügen: Das Feld zeigt rechtsbündig abgeschnitten nur das Ende: „✓ Ich bin kein Roboter – Verifizierungs-ID 7F3A“ (der kurze Strich – ist erlaubt).
6. PFLICHTSCHRITT „Genauer hinsehen“: Erst mit Pos1 oder Klick ins Feld wird der ganze Inhalt sichtbar: der rote Platzhalter, eine Raute mit Leerzeichen, dann der Tarntext. Dazu eine rote Erklärung: Alles nach der Raute ist für den Befehl nur ein Kommentar und dient der Täuschung. Enter vor diesem Schritt bewirkt nichts außer einem kurzen Wackeln der Anweisung.
7. Enter oder OK: Ein Stopp-Overlay über dem Bildschirm: „Stopp. Hier würde der Angriff starten.“ mit Erklärung (Schadsoftware würde Passwörter, Sitzungs-Cookies und gespeicherte Zugänge auslesen; Sie hätten nur drei Tastenkombinationen gedrückt) und „Simulation wiederholen“. Das Overlay steht außerhalb der Skalierung, damit es auch auf dem Handy lesbar bleibt.

Prüfe beide Wege (Tastatur und Maus) und das Zurücksetzen gedanklich durch. Gib die vollständige, aktualisierte Datei aus.
```

---

## Prompt 3: Simulation 2 (FileFix)

```text
Jetzt Kapitel 4, die zweite Übung, mit demselben Übungs-Desktop, derselben Steuerung und denselben Sicherheitsregeln. Diese Variante zeigt, dass auch die Adressleiste des Datei-Explorers Befehle ausführt. Das wissen die wenigsten.

ABLAUF SIMULATION 2 (Titel „Eine Kollegin hat ein Dokument freigegeben“):
1. Ein erfundenes „Freigabeportal“ mit Dokumentkarte: „Gehaltsanpassung_2026.pdf“, freigegeben von einer erfundenen Person aus der Personalabteilung, Knopf „Öffnen“.
2. „Öffnen“ zeigt einen Dialog „Datei im Explorer öffnen“ mit vier Schritten (Pfad kopieren, ⊞ + E, Strg + L, Strg + V und Enter) und einem Pfadfeld „C:\Firma\Personal\Freigaben\Gehaltsanpassung_2026.pdf“ mit Knopf „Kopieren“ (wird zu „✓ Kopiert“, nur in der Simulation).
3. Explorer öffnen über Klick auf das Ordner-Symbol in der Taskleiste (Ersatz für ⊞ + E, kurz erklären). Ein Windows-11-Explorer als Fenster: Tab „Start“, Adressleiste, Suchfeld, Befehlsleiste, Navigationsbereich, Schnellzugriff und „Zuletzt verwendet“ mit erfundenen Dateien.
4. Strg/⌘+L oder Klick: Adressleiste im Bearbeitungsmodus, „Start“ markiert.
5. Einfügen (Tastenkürzel oder Rechtsklick und „Einfügen“): Die Adressleiste zeigt rechtsbündig nur den Dateipfad. Daneben erscheint ein Pfeil zum Bestätigen.
6. PFLICHTSCHRITT „Genauer hinsehen“ wie in Simulation 1: Pos1 oder Klick zeigt Platzhalter, Raute und Pfad als Tarnung, mit roter Erklärung.
7. Enter oder Pfeil: Stopp-Overlay „Stopp. Auch hier startet der Angriff.“ mit Erklärung und „Simulation wiederholen“.

Gib die vollständige, aktualisierte Datei aus.
```

---

## Prompt 4: Prüfen und Feinschliff

```text
Prüfe die Datei jetzt kritisch wie ein Tester:
1. Spiele beide Simulationen per Tastatur und per Maus durch, dann „Simulation neu starten“ und noch einmal.
2. Prüfe den Wissenscheck und die SCORM-Meldungen.
3. Prüfe die Sicherheitsregeln: kein Zugriff auf die Zwischenablage, kein Befehl in der Datei, keine externen Skripte.
4. Prüfe die Darstellung bei 1440 px und bei 400 px Breite.
Behebe alle Fehler und gib mir danach eine kurze Liste, was du geändert hast, und die vollständige Datei.
```

---

## Prompts zum Anpassen

**Mehr Realismus, wenn es noch nach Übungsblatt aussieht**

```text
Die Übungs-Desktops wirken noch schematisch. Verbessere die Optik in Richtung Windows 11: Abstände, Schatten und Radien, Taskleiste mit Infobereich, Browser-Tab mit geschwungenen Ecken, ausgearbeitete Seiteninhalte statt leerer Karten. Ablauf und Sicherheitsregeln bleiben unverändert.
```

**An euren Alltag anpassen**

```text
Passe Simulation 1 an unseren Alltag an: Statt einer Meeting-Einladung kommt der Link aus [einer Paketbenachrichtigung / einem Lieferantenportal / einer Rechnungsmail]. Behalte Ablauf, Sicherheitsregeln und Stil bei.
```

**Eigenes Branding**

```text
Ersetze das Branding: Primärfarbe [#0B5394], Akzent [#F4B400], Logo [Datei]. Ändere sonst nichts.
```

**SCORM-Paket**

```text
Erzeuge zusätzlich eine imsmanifest.xml für SCORM 1.2 mit index.html als SCO, ohne Bestehensgrenze, und erkläre mir, wie ich beides zu einem ZIP packe.
```

---

## Prüfen, bevor ihr verteilt

- [ ] Beide Simulationen per Tastatur UND per Maus durchspielen, auch „Simulation neu starten“
- [ ] Nach der Lektion in einen Texteditor einfügen: Die Zwischenablage muss unverändert sein
- [ ] Im Quelltext nach `clipboard` und `execCommand` suchen: Beides darf nicht vorkommen
- [ ] Nach `powershell`, `mshta`, `curl` und `irm` suchen: Höchstens als Wort im Erklärtext, nie als Befehl
- [ ] Nach `<script src` suchen: Es darf kein externes Skript geladen werden
- [ ] Inhalte fachlich gegenlesen, vor allem Zahlen und Quellen
