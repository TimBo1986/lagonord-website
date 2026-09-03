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

## 2026-09-03 22:27 CEST · Nachtrag — Bild der Unica-Karte getauscht

Auf Anweisung. Meine Annahme aus dem ersten Durchgang ist damit erledigt.

- `assets/gardaunica.jpg` → `assets/unica_app.jpg` (Bildschirmfoto der App,
  1080×2115, 182 KB). Das alte Bild bleibt liegen, nichts gelöscht.
- Alt-Text von „Unica Benaco" auf „Bildschirmfoto der Unica-App" — das alte
  beschrieb eine Karte, das neue zeigt einen Bildschirm.
- **CSS mit angepasst, sonst wäre es falsch dargestellt worden.** Die Kachel
  war auf `aspect-ratio:4/3` mit `object-fit:cover` gestellt; ein Hochformat
  von 1:1,96 wäre darin auf einen Querstreifen aus der Bildmitte beschnitten
  worden. Jetzt natürliches Seitenverhältnis ohne Zuschnitt, 66 px breit,
  mit Rahmen und Radius, damit die Kachel als Bildschirm lesbar ist.

| Prüfung | Soll | Ist |
|---|---|---|
| Verweis auf `gardaunica.jpg` in `index.html` | 0 | **0** |
| Fremde Domains | 1 (unicabenaco.com) | **1** |
| Darstellung der Karte | ohne Zuschnitt | **geprüft im Browser, 1440 px** |

🟡 **Bekannte Grenze:** `alt` ist ein Attribut, kein Textknoten — die
Sprachumschaltung fasst es nicht an. Der Alt-Text bleibt in allen drei
Sprachen deutsch. Das gilt für alle Bilder der Seite und ist kein neuer
Zustand; für Vorleseprogramme in IT und EN wäre es zu beheben, wenn
`sprach-urls` kommt.

## 2026-09-03 22:58 CEST · Nachbesserung 1 zu Ä2 — Gerät im Opener auf 1:2

Das Gerät wirkte gedrungen (296×414, also rund 1:1,4) und las sich nicht als
Handy. Jetzt 296×592, exakt 1:2.

Umgesetzt nicht über eine geratene Chathöhe, sondern über `aspect-ratio:1/2`
auf dem Gerät selbst: `.telefon` wird zur Flex-Spalte, Kopf und Fuss behalten
ihre natürliche Höhe, der Chatbereich nimmt den Rest. Damit stimmt das
Verhältnis unabhängig davon, wie hoch Kopf und Fuss ausfallen — auch wenn die
Fusszeile in einer anderen Sprache umbricht.

Die Nachrichten sitzen jetzt unten (`justify-content:flex-end`) statt oben,
wie in einem laufenden Verlauf — dasselbe Muster wie das Gerät in der
Vorführung. Der Chatinhalt ist unverändert, vier Nachrichten wie zuvor.

| Prüfung | Soll | Ist |
|---|---|---|
| Seitenverhältnis bei 1440 px | ~1:2 | **296×592 = 2.00** |
| Breite unverändert | 296 px | **296 px** |
| Chatinhalt | unverändert | **unverändert** |

## 2026-09-03 23:01 CEST · Nachbesserung 2 zu Ä2 — Opener über drei Breiten

Gemessen in einem iframe fester Breite, weil das Browserfenster gezoomt
rendert und keine exakten Viewport-Breiten liefert.

**Befund vorher.** Der Opener hing am globalen Umbruchpunkt 940 px. Bei 768 px
fiel das Gerät damit unter den Text und stand mit 284 px in einer 724 px
breiten Spalte — daher der Leerraum. Bei 390 px war es mit 284 px viel zu
gross, die Knöpfe endeten bei 807 von 844 px.

**Geändert.** Der Opener bekommt eigene Umbruchpunkte:

- **bis 1100 px:** bleibt zweispaltig, Gerät tritt auf 238 px zurück,
  Spaltenverhältnis 1,25 : 0,75 zugunsten des Textes
- **bis 700 px:** erst hier untereinander. Gerät auf 186 px, Logo auf 196 px,
  Aussenabstand des Openers von 72/68 auf 44/52, Satz und Knopfabstand knapper

| Breite | Anlage | Gerät | Claim sichtbar | Knöpfe sichtbar |
|---|---|---|---|---|
| 390 × 844 | einspaltig, Gerät unter dem Text | 186 × 372 | **ja** (462) | **ja** (731) |
| 768 × 1024 | zweispaltig, Gerät neben dem Text | 238 × 476 | **ja** (529) | **ja** (784) |
| 1440 × 900 | zweispaltig, unverändert | 296 × 592 | **ja** (623) | **ja** (813) |

🟡 **Zwei Fälle ausserhalb der Vorgabe, die nicht passen:** Bei 390 × 667
(Geräteklasse 375 px, etwa iPhone SE) enden die Knöpfe bei 731 und liegen
unter der Falz; bei 1024 × 768 (Tablet quer) bei 793. Beide Höhen waren nicht
gefordert — 390 px Breite entspricht 844 px Höhe. Weiter zu kürzen ginge nur
über Überschrift oder Fliesstext, das ist eine Gestaltungsentscheidung.

🟡 **Bei 768 px läuft die oberste Chatnachricht oben weich aus.** Im schmaleren
Gerät umbrechen die Nachrichten stärker und überschreiten den Chatbereich. Das
ist die vorhandene Maske der Seite und liest sich als hochgescrollter Verlauf —
dasselbe Verhalten wie in der Vorführung. Der Chatinhalt blieb wie verlangt
unverändert.

## 2026-09-03 23:07 CEST · Nachbesserung 3 — Abschnitt heisst „In Aktion"

🟡 **Annahme notiert.** Der Auftrag nennt zwei sichtbare Stellen,
Navigationspunkt und Abschnittsüberschrift. Das Wort „Vorführung" stand
sichtbar aber nur an einer Stelle: im Navigationspunkt. Die Vorzeile des
Abschnitts hiess „Sehen Sie selbst", die Überschrift lautet „Drei Betriebe,
drei Oberflächen — dieselbe Grundlage."

Gedeutet als: der Abschnitt soll dort, wo er sich selbst benennt, „In Aktion"
heissen. Geändert wurden Navigationspunkt und Vorzeile. Die `h2` blieb
unangetastet — sie trägt keine Abschnittsbenennung, sondern eine Aussage.
Falls die `h2` gemeint war, ist das eine Zeile.

| Stelle | vorher | nachher |
|---|---|---|
| Navigationspunkt | Vorführung | **In Aktion** |
| Vorzeile des Abschnitts | Sehen Sie selbst | **In Aktion** |
| `h2` des Abschnitts | Drei Betriebe, drei Oberflächen … | **unverändert** |
| Knopf im Opener | Assistent in Aktion sehen | **unverändert** |
| Anker | `#vorfuehrung` | **unverändert** |

Beide Stellen tragen denselben Wortlaut und teilen sich daher einen
Wörterbucheintrag: IT „In azione", EN „In action". Die durch die Umbenennung
tot gewordenen Schlüssel „Vorführung" und „Sehen Sie selbst" wurden entfernt,
statt den vom Review bemängelten Bestand an toten Schlüsseln zu vergrössern.

| Prüfung | Ist |
|---|---|
| DE / IT / EN in Navigation und Vorzeile | **In Aktion · In azione · In action** |
| Anker `#vorfuehrung` vorhanden | **ja** |
| Links auf `#vorfuehrung` | **3, alle unverändert** |
| Knopf „Assistent in Aktion sehen" | **unverändert, schaltet weiter mit** |

Die Kommentare im Quelltext sprechen weiter von „Vorführung" — sie benennen
den Anker, und der bleibt `#vorfuehrung`.

---

# Nacharbeit nach Code-Review

Das Review lief im Hintergrund und kam erst nach den Commits `e76ce11` bis
`d63b228` zurück. Freigabe für die Nacharbeit liegt vor.

## 2026-09-03 23:24 CEST · Mechanische Defekte in `index.html`

Alle vier von mir selbst in dieser Sitzung eingebaut.

| Befund | Korrektur |
|---|---|
| `.unica img` reservierte keine Höhe. Beim Tausch auf das Hochformat hatte ich `aspect-ratio` entfernt und nur `width` gesetzt; das `<img>` trägt keine Maßattribute. Bis das Bild geladen war, war die Zelle 66×0, danach 66×129 — Karte und alles darunter sprangen. | `aspect-ratio:24/47` ergänzt. Das ist das gekürzte Verhältnis der Quelle (1080×2115) und bleibt beim Verkleinern gültig. |
| Der Kommentar über dem Ä5-Block erklärte weiter das 4:3-Verhältnis und den „Kupferrahmen", die es beide nicht mehr gibt — und widersprach dem Kommentar acht Zeilen darunter. | Veralteten Halbsatz entfernt. |
| `.reiter{margin:0}` stand global und war die dritte Deklaration derselben Klasse, allein durch Quellreihenfolge aufgelöst. Eine zweite Reiterleiste anderswo hätte still ihren Abstand verloren. | Auf `.reiter-zeile .reiter` gescoped. |
| Der Kommentar im Kopf behauptete, die Vorschau sei „immer deutsch" — richtig für Scraper, aber `<title>` und `description` schalten sehr wohl mit, `og:*` und `twitter:*` nicht. | Kommentar sagt jetzt, was tatsächlich geschieht. |

## 2026-09-03 23:31 CEST · Hygiene in `CLAUDE.md` und `sprachen.js`

**`CLAUDE.md`.** Die Zeilenkarte ist ganz entfernt statt aktualisiert — feste
Zählwerte veralten still und schicken den Nächsten in den falschen Bereich.
An ihre Stelle tritt eine Aufzählung der Abschnitte in ihrer Reihenfolge, die
über Marker auffindbar sind (`id="schrift"`, `<section id="…">`). Aus demselben
Grund nennt auch die `grep`-Warnung keine Zeilennummern mehr, sondern den
Block `<style id="schrift">`.

Der Pflicht-Check lautet jetzt nicht mehr „0 Treffer", sondern nennt die
erlaubten Domains:

```sh
grep -oh 'https\?://[a-z0-9.-]*' index.html sprachen.js | sort -u
# erlaubt: unicabenaco.com (Beleg im Team) und www.lagonord.de
# (Open Graph und canonical brauchen absolute Adressen)
```

Zwei Ergänzungen: der Abschnitt heisst dort jetzt „In Aktion" mit dem Hinweis,
dass der Anker `#vorfuehrung` bleibt, und ein Absatz hält fest, dass Attribute
(`alt`, `aria-label`, `title`) von der Sprachumschaltung nicht erfasst werden.

**`sprachen.js`.**

- **`hx` aufgelöst.** Das englische Auszeichnungswörterbuch stand als leeres
  `"hx": {}` im Objekt, während das echte `en.h` danach separat zugewiesen
  wurde. Wer die Struktur von `it` nachahmte und ein `"h"` inline ergänzte,
  wurde von der späteren Zuweisung still überschrieben. Der Inhalt steht jetzt
  im Objekt, die separate Zuweisung ist weg. **Geprüft:** Wörterbücher vor und
  nach dem Umbau maschinell verglichen — einziger Unterschied ist das
  entfallene leere `hx`, alle 27 Einträge je Sprache erhalten.
- **`"Ausserdem"` entfernt** — mit Ä4 endgültig durch „Beratung und
  Kommunikation" ersetzt.
- **Tote `t`-Dublette `"Reden wir über Ihren Betrieb."` entfernt.** Die Zeile
  ist eine `h2`, die lebende Fassung steht in `h`. Wer den deutschen Wortlaut
  ändert, hätte sonst eine der beiden übersehen.

🟡 **Einem Befund des Reviews widersprochen.** Es stufte `"Website · E-Mail"`
als tot und zu entfernen ein. Das ist der Kanal von Il Cerimoniere — derselben
Rolle, deren Beschreibung mit Ä3 bewusst stehen blieb, weil sie zurückkommt,
sobald die Rolle ausliefert. Der Schlüssel gehört zur selben Aussetzung und
bleibt.

## 2026-09-03 23:38 CEST · Lead-Satz über den Rollen

**Befund des Reviews, bestätigt.** Der Lead über dem Rollen-Schema behauptete
weiter „fünf Rollen" und „Jede Rolle … wird einzeln eingerichtet" — vierzig
Zeilen über dem Chip, der drei davon als unfertig ausweist. Das ist dieselbe
Behauptungsklasse, die Ä1 entfernen soll. Mein Fehler in Ä3: ich habe die
Karten zweistufig gemacht und den Satz stehen lassen, der sie zusammenfasst.

| | Wortlaut |
|---|---|
| vorher | „Eine Grundlage, zwei Linien, fünf Rollen. Jede Rolle hat einen klaren Auftrag, einen eigenen Ton und Zugriff auf das hinterlegte Wissen Ihres Hauses — und wird einzeln eingerichtet." |
| nachher | „Eine Grundlage, zwei Linien. Zwei Rollen richten wir heute ein, drei bereiten wir vor." |

**Offen für Cristina** — Entwurf aus der Freigabe, nicht redigiert. IT „Una
base, due linee. Due ruoli li attiviamo oggi, tre sono in preparazione.", EN
„One foundation, two lines. Two roles we set up today, three are in
preparation." Der alte Schlüssel ist entfernt.

Der neue Satz lässt weg, was der alte über Auftrag, Ton und Wissenszugriff je
Rolle sagte. Das steht weiter auf den beiden Rollenkarten selbst.

## 2026-09-03 23:46 CEST · Teilbarkeit nachgezogen

**OG-Bild neu gerendert.** Die Unterzeile sagte „Assistenten", die freigegebene
Copy sagt „KI-Assistenten" — im Artefakt, für das Ä8 überhaupt existiert, stand
damit in jedem geteilten Link das abweichende Wort. Sonst unverändert:
1200×630, 51 KB, gleiche Anlage.

**Beschreibungen gekürzt.** Die alte Fassung war 169 Zeichen lang und wurde bei
etwa 155 abgeschnitten — weg fiel gerade „auf Deutsch, Italienisch und
Englisch", also das Unterscheidungsmerkmal. Neu fällt der einleitende Halbsatz
weg, den der Titel ohnehin trägt; die drei Sprachen stehen jetzt im sichtbaren
Bereich.

| Sprache | Zeichen | Drei Sprachen genannt |
|---|---|---|
| DE | 137 | ja |
| IT | 117 | ja |
| EN | 127 | ja |

Gesetzt an allen vier Stellen der deutschen Fassung: `meta[description]`,
`og:description`, `twitter:description` und `META.de.d` im Skript, dazu
`META.it.d` und `META.en.d`.

## 2026-09-03 23:52 CEST · `unica_app.jpg` verkleinert

182 KB und 1080×2115 für eine Anzeige von 66 px war rund das Sechzehnfache
dessen, was gebraucht wird — und die grösste Nutzlast, die dieser Branch
hinzufügt, unterhalb der Falz eifrig geladen.

- **192 × 376, 16,3 KB** (Güte 88). Das Format hält 24:47 exakt, also genau das
  Verhältnis, das in `.unica img` als `aspect-ratio` steht — die
  Höhenreservierung bleibt dadurch gültig. Bei 66 px Anzeige reicht das auch
  bei dreifacher Pixeldichte.
- **`loading="lazy"` ergänzt.** Das Bild sitzt im Team-Abschnitt weit unter der
  Falz und konkurrierte bisher mit dem Opener um den ersten Aufbau.

Die Fassung in voller Auflösung bleibt über die Git-Historie erreichbar
(`d63b228`).

---

## Offene Punkte aus dem Review — bewusst nicht behoben

**`.t-fuss` mit 2,84:1 über der Falz.** Die Fusszeile des Geräts
(`Ihre Farben · Ihre Hausregeln · drei Sprachen`) steht in `--taupe` `#9A9078`
auf `--crema` `#F7F2EA` bei 8,5 px, versal — 2,84:1 und damit unter der
WCAG-AA-Schwelle von 4,5:1. Mit Ä2 ist die Komponente in den Opener gewandert
und steht jetzt weit oben. Der Bericht zu Abnahmekriterium 7 (9,3:1) galt nur
den in dieser Änderung neu geschriebenen Beschriftungen, nicht der von Ä2
übernommenen Komponente.

Nicht behoben: Der Fix wäre ein Token (`--cioccolato` gäbe 8,43:1), würde aber
dieselbe Komponente in der Vorführung mitverändern — zu viel Fläche für heute.
**Entscheidung von Tim, bewusst vertagt.**

**iPhone SE (390 × 667): Knöpfe unter der Falz.** Bei dieser Höhe enden die
Knöpfe im Opener bei 731 px. Bei 390 × 844, der zur Breite gehörenden
Geräteklasse, stehen sie bei 731 von 844 und damit sichtbar. Weiter zu kürzen
ginge nur über Überschrift oder Fliesstext. **Vermerkt, nicht behoben.**

**Alt-Texte bleiben deutsch.** Attribute erfasst die Sprachumschaltung nicht;
in `CLAUDE.md` festgehalten. Fällt mit `sprach-urls`.
