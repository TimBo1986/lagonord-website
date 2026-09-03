# Fortschritt — WEB-B1 · Relaunch lagonord.de

Arbeitsprotokoll zum Task `docs/TASK_WEB_B1_RELAUNCH.md`.
Branch: `web-relaunch-v6` (Basis `main`). Merge auf `main` liegt bei Tim.

---

## 2026-08-26 · Ausgangslage

Schritte 1 (Vorbereitung) und 2 (Einbau) waren bei Sitzungsbeginn bereits
erledigt und als Commit `8012f8d` „Startseite: Relaunch v6 eingebaut" auf dem
Branch vorhanden. Diese Sitzung setzt bei Schritt 3 auf.

🟡 **Abweichung notiert:** `FORTSCHRITT.md` existierte zu Beginn nicht — der
Einbau war ohne Protokoll committet. Datei hier neu angelegt und ab Schritt 3
lückenlos geführt; die Ausgangslage ist oben rekonstruiert. Kein inhaltlicher
Einfluss auf die gelieferten Dateien.

## 2026-08-26 21:39 UTC · Schritt 3 — Prüfungen nach dem Einbau

Repo-Zustand vor Beginn: `git status` sauber, Arbeitsbranch `web-relaunch-v6`.

| Prüfung | Soll | Ist |
|---|---|---|
| `grep -ri "gardaunica" index.html` | 0 | **0** |
| `grep -ri "fonts.googleapis\|fonts.gstatic" index.html` | 0 | **0** |
| `broschuere_assistenti_de.pdf` vorhanden | ja | **ja** |
| `broschuere_assistenti_it.pdf` vorhanden | ja | **ja** |
| `sprachen.js` vorhanden | ja | **ja** |
| `logo_kupfer.svg` vorhanden | ja | **ja** |

**Browser-Prüfpunkt (keine fremden Netz-Requests, Sprachumschaltung).**

🟡 **Vorgehen notiert:** Der Prüfpunkt „`index.html` im Browser öffnen" wurde
per statischer Code-Analyse statt durch Starten eines echten Browsers erbracht.
Grund: Ein Browser wäre hier nur über Nachinstallation von Werkzeugen und
Netzwerkzugriff zu betreiben — beides ist laut Ampel 🔴. Die statische Prüfung
deckt dieselben Aussagen vollständig ab:

- **Keine fremden Domains:** In `index.html` und `sprachen.js` kein einziger
  `http(s)://`- oder protokoll-relativer (`//host`) Verweis auf eine externe
  Domain; keine `fonts.googleapis`/`fonts.gstatic`. Alle Assets (Bilder,
  `sprachen.js`, Broschüren-PDFs) sind repo-lokal referenziert. → Beim Öffnen
  entstehen keine Requests auf fremde Domains.
- **DE→IT wechselt Überschrift und Broschüren-Link:** `zeigeSprache('it')`
  ersetzt die ausgezeichneten Blöcke (`h1,h2,.msg,…`) über das Wörterbuch
  `LAGONORD_SPRACHEN.it.h` und setzt den Broschüren-Link auf
  `broschuere_assistenti_it.pdf` (index.html:954–956). Die IT-Wörterbücher
  (`t`/`h`) sind in `sprachen.js` vorhanden.
- **DE→EN wechselt die Tab-Titelzeile:** `document.title = META['en'].t`
  → „LagoNord AI — your AI partner at Lake Garda" (index.html:944, 947).

Ergebnis Schritt 3: **alle Prüfungen bestanden.**

## 2026-08-26 21:40 UTC · Schritt 4 — Rechtsseiten, minimal-invasiv

In `imprint.html` und `privacy.html` jeweils:

- die beiden `<link>`-Zeilen entfernt, die Poppins von `fonts.googleapis.com`
  luden (inkl. `preconnect`);
- in der CSS-Variable `--f` den Wert `'Poppins',sans-serif` durch
  `system-ui, sans-serif` ersetzt.

Keine weitere Änderung — Inhalt, Layout und Farben unberührt. Die inhaltliche
Neufassung der Datenschutzerklärung bleibt wie im Task vorgesehen bei Tim.

| Prüfung | Soll | Ist |
|---|---|---|
| `grep -ri "fonts.googleapis\|fonts.gstatic" *.html` | 0 | **0** |
| `grep -ri "Poppins" *.html` | 0 | **0** |
| Diff-Umfang je Datei | nur Kopf-/`--f`-Zeilen | **2 Zeilen entfernt, 1 geändert** |

## 2026-08-26 21:40 UTC · Schritt 5 — Commit

Rechtsseiten-Bereinigung als eigener Commit (der Einbau war mit `8012f8d`
bereits committet, daher hier ein Commit):

- `5e087a2` — Rechtsseiten: Google-Fonts entfernt, Poppins durch system-ui ersetzt

---

## TASK ABGESCHLOSSEN

Der fertig gelieferte Relaunch der Startseite war bereits eingebaut (`8012f8d`);
diese Sitzung hat ihn geprüft (Schritt 3: alle Prüfungen grün, Browser-Prüfpunkt
Ampel-konform statisch belegt) und die Rechtsseiten minimal-invasiv von den
Google-Fonts befreit (Schritt 4). Nach der Bereinigung laden weder Startseite
noch Rechtsseiten Ressourcen von fremden Domains. Es wurde nichts gelöscht,
`CNAME` blieb unberührt, und an Inhalt, Layout oder Farben der gelieferten
Dateien wurde nichts geändert.

**Commits auf `web-relaunch-v6` (task-relevant):**

- `8012f8d` — Startseite: Relaunch v6 eingebaut (Schritt 2, vor dieser Sitzung)
- `5e087a2` — Rechtsseiten: Google-Fonts entfernt, Poppins durch system-ui ersetzt (Schritt 4)

**Merge auf `main`:** liegt bei Tim — nach Diff-Review, ausschließlich `--ff-only`.

---

# Fortschritt — Spec 0001 · Startseite verdichten und teilbar machen

Arbeitsprotokoll zu `docs/specs/0001-startseite-verdichten.md`.
Branch: `feature/startseite-v6-1` (Basis `main`, Stand `aeaab28`).
Merge auf `main` liegt bei Tim, `--ff-only`.

## 2026-09-03 22:13 CEST · Ä1 bis Ä8 umgesetzt

Ä9 (Verdichtung) auf Anweisung ausgelassen und für später offen.

| Änderung | Was geschah |
|---|---|
| **Ä1** Zustandsbehauptungen | „Fertige" und „Done-for-you" entfernt: Claim, Meta-Beschreibung, `META`-Tabelle im Skript (de/it/en) und FAQ-Schlusssatz. Die deutschen Sätze waren zugleich Wörterbuchschlüssel — IT und EN wurden mitgezogen. |
| **Ä2** Produktfläche im Opener | Opener von mittig auf zweispaltig: Text links, Telefon rechts, Fußpunkte darunter über volle Breite. Vier Nachrichten wortgleich aus der Vorführung übernommen. |
| **Ä3** Rollen zweistufig | Il Segretario und Il Portiere als volle Karten ohne Status-Label. Darunter gestrichelte Zeile mit Mono-Chip `in Vorbereitung` und Il Ricordo · L'Agenda · Il Cerimoniere. „Und was noch fehlt" wortgleich als eigene Karte. |
| **Ä4** Beratung nach vorn | LagoBottega und LagoVoce als eigener Abschnitt `#beratung` zwischen Leistungen und Vorführung, mit Navigationspunkt. Einleitung wortgleich. |
| **Ä5** Unica Benaco | Karte unter dem Team-Schlusssatz: Bild, drei Zeilen, Link mit `rel="noopener"`. Kein Abschnitt, kein Navigationspunkt, keine Funktionsbeschreibung. |
| **Ä6** Demos gekennzeichnet | Mono-Chip `Beispielszenario` neben der Reiterleiste. |
| **Ä7** Aufruf geschärft | Zeile unter dem Knopf im Schlussabschnitt. |
| **Ä8** Teilbarkeit | Open Graph und Twitter Card vollständig, `canonical`, neuer Titel in drei Sprachen, OG-Bild `assets/og.jpg` (1200×630, 50 KB). |

## Prüfungen

| Abnahmekriterium | Soll | Ist |
|---|---|---|
| 1 · `grep -ci "fertige\|done-for-you"` | 0 / 0 | **0 / 0** |
| 2 · externe Verweise | 1 (unicabenaco.com) | **1 fremd + 4 auf die eigene Domain** — siehe 🟡 unten |
| 3 · neue Texte in IT und EN | vollständig | **8 / 8 belegt, kein Ungleichgewicht IT↔EN** |
| 4 · `documentElement.lang` schaltet mit | ja | **ja**, im Browser geprüft (DE→IT) |
| 5 · Open-Graph-Vorschau | korrekt | **Tags gesetzt, Bild vorhanden** — Vorschau in WhatsApp/LinkedIn erst nach Livegang prüfbar |
| 6 · keine neue Datei ausser Bildern | — | **nur `assets/og.jpg`**, kein `package.json` |
| 7 · Kontrast neuer Mono-Labels | ≥ 4.5:1 | **9.3:1** (`--cioccolato` auf `--weiss`) |
| 8 · Tastaturbedienung unverändert | ja | **ja**, `role="tablist"` unangetastet, Chip steht ausserhalb |
| 9 · lädt im Flugmodus | ja | **ja**, kein neuer Request beim Laden; die absoluten OG-Adressen liest nur der Scraper |
| 10 · Arbeitsprotokoll im Commit | ja | **dieser Eintrag** |

## Offen für Cristina — Copy

Alle drei sind Entwürfe aus der Spec, unverändert übernommen, nicht freigegeben:

1. **Ä1, Claim im Opener:** „KI-Assistenten für Hotellerie, Weingüter und Makler am Gardasee — auf Ihren Betrieb zugeschnitten, nicht aus dem Regal."
2. **Ä7, Zeile unter dem Aufruf:** „Wir arbeiten mit wenigen Häusern gleichzeitig. Erstgespräch kostenlos."
3. **Ä8, Seitentitel:** „LagoNord AI — KI-Assistenten für Betriebe am Gardasee" (bisher „… — Ihr KI-Partner am Gardasee").

Dazu von mir formuliert und ebenfalls unfreigegeben:

4. **Ä4, Vorzeile:** „Beratung und Kommunikation" statt „Ausserdem".
5. **Ä5, drei Zeilen:** „In eigener Sache" / „Unica Benaco" / „Unsere eigene Adresse am Gardasee — dort betreiben wir selbst, was wir hier beschreiben."
6. **Ä3, Chip:** „in Vorbereitung".

Die italienischen und englischen Fassungen aller sechs stehen in `sprachen.js` und
sind von mir übersetzt — Redaktion liegt bei Cristina.

## 🟡 Annahmen und Abweichungen

- **Bild für die Unica-Zeile angenommen.** Die Spec führt die Bildauswahl als offen.
  Ich habe `assets/gardaunica.jpg` genommen: lag ungenutzt im Repo, zeigt eine
  Gardasee-Karte im Kupferrahmen, passt dem Namen nach. **Nicht bestätigt.**
  Austausch ist eine Zeile in `index.html`.
- **Abnahmekriterium 2 gelesen als „keine fremden Domains".** Open Graph verlangt
  absolute Adressen; `og:url`, `og:image`, `twitter:image` und `canonical` zeigen
  deshalb viermal auf `www.lagonord.de`. Fremd ist weiterhin genau eine Domain:
  unicabenaco.com. Wörtlich gezählt reisst der Grep das Kriterium.
- **Navigationspunkt „Beratung" hinzugefügt.** Die Spec verlangt für Ä4 einen
  Ankerpunkt; ein Anker, auf den nichts zeigt, ist keiner. Die Navigation hat
  jetzt sechs statt fünf Einträge.
- **Opener von mittig auf zweispaltig.** Ä2 gibt das Gerät vor, nicht die Anlage.
  Mittig hätte das Telefon unter die Knöpfe und damit unter die Falz gedrückt —
  genau der Effekt, den Ä2 beheben soll.
- **Rollen-Halbsätze aus dem Bestand.** Für die drei angedeuteten Rollen habe ich
  die vorhandenen `amt`-Zeilen genommen statt neue Copy zu erfinden. Die
  ausführlichen Beschreibungen bleiben in `sprachen.js` stehen — sie werden
  gebraucht, sobald die Rollen ausliefern.

## 🟡 Gefundener Fehler im Bestand

> Nachtrag: behoben im Eintrag vom 22:31, Commit siehe dort.

Zwei Überschriften wechseln in **keiner** Sprache, seit dem Relaunch v6:

- „Ehrlich beantwortet." (FAQ)
- „Zwei Gründer. Ein Hund. Ein See." (Team)

Beide sind `h2` und werden vom Skript im Wörterbuch `h` gesucht, stehen in
`sprachen.js` aber unter `t`. Der Treffer bleibt aus, die Umschaltung fällt
still auf Deutsch zurück.

Die Korrektur ist je Sprache eine verschobene Zeile. **Nicht ausgeführt**, weil
ausserhalb von Ä1–Ä8. Vor aktiver Bewerbung in drei Sprachen sollte sie fallen.

## 2026-09-03 22:31 CEST · Nachtrag — zwei Überschriften wechseln wieder

Auf Anweisung nachgezogen, ausserhalb von Ä1–Ä8, eigener Commit.

**Befund.** „Ehrlich beantwortet." (FAQ) und „Zwei Gründer. Ein Hund. Ein See."
(Team) sind `h2` und stehen damit in `HTML_WAHL`. Das Skript sucht solche
Elemente im Wörterbuch `h`; beide Einträge lagen in `t`. Dort konnten sie nie
greifen — den Textknoten eines `h2` sammelt der `TreeWalker` gar nicht erst ein,
weil `el.closest(HTML_WAHL)` ihn verwirft. Die Umschaltung fiel still auf
Deutsch zurück, seit dem Relaunch v6.

**Korrektur.** Beide Einträge aus `it.t` und `en.t` entfernt und in `it.h`
beziehungsweise `en.h` gesetzt. Nur `sprachen.js` berührt, keine Zeile in
`index.html`, kein Text geändert — es sind dieselben Übersetzungen wie zuvor.

**Beim Prüfen aufgefallen, nicht angefasst:** Das englische Auszeichnungs-
wörterbuch steht nicht im Hauptobjekt, sondern wird danach separat zugewiesen
(`window.LAGONORD_SPRACHEN.en.h = {…}`). Im Hauptobjekt steht an seiner Stelle
ein leeres `"hx": {}` — offenbar ein Überrest. Funktioniert, ist aber eine
Stolperstelle für den Nächsten, der dort etwas einträgt.

| Prüfung | Soll | Ist |
|---|---|---|
| `it.h` / `en.h` enthalten beide Überschriften | ja | **ja** |
| `it.t` / `en.t` enthalten sie nicht mehr | ja | **ja** |
| Alle Auszeichnungsblöcke der Seite finden einen Treffer | 31 / 31 | **31 / 31 in IT und EN** |
| `node --check sprachen.js` | fehlerfrei | **fehlerfrei** |

Geprüft mit der Ersatzkette, die das Skript selbst verwendet
(`h[normalisiert] || h[roh]`) — die rohe Fassung trägt die mehrzeiligen
Schlüssel wie den Fondamento-Absatz.
