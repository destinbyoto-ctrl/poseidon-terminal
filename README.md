# 🔱 Poseidon Terminal

Terminal de trading IA **réel** — pas une simulation : chaque ordre, en mode réel, part directement de ton navigateur vers Binance via tes clés API (ordres signés HMAC-SHA256, `/api/v3/order`).

## L'application
https://destinbyoto-ctrl.github.io/poseidon-terminal/

## Landing page
https://destinbyoto-ctrl.github.io/poseidon-terminal/landing.html

---

## Comment ça fonctionne

### Architecture
- **Application 100% navigateur** (un seul fichier `index.html`, JS pur) : aucun serveur, aucune donnée envoyée à un tiers.
- Clés API et tokens restent dans le `localStorage` de ton appareil.
- Flux prix toutes les **4 secondes** (`/api/v3/ticker/price`, fallback Bybit) → P&L flottant et clôtures TP/SL quasi instantanés.
- Moteur **ML intégré** (pur JS) : entraînable sur l'historique réel des bougies, combiné aux indicateurs (RSI, MACD, ATR, ROC, volume).

### Les 5 robots (Binance spot)
| Bot | Cycle | Règle d'entrée | TP / SL |
|---|---|---|---|
| 🟢 Chasseur de vert | 8 s | Confluence momentum+volume+ML ≥ seuil, 6 paires | +0,35 % / −0,25 % |
| 🚀 Momentum ML | 2 min | Score RSI+MACD+ATR+ROC + ML ≥ 65 % | +1,2 % / −0,6 % |
| 🔄 RSI Mean Reversion | 1 min | RSI < 25 + confirmation bougie verte | +1,5 % / −0,8 % |
| 🎯 Sniper Élite BTC | 30 s | Confluence mom 1m + trend 5m + volume + RSI + ML ≥ 75 %, BTCUSDT seulement | +1,0 % / −0,5 % |
| 🪙 Accumulateur DCA | programmé | Achats réguliers + renforts en baisse | long terme |

### Les 3 brokers (espaces séparés)
- 🟡 **Binance** : crypto spot, réel ou papier (frais 0,1 %/côté simulés).
- 🔴 **Deriv** : indices de volatilité R_10–R_100, WebSocket direct.
- 🔵 **MT4/MT5** : copieur de signaux via MetaAPI, mapping de symboles.

---

## Passer en RÉEL sur le marché (mode par défaut : démo)

1. Ouvre l'app → mot de passe → onglet **Binance**.
2. **Réglages → 🟡 Binance** : colle ta clé API et ton secret (clé créée sur binance.com → API Management, avec *Enable Spot & Margin Trading*, **sans** restriction IP, retraits désactivés).
3. **« 🔴 Tester & activer le réel »** → Poseidon vérifie tes clés, lit ton vrai solde USDT et bascule en mode réel.
4. Arme le switch **« Trading autonome »** de l'accueil (sans lui, aucun bot n'exécute, même en démo).
5. Arme les bots que tu veux (🟢 🚀 🔄 🎯 🪙). Chaque achat et chaque vente aux TP/SL est alors un **vrai ordre Binance market** signé depuis ton navigateur.

Sécurités :
- Le mode démo reste **par défaut** ; le réel n'est jamais implicite.
- Switch « Trading autonome » séparé **par broker**.
- Chaque position part toujours avec son TP et son SL.
- Mode papier = frais réalistes (0,1 % par côté) pour des stats honnêtes.

## Avertissement
Le trading comporte un risque élevé de perte en capital. Les bots suivent des règles strictes mais **aucun système ne garantit un profit**. Commence toujours en démo, puis avec de petites mises.
