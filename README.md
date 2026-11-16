# Trois tâches de NLP : classification, séquences et traduction

Ce dépôt contient trois notebooks **autonomes et exécutés** en Python. Les données sont intégrées aux notebooks : aucun téléchargement, compte externe ou GPU n'est nécessaire. Leur but est de montrer un protocole complet et lisible, avec séparation apprentissage/test, référence simple, métriques et analyse des limites. Les corpus sont petits et pédagogiques ; les scores ne représentent pas la performance sur un jeu de données réel.

## Correspondance avec les objectifs

| Tâche | Notebook | Problème et méthode | Évaluation enregistrée |
| --- | --- | --- | --- |
| Classification | [01_classification.ipynb](01_classification.ipynb) | Classer des requêtes françaises en `meteo`, `transport` et `restaurant` avec TF-IDF caractère et régression logistique | Accuracy **0,714**, F1 macro **0,715** ; matrice de confusion et rapport par classe |
| Séquences | [02_sequences.ipynb](02_sequences.ipynb) | Étiqueter les jetons d'une phrase en BIO (`PER`, `ORG`, `LOC`) à l'aide du contexte local | Accuracy jeton **0,957**, F1 entité exacte **0,926** ; rapport par étiquette |
| Traduction | [03_traduction.ipynb](03_traduction.ipynb) | Apprendre un lexique aligné français → anglais et traduire des combinaisons nouvelles dans une grammaire contrôlée | Exact match **1,000**, BLEU-4 **100,00** sur le test contrôlé ; **0,000** et **0,00** sur un défi hors grammaire |

Les trois notebooks comprennent une introduction, les données et leurs contrôles, l'entraînement, les métriques, des exemples de prédictions et une section consacrée aux limites.

## Exécution

Prérequis : Python 3.10 ou plus récent, Jupyter et scikit-learn. Dans un environnement virtuel :

```bash
python -m pip install -r requirements.txt
jupyter notebook
```

Ouvrir ensuite les notebooks dans l'ordre numérique et choisir **Run All**. Les sorties déjà enregistrées permettent aussi de lire les résultats sans les relancer. `random_state=42` fixe les séparations et les modèles stochastiques. Les résultats ont été obtenus avec Python 3.13 et scikit-learn dans l'environnement local ; de légères variations restent possibles avec d'autres versions.

## Lecture des métriques

- **Classification :** le F1 macro donne le même poids aux trois intentions. Une référence qui prédit toujours la classe majoritaire obtient un F1 macro de **0,148**. Le test contient 14 phrases ; une seule erreur change donc sensiblement le score.
- **Séquences :** l'accuracy jeton compte aussi les nombreux jetons `O`. Le F1 entité compare des segments complets avec leur type, ce qui le rend plus informatif pour la reconnaissance d'entités. Une référence « tout `O` » obtient un F1 entité de **0**.
- **Traduction :** l'exact match exige l'égalité de la phrase entière. BLEU-4 mesure le recouvrement des n-grammes sur le corpus avec pénalité de longueur. Le score parfait sur 24 phrases de test vient de la grammaire artificielle et de l'alignement mot à mot fourni par construction. Les trois exemples hors grammaire échouent ; ce traducteur ne doit pas être interprété comme un modèle de traduction générale.

## Limites et prolongements

Les jeux de données sont rédigés ou générés pour la démonstration. Ils ne couvrent ni les variantes naturelles, ni les entités de plusieurs mots, ni les changements d'ordre de mots entre langues. Pour une étude plus robuste, utiliser des corpus externes indépendants, séparer les exemples par source ou document, conserver un test final intact et analyser les erreurs par catégorie. Pour la traduction réelle, employer un corpus parallèle adapté et compléter les scores automatiques par une évaluation humaine.
