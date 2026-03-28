# ₿ BTC Analyzer

> Outil d'analyse technique Bitcoin **standalone** (un seul fichier HTML).  
> Données live via **Binance API** + **Alternative.me** · Analyse IA via **Claude API** (Anthropic).

![License](https://img.shields.io/badge/license-MIT-blue.svg)
![HTML](https://img.shields.io/badge/HTML-standalone-orange.svg)
![Binance](https://img.shields.io/badge/data-Binance%20API-yellow.svg)
![Claude](https://img.shields.io/badge/AI-Claude%20API-purple.svg)

---

## Aperçu

BTC Analyzer est un dashboard d'analyse technique Bitcoin 100% local, sans serveur, sans dépendances npm. Un double-clic suffit pour lancer l'outil.

**Ce que l'outil fait :**
- Prix BTC en **temps réel** via WebSocket Binance
- Calcul de **RSI 14**, **EMA 50**, **EMA 200** depuis les klines Binance
- **Fear & Greed Index** via Alternative.me
- **Pivot Points** (S1/S2/S3, R1/R2/R3) calculés automatiquement
- **Grille de confluence** (6 signaux) pour scorer les opportunités d'entrée
- **Switcher 4H / Daily** pour l'analyse intraday ou swing
- **3 analyses Claude AI** générées en un seul clic ↻ Refresh

---

## Démarrage rapide

```bash
# 1. Cloner le repo
git clone https://github.com/TON_USERNAME/btc-analyzer.git

# 2. Ouvrir le fichier dans le navigateur
open btc-analyzer.html   # macOS
# ou double-cliquer sur le fichier sous Windows/Linux
```

> Aucune installation, aucun `npm install`, aucun serveur requis.

### Clé API Claude (optionnel)

Les données de marché fonctionnent sans clé. La clé API Claude est nécessaire uniquement pour les analyses IA.

```
1. Créer une clé sur https://console.anthropic.com
2. Coller la clé dans le bandeau en haut de la page
3. Cliquer "Sauvegarder" → la clé est stockée dans le localStorage du navigateur
```

> ⚠️ La clé n'est **jamais** stockée dans le fichier HTML — safe à partager et committer.

---

## Fonctionnalités

### Données live (gratuites, sans clé)

| Source | Données | Fréquence |
|---|---|---|
| Binance WebSocket | Prix temps réel | ~1-2 secondes |
| Binance REST | Klines 4H + Daily (500/220 bougies) | Toutes les 60s |
| Alternative.me | Fear & Greed Index | Toutes les 60s |

### Indicateurs calculés

| Indicateur | Méthode | Bougies utilisées |
|---|---|---|
| RSI 14 | Wilder Smoothed | 500 (4H) / 220 (Daily) |
| EMA 50 | Exponentielle | 500 (4H) / 220 (Daily) |
| EMA 200 | Exponentielle | 220 (Daily) |
| Pivot Points | Classiques (H+L+C)/3 | Dernière bougie complète |
| Volume ratio | vs Moyenne 20 périodes | TF actif |

### Switcher 4H / Daily

| | **4H** | **Daily** |
|---|---|---|
| Horizon | Quelques heures à 2-3 jours | 1 à 4 semaines |
| Usage | Timer l'entrée précise | Identifier la zone d'achat |
| Bougies | 500 | 220 |

### Grille de confluence (6 signaux)

Le score d'entrée est calculé en temps réel :

```
RSI < 30 — Zone survente         ●●● (obligatoire)
Divergence RSI haussière         ●●
Volume spike ≥ 1.5× moyenne      ●●● (obligatoire)
Fear & Greed < 20                ●●
BTC sous EMA 200 (contexte)      ●
Support pivot proche (S1/S2)     ●●

→ Minimum pour entrer : 3/6 dont RSI < 30 ET Volume ≥ 1.5×
```

### Analyses Claude AI

Le bouton **↻ Refresh** génère les 3 analyses simultanément (~$0.03) :

| Onglet | Contenu |
|---|---|
| 📊 Points d'entrée | Niveaux précis, RSI, conditions exactes, TP/SL |
| 🛡 Gestion du risque | Taille position, R:R, niveaux stop |
| 🎯 Scénarios | Haussier / Neutre / Baissier avec probabilités |

Les onglets permettent de **naviguer entre les analyses sans coût supplémentaire** ($0.00 par clic).

### Alertes automatiques

La page surveille 4 zones toutes les 60s. Si une zone change, les onglets Claude s'illuminent avec un badge **NEW** :

- Zone RSI (6 paliers : <25 / 25-30 / 30-40 / 40-60 / 60-70 / >70)
- Zone Fear & Greed (5 catégories)
- Zone Volume (normal / elevated ×1.5 / spike ×2)
- EMA cross (BTC passe au-dessus/en-dessous de l'EMA 50 ou 200)

---

## Architecture

```
btc-analyzer.html          ← fichier unique, tout inclus
├── HTML                   Layout de la page
├── CSS                    Thème dark, variables, animations
└── JavaScript
    ├── Binance WebSocket  Prix temps réel
    ├── Binance REST       Klines 4H + Daily
    ├── Alternative.me     Fear & Greed
    ├── Calculs            RSI, EMA, Pivots, Volume
    ├── Render             KPIs, Sparkline (Canvas), Niveaux, Signaux
    ├── Signal Detection   Surveillance des 4 zones, alertes
    └── Claude API         callClaude(), switchTab(), refreshAll()
```

**Aucune dépendance externe** — tout le code est inline dans le fichier HTML.

---

## Stratégie — Rappel

L'outil est conçu autour de la stratégie **RSI + EMA 200 + Volume** :

```
Filtre 1 — EMA 200 (tendance macro)
  BTC sous EMA 200 → longs courts, cibles réduites
  BTC au-dessus    → longs normaux, cibles étendues

Filtre 2 — RSI (timing)
  RSI < 30 sur le TF actif → zone d'entrée

Filtre 3 — Volume (confirmation)
  Spike ≥ 1.5× la moyenne → signal valide
```

**Hausse moyenne après spike ×1.5 + RSI < 30 + support en bear market :**
- 4H : +4% à +8% sur les 3-5 bougies suivantes
- Daily : +6% à +12% si support solide

> Pas un conseil financier — DYOR.

---

## Coût API Claude

| Action | Coût |
|---|---|
| Données Binance + Alternative.me | $0.00 (APIs publiques) |
| ↻ Refresh (3 analyses) | ~$0.03 |
| Clic sur un onglet 📊 / 🛡 / 🎯 | $0.00 |
| **Avec $5 de crédit** | **~150 Refresh complets** |

---

## Compatibilité

| Navigateur | Support |
|---|---|
| Chrome 90+ | ✅ Recommandé |
| Firefox 88+ | ✅ |
| Edge 90+ | ✅ |
| Safari 14+ | ✅ |

**Protocoles testés :** `file://` (local) · `http://` · `https://`

---

## Fichiers

| Fichier | Description |
|---|---|
| `btc-analyzer.html` | L'outil complet (standalone) |
| `DOCUMENTATION.md` | Guide complet d'utilisation avec exemples |
| `CHANGELOG.md` | Historique des versions |

---

## Licence

MIT — libre d'utilisation, modification et redistribution.

---

*Données : Binance Public API · Alternative.me · Claude API (Anthropic)*
