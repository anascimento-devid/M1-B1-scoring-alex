# Expériences — M1-B1 Pyrenex Crédit (Lending Club)

> Trace tes runs au fur et à mesure. Format imposé : un bloc par run, avec
> date, modèle, hyperparams, métriques **test interne uniquement**, verdict.
> Commit à chaque run final (pas à chaque essai jetable).
>
> ⚠️ **Règle d'or — comparabilité.** Le holdout **n'apparaît jamais** dans les
> blocs `exp_NNN`. Il sort **une seule fois**, pour le modèle retenu, dans
> la section finale en bas de fichier. Cf. mini-cours 04.

---

## exp_001 — RF par défaut

- **Date** : 2026-07-06 14:15
- **Modèle** : RandomForestClassifier (sklearn X.Y.Z)
- **Dataset** : lending_club_train.csv (sha256 …), n=…
- **Split** : test_size=0.2, stratify=y, random_state=42
- **Hyperparamètres** : tous par défaut, `n_jobs=-1`, `random_state=42`
- **Pré-traitement** : OneHotEncoder + StandardScaler (Pipeline scikit-learn)
- **Métriques (test interne)** :
  - F1 macro : 0.5130781275832185,
  - F1 défaut : 0.1268882175226586,
  - ROC-AUC : 0.7170383706531133
  - Recall défaut : 0.07134767836919592
- **Temps d'entraînement** : 00.682319s
- **Verdict** : Le modèle par défaut à un recall très bas. Il ne détecte que 7% des défauts. Il est donc très conservateur et peu utile pour la détection de défauts.

---

## exp_002 — RF balanced (TODO — remplis avec ta config)

- **Date** : 2026-07-06 14:30
- **Modèle** : andomForestClassifier (sklearn X.Y.Z)
- **Hyperparamètres** : tous par défaut, `n_jobs=-1`, `random_state=42`, `n_estimators=200`, `class_weight='balanced'`, `min_samples_leaf_=10`, `max_depth=10`
- **Pré-traitement** : OneHotEncoder + StandardScaler (Pipeline scikit-learn)
- **Métriques (test interne)** :
  - F1 macro : 0.6123249144870954,,
  - F1 défaut : 0.43262135922330097,,
  - ROC-AUC : 0.7441708775321211,
  - Recall défaut : 0.6308040770101925
- **Temps d'entraînement** : 00.587717s
- **Verdict** : Ces paramètres améliorent massivement le recall, passant de 7% à 63%. Le modèle est beaucoup plus sensible aux défauts, ce qui est crucial pour l'objectif métier. Cependant, il faut surveiller la précision pour éviter trop de faux positifs.
- Egalement, le F1 defaut est passé de 0.126 à 0.432, ce qui est une amélioration significative. Le F1 macro a également augmenté, indiquant un meilleur équilibre entre les classes.
- On note aussi une amélioration du ROC-AUC, passant de 0.717 à 0.744, ce qui montre que le modèle est globalement meilleur pour distinguer entre les classes.

---

## exp_003 —  RF balanced_regularized 

- **Date** : 2026-07-06 14:30
- **Modèle** : andomForestClassifier (sklearn X.Y.Z)
- **Hyperparamètres** : tous par défaut, `n_estimators: 500`,
        `max_depth: 12`,
        `min_samples_leaf: 30`,
        `min_samples_split: 50`,
        `max_features: sqrt`,
        `class_weight: balanced_subsample`,
        `random_state: 42`,
        `n_jobs: -1`,
- **Pré-traitement** : OneHotEncoder + StandardScaler (Pipeline scikit-learn)
- **Métriques (test interne)** :
  - F1 macro : 0.6088421829868015,
  - F1 défaut : 0.43273967411898445,
  - ROC-AUC : 0.7443541828155056,
  - Recall défaut : 0.6466591166477916
- **Temps d'entraînement** : 01.783303s
- **Verdict** : Le modèle regularisé avec des hyperparamètres ajustés montre une amélioration du recall à 64,7%, ce qui est légèrement supérieur à l'expérience précédente. 
- Le F1 défaut reste stable autour de 0.432, et le ROC-AUC est également légèrement meilleur. 
- Cela suggère que la régularisation et les ajustements des hyperparamètres ont permis de mieux capturer les défauts tout en maintenant un bon équilibre entre les classes.
---

## 🏁 Évaluation finale sur holdout (modèle retenu)

> **À remplir une seule fois**, à la tâche 5 du brief, **après** avoir choisi
> ton modèle retenu parmi les `exp_NNN` ci-dessus. Le holdout n'est consulté
> qu'ici.

- **Date** : YYYY-MM-DD HH:MM
- **Expérience retenue** : exp_NNN
- **Modèle persisté** : `models/pyrenex_risk_v2.joblib`
- **Données holdout** : `data/lending_club_holdout.csv` (sha256 …, n=…)
- **Métriques** :
  - F1 macro : …
  - F1 défaut : …
  - ROC-AUC : …
  - Recall défaut : …
- **Matrice de confusion** :

|  | Pred Fully Paid | Pred Charged Off |
|---|---|---|
| **Vrai Fully Paid** | … | … |
| **Vrai Charged Off** | … | … |

- **Comparaison baseline 2017** : (cf. `verdict.md`)