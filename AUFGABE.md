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

Die Dateien in `data/raw/` werden nie verändert. Jede Bereinigung passiert im Code. Zwischenergebnisse speichert ihr in `data/processed/`.

## Spielregeln

- Alles wird von Grund auf in Python mit den Rohdaten erarbeitet: Bereinigung, Analyse, Modellwahl, Parameterschätzung.
- Es ist **nicht** erlaubt, Modellspezifikationen, Parameter, Feature-Transformationen oder Ergebnisse aus bestehenden Studien, Papers, Tutorials, Kaggle-Notebooks oder Repositories zu diesem Datensatz zu übernehmen.
- Allgemeine Dokumentation von Bibliotheken (pandas, scikit-learn, statsmodels usw.) dürft ihr selbstverständlich nutzen.
- Jede wesentliche Entscheidung muss aus euren eigenen Analysen der Daten heraus begründet werden.

### Arbeiten mit dem Coding-Agenten

Ihr dürft den Agenten für Code nutzen. Die fachlichen Entscheidungen trefft und begründet aber ihr:

- Wie wird bereinigt?
- Welche Variablen werden wie gruppiert oder transformiert?
- Welches Modell, welche Verlustfunktion, welche Metrik?

Haltet jede dieser Entscheidungen in `ENTSCHEIDUNGEN.md` fest. Zu jedem Eintrag gehören:

- was ihr in den Daten gesehen habt (mit Verweis auf Plot oder Tabelle),
- welche Optionen ihr erwogen habt,
- wofür ihr euch entschieden habt und warum.

Ihr müsst jede Entscheidung in der Präsentation ohne Agent erklären können.

## Teil 1 – Datenverständnis und Bereinigung

- Verschafft euch einen Überblick über alle Variablen: Verteilungen, fehlende oder unplausible Werte, Ausreisser.
- Prüft, ob die beiden Datensätze konsistent zueinander sind.
- Dokumentiert jede Bereinigung mit Begründung und Anzahl betroffener Zeilen.

## Teil 2 – Visualisierung der Schadenfrequenz

- Definiert sauber, was „Schadenfrequenz“ ist. Berücksichtigt, dass Policen unterschiedlich lange versichert waren.
- Visualisiert die Frequenz insgesamt und in Abhängigkeit jedes Merkmals.
- Achtet darauf, wie viel Exposure hinter jedem Datenpunkt steckt. Eine hohe Frequenz in einer winzigen Gruppe ist wenig aussagekräftig.
- Visualisiert auch die Verteilung der Schadenhöhen.

## Teil 3 – Welche Merkmale beeinflussen den Preis?

- Erstellt ein Modell für die erwartete Schadenhäufigkeit und eines für die erwartete Schadenhöhe. Alternativ ein Modell für den erwarteten Schadenaufwand direkt, wenn ihr das gut begründen könnt.
- Überlegt euch, welche Art von Problem das ist und welche Verteilungsannahmen und Verlustfunktionen dazu passen. Ist es Klassifikation, Regression, etwas anderes?
- Vergleicht mindestens zwei Modellansätze unterschiedlicher Komplexität, z. B. ein gut interpretierbares und ein flexibleres Modell.
- Evaluiert die Modelle auf Daten, die für das Training nicht verwendet wurden, mit einer begründet gewählten Metrik.
- Quantifiziert, welche Merkmale wie stark wirken und in welche Richtung.
- Diskutiert kritisch:
  - Sind Merkmale stark miteinander korreliert?
  - Gibt es Merkmale, die selbst schon das Ergebnis eines früheren Tarifs sind?
  - Gibt es Merkmale, die ihr aus ethischen oder regulatorischen Gründen nicht verwenden würdet?

## Teil 4 – Tarif-App

Entwickelt eine einfache App, z. B. mit Streamlit, Dash oder Gradio. Darin wählt man ein Kundenprofil oder eine Kundengruppe aus, und die App zeigt:

- die erwartete Schadenhäufigkeit pro Jahr,
- die erwartete Schadenhöhe,
- die daraus resultierende Nettorisikoprämie,
- eine daraus abgeleitete Bruttoprämie mit selbst gewählten und dokumentierten Zuschlägen,
- eine **Begründung des Preises**: Wie trägt jedes Merkmal des Profils zum Preis bei, im Vergleich zu einem Referenzkunden oder zum Portfoliodurchschnitt?

## Abgabe

Alles liegt im Git-Repo. Stichtag ist der letzte Commit auf `main`.

- Reproduzierbare Pipeline: Nach `pip install -r requirements.txt` erzeugt ein dokumentierter Befehl alle Ergebnisse aus den Rohdaten.
- Die App startet nach `git clone` mit einem Befehl, z. B. `streamlit run app/app.py`. Zusätzlich ist sie öffentlich erreichbar, z. B. über Streamlit Community Cloud. Der Link steht im README.
- `ENTSCHEIDUNGEN.md` ist vollständig ausgefüllt.
- Ein Bericht von max. [X] Seiten mit euren Entscheidungen, Ergebnissen und Limitationen.
- Eine Präsentation von [X] Minuten inkl. Live-Demo der App.

## Bewertung

Bewertet werden vor allem:

- die Nachvollziehbarkeit und Begründung eurer Entscheidungen,
- die fachliche Korrektheit,
- die Erklärbarkeit des Preises in der App.

Die reine Modellgüte zählt weniger.
