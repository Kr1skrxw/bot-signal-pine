# Bot Signal — EMA/RSI/Fractals (Pine Script v6)

Stratégie complète pour TradingView en Pine Script v6 avec alertes webhook JSON.

## Indicateurs
| Indicateur | Paramètre |
|---|---|
| EMA | 55 et 233 (sur le close) |
| RSI | période 13 |
| SMA du RSI | période 8 |
| Fractals | Williams (5 bougies) |

## Logique de signal

| Sens | Condition |
|---|---|
| **ACHAT** | EMA 55 > EMA 233 **ET** RSI > 50 **ET** RSI > SMA(RSI) |
| **VENTE** | EMA 55 < EMA 233 **ET** RSI < 50 **ET** RSI < SMA(RSI) |

> Le signal se déclenche sur le **bord montant** uniquement (transition false→true) pour éviter les alertes répétées.

## Gestion de l'ordre
- **Un seul ordre actif à la fois** — nouveau signal ignoré si position ouverte
- **SL** : dernière fractal basse (achat) / dernière fractal haute (vente) + buffer configurable
- **TP1** : 33 % de la position fermée (configurable, défaut 1 %)
- **TP2** : 50 % du restant fermé (configurable, défaut 2 %)
- **TP Libre** : solde de la position, à déplacer manuellement

## Alertes Webhook

Quatre `alertcondition()` disponibles :

| Nom | Déclenchement | Payload JSON |
|---|---|---|
| `ACHAT` | Signal long (bord montant) | `action, symbol, price, sl, tp1, tp2, time, tf` |
| `VENTE` | Signal short (bord montant) | idem |
| `CLOSE LONG (SL)` | `low <= longSL` en position long | `action:close, side, reason, symbol, price, time` |
| `CLOSE SHORT (SL)` | `high >= shortSL` en position short | idem |
| `BOT SIGNAL (all)` | Toutes les conditions combinées | JSON dynamique selon le cas |

### Exemple de payload ACHAT
```json
{
  "action": "buy",
  "symbol": "BTCUSDT",
  "price": 65432.10,
  "sl": 64100.00,
  "tp1": 66086.42,
  "tp2": 66741.74,
  "time": "2026-07-03T21:00:00Z",
  "tf": "60"
}
```

### Configuration dans TradingView
1. Clic droit sur l'indicateur → **Add alert**
2. Condition : choisir l'alertcondition souhaitée
3. Onglet **Notifications** → cocher **Webhook URL** → coller l'URL de ton bot
4. Le champ **Message** est pré-rempli automatiquement par le script

### Compatibilité brokers
| Broker/Middleware | Utiliser |
|---|---|
| 3Commas | `BOT SIGNAL (all)` ou alertes séparées |
| Alertatron | Alertes séparées (`ACHAT`, `VENTE`, `CLOSE *`) |
| WunderTrading | `BOT SIGNAL (all)` |
| Bot maison (FastAPI, n8n…) | `BOT SIGNAL (all)` + parser `action` |

## Paramètres
| Paramètre | Défaut | Description |
|---|---|---|
| TP1 (%) | 1.0 | Premier objectif en % |
| TP2 (%) | 2.0 | Deuxième objectif en % |
| SL Buffer (%) | 0.1 | Marge sous/sur la fractal |

## Installation
1. Ouvrir TradingView → Pine Editor
2. Coller le contenu de `bot_signal_ema_rsi_fractals.pine`
3. Cliquer **Add to chart**
4. Créer les alertes via clic droit → **Add alert**
5. Configurer le Webhook URL dans les notifications
