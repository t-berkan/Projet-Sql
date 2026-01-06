# Modèle Conceptuel de Données (MCD) - cIAra Mobility

## ENTITÉS

### VEHICULE
- id_vehicule (Identifiant)
- type_vehicule
- marque
- modele
- immatriculation
- statut
- autonomie_km
- niveau_batterie
- date_mise_service

### STATION
- id_station (Identifiant)
- nom_station
- adresse
- ville
- code_postal
- latitude
- longitude
- capacite_totale
- nombre_bornes

### BORNE_RECHARGE
- id_borne (Identifiant)
- numero_borne
- type_prise
- puissance_kw
- statut

### CLIENT
- id_client (Identifiant)
- nom
- prenom
- email
- telephone
- date_inscription
- date_naissance
- numero_permis
- statut_compte

### TECHNICIEN
- id_technicien (Identifiant)
- nom
- prenom
- email
- telephone
- specialite
- date_embauche

### LOCATION
- id_location (Identifiant)
- date_debut
- date_fin_prevue
- date_fin_reelle
- km_parcourus
- statut_location

### PAIEMENT
- id_paiement (Identifiant)
- montant
- date_paiement
- mode_paiement
- statut_paiement

### MAINTENANCE
- id_maintenance (Identifiant)
- date_debut
- date_fin
- type_intervention
- description
- cout
- statut_maintenance

## ASSOCIATIONS

### EFFECTUER (CLIENT → LOCATION)
- Cardinalités: CLIENT (1,n) --- EFFECTUER --- (1,1) LOCATION
- Un client peut effectuer plusieurs locations
- Une location est effectuée par un seul client

### CONCERNER (VEHICULE → LOCATION)
- Cardinalités: VEHICULE (1,n) --- CONCERNER --- (1,1) LOCATION
- Un véhicule peut être concerné par plusieurs locations
- Une location concerne un seul véhicule

### PARTIR_DE (STATION → LOCATION départ)
- Cardinalités: STATION (1,n) --- PARTIR_DE --- (1,1) LOCATION
- Une station peut être le point de départ de plusieurs locations
- Une location part d'une seule station

### RETOURNER_A (STATION → LOCATION retour)
- Cardinalités: STATION (1,n) --- RETOURNER_A --- (0,1) LOCATION
- Une station peut être le point de retour de plusieurs locations
- Une location retourne à une seule station (ou aucune si en cours)

### SE_TROUVER_A (VEHICULE → STATION)
- Cardinalités: VEHICULE (0,n) --- SE_TROUVER_A --- (0,1) STATION
- Un véhicule se trouve dans une station (ou aucune si en location)
- Une station peut accueillir plusieurs véhicules

### REGLER (LOCATION → PAIEMENT)
- Cardinalités: LOCATION (1,1) --- REGLER --- (1,n) PAIEMENT
- Une location peut avoir plusieurs paiements (acompte, solde, etc.)
- Un paiement concerne une seule location

### SUBIR (VEHICULE → MAINTENANCE)
- Cardinalités: VEHICULE (1,n) --- SUBIR --- (1,1) MAINTENANCE
- Un véhicule peut subir plusieurs maintenances
- Une maintenance concerne un seul véhicule

### REALISER (TECHNICIEN → MAINTENANCE)
- Cardinalités: TECHNICIEN (1,n) --- REALISER --- (1,1) MAINTENANCE
- Un technicien peut réaliser plusieurs maintenances
- Une maintenance est réalisée par un seul technicien

### APPARTENIR_A (BORNE_RECHARGE → STATION)
- Cardinalités: BORNE_RECHARGE (1,1) --- APPARTENIR_A --- (1,n) STATION
- Une borne appartient à une seule station
- Une station peut avoir plusieurs bornes

## RÈGLES DE GESTION

1. Un client doit être inscrit pour effectuer une location
2. Un véhicule ne peut être loué que s'il est disponible
3. Une location doit être payée avant la fin de celle-ci
4. Un véhicule en maintenance ne peut pas être loué
5. Une station a une capacité limitée de véhicules
6. Un technicien est spécialisé dans un type d'intervention
7. Le niveau de batterie doit être entre 0 et 100%
8. Une borne ne peut recharger qu'un véhicule à la fois
9. Un client doit avoir un permis de conduire valide pour louer une voiture
10. Le coût de la location dépend de la durée et du type de véhicule
