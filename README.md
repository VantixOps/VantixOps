# VantixOps

**Wir entwickeln Software, die Unternehmen voranbringt.** VantixOps aus Bremen steht für individuelle Software, leistungsfähige Webanwendungen, moderne Buchungssysteme und digitale Plattformen. Wir entwickeln nicht von der Stange. Wir entwickeln für echte Anforderungen.

Wir bauen Lösungen, die zu realen Abläufen passen. Büroautomatisierung ergänzt unser Angebot: Sie reduziert Routinearbeit und schafft Freiraum für das eigentliche Geschäft.

**Unser technisches Fundament:** Softwareentwicklung, Linux, Self-Hosting, WordPress und n8n.

🌐 [vantix-ops.de](https://vantix-ops.de) · ✉️ kontakt@vantix-ops.de

## Warum wir hier sind

**Wir entwickeln nicht nur Software. Wir übernehmen Verantwortung für ihre Qualität.** Wir betreiben eigene Systeme, untersuchen technische Fehler und veröffentlichen nachvollziehbare Lösungen. Bugs melden wir dort, wo sie behoben werden können. Erkenntnisse aus realen Entwicklungs- und Betriebsproblemen dokumentieren wir offen – damit andere daraus lernen und dieselben Fehler vermeiden können.

## Was wir entwickeln

**Software statt Standardlösungen.** Unser Schwerpunkt liegt auf maßgeschneiderter Software und digitalen Produkten:

- **Individuelle Anwendungen:** Software, die konkrete Anforderungen und Prozesse abbildet.
- **Webanwendungen und digitale Plattformen:** moderne Lösungen für Browser und mobile Geräte.
- **Buchungs- und Verwaltungssysteme:** digitale Abläufe für Dienstleister, Studios und Unternehmen.
- **Büroautomatisierung:** weniger Routinearbeit durch vernetzte Systeme und gezielte Automatisierung – als Ergänzung unserer Softwareentwicklung.

Unsere Kundenprojekte und internen Systeme sind nicht automatisch öffentlich. Hier auf GitHub zeigen wir ausgewählte technische Arbeiten und Beiträge zur Entwicklergemeinschaft.

## Öffentliche Projekte & Open Source

| Projekt | Unser Beitrag |
|---|---|
| [n8n-workflow-templates](https://github.com/VantixOps/n8n-workflow-templates) | Importfertige n8n-Workflows: MCP-Server-Prüfung mit Benotung A–F, Fehleralarm ohne Mailflut, Uptime-Monitor mit bestätigtem Ausfall und Entwarnung. |
| [n8n-nodes-etermin](https://github.com/VantixOps/n8n-nodes-etermin) | Community-Node für n8n, die Termine und Kalender aus der eTermin-Buchungs-API liest; auf npm veröffentlicht. |
| [agent-root-guard](https://github.com/VantixOps/agent-root-guard) | Sicherheits-Hook für KI-Coding-Agenten: erkennt und blockiert bestimmte gefährliche Shell-Befehle, bevor sie ausgeführt werden. Aus einem realen Systemausfall entwickelt. |
| [silent-failures](https://github.com/VantixOps/silent-failures) | Ausführbare Tests für schwer erkennbare Fehler in Bash und Linux. Automatische Prüfungen helfen, die dokumentierten Verhaltensweisen nachvollziehbar zu halten. |
| [Thunderbird Developer Docs](https://github.com/VantixOps/developer-docs) und [WebExtension Examples](https://github.com/VantixOps/webext-examples) | Beiträge zur Fehlerkorrektur in Entwicklerdokumentation und Erweiterungsbeispielen; die zugehörigen Pull Requests sind unten verlinkt. |

**Unser Anspruch:** Wir bauen belastbare Software, prüfen Ergebnisse und teilen technische Erkenntnisse, die anderen Entwicklern weiterhelfen.

## Was wir gemeldet haben

| Datum | Projekt | Thema |
|---|---|---|
| 30.09.2026 | [bitwarden/clients#23569](https://github.com/bitwarden/clients/issues/23569) | Linux-Snap 2026.9.0 nimmt nach dem Entsperren keine Tastatureingabe mehr an. Umgehung per `snap revert` + `snap refresh --hold` |
| 13.10.2026 | [thunderbird/developer-docs#260](https://github.com/thunderbird/developer-docs/pull/260) · [thunderbird/webext-examples#42](https://github.com/thunderbird/webext-examples/pull/42) | „Hello World“-Tutorial für Erweiterungen: Der Filter „letzte 24 Stunden“ rechnet mit `24 * 60 * 1000` ms und erfasst nur 24 Minuten. Korrektur als Pull Request eingereicht |

## Was wir gelernt haben

Die meisten teuren Fehler bei uns waren **still**: kein roter Test, kein Fehlercode, alles sah grün aus. Daraus sind diese Regeln entstanden.

### Grundsätze

- **Eine Null braucht eine Gegenprobe.** „0 Treffer“ heißt oft „nicht gemessen“: falscher Pfad, abgebrochener Befehl, falsches Werkzeug. Vor jeder Null-Aussage denselben Befehl auf etwas ansetzen, das es sicher gibt.
- **Grün ist kein Beleg.** HTTP 200, „success“, `active` oder ein bestandener Test sagen, dass etwas *geantwortet* hat, nicht dass es *gewirkt* hat. Das Ergebnis dort nachlesen, wo es ankommen soll.
- **Nach einem Update erst die Vorversion probieren.** Das trennt in Minuten „das Programm ist kaputt“ von „mein System ist kaputt“, bevor man an Einstellungen dreht.
- **Den Rückweg testen, solange man ihn nicht braucht.** Eine Sicherung, die sich im Ernstfall nicht findet oder nicht öffnet, ist schlimmer als keine, weil man sich auf sie verlässt.
- **Nur löschen, was man selbst angelegt hat, und zwar nach Liste statt nach Muster.** Ein Stern (`*`) trifft auch Dateien aus früheren Läufen und anderen Sitzungen.
- **Eine Prüfung ist selbst Code.** Neu gebaute Prüfungen enthalten Fehler wie alles andere. Einmal absichtlich einen Fehler einbauen und schauen, ob die Prüfung ihn meldet.
- **Warnungen im Log gegen einen funktionierenden Stand halten, bevor man sie meldet.** Was auch dort auftaucht, ist nicht die Ursache. Eine falsche Spur kostet die Entwickler mehr Zeit als gar keine.

### Bash

- `set -e` beendet das Skript schon bei `x=$(befehl)`, wenn der Befehl scheitert. Ein `$?` in der Zeile danach wird nie gelesen.
- Zusammen mit `pipefail` bricht `X=$(… | grep -c muster)` das Skript ab, sobald es **0 Treffer** gibt, weil `grep -c` dann mit Status 1 endet. Ausgerechnet Prüfungen nach dem Muster „0 Fehler ist gut“ sterben so mitten im Lauf.
- `if befehl; then …; fi; rc=$?` liefert **immer 0**: Ein `if` ohne `else` hat selbst den Status 0.
- `befehl | tail` meldet den Status von `tail`, nicht von `befehl`. Abhilfe: `set -o pipefail` oder `${PIPESTATUS[0]}`.
- `grep … | wc -l` meldet `0` mit Status 0, auch wenn die Datei gar nicht existiert.
- Eine ungesetzte Variable unter `set -u` beendet bash **ohne** den ERR-Trap auszulösen. Im Cron sieht das aus wie Erfolg; nur ein EXIT-Trap mit Ende-Marke bemerkt es.
- `exit` in einer Funktion, die per `$(…)` gerufen wird, beendet nur die Subshell. Das Skript läuft weiter.
- Bei `sudo ls /root/datei-*` löst die **eigene** Shell den Stern auf, nicht root. Ohne Leserecht bleibt er wörtlich stehen, und die Meldung heißt „Datei nicht gefunden“. Richtig: `sudo sh -c 'ls /root/datei-*'`.
- `pkill -f` durchsucht die ganze Befehlszeile und trifft fremde Prozesse und das eigene Skript gleich mit. Vorher `pgrep -af` ansehen.
- `awk` rechnet unter deutscher Locale mit `0.18` als `0`. Vor Rechnungen `LC_ALL=C` setzen.

### Linux, systemd, cron

- Cron startet mit minimalem `PATH`. Programme aus `~/.local/bin` fehlen dort, das Skript scheitert still.
- Cron holt verpasste Läufe nicht nach. Ein Wächter, der prüft, wann ein Job zuletzt *erfolgreich* war, findet das.
- `OnCalendar=04:00..09:45/5` heißt nicht „alle 5 Minuten von 4 bis 9:45“. Der Schritt gilt für die **Sekunden**. Jeden Ausdruck mit `systemd-analyze calendar` prüfen.
- Ein Timer steht auf `active`, solange er feuert. Ob der Job lief, steht in `Result`/`ExecMainStatus` der Unit.
- `systemctl show` für eine Unit, die es gar nicht gibt, liefert Status 0 und Standardwerte. Immer `LoadState` mitlesen.
- Mehrere systemd-Drop-ins werden nach Dateinamen geladen, das letzte gewinnt. Vor dem Neustart `systemctl show DIENST -p Environment` gegen die Sollwerte halten.
- `rsync -a` nimmt die Dateirechte der Quelle mit. Danach als Dienstnutzer prüfen, ob der Dienst seinen Code noch lesen kann, **vor** dem Neustart.
- `rsync --delete` eine Ebene zu hoch angesetzt löscht die Nachbarordner. Erst mit `-n -v` laufen lassen und die `deleting`-Zeilen lesen.
- rclone: `--max-age 18m` heißt 18 **Minuten**, Monate sind `M`.
- Snap: `snap revert` holt die Vorversion zurück. Ohne `snap refresh --hold=<dauer>` spielt snapd das Update nach wenigen Stunden still wieder ein. Die Sperre immer mit Ablaufdatum setzen.

### Git

- `.gitignore` wirkt nur auf Dateien, die noch nicht versioniert sind. Schon eingecheckte Dateien bleiben drin, bis man `git rm --cached` ausführt.
- `git diff` sieht keine untrackten Dateien. Vor dem Löschen eines Branches `git status --porcelain` ohne Pfadfilter.
- Ein Branchname sagt nichts über den Inhalt. Vor dem Merge `git log main..branch` Commit für Commit lesen.

### n8n und Webhooks

- Ein Webhook, der sofort mit 200 antwortet, bestätigt nur den **Empfang**, nicht die Verarbeitung. Falsche Feldnamen ergeben dann leere Mails mit Erfolgsmeldung. Beleg ist das Ergebnis, etwa die Größe der gesendeten Nachricht.
- Nach einem n8n-Upgrade erst `healthz` abwarten, bevor abhängige Module neu gestartet werden.

### Web, WordPress, Cache

- Bei Single-Page-Apps beweist HTTP 200 nichts, weil der Fallback jeden Pfad mit 200 beantwortet. Der `Content-Type` sagt, ob die Datei wirklich da ist.
- Hinter einem CDN verhalten sich `HEAD` und `GET` unterschiedlich, und ein `?cb=` umgeht den Cache. Den Besucherweg misst man mit `GET` ohne Query, mehrfach, mit Blick auf `cf-cache-status` und `age`.
- Zwischen Deploy und Cache-Ablauf kann die Seite altes HTML mit schon neuen Bildern ausliefern. Hängt die Zulässigkeit eines Inhalts am umgebenden Text, bekommt er einen **neuen Dateinamen**.
- `wp eval` läuft ohne angemeldeten Benutzer. Plugin-Funktionen mit Rechteprüfung kehren dann still zurück. Abhilfe: `--user=1`, danach den Wert zurücklesen.
- `overflow:hidden` + `border-radius` schneidet Text in der Ecke ab, ohne dass die Box einen Überlauf meldet. Das findet nur ein Blick aufs Bild.

### PDF und Dokumente

- Eine Textsuche in der PDF-Datei selbst ist blind, weil die Inhalte komprimiert sind. Den Text mit `pdftotext` herausziehen.
- `pdftotext` ohne `-layout` verschluckt den Bindestrich am Zeilenende. Aus Kennungen und URLs wird dann etwas anderes.
- Formularfelder nur mit Wert und ohne Darstellung (`/AP`) sehen im Browser gut aus und drucken in Ghostscript **leer**. Der Renderer des Empfängers entscheidet.

### Python und Tests

- Ein Test, der gar nicht startet, schützt nichts. Die Zahl der gelaufenen **Dateien** mitlesen, nicht nur „passed“.
- Bei Mutationstests hält Python den Bytecode-Cache der mutierten Fassung, wenn Änderung und Rückbau in dieselbe Sekunde fallen. `__pycache__` danach löschen.
- `py_compile` prüft nur die Syntax. Erst der echte Import zeigt, ob das Modul lädt.
- Ein `import` führt den ganzen Modulcode aus. Fehlt der `__main__`-Wächter, schreibt er womöglich Dateien.
- Frisch angelegte Test-Datensätze über „ID größer als vorher“ prüfen, nicht über „der neueste“. Sonst besteht der Test mit einem alten Datensatz.

---
*English: VantixOps develops custom software, web applications, booking systems and digital platforms in Bremen, Germany. Business process automation complements our development services. We investigate bugs, contribute fixes and share practical engineering lessons.*
