# Ad-Creatives · Physiotherapeut:in (m/w/d)

Elf Recruiting-Motive für Meta in zwei Formaten – **Feed 4:5 (1080×1350)** und
**Story/Reels 9:16 (1080×1920)**. Gebaut als HTML/CSS in [`index.html`](index.html),
gerendert als PNG nach [`png/`](png). Farben, Logo und Schriften kommen aus
derselben CI wie die Karriereseite.

Die Anzeigentexte stehen in [`WERBETEXTE.md`](WERBETEXTE.md): drei Primärtexte,
dazu eine Überschrift und eine Beschreibung, die über alle Anzeigen gleich
bleiben – so wackelt beim Testen nicht alles gleichzeitig.

## Die elf Konzepte

Zwei Motive liegen auf dem echten Praxisfoto, die übrigen arbeiten nur mit
Typografie, CI-Farben und Logo. Alle sind sofort schaltbar.

| Konzept | Hook | Fläche |
|---|---|---|
| `dienstwagen-null` | „0 € für deinen Dienstwagen.“ – der Preis als Aufhänger | dunkelgrüner Verlauf |
| `dienstwagen` | „Dienstwagen. Auch privat.“ – gestellt, ohne Eigenanteil | Türkis |
| `dienstwagen-privat` | „Auch sonntags dein Auto.“ – die private Nutzung als Pointe | helle Fläche |
| `wochenende` | „Samstag gehört dir." – Mo–Fr, 07–19 Uhr, keine Wochenenddienste | **Praxisfoto** |
| `fortbildung` | „Fortbildung? Zahlen wir." – MT, MLD, Bobath, CMD, nach Absprache | helle Fläche |
| `sieben` | „7 Kolleg:innen. Eine:r fehlt noch." – Team und Rezeption | Türkis |
| `ohne-lebenslauf` | „Vier Fragen. Kein Lebenslauf." – Anti-Aufwand, gut fürs Retargeting | **Praxisfoto** |
| `zeit-pro-patient` | „Wie viel Zeit bleibt dir pro Patient?" – die Frage, die keiner stellt | helle Fläche |
| `spektrum` | „Kein Tag wie der davor." – das Behandlungsspektrum als Wortfeld | dunkelgrüner Verlauf |
| `rezeption` | „Termine macht die Rezeption. Du machst Therapie." – weniger Papierkram | Türkis |
| `seit-1998` | „1998 gegründet. Nie ein Fließband." – Stabilität | dunkelgrüner Verlauf |

Bewusst kein „Wir suchen …": Jedes Motiv nennt einen konkreten Grund, warum die
Stelle besser ist als die aktuelle – das zieht qualifizierte Bewerber:innen an
statt möglichst vieler.

**Nicht alle elf gleichzeitig schalten.** Bei 15–25 € Tagesbudget zersplittert
das die Ausspielung, und keins bekommt genug Daten. Mit vier starten, nach
5–7 Tagen die schwächsten zwei gegen frische tauschen; drei davon spielen den
Dienstwagen, weil er das stärkste Argument der Praxis ist (Vorschlag in
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

## Fotos in Motiven

Ein Motiv bekommt ein Foto, indem im `MOTIVE`-Eintrag `photo` gesetzt wird –
der Dateiname relativ zu `../bilder/`:

```js
{ name:'wochenende', theme:'t-dark', photo:'behandlungsraum.jpg', … }
```

Das Bild liegt dann ganz hinten, darüber legt sich automatisch der CI-Schleier,
damit die Typografie lesbar bleibt; das Figuren-Wasserzeichen entfällt bei
Fotomotiven. Fotos funktionieren nur mit `t-dark` – auf den hellen und
türkisen Flächen stünde weiße Schrift auf hellem Bild.

Aktuell nutzen `wochenende` und `ohne-lebenslauf` das Foto
`bilder/behandlungsraum.jpg`. Kommen weitere Fotos dazu, einfach ablegen und
bei weiteren dunklen Motiven (`spektrum`, `seit-1998`) eintragen – unterschiedliche
Bilder pro Motiv sind besser als dasselbe überall.
