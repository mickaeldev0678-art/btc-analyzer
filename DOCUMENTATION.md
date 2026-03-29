# BTC Analyzer — Documentation

> Outil d'analyse technique Bitcoin **standalone** (un seul fichier HTML local).  
> Données live : Binance · Alternative.me · Twelve Data · Claude API (Anthropic).

---

## Table des matières

1. [Installation & démarrage](#1-installation--démarrage)
2. [Configuration des clés API](#2-configuration-des-clés-api)
3. [Layout & navigation](#3-layout--navigation)
4. [Switcher 4H / Daily](#4-switcher-4h--daily)
5. [Indicateurs en temps réel](#5-indicateurs-en-temps-réel)
6. [Graphique interactif](#6-graphique-interactif)
7. [Supports & résistances](#7-supports--résistances)
8. [Calendrier macro & corrélations](#8-calendrier-macro--corrélations)
9. [Grille de confluence — Score d'entrée](#9-grille-de-confluence--score-dentrée)
10. [Analyses Claude AI](#10-analyses-claude-ai)
11. [Historique des analyses](#11-historique-des-analyses)
12. [Alertes automatiques](#12-alertes-automatiques)
13. [Actualisation des données](#13-actualisation-des-données)
14. [Stratégie RSI + Volume + EMA 200 — rappel](#14-stratégie-rsi--volume--ema-200--rappel)
15. [FAQ & cas pratiques](#15-faq--cas-pratiques)

---

## 1. Installation & démarrage

Aucune installation requise. Le fichier `btc-analyzer.html` est **100% standalone**.

```
1. Télécharger btc-analyzer.html
2. Double-cliquer pour l'ouvrir dans le navigateur
3. Les données Binance se chargent automatiquement
```

> **Navigateurs compatibles** : Chrome, Firefox, Edge, Safari (version récente).  
> **Connexion internet requise** pour les données live.

Les APIs Binance et Alternative.me sont **gratuites et sans clé**. Seules les analyses Claude AI et les données macro (DXY/SP500/Or) nécessitent des clés configurées en bas de page.

---

## 2. Configuration des clés API

Le bloc de configuration se trouve **en bas de la page**, juste avant le footer. Les clés sont stockées dans le `localStorage` du navigateur — elles **ne sont jamais écrites dans le fichier HTML**.

> ⚠️ Ne partagez jamais le fichier si vos clés sont sauvegardées dans votre navigateur local.

### Clé Claude (analyses IA)

```
1. Aller sur https://console.anthropic.com
2. Section "API Keys" → créer une clé (format : sk-ant-api03-...)
3. Coller dans le champ 🔑 Claude → Sauvegarder
```

**Coût** : ~$0.03 par clic ✦ Analyser (3 analyses × ~$0.01).  
Avec $5 de crédit → ~150 sessions d'analyse complètes.

### Clé Twelve Data (données macro)

Nécessaire pour afficher DXY (UUP), SP500 (SPY), Nasdaq 100 (QQQ) et Or (XAU/USD).

```
1. Aller sur https://twelvedata.com → Sign Up (gratuit)
2. Dashboard → API Keys → copier la clé
3. Coller dans le champ 📈 Twelve Data → Sauvegarder
4. Les données macro se chargent immédiatement
```

**Limite gratuite** : 800 requêtes/jour — 4 symboles par clic ↻ Données = 200 clics/jour.

---

## 3. Layout & navigation

La page est organisée en blocs verticaux :

```
┌─────────────────────────────────────────────────┐
│  Header : prix live · ↻ Données · badge LIVE    │
├─────────────────────────────────────────────────┤
│  KPIs : RSI · EMA 200 · EMA 50 · Fear & Greed   │
├────────────────────┬────────────────────────────┤
│  Calendrier macro  │  Corrélations & DXY         │
├────────────────────┴────────────────────────────┤
│  Graphique prix pleine largeur                   │
│  (ligne + area + volume + tooltip + EMA)         │
├────────────────────┬────────────────────────────┤
│  Supports &        │  Grille de confluence       │
│  Résistances       │  + Score + Plan d'action    │
├────────────────────┴────────────────────────────┤
│  Claude AI (onglets + texte) │  Historique       │
├─────────────────────────────────────────────────┤
│  Configuration API Keys · Footer                 │
└─────────────────────────────────────────────────┘
```

---

## 4. Switcher 4H / Daily

Deux boutons en haut à droite des indicateurs permettent de basculer entre les deux timeframes.

| | **4H** | **Daily** |
|---|---|---|
| Horizon | Quelques heures à 2-3 jours | 1 à 4 semaines |
| Type | Intraday / Day trading | Swing trading |
| RSI réagit en | Quelques heures | 2-3 jours |
| Bougies chargées | 500 | 220 |
| Usage | Timer l'entrée précise | Identifier la zone d'achat |

### Comportement au switch

- Labels des cartes RSI / EMA mis à jour
- Graphique : 60 bougies 4H ou 30 bougies Daily
- Volume recalculé sur les 20 dernières bougies du TF actif
- **Cache Claude vidé** — les analyses 4H ne sont pas valides en Daily et vice versa
- **Historique Claude rechargé** — chaque TF a son propre historique de 5 entrées
- Grille de scoring recalculée
- Détection de signal réinitialisée

### Workflow optimal

```
1. Daily → identifier la zone (RSI ~35, support proche)
2. 4H   → timer l'entrée (RSI < 30 + volume spike ≥ 1.5×)
3. ✦ Analyser en 4H → obtenir niveaux précis d'entrée/TP/SL
```

---

## 5. Indicateurs en temps réel

### RSI 14

| Valeur | Zone | Signal |
|---|---|---|
| < 25 | Survente extrême | 🟢 Setup prioritaire |
| 25–30 | Survente | 🟢 Zone d'entrée RSI |
| 30–40 | Bas-neutre | 🟡 Surveiller |
| 40–60 | Neutre | Attendre |
| 60–70 | Haut-neutre | Prudence |
| > 70 | Surachat | 🔴 Éviter les longs |

### EMA 200

Filtre de tendance macro :

- **BTC au-dessus** → biais haussier → longs normaux, cibles étendues
- **BTC en-dessous** → biais baissier → longs réduits, cibles courtes uniquement

Sur le graphique, l'EMA 200 est toujours visible même hors plage — elle s'affiche en bord du graphique avec son prix et la distance en % depuis le prix actuel.

### EMA 50

En bear market (BTC sous EMA 200), l'EMA 50 plafonne souvent les rebonds. Un retour au-dessus = premier signal d'amélioration structurelle.

### Fear & Greed Index

Source : Alternative.me. Mise à jour automatique toutes les 60s.

| Valeur | Zone | Lecture |
|---|---|---|
| 0–15 | Extrême Fear | Potentiel bottom — surveiller |
| 15–30 | Fear | Accumulation possible |
| 45–55 | Neutre | Pas de signal |
| 75–100 | Extrême Greed | Éviter les longs |

> F&G < 15 pendant 3+ jours + RSI < 30 = signal de confluence rare et puissant.

---

## 6. Graphique interactif

Le graphique occupe **toute la largeur** de la page.

### Éléments affichés

- Ligne de prix colorée (vert = hausse sur la période, rouge = baisse)
- Zone de remplissage sous la ligne
- EMA 200 — ligne bleue pointillée (toujours visible, même hors plage de prix)
- EMA 50 — ligne violette pointillée (toujours visible)
- Histogramme de volume en bas du graphique
- Point orange = prix actuel
- Badge prix live ancré à droite

### Tooltip au survol

Survoler le graphique affiche pour chaque bougie :

```
Date / heure
PRIX      VARIATION    VOLUME      H / L
$66 834   +1.21%       $2.4B       $67 163 / $65 548
```

Un crosshair (ligne verticale + horizontale) suit la souris. Le curseur passe en pointeur au survol des badges.

### Histogramme de volume

| Couleur | Signification |
|---|---|
| Vert vif | Spike ≥ 1.5× la moyenne |
| Vert transparent | Bougie haussière normale |
| Rouge transparent | Bougie baissière |

### EMA hors plage visible

Quand l'EMA 200 est loin du prix (ex: EMA 200 Daily à $88K, BTC à $66K), elle s'affiche en bord supérieur du graphique avec :

```
EMA200 $88K (+32.5%)
```

### Badges historique sur le graphique

Après chaque génération Claude, un badge **AI** est placé sur le graphique à la date et au prix de la génération :

| Couleur | RSI au moment de l'analyse |
|---|---|
| 🟢 Vert | RSI < 30 |
| 🟡 Orange | RSI 30–40 |
| ⚫ Gris | RSI ≥ 40 |

**Survol** → tooltip : date, prix, RSI, F&G au moment de l'analyse + invitation à cliquer.  
**Clic** → charge les 3 analyses de ce moment dans les onglets + scroll vers le panneau Claude.

Les badges n'apparaissent que si l'analyse est dans la plage de dates visible du graphique.

---

## 7. Supports & résistances

Tableau trié du plus haut au plus bas avec la distance en % depuis le prix actuel.

| Badge | Type | Description |
|---|---|---|
| `EMA` | EMA 50 / EMA 200 | Moyennes mobiles dynamiques |
| `R` | R1, R2, R3 | Résistances pivot classiques |
| `▶` | PRIX NOW | Position actuelle |
| `S` | S1, S2, S3 | Supports pivot classiques |
| `P` | Pivot | Point pivot central |

### Calcul des pivots

Calculés depuis la dernière bougie complète :

```
P  = (H + L + C) / 3
R1 = 2×P − L       S1 = 2×P − H
R2 = P + (H − L)   S2 = P − (H − L)
R3 = H + 2×(P−L)   S3 = L − 2×(H−P)
```

---

## 8. Calendrier macro & corrélations

### Calendrier macro

Affiche les **5 prochains événements** macro triés par proximité, avec compte à rebours live :

| Couleur | Signification |
|---|---|
| Rouge + `Xh Xm` | Dans les 24h |
| Orange + `J−X` | Dans les 7 jours |
| Gris + `J−X` | Plus lointain |

Dots d'impact : ●●● = Fort (FOMC) · ●● = Modéré (CPI, NFP, PCE)

Le compte à rebours se met à jour **toutes les 60s automatiquement** (calcul JS pur, aucune API).

Événements couverts : FOMC (8×/an), CPI (mensuel), NFP (mensuel), PCE (mensuel), Minutes Fed.

### Corrélations & DXY

Mis à jour uniquement au clic **↻ Données** via Twelve Data.

| Indicateur | ETF utilisé | Corrélation BTC | Note |
|---|---|---|---|
| Dollar Index | UUP | −0.72 inverse | Rouge = dollar ↑ = pression BTC |
| S&P 500 | SPY | +0.55 modérée | Rouge = baisse = risk-off |
| Nasdaq 100 | QQQ | +0.61 modérée | Rouge = baisse |
| Or | XAU/USD | +0.18 faible | |

**Lecture couleurs DXY** : inversée par rapport aux autres. Dollar qui monte = rouge = mauvais pour BTC.

> Contexte risk-off typique : UUP rouge (↑) + SPY rouge (↓) + QQQ rouge (↓) = triple pression baissière sur BTC.

---

## 9. Grille de confluence — Score d'entrée

| Signal | Poids | Obligatoire |
|---|---|---|
| RSI < 30 — Zone survente | ●●● | **Oui** |
| Divergence RSI haussière possible | ●● | Non |
| Volume spike ≥ 1.5× moyenne | ●●● | **Oui** |
| Fear & Greed < 20 (capitulation) | ●● | Non |
| BTC sous EMA 200 (biais baissier) | ● | Contexte |
| Support pivot proche (S1/S2) | ●● | Non |

> **Minimum : 3 signaux actifs**, dont **RSI < 30 ET Volume ≥ 1.5×** obligatoirement.

### Règle fondamentale

```
RSI < 30 seul sans volume → Piège baissier → NE PAS ENTRER
Volume ×2 seul sans RSI   → Direction inconnue → NE PAS ENTRER
RSI < 30 + Volume ×1.5 + Support → Signal valide → ÉVALUER
```

### Exemples de scores

**2/6 — Trop tôt** : RSI = 33, Volume = ×0.13, F&G = 12 → attendre  
**4/6 — Favorable** : RSI = 27 ✅, Volume = ×1.8 ✅, F&G = 11 ✅, S1 proche ✅ → entrée envisageable  
**5/6 — Optimal** : RSI = 24 ✅, Volume = ×2.4 ✅, F&G = 8 ✅, S2 ✅, Divergence ✅ → meilleur R:R

---

## 10. Analyses Claude AI

### Deux boutons distincts

**↻ Données** (header) :
- Binance klines + ticker + Fear & Greed + DXY/SP500/QQQ/Or
- Coût : **$0.00**

**✦ Analyser** (panneau Claude) :
- 3 appels Claude sur les données déjà en mémoire (~15-30s)
- Coût : **~$0.03**

**Onglets 📊 / 🛡 / 🎯** :
- Navigation dans le cache — Coût : **$0.00**

### Contexte envoyé à Claude

```
Timeframe actif + horizon · Prix + variation 24h
RSI 14 (TF actif + autre TF) · EMA 50 et EMA 200 (deux TF)
Volume ratio · Fear & Greed · Niveaux pivot S1/S2/R1/R2
Contexte macro (géopolitique, corrélation SP500)
```

### Les 3 onglets

**📊 Points d'entrée** — niveaux précis, RSI/volume/prix conditions, TP et SL adaptés au TF.

**🛡 Gestion du risque** — taille de position, SL et TP avec prix exacts, ratio R:R, signaux d'invalidation.

**🎯 Scénarios** — haussier / neutre / baissier avec conditions déclenchantes, cibles et probabilités.

### Workflow type

```
1. Badge NEW sur les onglets → signal de changement détecté
2. ↻ Données → rafraîchir les données de marché
3. ✦ Analyser → "Appel 1/3… 2/3… 3/3…" (~15-30s)
4. Naviguer avec 📊 / 🛡 / 🎯 librement ($0.00)
5. Badge AI apparaît sur le graphique à la date/prix actuel
```

---

## 11. Historique des analyses

### Stockage séparé par TF

```
localStorage:
  btc_history_4h  → 5 analyses 4H max
  btc_history_1d  → 5 analyses Daily max
```

Au switch de TF, l'historique bascule vers celui du TF actif.

### Interface

Colonne à droite du texte Claude (en dessous sur mobile) :

```
Historique 4H (3/5)                     [Effacer]
● 28/03 17:42   $66 834   RSI 37.8   F&G 12  ← actif
○ 28/03 16:41   $67 200   RSI 39.1   F&G 12
○ 28/03 15:39   $68 100   RSI 41.2   F&G 13
```

**Cliquer une entrée** → charge les 3 analyses de ce moment. Le statut affiche :  
`Analyse du 28/03 17:42 · Prix $66 834 · RSI 37.8 · 4H`

**Cliquer un badge AI sur le graphique** → même effet.

**Effacer** → supprime uniquement l'historique du TF actif.

---

## 12. Alertes automatiques

La page surveille 4 zones toutes les 60s. Si une zone change, les onglets Claude s'illuminent (badge **NEW**).

| Zone | Changement détecté si… |
|---|---|
| **RSI** | Franchit un seuil parmi : `<25` / `25–30` / `30–40` / `40–60` / `60–70` / `>70` |
| **Fear & Greed** | Change de catégorie |
| **Volume** | Passe `normal` → `elevated` (×1.5) → `spike` (×2) ou inverse |
| **EMA cross** | BTC passe au-dessus/en-dessous de l'EMA 50 ou 200 |

```
Changement → badge NEW + message "⚡ Signal changé — RSI: neutral → low-neutral"
↻ Données + ✦ Analyser → surbrillance effacée
```

---

## 13. Actualisation des données

| Source | Fréquence | Coût |
|---|---|---|
| Binance WebSocket | ~1-2s auto | $0.00 |
| Binance REST + Fear & Greed | 60s auto | $0.00 |
| Calendrier macro (compte à rebours) | 60s auto (JS pur) | $0.00 |
| **↻ Données** (manuel) | Au clic | $0.00 |
| DXY / SP500 / QQQ / Or | Au clic ↻ Données uniquement | $0.00 |
| **✦ Analyser** (manuel) | Au clic | ~$0.03 |

Les données Twelve Data ne se mettent pas à jour automatiquement — le plan gratuit est limité à 8 req/min, un polling toutes les 60s dépasserait la limite.

### Calculs recalculés à chaque cycle auto

RSI 14 (Wilder) · EMA 50 / EMA 200 (deux TF) · Volume ratio · Pivot points · Grille de scoring · Détection zones de signal

---

## 14. Stratégie RSI + Volume + EMA 200 — rappel

### Les 3 filtres

```
Filtre 1 — EMA 200 (macro)
  BTC sous EMA 200 → longs courts, cibles réduites
  BTC au-dessus    → longs normaux, cibles étendues

Filtre 2 — RSI (timing)
  RSI < 30 sur le TF actif → zone d'entrée

Filtre 3 — Volume (confirmation)
  Spike ≥ 1.5× la moyenne sur la bougie d'entrée
```

### Les 3 setups par fiabilité

**1. Divergence haussière + volume** *(rare, puissant)* — Prix LL + RSI HL + volume en hausse → épuisement baissier.

**2. RSI < 30 + spike volume ≥ 2×** *(capitulation)* — RSI < 25 idéalement + bougie englobante haussière.

**3. Cross RSI 50 + volume** *(momentum)* — RSI croise 50 à la hausse en sortie de range.

### Gestion du risque en bear market

| Élément | Règle |
|---|---|
| Taille de position | Max 50% normale |
| Stop-Loss | Sous dernier support structurel |
| Take-Profit 1 | EMA 50 |
| Take-Profit 2 | Résistance suivante |
| Ne jamais viser | EMA 200 comme TP direct |
| R:R minimum | 1:2 |

---

## 15. FAQ & cas pratiques

### Q : Le score est à 2/6, je peux entrer ?

Non. Minimum 3/6 avec RSI < 30 ET volume ≥ 1.5× obligatoirement actifs.

### Q : RSI < 30 mais volume à ×0.3 ?

Attendre le spike. Volume faible = piège baissier fréquent. Placer une alerte sur TradingView.

### Q : Les onglets sont en surbrillance — que faire ?

Cliquer **↻ Données** puis **✦ Analyser**. Les onglets eux-mêmes sont de la navigation pure ($0.00).

### Q : Combien coûte une session type ?

↻ Données = $0.00 · ✦ Analyser = ~$0.03 · Navigation onglets = $0.00. Avec $5 → ~150 sessions.

### Q : RSI 4H = 28 et RSI Daily = 38 — j'entre ?

Daily confirme la zone, 4H confirme le timing. Si le volume confirme aussi → setup valide court terme. Cible conservatrice (EMA 50 4H) car le Daily n'est pas encore en survente.

### Q : Quelle hausse espérer après un spike ×1.5 en bear market ?

4H : +4% à +8% sur 3-5 bougies. Daily : +6% à +12% si RSI < 30 + support. L'EMA 50 plafonne souvent le rebond.

### Q : Les badges AI n'apparaissent pas sur le graphique ?

L'analyse doit être dans la plage de dates visible. En 4H (60 bougies ≈ 10 jours), une analyse de J-15 est invisible. Passer en Daily (30 jours) ou générer une nouvelle analyse.

### Q : DXY affiche "Clé Twelve Data requise" ?

Configurer la clé Twelve Data dans le bloc en bas de page. Sans elle, DXY/SP500/QQQ/Or ne se chargent pas — le reste fonctionne normalement.

### Q : Failed to fetch sur Claude en local ?

Ouvrir le fichier directement depuis le navigateur (`file://`). Le header CORS Anthropic est inclus. Si ça persiste, vérifier la clé sur [console.anthropic.com](https://console.anthropic.com).

---

*Pas un conseil financier — DYOR.*  
*Sources : Binance API · Alternative.me · Twelve Data · Claude API (Anthropic)*
