# PostgreSQL CheatSheet 

## Table des matières
- [Connexion et administration](#connexion-et-administration)
- [Gestion des bases de données](#gestion-des-bases-de-données)
- [Gestion des tables](#gestion-des-tables)
- [Types de données](#types-de-données)
- [Requêtes de base (CRUD)](#requêtes-de-base-crud)
- [Jointures](#jointures)
- [Fonctions d'agrégation](#fonctions-dagrégation)
- [Sous-requêtes et CTE](#sous-requêtes-et-cte)
- [Index et optimisation](#index-et-optimisation)
- [Transactions](#transactions)
- [Contraintes](#contraintes)
- [Vues et vues matérialisées](#vues-et-vues-matérialisées)
- [Fonctions et procédures](#fonctions-et-procédures)
- [Triggers](#triggers)
- [JSON et JSONB](#json-et-jsonb)
- [Full-Text Search](#full-text-search)
- [Partitionnement](#partitionnement)
- [Sécurité et permissions](#sécurité-et-permissions)
- [Nouveautés récentes](#nouveautés-récentes)
- [Window Functions](#window-functions)
- [Backup et Restore](#backup-et-restore)
- [Monitoring et Maintenance](#monitoring-et-maintenance)
- [Configuration PostgreSQL](#configuration-postgresql)
- [Réplication](#réplication)
- [Arrays et fonctions](#arrays-et-fonctions)
- [Extensions populaires](#extensions-populaires)
- [Astuces et best practices](#astuces-et-best-practices)
- [Requêtes avancées exemples](#requêtes-avancées-exemples)
- [Ressources supplémentaires](#ressources-supplémentaires)

---

## Connexion et Administration

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

## Gestion des Bases de Données

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

## Gestion des Tables

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

## Types de Données

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

## Requêtes de Base (CRUD)

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

## Jointures

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

-- FULL OUTER JOIN (tous les enregistrements des deux côtés)
SELECT u.nom, c.numero
FROM utilisateurs u
FULL OUTER JOIN commandes c ON u.id = c.user_id;

-- CROSS JOIN (produit cartésien)
SELECT * FROM couleurs CROSS JOIN tailles;

-- SELF JOIN (jointure avec elle-même)
SELECT e1.nom as employe, e2.nom as manager
FROM employes e1
LEFT JOIN employes e2 ON e1.manager_id = e2.id;

-- Jointures multiples
SELECT u.nom, c.numero, p.nom as produit
FROM utilisateurs u
INNER JOIN commandes c ON u.id = c.user_id
INNER JOIN produits p ON c.produit_id = p.id;

-- USING (quand même nom de colonne)
SELECT u.nom, c.numero
FROM utilisateurs u
INNER JOIN commandes c USING (user_id);

-- NATURAL JOIN (jointure automatique sur colonnes communes)
SELECT * FROM utilisateurs NATURAL JOIN profils;

-- LATERAL JOIN (jointure latérale)
SELECT u.nom, recent.*
FROM utilisateurs u
CROSS JOIN LATERAL (
    SELECT * FROM commandes c
    WHERE c.user_id = u.id
    ORDER BY c.date DESC
    LIMIT 3
) recent;
```

---

## Fonctions d'Agrégation

```sql
-- Fonctions de base
SELECT
    COUNT(*) as total,
    COUNT(DISTINCT ville) as villes_uniques,
    SUM(prix) as total_prix,
    AVG(prix) as prix_moyen,
    MIN(prix) as prix_min,
    MAX(prix) as prix_max
FROM produits;

-- Agrégations conditionnelles avec FILTER
SELECT
    COUNT(*) as total,
    COUNT(*) FILTER (WHERE actif = true) as actifs,
    COUNT(*) FILTER (WHERE actif = false) as inactifs,
    AVG(age) FILTER (WHERE age < 30) as age_moyen_jeunes
FROM utilisateurs;

-- STRING_AGG (concaténation)
SELECT categorie, STRING_AGG(nom, ', ' ORDER BY nom) as produits
FROM produits
GROUP BY categorie;

-- ARRAY_AGG (agrégation en tableau)
SELECT categorie, ARRAY_AGG(nom ORDER BY nom) as produits
FROM produits
GROUP BY categorie;

-- Agrégations statistiques
SELECT
    STDDEV(prix) as ecart_type,
    VARIANCE(prix) as variance,
    PERCENTILE_CONT(0.5) WITHIN GROUP (ORDER BY prix) as mediane,
    PERCENTILE_CONT(0.95) WITHIN GROUP (ORDER BY prix) as percentile_95
FROM produits;

-- GROUPING SETS, ROLLUP, CUBE
SELECT annee, trimestre, SUM(ventes)
FROM ventes
GROUP BY GROUPING SETS ((annee, trimestre), (annee), ());

SELECT annee, trimestre, SUM(ventes)
FROM ventes
GROUP BY ROLLUP (annee, trimestre);

SELECT produit, region, SUM(ventes)
FROM ventes
GROUP BY CUBE (produit, region);
```

---

## Sous-requêtes et CTE

### Sous-requêtes
```sql
-- Sous-requête dans WHERE
SELECT nom FROM utilisateurs
WHERE id IN (SELECT user_id FROM commandes WHERE montant > 100);

-- Sous-requête dans SELECT
SELECT nom,
    (SELECT COUNT(*) FROM commandes WHERE user_id = u.id) as nb_commandes
FROM utilisateurs u;

-- Sous-requête dans FROM
SELECT avg_age FROM (
    SELECT AVG(age) as avg_age FROM utilisateurs
) subquery;

-- Sous-requête corrélée
SELECT nom FROM produits p
WHERE prix > (
    SELECT AVG(prix) FROM produits
    WHERE categorie = p.categorie
);

-- EXISTS
SELECT nom FROM clients c
WHERE EXISTS (
    SELECT 1 FROM commandes WHERE client_id = c.id
);

-- ANY/ALL
SELECT nom FROM produits
WHERE prix > ALL (SELECT prix FROM produits WHERE categorie = 'budget');
```

### CTE (Common Table Expressions)
```sql
-- CTE simple
WITH clients_actifs AS (
    SELECT * FROM clients WHERE actif = true
)
SELECT * FROM clients_actifs WHERE ville = 'Paris';

-- CTE multiples
WITH
    ventes_2023 AS (
        SELECT * FROM ventes WHERE EXTRACT(YEAR FROM date) = 2023
    ),
    top_clients AS (
        SELECT client_id, SUM(montant) as total
        FROM ventes_2023
        GROUP BY client_id
        ORDER BY total DESC
        LIMIT 10
    )
SELECT c.nom, tc.total
FROM top_clients tc
JOIN clients c ON tc.client_id = c.id;

-- CTE récursive (hiérarchie)
WITH RECURSIVE employee_hierarchy AS (
    -- Ancre : employés sans manager
    SELECT id, nom, manager_id, 0 as niveau
    FROM employes
    WHERE manager_id IS NULL
    
    UNION ALL
    
    -- Récursion : employés avec manager
    SELECT e.id, e.nom, e.manager_id, eh.niveau + 1
    FROM employes e
    JOIN employee_hierarchy eh ON e.manager_id = eh.id
)
SELECT * FROM employee_hierarchy ORDER BY niveau, nom;

-- CTE récursive (génération de séries)
WITH RECURSIVE dates(date) AS (
    SELECT DATE '2024-01-01'
    UNION ALL
    SELECT date + 1 FROM dates WHERE date < DATE '2024-12-31'
)
SELECT * FROM dates;
```

---

## Index et Optimisation

### Types d'index
```sql
-- Index B-tree (par défaut)
CREATE INDEX idx_users_email ON users(email);

-- Index unique
CREATE UNIQUE INDEX idx_users_email_unique ON users(email);

-- Index composite (multi-colonnes)
CREATE INDEX idx_orders_user_date ON orders(user_id, created_at);

-- Index partiel (avec condition)
CREATE INDEX idx_active_users ON users(email) WHERE actif = true;

-- Index GIN (pour JSONB, arrays, full-text)
CREATE INDEX idx_metadata ON documents USING GIN(metadata);
CREATE INDEX idx_tags ON articles USING GIN(tags);

-- Index GiST (pour géométrie, full-text)
CREATE INDEX idx_location ON places USING GIST(coordinates);

-- Index BRIN (pour très grandes tables avec corrélation)
CREATE INDEX idx_logs_timestamp ON logs USING BRIN(timestamp);

-- Index sur expression
CREATE INDEX idx_lower_email ON users(LOWER(email));
CREATE INDEX idx_year ON orders(EXTRACT(YEAR FROM created_at));

-- Index avec INCLUDE (colonnes supplémentaires)
CREATE INDEX idx_users_email_inc ON users(email) INCLUDE (nom, prenom);

-- Index concurrently (sans bloquer)
CREATE INDEX CONCURRENTLY idx_users_email ON users(email);
```

### Gestion des index
```sql
-- Lister les index
\di
SELECT * FROM pg_indexes WHERE tablename = 'users';

-- Supprimer un index
DROP INDEX idx_users_email;
DROP INDEX CONCURRENTLY idx_users_email;

-- Reconstruire un index
REINDEX INDEX idx_users_email;
REINDEX TABLE users;
REINDEX DATABASE ma_base;

-- Taille des index
SELECT
    schemaname,
    tablename,
    indexname,
    pg_size_pretty(pg_relation_size(indexrelid)) as index_size
FROM pg_stat_user_indexes
ORDER BY pg_relation_size(indexrelid) DESC;

-- Index inutilisés
SELECT
    schemaname,
    tablename,
    indexname,
    idx_scan
FROM pg_stat_user_indexes
WHERE idx_scan = 0 AND indexrelname NOT LIKE 'pg_toast%'
ORDER BY pg_relation_size(indexrelid) DESC;
```

### EXPLAIN et optimisation
```sql
-- Plan d'exécution simple
EXPLAIN SELECT * FROM users WHERE age > 25;

-- Plan avec exécution réelle
EXPLAIN ANALYZE SELECT * FROM users WHERE age > 25;

-- Plan détaillé
EXPLAIN (ANALYZE, BUFFERS, VERBOSE, COSTS, TIMING)
SELECT * FROM users WHERE age > 25;

-- Format JSON
EXPLAIN (ANALYZE, FORMAT JSON) SELECT * FROM users;

-- Statistiques de la table
ANALYZE users;

-- Vacuum et analyse
VACUUM ANALYZE users;
VACUUM FULL users;  -- Attention : bloque la table

-- Autovacuum (automatique)
-- Configuration dans postgresql.conf
```

---

## Transactions

```sql
-- Transaction basique
BEGIN;
    UPDATE comptes SET solde = solde - 100 WHERE id = 1;
    UPDATE comptes SET solde = solde + 100 WHERE id = 2;
COMMIT;

-- Rollback en cas d'erreur
BEGIN;
    UPDATE comptes SET solde = solde - 100 WHERE id = 1;
    -- Erreur ici
ROLLBACK;

-- Savepoints (points de sauvegarde)
BEGIN;
    UPDATE users SET actif = false WHERE id = 1;
    SAVEPOINT sp1;
    
    UPDATE users SET actif = false WHERE id = 2;
    SAVEPOINT sp2;
    
    -- Erreur, revenir à sp2
    ROLLBACK TO sp2;
    
    -- Continuer
    UPDATE users SET actif = false WHERE id = 3;
COMMIT;

-- Niveaux d'isolation
BEGIN TRANSACTION ISOLATION LEVEL READ UNCOMMITTED;
BEGIN TRANSACTION ISOLATION LEVEL READ COMMITTED;  -- Par défaut
BEGIN TRANSACTION ISOLATION LEVEL REPEATABLE READ;
BEGIN TRANSACTION ISOLATION LEVEL SERIALIZABLE;

-- Transaction en lecture seule
BEGIN TRANSACTION READ ONLY;

-- Vérifier l'état de la transaction
SELECT current_setting('transaction_isolation');
SELECT current_setting('transaction_read_only');

-- Lock explicite
BEGIN;
    SELECT * FROM users WHERE id = 1 FOR UPDATE;  -- Lock en écriture
    -- Modifier la ligne
    UPDATE users SET nom = 'Nouveau' WHERE id = 1;
COMMIT;

-- Locks multiples
SELECT * FROM users WHERE id IN (1,2,3) FOR UPDATE;
SELECT * FROM users WHERE age > 25 FOR SHARE;  -- Lock en lecture

-- Deadlock timeout
SET deadlock_timeout = '1s';

-- Transaction advisory locks
SELECT pg_advisory_lock(123);
SELECT pg_advisory_unlock(123);
SELECT pg_try_advisory_lock(123);
```

---

## Contraintes

```sql
-- PRIMARY KEY
CREATE TABLE users (
    id SERIAL PRIMARY KEY
);

-- PRIMARY KEY composite
CREATE TABLE enrollments (
    user_id INT,
    course_id INT,
    PRIMARY KEY (user_id, course_id)
);

-- FOREIGN KEY
CREATE TABLE orders (
    id SERIAL PRIMARY KEY,
    user_id INT NOT NULL,
    FOREIGN KEY (user_id) REFERENCES users(id)
);

-- FOREIGN KEY avec actions
CREATE TABLE orders (
    id SERIAL PRIMARY KEY,
    user_id INT NOT NULL,
    FOREIGN KEY (user_id) REFERENCES users(id)
        ON DELETE CASCADE        -- Supprimer en cascade
        ON UPDATE CASCADE        -- Mettre à jour en cascade
);

-- Autres actions : SET NULL, SET DEFAULT, RESTRICT, NO ACTION

-- UNIQUE
CREATE TABLE users (
    email VARCHAR(255) UNIQUE
);

-- UNIQUE composite
CREATE TABLE likes (
    user_id INT,
    post_id INT,
    UNIQUE (user_id, post_id)
);

-- CHECK
CREATE TABLE products (
    price DECIMAL CHECK (price > 0),
    discount DECIMAL CHECK (discount BETWEEN 0 AND 1),
    stock INT CHECK (stock >= 0)
);

-- CHECK avec nom
ALTER TABLE products
ADD CONSTRAINT ck_price_positive CHECK (price > 0);

-- CHECK multi-colonnes
ALTER TABLE products
ADD CONSTRAINT ck_price_discount CHECK (price * (1 - discount) > 0);

-- NOT NULL
CREATE TABLE users (
    nom VARCHAR(100) NOT NULL,
    email VARCHAR(255) NOT NULL
);

-- DEFAULT
CREATE TABLE users (
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    actif BOOLEAN DEFAULT true,
    role VARCHAR(50) DEFAULT 'user'
);

-- EXCLUDE (nécessite btree_gist)
CREATE TABLE reservations (
    room_id INT,
    during TSRANGE,
    EXCLUDE USING GIST (room_id WITH =, during WITH &&)
);

-- Ajouter/Supprimer des contraintes
ALTER TABLE users ADD CONSTRAINT uk_email UNIQUE (email);
ALTER TABLE users DROP CONSTRAINT uk_email;

-- Désactiver/Activer les contraintes
ALTER TABLE orders DISABLE TRIGGER ALL;
ALTER TABLE orders ENABLE TRIGGER ALL;

-- Vérifier les contraintes
ALTER TABLE users VALIDATE CONSTRAINT ck_age;

-- Lister les contraintes
SELECT * FROM information_schema.table_constraints
WHERE table_name = 'users';
```

---

## Vues et Vues Matérialisées

### Vues classiques
```sql
-- Créer une vue
CREATE VIEW active_users AS
SELECT id, nom, email
FROM users
WHERE actif = true;

-- Utiliser une vue
SELECT * FROM active_users;

-- Vue avec options
CREATE OR REPLACE VIEW user_stats AS
SELECT
    u.id,
    u.nom,
    COUNT(c.id) as nb_commandes,
    SUM(c.montant) as total_depense
FROM users u
LEFT JOIN commandes c ON u.id = c.user_id
GROUP BY u.id, u.nom;

-- Vue récursive
CREATE RECURSIVE VIEW employee_tree AS
    SELECT id, nom, manager_id, 1 as niveau
    FROM employes
    WHERE manager_id IS NULL
    UNION ALL
    SELECT e.id, e.nom, e.manager_id, et.niveau + 1
    FROM employes e
    JOIN employee_tree et ON e.manager_id = et.id;

-- Supprimer une vue
DROP VIEW active_users;
DROP VIEW IF EXISTS active_users CASCADE;

-- Vue updatable (modifiable)
CREATE VIEW simple_users AS
SELECT id, nom, email FROM users;

UPDATE simple_users SET nom = 'Nouveau' WHERE id = 1;

-- Vue avec CHECK OPTION
CREATE VIEW adult_users AS
SELECT * FROM users WHERE age >= 18
WITH CHECK OPTION;

-- Lister les vues
\dv
SELECT * FROM information_schema.views WHERE table_schema = 'public';
```

### Vues matérialisées
```sql
-- Créer une vue matérialisée
CREATE MATERIALIZED VIEW monthly_sales AS
SELECT
    DATE_TRUNC('month', date) as mois,
    SUM(montant) as total
FROM ventes
GROUP BY mois;

-- Créer avec index
CREATE MATERIALIZED VIEW monthly_sales AS
SELECT
    DATE_TRUNC('month', date) as mois,
    SUM(montant) as total
FROM ventes
GROUP BY mois;

CREATE UNIQUE INDEX ON monthly_sales (mois);

-- Rafraîchir une vue matérialisée
REFRESH MATERIALIZED VIEW monthly_sales;

-- Rafraîchir sans bloquer les lectures
REFRESH MATERIALIZED VIEW CONCURRENTLY monthly_sales;

-- Supprimer une vue matérialisée
DROP MATERIALIZED VIEW monthly_sales;

-- Lister les vues matérialisées
\dm
SELECT * FROM pg_matviews;
```

---

## Fonctions et Procédures

### Fonctions
```sql
-- Fonction simple
CREATE FUNCTION add_numbers(a INT, b INT)
RETURNS INT AS $$
BEGIN
    RETURN a + b;
END;
$$ LANGUAGE plpgsql;

-- Utilisation
SELECT add_numbers(5, 3);

-- Fonction avec valeur par défaut
CREATE FUNCTION greet(name TEXT DEFAULT 'World')
RETURNS TEXT AS $$
BEGIN
    RETURN 'Hello, ' || name || '!';
END;
$$ LANGUAGE plpgsql;

-- Fonction retournant une table
CREATE FUNCTION get_active_users()
RETURNS TABLE (id INT, nom TEXT, email TEXT) AS $$
BEGIN
    RETURN QUERY
    SELECT u.id, u.nom, u.email
    FROM users u
    WHERE u.actif = true;
END;
$$ LANGUAGE plpgsql;

-- Utilisation
SELECT * FROM get_active_users();

-- Fonction avec OUT parameters
CREATE FUNCTION get_user_stats(user_id INT, OUT nb_orders INT, OUT total_amount DECIMAL)
AS $$
BEGIN
    SELECT COUNT(*), SUM(montant)
    INTO nb_orders, total_amount
    FROM commandes
    WHERE commandes.user_id = $1;
END;
$$ LANGUAGE plpgsql;

-- Fonction avec SETOF
CREATE FUNCTION get_numbers(n INT)
RETURNS SETOF INT AS $$
BEGIN
    FOR i IN 1..n LOOP
        RETURN NEXT i;
    END LOOP;
    RETURN;
END;
$$ LANGUAGE plpgsql;

-- Fonction SQL simple
CREATE FUNCTION square(n INT)
RETURNS INT AS $$
    SELECT n * n;
$$ LANGUAGE sql IMMUTABLE;

-- Types de volatilité
-- VOLATILE : valeur peut changer (défaut)
-- STABLE : valeur constante dans une transaction
-- IMMUTABLE : valeur toujours identique pour les mêmes paramètres

-- Fonction avec gestion d'erreurs
CREATE FUNCTION safe_divide(a DECIMAL, b DECIMAL)
RETURNS DECIMAL AS $$
BEGIN
    IF b = 0 THEN
        RAISE EXCEPTION 'Division par zéro';
    END IF;
    RETURN a / b;
EXCEPTION
    WHEN division_by_zero THEN
        RAISE NOTICE 'Tentative de division par zéro';
        RETURN NULL;
END;
$$ LANGUAGE plpgsql;

-- Supprimer une fonction
DROP FUNCTION add_numbers;
DROP FUNCTION add_numbers(INT, INT);  -- Si surchargée

-- Lister les fonctions
\df
SELECT * FROM pg_proc WHERE proname = 'add_numbers';
```

### Procédures (PostgreSQL 11+)
```sql
-- Créer une procédure
CREATE PROCEDURE transfer_funds(
    from_account INT,
    to_account INT,
    amount DECIMAL
)
LANGUAGE plpgsql
AS $$
BEGIN
    UPDATE accounts SET balance = balance - amount WHERE id = from_account;
    UPDATE accounts SET balance = balance + amount WHERE id = to_account;
    COMMIT;
END;
$$;

-- Appeler une procédure
CALL transfer_funds(1, 2, 100.00);

-- Procédure avec transaction
CREATE PROCEDURE cleanup_old_data()
LANGUAGE plpgsql
AS $$
BEGIN
    DELETE FROM logs WHERE created_at < NOW() - INTERVAL '90 days';
    DELETE FROM temp_data WHERE created_at < NOW() - INTERVAL '7 days';
    COMMIT;
END;
$$;

-- Supprimer une procédure
DROP PROCEDURE transfer_funds;
```

---

## Triggers

```sql
-- Fonction trigger
CREATE OR REPLACE FUNCTION update_modified_timestamp()
RETURNS TRIGGER AS $$
BEGIN
    NEW.updated_at = NOW();
    RETURN NEW;
END;
$$ LANGUAGE plpgsql;

-- Créer un trigger
CREATE TRIGGER set_timestamp
BEFORE UPDATE ON users
FOR EACH ROW
EXECUTE FUNCTION update_modified_timestamp();

-- Trigger AFTER
CREATE TRIGGER log_user_deletion
AFTER DELETE ON users
FOR EACH ROW
EXECUTE FUNCTION log_deletion();

-- Trigger avec condition
CREATE TRIGGER check_stock
BEFORE INSERT OR UPDATE ON orders
FOR EACH ROW
WHEN (NEW.quantity > 0)
EXECUTE FUNCTION verify_stock();

-- Trigger sur événement
CREATE TRIGGER audit_changes
AFTER INSERT OR UPDATE OR DELETE ON products
FOR EACH ROW
EXECUTE FUNCTION audit_log();

-- Fonction trigger avec OLD et NEW
CREATE OR REPLACE FUNCTION track_price_changes()
RETURNS TRIGGER AS $$
BEGIN
    IF OLD.price IS DISTINCT FROM NEW.price THEN
        INSERT INTO price_history (product_id, old_price, new_price, changed_at)
        VALUES (NEW.id, OLD.price, NEW.price, NOW());
    END IF;
    RETURN NEW;
END;
$$ LANGUAGE plpgsql;

-- Trigger INSTEAD OF (pour vues)
CREATE OR REPLACE FUNCTION update_user_view()
RETURNS TRIGGER AS $$
BEGIN
    UPDATE users SET nom = NEW.nom WHERE id = NEW.id;
    RETURN NEW;
END;
$$ LANGUAGE plpgsql;

CREATE TRIGGER update_via_view
INSTEAD OF UPDATE ON user_view
FOR EACH ROW
EXECUTE FUNCTION update_user_view();

-- Désactiver/Activer un trigger
ALTER TABLE users DISABLE TRIGGER set_timestamp;
ALTER TABLE users ENABLE TRIGGER set_timestamp;
ALTER TABLE users DISABLE TRIGGER ALL;
ALTER TABLE users ENABLE TRIGGER ALL;

-- Supprimer un trigger
DROP TRIGGER set_timestamp ON users;

-- Lister les triggers
\dS
SELECT * FROM pg_trigger WHERE tgrelid = 'users'::regclass;
```

---

## JSON et JSONB

```sql
-- Créer une table avec JSON
CREATE TABLE products (
    id SERIAL PRIMARY KEY,
    name TEXT,
    attributes JSONB
);

-- Insérer des données JSON
INSERT INTO products (name, attributes) VALUES
    ('Laptop', '{"brand": "Dell", "ram": 16, "storage": 512}'),
    ('Phone', '{"brand": "Apple", "color": "black", "storage": 128}');

-- Accéder aux champs JSON
SELECT
    name,
    attributes->>'brand' as brand,           -- Texte
    attributes->'storage' as storage,         -- JSON
    (attributes->>'storage')::INT as storage_int  -- Converti en INT
FROM products;

-- Opérateurs JSON
SELECT * FROM products WHERE attributes->>'brand' = 'Dell';
SELECT * FROM products WHERE attributes @> '{"brand": "Apple"}';
SELECT * FROM products WHERE attributes ? 'color';  -- Clé existe
SELECT * FROM products WHERE attributes ?| ARRAY['color', 'ram'];  -- OR
SELECT * FROM products WHERE attributes ?& ARRAY['brand', 'storage'];  -- AND

-- Fonctions JSON
SELECT
    jsonb_object_keys(attributes) as cles,
    jsonb_each(attributes) as paires,
    jsonb_array_length(tags) as nb_tags
FROM products;

-- Modifier du JSON
UPDATE products
SET attributes = attributes || '{"warranty": "2 years"}'
WHERE id = 1;

UPDATE products
SET attributes = attributes - 'color'
WHERE id = 2;

UPDATE products
SET attributes = jsonb_set(attributes, '{storage}', '256')
WHERE id = 1;

-- Construire du JSON
SELECT
    json_build_object(
        'id', id,
        'name', name,
        'attributes', attributes
    ) as product_json
FROM products;

SELECT json_agg(name) as all_names FROM products;

-- Path JSON
SELECT
    attributes #> '{specs, cpu}' as cpu,
    attributes #>> '{specs, cpu}' as cpu_text
FROM products;

-- Index sur JSONB
CREATE INDEX idx_attributes ON products USING GIN (attributes);
CREATE INDEX idx_brand ON products ((attributes->>'brand'));

-- Recherche full-text dans JSON
SELECT * FROM products
WHERE to_tsvector('english', attributes::text) @@ to_tsquery('Dell');
```

---

## Full-Text Search

```sql
-- Créer un vecteur de recherche
SELECT to_tsvector('english', 'The quick brown fox jumps over the lazy dog');

-- Créer une requête de recherche
SELECT to_tsquery('english', 'fox & dog');

-- Recherche basique
SELECT * FROM articles
WHERE to_tsvector('french', title || ' ' || content) @@ to_tsquery('french', 'postgresql');

-- Avec classement
SELECT
    title,
    ts_rank(to_tsvector('french', content), query) as rank
FROM articles, to_tsquery('french', 'postgresql & bases') query
WHERE to_tsvector('french', content) @@ query
ORDER BY rank DESC;

-- Colonne calculée avec index
ALTER TABLE articles
ADD COLUMN textsearch tsvector
GENERATED ALWAYS AS (to_tsvector('french', title || ' ' || content)) STORED;

CREATE INDEX idx_articles_textsearch ON articles USING GIN (textsearch);

SELECT * FROM articles
WHERE textsearch @@ to_tsquery('french', 'postgresql');

-- Trigger pour mise à jour automatique
CREATE FUNCTION articles_search_update() RETURNS trigger AS $$
BEGIN
    NEW.textsearch :=
        setweight(to_tsvector('french', coalesce(NEW.title,'')), 'A') ||
        setweight(to_tsvector('french', coalesce(NEW.content,'')), 'B');
    RETURN NEW;
END;
$$ LANGUAGE plpgsql;

CREATE TRIGGER tsvector_update
BEFORE INSERT OR UPDATE ON articles
FOR EACH ROW
EXECUTE FUNCTION articles_search_update();

-- Recherche avec préfixe
SELECT * FROM articles
WHERE textsearch @@ to_tsquery('french', 'postg:*');

-- Recherche avec phrase
SELECT * FROM articles
WHERE textsearch @@ phraseto_tsquery('french', 'base de données');

-- Highlight des résultats
SELECT
    title,
    ts_headline('french', content, to_tsquery('french', 'postgresql'))
FROM articles
WHERE textsearch @@ to_tsquery('french', 'postgresql');

-- Configuration personnalisée
CREATE TEXT SEARCH CONFIGURATION french_custom (COPY = french);
ALTER TEXT SEARCH CONFIGURATION french_custom
    ALTER MAPPING FOR word WITH french_stem;
```

---

## Partitionnement

### Partitionnement par plage (RANGE)
```sql
-- Table parent
CREATE TABLE logs (
    id SERIAL,
    user_id INT,
    action TEXT,
    created_at TIMESTAMP NOT NULL
) PARTITION BY RANGE (created_at);

-- Partitions
CREATE TABLE logs_2023 PARTITION OF logs
    FOR VALUES FROM ('2023-01-01') TO ('2024-01-01');

CREATE TABLE logs_2024 PARTITION OF logs
    FOR VALUES FROM ('2024-01-01') TO ('2025-01-01');

CREATE TABLE logs_2025 PARTITION OF logs
    FOR VALUES FROM ('2025-01-01') TO ('2026-01-01');

-- Partition par défaut (PostgreSQL 11+)
CREATE TABLE logs_default PARTITION OF logs DEFAULT;

-- Insertion (automatiquement routée vers la bonne partition)
INSERT INTO logs (user_id, action, created_at)
VALUES (1, 'login', '2024-05-15 10:30:00');
```

### Partitionnement par liste (LIST)
```sql
-- Table parent
CREATE TABLE orders (
    id SERIAL,
    customer_id INT,
    country VARCHAR(2) NOT NULL,
    amount DECIMAL
) PARTITION BY LIST (country);

-- Partitions
CREATE TABLE orders_eu PARTITION OF orders
    FOR VALUES IN ('FR', 'DE', 'IT', 'ES', 'BE');

CREATE TABLE orders_us PARTITION OF orders
    FOR VALUES IN ('US');

CREATE TABLE orders_asia PARTITION OF orders
    FOR VALUES IN ('JP', 'CN', 'IN');

CREATE TABLE orders_other PARTITION OF orders DEFAULT;
```

### Partitionnement par hash (HASH)
```sql
-- Table parent
CREATE TABLE users (
    id SERIAL,
    email VARCHAR(255) NOT NULL,
    name VARCHAR(100)
) PARTITION BY HASH (id);

-- Partitions (nombre de partitions)
CREATE TABLE users_0 PARTITION OF users
    FOR VALUES WITH (MODULUS 4, REMAINDER 0);

CREATE TABLE users_1 PARTITION OF users
    FOR VALUES WITH (MODULUS 4, REMAINDER 1);

CREATE TABLE users_2 PARTITION OF users
    FOR VALUES WITH (MODULUS 4, REMAINDER 2);

CREATE TABLE users_3 PARTITION OF users
    FOR VALUES WITH (MODULUS 4, REMAINDER 3);
```

### Sous-partitionnement
```sql
-- Partitionnement par année puis par mois
CREATE TABLE sales (
    id SERIAL,
    amount DECIMAL,
    sale_date DATE NOT NULL
) PARTITION BY RANGE (EXTRACT(YEAR FROM sale_date));

CREATE TABLE sales_2024 PARTITION OF sales
    FOR VALUES FROM (2024) TO (2025)
    PARTITION BY RANGE (EXTRACT(MONTH FROM sale_date));

CREATE TABLE sales_2024_01 PARTITION OF sales_2024
    FOR VALUES FROM (1) TO (2);

CREATE TABLE sales_2024_02 PARTITION OF sales_2024
    FOR VALUES FROM (2) TO (3);
-- etc.
```

### Gestion des partitions
```sql
-- Attacher une partition existante
ALTER TABLE logs ATTACH PARTITION logs_2026
    FOR VALUES FROM ('2026-01-01') TO ('2027-01-01');

-- Détacher une partition
ALTER TABLE logs DETACH PARTITION logs_2023;

-- Supprimer une partition
DROP TABLE logs_2023;

-- Lister les partitions
SELECT
    parent.relname as parent_table,
    child.relname as partition_name
FROM pg_inherits
JOIN pg_class parent ON pg_inherits.inhparent = parent.oid
JOIN pg_class child ON pg_inherits.inhrelid = child.oid
WHERE parent.relname = 'logs';
```

---

## Sécurité et Permissions

### Gestion des rôles et utilisateurs
```sql
-- Créer un utilisateur
CREATE USER john WITH PASSWORD 'secret123';
CREATE ROLE readonly;

-- Modifier un utilisateur
ALTER USER john WITH PASSWORD 'new_secret';
ALTER USER john VALID UNTIL '2025-12-31';
ALTER USER john CREATEDB;

-- Créer un rôle avec options
CREATE ROLE admin WITH
    LOGIN
    PASSWORD 'admin_pass'
    SUPERUSER
    CREATEDB
    CREATEROLE
    REPLICATION;

-- Rôle de groupe
CREATE ROLE developers;
GRANT developers TO john, jane, bob;

-- Supprimer un utilisateur/rôle
DROP USER john;
DROP ROLE readonly;

-- Lister les rôles
\du
SELECT * FROM pg_roles;
```

### Permissions (GRANT/REVOKE)
```sql
-- Permissions sur base de données
GRANT CONNECT ON DATABASE ma_base TO john;
GRANT CREATE ON DATABASE ma_base TO john;
REVOKE CONNECT ON DATABASE ma_base FROM john;

-- Permissions sur schéma
GRANT USAGE ON SCHEMA public TO john;
GRANT CREATE ON SCHEMA public TO john;
GRANT ALL ON SCHEMA public TO admin;

-- Permissions sur tables
GRANT SELECT ON users TO readonly;
GRANT SELECT, INSERT, UPDATE ON users TO john;
GRANT ALL PRIVILEGES ON users TO admin;
REVOKE INSERT ON users FROM john;

-- Permissions sur toutes les tables d'un schéma
GRANT SELECT ON ALL TABLES IN SCHEMA public TO readonly;
GRANT ALL ON ALL TABLES IN SCHEMA public TO admin;

-- Permissions par défaut pour les futures tables
ALTER DEFAULT PRIVILEGES IN SCHEMA public
    GRANT SELECT ON TABLES TO readonly;

ALTER DEFAULT PRIVILEGES IN SCHEMA public
    GRANT ALL ON TABLES TO admin;

-- Permissions sur colonnes spécifiques
GRANT SELECT (id, nom, email) ON users TO john;
GRANT UPDATE (email) ON users TO john;

-- Permissions sur séquences
GRANT USAGE, SELECT ON ALL SEQUENCES IN SCHEMA public TO john;

-- Permissions sur fonctions
GRANT EXECUTE ON FUNCTION calculate_total TO john;
GRANT EXECUTE ON ALL FUNCTIONS IN SCHEMA public TO developers;

-- Row Level Security (RLS)
ALTER TABLE documents ENABLE ROW LEVEL SECURITY;

CREATE POLICY user_documents ON documents
    FOR ALL
    TO public
    USING (user_id = current_user_id());

CREATE POLICY admin_all ON documents
    FOR ALL
    TO admin
    USING (true);

-- Voir les permissions
\dp users
SELECT * FROM information_schema.table_privileges WHERE table_name = 'users';
```

### Sécurité avancée
```sql
-- Chiffrement de colonne
CREATE EXTENSION pgcrypto;

-- Stocker un mot de passe haché
INSERT INTO users (email, password)
VALUES ('user@mail.com', crypt('password123', gen_salt('bf')));

-- Vérifier un mot de passe
SELECT * FROM users
WHERE email = 'user@mail.com'
AND password = crypt('password123', password);

-- Chiffrement de données
INSERT INTO secrets (data)
VALUES (pgp_sym_encrypt('données sensibles', 'clé_secrète'));

SELECT pgp_sym_decrypt(data, 'clé_secrète') FROM secrets;

-- Audit trail
CREATE TABLE audit_log (
    id SERIAL PRIMARY KEY,
    table_name TEXT,
    operation TEXT,
    old_data JSONB,
    new_data JSONB,
    user_name TEXT DEFAULT CURRENT_USER,
    timestamp TIMESTAMP DEFAULT NOW()
);

-- Fonction d'audit
CREATE OR REPLACE FUNCTION audit_trigger()
RETURNS TRIGGER AS $$
BEGIN
    INSERT INTO audit_log (table_name, operation, old_data, new_data)
    VALUES (TG_TABLE_NAME, TG_OP, to_jsonb(OLD), to_jsonb(NEW));
    RETURN NEW;
END;
$$ LANGUAGE plpgsql;

-- Connection limits
ALTER USER john CONNECTION LIMIT 10;
```

---

## Nouveautés Récentes

### PostgreSQL 17 (2024)
```sql
-- Amélioration des performances VACUUM
-- VACUUM plus rapide pour les grandes tables

-- Nouvelle fonction JSON_TABLE
SELECT *
FROM JSON_TABLE(
    '[{"name": "John", "age": 30}, {"name": "Jane", "age": 25}]',
    '$[*]' COLUMNS (
        name TEXT PATH '$.name',
        age INT PATH '$.age'
    )
) AS jt;

-- Meilleure compression avec LZ4
CREATE TABLE compressed_data (data TEXT)
WITH (toast_compression = lz4);

-- Amélioration des index BRIN
CREATE INDEX idx_brin ON large_table USING BRIN (timestamp)
WITH (pages_per_range = 32);

-- MERGE amélioré
MERGE INTO inventory t
USING new_stock s ON t.product_id = s.product_id
WHEN MATCHED THEN
    UPDATE SET quantity = t.quantity + s.quantity
WHEN NOT MATCHED THEN
    INSERT (product_id, quantity) VALUES (s.product_id, s.quantity);
```

### PostgreSQL 16 (2023)
```sql
-- COPY avec WHERE
COPY users FROM '/data/users.csv'
WHERE created_at > '2024-01-01';

-- Amélioration des performances de la réplication logique
-- Plus rapide et moins de surcharge

-- Monitoring amélioré
SELECT * FROM pg_stat_io;  -- Statistiques I/O détaillées

-- ANY_VALUE (pour GROUP BY)
SELECT department, ANY_VALUE(manager_name)
FROM employees
GROUP BY department;
```

### PostgreSQL 15 (2022)
```sql
-- MERGE (UPSERT avancé)
MERGE INTO products p
USING new_products n ON p.id = n.id
WHEN MATCHED THEN
    UPDATE SET price = n.price, stock = n.stock
WHEN NOT MATCHED THEN
    INSERT VALUES (n.id, n.name, n.price, n.stock);

-- Amélioration des permissions publiques
-- Schéma public plus sécurisé par défaut

-- Compression LZ4/ZSTD pour TOAST
ALTER TABLE big_table SET (toast_compression = 'lz4');

-- ICU collations par défaut
CREATE DATABASE ma_base
    LOCALE_PROVIDER = icu
    ICU_LOCALE = 'fr-FR-x-icu';
```

### PostgreSQL 14 (2021)
```sql
-- Multi-range types
CREATE TABLE reservations (
    room_id INT,
    reserved_periods INT4MULTIRANGE
);

INSERT INTO reservations VALUES
    (101, '{[2024-01-01, 2024-01-05), [2024-01-10, 2024-01-15)}');

-- Pipeline de requêtes
-- Envoyer plusieurs requêtes sans attendre les résultats

-- Amélioration VACUUM
-- VACUUM automatique plus intelligent

-- Stored procedures avec OUT parameters
CREATE PROCEDURE get_stats(IN dept_id INT, OUT emp_count INT, OUT avg_salary DECIMAL)
AS $$
BEGIN
    SELECT COUNT(*), AVG(salary)
    INTO emp_count, avg_salary
    FROM employees
    WHERE department_id = dept_id;
END;
$$ LANGUAGE plpgsql;
```

---

## Window Functions

```sql
-- ROW_NUMBER : Numéro de ligne
SELECT name, salary,
    ROW_NUMBER() OVER (ORDER BY salary DESC) as rang
FROM employees;

-- RANK : Rang avec ex-aequo
SELECT name, salary,
    RANK() OVER (ORDER BY salary DESC) as rang
FROM employees;

-- DENSE_RANK : Rang dense
SELECT name, salary,
    DENSE_RANK() OVER (ORDER BY salary DESC) as rang
FROM employees;

-- NTILE : Diviser en groupes
SELECT name, salary,
    NTILE(4) OVER (ORDER BY salary) as quartile
FROM employees;

-- LAG/LEAD : Valeur précédente/suivante
SELECT date, sales,
    LAG(sales, 1) OVER (ORDER BY date) as prev_sales,
    LEAD(sales, 1) OVER (ORDER BY date) as next_sales
FROM daily_sales;

-- FIRST_VALUE/LAST_VALUE : Première/dernière valeur
SELECT name, department, salary,
    FIRST_VALUE(name) OVER (PARTITION BY department ORDER BY salary DESC) as top_earner,
    LAST_VALUE(name) OVER (PARTITION BY department ORDER BY salary DESC
        ROWS BETWEEN UNBOUNDED PRECEDING AND UNBOUNDED FOLLOWING) as lowest_earner
FROM employees;

-- Agrégations window
SELECT name, department, salary,
    AVG(salary) OVER (PARTITION BY department) as dept_avg,
    SUM(salary) OVER (PARTITION BY department) as dept_total,
    COUNT(*) OVER (PARTITION BY department) as dept_count
FROM employees;

-- Frames (cadres de fenêtre)
SELECT date, amount,
    -- 7 derniers jours
    SUM(amount) OVER (
        ORDER BY date
        ROWS BETWEEN 6 PRECEDING AND CURRENT ROW
    ) as rolling_7day,
    
    -- Mois en cours
    SUM(amount) OVER (
        ORDER BY date
        RANGE BETWEEN INTERVAL '1 month' PRECEDING AND CURRENT ROW
    ) as monthly_total
FROM sales;

-- CUME_DIST : Distribution cumulative
SELECT name, salary,
    CUME_DIST() OVER (ORDER BY salary) as percentile
FROM employees;

-- PERCENT_RANK : Rang en pourcentage
SELECT name, salary,
    PERCENT_RANK() OVER (ORDER BY salary) as pct_rank
FROM employees;

-- Exemple complet : Top 3 par département
SELECT * FROM (
    SELECT
        department,
        name,
        salary,
        ROW_NUMBER() OVER (PARTITION BY department ORDER BY salary DESC) as dept_rank
    FROM employees
) ranked
WHERE dept_rank <= 3;
```

---

## Backup et Restore

### pg_dump
```bash
# Dump d'une base complète
pg_dump -U postgres ma_base > backup.sql

# Dump avec options
pg_dump -U postgres -h localhost -p 5432 ma_base \
    --format=custom \
    --compress=9 \
    --verbose \
    --file=backup.dump

# Dump en format directory (parallélisable)
pg_dump -U postgres ma_base -Fd -f backup_dir/ -j 4

# Dump d'une table spécifique
pg_dump -U postgres ma_base -t users > users_backup.sql

# Dump de plusieurs tables
pg_dump -U postgres ma_base -t users -t orders > backup.sql

# Dump avec schéma uniquement
pg_dump -U postgres ma_base --schema-only > schema.sql

# Dump avec données uniquement
pg_dump -U postgres ma_base --data-only > data.sql

# Exclure certaines tables
pg_dump -U postgres ma_base --exclude-table=logs > backup.sql
```

### pg_restore
```bash
# Restore d'un dump custom/directory
pg_restore -U postgres -d ma_base backup.dump

# Restore avec options
pg_restore -U postgres -h localhost -d ma_base \
    --verbose \
    --clean \
    --if-exists \
    backup.dump

# Restore en parallèle
pg_restore -U postgres -d ma_base -Fd -j 4 backup_dir/

# Restore d'une table spécifique
pg_restore -U postgres -d ma_base -t users backup.dump

# Restore vers une nouvelle base
createdb -U postgres nouvelle_base
pg_restore -U postgres -d nouvelle_base backup.dump

# Restore avec liste
pg_restore -U postgres -d ma_base -L backup.list backup.dump
```

### pg_dumpall
```bash
# Dump de toutes les bases
pg_dumpall -U postgres > all_databases.sql

# Dump des rôles et tablespaces uniquement
pg_dumpall -U postgres --roles-only > roles.sql
pg_dumpall -U postgres --tablespaces-only > tablespaces.sql

# Restore
psql -U postgres < all_databases.sql
```

### Backup continu (PITR)
```sql
-- Configuration dans postgresql.conf
wal_level = replica
archive_mode = on
archive_command = 'cp %p /backup/archives/%f'

-- Créer un backup de base
SELECT pg_start_backup('daily_backup', true);
-- Copier les fichiers data/
SELECT pg_stop_backup();

-- Point-in-time recovery
-- Dans recovery.conf ou postgresql.auto.conf
restore_command = 'cp /backup/archives/%f %p'
recovery_target_time = '2024-06-15 10:30:00'
```

### Réplication en streaming
```bash
# Sur le serveur primary : postgresql.conf
wal_level = replica
max_wal_senders = 10
wal_keep_size = 1GB

# Créer un rôle de réplication
CREATE ROLE replicator WITH REPLICATION LOGIN PASSWORD 'password';

# Sur le serveur standby
pg_basebackup -h primary_host -D /var/lib/postgresql/data -U replicator -P

# standby.signal
echo "standby_mode = 'on'" > standby.signal

# postgresql.auto.conf
primary_conninfo = 'host=primary_host port=5432 user=replicator password=password'
```

---

## Monitoring et Maintenance

### Statistiques système
```sql
-- Statistiques sur les bases de données
SELECT
    datname,
    numbackends as connections,
    xact_commit as commits,
    xact_rollback as rollbacks,
    blks_read as disk_reads,
    blks_hit as cache_hits,
    pg_size_pretty(pg_database_size(datname)) as size
FROM pg_stat_database;

-- Statistiques sur les tables
SELECT
    schemaname,
    tablename,
    seq_scan,
    seq_tup_read,
    idx_scan,
    idx_tup_fetch,
    n_tup_ins,
    n_tup_upd,
    n_tup_del,
    pg_size_pretty(pg_total_relation_size(schemaname||'.'||tablename)) as total_size
FROM pg_stat_user_tables
ORDER BY pg_total_relation_size(schemaname||'.'||tablename) DESC;

-- Statistiques sur les index
SELECT
    schemaname,
    tablename,
    indexname,
    idx_scan,
    idx_tup_read,
    idx_tup_fetch,
    pg_size_pretty(pg_relation_size(indexrelid)) as size
FROM pg_stat_user_indexes
ORDER BY idx_scan;

-- Connexions actives
SELECT
    pid,
    usename,
    application_name,
    client_addr,
    state,
    query_start,
    state_change,
    query
FROM pg_stat_activity
WHERE state != 'idle'
ORDER BY query_start;

-- Tuer une connexion
SELECT pg_terminate_backend(pid) FROM pg_stat_activity WHERE pid = 12345;

-- Locks
SELECT
    locktype,
    relation::regclass,
    mode,
    granted,
    pid,
    query
FROM pg_locks
JOIN pg_stat_activity USING (pid);

-- Bloat (gonflement des tables)
SELECT
    schemaname,
    tablename,
    pg_size_pretty(pg_total_relation_size(schemaname||'.'||tablename)) as size,
    round((CASE WHEN pg_total_relation_size(schemaname||'.'||tablename) = 0 THEN 0
        ELSE (pg_total_relation_size(schemaname||'.'||tablename) - 
              pg_relation_size(schemaname||'.'||tablename))::numeric / 
              pg_total_relation_size(schemaname||'.'||tablename) * 100
    END), 2) as bloat_pct
FROM pg_stat_user_tables
ORDER BY pg_total_relation_size(schemaname||'.'||tablename) DESC;

-- Cache hit ratio
SELECT
    'cache hit ratio' as metric,
    round(sum(blks_hit) / nullif(sum(blks_hit + blks_read), 0) * 100, 2) as value
FROM pg_stat_database;

-- Requêtes lentes (nécessite pg_stat_statements)
SELECT
    query,
    calls,
    round(total_exec_time::numeric / calls, 2) as avg_time,
    round(total_exec_time::numeric, 2) as total_time,
    round((100 * total_exec_time / sum(total_exec_time) OVER ())::numeric, 2) as pct
FROM pg_stat_statements
ORDER BY total_exec_time DESC
LIMIT 20;
```

### Maintenance
```sql
-- VACUUM : Nettoyer l'espace mort
VACUUM users;
VACUUM VERBOSE users;
VACUUM ANALYZE users;
VACUUM FULL users;  -- Plus intensif, bloque la table

-- ANALYZE : Mettre à jour les statistiques
ANALYZE users;
ANALYZE;  -- Toutes les tables

-- REINDEX : Reconstruire les index
REINDEX TABLE users;
REINDEX INDEX idx_users_email;
REINDEX DATABASE ma_base;

-- Autovacuum : Vérifier la configuration
SHOW autovacuum;
SELECT * FROM pg_stat_user_tables WHERE last_autovacuum IS NOT NULL;

-- Taille des tables
SELECT
    schemaname,
    tablename,
    pg_size_pretty(pg_table_size(schemaname||'.'||tablename)) as table_size,
    pg_size_pretty(pg_indexes_size(schemaname||'.'||tablename)) as indexes_size,
    pg_size_pretty(pg_total_relation_size(schemaname||'.'||tablename)) as total_size
FROM pg_tables
WHERE schemaname = 'public'
ORDER BY pg_total_relation_size(schemaname||'.'||tablename) DESC;

-- Taille de la base
SELECT
    pg_size_pretty(pg_database_size(current_database())) as db_size;

-- Vérifier la santé de la base
SELECT
    datname,
    age(datfrozenxid) as xid_age,
    pg_size_pretty(pg_database_size(datname)) as size
FROM pg_database
ORDER BY age(datfrozenxid) DESC;
```

---

## Configuration PostgreSQL

### Fichiers de configuration
```ini
# postgresql.conf : Configuration principale

# Connexions
max_connections = 100
superuser_reserved_connections = 3

# Mémoire
shared_buffers = 256MB              # 25% de RAM
effective_cache_size = 1GB          # 50-75% de RAM
work_mem = 4MB                      # Par opération
maintenance_work_mem = 64MB         # Pour VACUUM, CREATE INDEX
temp_buffers = 8MB

# WAL
wal_level = replica
wal_buffers = 16MB
checkpoint_timeout = 5min
checkpoint_completion_target = 0.9
max_wal_size = 1GB

# Query tuning
random_page_cost = 1.1             # Pour SSD
effective_io_concurrency = 200     # Pour SSD
default_statistics_target = 100

# Logging
logging_collector = on
log_directory = 'log'
log_filename = 'postgresql-%Y-%m-%d.log'
log_rotation_age = 1d
log_rotation_size = 10MB
log_min_duration_statement = 1000  # Log queries > 1s
log_line_prefix = '%t [%p]: [%l-1] user=%u,db=%d,app=%a,client=%h '
log_checkpoints = on
log_connections = on
log_disconnections = on
log_lock_waits = on

# Autovacuum
autovacuum = on
autovacuum_max_workers = 3
autovacuum_naptime = 1min
```

### pg_hba.conf : Authentification
```ini
# TYPE  DATABASE        USER            ADDRESS                 METHOD

# Local connections
local   all             all                                     peer
local   all             postgres                                peer

# IPv4 local connections:
host    all             all             127.0.0.1/32            scram-sha-256

# IPv6 local connections:
host    all             all             ::1/128                 scram-sha-256

# Réseau local
host    all             all             192.168.1.0/24          scram-sha-256

# Connexions SSL uniquement
hostssl all             all             0.0.0.0/0               scram-sha-256

# Méthodes d'authentification :
# - trust : Aucune authentification
# - peer : Utilisateur système
# - password : Mot de passe en clair (à éviter)
# - md5 : Mot de passe hashé MD5 (déprécié)
# - scram-sha-256 : Recommandé
# - cert : Certificat SSL
```

### Commandes de configuration
```sql
-- Voir une configuration
SHOW max_connections;
SHOW ALL;

-- Modifier une configuration (session)
SET work_mem = '16MB';
SET enable_seqscan = off;

-- Modifier une configuration (global, nécessite reload)
ALTER SYSTEM SET shared_buffers = '512MB';
SELECT pg_reload_conf();

-- Réinitialiser
RESET work_mem;
RESET ALL;

-- Voir les configurations modifiées
SELECT name, setting, unit, context
FROM pg_settings
WHERE source != 'default'
ORDER BY name;
```

---

## Réplication

### Réplication logique
```sql
-- Sur le serveur primaire
-- Configuration
ALTER SYSTEM SET wal_level = logical;
SELECT pg_reload_conf();

-- Créer une publication
CREATE PUBLICATION my_publication FOR ALL TABLES;
CREATE PUBLICATION my_pub FOR TABLE users, orders;
CREATE PUBLICATION my_pub FOR TABLE users WHERE (active = true);

-- Sur le serveur secondaire
-- Créer un abonnement
CREATE SUBSCRIPTION my_subscription
    CONNECTION 'host=primary_host dbname=mydb user=replicator password=pass'
    PUBLICATION my_publication;

-- Gérer les publications
\dRp+  -- Lister
ALTER PUBLICATION my_pub ADD TABLE products;
ALTER PUBLICATION my_pub DROP TABLE users;
DROP PUBLICATION my_pub;

-- Gérer les abonnements
\dRs+  -- Lister
ALTER SUBSCRIPTION my_sub ENABLE;
ALTER SUBSCRIPTION my_sub DISABLE;
ALTER SUBSCRIPTION my_sub REFRESH PUBLICATION;
DROP SUBSCRIPTION my_sub;

-- Monitoring
SELECT * FROM pg_stat_replication;
SELECT * FROM pg_replication_slots;
```

### Réplication physique (streaming)
```sql
-- Sur le primaire : postgresql.conf
wal_level = replica
max_wal_senders = 10
wal_keep_size = 1GB
hot_standby = on

-- Créer un slot de réplication
SELECT * FROM pg_create_physical_replication_slot('standby_slot');

-- Sur le standby
-- Créer la base via pg_basebackup
pg_basebackup -h primary -D /var/lib/postgresql/data -U replicator -P -R

-- postgresql.auto.conf (créé par -R)
primary_conninfo = 'host=primary port=5432 user=replicator'
primary_slot_name = 'standby_slot'

-- Promouvoir un standby en primaire
SELECT pg_promote();

-- Monitoring
SELECT * FROM pg_stat_wal_receiver;
SELECT * FROM pg_stat_replication;
```

---

## Arrays et Fonctions

### Arrays
```sql
-- Créer un array
SELECT ARRAY[1, 2, 3, 4, 5];
SELECT '{1,2,3,4,5}'::INT[];

-- Accéder aux éléments
SELECT ARRAY[1,2,3,4,5][1];  -- Retourne 1 (1-indexed!)
SELECT ARRAY[1,2,3,4,5][2:4];  -- Retourne {2,3,4}

-- Opérations sur arrays
SELECT ARRAY[1,2,3] || ARRAY[4,5,6];  -- Concaténation
SELECT ARRAY[1,2,3] || 4;  -- Ajouter un élément
SELECT array_append(ARRAY[1,2,3], 4);
SELECT array_prepend(0, ARRAY[1,2,3]);
SELECT array_remove(ARRAY[1,2,3,2], 2);  -- Retourne {1,3}
SELECT array_replace(ARRAY[1,2,3,2], 2, 99);

-- Fonctions d'arrays
SELECT array_length(ARRAY[1,2,3,4], 1);  -- 4
SELECT array_upper(ARRAY[1,2,3], 1);  -- 3
SELECT array_lower(ARRAY[1,2,3], 1);  -- 1
SELECT cardinality(ARRAY[1,2,3]);  -- 3 (PostgreSQL 9.4+)

-- Recherche dans arrays
SELECT 2 = ANY(ARRAY[1,2,3]);  -- true
SELECT 4 = ALL(ARRAY[4,4,4]);  -- true
SELECT ARRAY[1,2] <@ ARRAY[1,2,3];  -- true (contenu dans)
SELECT ARRAY[1,2,3] @> ARRAY[2];  -- true (contient)
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

## Extensions Populaires

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

## Astuces et Best Practices

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

## Requêtes Avancées Exemples

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

## Ressources Supplémentaires

- Documentation officielle : https://www.postgresql.org/docs/
- Wiki PostgreSQL : https://wiki.postgresql.org/
- PostgreSQL Tutorial : https://www.postgresqltutorial.com/
- Explain Visualizer : https://explain.dalibo.com/
- pgAdmin : Interface graphique officielle
- DBeaver : Client SQL multi-plateformes

---

**Version PostgreSQL couverte : 17.x (2024) avec compatibilité 14-16**

*Mis à jour avec les dernières fonctionnalités et bonnes pratiques*
