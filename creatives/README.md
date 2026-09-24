# Ad-Creatives · Physiotherapeut:in (m/w/d)

Acht Recruiting-Motive für Meta in zwei Formaten – **Feed 4:5 (1080×1350)** und
**Story/Reels 9:16 (1080×1920)**. Gebaut als HTML/CSS in [`index.html`](index.html),
gerendert als PNG nach [`png/`](png). Farben, Logo und Schriften kommen aus
derselben CI wie die Karriereseite.

Die Anzeigentexte stehen in [`WERBETEXTE.md`](WERBETEXTE.md) – ein Satz für alle
Motive, damit sich die Motive sauber gegeneinander messen lassen.

## Die acht Konzepte

Alle kommen **ohne Fotos** aus: nur Typografie, CI-Farben und das Logo.
Sie sind damit sofort schaltbar.

| Konzept | Hook | Fläche |
|---|---|---|
| `wochenende` | „Samstag gehört dir." – Mo–Fr, 07–19 Uhr, keine Wochenenddienste | dunkelgrüner Verlauf |
| `fortbildung` | „Deine nächste Fortbildung zahlen wir." – MT, MLD, Bobath, CMD | helle Fläche |
| `dreizehn` | „13 Kolleg:innen. Eine:r fehlt noch." – Team und Rezeption | Türkis |
| `ohne-lebenslauf` | „Vier Fragen. Kein Lebenslauf." – Anti-Aufwand, gut fürs Retargeting | dunkelgrüner Verlauf |
| `zeit-pro-patient` | „Wie viel Zeit bleibt dir pro Patient?" – die Frage, die keiner stellt | helle Fläche |
| `spektrum` | „Kein Tag wie der davor." – das Behandlungsspektrum als Wortfeld | dunkelgrüner Verlauf |
| `rezeption` | „Termine macht die Rezeption. Du machst Therapie." – weniger Papierkram | Türkis |
| `seit-1998` | „1998 gegründet. Nie ein Fließband." – Stabilität | dunkelgrüner Verlauf |

Bewusst kein „Wir suchen …": Jedes Motiv nennt einen konkreten Grund, warum die
Stelle besser ist als die aktuelle – das zieht qualifizierte Bewerber:innen an
statt möglichst vieler.

**Nicht alle acht gleichzeitig schalten.** Bei 15–25 € Tagesbudget zersplittert
das die Ausspielung, und keins bekommt genug Daten. Mit vier starten, nach
5–7 Tagen die schwächsten zwei gegen frische tauschen (Vorschlag in
[`WERBETEXTE.md`](WERBETEXTE.md)).

## Neu rendern

```bash
pip install playwright        # Chromium wird über PLAYWRIGHT_BROWSERS_PATH gefunden
python3 render.py             # überschreibt die PNGs in png/
```

`render.py` startet einen lokalen Server (die Schriften unter `../assets/fonts/`
lassen sich per `file://` nicht laden), öffnet `index.html?render=1` und
fotografiert jedes `.canvas`-Element. Der Dateiname kommt aus `data-name`.

## Motive ändern oder ergänzen

Alles steckt im `MOTIVE`-Array unten in [`index.html`](index.html):

```js
{
  name:'wochenende',              // Dateiname: wochenende-45.png / wochenende-story.png
  theme:'t-dark',                 // t-dark | t-mint | t-light
  eyebrow:'Physiotherapeut:in (m/w/d)',
  h1:'Samstag gehört <em>dir</em>.',   // <em> färbt das Wort im Akzentton
  sub:'…',
  ticks:['…','…'],                // optionale Hakenliste
  bignum:'13'                     // optionale grosse Zahl über der Headline
}
```

Ein neuer Eintrag erzeugt beim nächsten `render.py` automatisch beide Formate.

## Sobald Bildmaterial da ist

Die Motive sind so gebaut, dass ein Foto als Hintergrund ergänzt werden kann,
ohne die Textebene anzufassen: Foto in `../bilder/` ablegen, im `.canvas` als
`background-image` setzen und den bestehenden Farbverlauf als Overlay
darüberlegen. `wochenende` und `dreizehn` profitieren am meisten von einem
echten Team- oder Praxisbild.
