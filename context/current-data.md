# Aktuelle Daten

> Diese Datei enthält Metriken, Datenpunkte und aktuelle Statusinformationen, die für deine Rolle und Strategie relevant sind. Sie liefert Claude konkreten Kontext für Analysen und Entscheidungsfindung.

---

## Wie das zusammenhängt

- **business-info.md** liefert den organisatorischen Kontext
- **personal-info.md** definiert, wofür du verantwortlich bist
- **strategy.md** beschreibt, worauf du optimierst
- **Diese Datei** gibt Claude die Zahlen hinter der Erzählung

---

## Schlüsselmetriken Hauptjob

| Metrik | Aktueller Wert | Zielwert | Notizen |
| ------ | -------------- | -------- | ------- |
| Teamgröße 2nd Level | 6–10 | — | Region West |
| First-Fix-Rate (FTR) | <!-- WERT EINTRAGEN --> | <!-- ZIEL --> | Wird unternehmensweit erfasst, nicht teamspezifisch |
| Kundenzufriedenheit | <!-- WERT EINTRAGEN --> | <!-- ZIEL --> | Wird unternehmensweit erfasst, nicht teamspezifisch |
| Schulungsstand Team | Digitalisierung läuft | 100% dokumentiert | OCR-Transkription der Teilnahmelisten abgeschlossen |
| Offene Eskalationen | <!-- WERT EINTRAGEN --> | — | — |

> **Hinweis:** FTR und Kundenzufriedenheit werden nicht auf Teamebene gemessen, sondern unternehmensweit. Trotzdem relevant als Orientierung — das Team kann und will daran mitwirken.

---

## Schlüsselmetriken Side Business

### AI-Telefonanlage

| Metrik | Aktueller Wert | Zielwert | Notizen |
| ------ | -------------- | -------- | ------- |
| Warme Leads | 2 | — | Konkrete Interessenten identifiziert |
| MVP-Status | Konzept steht | Einsatzbereit | Asterisk + n8n + GPT-4o Realtime |
| Zahlende Kunden | 0 | 1 | Erster Pilotkunde = Hauptziel |
| UG gegründet | Nein | Ja | Steht noch aus |
| Monatl. Zeitinvest | <!-- STUNDEN/WOCHE --> | — | Begrenzt durch Hauptjob + Familie |

### Felddiagnostik-Device

| Metrik | Aktueller Wert | Zielwert | Notizen |
| ------ | -------------- | -------- | ------- |
| Konzeptdokument | Fertig | — | Hardware, Software, Regulatorik, Roadmap |
| Prototyp-Hardware | Ausstehend | Bestellt/getestet | Unipi Neuron oder RevPi Connect |
| Software-Grundgerüst | Noch nicht begonnen | Lauffähig | — |
| Erste Feldtests | — | 1 Test | Im eigenen Arbeitsumfeld möglich |

---

## Aktueller Stand

### Hauptjob
- Schulungs-Digitalisierung: OCR/Transkription über mehrere Trainingsmodule in strukturierte Excel-Datei abgeschlossen
- AI-Workflow-Stack aktiv im Einsatz: Telegram Bot, Whisper, Claude Code mit Custom Commands, Playwright MCP für SAP FSM / SmartHub Orbit / OSticket / SteelcoBelimed Desk
- Klinikum-spezifische Analyse-Prompts erstellt (z.B. Klinikum Herford, ProServ Pulheim RFID-Störung)

### Side Business
- AI-Telefonie: Technisches Konzept steht, zwei Leads identifiziert, kein MVP im Einsatz
- Felddiagnostik: Umfassendes Konzeptdokument erstellt, Hardware noch nicht bestellt
- UG-Gründung: Noch nicht begonnen
- RS232-Datenlogger (Raspberry Pi): Komponenten bestellt, Lieferung ausstehend

### Infrastruktur
- Mac Mini: Telegram Bot (Lord3000bot), Whisper, TTS via macOS `say`
- Debian-Server: VMs, OpenClaw, Cloudflare Tunnel (Netzwerk-Outage durch Bridge-Fehlkonfiguration gelöst)
- SAIA PLC Datenlogger: 9-Dateien-Softwarepaket fertig (S-Bus/UDP, SQLite, FastAPI, Web-Dashboard)

---

## Datenquellen

| Quelle | Was | Zugriff |
| ------ | --- | ------- |
| SAP FSM | Einsatzdaten, Servicehistorien, Plantafel | Web (de.fsm.cloud.sap) |
| SmartHub Orbit | Live-Maschinendaten, Chargen, Alarme | Web (smarthuborbit.belimed.com) |
| InsightLoop | KI-aufbereitetes Servicewissen | Web (assistant.insightloop.com) |
| OSticket | Support-Tickets, Knowledgebase | Web (support.belimed.com) |
| SteelcoBelimed Desk | Dokumentation, Ersatzteile, Schaltpläne | Web (desk.steelcogroup.com) |
| Outlook | E-Mail-Kommunikation | Web (outlook.office.com) |

---

## Automatisierungshinweis

_Diese Datei funktioniert als statischer Snapshot. Potenzial für Automatisierung:_
- _SAP FSM Plantafel-Scraper existiert bereits (Trainingsblöcke extrahieren)_
- _SmartHub Orbit Daten könnten per Playwright MCP automatisch gezogen werden_
- _Schulungsstände könnten aus der digitalisierten Excel automatisch aktualisiert werden_

---

_Regelmäßig aktualisieren — veraltete Daten begrenzen Claudes Nützlichkeit als analytischer Partner._
