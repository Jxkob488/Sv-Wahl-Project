# Präsentationen mit Markdown und reveal.js

Du schreibst deine Folien in [Markdown](https://www.markdownguide.org/basic-syntax/), genau wie eine `Readme.md`. [reveal.js](https://revealjs.com/) zeigt sie im Browser als Präsentation an.

Warum nicht PowerPoint?

- Deine Folien sind Text: Du verwaltest sie mit Git, siehst jede Änderung im Diff und arbeitest im Team – ganz ohne „final_v3_wirklich_final.pptx“.
- Codebeispiele werden automatisch mit Syntaxhervorhebung dargestellt.
- Das Ergebnis ist eine Webseite und läuft in jedem Browser, auch auf GitHub Pages.

Die Beispielpräsentation in diesem Ordner ([slides.md](./slides.md)) zeigt alle Bausteine aus dieser Anleitung.

## 1. Voraussetzungen

- [Node.js](https://nodejs.org/) Version 22 (LTS) oder neuer. Prüfe die Version mit `node -v`.
- Ein Editor, z. B. VS Code.

## 2. Beispiel ausprobieren

```bash
cd 01-ProjectStart/revealjs
npm install
npm start
```

Dein Browser öffnet `http://localhost:8000`. Ändere etwas in `slides.md` und speichere die Datei: Die Seite wird automatisch neu geladen. Beende den Server mit `Ctrl + C`.

## 3. Eigenes Projekt erstellen

Ein Projekt besteht nur aus wenigen Dateien:

```text
my-presentation/
├── images/        Bilder für die Folien
├── index.html     lädt reveal.js und liest slides.md (einmal kopieren)
├── slides.md      deine Folien (die einzige Datei, die du bearbeitest)
├── package.json
└── .gitignore
```

1. Erstelle den Ordner und installiere die Pakete:

   ```bash
   mkdir my-presentation
   cd my-presentation
   npm init -y
   npm install reveal.js
   npm install -D browser-sync
   mkdir images
   ```

   `reveal.js` ist die Präsentationssoftware. `browser-sync` ist ein kleiner Webserver, der den Browser bei jedem Speichern neu lädt.

2. Kopiere [index.html](./index.html) aus diesem Ordner in dein Projekt.

3. Ersetze in `package.json` den `"scripts"`-Block durch:

   ```json
   "scripts": {
     "start": "browser-sync start --server --files \"index.html\" \"slides.md\" \"images/**\" --port 8000 --no-notify"
   }
   ```

4. Erstelle eine `.gitignore`-Datei, damit die installierten Pakete nicht im Repository landen:

   ```text
   node_modules/
   ```

Wer dein Repository klont, führt `npm install` und `npm start` aus.

## 4. Deine erste Folie

Erstelle `slides.md`:

```markdown
# Unser Projekt

Anna Huber, Ben Koller

---

## Aktuelle Situation

- Die Schule verwaltet Leihgeräte in Excel
- Niemand weiß, wer welchen Laptop hat

---

## Ziel

Eine Web-App, die jede Ausleihe erfasst.
```

Eine Zeile mit `---` beginnt eine neue Folie. Davor und danach muss jeweils eine Leerzeile stehen.

Starte die Präsentation mit `npm start` und wechsle mit den Pfeiltasten zwischen den Folien.

## 5. Bausteine für Folien

### Vertikale Folien

Eine Zeile mit `--` erstellt eine Unterfolie. Du erreichst sie mit der Pfeiltaste nach unten. Das ist nützlich für Details, die du bei Zeitmangel überspringen kannst.

```markdown
## Architektur

--

## Frontend

Angular

--

## Backend

Quarkus
```

### Aufzählungspunkte nacheinander einblenden

Füge nach jedem Aufzählungspunkt einen Kommentar ein, der ihn zu einem „Fragment“ macht:

```markdown
- zuerst das <!-- .element: class="fragment" -->
- dann das <!-- .element: class="fragment" -->
```

### Code mit hervorgehobenen Zeilen

Gib nach der Sprache in eckigen Klammern die hervorzuhebenden Zeilen an: `[3]` für eine Zeile, `[2-4]` für einen Bereich.

````markdown
```java [3]
public class HelloWorld {
    public static void main(String[] args) {
        System.out.println("Hello, World!");
    }
}
```
````

Mit `|` getrennt werden die Zeilen nacheinander hervorgehoben – bei jedem Weiterblättern die nächste: `[1|2|3]`.

### Bilder

Bilder gehören in den Ordner `images`.

```markdown
![Architektur](images/architecture.png)
```

Wenn das Bild zu groß ist, lege seine Höhe fest (`60vh` = 60 % der Bildschirmhöhe):

```markdown
![Architektur](images/architecture.png) <!-- .element: style="height: 60vh" -->
```

### Zwei Spalten

Markdown unterstützt keine Spalten. Dafür definiert `index.html` die Klasse `columns`. Die Leerzeilen um den Markdown-Inhalt sind wichtig, sonst wird er nicht umgewandelt.

```markdown
## Vorher und nachher

<div class="columns">
<div>

![Excel](images/excel.png)

</div>
<div>

- Excel-Liste
- kein Verlauf

</div>
</div>
```

### Tabellen

```markdown
| Rolle | Person |
| --- | --- |
| Produktverantwortliche Person | Anna |
| Scrum Master | Ben |
```

### Notizen für Vortragende

Nur die vortragende Person sieht die Notizen. Alles nach einer Zeile `Note:` bis zum Ende der Folie ist eine Notiz. Drücke `S`, um die Ansicht für Vortragende in einem eigenen Fenster zu öffnen: aktuelle Folie, nächste Folie, Notizen und eine Uhr.

```markdown
## Ziel

Eine Web-App, die jede Ausleihe erfasst.

Note:
Erzähle hier die Geschichte vom verlorenen Laptop.
```

### Hintergrund

Ein Kommentar am Anfang der Folie:

```markdown
<!-- .slide: data-background-color="#1e3a5f" -->

## Danke!
```

Ein Bild als Hintergrund der ganzen Folie:

```markdown
<!-- .slide: data-background-image="images/title.jpg" -->

## Willkommen
```

### Designvorlage

Das Erscheinungsbild legst du in `index.html` in der Zeile mit `theme/white.css` fest. Statt `white` kannst du `black`, `league`, `beige`, `night`, `serif`, `simple`, `solarized`, `moon`, `dracula`, `sky` oder `blood` verwenden. Auf einem Beamer in einem hellen Raum sind `white` und `simple` am besten lesbar.

## 6. Präsentieren

| Taste | Funktion |
| --- | --- |
| Pfeiltasten, Leertaste | zwischen Folien wechseln |
| `F` | Vollbild |
| `S` | Ansicht für Vortragende mit Notizen |
| `O` oder `Esc` | Übersicht aller Folien |
| `B` oder `.` | schwarzer Bildschirm |
| `Alt` + Klick | Bereich vergrößern |

## 7. Als PDF speichern

1. Öffne die Präsentation mit `npm start`.
2. Ergänze die Adresse um `?print-pdf`: `http://localhost:8000/?print-pdf`
3. Drucke in **Chrome oder Edge** (`Ctrl + P`): Ziel „Als PDF speichern“, Querformat, Ränder „Keine“, Hintergrundgrafiken aktivieren.

## 8. Online veröffentlichen (GitHub Pages)

`index.html` lädt reveal.js aus `node_modules`, das nicht Teil des Repositorys ist. Für die Online-Version lädst du reveal.js daher aus dem Internet (CDN): Ersetze in `index.html` jedes Vorkommen von

```text
node_modules/reveal.js/
```

durch

```text
https://cdn.jsdelivr.net/npm/reveal.js@6.0.2/
```

Veröffentliche anschließend `index.html`, `slides.md` und den Ordner `images`. Wähle im Repository unter *Settings → Pages* den Branch aus.

## 9. Fehlerbehebung

| Problem | Ursache und Lösung |
| --- | --- |
| Die Seite bleibt weiß | Du hast `index.html` per Doppelklick geöffnet. Dann darf der Browser `slides.md` nicht laden. Öffne die Präsentation immer mit `npm start`. |
| Alles erscheint auf einer einzigen Folie | Die Leerzeile vor oder nach `---` fehlt. |
| Der Text `<!-- .element ... -->` wird angezeigt | Der Kommentar muss in derselben Zeile wie der Aufzählungspunkt stehen. |
| In den Spalten steht `![Image](...)` statt des Bildes | Im `<div>` fehlen die Leerzeilen vor oder nach dem Markdown-Inhalt. |
| Ein Bild fehlt | Die Datei liegt nicht in `images` oder der Name ist falsch geschrieben (online wird zwischen Groß- und Kleinschreibung unterschieden). |
| Text ragt unten aus der Folie heraus | Zu viel Inhalt: Teile ihn auf zwei Folien auf oder füge eine vertikale Folie hinzu. |
| Port 8000 ist belegt | Gib in `package.json` nach `--port` eine andere Nummer ein. |

## 10. Weiterlesen

- Markdown in reveal.js: https://revealjs.com/markdown/
- Alles über reveal.js (Übergänge, Animationen, Layouts): https://revealjs.com/
- Markdown-Grundlagen: https://www.markdownguide.org/basic-syntax/

## 11. Tipp: Quarto

Wenn du mehr möchtest, sieh dir [Quarto](https://quarto.org/docs/presentations/revealjs/) an. Auch dort schreibst du Markdown, aus dem Quarto eine reveal.js-Präsentation erstellt. Spalten, Hinweisboxen, Diagramme und Code aus echten Dateien sind ohne HTML bereits integriert. Dafür musst du Quarto installieren und einen zusätzlichen Build-Schritt ausführen. Bei Bedarf kannst du aus derselben Datei auch PowerPoint- oder PDF-Folien erstellen.
