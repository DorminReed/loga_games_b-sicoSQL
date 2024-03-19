# loga_games_b-sicoSQL
Banco de dados simples, com o contexto de ser uma loja de jogos, onde manipulamos e fazemos consultas.

create database SuperGames;
use SuperGames;

CREATE TABLE Localizacao(
	Id INT PRIMARY KEY AUTO_INCREMENT,
    Secao VARCHAR(50) NOT NULL,
    Prateleira INT(3) ZEROFILL NOT NULL,
    FOREIGN KEY (Localizacao_Id)
		REFERENCES Localizacao (Id)
);

CREATE TABLE jogo(
	Cod INT PRIMARY KEY AUTO_INCREMENT,
    Nome VARCHAR(50) NOT NULL,
    Valor DECIMAL(6 , 2 ) NOT NULL,
    Localizacao_Id INT(3) NOT NULL
);

INSERT localizacao VALUES
(0, "Guerra", "001"),
(0, "Guerra", "002"),
(0, "Aventura", "100"),
(0, "Aventura", "101"),
(0, "RPG", "150"),
(0, "RPG", "151"),
(0, "Plataforma 2D", "200"),
(0, "Plataforma 2D", "201");

INSERT jogo VALUES
(0, "COD 3", 125.00, 1),
(0, "BF 1", 150.00, 2),
(0, "Zelda Botw", 200.00, 3),
(0, "Zelda OoT", 99.00, 4),
(0, "Chrono", 205.00, 5);

SELECT * FROM localizacao;
SELECT * FROM jogo;

-- Identificar o nome do jogo e a prateleira
SELECT jogo.nome, localizacao.prateleira
FROM jogo INNER JOIN localizacao
ON localizacao.id = jogo.localizacao_id;

-- Identificar o nome dos jogos da seção de jogo de aventura
SELECT jogo.nome, localizacao.prateleira
FROM jogo INNER JOIN localizacao
ON localizacao.id = jogo.localizacao_id
WHERE secao = 'Aventura';

-- Identificar TODAS as seções e os respectivos nomes dos jogos
-- ordenando em ordem crescente pelo nome do jogo.
SELECT jogo.nome, localizacao.prateleira, localizacao.secao
FROM localizacao LEFT JOIN jogo 
ON localizacao.id = jogo.localizacao_id
ORDER BY jogo.nome ASC;

-- AGREGAÇÃO -- 
-- valor do jogo de maior preço (valor).
SELECT MAX(valor) FROM jogo;
select * from jogo;

-- valor do jogo de menor preço (valor).
SELECT jogo.nome, MIN(valor)
FROM jogo
WHERE jogo.valor = (SELECT MIN(valor) FROM jogo);

-- valor médio jogo de  guerra
SELECT AVG(valor) AS "Media guerra"
FROM jogo INNER JOIN localizacao
ON localizacao.id = jogo.localizacao_id
WHERE localizacao.secao = "Guerra";

-- retorne valor total da loja
SELECT SUM(valor) AS "Total" FROM jogo;

# Aqui é outra file
Use supergames;

SELECT * FROM localizacao;

INSERT jogo VALUES 
(0, "Super Metroid", 205.00, 7),
(0, "DKC 2", 100.00, 8),
(0, "FF XV", 120.00, 5),
(0, "Xenoblade 2", 199.00, 6);

SELECT * FROM jogo;

/* Alterar valor dos jogos em promoção */
UPDATE jogo SET valor = valor * 0.5
WHERE cod = 2;

CREATE TABLE promocao (
	Promo INT(3) PRIMARY KEY AUTO_INCREMENT,
    Cod_Jogo INT(3) NOT NULL,
    FOREIGN KEY (Cod_jogo)
		REFERENCES Jogo (Cod)
);

/* inserção dos jogos em promoção */
INSERT promocao VALUES (0,1), (0,2);

/* Selecionar os jogos em promoção */
SELECT jogo.nome, jogo.valor
FROM jogo
WHERE jogo.cod IN (SELECT cod_jogo FROM promocao);

/* Selecionar os jogos que não estão em promocao */
SELECT jogo.nome, jogo.valor
FROM jogo
WHERE jogo.cod NOT IN (SELECT cod_jogo FROM promocao);

/* Jogos mais baratos com subconsultas e funções de agregação */
SELECT nome AS 'mais barato'
FROM jogo WHERE valor = SOME (SELECT MIN(valor) FROM jogo);



