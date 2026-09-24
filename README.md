# Karriereseite · Physiotherapiepraxis Gscheidle GmbH

Recruiting-Landingpage mit Bewerber-Vorfilter für die **Physiotherapiepraxis Gscheidle GmbH**
in Rottweil. Ausgeschriebene Stelle: **Physiotherapeut:in (m/w/d)**.

Aufbau und Funnel-Logik entsprechen der ALWA-Seite (`saviold/alwa-gmbh`); Farben, Schriften,
Logo und sämtliche Texte sind auf die Praxis Gscheidle umgestellt.

Eine einzige, in sich geschlossene Datei – kein Build, keine Abhängigkeiten, keine
Fremd-Requests (Schriften liegen im Repo).

**Live (nach Aktivierung von GitHub Pages):** https://saviold.github.io/Gscheidle-Physiotherapie/

---

## Vor dem Livegang: Empfänger eintragen

In `index.html` im `<script>`-Block ganz unten:

```js
var WEB3FORMS_KEY = "";   /* <<< Access Key von web3forms.com hier einsetzen */
var WEBHOOK_URL   = "";   /* optional: LeadTable Generic Webhook */
```

**Web3Forms** ist der vorgesehene Weg: Jede abgeschlossene Bewerbung geht als
formatierte E-Mail direkt an die Praxis. Die Empfängeradresse wird im
web3forms-Konto hinterlegt, nicht hier – sie steht damit auch nicht im
Quelltext und lässt sich ohne Code-Änderung wechseln. Die Mail trägt den
Bewerber als `Reply-To`, ein Klick auf Antworten geht also direkt an ihn.

**LeadTable** kann optional zusätzlich laufen. Sind beide gesetzt, geht die
Bewerbung an beide; es reicht, wenn einer der Wege durchkommt.

Ist keiner von beiden gesetzt, wird **nichts** übertragen und über dem Formular
erscheint ein oranger Hinweis für die Praxis. Sobald ein Empfänger drinsteht,
verschwindet der Hinweis von selbst.

## Aufbau der Seite

1. **Topbar** – Logo, Ankernavigation, „Jetzt bewerben“
2. **Hero** – Logo, Stellentitel, Kurzversprechen, zwei CTAs (passt komplett in den ersten Bildschirm)
3. **Trust-Strip** – Seit 1998 · 7 Kolleg:innen · Mo–Fr 07–19 Uhr · Königstraße 28
4. **Die Stelle** – Aufgaben und Anforderungen, Pflicht und Wünschenswert getrennt ausgewiesen
5. **Warum Gscheidle** – sechs Benefits
6. **Ablauf** – drei Schritte
7. **Mid-CTA**
8. **Bewerbung** – der fünfstufige Funnel
9. **FAQ** – sechs Fragen
10. **Footer** – Kontakt, Öffnungszeiten, Impressum, Datenschutz

## Der Vorfilter

Vier Fragen, jede eindeutig als **Pflichtkriterium** oder **Wünschenswert** gekennzeichnet,
danach ein Kontaktschritt. Immer nur eine Frage pro Schritt.

| # | Frage | Kategorie | Verhalten |
|---|---|---|---|
| 1 | Abgeschlossene Ausbildung / Studium als Physiotherapeut:in? | **Pflicht** | „Nein – keine Ausbildung“ → sofortiger Abbruch |
| 2 | Deutschkenntnisse | **Pflicht** | „Grundkenntnisse (bis B1)“ → sofortiger Abbruch |
| 3 | Zusatzqualifikationen (MT, MLD, Bobath, KGG, CMD) – Mehrfachauswahl | Wünschenswert | „Noch keine“ → geht weiter, wird als *nicht erfüllt* übertragen |
| 4 | Hausbesuche / Führerschein Klasse B | Wünschenswert | „Lieber nur Praxis“ → geht weiter, wird als *nicht erfüllt* übertragen |
| 5 | Kontaktdaten, Wunsch-Umfang, frühester Start | — | Absenden |

**Pflichtkriterium nicht erfüllt:** Die Bewerbung endet sofort auf einem freundlichen
Absage-Screen mit der Telefonnummer der Praxis als Weg zurück. Es wird **nichts** an den
Webhook übertragen – K.-o.-Abbrüche erzeugen also keine Leads.

**Optionales Kriterium nicht erfüllt:** Der Bewerber läuft ganz normal weiter. Die Antwort
wird übertragen und im Feld selbst mit `(Optional: nicht erfüllt)` markiert, zusätzlich im
Sammelfeld `nicht_erfuellt`.

Alle Fragen sind rein berufsbezogen. Es wird nicht nach Alter, Herkunft, Gesundheit,
Religion oder Familienstand gefragt (AGG).

## Was übertragen wird

Nur abgeschlossene, qualifizierte Bewerbungen. In der E-Mail stehen die Felder
ausgeschrieben, an LeadTable gehen dieselben Daten als JSON:

| Feld (E-Mail) | JSON-Schlüssel | Inhalt |
|---|---|---|
| Stelle | `stelle` | Physiotherapeut:in (m/w/d) |
| Name, Telefon, E-Mail | `vorname`, `nachname`, `telefon`, `email` | Kontaktdaten |
| Wunsch-Umfang, Frühester Start | `umfang`, `starttermin` | Pensum und Einstieg |
| Berufsabschluss / Deutschkenntnisse (Pflicht) | `qualifikation`, `deutsch` | jeweils mit `(Pflicht: erfüllt)` |
| Zusatzqualifikationen / Hausbesuche (optional) | `zusatzqualifikation`, `hausbesuche` | mit `(Optional: erfüllt)` bzw. `(Optional: nicht erfüllt)` |
| Optionale Kriterien | `optionale_kriterien` | z. B. `1 von 2 erfüllt` |
| Davon offen | `nicht_erfuellt` | Klartextliste, sonst `–` |
| Bewertung | `match` | `Top-Match` / `Guter Match` / `Grundprofil erfüllt` |
| Eingegangen am, Einwilligung, Quelle | `datum`, `datenschutz`, `quelle`, `seite` | Metadaten |

## Mobile Laufruhe

Der Funnel ist so gebaut, dass am Handy beim Fragenwechsel **nichts springt**:

- kein `window.scrollTo`, kein `scrollIntoView`, kein automatisches `focus()`,
  kein Reload und kein Anker-/Hash-Sprung beim Schrittwechsel
- die Schritt-Bühne (`#fsteps`) bekommt per JS eine feste Mindesthöhe in Höhe des
  höchsten Schritts – neu gemessen nach dem Laden der Schriften und bei echter
  Breitenänderung, aber nicht, wenn nur die Adressleiste ein- oder ausfährt
- Fortschrittsbalken oben und Weiter-Button unten bleiben dadurch pixelgenau stehen
- Einfachauswahl springt nach 260 ms automatisch weiter, die Mehrfachauswahl in
  Schritt 3 wartet auf „Weiter“

Nachgemessen mit Playwright auf 390×844, 360×640 und 430×932: Scrollposition,
Bühnenposition und Button-Position ändern sich über alle Schritte um **0 px**, und die
komplette Formularkarte passt auf allen drei Größen ohne Scrollen ins Display.

## Bilder

Die Seite läuft ohne Bildmaterial – im Hero steht dann der CI-Farbverlauf.
Sobald ein Foto unter `bilder/hero.jpg` liegt, wird es automatisch eingeblendet
(Details siehe `bilder/HIER-BILDER-ABLEGEN.txt`).

## CI

Direkt aus dem Logo entnommen:

| Rolle | Wert |
|---|---|
| Türkis (Figur, Akzente, CTA im Hero) | `#59C3B7` |
| Dunkelgrün (Wortmarke, Überschriften) | `#134842` |
| Primär-Button | `#1E7A70` |
| Flächen hell | `#F2FAF8` / `#E7F6F3` |

Schriften: **Montserrat** (Überschriften, nah an der Wortmarke) und **Inter** (Fließtext),
beide als woff2 unter `assets/fonts/` selbst gehostet.

Das Logo liegt freigestellt in zwei Varianten vor: `assets/logo.png` (Originalfarben)
und `assets/logo-weiss.png` (weiße Wortmarke für dunkle Flächen).

## Live schalten (GitHub Pages)

Repo-Settings → **Pages** → Source: *Deploy from a branch*, Branch: `main` / `/root`.
Die Datei `.nojekyll` sorgt dafür, dass alles 1:1 ausgeliefert wird.
