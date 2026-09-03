# Spec 0001 — Startseite verdichten und teilbar machen

**Repo:** lagonord-website · **Dateien:** `index.html`, `sprachen.js`, `assets/`
**Grundlage:** Inventur vom 3. September 2026, Commit 6e74014
**Läuft auf dem Bestand.** Unabhängig vom Relaunch v7, geht dort nicht verloren.
**Branch:** `feature/startseite-v6-1` · Merge durch Tim mit `--ff-only`

---

## Ziel

Die Startseite soll heute Abend so weit sein, dass man den Link aktiv weitergeben kann.

Drei Dinge stehen dem entgegen:

1. Sie behauptet einen Zustand, den es nicht gibt („Fertige … ‚Done-for-you'")
2. Sie gewichtet falsch — was sofort lieferbar ist, steht unter „Ausserdem"
3. Sie zeigt keine Software und liest sich deshalb als Beratung, nicht als Produktunternehmen

**Leitsatz:** Nichts behaupten, was nicht stimmt. Nichts preisgeben, wonach niemand gefragt hat.

---

## Ä1 — Zustandsbehauptungen entfernen

Die Wörter **„Fertige"** und **„‚Done-for-you' eingerichtet"** entfallen aus dem Opener.

Rollenbeschreibungen im Präsens („Il Portiere klärt rund um die Uhr…") bleiben unverändert. Das ist Produktbeschreibung, keine Zustandsaussage, und völlig üblich.

**Entwurf, Cristina entscheidet:**
> „KI-Assistenten für Hotellerie, Weingüter und Makler am Gardasee — auf Ihren Betrieb zugeschnitten, nicht aus dem Regal."

Begründung für die Akte: Die Seite verspricht an fünf Stellen „Belegt statt erfunden". Eine unbelegte Zustandsaussage auf der Startseite entwertet das Versprechen und ist nach UWG angreifbar.

## Ä2 — Produktfläche in den Opener

Der größte Hebel für den Eindruck „Softwareunternehmen".

Eines der drei Demo-Geräte aus `#vorfuehrung` wandert als statische Ansicht in den Opener — mit Geräterahmen und Schatten, nicht animiert. Die Vorführung selbst bleibt an ihrer Stelle unverändert.

Heute steht das Beste, was die Seite hat, an Position 04 und wird von vielen nie gesehen.

Keine neuen Assets nötig, das Markup existiert bereits.

## Ä3 — Zwei Rollen tragen, vier deuten an

Der Abschnitt `02 — Ruoli` wird zweistufig.

**Volle Karten:**
- **Il Segretario** — alles, was hereinkommt
- **Il Portiere** — auf der Website, drei Sprachen

**Kompakte Zeile darunter:** Il Ricordo · L'Agenda · Il Cerimoniere, je Name und Halbsatz, Mono-Label `in Vorbereitung`. „Und was noch fehlt" bleibt unverändert.

Die beiden Hauptrollen bekommen **kein** Status-Label. Kein Label ist keine Behauptung.

## Ä4 — Beratung und Kommunikation nach vorn

`LagoBottega` und `LagoVoce` verlassen die Position unter „Ausserdem" und werden ein eigener, gleichrangiger Abschnitt mit Überschrift und Ankerpunkt.

Beides ist heute ohne Software lieferbar und nutzt vorhandene Ausweise. Als Fußnote unter fünf Rollen steht das Verkäufliche unter dem Unfertigen.

Die Einleitung „Nicht alles gehört an einen Assistenten…" bleibt im Wortlaut.

## Ä5 — Unica Benaco andeuten

Im Abschnitt `#team`, direkt unter „Was wir für unsere Kunden bauen, betreiben wir zuerst für uns selbst."

Ein Bild, drei Zeilen, ein Link auf `https://unicabenaco.com` mit `rel="noopener"`.

**Grenzen:** Kein eigener Abschnitt, kein Navigationspunkt, keine Funktionsbeschreibung. Unica Benaco ist hier Beleg, kein Angebot. Es ist der einzige Ort, an dem ein Besucher die Seite verlässt — beabsichtigt.

Bild lokal in `assets/`. **Kein Hotlink** auf unicabenaco.com.

## Ä6 — Demos kennzeichnen

Mono-Label `Beispielszenario` an den drei Reitern. Inhaltlich bleibt alles gleich. Die Kennzeichnung nimmt den Demos nichts und schützt „Belegt statt erfunden".

## Ä7 — Aufruf schärfen

**Entwurf, Cristina entscheidet:**
> „Wir arbeiten mit wenigen Häusern gleichzeitig. Erstgespräch kostenlos."

Wahr, weil ein Gründer. Liest sich als Auswahl, nicht als Leere.

## Ä8 — Teilbarkeit

Das ist der Punkt, ohne den „nach außen teilen" nicht funktioniert. Geteilt wird ein Link in WhatsApp, LinkedIn oder per Mail — und dort erscheint heute nichts.

- `og:title`, `og:description`, `og:image`, `og:url`, `og:type`
- `twitter:card` als `summary_large_image`
- Aussagekräftiger `<title>` und `meta description`
- Bild 1200×630, lokal in `assets/`, unter 300 KB

Vorbild ist die eigene Schwesterseite: unicabenaco.com hat das bereits vollständig.

## Ä9 — Verdichtung

Vier Eingriffe, die den technischen Eindruck verschieben, ohne Palette oder Schriften anzufassen:

1. **JetBrains Mono konsequent als Datenebene** — Statuslabels, Kennzahlen, Rollenmetadaten. Die Schrift ist da und wird halb genutzt
2. **Ein zweiter dunkler Block** als Taktgeber. Heute gibt es genau einen, dadurch fehlt der Seite Rhythmus
3. **Vertikale Abstände zwischen Abschnitten reduzieren.** Die Seite atmet redaktionell, nicht technisch
4. **Demo-Geräte mit mehr Präsenz** — Rahmen, Schatten, Tiefe

---

## Nicht-Ziele

- **Keine Änderung an Farben und Schriften.** Papier `#FAF5E8`, Kupfer `#C9906F`, Nacht `#141D33`, Sora, JetBrains Mono. Die Schriftpaarung ist das, was die Seite von der KI-Standardsignatur trennt
- **Keine neuen externen Requests.** Genau ein ausgehender Link (unicabenaco.com), sonst nichts. Keine CDN, keine Analytik, keine Schriften von außen
- **Kein Build.** Kein `package.json`, kein Bundler, kein Framework
- **Keine Kundennamen, Logos, Nutzerzahlen, Piloten oder Referenzen.** Es gibt keine
- **Keine zusätzlichen `01/02/03`-Marker**
- **LagoTerra bleibt** bestehen, wird aber nicht ausgebaut
- Sprach-URLs, Formular, Buchung, Messung: eigene Module, siehe `MAP-relaunch-v7.md`

---

## Technische Vorgaben

- Neue Sichttexte als Schlüssel in `sprachen.js`, **DE / IT / EN**. Deutsch zuerst zur Freigabe, Italienisch danach
- Produkt- und Rollennamen bleiben in allen Sprachen italienisch
- Neue Mono-Labels folgen der vorhandenen Größenskala, keine neue Größe erfinden
- `prefers-reduced-motion` weiterhin beachten
- Bestehende ARIA-Muster unverändert: `role="tablist"`, `aria-selected`, `role="dialog"`, Fokusrückgabe, Escape
- `index.html` bleibt eine Datei

---

## Abnahmekriterien

1. `grep -ci "fertige\|done-for-you" index.html sprachen.js` → **0** in beiden
2. Externe `http(s)`-Verweise in `index.html`: genau **einer**, unicabenaco.com
3. Alle neuen Texte wechseln korrekt nach IT und EN, keine deutschen Reste
4. `documentElement.lang` schaltet weiterhin mit
5. Open-Graph-Vorschau prüfbar korrekt: Titel, Beschreibung, Bild
6. Kein `package.json`, keine neue Datei außer Bildern in `assets/`
7. Kontrast neuer Mono-Labels ≥ 4.5:1
8. Tastaturbedienung von Reitern und Pop-ups unverändert funktionsfähig
9. Seite lädt vollständig im Flugmodus
10. `FORTSCHRITT.md` mit Arbeitsprotokoll im Commit

---

## Offen vor dem Livegang

- **Copy-Freigabe Cristina** für Ä1 und Ä7 — die Entwürfe oben sind Vorschläge
- **IT- und EN-Fassungen** aller neuen Texte
- **OG-Bild** 1200×630 erstellen
- **Bildauswahl** für die Unica-Zeile
- **Abgrenzung LagoHost ↔ unicabenaco.com/partner** — beide sprechen Hotels mit einem Concierge-Versprechen an. Nicht Teil dieser Spec, aber vor Cristinas Partnerakquise zu klären
