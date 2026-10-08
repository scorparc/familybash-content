# familybash-content

Öffentliches Content-Repo für die App **FamilyBash** (`de.familybash.app`).
Hier liegen die versionierten Content-Packs, die die App **offline-first** nutzt und bei
Bedarf **ohne App-Release** nachlädt.

## Struktur
```
content/
  manifest.json          Liste aller Packs mit revision + remoteBaseUrl
  <mode>.<lang>.json      z.B. impostor.de.json, quiz.en.json
```

Schema aller Dateien: siehe `CONTENT_SCHEMA.md` in der App.

## So funktioniert das Update
1. Die App liest beim Öffnen ihre **gebündelten** Packs (Stand zum Release).
2. In den Einstellungen → „Inhalte aktualisieren" holt sie `content/manifest.json` von
   der in der App hinterlegten `remoteBaseUrl`
   (`https://raw.githubusercontent.com/scorparc/familybash-content/main/content/`).
3. Für jedes Pack mit **höherer `revision`** als lokal wird die Datei geladen, validiert
   und im lokalen Cache gespeichert (überschreibt das gebündelte Pack).

## Content ändern / ergänzen
- Item hinzufügen/ändern → im jeweiligen `<mode>.<lang>.json` und dessen `revision`
  **hochzählen** (und denselben Wert im `manifest.json` setzen). Nur dann zieht die App das Update.
- **Neue Sprache**: `<mode>.<lang>.json` anlegen + Eintrag in `manifest.json`. Keine App-Änderung nötig.
- **Neues Pack/Feiertags-Pack**: analog; `tier: "pro"` für kostenpflichtige Inhalte.

## Einrichtung (einmalig, durch Marc)
```bash
# Repo erstellen (GitHub-Konto scorparc) und pushen:
gh repo create scorparc/familybash-content --public --source=. --push
```
Danach reicht `git push` nach jeder Content-Änderung – die App zieht Updates automatisch.

> Hinweis: Dieses Verzeichnis ist ein **Staging-Ordner** innerhalb des App-Projekts. Für den
> Go-Live den Inhalt in ein eigenes GitHub-Repo `scorparc/familybash-content` übertragen.
