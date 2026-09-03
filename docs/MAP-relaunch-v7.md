# Capability Map — Relaunch v7 lagonord.de

**Grundlage:** Entwurf der Claude-Code-Session, ergänzt um zwei Module und mit korrigierter Reihenfolge.
**Vorgelagert:** `SPEC-0001-startseite-verdichten.md` läuft auf dem Bestand und blockiert nichts.

---

## Module

| ID | Verantwortung | Hängt ab von |
|---|---|---|
| `rechtstexte` | Neufassung `privacy.html`, Prüfung `imprint.html`. **Inhaltlich sofort möglich**, die Seiten stehen heute eigenständig. Strukturelle Eingliederung später | — |
| `rueckkanal` | Kontaktformular und Terminbuchung auf eigener Infrastruktur | — |
| `messung` | Erstanbieter-Analytik auf eigener Infrastruktur | — |
| `seitengeruest` | Gemeinsames Gerüst: Kopf, Fußzeile, Tokens, eingebettete Schriften. Entscheidet den Erzeugungsweg | — |
| `sprach-urls` | Eine URL je Sprache und Seite, Wurzelweiterleitung, `hreflang`, `lang`. Ablösung von `sprachen.js` | `seitengeruest` |
| `seitenbaum` | Welche Seiten es gibt, Verteilung des Einseiters, Navigation, Broschüren je Sprache | `seitengeruest`, `sprach-urls` |
| `auffindbarkeit` | Titel und Beschreibung je Seite, `sitemap.xml`, `robots.txt`, strukturierte Daten | `seitenbaum`, `sprach-urls` |

## Reihenfolge

```
0001 (Bestand)
   ↓
rechtstexte ──┐
rueckkanal  ──┤ parallel, unabhängig voneinander
messung     ──┘
   ↓
seitengeruest → sprach-urls → seitenbaum → auffindbarkeit
```

`rechtstexte` zuerst, weil es die aktive Bewerbung sperrt und an keinem Umbau hängt.
`messung` vor dem Relaunch, damit messbar ist, ob er etwas bringt.

---

## Entscheidungen, die die Map voraussetzt

### E1 — Rückkanal und Messung laufen auf eigener Infrastruktur

Vorhanden: Hetzner mit Docker, n8n, Brevo, PostgreSQL. Damit braucht keiner der drei Bedarfe einen fremden Dienst.

| Bedarf | Lösung | Aufwand |
|---|---|---|
| Kontaktformular | `POST` an n8n-Webhook, Versand über Brevo | läuft bereits |
| Terminbuchung | Cal.com im Container | ein Abend |
| Messung | Umami oder Plausible CE im Container | ein Abend |

Alles unter `*.lagonord.de`, alles in Deutschland, keine Dritten.

**Die Aussage ändert sich ehrlich:** von „null externe Requests" zu „keine Dritten, keine Cookies, eigene Server". Das bleibt stärker als der gesamte Wettbewerb — branchly lädt Google Tag Manager, während es DSGVO-Konformität behauptet.

**Rechtlicher Vorbehalt:** Ob cookielose Erstanbieter-Analytik einwilligungsfrei zulässig ist, wird uneinheitlich beurteilt. Vor dem Livegang klären. Rechtsfrage, keine technische.

### E2 — Seitenschnitt klein halten

Vier Seitentypen, nicht acht:

- Start
- Assistenti (beide Linien als Blöcke einer Seite)
- Beratung (LagoBottega, LagoVoce)
- Rechtstexte

Bei drei Sprachen sind das zwölf Dateien.

**Keine eigenen Rollenseiten.** Fünf Rollenseiten brauchen fünf eigenständige Inhalte, die es nicht gibt. Dünne Unterseiten schaden mehr, als sie nützen.

Die Trennung der Linien in zwei Seiten bleibt später möglich, sobald es je Linie genug Inhalt gibt. Nicht jetzt.

### E3 — Generator ja, klein

Zwölf Dateien von Hand driften auseinander, sobald sich ein Satz im Kopf ändert.

Ein kurzes Node-Skript liest `sprachen.js` als Quelle und erzeugt die Dateien. Kein Framework. Die Ausgabe wird committet, GitHub Pages liefert weiter statisches HTML aus.

Der Satz „kein Build-Schritt" stimmt danach nicht mehr. Die Eigenschaft dahinter — statische Auslieferung, keine Laufzeitabhängigkeit — bleibt vollständig erhalten.

### E4 — Der Konflikt, der `seitengeruest` bestimmt

Heute ist der deutsche Wortlaut selbst der Wörterbuchschlüssel und wird zur Laufzeit ersetzt. Mit einer URL je Sprache muss der Text je Sprache ausgeliefert vorliegen. `sprachen.js` verliert seine Laufzeitrolle und wird zur Quelle des Erzeugungsschritts.

Diese Entscheidung fällt in `seitengeruest`, bevor Seiten entstehen.

---

## Rahmen für alle Module

- Arbeit auf Feature-Branches, `main` unberührt, Merge durch Tim mit `--ff-only`
- Deutsch ist Quellsprache; jede neue Sichttext-Zeichenkette braucht IT und EN
- Arbeitsstand in der Commit-Meldung, nicht in einer Datei; `FORTSCHRITT.md`
  läuft lokal weiter und ist nicht versioniert
- Produkt- und Rollennamen bleiben in allen Sprachen italienisch
- Palette und Schriftpaarung sind gesetzt und nicht Gegenstand der Module

---

## Bekannte offene Punkte

Aus `docs/TASK_WEB_B1_RELAUNCH.md`, Abschnitt „Nicht Teil dieses Tasks":

- Sechs ungenutzte Bilder in `assets/` aufräumen
- Englische Broschüre — heute zeigt EN auf die deutsche PDF
- Sprachwahl über Seitenwechsel hinweg halten (fällt mit `sprach-urls` weg)
