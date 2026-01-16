# Guide des Commandes SQL

---

## 1. Commandes de base

### Sélection de données
```sql
-- Sélectionner toutes les colonnes d'une table
SELECT * FROM clients;

-- Sélectionner des colonnes spécifiques
SELECT nom, prenom, email FROM clients;

-- Sélectionner avec une condition (WHERE)
SELECT * FROM clients WHERE age > 18;

-- Sélectionner avec tri (ORDER BY)
SELECT * FROM clients ORDER BY nom ASC; -- ASC : croissant, DESC : décroissant

-- Limiter le nombre de résultats
SELECT * FROM clients LIMIT 10;
```
## 2.Filtrage avancé
```sql
-- Conditions multiples (AND, OR, NOT)
SELECT * FROM clients WHERE age > 18 AND ville = 'Paris';

-- Filtrer avec IN
SELECT * FROM clients WHERE ville IN ('Paris', 'Lyon', 'Marseille');

-- Filtrer avec LIKE (recherche de motif)
SELECT * FROM clients WHERE nom LIKE 'D%'; -- Commence par "D"
SELECT * FROM clients WHERE nom LIKE '%on'; -- Finit par "on"
SELECT * FROM clients WHERE nom LIKE '%ar%'; -- Contient "ar"
```
## 3.Agréation et regroupement
```sql
-- Compter le nombre d'enregistrements
SELECT COUNT(*) FROM clients;

-- Calculer la moyenne, somme, min, max
SELECT AVG(age) FROM clients;
SELECT SUM(montant) FROM commandes;
SELECT MIN(prix), MAX(prix) FROM produits;

-- Regrouper avec GROUP BY
SELECT ville, COUNT(*) FROM clients GROUP BY ville;

-- Filtrer après regroupement (HAVING)
SELECT ville, COUNT(*) FROM clients GROUP BY ville HAVING COUNT(*) > 5;
```
## 4.jointure
```sql
-- Jointure interne (INNER JOIN)
SELECT clients.nom, commandes.montant
FROM clients
INNER JOIN commandes ON clients.id = commandes.client_id;

-- Jointure gauche (LEFT JOIN)
SELECT clients.nom, commandes.montant
FROM clients
LEFT JOIN commandes ON clients.id = commandes.client_id;

-- Jointure droite (RIGHT JOIN)
SELECT clients.nom, commandes.montant
FROM clients
RIGHT JOIN commandes ON clients.id = commandes.client_id;
```
## 5.Modification des données
```sql
-- Insérer un nouvel enregistrement
INSERT INTO clients (nom, prenom, email)
VALUES ('Dupont', 'Jean', 'jean.dupont@example.com');

-- Mettre à jour un enregistrement
UPDATE clients
SET email = 'nouvel.email@example.com'
WHERE id = 1;

-- Supprimer un enregistrement
DELETE FROM clients WHERE id = 1;
```

-- Créer une table
CREATE TABLE clients (
    id INT AUTO_INCREMENT PRIMARY KEY,
    nom VARCHAR(50) NOT NULL,
    prenom VARCHAR(50) NOT NULL,
    email VARCHAR(100) UNIQUE,
    age INT
);

-- Modifier une table (ajouter une colonne)
ALTER TABLE clients ADD COLUMN ville VARCHAR(50);

-- Supprimer une table
DROP TABLE clients;

-- Fonctions de date
SELECT CURRENT_DATE; -- Date du jour
SELECT DATE_ADD(CURRENT_DATE, INTERVAL 7 DAY); -- Ajouter 7 jours

-- Fonctions de texte
SELECT CONCAT(nom, ' ', prenom) FROM clients;
SELECT UPPER(nom), LOWER(nom) FROM clients;

-- Fonctions conditionnelles
SELECT nom,
       CASE
           WHEN age < 18 THEN 'Mineur'
           WHEN age BETWEEN 18 AND 65 THEN 'Adulte'
           ELSE 'Sénior'
       END AS categorie_age
FROM clients;
