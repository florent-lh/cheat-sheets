# Cheat Sheet MySQL Complet

## Table des Matières

1. [Installation et Configuration](#installation-et-configuration)
2. [Connexion et Déconnexion](#connexion-et-déconnexion)
3. [Bases de Données](#bases-de-données)
4. [Tables](#tables)
5. [Types de Données](#types-de-données)
6. [Contraintes](#contraintes)
7. [Index](#index)
8. [Insertion de Données](#insertion-de-données)
9. [Sélection de Données (SELECT)](#sélection-de-données-select)
10. [Mise à Jour de Données](#mise-à-jour-de-données)
11. [Suppression de Données](#suppression-de-données)
12. [Jointures](#jointures)
13. [Fonctions d'Agrégation](#fonctions-dagrégation)
14. [Sous-Requêtes](#sous-requêtes)
15. [Vues](#vues)
16. [Procédures Stockées](#procédures-stockées)
17. [Fonctions](#fonctions)
18. [Triggers](#triggers)
19. [Transactions](#transactions)
20. [Gestion des Utilisateurs](#gestion-des-utilisateurs)
21. [Sauvegarde et Restauration](#sauvegarde-et-restauration)
22. [Optimisation et Performance](#optimisation-et-performance)
23. [Fonctions de Chaînes](#fonctions-de-chaînes)
24. [Fonctions de Dates](#fonctions-de-dates)
25. [Fonctions Mathématiques](#fonctions-mathématiques)
26. [Expressions Régulières](#expressions-régulières)
27. [JSON](#json)
28. [Window Functions](#window-functions)
29. [CTE (Common Table Expressions)](#cte-common-table-expressions)
30. [Partitionnement](#partitionnement)
31. [Réplication](#réplication)
32. [Nouveautés MySQL 8.0+](#nouveautés-mysql-80)
33. [Dépannage](#dépannage)

---

## Installation et Configuration

### Installation (Ubuntu/Debian)
```bash
sudo apt update
sudo apt install mysql-server
sudo mysql_secure_installation
```

### Installation (macOS)
```bash
brew install mysql
brew services start mysql
```

### Vérifier la version
```sql
SELECT VERSION();
SHOW VARIABLES LIKE '%version%';
```

### Configuration de base
```bash
# Fichier de configuration
/etc/mysql/my.cnf  # Linux
/usr/local/etc/my.cnf  # macOS

# Variables importantes
[mysqld]
max_connections = 200
innodb_buffer_pool_size = 1G
character-set-server = utf8mb4
collation-server = utf8mb4_unicode_ci
```

---

## Connexion et Déconnexion

### Se connecter
```bash
mysql -u root -p
mysql -u username -p database_name
mysql -h hostname -u username -p database_name
mysql -u username -p -P 3306 -h localhost
```

### Se déconnecter
```sql
EXIT;
QUIT;
\q
```

### Afficher la connexion actuelle
```sql
SELECT USER();
SELECT DATABASE();
SHOW PROCESSLIST;
```

---

## Bases de Données

### Créer une base de données
```sql
CREATE DATABASE ma_base;
CREATE DATABASE IF NOT EXISTS ma_base;
CREATE DATABASE ma_base CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
```

### Lister les bases de données
```sql
SHOW DATABASES;
SHOW DATABASES LIKE 'ma%';
```

### Utiliser une base de données
```sql
USE ma_base;
```

### Supprimer une base de données
```sql
DROP DATABASE ma_base;
DROP DATABASE IF EXISTS ma_base;
```

### Informations sur une base
```sql
SHOW CREATE DATABASE ma_base;
SELECT SCHEMA_NAME FROM information_schema.SCHEMATA;
```

---

## Tables

### Créer une table
```sql
CREATE TABLE utilisateurs (
    id INT AUTO_INCREMENT PRIMARY KEY,
    nom VARCHAR(100) NOT NULL,
    email VARCHAR(255) UNIQUE NOT NULL,
    age INT,
    date_creation TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- Avec moteur de stockage spécifique
CREATE TABLE produits (
    id INT PRIMARY KEY,
    nom VARCHAR(100)
) ENGINE=InnoDB;
```

### Lister les tables
```sql
SHOW TABLES;
SHOW TABLES LIKE 'user%';
SHOW FULL TABLES;
```

### Décrire une table
```sql
DESCRIBE utilisateurs;
DESC utilisateurs;
SHOW COLUMNS FROM utilisateurs;
SHOW CREATE TABLE utilisateurs;
```

### Modifier une table
```sql
-- Ajouter une colonne
ALTER TABLE utilisateurs ADD COLUMN telephone VARCHAR(20);
ALTER TABLE utilisateurs ADD COLUMN adresse TEXT AFTER nom;

-- Modifier une colonne
ALTER TABLE utilisateurs MODIFY COLUMN nom VARCHAR(150);
ALTER TABLE utilisateurs CHANGE ancien_nom nouveau_nom VARCHAR(100);

-- Supprimer une colonne
ALTER TABLE utilisateurs DROP COLUMN telephone;

-- Renommer une table
ALTER TABLE utilisateurs RENAME TO users;
RENAME TABLE utilisateurs TO users;
```

### Supprimer une table
```sql
DROP TABLE utilisateurs;
DROP TABLE IF EXISTS utilisateurs;
TRUNCATE TABLE utilisateurs;  -- Vide la table mais conserve la structure
```

### Copier une table
```sql
-- Structure et données
CREATE TABLE users_backup AS SELECT * FROM utilisateurs;

-- Structure uniquement
CREATE TABLE users_copy LIKE utilisateurs;

-- Copier les données
INSERT INTO users_copy SELECT * FROM utilisateurs;
```

---

## Types de Données

### Types numériques
```sql
TINYINT          -- -128 à 127 (1 octet)
SMALLINT         -- -32768 à 32767 (2 octets)
MEDIUMINT        -- -8388608 à 8388607 (3 octets)
INT ou INTEGER   -- -2147483648 à 2147483647 (4 octets)
BIGINT           -- Très grands nombres (8 octets)
DECIMAL(M,D)     -- Nombres décimaux exacts
FLOAT            -- Nombres à virgule flottante
DOUBLE           -- Double précision
BIT(M)           -- Stockage de bits
```

### Types de chaînes
```sql
CHAR(M)          -- Chaîne de longueur fixe
VARCHAR(M)       -- Chaîne de longueur variable (jusqu'à 65535)
TINYTEXT         -- Texte court (255 caractères)
TEXT             -- Texte moyen (65535 caractères)
MEDIUMTEXT       -- Texte long (16 Mo)
LONGTEXT         -- Texte très long (4 Go)
BINARY(M)        -- Binaire de longueur fixe
VARBINARY(M)     -- Binaire de longueur variable
BLOB             -- Binary Large Object
ENUM('val1', 'val2')  -- Énumération
SET('val1', 'val2')   -- Ensemble de valeurs
```

### Types de dates
```sql
DATE             -- YYYY-MM-DD
TIME             -- HH:MM:SS
DATETIME         -- YYYY-MM-DD HH:MM:SS
TIMESTAMP        -- Timestamp Unix (1970-2038)
YEAR             -- Année sur 4 chiffres
```

### Types JSON (MySQL 5.7+)
```sql
JSON             -- Stockage natif JSON
```

### Types spatiaux
```sql
GEOMETRY
POINT
LINESTRING
POLYGON
```

---

## Contraintes

### Contraintes de base
```sql
CREATE TABLE commandes (
    id INT AUTO_INCREMENT,
    user_id INT NOT NULL,
    montant DECIMAL(10,2) DEFAULT 0.00,
    statut ENUM('pending', 'completed', 'cancelled') DEFAULT 'pending',
    email VARCHAR(255) UNIQUE,
    date_commande TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    
    PRIMARY KEY (id),
    FOREIGN KEY (user_id) REFERENCES utilisateurs(id) ON DELETE CASCADE,
    CHECK (montant >= 0),
    INDEX idx_statut (statut)
);
```

### Types de contraintes
```sql
-- Clé primaire
PRIMARY KEY (colonne)
PRIMARY KEY (col1, col2)  -- Clé primaire composite

-- Clé étrangère
FOREIGN KEY (colonne) REFERENCES autre_table(id)
FOREIGN KEY (colonne) REFERENCES autre_table(id) ON DELETE CASCADE
FOREIGN KEY (colonne) REFERENCES autre_table(id) ON UPDATE SET NULL

-- Unique
UNIQUE (colonne)
CONSTRAINT nom_contrainte UNIQUE (col1, col2)

-- Check (MySQL 8.0.16+)
CHECK (age >= 18)
CONSTRAINT age_check CHECK (age >= 18 AND age <= 120)

-- Not Null
colonne VARCHAR(100) NOT NULL

-- Default
colonne INT DEFAULT 0
date_creation TIMESTAMP DEFAULT CURRENT_TIMESTAMP
```

### Ajouter/Supprimer des contraintes
```sql
-- Ajouter
ALTER TABLE utilisateurs ADD CONSTRAINT uk_email UNIQUE (email);
ALTER TABLE commandes ADD CONSTRAINT fk_user 
    FOREIGN KEY (user_id) REFERENCES utilisateurs(id);
ALTER TABLE produits ADD CONSTRAINT chk_prix CHECK (prix > 0);

-- Supprimer
ALTER TABLE utilisateurs DROP CONSTRAINT uk_email;
ALTER TABLE commandes DROP FOREIGN KEY fk_user;
ALTER TABLE produits DROP CHECK chk_prix;
```

---

## Index

### Créer des index
```sql
-- Index simple
CREATE INDEX idx_nom ON utilisateurs(nom);

-- Index composite
CREATE INDEX idx_nom_prenom ON utilisateurs(nom, prenom);

-- Index unique
CREATE UNIQUE INDEX idx_email ON utilisateurs(email);

-- Index full-text
CREATE FULLTEXT INDEX idx_description ON produits(description);

-- Index spatial
CREATE SPATIAL INDEX idx_location ON magasins(coordonnees);
```

### Voir les index
```sql
SHOW INDEX FROM utilisateurs;
SHOW KEYS FROM utilisateurs;
EXPLAIN SELECT * FROM utilisateurs WHERE nom = 'Dupont';
```

### Supprimer un index
```sql
DROP INDEX idx_nom ON utilisateurs;
ALTER TABLE utilisateurs DROP INDEX idx_nom;
```

### Index avec ALTER TABLE
```sql
ALTER TABLE utilisateurs ADD INDEX idx_age (age);
ALTER TABLE utilisateurs ADD UNIQUE KEY uk_email (email);
ALTER TABLE utilisateurs ADD FULLTEXT KEY ft_bio (bio);
```

---

## Insertion de Données

### INSERT simple
```sql
INSERT INTO utilisateurs (nom, email, age) 
VALUES ('Dupont', 'dupont@email.com', 30);

-- Insertion multiple
INSERT INTO utilisateurs (nom, email, age) VALUES 
    ('Martin', 'martin@email.com', 25),
    ('Durand', 'durand@email.com', 35),
    ('Bernard', 'bernard@email.com', 28);
```

### INSERT avec SELECT
```sql
INSERT INTO utilisateurs_archive 
SELECT * FROM utilisateurs WHERE age > 50;
```

### INSERT ... ON DUPLICATE KEY
```sql
INSERT INTO utilisateurs (id, nom, email) 
VALUES (1, 'Dupont', 'dupont@email.com')
ON DUPLICATE KEY UPDATE nom = 'Dupont', email = 'dupont@email.com';
```

### REPLACE
```sql
REPLACE INTO utilisateurs (id, nom, email) 
VALUES (1, 'Nouveau Nom', 'nouveau@email.com');
```

### INSERT IGNORE
```sql
INSERT IGNORE INTO utilisateurs (email, nom) 
VALUES ('existant@email.com', 'Test');
```

---

## Sélection de Données (SELECT)

### SELECT de base
```sql
-- Tout sélectionner
SELECT * FROM utilisateurs;

-- Colonnes spécifiques
SELECT nom, email FROM utilisateurs;

-- Avec alias
SELECT nom AS nom_complet, email AS adresse_email FROM utilisateurs;

-- DISTINCT
SELECT DISTINCT ville FROM utilisateurs;
```

### WHERE (filtres)
```sql
SELECT * FROM utilisateurs WHERE age > 25;
SELECT * FROM utilisateurs WHERE nom = 'Dupont';
SELECT * FROM utilisateurs WHERE age BETWEEN 20 AND 30;
SELECT * FROM utilisateurs WHERE ville IN ('Paris', 'Lyon', 'Marseille');
SELECT * FROM utilisateurs WHERE email LIKE '%@gmail.com';
SELECT * FROM utilisateurs WHERE nom LIKE 'Du%';
SELECT * FROM utilisateurs WHERE age IS NULL;
SELECT * FROM utilisateurs WHERE age IS NOT NULL;
```

### Opérateurs logiques
```sql
SELECT * FROM utilisateurs WHERE age > 25 AND ville = 'Paris';
SELECT * FROM utilisateurs WHERE age < 20 OR age > 60;
SELECT * FROM utilisateurs WHERE NOT ville = 'Paris';
SELECT * FROM utilisateurs WHERE age > 25 AND (ville = 'Paris' OR ville = 'Lyon');
```

### ORDER BY
```sql
SELECT * FROM utilisateurs ORDER BY nom;
SELECT * FROM utilisateurs ORDER BY nom ASC;
SELECT * FROM utilisateurs ORDER BY age DESC;
SELECT * FROM utilisateurs ORDER BY ville, nom;
SELECT * FROM utilisateurs ORDER BY age DESC, nom ASC;
```

### LIMIT et OFFSET
```sql
SELECT * FROM utilisateurs LIMIT 10;
SELECT * FROM utilisateurs LIMIT 10 OFFSET 20;
SELECT * FROM utilisateurs LIMIT 20, 10;  -- OFFSET 20, LIMIT 10
```

### GROUP BY
```sql
SELECT ville, COUNT(*) FROM utilisateurs GROUP BY ville;
SELECT ville, AVG(age) FROM utilisateurs GROUP BY ville;
SELECT ville, COUNT(*) as total FROM utilisateurs 
    GROUP BY ville 
    HAVING total > 5;
```

### HAVING
```sql
SELECT ville, COUNT(*) as nb 
FROM utilisateurs 
GROUP BY ville 
HAVING nb > 10;

SELECT categorie, AVG(prix) as prix_moyen 
FROM produits 
GROUP BY categorie 
HAVING prix_moyen > 100;
```

---

## Mise à Jour de Données

### UPDATE simple
```sql
UPDATE utilisateurs SET age = 31 WHERE id = 1;

UPDATE utilisateurs 
SET nom = 'Nouveau Nom', email = 'nouveau@email.com' 
WHERE id = 5;
```

### UPDATE avec calculs
```sql
UPDATE produits SET prix = prix * 1.1;  -- Augmentation de 10%
UPDATE utilisateurs SET age = age + 1 WHERE ville = 'Paris';
```

### UPDATE avec JOIN
```sql
UPDATE utilisateurs u
JOIN commandes c ON u.id = c.user_id
SET u.dernier_achat = c.date_commande
WHERE c.statut = 'completed';
```

### UPDATE avec CASE
```sql
UPDATE produits 
SET categorie = CASE 
    WHEN prix < 10 THEN 'economique'
    WHEN prix BETWEEN 10 AND 50 THEN 'standard'
    ELSE 'premium'
END;
```

---

## Suppression de Données

### DELETE simple
```sql
DELETE FROM utilisateurs WHERE id = 1;
DELETE FROM utilisateurs WHERE age < 18;
```

### DELETE avec JOIN
```sql
DELETE u FROM utilisateurs u
JOIN commandes c ON u.id = c.user_id
WHERE c.statut = 'cancelled';
```

### DELETE tout
```sql
DELETE FROM utilisateurs;  -- Supprime toutes les lignes
TRUNCATE TABLE utilisateurs;  -- Plus rapide, réinitialise AUTO_INCREMENT
```

---

## Jointures

### INNER JOIN
```sql
SELECT u.nom, c.montant 
FROM utilisateurs u
INNER JOIN commandes c ON u.id = c.user_id;

-- Alias plus court
SELECT u.nom, c.montant 
FROM utilisateurs u
JOIN commandes c ON u.id = c.user_id;
```

### LEFT JOIN (LEFT OUTER JOIN)
```sql
SELECT u.nom, c.montant 
FROM utilisateurs u
LEFT JOIN commandes c ON u.id = c.user_id;

-- Trouver les utilisateurs sans commandes
SELECT u.* 
FROM utilisateurs u
LEFT JOIN commandes c ON u.id = c.user_id
WHERE c.id IS NULL;
```

### RIGHT JOIN
```sql
SELECT u.nom, c.montant 
FROM utilisateurs u
RIGHT JOIN commandes c ON u.id = c.user_id;
```

### CROSS JOIN
```sql
SELECT * FROM couleurs CROSS JOIN tailles;
```

### SELF JOIN
```sql
SELECT e1.nom as employe, e2.nom as manager
FROM employes e1
JOIN employes e2 ON e1.manager_id = e2.id;
```

### Jointures multiples
```sql
SELECT u.nom, c.montant, p.nom_produit
FROM utilisateurs u
JOIN commandes c ON u.id = c.user_id
JOIN produits p ON c.produit_id = p.id
WHERE c.statut = 'completed';
```

---

## Fonctions d'Agrégation

### Fonctions de base
```sql
SELECT COUNT(*) FROM utilisateurs;
SELECT COUNT(DISTINCT ville) FROM utilisateurs;
SELECT SUM(montant) FROM commandes;
SELECT AVG(age) FROM utilisateurs;
SELECT MIN(prix) FROM produits;
SELECT MAX(prix) FROM produits;
```

### GROUP_CONCAT
```sql
SELECT ville, GROUP_CONCAT(nom) as noms
FROM utilisateurs
GROUP BY ville;

SELECT ville, GROUP_CONCAT(nom SEPARATOR ' | ') as noms
FROM utilisateurs
GROUP BY ville;
```

### Agrégations avec GROUP BY
```sql
SELECT ville, COUNT(*) as total, AVG(age) as age_moyen
FROM utilisateurs
GROUP BY ville;

SELECT YEAR(date_commande) as annee, SUM(montant) as total
FROM commandes
GROUP BY YEAR(date_commande);
```

---

## Sous-Requêtes

### Sous-requête simple
```sql
SELECT * FROM utilisateurs 
WHERE age > (SELECT AVG(age) FROM utilisateurs);
```

### Sous-requête avec IN
```sql
SELECT * FROM produits 
WHERE categorie_id IN (SELECT id FROM categories WHERE actif = 1);
```

### Sous-requête corrélée
```sql
SELECT u.nom, 
    (SELECT COUNT(*) FROM commandes c WHERE c.user_id = u.id) as nb_commandes
FROM utilisateurs u;
```

### EXISTS
```sql
SELECT * FROM utilisateurs u
WHERE EXISTS (
    SELECT 1 FROM commandes c WHERE c.user_id = u.id
);

-- NOT EXISTS
SELECT * FROM utilisateurs u
WHERE NOT EXISTS (
    SELECT 1 FROM commandes c WHERE c.user_id = u.id
);
```

### ALL, ANY, SOME
```sql
SELECT * FROM produits 
WHERE prix > ALL (SELECT prix FROM produits WHERE categorie = 'economique');

SELECT * FROM produits 
WHERE prix > ANY (SELECT prix FROM produits WHERE categorie = 'economique');
```

---

## Vues

### Créer une vue
```sql
CREATE VIEW vue_utilisateurs_actifs AS
SELECT id, nom, email 
FROM utilisateurs 
WHERE actif = 1;

-- Avec CHECK OPTION
CREATE VIEW vue_prix_eleves AS
SELECT * FROM produits WHERE prix > 100
WITH CHECK OPTION;
```

### Utiliser une vue
```sql
SELECT * FROM vue_utilisateurs_actifs;
SELECT nom FROM vue_utilisateurs_actifs WHERE ville = 'Paris';
```

### Modifier une vue
```sql
CREATE OR REPLACE VIEW vue_utilisateurs_actifs AS
SELECT id, nom, email, ville 
FROM utilisateurs 
WHERE actif = 1;

ALTER VIEW vue_utilisateurs_actifs AS
SELECT id, nom FROM utilisateurs WHERE actif = 1;
```

### Supprimer une vue
```sql
DROP VIEW vue_utilisateurs_actifs;
DROP VIEW IF EXISTS vue_utilisateurs_actifs;
```

### Voir les vues
```sql
SHOW FULL TABLES WHERE TABLE_TYPE = 'VIEW';
SHOW CREATE VIEW vue_utilisateurs_actifs;
```

---

## Procédures Stockées

### Créer une procédure
```sql
DELIMITER //
CREATE PROCEDURE GetUtilisateurs()
BEGIN
    SELECT * FROM utilisateurs;
END //
DELIMITER ;

-- Avec paramètres
DELIMITER //
CREATE PROCEDURE GetUtilisateurParVille(IN ville_param VARCHAR(100))
BEGIN
    SELECT * FROM utilisateurs WHERE ville = ville_param;
END //
DELIMITER ;

-- Avec paramètres OUT
DELIMITER //
CREATE PROCEDURE CompterUtilisateurs(OUT total INT)
BEGIN
    SELECT COUNT(*) INTO total FROM utilisateurs;
END //
DELIMITER ;
```

### Appeler une procédure
```sql
CALL GetUtilisateurs();
CALL GetUtilisateurParVille('Paris');

-- Avec OUT
CALL CompterUtilisateurs(@total);
SELECT @total;
```

### Procédure avec logique
```sql
DELIMITER //
CREATE PROCEDURE AugmenterPrix(IN categorie_id INT, IN pourcentage DECIMAL(5,2))
BEGIN
    DECLARE EXIT HANDLER FOR SQLEXCEPTION
    BEGIN
        ROLLBACK;
    END;
    
    START TRANSACTION;
    UPDATE produits 
    SET prix = prix * (1 + pourcentage / 100)
    WHERE categorie = categorie_id;
    COMMIT;
END //
DELIMITER ;
```

### Voir et supprimer
```sql
SHOW PROCEDURE STATUS WHERE Db = 'ma_base';
SHOW CREATE PROCEDURE GetUtilisateurs;
DROP PROCEDURE IF EXISTS GetUtilisateurs;
```

---

## Fonctions

### Créer une fonction
```sql
DELIMITER //
CREATE FUNCTION Calculer_TVA(prix DECIMAL(10,2)) 
RETURNS DECIMAL(10,2)
DETERMINISTIC
BEGIN
    RETURN prix * 0.20;
END //
DELIMITER ;

-- Fonction plus complexe
DELIMITER //
CREATE FUNCTION NombreCommandes(user_id INT) 
RETURNS INT
READS SQL DATA
BEGIN
    DECLARE total INT;
    SELECT COUNT(*) INTO total 
    FROM commandes 
    WHERE utilisateur_id = user_id;
    RETURN total;
END //
DELIMITER ;
```

### Utiliser une fonction
```sql
SELECT nom, prix, Calculer_TVA(prix) as tva FROM produits;
SELECT nom, NombreCommandes(id) as nb_commandes FROM utilisateurs;
```

### Supprimer une fonction
```sql
DROP FUNCTION IF EXISTS Calculer_TVA;
```

---

## Triggers

### Créer un trigger
```sql
-- Trigger BEFORE INSERT
DELIMITER //
CREATE TRIGGER before_user_insert
BEFORE INSERT ON utilisateurs
FOR EACH ROW
BEGIN
    SET NEW.date_creation = NOW();
    SET NEW.email = LOWER(NEW.email);
END //
DELIMITER ;

-- Trigger AFTER INSERT
DELIMITER //
CREATE TRIGGER after_commande_insert
AFTER INSERT ON commandes
FOR EACH ROW
BEGIN
    UPDATE utilisateurs 
    SET nombre_commandes = nombre_commandes + 1
    WHERE id = NEW.user_id;
END //
DELIMITER ;

-- Trigger BEFORE UPDATE
DELIMITER //
CREATE TRIGGER before_produit_update
BEFORE UPDATE ON produits
FOR EACH ROW
BEGIN
    SET NEW.date_modification = NOW();
    IF NEW.prix < 0 THEN
        SIGNAL SQLSTATE '45000'
        SET MESSAGE_TEXT = 'Le prix ne peut pas être négatif';
    END IF;
END //
DELIMITER ;
```

### Voir les triggers
```sql
SHOW TRIGGERS;
SHOW TRIGGERS FROM ma_base;
SHOW CREATE TRIGGER before_user_insert;
```

### Supprimer un trigger
```sql
DROP TRIGGER IF EXISTS before_user_insert;
```

---

## Transactions

### Transaction de base
```sql
START TRANSACTION;
-- ou BEGIN;

INSERT INTO comptes (user_id, solde) VALUES (1, 1000);
UPDATE comptes SET solde = solde - 100 WHERE user_id = 1;
UPDATE comptes SET solde = solde + 100 WHERE user_id = 2;

COMMIT;
-- ou ROLLBACK; pour annuler
```

### Points de sauvegarde
```sql
START TRANSACTION;

INSERT INTO utilisateurs (nom) VALUES ('Test1');
SAVEPOINT point1;

INSERT INTO utilisateurs (nom) VALUES ('Test2');
SAVEPOINT point2;

INSERT INTO utilisateurs (nom) VALUES ('Test3');

ROLLBACK TO point2;  -- Annule seulement Test3
COMMIT;
```

### Isolation
```sql
-- Niveaux d'isolation
SET TRANSACTION ISOLATION LEVEL READ UNCOMMITTED;
SET TRANSACTION ISOLATION LEVEL READ COMMITTED;
SET TRANSACTION ISOLATION LEVEL REPEATABLE READ;
SET TRANSACTION ISOLATION LEVEL SERIALIZABLE;

-- Pour la session
SET SESSION TRANSACTION ISOLATION LEVEL REPEATABLE READ;
```

### Autocommit
```sql
SET autocommit = 0;  -- Désactiver
SET autocommit = 1;  -- Activer
SHOW VARIABLES LIKE 'autocommit';
```

---

## Gestion des Utilisateurs

### Créer un utilisateur
```sql
CREATE USER 'username'@'localhost' IDENTIFIED BY 'password';
CREATE USER 'username'@'%' IDENTIFIED BY 'password';  -- Tous les hôtes
CREATE USER IF NOT EXISTS 'username'@'localhost' IDENTIFIED BY 'password';
```

### Accorder des privilèges
```sql
-- Tous les privilèges sur une base
GRANT ALL PRIVILEGES ON ma_base.* TO 'username'@'localhost';

-- Privilèges spécifiques
GRANT SELECT, INSERT, UPDATE ON ma_base.* TO 'username'@'localhost';

-- Sur des tables spécifiques
GRANT SELECT ON ma_base.utilisateurs TO 'username'@'localhost';

-- Avec WITH GRANT OPTION
GRANT SELECT ON ma_base.* TO 'username'@'localhost' WITH GRANT OPTION;

-- Appliquer les changements
FLUSH PRIVILEGES;
```

### Révoquer des privilèges
```sql
REVOKE ALL PRIVILEGES ON ma_base.* FROM 'username'@'localhost';
REVOKE SELECT, INSERT ON ma_base.* FROM 'username'@'localhost';
```

### Modifier un utilisateur
```sql
-- Changer le mot de passe
ALTER USER 'username'@'localhost' IDENTIFIED BY 'nouveau_password';
SET PASSWORD FOR 'username'@'localhost' = PASSWORD('nouveau_password');

-- Renommer
RENAME USER 'ancien'@'localhost' TO 'nouveau'@'localhost';
```

### Voir les utilisateurs
```sql
SELECT User, Host FROM mysql.user;
SHOW GRANTS FOR 'username'@'localhost';
SELECT CURRENT_USER();
```

### Supprimer un utilisateur
```sql
DROP USER 'username'@'localhost';
DROP USER IF EXISTS 'username'@'localhost';
```

---

## Sauvegarde et Restauration

### Sauvegarde avec mysqldump
```bash
# Sauvegarder une base
mysqldump -u root -p ma_base > backup.sql

# Sauvegarder plusieurs bases
mysqldump -u root -p --databases base1 base2 > backup.sql

# Sauvegarder toutes les bases
mysqldump -u root -p --all-databases > backup.sql

# Sauvegarder une table
mysqldump -u root -p ma_base utilisateurs > users_backup.sql

# Avec compression
mysqldump -u root -p ma_base | gzip > backup.sql.gz

# Structure uniquement
mysqldump -u root -p --no-data ma_base > structure.sql

# Données uniquement
mysqldump -u root -p --no-create-info ma_base > data.sql

# Avec routines et triggers
mysqldump -u root -p --routines --triggers ma_base > backup.sql
```

### Restauration
```bash
# Restaurer une base
mysql -u root -p ma_base < backup.sql

# Restaurer depuis une compression
gunzip < backup.sql.gz | mysql -u root -p ma_base

# Créer la base et restaurer
mysql -u root -p -e "CREATE DATABASE ma_base"
mysql -u root -p ma_base < backup.sql
```

### Export/Import CSV
```sql
-- Export
SELECT * FROM utilisateurs
INTO OUTFILE '/tmp/users.csv'
FIELDS TERMINATED BY ','
ENCLOSED BY '"'
LINES TERMINATED BY '\n';

-- Import
LOAD DATA INFILE '/tmp/users.csv'
INTO TABLE utilisateurs
FIELDS TERMINATED BY ','
ENCLOSED BY '"'
LINES TERMINATED BY '\n'
IGNORE 1 ROWS;  -- Ignore l'en-tête

-- Import avec LOCAL
LOAD DATA LOCAL INFILE '/path/to/file.csv'
INTO TABLE utilisateurs
FIELDS TERMINATED BY ','
ENCLOSED BY '"'
LINES TERMINATED BY '\n';
```

---

## Optimisation et Performance

### EXPLAIN
```sql
EXPLAIN SELECT * FROM utilisateurs WHERE email = 'test@email.com';
EXPLAIN EXTENDED SELECT * FROM utilisateurs WHERE email = 'test@email.com';
EXPLAIN FORMAT=JSON SELECT * FROM utilisateurs WHERE email = 'test@email.com';
```

### ANALYZE TABLE
```sql
ANALYZE TABLE utilisateurs;
```

### OPTIMIZE TABLE
```sql
OPTIMIZE TABLE utilisateurs;
```

### Variables de performance
```sql
-- Voir les variables
SHOW VARIABLES LIKE 'innodb_buffer_pool_size';
SHOW VARIABLES LIKE '%cache%';

-- Définir des variables
SET GLOBAL innodb_buffer_pool_size = 1073741824;  -- 1GB
SET SESSION query_cache_size = 1048576;
```

### Status et statistiques
```sql
SHOW STATUS LIKE 'Threads%';
SHOW STATUS LIKE 'Slow_queries';
SHOW TABLE STATUS FROM ma_base;
SHOW INDEX FROM utilisateurs;
```

### Profiling
```sql
SET profiling = 1;
SELECT * FROM utilisateurs WHERE ville = 'Paris';
SHOW PROFILES;
SHOW PROFILE FOR QUERY 1;
SET profiling = 0;
```

### Requêtes lentes
```sql
-- Activer le log des requêtes lentes
SET GLOBAL slow_query_log = 'ON';
SET GLOBAL long_query_time = 2;  -- Secondes
SET GLOBAL slow_query_log_file = '/var/log/mysql/slow-query.log';
```

---

## Fonctions de Chaînes

### Manipulation de chaînes
```sql
SELECT CONCAT('Hello', ' ', 'World');
SELECT CONCAT_WS('-', '2024', '01', '15');  -- 2024-01-15
SELECT LENGTH('Hello');  -- 5
SELECT CHAR_LENGTH('Hello');  -- 5
SELECT UPPER('hello');  -- HELLO
SELECT LOWER('HELLO');  -- hello
SELECT SUBSTRING('Hello World', 1, 5);  -- Hello
SELECT SUBSTRING('Hello World', 7);  -- World
SELECT LEFT('Hello World', 5);  -- Hello
SELECT RIGHT('Hello World', 5);  -- World
SELECT TRIM('  Hello  ');  -- 'Hello'
SELECT LTRIM('  Hello');  -- 'Hello'
SELECT RTRIM('Hello  ');  -- 'Hello'
SELECT REPLACE('Hello World', 'World', 'MySQL');  -- Hello MySQL
SELECT REVERSE('Hello');  -- olleH
SELECT REPEAT('Hi', 3);  -- HiHiHi
SELECT LPAD('5', 3, '0');  -- 005
SELECT RPAD('5', 3, '0');  -- 500
SELECT LOCATE('World', 'Hello World');  -- 7
SELECT POSITION('World' IN 'Hello World');  -- 7
SELECT INSTR('Hello World', 'World');  -- 7
SELECT STRCMP('text1', 'text2');  -- Comparaison
SELECT FORMAT(12332.123456, 2);  -- 12,332.12
```

### Recherche et remplacement
```sql
SELECT FIELD('b', 'a', 'b', 'c');  -- 2
SELECT FIND_IN_SET('b', 'a,b,c,d');  -- 2
SELECT ELT(2, 'a', 'b', 'c');  -- b
SELECT INSERT('Hello World', 7, 5, 'MySQL');  -- Hello MySQL
```

---

## Fonctions de Dates

### Dates actuelles
```sql
SELECT NOW();  -- Date et heure actuelles
SELECT CURDATE();  -- Date actuelle
SELECT CURRENT_DATE();  -- Date actuelle
SELECT CURTIME();  -- Heure actuelle
SELECT CURRENT_TIME();  -- Heure actuelle
SELECT CURRENT_TIMESTAMP();  -- Timestamp actuel
SELECT UNIX_TIMESTAMP();  -- Timestamp Unix
SELECT UTC_TIMESTAMP();  -- UTC
```

### Extraction de parties de dates
```sql
SELECT YEAR('2024-05-15');  -- 2024
SELECT MONTH('2024-05-15');  -- 5
SELECT DAY('2024-05-15');  -- 15
SELECT HOUR('14:30:45');  -- 14
SELECT MINUTE('14:30:45');  -- 30
SELECT SECOND('14:30:45');  -- 45
SELECT DAYNAME('2024-05-15');  -- Wednesday
SELECT MONTHNAME('2024-05-15');  -- May
SELECT DAYOFWEEK('2024-05-15');  -- 1-7 (Dimanche = 1)
SELECT DAYOFMONTH('2024-05-15');  -- 15
SELECT DAYOFYEAR('2024-05-15');  -- 136
SELECT WEEK('2024-05-15');  -- Numéro de semaine
SELECT QUARTER('2024-05-15');  -- 2
SELECT WEEKDAY('2024-05-15');  -- 0-6 (Lundi = 0)
```

### Calculs de dates
```sql
SELECT DATE_ADD('2024-05-15', INTERVAL 1 DAY);
SELECT DATE_ADD('2024-05-15', INTERVAL 1 MONTH);
SELECT DATE_ADD('2024-05-15', INTERVAL 1 YEAR);
SELECT DATE_SUB('2024-05-15', INTERVAL 7 DAY);
SELECT ADDDATE('2024-05-15', 7);  -- Ajoute 7 jours
SELECT SUBDATE('2024-05-15', 7);  -- Soustrait 7 jours
SELECT DATEDIFF('2024-12-31', '2024-01-01');  -- 365
SELECT TIMEDIFF('14:30:00', '12:15:00');  -- 02:15:00
SELECT TIMESTAMPDIFF(YEAR, '2000-01-01', '2024-01-01');  -- 24
SELECT TIMESTAMPDIFF(MONTH, '2024-01-01', '2024-12-01');  -- 11
SELECT TIMESTAMPDIFF(DAY, '2024-01-01', '2024-01-15');  -- 14
```

### Formatage de dates
```sql
SELECT DATE_FORMAT('2024-05-15', '%d/%m/%Y');  -- 15/05/2024
SELECT DATE_FORMAT('2024-05-15', '%W %M %Y');  -- Wednesday May 2024
SELECT DATE_FORMAT(NOW(), '%Y-%m-%d %H:%i:%s');
SELECT TIME_FORMAT('14:30:45', '%H:%i');  -- 14:30
```

### Conversion de dates
```sql
SELECT STR_TO_DATE('15/05/2024', '%d/%m/%Y');
SELECT FROM_UNIXTIME(1609459200);  -- Convertit timestamp Unix
SELECT UNIX_TIMESTAMP('2024-01-01 00:00:00');
SELECT CONVERT_TZ('2024-05-15 12:00:00', 'UTC', 'Europe/Paris');
```

### Dates spéciales
```sql
SELECT LAST_DAY('2024-02-15');  -- 2024-02-29
SELECT MAKEDATE(2024, 100);  -- 100e jour de 2024
SELECT MAKETIME(14, 30, 45);  -- 14:30:45
```

---

## Fonctions Mathématiques

### Fonctions de base
```sql
SELECT ABS(-15);  -- 15
SELECT CEIL(4.3);  -- 5
SELECT CEILING(4.3);  -- 5
SELECT FLOOR(4.7);  -- 4
SELECT ROUND(4.567, 2);  -- 4.57
SELECT TRUNCATE(4.567, 2);  -- 4.56
SELECT MOD(10, 3);  -- 1 (modulo)
SELECT POW(2, 3);  -- 8
SELECT POWER(2, 3);  -- 8
SELECT SQRT(16);  -- 4
SELECT EXP(1);  -- e
SELECT LOG(10);  -- Logarithme naturel
SELECT LOG10(100);  -- 2
SELECT LOG2(8);  -- 3
```

### Fonctions trigonométriques
```sql
SELECT PI();  -- 3.141593
SELECT SIN(PI()/2);  -- 1
SELECT COS(0);  -- 1
SELECT TAN(PI()/4);  -- 1
SELECT ASIN(1);  -- Arc sinus
SELECT ACOS(1);  -- Arc cosinus
SELECT ATAN(1);  -- Arc tangente
SELECT DEGREES(PI());  -- 180
SELECT RADIANS(180);  -- π
```

### Fonctions aléatoires
```sql
SELECT RAND();  -- Nombre aléatoire entre 0 et 1
SELECT RAND() * 100;  -- Entre 0 et 100
SELECT FLOOR(RAND() * 100);  -- Entier entre 0 et 99
SELECT ROUND(RAND() * 100);  -- Entier arrondi
```

### Autres fonctions
```sql
SELECT SIGN(-15);  -- -1 (signe)
SELECT GREATEST(10, 20, 5, 30);  -- 30
SELECT LEAST(10, 20, 5, 30);  -- 5
SELECT CONV('FF', 16, 10);  -- 255 (conversion de base)
```

---

## Expressions Régulières

### REGEXP / RLIKE
```sql
-- Correspondance simple
SELECT * FROM utilisateurs WHERE email REGEXP '^[a-z]+@gmail.com;
SELECT * FROM produits WHERE nom RLIKE 'laptop|desktop|tablet';

-- Patterns courants
SELECT * FROM utilisateurs WHERE telephone REGEXP '^[0-9]{10};
SELECT * FROM articles WHERE contenu REGEXP '\\bpython\\b';  -- Mot exact
SELECT * FROM users WHERE nom REGEXP '^[A-Z][a-z]+;  -- Commence par majuscule
```

### REGEXP_LIKE (MySQL 8.0+)
```sql
SELECT * FROM utilisateurs 
WHERE REGEXP_LIKE(email, '^[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\\.[a-zA-Z]{2,});
```

### REGEXP_REPLACE (MySQL 8.0+)
```sql
SELECT REGEXP_REPLACE('Hello World', 'World', 'MySQL');
SELECT REGEXP_REPLACE(telephone, '[^0-9]', '');  -- Garde seulement les chiffres
SELECT REGEXP_REPLACE(texte, '\\s+', ' ');  -- Remplace espaces multiples
```

### REGEXP_SUBSTR (MySQL 8.0+)
```sql
SELECT REGEXP_SUBSTR('Contact: email@example.com', '[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\\.[a-zA-Z]{2,}');
```

### REGEXP_INSTR (MySQL 8.0+)
```sql
SELECT REGEXP_INSTR('Hello World', 'World');  -- Position
```

---

## JSON

### Créer JSON
```sql
SELECT JSON_OBJECT('nom', 'Dupont', 'age', 30);
SELECT JSON_ARRAY(1, 2, 3, 4, 5);
SELECT JSON_QUOTE('Hello "World"');
```

### Extraire données JSON
```sql
-- Extraire avec ->
SELECT data->'$.nom' FROM utilisateurs;
SELECT data->>'$.nom' FROM utilisateurs;  -- Sans guillemets

-- JSON_EXTRACT
SELECT JSON_EXTRACT(data, '$.nom') FROM utilisateurs;
SELECT JSON_EXTRACT(data, '$.adresse.ville') FROM utilisateurs;
SELECT JSON_EXTRACT(data, '$.tags[0]') FROM articles;

-- JSON_UNQUOTE
SELECT JSON_UNQUOTE(JSON_EXTRACT(data, '$.nom')) FROM utilisateurs;
```

### Modifier JSON
```sql
-- JSON_SET (modifie ou ajoute)
UPDATE utilisateurs 
SET data = JSON_SET(data, '$.age', 31) 
WHERE id = 1;

-- JSON_INSERT (ajoute seulement si n'existe pas)
UPDATE utilisateurs 
SET data = JSON_INSERT(data, '$.telephone', '0123456789');

-- JSON_REPLACE (modifie seulement si existe)
UPDATE utilisateurs 
SET data = JSON_REPLACE(data, '$.age', 32);

-- JSON_REMOVE
UPDATE utilisateurs 
SET data = JSON_REMOVE(data, '$.ancien_champ');

-- JSON_ARRAY_APPEND
UPDATE articles 
SET data = JSON_ARRAY_APPEND(data, '$.tags', 'nouveau-tag');

-- JSON_ARRAY_INSERT
UPDATE articles 
SET data = JSON_ARRAY_INSERT(data, '$.tags[0]', 'premier-tag');
```

### Recherche JSON
```sql
-- JSON_CONTAINS
SELECT * FROM utilisateurs 
WHERE JSON_CONTAINS(data, '"Paris"', '$.ville');

-- JSON_CONTAINS_PATH
SELECT * FROM utilisateurs 
WHERE JSON_CONTAINS_PATH(data, 'one', '$.telephone', '$.email');

-- JSON_SEARCH
SELECT JSON_SEARCH(data, 'one', 'Paris') FROM utilisateurs;
```

### Fonctions utiles JSON
```sql
SELECT JSON_KEYS(data) FROM utilisateurs;
SELECT JSON_LENGTH(data) FROM utilisateurs;
SELECT JSON_DEPTH(data) FROM utilisateurs;
SELECT JSON_TYPE(data) FROM utilisateurs;
SELECT JSON_VALID('{"nom":"test"}');  -- 1 si valide
SELECT JSON_PRETTY(data) FROM utilisateurs;  -- Formatage lisible

-- Fusionner
SELECT JSON_MERGE_PRESERVE('{"a":1}', '{"b":2}');
SELECT JSON_MERGE_PATCH('{"a":1}', '{"a":2}');
```

### Table JSON
```sql
-- JSON_TABLE (MySQL 8.0+)
SELECT jt.* 
FROM utilisateurs,
JSON_TABLE(data, '$.commandes[*]' COLUMNS(
    id INT PATH '$.id',
    montant DECIMAL(10,2) PATH '$.montant',
    date DATE PATH '$.date'
)) AS jt;
```

---

## Window Functions

### Fonctions de classement
```sql
-- ROW_NUMBER
SELECT nom, salaire,
    ROW_NUMBER() OVER (ORDER BY salaire DESC) as rang
FROM employes;

-- RANK (avec ex-aequo, saute des rangs)
SELECT nom, salaire,
    RANK() OVER (ORDER BY salaire DESC) as rang
FROM employes;

-- DENSE_RANK (avec ex-aequo, sans sauter)
SELECT nom, salaire,
    DENSE_RANK() OVER (ORDER BY salaire DESC) as rang
FROM employes;

-- NTILE (divise en groupes)
SELECT nom, salaire,
    NTILE(4) OVER (ORDER BY salaire DESC) as quartile
FROM employes;
```

### Fonctions d'agrégation avec fenêtres
```sql
-- Moyenne mobile
SELECT date, ventes,
    AVG(ventes) OVER (ORDER BY date ROWS BETWEEN 2 PRECEDING AND CURRENT ROW) as moyenne_3j
FROM ventes_quotidiennes;

-- Somme cumulée
SELECT date, ventes,
    SUM(ventes) OVER (ORDER BY date) as cumul
FROM ventes_quotidiennes;

-- Avec PARTITION BY
SELECT departement, nom, salaire,
    AVG(salaire) OVER (PARTITION BY departement) as salaire_moyen_dept
FROM employes;
```

### Fonctions de valeur
```sql
-- LEAD (valeur suivante)
SELECT date, ventes,
    LEAD(ventes, 1) OVER (ORDER BY date) as ventes_jour_suivant
FROM ventes_quotidiennes;

-- LAG (valeur précédente)
SELECT date, ventes,
    LAG(ventes, 1) OVER (ORDER BY date) as ventes_jour_precedent
FROM ventes_quotidiennes;

-- FIRST_VALUE
SELECT nom, salaire,
    FIRST_VALUE(nom) OVER (PARTITION BY departement ORDER BY salaire DESC) as mieux_paye
FROM employes;

-- LAST_VALUE
SELECT nom, salaire,
    LAST_VALUE(nom) OVER (
        PARTITION BY departement 
        ORDER BY salaire DESC
        RANGE BETWEEN UNBOUNDED PRECEDING AND UNBOUNDED FOLLOWING
    ) as moins_paye
FROM employes;

-- NTH_VALUE
SELECT nom, salaire,
    NTH_VALUE(salaire, 2) OVER (PARTITION BY departement ORDER BY salaire DESC) as deuxieme_salaire
FROM employes;
```

### Frames (fenêtres de lignes)
```sql
-- ROWS BETWEEN
SELECT date, ventes,
    SUM(ventes) OVER (
        ORDER BY date 
        ROWS BETWEEN 6 PRECEDING AND CURRENT ROW
    ) as somme_7_jours
FROM ventes_quotidiennes;

-- RANGE BETWEEN
SELECT date, ventes,
    AVG(ventes) OVER (
        ORDER BY date 
        RANGE BETWEEN INTERVAL 7 DAY PRECEDING AND CURRENT ROW
    ) as moyenne_semaine
FROM ventes_quotidiennes;
```

---

## CTE (Common Table Expressions)

### CTE simple
```sql
WITH utilisateurs_actifs AS (
    SELECT * FROM utilisateurs WHERE actif = 1
)
SELECT * FROM utilisateurs_actifs WHERE ville = 'Paris';
```

### CTE multiples
```sql
WITH 
    ventes_2024 AS (
        SELECT * FROM ventes WHERE YEAR(date) = 2024
    ),
    top_produits AS (
        SELECT produit_id, SUM(quantite) as total
        FROM ventes_2024
        GROUP BY produit_id
        ORDER BY total DESC
        LIMIT 10
    )
SELECT p.nom, tp.total
FROM top_produits tp
JOIN produits p ON tp.produit_id = p.id;
```

### CTE récursive (MySQL 8.0+)
```sql
-- Hiérarchie d'employés
WITH RECURSIVE hierarchie AS (
    -- Ancre: employés de niveau supérieur
    SELECT id, nom, manager_id, 1 as niveau
    FROM employes
    WHERE manager_id IS NULL
    
    UNION ALL
    
    -- Partie récursive
    SELECT e.id, e.nom, e.manager_id, h.niveau + 1
    FROM employes e
    JOIN hierarchie h ON e.manager_id = h.id
)
SELECT * FROM hierarchie ORDER BY niveau, nom;

-- Suite de Fibonacci
WITH RECURSIVE fibonacci(n, fib_n, fib_n1) AS (
    SELECT 1, 0, 1
    UNION ALL
    SELECT n + 1, fib_n1, fib_n + fib_n1
    FROM fibonacci
    WHERE n < 10
)
SELECT n, fib_n FROM fibonacci;

-- Génération de dates
WITH RECURSIVE dates AS (
    SELECT DATE('2024-01-01') as date
    UNION ALL
    SELECT DATE_ADD(date, INTERVAL 1 DAY)
    FROM dates
    WHERE date < '2024-12-31'
)
SELECT * FROM dates;
```

---

## Partitionnement

### Partitionnement par RANGE
```sql
CREATE TABLE ventes (
    id INT,
    date DATE,
    montant DECIMAL(10,2)
)
PARTITION BY RANGE (YEAR(date)) (
    PARTITION p2022 VALUES LESS THAN (2023),
    PARTITION p2023 VALUES LESS THAN (2024),
    PARTITION p2024 VALUES LESS THAN (2025),
    PARTITION p_future VALUES LESS THAN MAXVALUE
);
```

### Partitionnement par LIST
```sql
CREATE TABLE clients (
    id INT,
    nom VARCHAR(100),
    region VARCHAR(50)
)
PARTITION BY LIST COLUMNS(region) (
    PARTITION p_nord VALUES IN ('Nord', 'Nord-Est', 'Nord-Ouest'),
    PARTITION p_sud VALUES IN ('Sud', 'Sud-Est', 'Sud-Ouest'),
    PARTITION p_centre VALUES IN ('Centre', 'Île-de-France')
);
```

### Partitionnement par HASH
```sql
CREATE TABLE logs (
    id INT,
    message TEXT,
    date TIMESTAMP
)
PARTITION BY HASH(id)
PARTITIONS 4;
```

### Partitionnement par KEY
```sql
CREATE TABLE sessions (
    id INT PRIMARY KEY,
    user_id INT,
    data TEXT
)
PARTITION BY KEY(id)
PARTITIONS 8;
```

### Gestion des partitions
```sql
-- Voir les partitions
SELECT PARTITION_NAME, TABLE_ROWS 
FROM information_schema.PARTITIONS 
WHERE TABLE_NAME = 'ventes';

-- Ajouter une partition
ALTER TABLE ventes ADD PARTITION (
    PARTITION p2025 VALUES LESS THAN (2026)
);

-- Supprimer une partition
ALTER TABLE ventes DROP PARTITION p2022;

-- Tronquer une partition
ALTER TABLE ventes TRUNCATE PARTITION p2023;

-- Réorganiser les partitions
ALTER TABLE ventes REORGANIZE PARTITION p_future INTO (
    PARTITION p2025 VALUES LESS THAN (2026),
    PARTITION p_future VALUES LESS THAN MAXVALUE
);
```

---

## Réplication

### Configuration Master
```sql
-- Sur le serveur master
-- Éditer my.cnf
[mysqld]
server-id = 1
log-bin = /var/log/mysql/mysql-bin.log
binlog-do-db = ma_base

-- Créer un utilisateur de réplication
CREATE USER 'repl'@'%' IDENTIFIED BY 'password';
GRANT REPLICATION SLAVE ON *.* TO 'repl'@'%';
FLUSH PRIVILEGES;

-- Obtenir la position du binlog
SHOW MASTER STATUS;
```

### Configuration Slave
```sql
-- Sur le serveur slave
-- Éditer my.cnf
[mysqld]
server-id = 2
relay-log = /var/log/mysql/mysql-relay-bin
log-bin = /var/log/mysql/mysql-bin.log
read-only = 1

-- Configurer le slave
CHANGE MASTER TO
    MASTER_HOST='master_ip',
    MASTER_USER='repl',
    MASTER_PASSWORD='password',
    MASTER_LOG_FILE='mysql-bin.000001',
    MASTER_LOG_POS=12345;

-- Démarrer la réplication
START SLAVE;

-- Vérifier le statut
SHOW SLAVE STATUS\G
```

### Commandes de réplication
```sql
-- Sur le master
SHOW MASTER STATUS;
SHOW BINARY LOGS;
SHOW BINLOG EVENTS IN 'mysql-bin.000001';

-- Sur le slave
SHOW SLAVE STATUS\G
START SLAVE;
STOP SLAVE;
RESET SLAVE;

-- Sauter une erreur
SET GLOBAL sql_slave_skip_counter = 1;
START SLAVE;
```

---

## Nouveautés MySQL 8.0+

### Rôles
```sql
-- Créer un rôle
CREATE ROLE 'app_developer', 'app_reader';

-- Accorder des privilèges au rôle
GRANT SELECT, INSERT, UPDATE ON ma_base.* TO 'app_developer';
GRANT SELECT ON ma_base.* TO 'app_reader';

-- Assigner un rôle à un utilisateur
GRANT 'app_developer' TO 'john'@'localhost';
GRANT 'app_reader' TO 'jane'@'localhost';

-- Définir le rôle par défaut
SET DEFAULT ROLE 'app_developer' TO 'john'@'localhost';

-- Activer le rôle
SET ROLE 'app_developer';
```

### Index invisibles
```sql
-- Créer un index invisible
CREATE INDEX idx_nom ON utilisateurs(nom) INVISIBLE;

-- Rendre un index invisible
ALTER TABLE utilisateurs ALTER INDEX idx_nom INVISIBLE;

-- Rendre un index visible
ALTER TABLE utilisateurs ALTER INDEX idx_nom VISIBLE;
```

### Index descendants
```sql
CREATE INDEX idx_date ON commandes(date_commande DESC);
CREATE INDEX idx_composite ON produits(categorie ASC, prix DESC);
```

### Expressions dans les index (MySQL 8.0.13+)
```sql
CREATE INDEX idx_upper_nom ON utilisateurs((UPPER(nom)));
CREATE INDEX idx_year ON commandes((YEAR(date_commande)));

-- Utilisation
SELECT * FROM utilisateurs WHERE UPPER(nom) = 'DUPONT';
```

### Check Constraints (MySQL 8.0.16+)
```sql
ALTER TABLE produits ADD CONSTRAINT chk_prix CHECK (prix > 0);
ALTER TABLE utilisateurs ADD CONSTRAINT chk_age CHECK (age >= 18 AND age <= 120);
ALTER TABLE commandes ADD CONSTRAINT chk_statut 
    CHECK (statut IN ('pending', 'completed', 'cancelled'));
```

### DEFAULT expressions
```sql
CREATE TABLE events (
    id INT AUTO_INCREMENT PRIMARY KEY,
    nom VARCHAR(100),
    date_debut DATETIME DEFAULT (CURRENT_TIMESTAMP),
    date_fin DATETIME DEFAULT (CURRENT_TIMESTAMP + INTERVAL 1 HOUR),
    uuid VARCHAR(36) DEFAULT (UUID())
);
```

### Amélioration des CTE
```sql
-- CTE avec MATERIALIZED
WITH cte_name AS MATERIALIZED (
    SELECT expensive_query FROM large_table
)
SELECT * FROM cte_name;
```

### LATERAL (MySQL 8.0.14+)
```sql
SELECT u.nom, c.total
FROM utilisateurs u,
LATERAL (
    SELECT SUM(montant) as total
    FROM commandes
    WHERE user_id = u.id
) c;
```

### GROUPING
```sql
SELECT 
    COALESCE(categorie, 'TOTAL') as categorie,
    COALESCE(annee, 'TOUTES') as annee,
    SUM(ventes) as total,
    GROUPING(categorie) as grp_cat,
    GROUPING(annee) as grp_annee
FROM ventes
GROUP BY categorie, annee WITH ROLLUP;
```

---

## Dépannage

### Voir les connexions
```sql
SHOW PROCESSLIST;
SHOW FULL PROCESSLIST;
SELECT * FROM information_schema.PROCESSLIST;

-- Tuer une connexion
KILL 12345;  -- ID du processus
```

### Verrous et blocages
```sql
-- Voir les verrous
SHOW OPEN TABLES WHERE In_use > 0;
SELECT * FROM information_schema.INNODB_LOCKS;
SELECT * FROM information_schema.INNODB_LOCK_WAITS;

-- Voir les transactions
SELECT * FROM information_schema.INNODB_TRX;
```

### Logs d'erreurs
```sql
SHOW VARIABLES LIKE 'log_error';
SHOW VARIABLES LIKE '%log%';
```

### Espace disque
```sql
-- Taille des bases
SELECT 
    table_schema AS 'Database',
    ROUND(SUM(data_length + index_length) / 1024 / 1024, 2) AS 'Size (MB)'
FROM information_schema.TABLES
GROUP BY table_schema;

-- Taille des tables
SELECT 
    table_name AS 'Table',
    ROUND(((data_length + index_length) / 1024 / 1024), 2) AS 'Size (MB)'
FROM information_schema.TABLES
WHERE table_schema = 'ma_base'
ORDER BY (data_length + index_length) DESC;
```

### Réparer les tables
```sql
REPAIR TABLE utilisateurs;
CHECK TABLE utilisateurs;
ANALYZE TABLE utilisateurs;
OPTIMIZE TABLE utilisateurs;
```

### Réinitialiser le mot de passe root
```bash
# 1. Arrêter MySQL
sudo systemctl stop mysql

# 2. Démarrer en mode safe
sudo mysqld_safe --skip-grant-tables &

# 3. Se connecter sans mot de passe
mysql -u root

# 4. Changer le mot de passe
USE mysql;
UPDATE user SET authentication_string=PASSWORD('nouveau_password') WHERE User='root';
FLUSH PRIVILEGES;

# 5. Redémarrer normalement
sudo systemctl restart mysql
```

### Information Schema utiles
```sql
-- Colonnes d'une table
SELECT * FROM information_schema.COLUMNS 
WHERE TABLE_SCHEMA = 'ma_base' AND TABLE_NAME = 'utilisateurs';

-- Index d'une table
SELECT * FROM information_schema.STATISTICS 
WHERE TABLE_SCHEMA = 'ma_base' AND TABLE_NAME = 'utilisateurs';

-- Contraintes
SELECT * FROM information_schema.TABLE_CONSTRAINTS 
WHERE TABLE_SCHEMA = 'ma_base';

-- Triggers
SELECT * FROM information_schema.TRIGGERS 
WHERE TRIGGER_SCHEMA = 'ma_base';

-- Procédures et fonctions
SELECT * FROM information_schema.ROUTINES 
WHERE ROUTINE_SCHEMA = 'ma_base';
```

---
- MySQL Performance Blog: https://www.percona.com/blog/
- Community: https://forums.mysql.com/
