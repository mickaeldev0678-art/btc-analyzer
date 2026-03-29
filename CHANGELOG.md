# Changelog

## [1.3.0] — 2026-03-29

### Ajouté
- Graphique pleine largeur avec tooltip interactif, crosshair et histogramme de volume
- Badges AI cliquables sur le graphique — marquent les dates/prix de génération Claude
- EMA 200 et EMA 50 toujours visibles même hors plage de prix (label + distance %)
- Calendrier macro FOMC/CPI/NFP/PCE avec compte à rebours live (JS pur, sans API)
- Bloc corrélations : DXY (UUP), SP500 (SPY), Nasdaq 100 (QQQ), Or (XAU/USD)
- Clé API Twelve Data pour les données macro (plan gratuit 800 req/jour)
- Historique des analyses séparé par TF — 5 entrées max par TF en localStorage
- Deux boutons distincts : ↻ Données (KPIs) et ✦ Analyser (Claude)
- Bloc configuration API Keys déplacé en bas de page

### Modifié
- Layout restructuré : graphique pleine largeur, S&R + Confluence côte à côte,
  Claude pleine largeur avec historique en colonne droite
- Switch TF vide le cache Claude et recharge l'historique du TF actif
- ↻ Données : ne génère plus les analyses Claude (séparation nette des responsabilités)
- DXY couleur inversée (rouge = dollar monte = pression baissière BTC)

---

## [1.2.0] — 2026-03-28

### Ajouté
- Switcher 4H / Daily — les deux TF chargés simultanément au démarrage
- Alertes automatiques — badges NEW sur les onglets quand une zone de signal change
- Historique des analyses Claude (5 entrées en localStorage)
- Séparation refreshData() (auto) / refreshKPIs() (manuel)

### Modifié
- Les 3 boutons Claude deviennent des onglets de navigation (sans appel API au clic)
- ↻ Refresh génère les 3 analyses en une passe
- Header CORS `anthropic-dangerous-direct-browser-access: true` pour usage en file://

---

## [1.1.0] — 2026-03-27

### Ajouté
- Alertes de changement de zone (RSI, F&G, Volume, EMA cross)
- Surbrillance des boutons avec badge NEW
- Snapshot initial au premier chargement

---

## [1.0.0] — 2026-03-27

### Initial
- Prix BTC temps réel via WebSocket Binance
- RSI 14, EMA 50, EMA 200 depuis Binance klines
- Fear & Greed Index via Alternative.me
- Pivot Points (S1/S2/S3, R1/R2/R3)
- Grille de confluence 6 signaux
- Sparkline Canvas avec EMA overlay
- 3 analyses Claude AI (Points d'entrée, Gestion du risque, Scénarios)
- Clé API Claude en localStorage
- Thème dark, responsive
