
# OpenClassrooms — Student Analytics & Strategic Decision Support

## Executive Summary

Conception d'une solution analytique de bout en bout
visant à accompagner la direction d'OpenClassrooms
dans la compréhension du profil sociodémographique
de ses étudiants et le pilotage des inscriptions
aux parcours Data.

Le projet combine Analytics Engineering,
Data Quality, Business Intelligence et analyse
stratégique à travers une architecture reposant
sur Snowflake, dbt et Power BI.

L'analyse de 4 647 étudiants entre 2022 et 2025
met en évidence une diminution de 43,9 % des
effectifs sur la période, malgré une reprise
de 11,9 % en 2025 par rapport à 2024.

Ces résultats constituent une base d'aide à la
décision pour orienter les investigations relatives
au recrutement étudiant, au développement régional
et à l'évolution de l'offre de formation.

## Key Business Findings

| Indicateur | Résultat |
|---|---:|
| Étudiants analysés | 4 647 |
| Période étudiée | 2022–2025 |
| Zones régionales représentées | 13 |
| Sources de données | 2 |
| Couches de transformation dbt | 3 |
| Évolution des effectifs 2022–2025 | -43,9 % |
| Évolution des effectifs 2024–2025 | +11,9 % |
| Région dominante | Île-de-France |
| Tranche d'âge dominante | 30–34 ans |
| Genre non renseigné | 26,81 % |

## Business Challenge

OpenClassrooms souhaite mieux comprendre le profil
de ses étudiants afin de disposer d'indicateurs
fiables pour accompagner ses décisions.

Les principaux enjeux métier sont :

- Comprendre l'évolution des inscriptions.
- Identifier les profils étudiants majoritaires.
- Analyser la représentation géographique.
- Identifier les territoires à investiguer.
- Améliorer la qualité des données décisionnelles.
- Centraliser les indicateurs métier.
- Faciliter le pilotage stratégique de l'activité.

## Data Sources

### OpenClassrooms — Students

- 4 647 lignes.
- 6 colonnes.
- Identifiant technique étudiant.
- Catégorie de formation.
- Tranche d'âge.
- Genre.
- Région de résidence.
- Année de début du parcours.

### INSEE — Population régionale

- 21 lignes dans la source initiale.
- 2 colonnes.
- Population régionale de référence : 2025.

L'enrichissement avec les données INSEE permet
de comparer les effectifs étudiants en tenant
compte de la taille démographique des territoires.

## Data Architecture

CSV Sources
    ↓
Snowflake — Cloud Data Warehouse
    ↓
dbt — Staging
    ↓
dbt — Intermediate
    ↓
dbt — Business Data Mart
    ↓
CSV Export
    ↓
Power BI — Strategic Dashboard

Cette architecture sépare les données sources,
les transformations techniques et les indicateurs
destinés aux utilisateurs métier.

## Analytics Engineering

### 1. Data Ingestion

Importation des fichiers sources dans Snowflake.

Centralisation des données étudiants et des
données démographiques de référence.

### 2. Staging Layer

Modèles :

- stg_raw__STUDENTS
- stg_raw_POPULATION_REGION_INSEE

Objectifs :

- Structurer les données sources.
- Préparer les champs nécessaires aux transformations.
- Faciliter la réutilisation des données.
- Séparer les sources des traitements métier.

### 3. Intermediate Layer

Modèle :

- int_students_clean

Objectifs :

- Nettoyer les données étudiants.
- Harmoniser les valeurs.
- Traiter les informations manquantes.
- Préparer les données pour leur enrichissement.
- Contrôler la cohérence des informations.

Les genres non renseignés sont conservés sous
une catégorie explicite NON_RENSEIGNE afin
de ne pas supprimer les observations concernées.

### 4. Business Data Mart

Modèle :

- mart_student_profiles

Création d'un modèle analytique regroupant
les étudiants selon les dimensions suivantes :

- Année de début de parcours.
- Région.
- Tranche d'âge.
- Genre.
- Catégorie de formation.

Enrichissement avec les populations régionales
issues de l'INSEE.

Construction des indicateurs métier :

- Nombre d'étudiants.
- Population régionale.
- Nombre d'étudiants pour 100 000 habitants.

### 5. Data Quality & Validation

Contrôles réalisés dans Snowflake :

- Recherche de valeurs NULL.
- Contrôle des doublons à la granularité du mart.
- Vérification des effectifs.
- Contrôle des populations régionales.
- Vérification de la cohérence du résultat final.

Selon les contrôles documentés dans la soutenance,
le mart final conserve les 4 647 étudiants.

La présence d'une catégorie NON_RENSEIGNE
reste néanmoins un enjeu de complétude métier.

## Business Intelligence Dashboard

Création d'un tableau de bord interactif Power BI
permettant d'explorer les indicateurs selon :

- L'année de début de parcours.
- Le genre.
- La région.

Le dashboard présente :

- Le nombre total d'étudiants.
- La couverture géographique.
- L'évolution annuelle des effectifs.
- La répartition par région.
- La répartition par tranche d'âge.
- La répartition par genre.
- La densité étudiante pour 100 000 habitants.
- Une cartographie des effectifs.

## Strategic Business Findings

### 1. Student Enrolment Trends

| Année | Étudiants | Évolution annuelle |
|---|---:|---:|
| 2022 | 1 696 | — |
| 2023 | 1 150 | -32,2 % |
| 2024 | 850 | -26,1 % |
| 2025 | 951 | +11,9 % |

Les effectifs diminuent de 49,9 % entre
2022 et 2024.

Une reprise est observée en 2025, mais les
effectifs restent inférieurs de 43,9 %
à ceux de 2022.

Business implication :

La direction dispose d'un signal justifiant
une investigation approfondie des facteurs
susceptibles d'influencer les inscriptions.

Les données disponibles ne permettent pas
d'établir les causes de cette évolution.

### 2. Regional Market Analysis

L'Île-de-France constitue le territoire
le plus représenté dans les données étudiants.

La comparaison démographique permet également
d'identifier les différences de représentation
entre les territoires.

| Région | Étudiants pour 100 000 habitants |
|---|---:|
| Île-de-France | 17,0 |
| Provence-Alpes-Côte d'Azur | 5,6 |
| Auvergne-Rhône-Alpes | 5,3 |
| Hauts-de-France | 5,0 |
| Nouvelle-Aquitaine | 4,8 |
| Centre-Val de Loire | 4,7 |
| Occitanie | 4,6 |
| Grand Est | 4,6 |
| Pays de la Loire | 4,0 |
| Bretagne | 4,0 |
| Normandie | 3,4 |
| Bourgogne-Franche-Comté | 2,8 |
| DROM | 2,0 |

Ces indicateurs utilisent les effectifs cumulés
sur la période étudiée et la population de
référence INSEE de 2025.

Business implication :

L'analyse permet de comparer la représentation
géographique et d'identifier les territoires
nécessitant des investigations complémentaires.

Une faible densité ne constitue pas à elle seule
une preuve de potentiel commercial.

### 3. Student Demographic Segmentation

La tranche d'âge 30–34 ans est la plus représentée,
suivie des tranches 25–29 ans et 35–39 ans.

Business implication :

Ces résultats peuvent contribuer à orienter
les études relatives aux profils étudiants,
aux besoins de formation et aux stratégies
de communication.

### 4. Gender Distribution & Data Quality

| Catégorie | Part |
|---|---:|
| Masculin | 50,44 % |
| Féminin | 22,75 % |
| Non renseigné | 26,81 % |

Plus d'un quart des observations ne disposent
pas d'une information de genre renseignée.

Business implication :

Cette proportion constitue un enjeu de qualité
des données et limite l'interprétation de
la répartition par genre.

L'amélioration de la complétude doit respecter
le caractère facultatif de la collecte et
les principes de protection des données.

## Strategic Recommendations

### 1. Student Acquisition Strategy

- Investiguer les causes de la baisse des effectifs.
- Analyser les canaux d'acquisition étudiants.
- Étudier les taux de conversion des candidatures.
- Identifier les évolutions de la demande.
- Approfondir les facteurs associés à la reprise de 2025.

### 2. Regional Development Strategy

- Comparer la représentation régionale.
- Étudier les territoires moins représentés.
- Analyser la demande locale de formation.
- Intégrer les besoins du marché de l'emploi.
- Évaluer la pertinence d'actions régionales ciblées.

### 3. Student Experience & Performance

- Intégrer les taux de réussite.
- Suivre les taux d'abandon.
- Analyser les durées de formation.
- Étudier les indicateurs de satisfaction.
- Construire des indicateurs de performance
  complémentaires par cohorte.

### 4. Data Quality & Governance

- Formaliser les définitions des indicateurs.
- Documenter les transformations.
- Suivre les valeurs non renseignées.
- Renforcer les contrôles de qualité.
- Intégrer les populations INSEE de chaque année.

### 5. Reporting Industrialisation

- Planifier les transformations dbt.
- Mettre en place des tests automatisés.
- Étudier une connexion directe entre
  Snowflake et Power BI.
- Automatiser l'actualisation du reporting.
- Développer un suivi mensuel ou trimestriel.

Ces actions constituent des perspectives
d'amélioration et non des fonctionnalités
déjà déployées dans le cadre du projet.

## Business Value

Cette solution analytique fournit une base
structurée pour accompagner la direction dans :

- Le pilotage des inscriptions étudiantes.
- L'identification des tendances.
- La compréhension des profils étudiants.
- L'analyse de la représentation géographique.
- La fiabilisation des indicateurs métier.
- La préparation des décisions stratégiques.

L'architecture modulaire facilite également
la maintenance et l'évolution des transformations.

Les gains financiers, les économies de temps
et l'amélioration des taux de conversion
n'ont pas été mesurés.

## Analytical Limitations

- Données limitées aux années 2022–2025.
- Population INSEE de référence limitée à 2025.
- Périmètre limité aux parcours Data.
- Genre non renseigné pour 26,81 % des observations.
- Absence de données permettant d'expliquer
  les causes de la baisse des inscriptions.
- Absence d'indicateurs de réussite et d'abandon.
- Comparabilité des périodes annuelles à confirmer.
- Export CSV intermédiaire vers Power BI.

Les résultats constituent une base descriptive
d'aide à la décision et non une analyse causale.

## Technical Stack

- SQL
- Snowflake
- dbt Cloud
- Power BI
- Data Modeling
- Data Transformation
- Data Warehouse
- Data Mart
- Data Quality
- Analytics Engineering
- Business Intelligence
- KPI Monitoring
- Strategic Reporting

## Project Context

Étude de cas professionnalisante réalisée dans
le cadre du projet 8 de la certification
Data Analyst OpenClassrooms.

## Author

Ines Belhoussine

Business Data Analyst | Business Intelligence
& Strategic Decision Support
