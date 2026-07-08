# Verdict — Modèle de scoring Pyrenex Crédit v2

> Document destiné à Sophie Léger (Lead Data, Pyrenex Crédit).
> 1 page max.

## Contexte

(En une phrase : pourquoi ce travail, qu'est-ce qui était attendu.)

## Démarche

(En 2-3 phrases : dataset utilisé, split, nombre de configurations testées,
critères d'évaluation.)

## Verdict chiffré

| Métrique | Baseline 2017 (Pyrenex-risk-v1) | Modèle retenu (v2) | Variation |
|---|---------------------------------|---|-----------|
| F1 macro (holdout) | 0.5018                          |  0.6088 | +0.10     |
| F1 défaut | 0.09                             | 0.4327 | +0.34     |
| ROC-AUC | 0.7296                          | 0.7443 | +0.01     |
| Recall défaut | 0.05                            | 0.64 | +0.59     |

**Configuration retenue** : (rappel des hyperparamètres principaux)
    
```
"balanced_regularized": {
        "n_estimators": 500,
        "max_depth": 12,
        "min_samples_leaf": 30,
        "min_samples_split": 50,
        "max_features": "sqrt",
        "class_weight": "balanced_subsample",
        "random_state": 42,
        "n_jobs": -1,
    },
```
## Trade-off explicité au métier

Le nouveau modèle déclare plus facilement des faux positifs, et détecte donc bien plus de dossiers problématiques, parfois à tort.

En contrepartie, il détecte avec une précision supérieure à la baseline les vrais défauts (F1 défaut : 0.43 vs 0.09).


## Précautions avant mise en production

- Vérifier que le **schéma d'entrée** en production correspond exactement
  au schéma d'entraînement (cf. `pyrenex_risk_v2.json` → `feature_columns`)
- Re-évaluer le **seuil de décision** (0.5 par défaut) avec l'équipe
  métier — un seuil 0.3 peut être plus adapté selon l'appétence au risque
- Mettre en place un **monitoring** dès le déploiement (cf. M5/M6)
- Surveiller les **variables sensibles** identifiées (FICO, état US,
  revenu) — risque de disparate impact à auditer (M2/M7)

## Recommandation

✅ **Remplacer Pyrenex-risk-v1** 
L'objectif étant de détecter plus de défauts, le modèle v2 est recommandé pour la mise en production au vu de performances bien supérieures sur le sujet.
---

*Signé : Alexandre NASCIMENTO, FastIA, le 2026-07-08*
