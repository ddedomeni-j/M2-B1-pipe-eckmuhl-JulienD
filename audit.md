# Audit Banque Eckmühl — Pipeline scoring crédit conso

> Document à rendre au DPO Klaus Eichmann. **Max 2 pages.**
> Public cible : DPO non-technicien — vocabulaire métier, pas de jargon
> scikit-learn.

## 1. Verdict qualité

1. Déséquilibre de certaines variables
- foreign_worker : la modalité majoritaire représente environ 96 % des observations
- other_debtors : la modalité none représente environ 90 % des observations
- other_installment_plans : la modalité none représente environ 80 % des observations
- credit_history, savings_account ou job ont également une catégorie sur-représentée

2. La variable "personal_status_sex" est une variable composite qui agrège genre et statut marital et qui n'est pas homogène entre les genres. L'influence liée au genre ou au statut marital pourra être difficile à distinguer si on l'utilise

## 2. Verdict éthique

1. Les variables suivantes sont sensibles
- personal_status_sex : contient une information liée au genre
- age : peut être considérée comme sensible selon le contexte
- foreign_worker : peut renvoyer vers la nationalité ou l'origine du client

2. Disparate impact :
- Le calcul du DI donne un ratio de 0.777 pour le groupe foreign_worker = yes, qui est inférieur au seuil d'alerte, ce qui indique un risque de biais pour le calcul du risque de crédit
- Pour la variable age, le groupe des individus âgés de 25 ans ou moins présente un disparate impact de 0.759. Ce résultat indique un risque de biais pour les plus jeunes. L’utilisation de cette variable devra donc être discutée avec le métier pour étudier sa pertinence au regard du modèle à créer.

## 3. Recommandations

1. Ne pas utiliser les variables foreign_worker et personal_status_sex
   → documenter leur usage, tester des modèles avec et sans ces variables, et valider leur conformité avec le métier et les contraintes réglementaires.
2. Prendre en compte les déséquilibres en pondérant les classes
3. Utiliser le one-hot encoding pour les variables non ordonnées
4. Revoir l’utilisation de la variable age qui peut avoir un impact pour le groupe < 25 ans

## 4. Limites de cet audit

- Pas de mitigation des biais à ce stade (cf. modules M7-M8 du parcours).
- Pas de test de robustesse sur dataset d'évaluation séparé (déjà discuté avec Karim).


*Audit produit par <Julien D>, <17/06/2026>, dans le cadre du brief M2-B1 ATOS.