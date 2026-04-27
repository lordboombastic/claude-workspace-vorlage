# Plan: Team-Workspace-Vorlage für Kollegen erstellen

**Erstellt:** 2026-04-23
**Status:** Implementiert
**Anforderung:** Neuen Workspace auf Basis des aktuellen Templates bauen, bereinigt von persönlichen Daten, mit Onboarding-Flow bei `/prime` und Anleitung zur Erstellung spezialisierter Themen-Workspaces.

---

## Überblick

### Was dieser Plan erreicht

Wir erzeugen einen zweiten, komplett unpersonalisierten Workspace-Ordner parallel zu `claude-workspace-vorlage/`, den Kollegen als Startpunkt klonen können. Der erste `/prime`-Aufruf erkennt die Template-Rohfassung und führt den neuen Benutzer durch die Grundkonzepte (Workspaces, optionale Subagents, context-Befüllung, Plan-first-Arbeitsweise), statt wie bisher sofort nach Felix' Daten zu suchen.

### Warum das wichtig ist

Felix will das Setup weitergeben. Aktuell ist der Workspace persönlich kontaminiert — eine direkte Kopie würde SteelcoBelimed-Details, Side-Business-Pläne und Familienhinweise an Kollegen leaken. Außerdem fehlt einem Erstbenutzer das mentale Modell: „Warum überhaupt context-Dateien?", „Wann plane ich mit `/create-plan`?", „Kann ich mehrere Workspaces haben?". Ein onboarding-fähiges Template beantwortet diese Fragen beim ersten Start — ohne dass Felix jedem Kollegen persönlich erklären muss.

---

## Aktueller Zustand

### Relevante bestehende Struktur

- `claude-workspace-vorlage/CLAUDE.md` — Fundament-Dokument, vollständig einsatzbereit, aber mit Felix-Referenzen (SteelcoBelimed, Side Business) und Du-Form auf Felix zugeschnitten
- `claude-workspace-vorlage/context/personal-info.md` — persönliche Daten (Name, Arbeitgeber, Rolle, Lebenssituation)
- `claude-workspace-vorlage/context/business-info.md` — SteelcoBelimed-Kontext, Side-Business-Details
- `claude-workspace-vorlage/context/strategy.md` — konkrete Ziele (AI-Telefonie, Felddiagnostik, Team-Schulung)
- `claude-workspace-vorlage/context/current-data.md` — persönliche Metriken, Infrastruktur-Details
- `claude-workspace-vorlage/.claude/commands/prime.md` — lädt context/, fasst zusammen, bestätigt Bereitschaft
- `claude-workspace-vorlage/.claude/commands/create-plan.md` — vollständiger, sehr guter Planungs-Command
- `claude-workspace-vorlage/.claude/commands/implement.md` — Plan-Ausführungs-Command
- `claude-workspace-vorlage/.claude/commands/shutdown.md` — Session-Abschluss
- `claude-workspace-vorlage/.claude/skills/mcp-integration/` — generischer MCP-Skill, direkt übertragbar
- `claude-workspace-vorlage/.claude/skills/skill-creator/` — generischer Skill-Ersteller, direkt übertragbar
- `claude-workspace-vorlage/.claude/settings.local.json` — lokale Permissions, nicht geeignet für Template
- `claude-workspace-vorlage/shell-aliases.md` — generische `cs`/`cr` Aliase, direkt übertragbar
- `claude-workspace-vorlage/.gitignore` — `.DS_Store`, passt
- `Vorlagen/context-blanko/` — bereits vorhandener leerer Kontext-Ordner (siehe offene Frage zur Wiederverwendung)
- `Vorlagen/claude-workspace-SPS-Programmierer/` — zeigt, dass Felix bereits das Muster „mehrere Themen-Workspaces" lebt

### Verifizierte Claude-Code-Fakten (aus Recherche)

Damit das Template keine falschen Begriffe transportiert, wurde `claude-code-guide` konsultiert. Relevante Ergebnisse:

- **Subagents** sind spezialisierte KI-Assistenten mit eigenem Kontext-Fenster, eigenem System-Prompt und eigenen Tool-Restrictions. Sie liegen in `.claude/agents/<name>.md` (projektlokal) oder `~/.claude/agents/<name>.md` (benutzerweit), Format: Markdown mit YAML-Frontmatter.
- **Workspace** ist kein offizieller Claude-Code-Begriff, sondern eine selbst gewählte Bezeichnung für einen Projektordner mit `CLAUDE.md` + `.claude/`. Das ist **nicht dasselbe** wie ein Subagent: Workspaces organisieren den Menschen, Subagents erweitern Claude.
- **Plugins** (offizielles Feature) sind verteilbare Pakete mit Skills/MCP-Servern/Subagents — für das Template nicht relevant, soll daher nicht erwähnt werden (Verwirrungsgefahr).
- **Slash-Commands** können projektlokal (`.claude/commands/`) oder benutzerweit (`~/.claude/commands/`) liegen. Wir bleiben für das Template bei projektlokal.

### Lücken oder Probleme, die adressiert werden

1. **Persönliche Daten:** Aktuell überall in CLAUDE.md und context/.
2. **Fehlendes Onboarding:** Kein Erstbenutzer weiß, dass/wie er context/ ausfüllen soll, ob Subagents dieselbe Sache wie Workspaces sind, oder warum `/create-plan` vor `/implement` Token spart.
3. **Fehlende Anleitung für Themen-Workspaces:** Felix hat `claude-workspace-SPS-Programmierer` und die Hauptvorlage parallel — das Muster „ein Workspace pro Thema/Domäne" ist aber nirgendwo dokumentiert.
4. **Fehlendes Modell für Subagents:** Kollegen könnten „Workspace" und „Subagent" verwechseln oder sich fragen, ob sie mehrere Workspaces brauchen oder stattdessen Subagents einsetzen.

---

## Vorgeschlagene Änderungen

### Zusammenfassung der Änderungen

- Neuen Ordner `Vorlagen/claude-workspace-vorlage-team/` als bereinigte Kopie anlegen
- CLAUDE.md entpersonalisieren und um einen Abschnitt „Wie dieses Template verwenden" ergänzen
- context/-Dateien in leere, gut kommentierte Templates mit Platzhaltern umwandeln
- `/prime` um einen Onboarding-Modus erweitern, der bei nicht ausgefülltem Kontext aktiv wird
- Neues Dokument `ONBOARDING.md` im Root als ausführliche Erklärung der Konzepte (Workspace, Subagent, Themen-Workspaces, Plan-first-Philosophie)
- Leeren `.claude/agents/`-Ordner mit README als Hinweis auf die Subagent-Möglichkeit anlegen
- Generische Skills (`mcp-integration`, `skill-creator`) unverändert übernehmen
- `settings.local.json` **nicht** übernehmen (persönlich), stattdessen nichts anlegen — Claude Code erzeugt die Datei bei Bedarf selbst
- `/create-plan`, `/implement`, `/shutdown` unverändert übernehmen (sind bereits generisch)
- `shell-aliases.md` unverändert übernehmen
- `.gitignore` unverändert übernehmen, um `history/` und persönliche Caches erweitern, falls relevant

### Neue Dateien erstellen

| Dateipfad                                                                              | Zweck                                                                                                                |
| -------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------- |
| `Vorlagen/claude-workspace-vorlage-team/CLAUDE.md`                                     | Bereinigte, unpersonalisierte Fundament-Datei mit Abschnitt „Wie du mit diesem Template arbeitest"                   |
| `Vorlagen/claude-workspace-vorlage-team/ONBOARDING.md`                                 | Ausführliche Erklärung: Workspaces vs. Subagents, Themen-Workspaces, context-Sinn, Plan-first-Arbeitsweise           |
| `Vorlagen/claude-workspace-vorlage-team/context/personal-info.md`                      | Leeres Template mit Platzhaltern und Kommentaren, ausfüllbar in 5 Minuten                                            |
| `Vorlagen/claude-workspace-vorlage-team/context/business-info.md`                      | Leeres Template mit Platzhaltern und Kommentaren                                                                     |
| `Vorlagen/claude-workspace-vorlage-team/context/strategy.md`                           | Leeres Template mit Platzhaltern und Kommentaren                                                                     |
| `Vorlagen/claude-workspace-vorlage-team/context/current-data.md`                       | Leeres Template mit Platzhaltern und Kommentaren                                                                     |
| `Vorlagen/claude-workspace-vorlage-team/.claude/commands/prime.md`                     | Erweiterter Prime-Command mit Onboarding-Branch                                                                      |
| `Vorlagen/claude-workspace-vorlage-team/.claude/commands/create-plan.md`               | Unveränderte Kopie des bestehenden Commands                                                                          |
| `Vorlagen/claude-workspace-vorlage-team/.claude/commands/implement.md`                 | Unveränderte Kopie                                                                                                   |
| `Vorlagen/claude-workspace-vorlage-team/.claude/commands/shutdown.md`                  | Unveränderte Kopie                                                                                                   |
| `Vorlagen/claude-workspace-vorlage-team/.claude/agents/README.md`                      | Kurzer Hinweis: hier liegen optionale Subagents, Link zur offiziellen Doku                                           |
| `Vorlagen/claude-workspace-vorlage-team/.claude/skills/mcp-integration/…`              | Unveränderte Kopie des kompletten Skill-Ordners                                                                      |
| `Vorlagen/claude-workspace-vorlage-team/.claude/skills/skill-creator/…`                | Unveränderte Kopie des kompletten Skill-Ordners                                                                      |
| `Vorlagen/claude-workspace-vorlage-team/plans/.gitkeep`                                | Leerer Platzhalter, damit Ordner im Git erhalten bleibt                                                              |
| `Vorlagen/claude-workspace-vorlage-team/outputs/.gitkeep`                              | Leerer Platzhalter                                                                                                   |
| `Vorlagen/claude-workspace-vorlage-team/reference/.gitkeep`                            | Leerer Platzhalter                                                                                                   |
| `Vorlagen/claude-workspace-vorlage-team/scripts/.gitkeep`                              | Leerer Platzhalter                                                                                                   |
| `Vorlagen/claude-workspace-vorlage-team/shell-aliases.md`                              | Unveränderte Kopie                                                                                                   |
| `Vorlagen/claude-workspace-vorlage-team/.gitignore`                                    | Kopie plus Ergänzungen für Caches/History                                                                            |

### Zu ändernde Dateien

| Dateipfad                                                             | Änderungen                                                                                     |
| --------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------- |
| `claude-workspace-vorlage/plans/2026-04-23-team-workspace-vorlage-erstellen.md` | (Diese Plan-Datei selbst) — nach Umsetzung von `/implement` Status-Update auf „Implementiert"       |

**Hinweis:** Am bestehenden `claude-workspace-vorlage/` wird nichts geändert. Felix' persönlicher Workspace bleibt unberührt.

### Zu löschende Dateien (falls vorhanden)

Keine.

---

## Design-Entscheidungen

### Getroffene Schlüsselentscheidungen

1. **Neuer Ordner statt Umbau des bestehenden:** Felix' aktuellen Workspace unberührt lassen — er nutzt ihn produktiv. Parallelordner ist risikofrei und lässt beide unabhängig weiterleben.
2. **Ordnername `claude-workspace-vorlage-team`:** Bleibt im etablierten Deutsch-Schema (`vorlage`), erweitert um `-team` als klare Abgrenzung zum persönlichen Felix-Template. Passt zum Schwester-Ordner `claude-workspace-SPS-Programmierer`.
3. **Onboarding-Branch innerhalb `/prime`** statt separatem `/onboarding`-Command: Der Benutzer lernt so gleich, dass `/prime` der Standard-Einstieg ist. Ein separater Command wäre nach einer Nutzung überflüssig und würde verwaist zurückbleiben. Der Branch triggert, solange die context-Dateien Platzhalter enthalten (konkret: Vorkommen von `<AUSFÜLLEN>` oder ähnlichen Markern).
4. **ONBOARDING.md als Referenzdokument zusätzlich zum Prime-Flow:** Prime erklärt knapp und handlungsorientiert; ONBOARDING.md ist Tiefgang-Lektüre, die der Benutzer freiwillig lesen kann. So bleibt der Prime-Output kurz.
5. **Subagents erwähnen, aber nicht tief dokumentieren:** Wir verweisen auf die offizielle Doku (code.claude.com/docs), statt sie zu duplizieren. Ein leerer `.claude/agents/`-Ordner mit README signalisiert „hier hin, falls ihr Subagents braucht".
6. **Context-Templates mit HTML-Kommentaren `<!-- AUSFÜLLEN: … -->`:** Bestehendes Muster aus `strategy.md` übernehmen — die Datei bleibt als Markdown gültig, wird aber beim Lesen nicht durch ausgefüllte Beispieldaten vorgeprägt.
7. **`.claude/settings.local.json` nicht übernehmen:** Diese Datei ist maschinen- und benutzerspezifisch; Claude Code erstellt sie bei Bedarf selbst neu. Eine Kopie würde fremde Pfade/Permissions einschleppen.
8. **Keine Plan-/Output-Beispiele mitliefern:** Ein leerer `plans/`-Ordner ist besser als Beispieldaten, die missverstanden werden könnten. Die Struktur ist selbsterklärend.
9. **Plan-first-Philosophie wird in ONBOARDING.md und CLAUDE.md explizit begründet:** Gerade auf Opus-/Sonnet-Modellen wandert ohne Plan schnell ein großer Output-Stream durch, der am Endziel vorbeiläuft. `/create-plan` schreibt einen günstigen Plan, der vor dem teuren `/implement` geprüft/angepasst werden kann.
10. **Nur Deutsch:** Der bestehende Workspace ist durchgängig deutsch — wir halten die Sprache konsistent, die Kollegen sind vermutlich deutschsprachig.

### Betrachtete Alternativen

- **Bestehenden Workspace umbauen und persönliche Daten per Git-History-Rewrite entfernen:** Zu invasiv, zerstört Felix' produktive Umgebung und macht die Historie für ihn unleserlich.
- **`context-blanko/` aus `Vorlagen/` erweitern:** Enthält laut `ls` keine Dateien, Struktur unklar. Von Grund auf neu aufzubauen ist sauberer, als auf einem unvollständigen Zwischenstand aufzusetzen. (Kann als offene Frage gestellt werden.)
- **Ein dediziertes `/onboarding`-Command:** Verworfen — verwaister Command nach erster Nutzung; verwirrt Neulinge, welcher Command zuerst kommt.
- **Interaktiver Fragebogen via Claude:** Reizvoll, aber außerhalb des Scope. Der Benutzer füllt context/-Dateien in seinem Editor aus und ruft `/prime` erneut auf — das ist einfach und robust.
- **Zwei separate Templates (eines „leer", eines „mit Beispieldaten"):** Mehr Wartungsaufwand; ein Template mit klaren Platzhaltern reicht aus.

### Offene Fragen — beantwortet

1. **Ordnername:** `claude-workspace-vorlage-team` ✓
2. **`Vorlagen/context-blanko/`:** Ignorieren ✓
3. **Platzhalter-Syntax:** HTML-Kommentar `<!-- AUSFÜLLEN: … -->` ✓
4. **Git-Handling:** Neues eigenes, sauberes Repo im Template-Ordner (`git init` ohne Remote). **Keine** Verbindung zu Felix' persönlichen Repos. Ein initialer Commit, damit Kollegen direkt weiterarbeiten können. ✓
5. **Publikationsweg:** Manuelle Verteilung durch Felix. Zusätzlich zur Ordnerstruktur wird eine ZIP-Datei `claude-workspace-vorlage-team.zip` im Ordner `Vorlagen/` abgelegt. ✓

---

## Schritt-für-Schritt-Aufgaben

### Schritt 1: Zielordner und Grundstruktur anlegen

**Aktionen:**

- `mkdir -p /home/lord3000/Claude_workspace/Vorlagen/claude-workspace-vorlage-team/{context,plans,outputs,reference,scripts,.claude/commands,.claude/skills,.claude/agents}`
- `.gitkeep` in `plans/`, `outputs/`, `reference/`, `scripts/` ablegen, damit die Ordner im späteren Git erhalten bleiben

**Betroffene Dateien:**

- `Vorlagen/claude-workspace-vorlage-team/` (neu)
- inkl. aller Unterordner + `.gitkeep`-Dateien

---

### Schritt 2: `.gitignore` und `shell-aliases.md` übernehmen

**Aktionen:**

- `.gitignore` aus `claude-workspace-vorlage/` kopieren, ergänzen um übliche Caches (`.DS_Store`, `__pycache__/`, `*.pyc`, `*.log`, `.claude/settings.local.json`)
- `shell-aliases.md` 1:1 kopieren

**Betroffene Dateien:**

- `Vorlagen/claude-workspace-vorlage-team/.gitignore`
- `Vorlagen/claude-workspace-vorlage-team/shell-aliases.md`

---

### Schritt 3: Generische Commands übernehmen

**Aktionen:**

- `create-plan.md`, `implement.md`, `shutdown.md` 1:1 aus `claude-workspace-vorlage/.claude/commands/` in die neue `.claude/commands/` kopieren
- Prüfen: enthalten diese Dateien persönliche Referenzen? (aus vorheriger Lektüre: nein, sie sind generisch — trotzdem im Zuge der Implementierung nochmals überfliegen)

**Betroffene Dateien:**

- `Vorlagen/claude-workspace-vorlage-team/.claude/commands/create-plan.md`
- `Vorlagen/claude-workspace-vorlage-team/.claude/commands/implement.md`
- `Vorlagen/claude-workspace-vorlage-team/.claude/commands/shutdown.md`

---

### Schritt 4: Generische Skills übernehmen

**Aktionen:**

- Komplette Ordner `mcp-integration/` und `skill-creator/` (inkl. Unterordner `references/` etc.) rekursiv nach `Vorlagen/claude-workspace-vorlage-team/.claude/skills/` kopieren
- Kurzer Sanity-Check, ob Skill-Dateien persönliche Referenzen enthalten (unwahrscheinlich, da generisch)

**Betroffene Dateien:**

- `Vorlagen/claude-workspace-vorlage-team/.claude/skills/mcp-integration/` (rekursiv)
- `Vorlagen/claude-workspace-vorlage-team/.claude/skills/skill-creator/` (rekursiv)

---

### Schritt 5: Neuen `/prime` mit Onboarding-Branch schreiben

**Aktionen:**

- Datei `Vorlagen/claude-workspace-vorlage-team/.claude/commands/prime.md` erstellen
- Inhalt: bestehende `prime.md`-Logik (ls, find, Lesen) **plus** ein vorgeschalteter Check:
  - Grep in `context/` nach dem Marker `<!-- AUSFÜLLEN` (oder eine ähnliche Heuristik)
  - Wenn Treffer vorhanden → **Onboarding-Modus:** Ausgabe einer strukturierten Einführung (siehe unten), dann trotzdem den normalen Prime-Output in Kurzform liefern
  - Wenn keine Treffer → **Standard-Modus:** wie bisher, nur Zusammenfassung
- Onboarding-Modus-Output muss folgende Punkte in dieser Reihenfolge abdecken (der Command beschreibt, was Claude ausgeben soll):
  1. **Willkommen & was dieser Workspace ist**
  2. **Workspaces vs. Subagents — der wichtige Unterschied:** Ein Workspace ist dieser Ordner mit CLAUDE.md und context/. Ein Subagent ist ein separater KI-Helfer mit eigenem Kontext, der in `.claude/agents/` definiert wird und vom Haupt-Claude aufgerufen werden kann. Workspaces organisieren den Menschen, Subagents erweitern Claude. Für den Anfang brauchst du keine Subagents.
  3. **Spezialisierte Themen-Workspaces:** Diese Vorlage kann für beliebige Themen mehrfach genutzt werden (z.B. ein Workspace für „Marketing-Projekt X", einer für „Forschungsthema Y", einer fürs Hobbyprojekt). Jedes Klon ist ein eigener Ordner mit eigener CLAUDE.md und eigenem context/ — Claude weiß in jeder Session exakt, worum es geht. Das ist **kein** Subagent-Konzept.
  4. **Context-Dateien ausfüllen ist Pflicht, nicht Zierde:** Ohne ausgefüllte `context/`-Dateien arbeitet Claude blind. Die vier Dateien sind in 15 Minuten ausgefüllt, sparen in jeder Session danach Zeit und verhindern, dass Claude generische Antworten liefert. Pointer auf ONBOARDING.md für Details.
  5. **`/create-plan` vor `/implement`:** Größere Aufgaben **nicht** direkt ausführen lassen. `/create-plan` schreibt einen billigen, strukturierten Plan (wenige Tokens), den der Benutzer prüft, anpasst und freigibt. Erst dann `/implement`. Ohne Plan verbrennt Claude sonst leicht viele Tokens an einer Richtung, die nicht zusagt — und der Benutzer zahlt doppelt: einmal für den Fehlversuch, einmal für die Korrektur.
  6. **Nächster Schritt für den Benutzer:** „Füll die context/-Dateien aus, dann ruf `/prime` erneut auf."

**Betroffene Dateien:**

- `Vorlagen/claude-workspace-vorlage-team/.claude/commands/prime.md` (neu)

---

### Schritt 6: `CLAUDE.md` schreiben (entpersonalisiert)

**Aktionen:**

- Datei `Vorlagen/claude-workspace-vorlage-team/CLAUDE.md` erstellen
- Basis: Struktur des bestehenden `CLAUDE.md`, aber mit folgenden Anpassungen:
  - Einleitung in Du-Form an einen beliebigen Benutzer gerichtet, nicht an Felix
  - Alle SteelcoBelimed-/Side-Business-/Familien-Referenzen entfernen
  - Abschnitt „Die Claude-User-Beziehung" generisch formulieren (User ist Wissensarbeiter/Techniker/etc., nicht Felix)
  - Neuer Abschnitt **„Wie du dieses Template verwendest"** direkt nach „Was das hier ist":
    - Schritt 1: Ordner an den gewünschten Ort kopieren und umbenennen (z.B. `mein-workspace-marketing/`)
    - Schritt 2: `context/`-Dateien ausfüllen
    - Schritt 3: `claude` starten und `/prime` ausführen
    - Hinweis: Für ein anderes Thema einfach die Vorlage erneut klonen — jedes Thema bekommt seinen eigenen Workspace
  - Neuer Abschnitt **„Workspace ≠ Subagent"** (kurz, 3–5 Zeilen) mit Verweis auf ONBOARDING.md für Details
  - Neuer Abschnitt **„Warum `/create-plan` vor `/implement`?"** (kurz, 3–5 Zeilen) mit Verweis auf ONBOARDING.md
  - Workspace-Struktur-Tabelle übernehmen, um `.claude/agents/` erweitern mit Beschreibung „Optional: eigene Subagents"
  - Commands-Abschnitt: aktuelle vier Commands übernehmen, Beschreibungen aber entpersonalisiert
  - „Kritische Anweisung: Diese Datei pflegen" übernehmen (ist generisch)
  - Abschluss-Abschnitt: Verweis auf `ONBOARDING.md` für tiefergehende Erklärungen

**Betroffene Dateien:**

- `Vorlagen/claude-workspace-vorlage-team/CLAUDE.md` (neu)

---

### Schritt 7: `ONBOARDING.md` schreiben

**Aktionen:**

- Datei `Vorlagen/claude-workspace-vorlage-team/ONBOARDING.md` erstellen
- Struktur:
  1. **Was ist ein Workspace?** — Definition, Zweck, Unterschied zu einem „normalen" Projektordner (CLAUDE.md wird automatisch geladen)
  2. **Warum context/ ausfüllen?** — Ohne Kontext → generische Antworten. Mit Kontext → Claude weiß, wer du bist, was du tust, welche Ziele, welche Daten. Jede Datei einmal erklärt: personal-info, business-info, strategy, current-data
  3. **Spezialisierte Workspaces: Ein Ordner pro Thema** — Praktisches Muster: Vorlage klonen → Thema konfigurieren → per Shell-Alias starten. Beispiele (fiktiv, generisch): „Kundenprojekt Alpha", „Lernprojekt Rust", „Private Dokumentation". Jeder Workspace hat sein eigenes `context/`. Im selben Terminal arbeitest du immer im aktuell geöffneten Workspace.
  4. **Workspaces vs. Subagents** — Klare Tabelle, kurze Erklärung, Verweis auf offizielle Doku (`https://docs.claude.com/en/docs/claude-code/sub-agents`). Wann brauche ich einen Subagent? Wenn eine spezifische Teilaufgabe in jeder Session wiederkehrt (z.B. „Code-Reviewer" oder „Testsuite-Runner") und ein eigener System-Prompt + eigene Tools sinnvoll sind. Subagents liegen in `.claude/agents/<name>.md`.
  5. **Plan-first-Arbeitsweise: Warum `/create-plan` vor `/implement`?** — Ausführlich: Kostenstruktur (Plan = wenige Tokens, Implementierung = viele Tokens), Wert der Review-Schleife, Risiko ohne Plan (Fehlrichtung, wiederholte Arbeit). Typischer Ablauf: Aufgabe formulieren → `/create-plan X` → Plan lesen, Fragen/Änderungen → `/implement plans/…` → `/shutdown` am Ende.
  6. **Shell-Aliase:** Verweis auf `shell-aliases.md`
  7. **Troubleshooting:** Häufige Stolpersteine („Claude weiß meinen Namen nicht" → context/ nicht ausgefüllt; „Commands funktionieren nicht" → falscher Ordner; etc.)
- Länge: ca. 150–250 Zeilen, keine Beispiele aus Felix' Welt

**Betroffene Dateien:**

- `Vorlagen/claude-workspace-vorlage-team/ONBOARDING.md` (neu)

---

### Schritt 8: Context-Templates schreiben

**Aktionen:**

- Vier Dateien in `Vorlagen/claude-workspace-vorlage-team/context/` erstellen: `personal-info.md`, `business-info.md`, `strategy.md`, `current-data.md`
- Struktur jeder Datei folgt dem bestehenden Muster (Überschriften, Einleitung „Wie das zusammenhängt", dann Abschnitte)
- Inhalt: Platzhalter-Felder mit `<!-- AUSFÜLLEN: Beschreibung -->` als Kommentar, keine Beispieldaten aus Felix' Welt
- Jede Datei beginnt mit einer klaren Frage, die sie beantwortet, und einer Schätzung, wie lange das Ausfüllen dauert
- `personal-info.md`: Rolle, Hauptverantwortlichkeiten, Arbeitsweise/Präferenzen, wie dieser Workspace unterstützt
- `business-info.md`: Organisation/Projekt/Kontext, Produkte/Dienstleistungen/Inhalte, wichtiger Kontext (ggf. zweite Organisation für Nebenprojekte — optional)
- `strategy.md`: Fokuszeitraum, strategische Prioritäten (Slots 1–4), Erfolgskriterien, offene Fragen
- `current-data.md`: Schlüsselmetriken (Tabellen), aktueller Stand, Datenquellen, Automatisierungshinweis
- In jeder Datei am Ende ein Hinweis: „Halte diese Datei aktuell — veralteter Kontext führt zu schlechten Antworten."

**Betroffene Dateien:**

- `Vorlagen/claude-workspace-vorlage-team/context/personal-info.md`
- `Vorlagen/claude-workspace-vorlage-team/context/business-info.md`
- `Vorlagen/claude-workspace-vorlage-team/context/strategy.md`
- `Vorlagen/claude-workspace-vorlage-team/context/current-data.md`

---

### Schritt 9: `.claude/agents/README.md` schreiben

**Aktionen:**

- Datei `Vorlagen/claude-workspace-vorlage-team/.claude/agents/README.md` erstellen
- Inhalt (ca. 20–40 Zeilen):
  - Was dieser Ordner ist: optionale projektlokale Subagents
  - Kurzes Format-Beispiel (Markdown mit YAML-Frontmatter)
  - Link zur offiziellen Doku
  - Hinweis: Für den Start des Workspaces nicht nötig; erst relevant, wenn du wiederkehrende Teilaufgaben delegieren willst

**Betroffene Dateien:**

- `Vorlagen/claude-workspace-vorlage-team/.claude/agents/README.md` (neu)

---

### Schritt 10: Verifikation der Entpersonalisierung

**Aktionen:**

- `grep -ri "Felix\|SteelcoBelimed\|Belimed\|Willich\|lord3000\|felix.versen\|AI-Telefonanlage\|Felddiagnostik\|SAP FSM\|SmartHub\|InsightLoop\|OSticket\|ProServ\|Herford" Vorlagen/claude-workspace-vorlage-team/` — muss **null** Treffer liefern
- Falls Treffer: identifizieren, beheben, erneut grep

**Betroffene Dateien:**

- Potenziell alle kopierten Dateien (erwartet: null Treffer)

---

### Schritt 11: Git-Initialisierung (eigenes, sauberes Repo)

**Aktionen:**

- Im neuen Ordner `git init` — **ohne** Remote, **ohne** Verbindung zum persönlichen Repo von Felix
- Alle Dateien stagen (`git add .`)
- Initial-Commit: „Initial: Claude Workspace Vorlage (Team)"
- Verifizieren: `git remote -v` gibt nichts aus, `git log --oneline` zeigt genau einen Commit

**Betroffene Dateien:**

- `.git/` im neuen Ordner

---

### Schritt 12: Smoke-Test

**Aktionen:**

- Strukturell: `find Vorlagen/claude-workspace-vorlage-team -type f | sort` — vergleiche gegen die Soll-Dateiliste aus Schritt 1–9
- Manueller Laufzeittest (durch Felix, nicht Teil von `/implement`): In den neuen Ordner wechseln, `claude` starten, `/prime` ausführen → Onboarding-Modus sollte aktiv sein

**Betroffene Dateien:**

- Keine — nur Verifikation

---

### Schritt 13: ZIP-Paket erstellen

**Aktionen:**

- Aus `Vorlagen/` heraus: `zip -r claude-workspace-vorlage-team.zip claude-workspace-vorlage-team/ -x "claude-workspace-vorlage-team/.git/*"`
- Die `.git/`-Historie wird ausgeschlossen, damit Empfänger einen frischen Start haben (sie können selbst `git init` machen oder mit dem vorhandenen Initial-Commit weiterarbeiten — siehe unten)
- **Entscheidung:** `.git/` aus der ZIP ausschließen. Begründung: Das Repo enthält nur einen Initial-Commit ohne Mehrwert für den Empfänger, und manche Empfänger wollen sowieso sofort ihr eigenes Remote konfigurieren. Sie können beim Entpacken selbst `git init` ausführen.
- Ziel-Dateipfad: `Vorlagen/claude-workspace-vorlage-team.zip`
- Nach Erstellung: `unzip -l Vorlagen/claude-workspace-vorlage-team.zip | head -30` zur Kontrolle

**Betroffene Dateien:**

- `Vorlagen/claude-workspace-vorlage-team.zip` (neu)

---

## Verbindungen & Abhängigkeiten

### Dateien, die diesen Bereich referenzieren

- `claude-workspace-vorlage/CLAUDE.md` erwähnt den Workspace als Template für den Download durch andere — könnte in einer späteren Session aktualisiert werden, um auf die Team-Vorlage zu verweisen (nicht Teil dieses Plans)
- Kein bestehendes Dokument verweist aktiv auf den neuen Ordner; der neue Workspace ist eigenständig

### Nötige Updates für Konsistenz

- Keine externen Updates nötig. Alle Querverweise liegen innerhalb des neuen Ordners und werden beim Schreiben der Dateien mit korrekten Pfaden angelegt.

### Auswirkungen auf bestehende Workflows

- Keine. Felix' bestehender Workspace ist unberührt. Shutdown/Prime/Create-Plan/Implement funktionieren dort unverändert.

---

## Validierungs-Checkliste

- [ ] Ordner `Vorlagen/claude-workspace-vorlage-team/` existiert mit vollständiger Unterordnerstruktur (`context`, `plans`, `outputs`, `reference`, `scripts`, `.claude/commands`, `.claude/skills/mcp-integration`, `.claude/skills/skill-creator`, `.claude/agents`)
- [ ] `CLAUDE.md` enthält keinen persönlichen Namen, Arbeitgeber, Projektnamen aus Felix' Kontext
- [ ] `ONBOARDING.md` existiert und deckt alle sieben geplanten Abschnitte ab
- [ ] Alle vier `context/`-Dateien existieren, enthalten Platzhalter-Kommentare und **keine** Beispieldaten aus Felix' Welt
- [ ] Alle vier `.claude/commands/`-Dateien existieren; `prime.md` enthält den Onboarding-Branch mit den sechs Punkten aus Schritt 5
- [ ] `.claude/agents/README.md` existiert und erklärt den Ordnerzweck
- [ ] `.claude/skills/mcp-integration/` und `.claude/skills/skill-creator/` sind vollständig kopiert (alle Unterdateien)
- [ ] `.claude/settings.local.json` wurde **nicht** kopiert
- [ ] `shell-aliases.md` und `.gitignore` existieren
- [ ] `grep`-Test auf persönliche Marker liefert null Treffer
- [ ] `CLAUDE.md` erwähnt `ONBOARDING.md` und den neuen Abschnitt „Wie du dieses Template verwendest"
- [ ] `plans/`, `outputs/`, `reference/`, `scripts/` enthalten jeweils eine `.gitkeep`

---

## Erfolgskriterien

Die Implementierung ist abgeschlossen, wenn:

1. Ein Kollege kann den Ordner `claude-workspace-vorlage-team/` klonen, sofort `claude`/`/prime` ausführen und bekommt eine verständliche Einführung — ohne vorher Felix zu fragen, was Workspaces, Subagents oder `/create-plan` sind.
2. Der Kollege versteht nach dem Onboarding-Flow, dass er (a) mehrere Themen-Workspaces parallel haben kann und (b) `/create-plan` vor `/implement` nutzen sollte.
3. Nach dem Ausfüllen der vier `context/`-Dateien liefert `/prime` einen personalisierten Zusammenfassungs-Output für den neuen Benutzer (wie bei Felix heute, nur mit dessen eigenen Daten).
4. `grep -ri` auf persönliche Marker (siehe Schritt 10) liefert null Treffer.
5. Felix' bestehender Workspace (`claude-workspace-vorlage/`) ist inhaltlich identisch zum Zustand vor der Implementierung.

---

## Notizen

- **Spätere Verbreitung:** Nach der Implementierung stellt sich die Frage, wie Kollegen an das Template kommen (GitHub-Template-Repo, Zip-Download, interne Wiki-Anleitung). Das ist bewusst außerhalb des Scope dieses Plans — siehe offene Frage 5.
- **Nicht Teil dieses Plans, aber denkbar für später:** Ein optionaler Beispiel-Subagent (z.B. ein simpler „Dokumentations-Reviewer"), um das Subagent-Feature greifbar zu machen. Könnte in einem Folge-Plan ergänzt werden.
- **Synergie mit `claude-workspace-SPS-Programmierer`:** Dieser existierende Themen-Workspace zeigt, dass Felix das Multi-Workspace-Muster bereits lebt. In ONBOARDING.md kann man ihn als anonymisiertes Muster-Beispiel erwähnen („ein Kollege hat beispielsweise einen eigenen Workspace für Speicherprogrammierbare-Steuerungs-Projekte") — oder eben nicht, um es generisch zu halten. Vorschlag: nicht erwähnen, generisch bleiben.
- **Token-Argument für `/create-plan`:** Das explizite Argument im Onboarding ist ein wichtiger Verkaufspunkt — Kollegen, die mit teureren Modellen (Opus) arbeiten, sparen reales Geld durch Planung vorab. Das sollte in ONBOARDING.md ein kleines Rechenbeispiel wert sein (z.B. „Plan: ~2.000 Tokens, Implementierung: ~30.000 Tokens — Planungsfehler ohne Plan = 30.000 Tokens verbrannt").

---

## Implementierungsnotizen

**Implementiert:** 2026-04-27

### Zusammenfassung

Neuer Workspace `Vorlagen/claude-workspace-vorlage-team/` vollständig angelegt:

- 29 Dateien, vollständige Ordnerstruktur (context, plans, outputs, reference, scripts, .claude/{commands,agents,skills})
- `CLAUDE.md` — entpersonalisiert, mit neuen Abschnitten „Wie du dieses Template verwendest", „Workspace ≠ Subagent", „Warum `/create-plan` vor `/implement`?"
- `ONBOARDING.md` — 7 Abschnitte (Was ist ein Workspace, context-Sinn, Themen-Workspaces, Workspaces vs. Subagents inkl. Vergleichstabelle, Plan-first mit Rechenbeispiel, Shell-Aliase, Troubleshooting)
- `prime.md` — zwei-modig: Onboarding-Modus (sechs Punkte: Willkommen → Workspace ≠ Subagent → Themen-Workspaces → context/-Pflicht → Plan-first → nächster Schritt) und Standard-Modus
- 4 Context-Templates mit `<!-- AUSFÜLLEN: ... -->`-Markern, ohne Beispieldaten aus dem persönlichen Workspace
- `.claude/agents/README.md` als Hinweis auf optionale Subagents mit Format-Beispiel und Doku-Link
- Generische Übernahme: `create-plan.md`, `implement.md`, `shutdown.md`, `mcp-integration/`, `skill-creator/`, `shell-aliases.md`, `.gitignore`
- Eigenes, sauberes Git-Repo mit Initial-Commit, **kein** Remote
- ZIP-Paket `Vorlagen/claude-workspace-vorlage-team.zip` (~70 KB) ohne `.git/`-Historie

### Abweichungen vom Plan

- `.gitignore` zusätzlich um `__pycache__/`, `*.pyc`, `*.log`, `*.tmp`, `*.bak`, `.claude/settings.local.json` erweitert (Plan erwähnte „falls relevant" — schadet nicht, kollegenfreundlich).
- Initial-Commit mit lokalem Pseudo-User `template@local` / `Template`, damit Felix' globale git-Identität nicht in das Initial-Commit-Metadatum geschrieben wird (passt zu „nicht mit meinen repos").

### Aufgetretene Probleme

Keine. Alle Validierungs-Checks (grep auf persönliche Marker = 0 Treffer, Dateiliste vollständig, ZIP-Kontrolle ohne `.git/`) bestanden.

### Smoke-Test

Strukturell verifiziert per `find` und `unzip -l`. Laufzeittest (`claude /prime` im neuen Ordner) bewusst dem User überlassen — nicht Teil von `/implement`.
