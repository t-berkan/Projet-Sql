# 🚗 Projet clAra Mobility - Système de Gestion SQL

## 📖 Présentation du Projet
Ce dépôt contient l'implémentation complète de la base de données de **clAra Mobility**, une start-up spécialisée dans la location de véhicules électriques (voitures, trottinettes, scooters, vélos). 

L'objectif est de fournir une solution robuste pour centraliser la gestion de la flotte, le suivi des utilisateurs, les infrastructures de recharge et les cycles de maintenance.

---

## 📂 Organisation du Dépôt
Le projet est structuré selon les livrables attendus pour les trois missions :

1.  **Modélisation (Mission 1)** :
    * `MCD.md` : Modèle Conceptuel de Données détaillant les entités et règles de gestion.
    * `MLD.md` : Modèle Logique de Données (3ème Forme Normale) prêt pour PostgreSQL.
2.  **Implémentation (Mission 2)** :
    * `create_tables.sql` : Script de création des tables incluant les contraintes `PRIMARY KEY`, `FOREIGN KEY` et les clauses `CHECK`.
3.  **Exploitation (Mission 3)** :
    * `queries.sql` : Script contenant plus de 10 requêtes d'analyse métier (Jointures, Agrégations, Vues et Triggers).

---

## 🛠️ Installation et Mise en œuvre
Pour déployer le projet sur votre instance PostgreSQL :

1.  **Création du schéma** : Exécutez le script `create_tables.sql`.
2.  **Peuplement** : (Optionnel) Insérez des données de test via votre interface SQL.
3.  **Tests métier** : Exécutez le fichier `queries.sql` pour tester les fonctionnalités avancées (calcul du CA, alertes batterie, triggers de mise à jour automatique).

---

## 📊 Fonctionnalités Avancées implémentées
Pour répondre aux exigences de la Mission 3, le système inclut :
* **Vues d'exploitation** : Pour un accès rapide au planning de maintenance.
* **Calculs financiers** : Agrégation des revenus par type de véhicule et statut de paiement.
* **Automatisation (Triggers)** : Mise à jour automatique du statut des véhicules lors d'une nouvelle location.
* **Analyse de flotte** : Détection automatique des véhicules nécessitant une charge ou une intervention.

---

## 🚀 Technologies
* **SGBD** : PostgreSQL
* **Modélisation** : Merise
* **Environnement** : TablePlus / GitHub

## 👥 Équipe
* **Tekten Berkan / Bernimont Noah / Ouedraogo A.Aziz**
* Promo : B2
* Date du dernier push : Mercredi 7 janvier 2026