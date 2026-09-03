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
3. **Ziffer 3 der Datenschutzerklärung wird zutreffend.** Sie nennt heute
   schon Hetzner und ist damit falsch, weil GitHub Pages ausliefert — und
   enthält gerade deshalb *keinen* Drittlandhinweis, obwohl die Auslieferung
   heute in den USA liegt. Nach dem Umzug stimmt die Angabe, und ein Hinweis
   wird auch nicht nötig. **Der USA-Absatz in Ziffer 7 bleibt davon
   unberührt** — er betrifft Google, HubSpot und Meta, nicht das Hosting.
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

**Im selben Zug, nicht danach — aber aus dem umgekehrten Grund.** Ziffer 3
nennt **heute schon Hetzner** („Diese Website wird auf einem Server der Hetzner
Online GmbH … gehostet. Mit Hetzner besteht ein Auftragsverarbeitungsvertrag"),
unverändert seit `5e087a2`. Ausgeliefert wird die Seite aber von GitHub Pages —
belegt durch `CNAME`, den GitHub-Remote, sechs Pages-Deployment-Commits und das
Fehlen jeder Server- oder Containerkonfiguration.

**Die Angabe ist also jetzt falsch und wird durch den Umzug richtig**, nicht
umgekehrt. Das ändert die Dringlichkeit: es ist kein Folgeschritt, den der
Umzug auslöst, sondern eine unzutreffende Aussage, die live steht — samt einem
AV-Vertrag, der einem Anbieter zugeschrieben wird, der die Seite nicht
ausliefert.

**Geprüft werden muss der Text trotzdem im selben Zug**, denn richtig wird die
Angabe nicht dadurch, dass sie zufällig zum Zielzustand passt. Zu belegen sind:

- der **AV-Vertrag** mit Hetzner — abgeschlossen, gegengezeichnet, auffindbar
- das **Rechenzentrum**, in dem der Server tatsächlich steht (Hetzner betreibt
  auch Standorte ausserhalb Deutschlands)
- dass der **Absatz zur Übermittlung in die USA** danach wirklich entfallen
  kann — also kein weiterer Dienst im Auslieferungspfad bleibt

Ziffer 3 ist danach zu bestätigen oder zu korrigieren: Hetzner Online GmbH,
Industriestraße 25, 91710 Gunzenhausen, mit dem belegten Rechenzentrum und dem
belegten AV-Vertrag.

Ebenso: `docs/VERARBEITUNGEN.md` aktualisieren.

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

**Ziffer 9 der Datenschutzerklärung ist zu ergänzen** — nicht Ziffer 2, und
der Satz lautet dort anders als hier bisher angenommen: „Diese Website
verwendet aktuell keine Marketing- oder **Tracking-Cookies**." Eine cookielose
Erstanbieter-Analytik macht diesen Satz streng genommen nicht unwahr, weil er
nur von Cookies spricht. Ziffer 9 sagt aber selbst zu, die Erklärung zu
aktualisieren, „sollte sich dies ändern (z. B. durch den Einsatz von
Analyse-Tools)" — genau dieser Fall tritt ein.

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
