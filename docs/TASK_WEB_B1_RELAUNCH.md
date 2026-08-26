# TASK WEB-B1 · RELAUNCH LAGONORD.DE

**Art:** Umsetzung. Die neue Startseite liegt fertig in diesem Paket — der Task
ist Einbau, Prüfung und Bericht, **kein Gestalten und kein Texten**.

**Repo:** `~/Development/lagonord-website`
**Branch:** `web-relaunch-v6` — frisch von `main` abzweigen. `main` wird nie
direkt berührt; den Merge macht Tim nach Diff-Review, ausschließlich `--ff-only`.
**Arbeitsprotokoll:** `FORTSCHRITT.md` im Repo-Wurzelverzeichnis auf dem
Arbeitsbranch, fortlaufend mit Zeitstempeln.

---

## Ampel

- 🟢 Erlaubt: die unten aufgeführten Datei-Operationen; `FORTSCHRITT.md`;
  Commits auf `web-relaunch-v6`.
- 🟡 Escalieren (in `FORTSCHRITT.md` notieren, Annahme kennzeichnen,
  weiterarbeiten wo möglich): unerwarteter Repo-Zustand, Namenskollisionen,
  fehlgeschlagene Prüfschritte.
- 🔴 Verboten, ohne Ausnahme: `CNAME` anfassen; irgendeine Datei **löschen**;
  `imprint.html`/`privacy.html` über Schritt 4 hinaus verändern; Texte, Farben
  oder Layout der gelieferten Dateien ändern; auf `main` arbeiten; force-push;
  Werkzeuge nachinstallieren; Netzwerkzugriffe außer `git push`.

## Gelieferte Dateien (in diesem Paket, Verzeichnis `site/`)

    index.html                     neue Startseite, dreisprachig, in sich geschlossen
    sprachen.js                    Übersetzungen IT/EN — redigiert nur Cristina
    logo_kupfer.svg                Sperrung Kupfer (dunkler Grund)
    logo_hell.svg / logo_dunkel.svg  weitere Sperrungsfassungen
    broschuere_assistenti_de.pdf   verlinkt aus der Seite (DE/EN)
    broschuere_assistenti_it.pdf   verlinkt aus der Seite (IT)
    assets/cristina.jpg · tim.jpg · luna.jpg   Teamporträts (identisch zum Bestand)

## Schritte

1. **Vorbereitung.** `git -C ~/Development/lagonord-website status` muss sauber
   sein — sonst 🟡 stoppen und melden. Dann `git checkout -b web-relaunch-v6`.
2. **Einbau.** Alle Dateien aus `site/` ins Repo-Wurzelverzeichnis kopieren.
   `index.html` wird dabei überschrieben — das ist beabsichtigt; die alte
   Fassung bleibt über die Git-Historie erhalten. Nichts wird gelöscht;
   nicht mehr genutzte Bilder in `assets/` bleiben unangetastet liegen.
3. **Prüfungen nach dem Einbau** (jede in `FORTSCHRITT.md` mit Ergebnis):
   - `grep -ri "gardaunica" index.html` → **0 Treffer**
   - `grep -ri "fonts.googleapis\|fonts.gstatic" index.html` → **0 Treffer**
   - `ls broschuere_assistenti_de.pdf broschuere_assistenti_it.pdf sprachen.js logo_kupfer.svg` → alle vorhanden
   - `index.html` lokal im Browser öffnen: Seite lädt ohne Netz-Requests auf
     fremde Domains (Netzwerk-Tab), Sprachumschalter DE→IT wechselt Überschrift
     und Broschüren-Link auf die IT-PDF, DE→EN wechselt Titelzeile des Tabs.
4. **Rechtsseiten, minimal-invasiv.** In `imprint.html` und `privacy.html`
   ausschließlich die `<link>`-Zeilen entfernen, die `fonts.googleapis.com`
   oder `fonts.gstatic.com` laden, und im dortigen CSS `Poppins` durch
   `system-ui, sans-serif` ersetzen. **Keine weitere Änderung** — die
   inhaltliche Neufassung der Datenschutzerklärung macht Tim gesondert.
   Danach: `grep -ri "fonts.googleapis\|fonts.gstatic" *.html` → **0 Treffer**.
5. **Commits.** Zwei getrennte Commits nach der Zwei-Commit-Regel:
   erst der Einbau (Schritte 2–3), dann die Rechtsseiten-Bereinigung (Schritt 4).
   Aussagekräftige Meldungen auf Deutsch.
6. **Bericht.** `FORTSCHRITT.md` abschließen mit `TASK ABGESCHLOSSEN`, drei
   Sätzen Zusammenfassung, der Commit-Liste und dem Hinweis, dass der Merge
   auf `main` bei Tim liegt.

## Nicht Teil dieses Tasks (bekannt, absichtlich offen)

- Neufassung von `privacy.html` (Tim, vor aktiver Bewerbung der Seite)
- Aufräumen ungenutzter Alt-Bilder in `assets/` (späterer Task)
- Speichern der Sprachwahl über Seitenwechsel hinweg
- Englische Broschüre (der EN-Knopf verweist bewusst auf die deutsche PDF
  und sagt das im Beschriftungstext)
