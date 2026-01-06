# Modèle Logique de Données (MLD) - cIAra Mobility

## Passage du MCD au MLD

Le MLD traduit le MCD en structure relationnelle exploitable par PostgreSQL.

## TABLES AVEC CLÉS

### STATION
```
STATION (
    id_station SERIAL PRIMARY KEY,
    nom_station VARCHAR(100) NOT NULL,
    adresse VARCHAR(200) NOT NULL,
    ville VARCHAR(50) NOT NULL,
    code_postal VARCHAR(10) NOT NULL,
    latitude DECIMAL(10,8) NOT NULL,
    longitude DECIMAL(11,8) NOT NULL,
    capacite_totale INTEGER NOT NULL CHECK (capacite_totale > 0),
    nombre_bornes INTEGER NOT NULL DEFAULT 0
)
```

### VEHICULE
```
VEHICULE (
    id_vehicule SERIAL PRIMARY KEY,
    type_vehicule VARCHAR(20) NOT NULL CHECK (type_vehicule IN ('voiture', 'trottinette', 'scooter', 'velo')),
    marque VARCHAR(50) NOT NULL,
    modele VARCHAR(50) NOT NULL,
    immatriculation VARCHAR(20) UNIQUE NOT NULL,
    statut VARCHAR(20) NOT NULL CHECK (statut IN ('disponible', 'en_location', 'en_maintenance', 'en_charge')),
    autonomie_km INTEGER NOT NULL CHECK (autonomie_km > 0),
    niveau_batterie INTEGER NOT NULL CHECK (niveau_batterie >= 0 AND niveau_batterie <= 100),
    date_mise_service DATE NOT NULL,
    id_station_actuelle INTEGER,
    FOREIGN KEY (id_station_actuelle) REFERENCES STATION(id_station)
)
```

### BORNE_RECHARGE
```
BORNE_RECHARGE (
    id_borne SERIAL PRIMARY KEY,
    id_station INTEGER NOT NULL,
    numero_borne VARCHAR(10) NOT NULL,
    type_prise VARCHAR(30) NOT NULL,
    puissance_kw DECIMAL(5,2) NOT NULL CHECK (puissance_kw > 0),
    statut VARCHAR(20) NOT NULL CHECK (statut IN ('disponible', 'occupee', 'hors_service')),
    FOREIGN KEY (id_station) REFERENCES STATION(id_station) ON DELETE CASCADE
)
```

### CLIENT
```
CLIENT (
    id_client SERIAL PRIMARY KEY,
    nom VARCHAR(50) NOT NULL,
    prenom VARCHAR(50) NOT NULL,
    email VARCHAR(100) UNIQUE NOT NULL,
    telephone VARCHAR(20) NOT NULL,
    date_inscription DATE NOT NULL DEFAULT CURRENT_DATE,
    date_naissance DATE NOT NULL,
    numero_permis VARCHAR(30) UNIQUE,
    statut_compte VARCHAR(20) NOT NULL CHECK (statut_compte IN ('actif', 'suspendu', 'inactif'))
)
```

### TECHNICIEN
```
TECHNICIEN (
    id_technicien SERIAL PRIMARY KEY,
    nom VARCHAR(50) NOT NULL,
    prenom VARCHAR(50) NOT NULL,
    email VARCHAR(100) UNIQUE NOT NULL,
    telephone VARCHAR(20) NOT NULL,
    specialite VARCHAR(50) NOT NULL,
    date_embauche DATE NOT NULL
)
```

### LOCATION
```
LOCATION (
    id_location SERIAL PRIMARY KEY,
    id_client INTEGER NOT NULL,
    id_vehicule INTEGER NOT NULL,
    id_station_depart INTEGER NOT NULL,
    id_station_retour INTEGER,
    date_debut TIMESTAMP NOT NULL,
    date_fin_prevue TIMESTAMP NOT NULL,
    date_fin_reelle TIMESTAMP,
    km_parcourus DECIMAL(8,2) CHECK (km_parcourus >= 0),
    statut_location VARCHAR(20) NOT NULL CHECK (statut_location IN ('en_cours', 'terminee', 'annulee')),
    FOREIGN KEY (id_client) REFERENCES CLIENT(id_client),
    FOREIGN KEY (id_vehicule) REFERENCES VEHICULE(id_vehicule),
    FOREIGN KEY (id_station_depart) REFERENCES STATION(id_station),
    FOREIGN KEY (id_station_retour) REFERENCES STATION(id_station),
    CHECK (date_fin_prevue > date_debut)
)
```

### PAIEMENT
```
PAIEMENT (
    id_paiement SERIAL PRIMARY KEY,
    id_location INTEGER NOT NULL,
    montant DECIMAL(10,2) NOT NULL CHECK (montant > 0),
    date_paiement TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP,
    mode_paiement VARCHAR(30) NOT NULL CHECK (mode_paiement IN ('carte_bancaire', 'paypal', 'apple_pay', 'google_pay')),
    statut_paiement VARCHAR(20) NOT NULL CHECK (statut_paiement IN ('en_attente', 'valide', 'refuse', 'rembourse')),
    FOREIGN KEY (id_location) REFERENCES LOCATION(id_location) ON DELETE CASCADE
)
```

### MAINTENANCE
```
MAINTENANCE (
    id_maintenance SERIAL PRIMARY KEY,
    id_vehicule INTEGER NOT NULL,
    id_technicien INTEGER NOT NULL,
    date_debut TIMESTAMP NOT NULL,
    date_fin TIMESTAMP,
    type_intervention VARCHAR(50) NOT NULL,
    description TEXT NOT NULL,
    cout DECIMAL(10,2) CHECK (cout >= 0),
    statut_maintenance VARCHAR(20) NOT NULL CHECK (statut_maintenance IN ('en_cours', 'terminee', 'en_attente_pieces')),
    FOREIGN KEY (id_vehicule) REFERENCES VEHICULE(id_vehicule),
    FOREIGN KEY (id_technicien) REFERENCES TECHNICIEN(id_technicien),
    CHECK (date_fin IS NULL OR date_fin >= date_debut)
)
```

## DÉPENDANCES FONCTIONNELLES

### Table VEHICULE
- id_vehicule → type_vehicule, marque, modele, immatriculation, statut, autonomie_km, niveau_batterie, date_mise_service, id_station_actuelle

### Table STATION
- id_station → nom_station, adresse, ville, code_postal, latitude, longitude, capacite_totale, nombre_bornes

### Table CLIENT
- id_client → nom, prenom, email, telephone, date_inscription, date_naissance, numero_permis, statut_compte
- email → id_client (UNIQUE)

### Table LOCATION
- id_location → id_client, id_vehicule, id_station_depart, id_station_retour, date_debut, date_fin_prevue, date_fin_reelle, km_parcourus, statut_location

### Table PAIEMENT
- id_paiement → id_location, montant, date_paiement, mode_paiement, statut_paiement

### Table MAINTENANCE
- id_maintenance → id_vehicule, id_technicien, date_debut, date_fin, type_intervention, description, cout, statut_maintenance

## NORMALISATION

Toutes les tables sont en 3ème Forme Normale (3FN):
- 1FN: Tous les attributs sont atomiques
- 2FN: Pas de dépendance partielle (toutes les clés primaires sont simples)
- 3FN: Pas de dépendance transitive

## INDEX RECOMMANDÉS

```sql
CREATE INDEX idx_vehicule_statut ON VEHICULE(statut);
CREATE INDEX idx_vehicule_station ON VEHICULE(id_station_actuelle);
CREATE INDEX idx_location_client ON LOCATION(id_client);
CREATE INDEX idx_location_vehicule ON LOCATION(id_vehicule);
CREATE INDEX idx_location_dates ON LOCATION(date_debut, date_fin_reelle);
CREATE INDEX idx_paiement_location ON PAIEMENT(id_location);
CREATE INDEX idx_maintenance_vehicule ON MAINTENANCE(id_vehicule);
CREATE INDEX idx_maintenance_statut ON MAINTENANCE(statut_maintenance);
```
