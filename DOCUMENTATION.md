# BTC Analyzer — Documentation

> Outil d'analyse technique Bitcoin standalone (fichier HTML local).  
> Données live via Binance API + Alternative.me · Analyse IA via Claude API.

---

## Table des matières

1. [Installation & démarrage](#1-installation--démarrage)
2. [Configuration de la clé API Claude](#2-configuration-de-la-clé-api-claude)
3. [Switcher 4H / Daily](#3-switcher-4h--daily)
4. [Indicateurs en temps réel](#4-indicateurs-en-temps-réel)
5. [Graphique prix et volume](#5-graphique-prix-et-volume)
6. [Supports & résistances](#6-supports--résistances)
7. [Grille de confluence — Score d'entrée](#7-grille-de-confluence--score-dentrée)
8. [Analyses Claude AI](#8-analyses-claude-ai)
9. [Alertes automatiques (surbrillance des boutons)](#9-alertes-automatiques-surbrillance-des-boutons)
10. [Actualisation des données](#10-actualisation-des-données)
11. [Stratégie RSI + Volume + EMA 200 — rappel](#11-stratégie-rsi--volume--ema-200--rappel)
12. [FAQ & cas pratiques](#12-faq--cas-pratiques)

---

## 1. Installation & démarrage

Aucune installation requise. Le fichier `btc-analyzer.html` est **100% standalone**.

```
1. Télécharger btc-analyzer.html
2. Double-cliquer pour l'ouvrir dans votre navigateur
3. Les données Binance se chargent automatiquement
```

> **Navigateurs compatibles** : Chrome, Firefox, Edge, Safari (version récente).  
> **Connexion internet requise** pour les données live.

Les APIs de données de marché (Binance, Alternative.me) sont **publiques et gratuites** — aucune clé n'est nécessaire pour les indicateurs. Seule l'analyse Claude AI nécessite une clé.

---

## 2. Configuration de la clé API Claude

La clé API permet d'activer les 3 boutons d'analyse IA dans le panneau Claude.

### Comment configurer

```
1. Aller sur https://console.anthropic.com
2. Créer une clé API (section "API Keys")
3. Copier la clé (format : sk-ant-api03-...)
4. Coller dans le champ en haut de la page
5. Cliquer "Sauvegarder"
```

Le bandeau passe au vert : **Configurée ✓**

### Stockage

La clé est sauvegardée dans le `localStorage` du navigateur — elle persiste entre les sessions. Elle **n'est jamais envoyée** à un tiers, uniquement à `api.anthropic.com` lors des appels IA.

> ⚠️ **Sécurité** : ne partagez jamais le fichier HTML si votre clé y est sauvegardée. Révoquez la clé sur la console Anthropic si vous suspectez une fuite.

### Coût estimé

Chaque appel Claude (1 des 3 boutons) consomme environ **800–1 200 tokens input + 400–600 tokens output**.  
Avec `claude-sonnet-4`, cela représente environ **$0.005 à $0.01 par analyse**.

---

## 3. Switcher 4H / Daily

Deux boutons en haut à droite des indicateurs permettent de basculer entre les deux timeframes.

| | **4H** | **Daily** |
|---|---|---|
| Horizon de trading | Quelques heures à 2-3 jours | 1 à 4 semaines |
| Type | Intraday / Day trading | Swing trading |
| RSI réagit en | Quelques heures | 2-3 jours |
| Faux signaux | Plus fréquents | Moins fréquents |
| Usage recommandé | Timer l'entrée précise | Identifier la zone d'achat |

### Comment utiliser les deux ensemble

Le workflow optimal est de **lire le Daily d'abord, puis le 4H pour entrer** :

```
1. Passer en Daily
   → Le RSI Daily est à 32, BTC approche d'un support S1
   → Zone d'achat potentielle identifiée

2. Passer en 4H
   → Attendre que le RSI 4H descende sous 30
   → Attendre un spike de volume ≥ 1.5×
   → C'est le signal d'entrée précis
```

> **Exemple concret** : BTC à $66 000. RSI Daily = 34 (zone de surveillance). RSI 4H = 28 (survente) + volume ×1.8 sur la bougie → **entrée valide**.

### Ce qui change au switch

- Labels des cartes RSI / EMA 50 / EMA 200
- Sparkline : 60 bougies en 4H, 30 bougies en Daily
- Volume : comparé sur les 20 dernières bougies du TF actif
- Grille de scoring et plan d'action recalculés
- Détection de changement de signal réinitialisée

---

## 4. Indicateurs en temps réel

### RSI 14

Le RSI (Relative Strength Index) mesure la vitesse et l'amplitude des variations de prix.

| Valeur RSI | Zone | Interprétation |
|---|---|---|
| < 25 | Survente extrême | 🟢 Setup prioritaire — signal fort |
| 25–30 | Survente | 🟢 Zone d'entrée RSI active |
| 30–40 | Bas-neutre | 🟡 Surveiller, se rapprocher |
| 40–60 | Neutre | Attendre |
| 60–70 | Haut-neutre | Prudence sur les longs |
| > 70 | Surachat | 🔴 Éviter les entrées longues |

La jauge visuelle montre la position de l'aiguille en temps réel.

**Exemple de lecture** :
```
RSI 4H = 27.4 → 🟢 "Survente — zone d'entrée RSI"
→ Vérifier le volume avant d'agir
```

### EMA 200

La moyenne mobile exponentielle sur 200 périodes est le **filtre de tendance macro**.

- **BTC AU-DESSUS de l'EMA 200** → biais haussier → longs normaux, cibles étendues
- **BTC EN-DESSOUS de l'EMA 200** → biais baissier → longs réduits, cibles courtes uniquement

> En mars 2026, l'EMA 200 Daily est à ~$86K–$93K. BTC à $66K est donc 23% en dessous → **biais baissier confirmé**.

### EMA 50

Résistance/support intermédiaire. En bear market (sous EMA 200) :
- L'EMA 50 est souvent un **plafond** sur les rebonds
- Un retour au-dessus de l'EMA 50 est le **premier signal d'amélioration** avant de viser l'EMA 200

### Fear & Greed Index

Indice de sentiment de marché de 0 à 100 (source : Alternative.me).

| Valeur | Zone | Lecture contrarian |
|---|---|---|
| 0–15 | Extrême Fear | Potentiel bottom, surveiller entrée |
| 15–30 | Fear | Zone d'accumulation possible |
| 45–55 | Neutre | Pas de signal de sentiment |
| 75–100 | Extrême Greed | Éviter les nouveaux longs |

> Un F&G < 15 pendant 3+ jours coïncide historiquement avec des zones de rebond.  
> **Combiné à RSI < 30** = signal de confluence rare et puissant.

---

## 5. Graphique prix et volume

### Sparkline

Affiche les dernières bougies avec :
- **Ligne de prix** colorée (vert si hausse sur la période, rouge si baisse)
- **Zone de remplissage** sous la ligne
- **Point orange** = prix actuel
- **Ligne bleue pointillée** = EMA 200 (si dans la plage affichée)
- **Ligne violette pointillée** = EMA 50 (si dans la plage affichée)

### Volume vs Moyenne

Compare le volume de la dernière période à la moyenne des 20 périodes précédentes.

| Ratio | Couleur | Signal |
|---|---|---|
| < 1.0× | Gris | Volume faible — pas de confirmation |
| 1.0–1.5× | Gris | Volume normal |
| 1.5–2.0× | Orange | 🟡 Volume modéré — confirmation partielle |
| ≥ 2.0× | Vert | 🟢 Spike de volume — confirmation valide |

**En 4H** : volume de la dernière bougie 4H en M$ (millions)  
**En Daily** : volume 24h en B$ (milliards)

> **Exemple** : Volume ratio ×2.3 en vert + RSI 4H à 26 = confluence forte → signal d'entrée valide.

---

## 6. Supports & Résistances

Le tableau affiche les niveaux clés triés de haut en bas avec la distance en % depuis le prix actuel.

### Types de niveaux

| Badge | Type | Description |
|---|---|---|
| `EMA` | EMA 50 / EMA 200 | Moyennes mobiles dynamiques |
| `R` | R1, R2, R3 | Résistances pivot classiques |
| `▶` | PRIX NOW | Position actuelle (ligne orange) |
| `S` | S1, S2, S3 | Supports pivot classiques |
| `P` | Pivot | Point pivot central |

### Calcul des pivots

Les pivots sont calculés depuis la bougie précédente complète (H + L + C) :

```
Pivot (P) = (High + Low + Close) / 3
R1 = 2×P − Low     S1 = 2×P − High
R2 = P + (H − L)   S2 = P − (H − L)
R3 = High + 2×(P−Low)   S3 = Low − 2×(High−P)
```

### Lecture pratique

```
Prix NOW à $66 600 (orange)
S1 à $66 291 → −0.5% → support immédiat proche
S2 à $64 362 → −3.4% → prochain support si S1 casse
EMA 50 à $74 600 → +12% → résistance intermédiaire
EMA 200 à $88 000 → +32% → résistance majeure
```

---

## 7. Grille de confluence — Score d'entrée

6 signaux évalués en temps réel. Chaque signal est représenté par des points :
- 🟢 **Vert** = signal actif
- 🟡 **Orange** = signal en approche
- ⚫ **Gris** = signal inactif

### Les 6 signaux

| Signal | Poids | Obligatoire |
|---|---|---|
| RSI < 30 — Zone survente | ●●● | **Oui** |
| Divergence RSI haussière possible | ●● | Non |
| Volume spike ≥ 1.5× moyenne | ●●● | **Oui** |
| Fear & Greed < 20 (capitulation) | ●● | Non |
| BTC sous EMA 200 (biais baissier) | ● | Contexte |
| Support pivot proche (S1/S2) | ●● | Non |

> **Minimum pour entrer : 3 signaux actifs**, dont obligatoirement **RSI < 30 ET Volume ≥ 1.5×**.

### Règle fondamentale

```
RSI < 30 seul sans volume → Piège baissier fréquent → NE PAS ENTRER
Volume ×2 seul sans RSI → Direction inconnue → NE PAS ENTRER
RSI < 30 + Volume ×1.5 + Support → Signal valide → ÉVALUER L'ENTRÉE
```

### Exemples de scores

**Score 2/6 — Trop tôt** *(situation du screenshot)*
```
RSI = 33.5 → en approche mais pas encore < 30
Volume = ×0.13 → très faible
F&G = 12 → Extreme Fear (positif)
Support S1 = 0.5% → proche (positif)
→ Placer alertes RSI=30 et attendre
```

**Score 4/6 — Conditions favorables**
```
RSI = 27 → ✅ Survente active
Volume = ×1.8 → ✅ Spike modéré
F&G = 11 → ✅ Extreme Fear
Support S1 = 0.2% → ✅ BTC dessus
→ Entrée longue court terme envisageable
```

**Score 5/6 — Setup optimal**
```
RSI = 24 → ✅ Survente extrême
Volume = ×2.4 → ✅ Spike fort
F&G = 8 → ✅ Capitulation extrême
Support S2 = 0.3% → ✅ Niveau solide
Divergence RSI → ✅ HL sur RSI
→ Meilleur ratio R:R disponible
```

---

## 8. Analyses Claude AI

Les 3 analyses sont **générées simultanément** par le bouton ↻ Refresh. Les boutons 📊 / 🛡 / 🎯 servent uniquement à **naviguer** entre les analyses déjà générées — aucun appel API au clic.

### Fonctionnement

```
↻ Refresh
  → Met à jour les données Binance + Fear & Greed
  → Appelle Claude 3 fois de suite (entry → risk → scenarios)
  → Met chaque résultat en cache
  → Affiche l'onglet actif

📊 / 🛡 / 🎯
  → Affiche l'analyse en cache (navigation instantanée, $0.00)
  → Si pas encore généré : "cliquez ↻ Refresh"
```

**Coût par ↻ Refresh** : ~$0.03 (3 appels × ~$0.01)  
**Coût par clic sur un onglet** : $0.00

### Ce que Claude reçoit en contexte

```
- Prix actuel + variation 24h
- RSI 14 du TF actif + RSI de l'autre TF (4H ou Daily)
- EMA 50 et EMA 200 des deux TF
- Volume ratio vs moyenne 20 périodes
- Fear & Greed Index
- Niveaux pivot S1/S2/R1/R2
- Contexte macro (conflit géopolitique, corrélation SP500, etc.)
- Timeframe actif + horizon de trading
```

### Les 3 onglets

#### 📊 Points d'entrée
Analyse orientée **timing et niveaux** :
- Évaluation du RSI actuel pour un point d'entrée
- Rôle de l'EMA 200 comme filtre
- Niveaux d'entrée précis avec conditions exactes (prix, RSI, volume)
- Take-profit et stop-loss recommandés

#### 🛡 Gestion du risque
Analyse orientée **risk management** :
- Taille de position recommandée selon le biais macro
- Stop-loss et take-profit avec prix précis
- Ratio R:R de l'opportunité actuelle
- Signaux d'invalidation de la thèse

#### 🎯 Scénarios
Analyse orientée **probabilités** :
- Scénario haussier — conditions déclenchantes + cibles + probabilité
- Scénario neutre — range de consolidation attendu
- Scénario baissier — niveaux d'invalidation + supports critiques

### Workflow type

```
1. Signal change → boutons en surbrillance orange (badge NEW)
2. Cliquer ↻ Refresh
   → Pendant ~15-30s : "Génération 1/3… 2/3… 3/3…"
   → Statut final : "3 analyses générées · ~1 450 tokens total"
3. Naviguer avec 📊 / 🛡 / 🎯 sans aucun coût supplémentaire
4. Décision prise → ouvrir la position
```

### Utilisation locale (file://)

Le fichier fonctionne directement en double-cliquant (protocole `file://`). L'appel à l'API Anthropic est autorisé grâce au header `anthropic-dangerous-direct-browser-access: true` — prévu par Anthropic pour ce cas d'usage.

> Aucun serveur proxy ou backend requis.

---

## 9. Alertes automatiques (surbrillance des boutons)

La page surveille **4 zones de signal** à chaque actualisation (toutes les 60s). Quand une zone change, les 3 onglets Claude s'illuminent en orange avec un badge **NEW** — signal que le moment est venu de cliquer ↻ Refresh.

### Les 4 zones surveillées

| Zone | Changement détecté si… |
|---|---|
| **RSI** | Franchit un seuil : `>70`, `60–70`, `40–60`, `30–40`, `25–30`, `<25` |
| **Fear & Greed** | Change de catégorie : Extreme Fear / Fear / Neutral / Greed / Extreme Greed |
| **Volume** | Passe de `normal` → `elevated` (×1.5) → `spike` (×2) ou inverse |
| **EMA cross** | BTC passe au-dessus/en-dessous de l'EMA 50 ou EMA 200 |

### Comportement

```
Changement détecté → onglets pulsent en orange + badge "NEW"
                   → message : "⚡ Signal changé — RSI: neutral → low-neutral"

↻ Refresh cliqué → surbrillance disparaît + 3 analyses régénérées
```

> Le premier chargement de page n'alerte jamais — il initialise le snapshot de référence.

### Exemple pratique

```
Page ouverte → snapshot initial : RSI zone "neutral", Volume "low"
60s plus tard → RSI passe de 32 à 29 (zone "oversold")
→ Onglets s'illuminent + badge NEW
→ Message : "⚡ Signal changé — RSI: low-neutral → oversold"
→ Cliquer ↻ Refresh pour générer les 3 analyses avec le contexte actuel
→ Naviguer avec 📊 / 🛡 / 🎯 pour lire chaque analyse
```

---

## 10. Actualisation des données

| Source | Fréquence | Données |
|---|---|---|
| **Binance WebSocket** | Temps réel (~1-2s) | Prix, variation 24h |
| **Binance REST** | Auto toutes les 60s | Klines 4H + Daily, volume |
| **Alternative.me** | Auto toutes les 60s | Fear & Greed Index |
| **↻ Refresh (manuel)** | Au clic | Données publiques + 3 analyses Claude |

### Cycle complet d'une session

```
Ouverture de la page
  → Chargement Binance 4H + Daily + Fear & Greed (gratuit)
  → Prix live via WebSocket
  → Indicateurs calculés, grille de scoring initialisée

Toutes les 60s (automatique)
  → Binance + Fear & Greed rechargés
  → RSI, EMA, volume, pivot points recalculés
  → Alertes de changement de zone évaluées
  → Si changement → onglets en surbrillance (badge NEW)

↻ Refresh (manuel, sur surbrillance ou à la demande)
  → Données publiques rechargées
  → 3 appels Claude successifs (~15-30s)
  → Analyses mises en cache, onglet actif affiché
  → Surbrillance effacée
```

### Calculs recalculés à chaque cycle auto

- RSI 14 (méthode Wilder smoothed) sur les 220 bougies Daily / 500 bougies 4H
- EMA 50 et EMA 200 depuis les closes Binance
- Ratio volume vs moyenne 20 périodes du TF actif
- Pivot points depuis la dernière bougie complète
- Grille de scoring et plan d'action
- Détection de changement de zone (4 critères)

---

## 11. Stratégie RSI + Volume + EMA 200 — rappel

### La règle des 3 filtres

```
Filtre 1 — EMA 200 (tendance macro)
  BTC sous EMA 200 → longs courts uniquement, cibles réduites
  BTC au-dessus    → longs normaux, cibles étendues

Filtre 2 — RSI (timing d'entrée)
  Attendre RSI < 30 sur le TF actif avant d'entrer

Filtre 3 — Volume (confirmation)
  Spike ≥ 1.5× la moyenne sur la bougie d'entrée
```

### Les 3 setups par ordre de fiabilité

**1. Divergence haussière + volume** *(le plus rare, le plus puissant)*
```
Prix : Lower Low (LL)
RSI  : Higher Low (HL) simultané → divergence
Volume : en hausse sur la 2e bougie
→ Signal d'épuisement baissier — meilleur R:R
```

**2. RSI < 30 + spike volume ≥ 2×** *(entrée en capitulation)*
```
RSI < 30, idéalement < 25
Volume ≥ 2× la moyenne
Bougie englobante haussière
→ Entrée au close ou sur pull-back
```

**3. Cross RSI 50 à la hausse + volume** *(momentum)*
```
RSI croise 50 à la hausse avec volume fort
Sortie de zone de consolidation
→ Entrée momentum, moins défensive
```

### Gestion du risque en bear market (BTC sous EMA 200)

| Élément | Règle |
|---|---|
| Taille de position | Max 50% de la taille normale |
| Stop-Loss | Sous le dernier support structurel |
| Take-Profit 1 | EMA 50 (première résistance) |
| Take-Profit 2 | Résistance suivante si TP1 cassé |
| Ne jamais viser | L'EMA 200 comme TP direct |
| R:R minimum | 1:2 avant d'entrer |

---

## 12. FAQ & cas pratiques

### Q : Le score est à 2/6, je peux quand même entrer ?

Non. Le minimum est 3/6 **avec RSI < 30 ET volume ≥ 1.5×** actifs. Un score 2/6 sans ces deux signaux = attendre.

### Q : RSI < 30 mais volume à ×0.3 — que faire ?

Placer une alerte prix et RSI sur TradingView, ne rien faire. Volume faible = pas de conviction = risque de continuation baissière. Attendre le spike.

### Q : Les onglets Claude sont en surbrillance, que faire ?

Cliquer **↻ Refresh** — pas les onglets. Le Refresh génère les 3 analyses en une fois avec les données les plus récentes. Les onglets 📊 / 🛡 / 🎯 permettent ensuite de naviguer entre les analyses sans coût supplémentaire.

### Q : Combien coûte un ↻ Refresh ?

Environ **$0.03** (3 appels Claude × ~$0.01 chacun). Cliquer sur les onglets 📊 / 🛡 / 🎯 après le Refresh = $0.00.  
Avec $5 de crédit → environ **150 à 160 Refresh complets**, soit plusieurs mois d'usage quotidien.

### Q : En 4H le RSI est à 28, en Daily il est à 38 — j'entre ?

Le **Daily confirme la zone** (38 = neutre/bas), le **4H confirme le timing** (28 = survente). C'est le combo idéal — si le volume confirme aussi, c'est un setup valide pour un trade court (quelques heures à 2-3 jours). Cible courte car le Daily n'est pas encore en survente.

### Q : Quelle hausse espérer après un spike ×1.5 ?

En contexte bear market (BTC sous EMA 200), les rebonds post-spike se limitent généralement à :
- **+4% à +8%** en 4H sur les 3-5 bougies suivantes
- **+6% à +12%** en Daily si RSI < 30 + support solide

La résistance EMA 50 plafonne souvent le rebond. Prendre des profits partiels à l'approche de l'EMA 50.

### Q : À quelle fréquence le score atteint 4/6 ou plus ?

En bear market, rarement — peut-être **2 à 4 fois par mois** en Daily. C'est normal. La stratégie est conçue pour attendre les setups de qualité, pas pour trader en permanence.

### Q : La page ne fonctionne pas en local (Failed to fetch) ?

Le fichier doit être ouvert directement depuis le navigateur (`file://`). Le header `anthropic-dangerous-direct-browser-access: true` est inclus dans le code pour autoriser l'accès direct à l'API Anthropic sans serveur proxy. Si l'erreur persiste, vérifier que la clé API est bien configurée et valide sur [console.anthropic.com](https://console.anthropic.com).

---

*Pas un conseil financier — DYOR. Données : Binance API · Alternative.me · Claude API (Anthropic)*
