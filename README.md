ANALYSES DONNEES SUPER MARKET SQL


--création de la table clients

DROP TABLE IF EXISTS clients CASCADE;

CREATE TABLE clients (
    id INT GENERATED ALWAYS AS IDENTITY PRIMARY KEY,
    nom TEXT NOT NULL,
    pays TEXT,
    date_creation DATE
);

--création de la table produits

DROP TABLE IF EXISTS produits CASCADE;
CREATE TABLE produits (
    id INT GENERATED ALWAYS AS IDENTITY PRIMARY KEY,
    nom TEXT NOT NULL,
    prix NUMERIC(10,2) NOT NULL
);


--création de la table ventes

DROP TABLE IF EXISTS ventes CASCADE;
CREATE TABLE ventes (
    id INT GENERATED ALWAYS AS IDENTITY PRIMARY KEY,
    client_id INT NOT NULL REFERENCES clients(id) ON DELETE CASCADE,
    produit_id INT NOT NULL REFERENCES produits(id) ON DELETE CASCADE,
    quantite INT CHECK (quantite > 0),
    date_vente DATE NOT NULL
);
 

--Insertion de queques produits clients

INSERT INTO clients (nom, pays, date_creation) VALUES
('Aminata', 'Sénégal', '2025-01-10'),
('Jean', 'France', '2025-02-01'),
('Fatou', 'Cameroun', '2025-01-15'),
('Luc', 'Belgique', '2025-02-05');

--Insertion de queques produits produit

INSERT INTO produits (nom, prix) VALUES
('Ordinateur Portable', 1200),
('Casque Audio', 80),
('Souris USB', 30),
('Clé USB 32GB', 15);


--Insertion de queques produits ventes

INSERT INTO ventes (client_id, produit_id, quantite, date_vente) VALUES
(1, 1, 1, '2025-02-10'),
(1, 2, 2, '2025-02-10'),
(2, 3, 1, '2025-02-11'),
(3, 2, 1, '2025-02-12'),
(4, 4, 3, '2025-02-12');


SELECT * FROM produits;

-- Réponses à quelques questions!

-- Listez tout les produits de 

SELECT * FROM clients;

-- Quels sont les produits > 100$

SELECT * FROM produits WHERE prix > 100;

-- Combien avons-nous vendu au total
SELECT SUM(v.quantite * p.prix) AS total_ventes
FROM ventes v
JOIN produits p ON v.produit_id = p.id; 

-- Quel pays génère plus de vente?

SELECT cl.pays,
SUM(v.quantite * p.prix) AS total_ventes
FROM ventes v
JOIN clients cl ON v.client_id = cl.id
JOIN produits p ON v.produit_id = p.id
GROUP BY cl.pays
ORDER BY total_ventes DESC
LIMIT 1;


-- Aminata a réalisé combien de vente?

select cl.nom, count(ve.client_id) as nombre_vente
FROM clients cl
JOIN ventes ve ON cl.id = ve.client_id
WHERE cl.nom = 'Aminata'
GROUP BY cl.nom;
