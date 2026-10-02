# Projekt: Pricing einer Motorfahrzeug-Haftpflichtversicherung

## Ausgangslage

Ihr arbeitet im Pricing-Team eines Versicherers. Ihr erhaltet zwei Rohdatensätze aus einem französischen Motorfahrzeug-Haftpflichtportfolio.

Euer Ziel ist ein nachvollziehbares Tarifmodell. Es soll für eine beliebige Kundengruppe sagen, wie teuer die Versicherung sein sollte, und *warum*.

## Daten und Arbeitsumgebung

Ihr arbeitet in einer Kopie dieses Repos mit Claude Code (Web) oder Codex (Web). Die Rohdaten liegen bereits unter `data/raw/`. Ihr müsst nichts herunterladen.

| Datei | Inhalt | Erwarteter Umfang |
|---|---|---|
| `freMTPL2freq.csv` | Policen: Merkmale zu Fahrer, Fahrzeug und Wohnort, Versicherungsdauer (`Exposure`, in Jahren), Anzahl gemeldeter Schäden (`ClaimNb`) | 677'991 Zeilen × 12 Spalten |
| `freMTPL2sev.csv` | Einzelne Schadenbeträge (`ClaimAmount`), verknüpfbar über `IDpol` | 26'444 Zeilen × 2 Spalten |

```python
import pandas as pd
freq = pd.read_csv("data/raw/freMTPL2freq.csv")
sev  = pd.read_csv("data/raw/freMTPL2sev.csv")
```

Die Dateien in `data/raw/` bleiben unverändert. Alles, was ihr mit den Daten macht, passiert nachvollziehbar im Code. Aufbereitete Daten und Zwischenergebnisse speichert ihr in `data/processed/`.

## Spielregeln

- Alles wird von Grund auf in Python mit den Rohdaten erarbeitet: Datenaufbereitung, Analyse, Modellwahl, Parameterschätzung.
- Es ist **nicht** erlaubt, Modellspezifikationen, Parameter, Feature-Transformationen oder Ergebnisse aus bestehenden Studien, Papers, Tutorials, Kaggle-Notebooks oder Repositories zu diesem Datensatz zu übernehmen.
- Allgemeine Dokumentation von Bibliotheken (pandas, scikit-learn, statsmodels usw.) dürft ihr selbstverständlich nutzen.
- Jede wesentliche Entscheidung muss aus euren eigenen Analysen der Daten heraus begründet werden.

### Arbeiten mit dem Coding-Agenten

Ihr dürft den Agenten für Code nutzen. Die fachlichen Entscheidungen trefft und begründet aber ihr, zum Beispiel:

- Welche Daten verwendet ihr in welcher Form?
- Welche Variablen werden wie gruppiert oder transformiert?
- Welches Modell, welche Verlustfunktion, welche Metrik?

Haltet jede dieser Entscheidungen in `ENTSCHEIDUNGEN.md` fest. Zu jedem Eintrag gehören:

- was ihr in den Daten gesehen habt (mit Verweis auf Plot oder Tabelle),
- welche Optionen ihr erwogen habt,
- wofür ihr euch entschieden habt und warum.

Ihr müsst jede Entscheidung in der Präsentation ohne Agent erklären können.

## Teil 1 – Die Daten verstehen und nutzbar machen

**Ziel:** Ihr kennt eure Daten so gut, dass ihr ihnen für die Preisberechnung vertrauen könnt, und ihr könnt belegen, warum.

- Analysiert beide Datensätze und macht sie bereit, um damit zu arbeiten.
- Am Ende steht ein Datenstand, auf dem Teil 2 bis 4 aufbauen. Er entsteht reproduzierbar per Code aus den Rohdaten.
- Jeder Schritt von den Rohdaten zu diesem Datenstand ist in `ENTSCHEIDUNGEN.md` begründet, einschliesslich seiner Auswirkung auf die Daten.

## Teil 2 – Schadenfrequenz und Schadenhöhe sichtbar machen

**Ziel:** Jemand ohne Statistikkenntnisse versteht anhand eurer Grafiken, wie häufig und wie teuer Schäden sind und welche Merkmale damit zusammenhängen.

- Legt fest, was ihr unter „Schadenfrequenz“ versteht, und begründet die Definition.
- Zeigt die Frequenz insgesamt und in Abhängigkeit jedes Merkmals sowie die Verteilung der Schadenhöhen.
- Aus euren Grafiken geht auch hervor, wie belastbar die einzelnen Aussagen sind.

## Teil 3 – Welche Merkmale beeinflussen den Preis?

**Ziel:** Ihr könnt quantitativ belegen, welche Merkmale den erwarteten Schadenaufwand treiben, wie stark und in welche Richtung, und wie verlässlich eure Prognosen für neue Kundinnen und Kunden sind.

- Ihr modelliert die erwartete Schadenhäufigkeit und die erwartete Schadenhöhe getrennt oder den erwarteten Schadenaufwand direkt. Die Wahl begründet ihr.
- Modelltyp, Verteilungsannahmen und Verlustfunktion leitet ihr aus der Art des Problems und aus euren Daten ab.
- Ihr vergleicht mindestens zwei Ansätze unterschiedlicher Komplexität, z. B. ein gut interpretierbares und ein flexibleres Modell.
- Ihr belegt die Prognosegüte auf Daten, die nicht für das Training verwendet wurden, mit einer begründet gewählten Metrik.
- Euer Ergebnis hält einer kritischen Prüfung stand. Beantwortet dazu mindestens diese Fragen:
  - Sind Merkmale stark miteinander korreliert, und was bedeutet das für eure Aussagen?
  - Gibt es Merkmale, die selbst schon das Ergebnis eines früheren Tarifs sind?
  - Gibt es Merkmale, die ihr aus ethischen oder regulatorischen Gründen nicht verwenden würdet?

## Teil 4 – Tarif-Dashboard

**Ziel:** Jemand aus dem Vertrieb sieht für ein beliebiges Kundenprofil den Preis und versteht, wie er zustande kommt.

Erstellt keine App, sondern ein Dashboard, das über GitHub Pages veröffentlicht wird. GitHub Pages liefert nur statische Dateien aus (HTML, CSS, JavaScript), es läuft dort kein Python. Eure Pipeline erzeugt das Dashboard deshalb aus den Modellergebnissen, z. B. mit Plotly, Altair oder Quarto, und legt es im Repo ab (z. B. im Ordner `docs/`).

Im Dashboard wählt man ein Kundenprofil oder eine Kundengruppe aus, und es zeigt:

- die erwartete Schadenhäufigkeit pro Jahr,
- die erwartete Schadenhöhe,
- die daraus resultierende Nettorisikoprämie,
- eine daraus abgeleitete Bruttoprämie mit selbst gewählten und dokumentierten Zuschlägen,
- eine **Begründung des Preises**: Wie trägt jedes Merkmal des Profils zum Preis bei, im Vergleich zu einem Referenzkunden oder zum Portfoliodurchschnitt?

## Abgabe

Alles liegt im Git-Repo. Stichtag ist der letzte Commit auf `main`.

- Reproduzierbare Pipeline: Nach `pip install -r requirements.txt` erzeugt ein dokumentierter Befehl alle Ergebnisse aus den Rohdaten.
- Das Dashboard wird von der Pipeline erzeugt und liegt im Repo. Es ist über GitHub Pages öffentlich erreichbar. Der Link steht im README.
- `ENTSCHEIDUNGEN.md` ist vollständig ausgefüllt.
- Ein Bericht von max. [X] Seiten mit euren Entscheidungen, Ergebnissen und Limitationen.
- Eine Präsentation von [X] Minuten inkl. Live-Demo des Dashboards.

## Bewertung

Bewertet werden vor allem:

- die Nachvollziehbarkeit und Begründung eurer Entscheidungen,
- die fachliche Korrektheit,
- die Erklärbarkeit des Preises im Dashboard.

Die reine Modellgüte zählt weniger.
