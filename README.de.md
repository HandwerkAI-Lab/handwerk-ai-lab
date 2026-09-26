# Handwerk AI Lab

[English](README.md) · **Deutsch**

**Ein Handwerksbetrieb, der mit lokaler KI arbeitet – offen dokumentiert.**
Zwei NVIDIA DGX Sparks, ein Mac Studio, echte Kunden. Alles nachvollziehbar, jede Quelle genannt.

Ich bin Julian und führe einen Solar-Installationsbetrieb in Freiburg (8 Leute, über 1.000 Anlagen).
Ich bin kein KI-Entwickler, sondern Handwerker. Dieses Repo ist mein Laborbuch: was ich einsetze, was ich messe, was schiefgeht.

→ Mitlesen auf X: [@HandwerkAiLab](https://x.com/HandwerkAiLab)

## Warum lokale KI für deutsche Betriebe?

- **Kundendaten bleiben im Haus.** Anfragen, Angebote und Adressen gehen nicht an einen Cloud-Anbieter. Das vereinfacht die DSGVO-Frage erheblich.
- **Keine laufenden Kosten pro Anfrage.** Die Hardware kostet einmal, danach laufen die Modelle ohne Token-Rechnung.
- **Unabhängig.** Kein Anbieter kann Preise, Modelle oder Nutzungsbedingungen über Nacht ändern.

Der ehrliche Gegenpunkt: Die Einrichtung kostet Zeit und Lehrgeld. Genau das dokumentiere ich hier, damit andere Betriebe es leichter haben.

## Was bringt das im Handwerksalltag?

Aktuell teste ich lokale KI für:

| Aufgabe | Was die KI macht | Wer entscheidet |
| --- | --- | --- |
| Kundenanfragen | Antwortentwürfe auf Anfragen zu Balkonkraftwerken | Ein Mensch prüft und sendet |
| Angebote | Entwürfe für Angebotstexte | Ein Mensch prüft und sendet |
| Onlineshop | Produkt- und Kategorietexte | Ein Mensch prüft und veröffentlicht |

**Grundregel:** Die KI schreibt Entwürfe, ein Mensch schickt ab. Nichts wird automatisch versendet oder veröffentlicht.
Wie viel Zeit das wirklich spart, messe ich und trage es im [Laborbuch](logs/) ein, sobald belastbare Zahlen da sind.

## Einstieg für Nicht-Programmierer

Du musst nicht programmieren können, um hier etwas mitzunehmen:

1. **Lies das [Laborbuch](logs/).** Jeder Eintrag hat am Ende eine kurze deutsche Zusammenfassung.
2. **Schau in [`SOURCES.md`](SOURCES.md).** Dort stehen alle Projekte, auf denen ich aufbaue, mit Link und Lizenz.
3. **Frag auf X.** Ich antworte auf Deutsch und auf Englisch.

Die technische Doku (Hardware, Rezepte, Benchmarks) ist auf Englisch, weil die internationale Local-AI-Szene so arbeitet und Befehle, Configs und Messwerte ohnehin sprachunabhängig sind.

## Hardware

| Komponente | Aufgabe |
| --- | --- |
| 2× NVIDIA DGX Spark (je 128 GB Unified Memory) | Inference-Server (vLLM / SGLang) |
| Mac Studio | Lokaler Agent (HERMES), MLX-Experimente |
| Mellanox ConnectX-5 Ex im OWC Mercury Helios 5S (Thunderbolt) | RDMA-Verbindung Mac ↔ Spark über [MCDMA](https://github.com/ashhart/MCDMA) |
| QSFP28-DAC-Kabel | Physische Verbindung Mac ↔ Spark |

Details: [`hardware/`](hardware/) (englisch)

## Grundsätze

1. **Lokal zuerst.** Kundendaten verlassen das Haus nicht.
2. **Mensch entscheidet.** Die KI entwirft, ein Mensch sendet.
3. **Gemessen statt geschätzt.** Jede Zahl nennt Hardware, Modell, Quantisierung und Datum.
4. **Quellen nennen.** Wer mir mit seiner Arbeit geholfen hat, steht in [`SOURCES.md`](SOURCES.md) und wird im Post markiert.

## Wie ich Quellen nenne

Die vollständigen Regeln stehen auf Englisch in [`SOURCES.md`](SOURCES.md). Kurz gesagt:

- Jede genutzte Arbeit mit **Link, Autor und Lizenz**.
- Übernommener Code **behält seine Lizenz** und den ursprünglichen Copyright-Hinweis.
- **AGPL-Code** wird nicht in dieses Repo kopiert, sondern nur verlinkt.
- **„Inspiriert von“ ist nicht „kopiert aus“.** Ich schreibe dazu, was davon zutrifft.
- Zahlen von anderen werden mit Link zitiert, eigene Messungen als solche gekennzeichnet.

## Lizenz

Skripte: MIT · Texte: CC BY 4.0 · Fremde Rezepte behalten ihre ursprüngliche Lizenz.
