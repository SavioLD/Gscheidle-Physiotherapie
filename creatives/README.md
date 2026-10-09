# Ad-Creatives · Physiotherapeut:in (m/w/d)

Elf Recruiting-Motive für Meta in **drei Formaten** – Feed **4:5** (1080×1350),
Feed **1:1** (1080×1080) und Story/Reels **9:16** (1080×1920). Jedes Motiv wird
automatisch in allen drei Formaten gerendert, macht 33 Dateien in [`png/`](png).
Gebaut als HTML/CSS in [`index.html`](index.html). Farben, Logo und Schriften
kommen aus derselben CI wie die Karriereseite.

Die Anzeigentexte stehen in [`WERBETEXTE.md`](WERBETEXTE.md): drei Primärtexte,
dazu eine Überschrift und eine Beschreibung, die über alle Anzeigen gleich
bleiben – so wackelt beim Testen nicht alles gleichzeitig.

Dazu kommen drei **organische Instagram-Beiträge zur Arbeitgebermarke**, die
bewusst keine Stelle ausschreiben: [`INSTAGRAM.md`](INSTAGRAM.md).

## Die elf Konzepte

Drei Motive liegen auf dem echten Praxisfoto, die übrigen arbeiten nur mit
Typografie, CI-Farben und Logo. Alle sind sofort schaltbar.

| Konzept | Überschrift | Fläche |
|---|---|---|
| `stelle-dienstwagen` | **Physiotherapeut:in (m/w/d) gesucht** + Badge „Dienstwagen · auch privat" | **Praxisfoto** |
| `stelle-null-euro` | **Physiotherapeut:in (m/w/d) gesucht** + Badge „0 € Eigenanteil" | Türkis |
| `stelle-sonntags` | **Physiotherapeut:in (m/w/d) in Rottweil** + Badge „Auch sonntags dein Auto" | helle Fläche |
| `wochenende` | „Samstag gehört dir." – keine Wochenenddienste | **Praxisfoto** |
| `zeit-pro-patient` | „Wie viel Zeit bleibt dir pro Patient?" | helle Fläche |
| `sieben` | „7 Kolleg:innen. Eine:r fehlt noch." | Türkis |
| `ohne-lebenslauf` | „Vier Fragen. Kein Lebenslauf." – gut fürs Retargeting | **Praxisfoto** |
| `fortbildung` | „Fortbildung? Zahlen wir." | helle Fläche |
| `spektrum` | „Kein Tag wie der davor." – Behandlungsspektrum als Wortfeld | dunkelgrüner Verlauf |
| `rezeption` | „Termine macht die Rezeption. Du machst Therapie." | Türkis |
| `seit-1998` | „1998 gegründet. Nie ein Fließband." | dunkelgrüner Verlauf |

**Die ersten drei Motive nennen die Stelle in der Überschrift** und den
Dienstwagen als hervorgehobenes Badge direkt darunter – wer scrollt, sieht
zuerst, worauf er sich bewirbt, und erst danach den Vorteil. Die übrigen Motive
steigen über einen einzelnen Vorteil ein und tragen die Stelle in der Kopfzeile.

Bewusst kein „Wir suchen …": Jedes Motiv nennt einen konkreten Grund, warum die
Stelle besser ist als die aktuelle – das zieht qualifizierte Bewerber:innen an
statt möglichst vieler.

**Nicht alle elf gleichzeitig schalten.** Bei 15–25 € Tagesbudget zersplittert
das die Ausspielung, und keins bekommt genug Daten. Mit vier starten, nach
5–7 Tagen die schwächsten zwei gegen frische tauschen; drei davon spielen den
Dienstwagen, weil er das stärkste Argument der Praxis ist (Vorschlag in
[`WERBETEXTE.md`](WERBETEXTE.md)).

## Beiträge zur Arbeitgebermarke

| Beitrag | Überschrift | Fläche |
|---|---|---|
| `marke-empfang` | „Am Empfang laufen die Fäden zusammen." – Entlastung | **Praxisfoto** |
| `marke-praxis` | „Eine Praxis. Kurze Wege." – kleine Praxis, kurze Absprachen | Türkis |
| `marke-fortbildung` | „Fortbildung bleibt im Team." – Weiterentwicklung | helle Fläche |

Diese drei tragen das Feld `brand:true`. Damit fällt der „Jetzt bewerben"-Knopf
weg und im Fuß steht nur die Praxis – es sind Beiträge, keine Anzeigen.

Alle drei beantworten dieselbe Frage: **wie ist es, hier zu arbeiten?** Deshalb
durchgängig die Kopfzeile „Arbeiten bei uns". Sie sind bewusst schlicht und
allgemein gehalten: keine Rolle wird einem Geschlecht zugeordnet, es gibt keinen
Vergleich mit anderen Arbeitgebern und keine Superlative. Die Begründung dazu
und die Texte stehen in [`INSTAGRAM.md`](INSTAGRAM.md).

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
  bignum:'7',                     // optionale grosse Zahl über der Headline
  badge:'Dienstwagen · auch privat', // optionales Badge unter der Headline
  photo:'behandlungsraum.jpg',    // optionales Foto, nur mit theme t-dark
  brand:true                      // Beitrag statt Anzeige: ohne Bewerben-Knopf
}
```

Lange, nicht trennbare Wörter wie „Physiotherapeut:in" wären bei voller
Schriftgröße breiter als die Fläche. Die Überschrift wird deshalb beim Rendern
automatisch so weit verkleinert, bis sie passt – man muss nichts von Hand
nachstellen.

Ein neuer Eintrag erzeugt beim nächsten `render.py` automatisch alle drei Formate.

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

Aktuell nutzen `stelle-dienstwagen`, `wochenende` und `ohne-lebenslauf` das Foto
`bilder/behandlungsraum.jpg`. Kommen weitere Fotos dazu, einfach ablegen und
bei weiteren dunklen Motiven (`spektrum`, `seit-1998`) eintragen – unterschiedliche
Bilder pro Motiv sind besser als dasselbe überall.
