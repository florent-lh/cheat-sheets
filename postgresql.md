# PostgreSQL CheatSheet 

## 📋 Table des matières
- Connexion et administration
- Gestion des bases de données
- Gestion des tables
- Types de données
- Requêtes de base (CRUD)
- Jointures
- Fonctions d'agrégation
- Sous-requêtes et CTE
- Index et optimisation
- Transactions
- Contraintes
- Vues et vues matérialisées
- Fonctions et procédures
- Triggers
- JSON et JSONB
- Full-Text Search
- Partitionnement
- Sécurité et permissions
- Nouveautés récentes

---

## 🔌 Connexion et Administration

### Connexion
```bash
# Connexion standard
psql -U username -d database_name -h hostname -p port

# Connexion locale
psql -U postgres

# Connexion avec URL
psql postgresql://user:password@host:port/database
```

### Commandes psql
```sql
\l                    -- Liste des bases de données
\c database_name      -- Se connecter à une base
\dt                   -- Liste des tables
\d table_name         -- Décrire une table
\du                   -- Liste des utilisateurs/rôles
\dn                   -- Liste des schémas
\df                   -- Liste des fonctions
\dv                   -- Liste des vues
\di                   -- Liste des index
\x                    -- Affichage étendu (toggle)
\timing               -- Afficher le temps d'exécution
\q                    -- Quitter
\! clear              -- Effacer l'écran
\e                    -- Ouvrir l'éditeur
\i fichier.sql        -- Exécuter un fichier SQL
```

---

## 💾 Gestion des Bases de Données

### Créer/Supprimer
```sql
-- Créer une base
CREATE DATABASE ma_base
  ENCODING 'UTF8'
  LC_COLLATE 'fr_FR.UTF-8'
  LC_CTYPE 'fr_FR.UTF-8'
  TEMPLATE template0;

-- Supprimer une base
DROP DATABASE ma_base;
DROP DATABASE IF EXISTS ma_base;

-- Dupliquer une base
CREATE DATABASE nouvelle_base WITH TEMPLATE ancienne_base;
```

### Schémas
```sql
-- Créer un schéma
CREATE SCHEMA mon_schema;

-- Utiliser un schéma
SET search_path TO mon_schema, public;

-- Supprimer un schéma
DROP SCHEMA mon_schema CASCADE;
```

---

## 📊 Gestion des Tables

### Créer une table
```sql
CREATE TABLE utilisateurs (
    id SERIAL PRIMARY KEY,
    nom VARCHAR(100) NOT NULL,
    email VARCHAR(255) UNIQUE NOT NULL,
    age INTEGER CHECK (age >= 18),
    date_creation TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    actif BOOLEAN DEFAULT true,
    metadata JSONB
);

-- Table temporaire
CREATE TEMP TABLE temp_data (id INT, value TEXT);

-- Créer à partir d'une requête
CREATE TABLE nouvelle_table AS
SELECT * FROM ancienne_table WHERE condition;
```

### Modifier une table
```sql
-- Ajouter une colonne
ALTER TABLE utilisateurs ADD COLUMN telephone VARCHAR(20);

-- Supprimer une colonne
ALTER TABLE utilisateurs DROP COLUMN telephone;

-- Modifier une colonne
ALTER TABLE utilisateurs ALTER COLUMN nom TYPE VARCHAR(150);
ALTER TABLE utilisateurs ALTER COLUMN age SET NOT NULL;
ALTER TABLE utilisateurs ALTER COLUMN actif SET DEFAULT false;

-- Renommer
ALTER TABLE utilisateurs RENAME TO users;
ALTER TABLE users RENAME COLUMN nom TO name;
```

### Supprimer
```sql
DROP TABLE utilisateurs;
DROP TABLE IF EXISTS utilisateurs CASCADE;
TRUNCATE TABLE utilisateurs;  -- Vider la table rapidement
TRUNCATE TABLE utilisateurs RESTART IDENTITY CASCADE;
```

---

## 🔤 Types de Données

### Types numériques
```sql
SMALLINT              -- Entier 2 octets (-32768 à 32767)
INTEGER / INT         -- Entier 4 octets
BIGINT               -- Entier 8 octets
SERIAL               -- Auto-incrémenté (INTEGER)
BIGSERIAL            -- Auto-incrémenté (BIGINT)
DECIMAL(p,s)         -- Nombre exact
NUMERIC(p,s)         -- Nombre exact (identique à DECIMAL)
REAL                 -- Flottant 4 octets
DOUBLE PRECISION     -- Flottant 8 octets
```

### Types texte
```sql
CHAR(n)              -- Longueur fixe
VARCHAR(n)           -- Longueur variable
TEXT                 -- Texte illimité
```

### Types date/heure
```sql
DATE                 -- Date seulement
TIME                 -- Heure seulement
TIMESTAMP            -- Date et heure
TIMESTAMPTZ          -- Date et heure avec timezone
INTERVAL             -- Intervalle de temps
```

### Types spéciaux
```sql
BOOLEAN              -- true/false
UUID                 -- Identifiant unique universel
JSON                 -- JSON texte
JSONB                -- JSON binaire (performant)
ARRAY                -- Tableau
HSTORE               -- Paires clé-valeur
ENUM                 -- Type énuméré
BYTEA                -- Données binaires
INET                 -- Adresse IP
CIDR                 -- Réseau IP
MACADDR              -- Adresse MAC
```

### Créer un type personnalisé
```sql
-- Type ENUM
CREATE TYPE statut_commande AS ENUM ('en_attente', 'validée', 'expédiée', 'livrée');

-- Type composite
CREATE TYPE adresse AS (
    rue TEXT,
    ville TEXT,
    code_postal VARCHAR(10)
);
```

---

## ✏️ Requêtes de Base (CRUD)

### INSERT
```sql
-- Insertion simple
INSERT INTO utilisateurs (nom, email, age)
VALUES ('Dupont', 'dupont@mail.com', 30);

-- Insertions multiples
INSERT INTO utilisateurs (nom, email, age) VALUES
    ('Martin', 'martin@mail.com', 25),
    ('Bernard', 'bernard@mail.com', 35),
    ('Durand', 'durand@mail.com', 28);

-- Insertion avec RETURNING
INSERT INTO utilisateurs (nom, email)
VALUES ('Petit', 'petit@mail.com')
RETURNING id, date_creation;

-- Insertion depuis SELECT
INSERT INTO archive_users
SELECT * FROM utilisateurs WHERE actif = false;

-- Upsert (INSERT ... ON CONFLICT)
INSERT INTO utilisateurs (id, nom, email)
VALUES (1, 'Nouveau', 'nouveau@mail.com')
ON CONFLICT (id) DO UPDATE
SET nom = EXCLUDED.nom, email = EXCLUDED.email;

-- Ignorer les conflits
INSERT INTO utilisateurs (email, nom)
VALUES ('existe@mail.com', 'Test')
ON CONFLICT (email) DO NOTHING;
```

### SELECT
```sql
-- Sélection basique
SELECT * FROM utilisateurs;
SELECT nom, email FROM utilisateurs;
SELECT DISTINCT ville FROM utilisateurs;

-- Avec conditions
SELECT * FROM utilisateurs
WHERE age > 25 AND actif = true;

-- Opérateurs
WHERE age BETWEEN 20 AND 30;
WHERE nom IN ('Dupont', 'Martin');
WHERE email LIKE '%@gmail.com';
WHERE email ILIKE '%GMAIL%';  -- Insensible à la casse
WHERE nom ~ '^[A-M]';  -- Regex
WHERE description IS NULL;

-- Tri
ORDER BY age DESC, nom ASC;
ORDER BY age DESC NULLS LAST;

-- Limitation
LIMIT 10 OFFSET 20;

-- Groupement
SELECT ville, COUNT(*) as total
FROM utilisateurs
GROUP BY ville
HAVING COUNT(*) > 5;
```

### UPDATE
```sql
-- Mise à jour simple
UPDATE utilisateurs
SET age = 31, email = 'nouveau@mail.com'
WHERE id = 1;

-- Mise à jour avec calcul
UPDATE produits
SET prix = prix * 1.1
WHERE categorie = 'electronique';

-- Mise à jour depuis une autre table
UPDATE commandes c
SET statut = 'expédiée'
FROM expeditions e
WHERE c.id = e.commande_id AND e.date_envoi IS NOT NULL;

-- Avec RETURNING
UPDATE utilisateurs
SET actif = false
WHERE last_login < NOW() - INTERVAL '1 year'
RETURNING id, nom;
```

### DELETE
```sql
-- Suppression simple
DELETE FROM utilisateurs WHERE id = 1;

-- Suppression avec condition
DELETE FROM logs WHERE date < NOW() - INTERVAL '30 days';

-- Suppression avec RETURNING
DELETE FROM utilisateurs
WHERE actif = false
RETURNING *;

-- Suppression depuis une autre table
DELETE FROM commandes
WHERE client_id IN (
    SELECT id FROM clients WHERE actif = false
);
```

---

## 🔗 Jointures

```sql
-- INNER JOIN (seulement les correspondances)
SELECT u.nom, c.numero
FROM utilisateurs u
INNER JOIN commandes c ON u.id = c.user_id;

-- LEFT JOIN (tous les enregistrements de gauche)
SELECT u.nom, c.numero
FROM utilisateurs u
LEFT JOIN commandes c ON u.id = c.user_id;

-- RIGHT JOIN (tous les enregistrements de droite)
SELECT u.nom, c.numero
FROM utilisateurs u
RIGHT JOIN commandes c ON u.id = c.user_id;

-- FULL OUTER JOIN (tous les enregistrements)
SELECT u.nom, c.numero
FROM utilisateurs u
FULL OUTER JOIN commandes c ON u.id = c.user_id;

-- CROSS JOIN (produit cartésien)
SELECT * FROM couleurs CROSS JOIN tailles;

-- Self JOIN
SELECT e.nom as employe, m.nom as manager
FROM employes e
LEFT JOIN employes m ON e.manager_id = m.id;

-- Jointures multiples
SELECT u.nom, c.numero, p.nom_produit
FROM utilisateurs u
JOIN commandes c ON u.id = c.user_id
JOIN produits p ON c.produit_id = p.id;
```

---

## 📊 Fonctions d'Agrégation

```sql
-- Fonctions de base
SELECT COUNT(*) FROM utilisateurs;
SELECT COUNT(DISTINCT ville) FROM utilisateurs;
SELECT SUM(montant) FROM commandes;
SELECT AVG(age) FROM utilisateurs;
SELECT MIN(prix), MAX(prix) FROM produits;

-- Agrégations par groupe
SELECT ville, COUNT(*) as nb_users, AVG(age) as age_moyen
FROM utilisateurs
GROUP BY ville;

-- FILTER (nouveauté)
SELECT
    COUNT(*) FILTER (WHERE age < 30) as jeunes,
    COUNT(*) FILTER (WHERE age >= 30) as seniors
FROM utilisateurs;

-- Fonctions de fenêtrage (Window Functions)
SELECT nom, salaire,
    AVG(salaire) OVER () as salaire_moyen,
    RANK() OVER (ORDER BY salaire DESC) as rang,
    ROW_NUMBER() OVER (PARTITION BY departement ORDER BY salaire DESC) as rang_dept
FROM employes;

-- String aggregation
SELECT departement,
    STRING_AGG(nom, ', ' ORDER BY nom) as employes
FROM employes
GROUP BY departement;

-- Array aggregation
SELECT departement,
    ARRAY_AGG(nom ORDER BY nom) as employes
FROM employes
GROUP BY departement;
```

---

## 🎯 Sous-requêtes et CTE

### Sous-requêtes
```sql
-- Sous-requête dans WHERE
SELECT nom FROM utilisateurs
WHERE id IN (SELECT user_id FROM commandes WHERE montant > 1000);

-- Sous-requête dans FROM
SELECT dept, avg_salaire
FROM (
    SELECT departement as dept, AVG(salaire) as avg_salaire
    FROM employes
    GROUP BY departement
) sub
WHERE avg_salaire > 50000;

-- Sous-requête corrélée
SELECT nom, salaire
FROM employes e1
WHERE salaire > (
    SELECT AVG(salaire)
    FROM employes e2
    WHERE e1.departement = e2.departement
);
```

### CTE (Common Table Expressions)
```sql
-- CTE simple
WITH users_actifs AS (
    SELECT * FROM utilisateurs WHERE actif = true
)
SELECT * FROM users_actifs WHERE age > 25;

-- CTE multiples
WITH 
    ventes_2024 AS (
        SELECT * FROM ventes WHERE EXTRACT(YEAR FROM date) = 2024
    ),
    stats AS (
        SELECT produit_id, SUM(montant) as total
        FROM ventes_2024
        GROUP BY produit_id
    )
SELECT p.nom, s.total
FROM produits p
JOIN stats s ON p.id = s.produit_id;

-- CTE récursive (exemple: hiérarchie)
WITH RECURSIVE subordinates AS (
    SELECT id, nom, manager_id
    FROM employes
    WHERE id = 1  -- Point de départ
    
    UNION ALL
    
    SELECT e.id, e.nom, e.manager_id
    FROM employes e
    INNER JOIN subordinates s ON e.manager_id = s.id
)
SELECT * FROM subordinates;
```

---

## ⚡ Index et Optimisation

### Types d'index
```sql
-- Index B-tree (par défaut)
CREATE INDEX idx_nom ON utilisateurs(nom);

-- Index unique
CREATE UNIQUE INDEX idx_email ON utilisateurs(email);

-- Index composite
CREATE INDEX idx_nom_age ON utilisateurs(nom, age);

-- Index partiel
CREATE INDEX idx_actifs ON utilisateurs(email)
WHERE actif = true;

-- Index sur expression
CREATE INDEX idx_lower_email ON utilisateurs(LOWER(email));

-- Index GIN (pour JSONB, arrays, full-text)
CREATE INDEX idx_metadata ON utilisateurs USING GIN(metadata);

-- Index GiST (pour géométrie, plages)
CREATE INDEX idx_plage ON reservations USING GIST(periode_reservation);

-- Index BRIN (pour grosses tables triées)
CREATE INDEX idx_date_brin ON logs USING BRIN(date_creation);

-- Index Hash
CREATE INDEX idx_hash ON utilisateurs USING HASH(id);
```

### Gestion des index
```sql
-- Supprimer un index
DROP INDEX idx_nom;

-- Reconstruire un index
REINDEX INDEX idx_nom;
REINDEX TABLE utilisateurs;

-- Index concurrent (sans bloquer la table)
CREATE INDEX CONCURRENTLY idx_nom ON utilisateurs(nom);
```

### Analyse et optimisation
```sql
-- Analyser une requête
EXPLAIN SELECT * FROM utilisateurs WHERE age > 25;
EXPLAIN ANALYZE SELECT * FROM utilisateurs WHERE age > 25;

-- Mettre à jour les statistiques
ANALYZE utilisateurs;
VACUUM ANALYZE utilisateurs;

-- Voir les index inutilisés
SELECT schemaname, tablename, indexname
FROM pg_stat_user_indexes
WHERE idx_scan = 0;
```

---

## 🔄 Transactions

```sql
-- Transaction de base
BEGIN;
    UPDATE comptes SET solde = solde - 100 WHERE id = 1;
    UPDATE comptes SET solde = solde + 100 WHERE id = 2;
COMMIT;

-- Annulation
BEGIN;
    DELETE FROM utilisateurs WHERE id = 1;
ROLLBACK;

-- Points de sauvegarde
BEGIN;
    UPDATE produits SET prix = prix * 1.1;
    SAVEPOINT prix_augmentes;
    
    DELETE FROM produits WHERE stock = 0;
    ROLLBACK TO prix_augmentes;  -- Annule seulement le DELETE
COMMIT;

-- Niveaux d'isolation
BEGIN TRANSACTION ISOLATION LEVEL SERIALIZABLE;
BEGIN TRANSACTION ISOLATION LEVEL REPEATABLE READ;
BEGIN TRANSACTION ISOLATION LEVEL READ COMMITTED;  -- Par défaut
BEGIN TRANSACTION ISOLATION LEVEL READ UNCOMMITTED;
```

---

## 🔒 Contraintes

```sql
-- PRIMARY KEY
ALTER TABLE utilisateurs ADD PRIMARY KEY (id);

-- FOREIGN KEY
ALTER TABLE commandes
ADD CONSTRAINT fk_user
FOREIGN KEY (user_id) REFERENCES utilisateurs(id)
ON DELETE CASCADE
ON UPDATE CASCADE;

-- UNIQUE
ALTER TABLE utilisateurs ADD CONSTRAINT unique_email UNIQUE (email);

-- CHECK
ALTER TABLE produits
ADD CONSTRAINT check_prix CHECK (prix > 0);

ALTER TABLE employes
ADD CONSTRAINT check_salaire CHECK (salaire BETWEEN 20000 AND 200000);

-- NOT NULL
ALTER TABLE utilisateurs ALTER COLUMN email SET NOT NULL;

-- EXCLUSION (nouveauté - éviter les chevauchements)
ALTER TABLE reservations
ADD CONSTRAINT no_overlap
EXCLUDE USING GIST (salle WITH =, periode WITH &&);

-- Désactiver/activer temporairement
ALTER TABLE commandes DISABLE TRIGGER ALL;
ALTER TABLE commandes ENABLE TRIGGER ALL;
```

---

## 👁️ Vues et Vues Matérialisées

### Vues
```sql
-- Créer une vue
CREATE VIEW vue_users_actifs AS
SELECT id, nom, email, age
FROM utilisateurs
WHERE actif = true;

-- Utiliser une vue
SELECT * FROM vue_users_actifs;

-- Vue avec option
CREATE OR REPLACE VIEW vue_commandes AS
SELECT u.nom, c.numero, c.montant
FROM utilisateurs u
JOIN commandes c ON u.id = c.user_id;

-- Supprimer une vue
DROP VIEW vue_users_actifs;
```

### Vues matérialisées
```sql
-- Créer une vue matérialisée
CREATE MATERIALIZED VIEW stats_mensuelles AS
SELECT 
    DATE_TRUNC('month', date_commande) as mois,
    COUNT(*) as nb_commandes,
    SUM(montant) as total
FROM commandes
GROUP BY DATE_TRUNC('month', date_commande);

-- Rafraîchir
REFRESH MATERIALIZED VIEW stats_mensuelles;

-- Rafraîchir sans bloquer les lectures
REFRESH MATERIALIZED VIEW CONCURRENTLY stats_mensuelles;

-- Avec index
CREATE UNIQUE INDEX ON stats_mensuelles(mois);
```

---

## ⚙️ Fonctions et Procédures

### Fonctions
```sql
-- Fonction simple
CREATE OR REPLACE FUNCTION calculer_tva(montant NUMERIC)
RETURNS NUMERIC AS $$
BEGIN
    RETURN montant * 0.20;
END;
$$ LANGUAGE plpgsql;

-- Utilisation
SELECT calculer_tva(100);

-- Fonction avec table en retour
CREATE OR REPLACE FUNCTION get_commandes_utilisateur(user_id_param INT)
RETURNS TABLE(id INT, numero VARCHAR, montant NUMERIC) AS $$
BEGIN
    RETURN QUERY
    SELECT c.id, c.numero, c.montant
    FROM commandes c
    WHERE c.user_id = user_id_param;
END;
$$ LANGUAGE plpgsql;

-- Fonction avec logique complexe
CREATE OR REPLACE FUNCTION calculer_remise(montant NUMERIC)
RETURNS NUMERIC AS $$
DECLARE
    remise NUMERIC;
BEGIN
    IF montant > 1000 THEN
        remise := 0.15;
    ELSIF montant > 500 THEN
        remise := 0.10;
    ELSE
        remise := 0.05;
    END IF;
    
    RETURN montant * (1 - remise);
END;
$$ LANGUAGE plpgsql;
```

### Procédures (depuis PostgreSQL 11)
```sql
CREATE OR REPLACE PROCEDURE archiver_anciennes_commandes()
LANGUAGE plpgsql AS $$
BEGIN
    INSERT INTO commandes_archive
    SELECT * FROM commandes
    WHERE date_commande < NOW() - INTERVAL '1 year';
    
    DELETE FROM commandes
    WHERE date_commande < NOW() - INTERVAL '1 year';
    
    COMMIT;
END;
$$;

-- Appel
CALL archiver_anciennes_commandes();
```

---

## 🎬 Triggers

```sql
-- Fonction trigger pour audit
CREATE OR REPLACE FUNCTION audit_modification()
RETURNS TRIGGER AS $$
BEGIN
    INSERT INTO audit_log (table_name, operation, user_name, timestamp)
    VALUES (TG_TABLE_NAME, TG_OP, current_user, NOW());
    RETURN NEW;
END;
$$ LANGUAGE plpgsql;

-- Créer un trigger
CREATE TRIGGER trigger_audit
AFTER INSERT OR UPDATE OR DELETE ON utilisateurs
FOR EACH ROW
EXECUTE FUNCTION audit_modification();

-- Trigger BEFORE pour validation
CREATE OR REPLACE FUNCTION valider_email()
RETURNS TRIGGER AS $$
BEGIN
    IF NEW.email !~ '^[A-Za-z0-9._%+-]+@[A-Za-z0-9.-]+\.[A-Z|a-z]{2,}$' THEN
        RAISE EXCEPTION 'Email invalide: %', NEW.email;
    END IF;
    RETURN NEW;
END;
$$ LANGUAGE plpgsql;

CREATE TRIGGER check_email
BEFORE INSERT OR UPDATE ON utilisateurs
FOR EACH ROW
EXECUTE FUNCTION valider_email();

-- Désactiver/activer un trigger
ALTER TABLE utilisateurs DISABLE TRIGGER trigger_audit;
ALTER TABLE utilisateurs ENABLE TRIGGER trigger_audit;

-- Supprimer un trigger
DROP TRIGGER trigger_audit ON utilisateurs;
```

---

## 📦 JSON et JSONB

```sql
-- Insertion JSON
INSERT INTO produits (nom, details)
VALUES ('Laptop', '{"marque": "Dell", "ram": "16GB", "prix": 899}');

-- Extraction de données
SELECT details->>'marque' as marque FROM produits;
SELECT details->'prix' as prix FROM produits;  -- Retourne JSON
SELECT details->>'prix' as prix FROM produits; -- Retourne text

-- Navigation profonde
SELECT metadata->'user'->'address'->>'city' FROM orders;
SELECT metadata#>'{user,address,city}' FROM orders;

-- Vérifications
SELECT * FROM produits WHERE details ? 'marque';  -- Clé existe
SELECT * FROM produits WHERE details ?| array['ram', 'cpu'];  -- Au moins une clé
SELECT * FROM produits WHERE details ?& array['ram', 'cpu'];  -- Toutes les clés

-- Recherche dans JSON
SELECT * FROM produits WHERE details @> '{"marque": "Dell"}';
SELECT * FROM produits WHERE details->>'prix' = '899';

-- Modification JSON
UPDATE produits
SET details = details || '{"stock": 50}'
WHERE id = 1;

UPDATE produits
SET details = jsonb_set(details, '{prix}', '799')
WHERE id = 1;

-- Supprimer une clé
UPDATE produits
SET details = details - 'ancien_champ'
WHERE id = 1;

-- Fonctions JSON utiles
SELECT jsonb_pretty(details) FROM produits;
SELECT jsonb_array_elements(tags) FROM produits;
SELECT jsonb_object_keys(details) FROM produits;

-- Index sur JSON
CREATE INDEX idx_details ON produits USING GIN(details);
CREATE INDEX idx_marque ON produits((details->>'marque'));
```

---

## 🔍 Full-Text Search

```sql
-- Créer une colonne tsvector
ALTER TABLE articles ADD COLUMN texte_search tsvector;

-- Mettre à jour le vecteur de recherche
UPDATE articles
SET texte_search = to_tsvector('french', titre || ' ' || contenu);

-- Recherche full-text
SELECT titre, ts_rank(texte_search, query) as rang
FROM articles, to_tsquery('french', 'postgresql & performance') query
WHERE texte_search @@ query
ORDER BY rang DESC;

-- Index pour full-text
CREATE INDEX idx_fts ON articles USING GIN(texte_search);

-- Trigger pour mise à jour automatique
CREATE TRIGGER tsvector_update
BEFORE INSERT OR UPDATE ON articles
FOR EACH ROW
EXECUTE FUNCTION
tsvector_update_trigger(texte_search, 'pg_catalog.french', titre, contenu);

-- Recherche avec variations
SELECT * FROM articles
WHERE texte_search @@ to_tsquery('french', 'base:* & donné:*');

-- Headline (extrait avec mise en évidence)
SELECT ts_headline('french', contenu, to_tsquery('postgresql'))
FROM articles;
```

---

## 📂 Partitionnement

### Partitionnement par plage
```sql
-- Table parent
CREATE TABLE logs (
    id BIGSERIAL,
    message TEXT,
    created_at TIMESTAMP NOT NULL
) PARTITION BY RANGE (created_at);

-- Partitions
CREATE TABLE logs_2024_01 PARTITION OF logs
    FOR VALUES FROM ('2024-01-01') TO ('2024-02-01');

CREATE TABLE logs_2024_02 PARTITION OF logs
    FOR VALUES FROM ('2024-02-01') TO ('2024-03-01');

-- Partition par défaut (PostgreSQL 11+)
CREATE TABLE logs_default PARTITION OF logs DEFAULT;
```

### Partitionnement par liste
```sql
CREATE TABLE ventes (
    id SERIAL,
    pays VARCHAR(2),
    montant NUMERIC
) PARTITION BY LIST (pays);

CREATE TABLE ventes_fr PARTITION OF ventes FOR VALUES IN ('FR');
CREATE TABLE ventes_de PARTITION OF ventes FOR VALUES IN ('DE');
CREATE TABLE ventes_autres PARTITION OF ventes DEFAULT;
```

### Partitionnement par hachage
```sql
CREATE TABLE utilisateurs (
    id BIGSERIAL,
    nom VARCHAR(100)
) PARTITION BY HASH (id);

CREATE TABLE utilisateurs_p0 PARTITION OF utilisateurs
    FOR VALUES WITH (MODULUS 4, REMAINDER 0);

CREATE TABLE utilisateurs_p1 PARTITION OF utilisateurs
    FOR VALUES WITH (MODULUS 4, REMAINDER 1);
```

---

## 🔐 Sécurité et Permissions

### Gestion des rôles
```sql
-- Créer un rôle/utilisateur
CREATE ROLE lecteur;
CREATE USER dev_user WITH PASSWORD 'mot_de_passe';
CREATE ROLE admin WITH LOGIN PASSWORD 'admin123' SUPERUSER;

-- Modifier un rôle
ALTER ROLE lecteur WITH LOGIN;
ALTER USER dev_user WITH PASSWORD 'nouveau_mdp';

-- Supprimer un rôle
DROP ROLE lecteur;
```

### Permissions
```sql
-- Donner des permissions
GRANT SELECT ON utilisateurs TO lecteur;
GRANT SELECT, INSERT, UPDATE ON commandes TO dev_user;
GRANT ALL PRIVILEGES ON DATABASE ma_base TO admin;
GRANT USAGE ON SCHEMA public TO lecteur;

-- Permissions sur toutes les tables d'un schéma
GRANT SELECT ON ALL TABLES IN SCHEMA public TO lecteur;
GRANT ALL ON ALL TABLES IN SCHEMA public TO dev_user;

-- Permissions par défaut pour les futures tables
ALTER DEFAULT PRIVILEGES IN SCHEMA public
GRANT SELECT ON TABLES TO lecteur;

-- Révoquer des permissions
REVOKE INSERT ON utilisateurs FROM dev_user;
REVOKE ALL ON DATABASE ma_base FROM public;

-- Voir les permissions
\dp utilisateurs
SELECT * FROM information_schema.table_privileges
WHERE grantee = 'lecteur';
```

### Row Level Security (RLS)
```sql
-- Activer RLS
ALTER TABLE utilisateurs ENABLE ROW LEVEL SECURITY;

-- Créer une politique
CREATE POLICY user_isolation ON utilisateurs
    FOR ALL
    TO public
    USING (user_id = current_setting('app.user_id')::INT);

-- Politique pour lecture seule
CREATE POLICY read_own_data ON documents
    FOR SELECT
    USING (owner_id = current_user);

-- Politique pour insertion
CREATE POLICY insert_own_data ON documents
    FOR INSERT
    WITH CHECK (owner_id = current_user);
```

---

## 🆕 Nouveautés Récentes

### PostgreSQL 17 (2024)
```sql
-- MERGE command (SQL standard)
MERGE INTO stock s
USING commandes c ON s.produit_id = c.produit_id
WHEN MATCHED THEN
    UPDATE SET quantite = s.quantite - c.quantite
WHEN NOT MATCHED THEN
    INSERT (produit_id, quantite) VALUES (c.produit_id, 0);

-- Amélioration JSON_TABLE
SELECT *
FROM JSON_TABLE(
    '{"users": [{"name": "John", "age": 30}, {"name": "Jane", "age": 25}]}',
    '$.users[*]'
    COLUMNS (
        name TEXT PATH '$.name',
        age INT PATH '$.age'
    )
);
```

### PostgreSQL 16 (2023)
```sql
-- Support SQL/JSON amélioré
SELECT JSON_EXISTS(data, '$.user.email') FROM orders;
SELECT JSON_VALUE(data, '$.user.name') FROM orders;

-- Requêtes parallèles améliorées pour FULL/RIGHT JOIN
-- Amélioration des performances pour les requêtes logiques
```

### PostgreSQL 15 (2022)
```sql
-- MERGE statement (upsert avancé)
MERGE INTO customers c
USING new_customers n ON c.id = n.id
WHEN MATCHED THEN
    UPDATE SET name = n.name
WHEN NOT MATCHED THEN
    INSERT VALUES (n.id, n.name);

-- Support ICU pour les collations
CREATE COLLATION french_icu (provider = icu, locale = 'fr-FR');

-- Amélioration des performances DISTINCT
SELECT DISTINCT ON (category) * FROM products ORDER BY category, price DESC;
```

### PostgreSQL 14 (2021)
```sql
-- Requêtes multirequêtes en pipelines
-- Amélioration des performances de B-tree

-- Range types étendus
CREATE TABLE reservations (
    id SERIAL PRIMARY KEY,
    periode TSTZRANGE,
    EXCLUDE USING GIST (periode WITH &&)
);
```

### Fonctionnalités modernes utiles
```sql
-- GENERATED columns (colonnes calculées)
CREATE TABLE produits (
    prix_ht NUMERIC,
    tva NUMERIC,
    prix_ttc NUMERIC GENERATED ALWAYS AS (prix_ht * (1 + tva)) STORED
);

-- IDENTITY (alternative moderne à SERIAL)
CREATE TABLE employes (
    id INTEGER GENERATED ALWAYS AS IDENTITY PRIMARY KEY,
    nom VARCHAR(100)
);

-- Statistiques étendues
CREATE STATISTICS stats_ville_age ON ville, age FROM utilisateurs;

-- Window functions avec GROUPS et EXCLUDE
SELECT nom, salaire,
    AVG(salaire) OVER (
        ORDER BY salaire
        ROWS BETWEEN 2 PRECEDING AND 2 FOLLOWING
        EXCLUDE CURRENT ROW
    ) as avg_salaire
FROM employes;
```

---

## 🛠️ Commandes Utiles

### Backup et Restore
```bash
# Backup complet
pg_dump -U user -d database > backup.sql
pg_dump -U user -d database -F c -f backup.dump  # Format custom

# Backup d'une table
pg_dump -U user -d database -t table_name > table_backup.sql

# Restore
psql -U user -d database < backup.sql
pg_restore -U user -d database backup.dump

# Backup binaire (toutes les bases)
pg_dumpall -U postgres > all_databases.sql

# Backup incrémental avec WAL
pg_basebackup -U user -D /backup/dir -F tar -z -P
```

### Import/Export CSV
```bash
# Export vers CSV
psql -d database -c "\COPY table_name TO '/path/file.csv' CSV HEADER"

# Import depuis CSV
psql -d database -c "\COPY table_name FROM '/path/file.csv' CSV HEADER"
```

```sql
-- Export SQL
COPY utilisateurs TO '/tmp/users.csv' WITH CSV HEADER;
COPY (SELECT * FROM commandes WHERE statut = 'validée') 
TO '/tmp/commandes.csv' CSV HEADER DELIMITER ';';

-- Import SQL
COPY utilisateurs FROM '/tmp/users.csv' CSV HEADER;
COPY utilisateurs(nom, email) FROM '/tmp/users.csv' CSV HEADER;
```

### Performance et Monitoring
```sql
-- Voir les requêtes actives
SELECT pid, usename, state, query, query_start
FROM pg_stat_activity
WHERE state = 'active';

-- Tuer une requête
SELECT pg_cancel_backend(pid);
SELECT pg_terminate_backend(pid);  -- Force kill

-- Taille des bases de données
SELECT datname, pg_size_pretty(pg_database_size(datname))
FROM pg_database
ORDER BY pg_database_size(datname) DESC;

-- Taille des tables
SELECT tablename,
    pg_size_pretty(pg_total_relation_size(schemaname||'.'||tablename)) AS size
FROM pg_tables
WHERE schemaname = 'public'
ORDER BY pg_total_relation_size(schemaname||'.'||tablename) DESC;

-- Cache hit ratio (devrait être > 99%)
SELECT 
    sum(heap_blks_read) as heap_read,
    sum(heap_blks_hit) as heap_hit,
    sum(heap_blks_hit) / (sum(heap_blks_hit) + sum(heap_blks_read)) as ratio
FROM pg_statio_user_tables;

-- Index inutilisés
SELECT schemaname, tablename, indexname, idx_scan
FROM pg_stat_user_indexes
WHERE idx_scan = 0 AND indexname NOT LIKE '%_pkey';

-- Tables les plus volumineuses
SELECT
    schemaname,
    tablename,
    pg_size_pretty(pg_total_relation_size(schemaname||'.'||tablename)) AS total_size,
    pg_size_pretty(pg_relation_size(schemaname||'.'||tablename)) AS table_size,
    pg_size_pretty(pg_total_relation_size(schemaname||'.'||tablename) - pg_relation_size(schemaname||'.'||tablename)) AS index_size
FROM pg_tables
ORDER BY pg_total_relation_size(schemaname||'.'||tablename) DESC
LIMIT 10;

-- Bloat (espace gaspillé)
SELECT
    schemaname, tablename,
    pg_size_pretty(pg_total_relation_size(schemaname||'.'||tablename)) AS size,
    n_dead_tup, n_live_tup,
    round(n_dead_tup * 100.0 / NULLIF(n_live_tup + n_dead_tup, 0), 2) AS dead_ratio
FROM pg_stat_user_tables
WHERE n_live_tup > 0
ORDER BY n_dead_tup DESC
LIMIT 10;

-- Locks actifs
SELECT
    locktype, relation::regclass, mode, transactionid AS tid,
    virtualtransaction AS vtid, pid, granted
FROM pg_locks
ORDER BY pid;
```

### Maintenance
```sql
-- VACUUM (nettoyer l'espace)
VACUUM utilisateurs;
VACUUM FULL utilisateurs;  -- Plus agressif, bloque la table
VACUUM ANALYZE utilisateurs;  -- Nettoie et met à jour les stats

-- ANALYZE (mettre à jour les statistiques)
ANALYZE utilisateurs;
ANALYZE;  -- Toutes les tables

-- REINDEX
REINDEX TABLE utilisateurs;
REINDEX INDEX idx_nom;
REINDEX DATABASE ma_base;

-- Activer autovacuum (dans postgresql.conf)
autovacuum = on
autovacuum_max_workers = 3
autovacuum_naptime = 1min
```

---

## 📚 Fonctions Utiles

### Fonctions de chaînes
```sql
-- Concaténation
SELECT 'Hello' || ' ' || 'World';
SELECT CONCAT('Hello', ' ', 'World');
SELECT CONCAT_WS(', ', 'John', 'Doe', 'Smith');  -- Avec séparateur

-- Manipulation
SELECT LENGTH('Hello');
SELECT LOWER('HELLO');
SELECT UPPER('hello');
SELECT INITCAP('hello world');  -- Hello World
SELECT TRIM('  hello  ');
SELECT LTRIM('  hello');
SELECT RTRIM('hello  ');
SELECT SUBSTRING('Hello World' FROM 1 FOR 5);
SELECT LEFT('Hello', 3);
SELECT RIGHT('Hello', 3);
SELECT POSITION('World' IN 'Hello World');
SELECT REPLACE('Hello World', 'World', 'PostgreSQL');
SELECT SPLIT_PART('a,b,c', ',', 2);  -- Retourne 'b'
SELECT REPEAT('Ha', 3);  -- HaHaHa
SELECT REVERSE('Hello');
SELECT LPAD('42', 5, '0');  -- 00042
SELECT RPAD('42', 5, '0');  -- 42000

-- Regex
SELECT 'Hello' ~ '^H';  -- true
SELECT REGEXP_REPLACE('Hello123', '[0-9]', '', 'g');
SELECT REGEXP_MATCHES('abc123def456', '[0-9]+', 'g');
```

### Fonctions de dates
```sql
-- Date/heure actuelle
SELECT NOW();
SELECT CURRENT_TIMESTAMP;
SELECT CURRENT_DATE;
SELECT CURRENT_TIME;
SELECT CLOCK_TIMESTAMP();  -- Heure exacte à chaque appel

-- Extraction
SELECT EXTRACT(YEAR FROM NOW());
SELECT EXTRACT(MONTH FROM NOW());
SELECT EXTRACT(DAY FROM NOW());
SELECT EXTRACT(HOUR FROM NOW());
SELECT DATE_PART('year', NOW());

-- Troncature
SELECT DATE_TRUNC('day', NOW());
SELECT DATE_TRUNC('month', NOW());
SELECT DATE_TRUNC('year', NOW());
SELECT DATE_TRUNC('hour', NOW());

-- Calculs
SELECT NOW() + INTERVAL '1 day';
SELECT NOW() - INTERVAL '1 month';
SELECT NOW() + INTERVAL '2 hours 30 minutes';
SELECT AGE(NOW(), '1990-01-01');
SELECT AGE('2024-01-01', '2023-01-01');

-- Différence
SELECT '2024-12-31'::DATE - '2024-01-01'::DATE;  -- En jours
SELECT EXTRACT(EPOCH FROM (NOW() - '2024-01-01'::TIMESTAMP));  -- En secondes

-- Formatage
SELECT TO_CHAR(NOW(), 'YYYY-MM-DD');
SELECT TO_CHAR(NOW(), 'DD/MM/YYYY HH24:MI:SS');
SELECT TO_CHAR(NOW(), 'Day, DD Month YYYY');
SELECT TO_DATE('2024-01-15', 'YYYY-MM-DD');
SELECT TO_TIMESTAMP('2024-01-15 14:30:00', 'YYYY-MM-DD HH24:MI:SS');

-- Générer des séries
SELECT generate_series('2024-01-01'::DATE, '2024-01-31'::DATE, '1 day'::INTERVAL);
```

### Fonctions mathématiques
```sql
SELECT ABS(-42);
SELECT CEIL(4.3);  -- 5
SELECT FLOOR(4.7);  -- 4
SELECT ROUND(4.567, 2);  -- 4.57
SELECT TRUNC(4.567, 2);  -- 4.56
SELECT POWER(2, 3);  -- 8
SELECT SQRT(16);  -- 4
SELECT MOD(10, 3);  -- 1
SELECT RANDOM();  -- Entre 0 et 1
SELECT FLOOR(RANDOM() * 100);  -- Entre 0 et 99
SELECT GREATEST(1, 5, 3);  -- 5
SELECT LEAST(1, 5, 3);  -- 1
```

### Fonctions de conversion
```sql
-- Cast
SELECT CAST('123' AS INTEGER);
SELECT '123'::INTEGER;
SELECT 123::TEXT;
SELECT NOW()::DATE;

-- Conversions
SELECT TO_NUMBER('12,345.67', '99G999D99');
SELECT TO_CHAR(12345.67, '99,999.99');
```

### Fonctions conditionnelles
```sql
-- CASE
SELECT nom,
    CASE 
        WHEN age < 18 THEN 'Mineur'
        WHEN age < 65 THEN 'Adulte'
        ELSE 'Senior'
    END as categorie
FROM utilisateurs;

-- COALESCE (première valeur non-null)
SELECT COALESCE(telephone, email, 'Aucun contact') FROM utilisateurs;

-- NULLIF (retourne NULL si égal)
SELECT NULLIF(valeur, 0);  -- Évite la division par zéro

-- GREATEST / LEAST
SELECT GREATEST(valeur1, valeur2, valeur3);
SELECT LEAST(valeur1, valeur2, valeur3);
```

### Fonctions de tableaux
```sql
-- Créer un tableau
SELECT ARRAY[1, 2, 3, 4, 5];
SELECT ARRAY['a', 'b', 'c'];

-- Accès aux éléments
SELECT (ARRAY[1,2,3])[2];  -- 2 (indexé à 1)

-- Fonctions
SELECT ARRAY_LENGTH(ARRAY[1,2,3], 1);  -- 3
SELECT ARRAY_APPEND(ARRAY[1,2], 3);
SELECT ARRAY_PREPEND(1, ARRAY[2,3]);
SELECT ARRAY_CAT(ARRAY[1,2], ARRAY[3,4]);
SELECT ARRAY_REMOVE(ARRAY[1,2,3,2], 2);
SELECT ARRAY_POSITION(ARRAY['a','b','c'], 'b');  -- 2

-- Opérateurs
SELECT ARRAY[1,2] || ARRAY[3,4];  -- {1,2,3,4}
SELECT 2 = ANY(ARRAY[1,2,3]);  -- true
SELECT 2 = ALL(ARRAY[2,2,2]);  -- true
SELECT ARRAY[1,2] && ARRAY[2,3];  -- true (intersection)
SELECT ARRAY[1,2] @> ARRAY[2];  -- true (contient)
SELECT ARRAY[2] <@ ARRAY[1,2,3];  -- true (contenu dans)

-- Unnest (convertir en lignes)
SELECT UNNEST(ARRAY[1,2,3,4]);
```

### Fonctions système
```sql
-- Informations système
SELECT VERSION();
SELECT CURRENT_DATABASE();
SELECT CURRENT_SCHEMA();
SELECT CURRENT_USER;
SELECT SESSION_USER;
SELECT INET_CLIENT_ADDR();
SELECT INET_CLIENT_PORT();

-- UUID
SELECT GEN_RANDOM_UUID();  -- Nécessite l'extension pgcrypto

-- Séquences
SELECT NEXTVAL('ma_sequence');
SELECT CURRVAL('ma_sequence');
SELECT SETVAL('ma_sequence', 100);
SELECT LASTVAL();
```

---

## 🔧 Extensions Populaires

```sql
-- Voir les extensions disponibles
SELECT * FROM pg_available_extensions;

-- Voir les extensions installées
\dx
SELECT * FROM pg_extension;

-- Installer une extension
CREATE EXTENSION IF NOT EXISTS extension_name;
```

### Extensions essentielles
```sql
-- pgcrypto : Fonctions cryptographiques
CREATE EXTENSION pgcrypto;
SELECT CRYPT('password', GEN_SALT('bf'));
SELECT GEN_RANDOM_UUID();

-- hstore : Paires clé-valeur
CREATE EXTENSION hstore;
SELECT 'a=>1, b=>2'::hstore;

-- uuid-ossp : Génération UUID
CREATE EXTENSION "uuid-ossp";
SELECT UUID_GENERATE_V4();

-- pg_trgm : Similarité de texte
CREATE EXTENSION pg_trgm;
SELECT SIMILARITY('hello', 'hallo');
CREATE INDEX idx_trgm ON articles USING GIN(titre gin_trgm_ops);

-- postgis : Données géospatiales
CREATE EXTENSION postgis;
SELECT ST_Distance(point1, point2) FROM locations;

-- pg_stat_statements : Analyse des requêtes
CREATE EXTENSION pg_stat_statements;
SELECT query, calls, total_exec_time
FROM pg_stat_statements
ORDER BY total_exec_time DESC
LIMIT 10;

-- timescaledb : Séries temporelles
CREATE EXTENSION timescaledb;
SELECT CREATE_HYPERTABLE('metrics', 'time');

-- ltree : Hiérarchies
CREATE EXTENSION ltree;
-- Stocke des chemins comme '1.2.3.4'

-- pg_cron : Planificateur de tâches
CREATE EXTENSION pg_cron;
SELECT CRON.SCHEDULE('nightly-vacuum', '0 3 * * *', 'VACUUM');

-- plpython3u : Python dans PostgreSQL
CREATE EXTENSION plpython3u;

-- tablefunc : Fonctions de manipulation de tables
CREATE EXTENSION tablefunc;
SELECT * FROM CROSSTAB(...);  -- Tableaux croisés dynamiques
```

---

## 💡 Astuces et Best Practices

### Performance
```sql
-- Utiliser EXISTS au lieu de IN pour les grandes tables
-- ❌ Lent
SELECT * FROM users WHERE id IN (SELECT user_id FROM orders);

-- ✅ Rapide
SELECT * FROM users u WHERE EXISTS (
    SELECT 1 FROM orders o WHERE o.user_id = u.id
);

-- Utiliser LIMIT avec OFFSET prudemment
-- ❌ Lent pour les grandes offsets
SELECT * FROM logs ORDER BY id LIMIT 10 OFFSET 1000000;

-- ✅ Meilleur avec keyset pagination
SELECT * FROM logs WHERE id > 1000000 ORDER BY id LIMIT 10;

-- Éviter SELECT *
-- ❌ Mauvais
SELECT * FROM users;

-- ✅ Bon
SELECT id, name, email FROM users;

-- Utiliser EXPLAIN ANALYZE
EXPLAIN (ANALYZE, BUFFERS, VERBOSE) SELECT ...;
```

### Bonnes pratiques
```sql
-- Toujours utiliser des transactions pour les modifications multiples
BEGIN;
    -- Vos requêtes
COMMIT;

-- Utiliser des index appropriés
-- Pour les colonnes souvent utilisées dans WHERE, JOIN, ORDER BY

-- Partitionner les très grandes tables
-- Pour les tables > 10 millions de lignes

-- Utiliser des contraintes pour l'intégrité
-- PRIMARY KEY, FOREIGN KEY, CHECK, UNIQUE

-- Nommer explicitement les contraintes
ALTER TABLE users
ADD CONSTRAINT fk_users_roles
FOREIGN KEY (role_id) REFERENCES roles(id);

-- Utiliser des valeurs par défaut
ALTER TABLE users ALTER COLUMN created_at SET DEFAULT NOW();

-- Documenter avec COMMENT
COMMENT ON TABLE users IS 'Table des utilisateurs de l''application';
COMMENT ON COLUMN users.email IS 'Email unique de l''utilisateur';
```

### Sécurité
```sql
-- Ne jamais stocker de mots de passe en clair
-- Utiliser pgcrypto pour le hachage

-- Utiliser des requêtes préparées pour éviter les injections SQL
PREPARE get_user (INT) AS
    SELECT * FROM users WHERE id = $1;
EXECUTE get_user(42);

-- Limiter les permissions
-- Principe du moindre privilège

-- Activer SSL
-- Dans postgresql.conf : ssl = on

-- Chiffrer les données sensibles
SELECT PGP_SYM_ENCRYPT('données sensibles', 'clé_secrète');
SELECT PGP_SYM_DECRYPT(colonne_chiffrée, 'clé_secrète') FROM table;

-- Auditer les connexions
-- log_connections = on
-- log_disconnections = on
```

### Conventions de nommage
```sql
-- Tables : pluriel, minuscules, snake_case
users, order_items, product_categories

-- Colonnes : singulier, snake_case
user_id, created_at, is_active

-- Index : idx_table_columns
idx_users_email, idx_orders_user_id_created_at

-- Contraintes : type_table_columns
pk_users, fk_orders_users, uk_users_email, ck_users_age

-- Fonctions : verbe_nom
calculate_total, get_user_orders, validate_email

-- Vues : v_ ou vue_
v_active_users, vue_commandes_recentes
```

---

## 🎓 Requêtes Avancées Exemples

### Rank et Window Functions
```sql
-- Top 3 par catégorie
SELECT * FROM (
    SELECT *,
        ROW_NUMBER() OVER (PARTITION BY category ORDER BY sales DESC) as rang
    FROM products
) sub
WHERE rang <= 3;

-- Différence avec la ligne précédente
SELECT date, value,
    value - LAG(value) OVER (ORDER BY date) as difference
FROM metrics;

-- Moyenne mobile
SELECT date, value,
    AVG(value) OVER (
        ORDER BY date
        ROWS BETWEEN 6 PRECEDING AND CURRENT ROW
    ) as moving_avg_7days
FROM metrics;

-- Cumul
SELECT date, amount,
    SUM(amount) OVER (ORDER BY date) as cumulative_total
FROM sales;
```

### Pivots et Unpivots
```sql
-- Pivot avec FILTER
SELECT
    product,
    SUM(amount) FILTER (WHERE month = 1) as jan,
    SUM(amount) FILTER (WHERE month = 2) as feb,
    SUM(amount) FILTER (WHERE month = 3) as mar
FROM sales
GROUP BY product;

-- Pivot avec CROSSTAB (nécessite tablefunc)
SELECT *
FROM CROSSTAB(
    'SELECT product, month, SUM(amount) FROM sales GROUP BY 1,2 ORDER BY 1,2',
    'SELECT DISTINCT month FROM sales ORDER BY 1'
) AS ct(product TEXT, jan NUMERIC, feb NUMERIC, mar NUMERIC);
```

### Recherche de doublons
```sql
-- Trouver les doublons
SELECT email, COUNT(*)
FROM users
GROUP BY email
HAVING COUNT(*) > 1;

-- Supprimer les doublons (garder le plus récent)
DELETE FROM users a
USING users b
WHERE a.id < b.id
AND a.email = b.email;

-- Avec ROW_NUMBER
DELETE FROM users
WHERE id IN (
    SELECT id FROM (
        SELECT id,
            ROW_NUMBER() OVER (PARTITION BY email ORDER BY created_at DESC) as rn
        FROM users
    ) sub
    WHERE rn > 1
);
```

### Récursion avancée
```sql
-- Calculer la factorielle
WITH RECURSIVE factorielle(n, fact) AS (
    SELECT 1, 1
    UNION ALL
    SELECT n+1, (n+1)*fact
    FROM factorielle
    WHERE n < 10
)
SELECT * FROM factorielle;

-- Suite de Fibonacci
WITH RECURSIVE fib(a, b) AS (
    SELECT 0, 1
    UNION ALL
    SELECT b, a+b FROM fib WHERE b < 1000
)
SELECT a FROM fib;

-- Chemin le plus court (graph)
WITH RECURSIVE paths AS (
    SELECT id, source, target, cost, ARRAY[source] as path
    FROM edges
    WHERE source = 'A'
    
    UNION ALL
    
    SELECT e.id, p.source, e.target, p.cost + e.cost, path || e.target
    FROM paths p
    JOIN edges e ON p.target = e.source
    WHERE NOT e.target = ANY(path)  -- Éviter les cycles
)
SELECT * FROM paths WHERE target = 'Z' ORDER BY cost LIMIT 1;
```

---

## 📖 Ressources Supplémentaires

- Documentation officielle : https://www.postgresql.org/docs/
- Wiki PostgreSQL : https://wiki.postgresql.org/
- PostgreSQL Tutorial : https://www.postgresqltutorial.com/
- Explain Visualizer : https://explain.dalibo.com/
- pgAdmin : Interface graphique officielle
- DBeaver : Client SQL multi-plateformes

