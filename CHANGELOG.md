# Changelog

## [1.2.0] — 2026-03-28

### Ajouté
- **Switcher 4H / Daily** — bascule instantanée entre les deux timeframes
- Les deux TF sont chargés simultanément au démarrage (500 bougies 4H + 220 Daily)
- Calcul du volume ratio adapté au TF actif (dernière bougie 4H vs Daily 24h)
- Labels et sparkline mis à jour dynamiquement au switch
- Claude reçoit les indicateurs des deux TF en contexte

### Modifié
- Les 3 boutons Claude (📊 / 🛡 / 🎯) sont maintenant des **onglets de navigation**
- Le bouton **↻ Refresh** génère les 3 analyses simultanément (~$0.03)
- Navigation entre analyses sans coût supplémentaire ($0.00 par clic d'onglet)
- Progression visible pendant la génération : `Appel 1/3… 2/3… 3/3…`
- Statut final : `3 analyses générées · N tokens total`

### Corrigé
- Header CORS `anthropic-dangerous-direct-browser-access: true` pour l'usage en `file://`

---

## [1.1.0] — 2026-03-27

### Ajouté
- **Alertes automatiques** — les boutons Claude s'illuminent (badge NEW) quand une zone de signal change
- Surveillance de 4 zones toutes les 60s : RSI, Fear & Greed, Volume, EMA cross
- Message explicatif du changement détecté (`⚡ Signal changé — RSI: neutral → low-neutral`)
- Snapshot initial au premier chargement (pas d'alerte au démarrage)
- Séparation `refreshData()` (auto, APIs publiques) / `refreshAll()` (manuel, données + Claude)

---

## [1.0.0] — 2026-03-27

### Initial
- Prix BTC temps réel via WebSocket Binance
- RSI 14, EMA 50, EMA 200 calculés depuis Binance klines
- Fear & Greed Index via Alternative.me
- Pivot Points (S1/S2/S3, R1/R2/R3)
- Grille de confluence 6 signaux avec plan d'action
- Sparkline Canvas 30 jours avec EMA overlay
- 3 analyses Claude AI (Points d'entrée, Gestion du risque, Scénarios)
- Clé API Claude en localStorage (jamais dans le fichier)
- Thème dark, responsive
