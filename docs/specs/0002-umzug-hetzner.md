# Spec 0002 — Umzug auf eigene Infrastruktur

**Repos:** lagonord-website, später unicabenaco.com
**Server:** Hetzner, bestehende Docker-Umgebung mit Nginx, n8n, PostgreSQL
**Voraussetzung:** Spec 0001 ist live, Rechtstexte sind ausgetauscht
**Zeitrahmen:** ein bis zwei Abende

---

## Warum

Vier Dinge hängen an derselben Entscheidung und lösen sich gemeinsam:

1. **Die Aussage stimmt nicht.** Die Startseite führt „Europäische Anbieter"
   als eine von drei Säulen. Ausgeliefert wird die Seite von GitHub Inc. in
   den USA. Für eine Firma, die europäische KI-Infrastruktur verkauft, ist das
   die schwächste Stelle im Auftritt.
2. **Das Repo ist öffentlich**, weil GitHub Pages aus privaten Repos einen
   bezahlten Plan verlangt. Damit sind Commit-Historie und Arbeitsnotizen
   für jeden lesbar.
3. **Der Drittlandhinweis entfällt.** Die Neufassung nennt in Ziffer 3
   GitHub Inc. und die Übermittlung in die USA — korrekt, aber es ist die
   schwächste Stelle einer Erklärung, die europäische Infrastruktur verspricht.
   Nach dem Umzug fällt der Absatz ersatzlos weg. Die Erklärung wird kürzer und
   die Aussage stärker.
4. **Rückkanal und Messung brauchen ohnehin diesen Server.** Formular über
   n8n, Terminbuchung über Cal.com, Analytik. Getrennt umzuziehen wäre
   doppelte Arbeit.

---

## Umfang

### T1 — Statische Auslieferung auf dem Hetzner

- Verzeichnis für die Seite, eigener Nginx-Server-Block
- TLS über Let's Encrypt, automatische Erneuerung
- `www.lagonord.de` und `lagonord.de` beide erreichbar, eine kanonische Form,
  die andere leitet weiter
- HTTP leitet auf HTTPS
- Sinnvolle Header: HSTS, `X-Content-Type-Options`, Referrer-Policy
- Komprimierung und Cache-Header für Assets

### T2 — Deploy-Weg

Ein Push auf `main` soll weiterhin die Veröffentlichung sein.

Zwei Wege stehen zur Wahl, Entscheidung im Task:
- **GitHub Action mit rsync** über einen Deploy-Schlüssel
- **`git pull` auf dem Server**, ausgelöst per Webhook

Der zweite Weg braucht keinen Schlüssel bei GitHub, der erste keine
Git-Installation auf dem Webserver. Beide sind vertretbar.

**Rollback muss möglich sein**, ohne den vorherigen Stand neu bauen zu müssen.

### T3 — DNS-Umstellung

- Erst Server aufsetzen und unter einer Testadresse prüfen
- Danach A- und AAAA-Record umstellen, `CNAME`-Datei aus dem Repo entfernen
- TTL vorher senken, damit die Umstellung schnell greift
- **Die MX-Einträge auf `mx.zoho.eu` dürfen nicht angefasst werden** — sonst
  ist die Firmenmail tot

### T4 — Datenschutzerklärung nachziehen

**Im selben Zug, nicht danach.** Die Neufassung beschreibt den Ist-Zustand
korrekt: Ziffer 3 nennt GitHub Pages, GitHub Inc., San Francisco, als Anbieter
und trägt den Hinweis auf die Übermittlung in die USA samt
Standardvertragsklauseln. Ziffer 2 sagt „keine Analyse- oder Trackingdienste".

Damit ist T4 ein **Tausch des Hosters**, keine Korrektur mehr:

- Ziffer 3: GitHub Inc. → Hetzner Online GmbH, Industriestraße 25, 91710
  Gunzenhausen, mit dem Rechenzentrum, in dem der Server tatsächlich steht
- Der Absatz „Übermittlung in die USA" in Ziffer 3 entfällt ersatzlos
- Der Satz „Wir haben keinen Zugriff auf diese Logfiles und werten sie nicht
  aus" gilt nach dem Umzug nicht mehr — auf dem eigenen Server liegen die Logs
  bei uns. Er ist zu ersetzen, nicht zu streichen: Zweck, Rechtsgrundlage und
  **Löschfrist** der eigenen Logs gehören hinein
- **AV-Vertrag mit Hetzner** muss vor der Umstellung vorliegen und auffindbar
  sein. Die Altfassung hat ihn behauptet, ohne dass er belegt war — dieser
  Fehler darf sich nicht wiederholen

Ebenso: `docs/VERARBEITUNGEN.md` aktualisieren.

🔴 **Voraussetzung, die noch aussteht.** Ausgeliefert wird weiterhin die alte
`privacy.html` (unverändert seit `5e087a2`, nennt fälschlich Hetzner). Von der
Neufassung liegt bisher nur die **italienische** Fassung vor
(`docs/rechtstexte/datenschutz-lagonord-it.md`). Solange die deutsche
Neufassung nicht in `privacy.html` steht, beschreibt dieser Abschnitt einen
Zustand, den es noch nicht gibt.

### T5 — Repo auf privat

Erst nachdem T1 bis T4 stehen und die Seite vom Hetzner ausgeliefert wird.

Danach kann auch `FORTSCHRITT.md` im Repo bleiben.

**Hinweis:** Was einmal öffentlich war, bleibt über die Historie abrufbar.
Umstellung auf privat verhindert künftige Einsicht, löscht die Vergangenheit
nicht.

### T6 — Kontaktformular

- Formular auf der Seite, `POST` an einen n8n-Webhook auf demselben Server
- n8n versendet über Brevo an `info@lagonord.de`
- **Spam-Schutz ohne fremde Dienste:** Honeypot-Feld, Zeitmessung, Rate Limit
  in Nginx. Kein reCAPTCHA — das wäre wieder ein US-Dienst
- Bestätigungsseite oder Inline-Rückmeldung
- Datenschutzhinweis am Formular mit Verweis auf die Erklärung
- Neuer Abschnitt in der Datenschutzerklärung, **im selben Commit**

### T7 — Terminbuchung

- Cal.com im Container, unter einer eigenen Subdomain
- Anbindung an den Kalender, den ihr tatsächlich nutzt
- Ersetzt oder ergänzt den `mailto:`-Aufruf „Erstgespräch"
- Datenschutzabschnitt im selben Commit

### T8 — Reichweitenmessung

- Umami oder Plausible Community Edition im Container, eigene Subdomain
- Cookielos, keine Weitergabe an Dritte
- Ziel: welche der elf Handlungsaufforderungen tatsächlich geklickt werden
- Datenschutzabschnitt im selben Commit

**Rechtlicher Vorbehalt:** Ob cookielose Erstanbieter-Analytik einwilligungsfrei
zulässig ist, wird uneinheitlich beurteilt. Vor dem Livegang klären. Das ist
eine Rechtsfrage, keine technische.

**Ziffer 2 der Datenschutzerklärung wird dadurch unwahr.** Die Neufassung sagt
dort „keine Analyse- oder Trackingdienste" — eine Erstanbieter-Analytik ist ein
Analysedienst, auch ohne Cookies und auf eigenem Server.

Neue Formulierung etwa: „keine Dienste Dritter, keine Cookies, Auswertung auf
eigenen Servern in Deutschland."

---

## Reihenfolge

```
T1 Server → T2 Deploy → Test unter Testadresse
   ↓
T3 DNS + T4 Datenschutz  (gemeinsam, an einem Abend)
   ↓
T5 Repo privat
   ↓
T6 Formular → T7 Buchung → T8 Messung  (je eigener Abend)
```

T1 bis T5 sind ein Vorgang und sollten nicht auseinandergezogen werden.
T6 bis T8 sind unabhängig voneinander und können einzeln kommen.

---

## Nicht-Ziele

- **Kein Relaunch.** Der Seiteninhalt bleibt unverändert. Sprach-URLs,
  Seitenbaum und Generator sind eigene Module, siehe `MAP-relaunch-v7.md`
- **Keine Änderung an Palette, Schriften oder Layout**
- **unicabenaco.com bleibt zunächst auf GitHub Pages.** Ein Umzug pro Abend
- **Kein CDN.** Die Seite ist klein und die Nutzer sind in der Region
- **Kein Kubernetes, kein Orchestrierungsausbau.** Nginx und Docker Compose
  genügen

---

## Abnahmekriterien

1. `dig A lagonord.de` zeigt die Hetzner-Adresse
2. `dig MX lagonord.de` zeigt weiterhin `mx.zoho.eu` — Mail funktioniert
3. HTTPS gültig, HTTP leitet weiter, eine kanonische Domain
4. Ein Push auf `main` erscheint innerhalb weniger Minuten live
5. Ein Rollback auf den vorherigen Stand ist ohne Neubau möglich
6. Datenschutzerklärung nennt Hetzner, der USA-Absatz ist entfernt
7. `docs/VERARBEITUNGEN.md` ist aktualisiert
8. Repo ist privat, Seite weiterhin erreichbar
9. Bei T6 bis T8 jeweils: Verarbeitung und Datenschutzabschnitt im selben
   Commit
10. Weiterhin keine Ressource von fremden Servern

---

## Offene Entscheidungen

- **Deploy-Weg:** GitHub Action oder Webhook
- **Analytik:** Umami oder Plausible
- **Kalender** für Cal.com
- **AV-Verträge** mit Hetzner und Brevo: prüfen, ob abgeschlossen. Die
  Datenschutzerklärung behauptet es
- **Backup** der statischen Seite: liegt ohnehin in git, aber die Serverkonfiguration
  nicht — gehört in ein Repo oder in die Dokumentation
