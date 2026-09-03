# Verarbeitungen — Grundlage für ein Verzeichnis nach Art. 30 DSGVO

**Stand:** 4. September 2026 · **Repo-Stand:** `b64594b`
**Verantwortlich für die Fertigstellung:** Tim und Cristina

---

## Was dieses Dokument ist — und was nicht

Es ist eine **Bestandsaufnahme aus dem Quelltext dieses Repositorys**, gedacht
als Rohstoff für ein Verzeichnis von Verarbeitungstätigkeiten. Es ist **kein
fertiges Verzeichnis** und **keine Rechtsberatung**.

Zwei Sorten Aussagen stehen hier, und sie sind durchgehend gekennzeichnet:

- **🟢 Belegt** — im Repo nachweisbar, mit Fundstelle. Diese Aussagen kann man
  gegen den Quelltext prüfen.
- **🔴 Lücke** — liegt ausserhalb dieses Repos. Hier steht bewusst **keine
  Vermutung**, sondern die Frage, die zu beantworten ist, und wer sie
  beantworten kann.

Alles, was den Betrieb betrifft — n8n-Workflows, Datenbank, Zoho, die App,
Verträge — ist von hier aus **nicht einsehbar**. Diese Abschnitte sind
absichtlich leer und nicht geraten.

---

## Verantwortlicher

🟢 Aus `imprint.html` und `privacy.html`:

| Feld | Angabe |
|---|---|
| Name | Cristina Uhl |
| Anschrift | Via Boschetti 29, 37019 Peschiera del Garda (VR), Italien |
| E-Mail | info@lagonord.de |
| Vertreten durch | Cristina Uhl |

🔴 **Offen am Verantwortlichen:**

- **Telefonnummer und USt-IdNr./Partita IVA sind Platzhalter.** In
  `imprint.html` stehen sie als Platzhaltertext („Telefonnummer (Pflichtangabe
  bei geschäftsmäßigem Auftritt)", „USt-IdNr. bzw. italienische Partita IVA,
  falls vorhanden"), erkennbar an der Auszeichnung `class="ph"`.
- **Rechtsform und Registereintrag** sind nirgends genannt. Für das
  Verzeichnis wird die genaue Bezeichnung des Verantwortlichen gebraucht.
- **Zuständige Aufsichtsbehörde ist nicht benannt.** Der Sitz liegt in
  Italien; das Impressum beruft sich aber auf deutsches Recht (§ 5 TMG,
  § 18 Abs. 2 MStV). Ob und wie beides zusammenpasst, ist eine Rechtsfrage —
  hier nur als Beobachtung vermerkt.
- **Datenschutzbeauftragter:** nicht erwähnt. Ob einer zu benennen ist, hängt
  von Umständen ab, die hier nicht sichtbar sind.

---

## Teil A — Belegte Verarbeitungen

Die Startseite ist eine statische Datei ohne jede eigene Datenverarbeitung.
Was übrig bleibt, sind zwei Vorgänge: die Auslieferung selbst und das, was
passiert, wenn jemand den einzigen Kontaktweg benutzt.

### A1 · Auslieferung der Website (Server-Protokolle)

| Feld | Angabe |
|---|---|
| **Zweck** | Bereitstellung und Auslieferung der Website; technischer Betrieb und Sicherheit |
| **Betroffene Personen** | Besucherinnen und Besucher von `www.lagonord.de` |
| **Datenkategorien** | Was ein Webserver beim Abruf zwangsläufig sieht: IP-Adresse, Zeitpunkt, abgerufene Ressource, User-Agent, Referrer. 🔴 Der tatsächliche Umfang hängt vom Hoster ab und ist aus dem Repo nicht ersichtlich. |
| **Dienst** | 🟢 Alles im Repo weist auf **GitHub Pages**: `CNAME` mit `www.lagonord.de`, Remote `github.com/TimBo1986/lagonord-website`, sechs Commits „Re-trigger GitHub Pages deployment" in der Historie, und **keinerlei Server- oder Containerkonfiguration** (kein `Dockerfile`, kein `docker-compose.yml`, keine Workflows, keine Webserver-Konfiguration). |
| **Sitz / Rechenzentrum** | 🔴 **Lücke.** GitHub Pages wird von GitHub, Inc. (Microsoft-Konzern, USA) betrieben — das ist allgemein bekannt, aber nicht aus dem Repo belegt. In welchen Rechenzentren ausgeliefert wird, ist von hier aus nicht feststellbar. |
| **Löschfristen** | 🔴 **Lücke.** Protokollaufbewahrung von GitHub Pages ist aus dem Repo nicht ersichtlich. |
| **Drittlandübermittlung** | 🔴 **Lücke.** Bei einem US-Anbieter naheliegend, aber zu prüfen, nicht zu vermuten. |
| **AV-Vertrag** | 🔴 **Lücke.** Ob mit GitHub ein Auftragsverarbeitungsvertrag (Data Protection Addendum) besteht, ist nur im GitHub-Konto zu sehen. |

**🟢 Was die Seite beim Aufruf nachweislich *nicht* tut:**

| Geprüft | Treffer in `index.html` / `sprachen.js` |
|---|---|
| `document.cookie` | 0 / 0 |
| `localStorage`, `sessionStorage`, `indexedDB` | 0 / 0 |
| `fetch(`, `XMLHttpRequest`, `WebSocket`, `sendBeacon` | 0 / 0 |
| `<form>`, `<input>`, `<textarea>`, `<select>` | 0 in allen drei HTML-Dateien |

Schriften sind als base64 im Dokument eingebettet (`<style id="schrift">`), es
werden also **keine Web-Fonts von fremden Servern** geladen. Beim Aufruf
entsteht dadurch **kein Request an einen Dritten** — die IP-Adresse der
Besucherin sieht ausschliesslich der Hoster.

**🟢 Ausgehende Verweise** (Links, keine geladenen Ressourcen):

- `https://unicabenaco.com` in `index.html`, Team-Abschnitt
- `https://ec.europa.eu/consumers/odr/` in `imprint.html` (Pflichtangabe)
- `https://www.lagonord.de/…` in den Open-Graph- und `canonical`-Angaben —
  diese liest nur ein Vorschau-Dienst beim Teilen, der Browser lädt sie nicht

**🟡 Beobachtung:** Der Link auf unicabenaco.com trägt `rel="noopener"`, aber
nicht `noreferrer`. Beim Klick wird also die Herkunfts-URL an unicabenaco.com
übermittelt. Ob das gewollt ist, ist eine Entscheidung, keine Panne — hier nur
vermerkt, weil es die einzige Stelle ist, an der die Seite selbst etwas nach
aussen gibt.

### A2 · Kontaktaufnahme per E-Mail

| Feld | Angabe |
|---|---|
| **Zweck** | Beantwortung von Anfragen, Anbahnung eines Erstgesprächs (vorvertragliche Massnahme) |
| **Betroffene Personen** | Interessentinnen und Interessenten, die von sich aus schreiben |
| **Datenkategorien** | Was die Absenderin selbst mitschickt: Absenderadresse, Name, Betreff, Nachrichtentext, technische Kopfzeilen der E-Mail. 🟢 Die Seite gibt nichts vor: es gibt kein Formular, nur einen `mailto:`-Verweis. Vorbelegt ist ausschliesslich der Betreff „Erstgespräch". |
| **Dienst** | 🟢 Einziger Kontaktweg der Seite ist `mailto:info@lagonord.de?subject=Erstgespräch` — **genau ein** `mailto:`-Verweis in `index.html`, der Knopf im Schlussabschnitt. In der Fusszeile steht dieselbe Adresse als reiner Text, nicht als Link. 🔴 **Wer die Postfächer betreibt, ist im Repo nicht hinterlegt.** `privacy.html` nennt Zoho Mail — das ist eine Angabe der Rechtsseite, kein Beleg aus dem Code. |
| **Sitz / Rechenzentrum** | 🔴 **Lücke.** In einer Minute über die MX-Einträge von `lagonord.de` und das Konto des Anbieters zu klären. |
| **Löschfristen** | 🔴 **Lücke.** Wie lange Anfragen im Postfach bleiben, ist eine betriebliche Festlegung, die es geben muss und die nirgends steht. |
| **Drittlandübermittlung** | 🔴 **Lücke.** |
| **AV-Vertrag** | 🔴 **Lücke.** `privacy.html` formuliert „Mit Zoho besteht, **soweit erforderlich**, ein Auftragsverarbeitungsvertrag" — das ist ein Vorbehalt, keine Bestätigung. |

---

## Teil B — Verarbeitungen ausserhalb dieses Repos

Hier steht bewusst nichts als Inhalt. Jeder Punkt ist eine Lücke mit der
Frage, die zu beantworten ist.

### B1 · 🔴 E-Mail-Betrieb

Anbieter, Rechenzentrum, Aufbewahrung, AV-Vertrag, Zugriffsberechtigte.
**Klärbar durch:** Tim, über MX-Einträge und das Anbieterkonto.

### B2 · 🔴 KI-Assistenten und n8n-Workflows

Welche Assistenten laufen tatsächlich? Welche Daten nehmen sie auf, wohin
schreiben sie, welches Sprachmodell verarbeitet den Text und wo läuft es?
Werden Verläufe gespeichert, wie lange?

**Aus dem Repo ist dazu nichts ersichtlich** — die Startseite bindet keinen
Assistenten ein; die Chats in der Vorführung sind ausgezeichnete
Beispielszenarien im HTML, keine Anwendung. **Klärbar durch:** Tim.

### B3 · 🔴 Datenbank

Welche personenbezogenen Daten liegen dort, mit welcher Rechtsgrundlage,
wie lange, wer hat Zugriff, wo steht der Server, gibt es ein Löschkonzept.
**Klärbar durch:** Tim.

### B4 · 🔴 Zoho

`privacy.html` nennt Zoho Mail. Ob darüber hinaus weitere Zoho-Dienste
(CRM, Formulare, Kalender) im Einsatz sind, ist von hier aus nicht zu sehen.
**Klärbar durch:** Tim und Cristina.

### B5 · 🔴 Die App (Unica Benaco / Garda Unica)

Aus `assets/unica_app.jpg` ist ersichtlich, dass eine Anwendung existiert, die
Personen mit Namen anspricht — mehr nicht. Nutzerkonten, Standortdaten,
Push-Nachrichten, Analyse, App-Store-Verarbeitung: alles unbekannt.

**Wichtig für die Abgrenzung:** Ob Unica Benaco überhaupt derselbe
Verantwortliche ist wie LagoNord AI, geht aus dem Repo nicht hervor. Ist es ein
eigener Verantwortlicher, braucht es ein eigenes Verzeichnis — und für die
Verlinkung auf der Startseite womöglich eine Erwähnung in der
Datenschutzerklärung. **Klärbar durch:** Tim und Cristina.

### B6 · 🔴 Weitere in `privacy.html` genannte Empfänger

Die Rechtsseite nennt Google Sheets/Drive, HubSpot, WhatsApp Business API und
Dify. **Keiner dieser Dienste erscheint irgendwo im Quelltext.** Ob sie im
Einsatz sind, ist eine Betriebsfrage — siehe dazu Teil C.

### B7 · 🔴 Geplante, noch nicht gebaute Verarbeitungen

`docs/MAP-relaunch-v7.md` sieht drei Module vor, die neue Verarbeitungen
schaffen würden: `rueckkanal` (Kontaktformular, Terminbuchung),
`messung` (Erstanbieter-Analytik) und die Ablösung der Sprachumschaltung.
Die Map nennt dafür Hetzner, n8n, Brevo, PostgreSQL, Cal.com sowie Umami oder
Plausible.

**Noch nicht gebaut** — im Quelltext ist nichts davon vorhanden. Sie gehören
ins Verzeichnis, sobald sie live gehen, nicht vorher. Die Map hält für die
Messung bereits einen offenen rechtlichen Punkt fest: ob cookielose
Erstanbieter-Analytik einwilligungsfrei zulässig ist.

---

## Teil C — Widersprüche zwischen `privacy.html` und dem Quelltext

Diese Punkte sind **keine Vermutung**, sondern eine Gegenüberstellung von zwei
Dokumenten, die beide im Repo liegen.

### C1 · Hosting

> `privacy.html`, Abschnitt 3: „Diese Website wird auf einem Server der
> **Hetzner Online GmbH**, Industriestr. 25, 91710 Gunzenhausen, Deutschland,
> gehostet. Mit Hetzner besteht ein Auftragsverarbeitungsvertrag (AVV)."

🟢 Dem steht das Repo entgegen: `CNAME`, GitHub-Remote, sechs
Pages-Deployment-Commits, keine Serverkonfiguration. **Die Aussage über den
Hoster passt nicht zu dem, was hier ausgeliefert wird.** Eine der beiden
Angaben ist falsch — welche, weiss nur Tim. Das ist der schwerwiegendste
Punkt in diesem Dokument, weil daran der genannte AV-Vertrag und die
Drittlandfrage hängen.

### C2 · Produktnamen

`privacy.html` beschreibt Assistenten namens **LagoEstate** und **LagoStay**.
Die Startseite kennt diese Namen nicht; sie führt LagoHost und LagoTerra als
Linien sowie Il Segretario, Il Portiere, Il Ricordo, L'Agenda und Il
Cerimoniere als Rollen. Die Rechtsseite beschreibt einen älteren Stand.

### C3 · Genannte Dienste

Dify, Google Sheets/Drive, HubSpot und die WhatsApp Business API stehen in der
Empfängerliste der Rechtsseite. Die Relaunch-Map dagegen sieht n8n, Brevo,
PostgreSQL und Cal.com vor. Zwei Dokumente im selben Repo beschreiben zwei
verschiedene Landschaften.

### C4 · Cookies — hier stimmt es

> `privacy.html`, Abschnitt 9: „Diese Website verwendet aktuell keine
> Marketing- oder Tracking-Cookies."

🟢 **Bestätigt.** Kein `document.cookie`, kein Speicherzugriff, kein
Analysewerkzeug, kein Einwilligungsbanner — und da die Schriften eingebettet
sind, auch kein Umweg über einen Schriftdienst.

---

## Teil D — Was zu klären ist

Nach Dringlichkeit, nicht nach Aufwand.

1. **Wer hostet die Seite wirklich?** (C1) Davon hängen Hoster, Rechenzentrum,
   Drittlandübermittlung und AV-Vertrag der einzigen belegten Verarbeitung ab.
   Die Datenschutzerklärung ist aktuell live und trifft dazu eine Aussage.
2. **Stimmt die Empfängerliste in `privacy.html`?** (C3) Dienste, die nicht
   eingesetzt werden, gehören dort ebenso wenig hin wie fehlende.
3. **Wer betreibt die Postfächer, mit welcher Aufbewahrung?** (A2, B1)
4. **Ist das Repository öffentlich?** Von hier aus nicht feststellbar. Ist es
   öffentlich, sind Commit-Historie und `FORTSCHRITT.md` mitsamt Namen und
   Arbeitsnotizen für jeden lesbar.
5. **Löschfristen festlegen** — für Anfragen, Assistentenverläufe,
   Datenbankinhalte. Ohne sie fehlt dem Verzeichnis eine Pflichtangabe
   (Art. 30 Abs. 1 lit. f).
6. **Technische und organisatorische Massnahmen** (Art. 30 Abs. 1 lit. g)
   sind hier gar nicht behandelt, weil dazu im Repo nichts steht.
7. **Impressum vervollständigen** — Telefon und Steuernummer sind Platzhalter.
8. **Abgrenzung zu Unica Benaco** (B5) vor der aktiven Bewerbung klären.

---

## Fundstellen

Alle 🟢-Aussagen dieses Dokuments lassen sich so nachprüfen:

```sh
# Hosting-Belege
cat CNAME && git remote get-url origin
git log --oneline --all | grep -i pages
ls Dockerfile docker-compose.yml .github/workflows 2>/dev/null   # nichts davon existiert

# Keine Speicherung, keine eigenen Requests, keine Formulare
grep -c "document.cookie\|localStorage\|sessionStorage\|indexedDB" index.html sprachen.js
grep -c "fetch(\|XMLHttpRequest\|WebSocket\|sendBeacon" index.html sprachen.js
grep -c "<form\|<input\|<textarea\|<select" index.html imprint.html privacy.html

# Fremde Domains insgesamt
grep -oh 'https\?://[a-z0-9.-]*' index.html sprachen.js imprint.html privacy.html | sort -u

# Kontaktweg
grep -o 'mailto:[^"]*' index.html
```
