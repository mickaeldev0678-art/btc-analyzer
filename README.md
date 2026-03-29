# ₿ BTC Analyzer

> Outil d'analyse technique Bitcoin **standalone** (un seul fichier HTML).  
> Binance API · Alternative.me · Twelve Data · Claude API (Anthropic).

![License](https://img.shields.io/badge/license-MIT-blue.svg)
![HTML](https://img.shields.io/badge/HTML-standalone-orange.svg)
![Version](https://img.shields.io/badge/version-1.3.0-green.svg)

---

## Démarrage rapide

```bash
git clone https://github.com/TON_USERNAME/btc-analyzer.git
# Puis double-cliquer sur btc-analyzer.html
```

Aucune installation, aucun serveur, aucun npm.

---

## Fonctionnalités

- Prix BTC temps réel via WebSocket Binance
- RSI 14, EMA 50, EMA 200 (4H et Daily)
- Graphique interactif pleine largeur — tooltip, crosshair, histogramme volume, badges AI
- Fear & Greed Index (Alternative.me)
- Pivot Points S1/S2/S3, R1/R2/R3
- Calendrier macro FOMC/CPI/NFP/PCE avec compte à rebours live
- Corrélations DXY · SP500 · Nasdaq 100 · Or (Twelve Data)
- Grille de confluence 6 signaux
- Switcher 4H / Daily avec cache et historique séparés
- 3 analyses Claude AI : Points d'entrée · Gestion du risque · Scénarios
- Historique 5 analyses par TF avec badges cliquables sur le graphique

---

## Configuration

Le bloc de configuration est en **bas de la page**.

| Clé | Requis pour | Gratuit |
|---|---|---|
| Claude API | Analyses IA | Oui (crédit offert) |
| Twelve Data | DXY · SP500 · Or | Oui (800 req/jour) |

Les clés sont stockées dans le `localStorage` — jamais dans le fichier HTML. Safe à committer.

---

## Deux boutons distincts

| Bouton | Action | Coût |
|---|---|---|
| ↻ Données | Binance + F&G + DXY/SP500/Or | $0.00 |
| ✦ Analyser | 3 analyses Claude | ~$0.03 |
| Onglets 📊/🛡/🎯 | Navigation cache | $0.00 |

---

## Fichiers

| Fichier | Description |
|---|---|
| `btc-analyzer.html` | L'outil complet |
| `DOCUMENTATION.md` | Guide complet (15 sections) |
| `CHANGELOG.md` | Historique des versions |

---

## Licence

MIT

---

*Pas un conseil financier — DYOR*
