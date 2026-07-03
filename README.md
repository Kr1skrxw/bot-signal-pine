# Bot Signal — EMA/RSI/Fractals (Pine Script v6)

Stratégie complète pour TradingView en Pine Script v6.

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

## Gestion de l'ordre
- **Un seul ordre actif à la fois** — nouveau signal ignoré si position ouverte
- **SL** : dernière fractal basse (achat) / dernière fractal haute (vente) + buffer configurable
- **TP1** : 33% de la position fermée (configurable, défaut 1%)
- **TP2** : 50% du restant fermé (configurable, défaut 2%)
- **TP Libre** : solde de la position, à déplacer manuellement

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
4. Ajuster TP1/TP2/SL Buffer dans les paramètres de la stratégie
