# Datasheet — German Credit (version Eckmühl, clean)

> **Modèle Gebru et al. (2018), version simplifiée 7 sections, 1 page max.**
> À transmettre avec le dataset Parquet livré à Eckmühl.
> Public cible : DPO + équipe métier — lisible par un non-data-scientist.

## 1. Motivation

Ce dataset a été crée par Hans Hofman, il classe les personnes, selon un ensemble d'attributs, en fonction de leur risque de crédit (bon ou mauvais). 

## 2. Composition

> Combien d'observations, quelles colonnes, quels types, distribution de la
> cible, **variables sensibles signalées explicitement**.

| Aspect | Valeur |
|---|---|
| Nombre de lignes | 1000 |
| Nombre de colonnes | 21 |
| Cible | `credit_risk` : `good_credit` / `bad_credit` |
| Distribution cible | `good_credit` 70 % / `bad_credit` 30 % |
| Variables sensibles | `age`, `personal_status_sex` ⚠️ composite, `foreign_worker` |

**Schéma des colonnes** (types + modalités pour les catégorielles) :

| Colonne | Type | Modalités / range | Note |
|---|---|---|---|
| `duration_months` | int | 4 — 72 | |
| `credit_amount` | int | 250 — 18424 | |
| `installment_rate_pct_income` | int | 1 — 4 | |
| `residence_since_years` | int | 1 — 4 | |
| `age` | int | 19 — 75 | |
| `n_existing_credits` | int | 1 — 4 | |
| `n_people_liable` | int | 1 — 4 | |
| `n_people_liable` | int | 1 — 4 | |
| `purpose` | str | 'radio/TV'-'education'-'furniture/equipment'-'car (new)'-'car (used)'-'business'-'domestic appliances'-'repairs'-'others'-'retraining' | |
| `other_installment_plans` | str | 'none'-'bank'-'stores' | |
| `housing` | str | 'own'-'for free'-'rent' | |
| `telephone` | str | 'yes, registered'-'none' | |
| `other_debtors` | str | 'none'-'guarantor'-'co-applicant' | |
| `job` | str | 'skilled employee / official'-'unskilled resident'-'management / self-employed / highly qualified'-'unemployed / unskilled non-resident' | |
| `credit_history` | str | 'critical account / other credits existing'-'existing credits paid back duly'-'delay in paying off in past'-'no credits taken / all paid'-'all credits at this bank paid' | |
| `checking_account_status` | int |'< 0 DM'-'0 to 200 DM'-'no checking account'-'>= 200 DM / salary assignments' | |
| `savings_account` | int | 'unknown / no savings'-'< 100 DM' '500-1000 DM'-'>= 1000 DM'-'100-500 DM' | |
| `employment_since` | int | '>= 7 years'-'1-4 years'-'4-7 years'-'unemployed'-'< 1 year' | |
| `personal_status_sex` | int | 'male single'-'female divorced/separated/married'-'male divorced/separated'-'male married/widowed' | |
| `property` | int | 'real estate'-'savings agreement / life insurance'-'unknown / no property'-'car or other' | |
| `foreign_worker` | int | 'yes'-'no' | |
| `credit_risk` | int | 'good_credit'-'bad_credit' | |

## 3. Processus de collecte

Le jeu de données German Credit a été fourni à l'origine par Hans Hofmann. Il vise à distinguer les bons et mauvais risques de crédit. Les variables décrivent notamment la situation financière, l'historique de crédit et certaines caractéristiques personnelles des demandeurs. La documentation publique disponible ne détaille pas la méthode d'échantillonnage, les modalités exactes de collecte, la période de collecte ou les procédures de consentement.

## 4. Préprocessing appliqué (TOI)

> Ce que tu as fait dans ton pipeline. Concis, listé.

- Imputation des manquants (pas de manquant dans le dataset initial, 4% dans le dataset additionnel) : 
    - Variables numériques : imputation par la médiane.
    - Variables ordinales : imputation par la modalité la plus fréquente.
    - Variables catégorielles nominales : imputation par la modalité la plus fréquente.
- Encodage des catégorielles : 
    - Les variables ordinales (checking_account_status, savings_account, employment_since, property, customer_segment) ont été encodées à l'aide d'un encodage ordinal préservant l'ordre sémantique des modalités.
    - Les variables catégorielles nominales (purpose, other_installment_plans, housing, telephone, other_debtors, job, credit_history) ont été transformées par One-Hot Encoding.
- Normalisation des numériques :
    - statardisation avec StandardScaler
- Variables exclues :
    - credit_risk : variable cible
    - foreign_worker : variable sensible présentant un risque de biais pour les modèles

## 5. Usages prévus / à éviter

**Usages prévus** :
- Développement et évaluation de modèles de classification binaire
- Études de biais, d'équité et d'explicabilité des modèles de scoring de crédit

**Usages à éviter** :
- Utilisation directe pour l'octroi ou le refus réel d'un crédit
- Prise de décision automatisée concernant des personnes physiques

## 6. Distribution

- Transmis à : membres de l'équipe projet
- Format : Parquet (compressé)
- Conditions : 
    - Utilisation limitée aux objectifs d'expérimentation ou d'analyse définis dans le projet
    - Redistribution interdite sans validation du responsable du projet
    - Respect des règles de gouvernance des données applicables au projet

## 7. Maintenance

> Qui maintient ce dataset ? Comment signaler un problème ? Quelle version ?

- Mainteneur·euse : équipe projet
- Version : v1.0.0 — <date>
- Contact issue : système de tickets ou gestionnaire d'issues du dépôt Git du projet

---

*Datasheet produite par <Julien D>, <26/06/2026>, dans le cadre du brief M2-B1 ATOS.*