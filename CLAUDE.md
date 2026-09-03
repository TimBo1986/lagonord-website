# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Was das hier ist

Marketing-Website von LagoNord AI (Peschiera del Garda) — **statisches HTML ohne
Build-Schritt, ohne Paketmanager, ohne Tests**. Kein `npm`, kein Bundler, keine
CI. Was im Repo liegt, ist exakt das, was ausgeliefert wird.

Deployment: **GitHub Pages** aus `main` (Repo `TimBo1986/lagonord-website`),
Custom Domain über `CNAME` → `www.lagonord.de`. Ein Push auf `main` ist das
Deployment.

## Arbeiten und Prüfen

```sh
open index.html                      # reicht — alle Pfade sind repo-relativ, file:// funktioniert
python3 -m http.server 8000          # falls ein echter Origin gebraucht wird

# Die Prüfungen, die in diesem Repo zählen:
grep -ri "fonts.googleapis\|fonts.gstatic" *.html    # muss 0 Treffer geben
grep -rn "http://\|https://" index.html sprachen.js  # muss 0 Treffer geben — nichts wird extern geladen
```

**Achtung bei `grep`/`sed` auf `index.html`:** Zeilen 8–14 enthalten die
base64-eingebetteten Schriften (~140 KB in sechs Zeilen). Ausgabe immer
begrenzen (`cut -c1-200`) oder ab Zeile 15 lesen.

## Aufbau von `index.html` (968 Zeilen, in sich geschlossen)

| Zeilen | Inhalt |
|---|---|
| 8–14 | `<style id="schrift">` — `@font-face` mit base64-WOFF2: Sora 300/400/500/600, JetBrains Mono 400/500 |
| 15–437 | gesamtes CSS, Design-Tokens in `:root` |
| 440–783 | Markup: Kopf, Opener, Leistungen, Vorführung, Ablauf, Warum, Team, FAQ, Kontakt, Bio-Popup |
| 785–966 | gesamtes JS in einer IIFE |

Externe Ressourcen sind **bewusst ausgeschlossen** — Schriften sind eingebettet,
nicht von Google Fonts geladen. Keine CDN-Links, keine Web-Fonts von fremden
Domains, keine Analytics hinzufügen; das war eine explizite Anforderung
(siehe `docs/TASK_WEB_B1_RELAUNCH.md`, Schritt 4). Dasselbe gilt für
`imprint.html` / `privacy.html`.

## Sprachumschaltung — der zentrale Mechanismus

Deutsch ist die Quelle und steht direkt im HTML. `sprachen.js` bildet es auf
IT/EN ab. Beim Laden sammelt das Skript (index.html:900 ff.):

- **Textknoten** über einen `TreeWalker` → Wörterbuch `t`
- **Blöcke mit Auszeichnung**, ausgewählt durch
  `HTML_WAHL = 'h1,h2,.msg,.geraet-notiz,.fondamento p'` → Wörterbuch `h`
  (ganzes `innerHTML` inkl. `<b>`-Tags als Schlüssel)

**Der Schlüssel ist der exakte deutsche Wortlaut**, whitespace-normalisiert
(`norm()` = `replace(/\s+/g,' ').trim()`). Daraus folgt die wichtigste Regel des
Repos:

> Jede Änderung an einem deutschen Text in `index.html` bricht still seine
> Übersetzung — der Schlüssel passt nicht mehr, `zeigeSprache()` fällt auf
> Deutsch zurück, ohne Fehler. Wer deutschen Text ändert, muss den Schlüssel in
> **beiden** Wörterbüchern (`it` und `en`) in `sprachen.js` mitziehen.

Zusätzlich in `sprachen.js`: `window.LAGONORD_BIOS` (`it`/`en`, Schlüssel `c`/`t`/`l`)
für die Team-Popups; die deutschen Bios stehen als `BIO`-Objekt im Skript von
`index.html`.

`zeigeSprache(lang)` setzt außerdem `document.documentElement.lang`, `document.title`
und `meta[description]` aus der inline `META`-Tabelle und tauscht den
Broschüren-Link: IT → `broschuere_assistenti_it.pdf`, DE **und EN** →
`broschuere_assistenti_de.pdf` (die englische Broschüre existiert absichtlich
nicht, der Beschriftungstext sagt das). Die Sprachwahl wird **nicht** über
Seitenwechsel hinweg gespeichert — bekannt und offen.

Cristina redigiert Übersetzungen in `sprachen.js`, nicht im HTML.

## Weitere Bausteine im Skript

- **Vorführung:** Reiter `button[data-ziel]` schalten `.geraet[data-geraet]` und
  `.bandszene[data-band]` gemeinsam; `spiele()` lässt den Chat neu einlaufen.
- **Einblendungen:** Elemente mit `.zeig` bekommen per `IntersectionObserver` die
  Klasse `.da`; die Ablauf-Punkte `.pn` leuchten gestaffelt auf.
- `prefers-reduced-motion` wird überall respektiert (`var sanft`) — bei neuen
  Animationen mitziehen: alles sofort im Endzustand zeigen statt animieren.

## Rechtsseiten

`imprint.html` und `privacy.html` sind eigenständig, mit eigenem Inline-CSS,
`meta robots=noindex`, nur auf Deutsch, ohne `sprachen.js`. Schrift ist dort
`--f: system-ui, sans-serif`. Die inhaltliche Neufassung von `privacy.html`
macht Tim selbst — nicht ungefragt umschreiben.

## Konventionen

- **Bezeichner und Kommentare sind deutsch**, im JS (`zeigeSprache`, `knoepfe`,
  `geraete`, `bloecke`, `sanft`, `laeuft`, `zu`/`auf`) wie in den CSS-Variablen
  (`--rame`, `--carta`, `--notte`, `--linie`). Neuer Code folgt dem.
- **Commit-Meldungen auf Deutsch.**
- Nie direkt auf `main` arbeiten: Feature-Branch, Merge macht Tim nach
  Diff-Review mit `--ff-only`.
- Für gelieferte Aufträge liegt die Spezifikation unter `docs/TASK_*.md` und das
  fortlaufende Arbeitsprotokoll in `FORTSCHRITT.md` (mit Zeitstempeln,
  Prüftabellen, Abschluss mit `TASK ABGESCHLOSSEN`).
- `CNAME` bleibt unangetastet — sonst fällt die Custom Domain aus.

## Bekannt und absichtlich offen

- Ungenutzte Alt-Bilder in `assets/` (`gardaunica.jpg`, `hero.jpg`,
  `lagoestate.jpg`, `lagosocial.jpg`, `lagostay.jpg`, `logo.jpg`) — bleiben
  liegen, Aufräumen ist ein späterer Task. Genutzt werden nur `cristina.jpg`,
  `tim.jpg`, `luna.jpg`.
- Speichern der Sprachwahl über Seitenwechsel hinweg.
- Englische Broschüre.
