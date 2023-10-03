# Banco-de-dados
--comando para seleção
SELECT * FROM produtos ORDER BY id ASC;

--comando para seleção
--DDL - Linguagem de Definição de Dados(Interagir com a tabela inteira)
--criação de tabela
CREATE TABLE produtos(
	id INT PRIMARY KEY NOT NULL,
	nome VARCHAR(255),
	preco DECIMAL(10,2)
);

--Excluir uma tabela
DROP TABLE produtos;

--alterar tabela
ALTER TABLE produtos ADD descricao TEXT;

--DML - Linguagem de manipulação de dados (mexer com linhas)



--Inserir dado na tabela
INSERT INTO produtos(id, nome, preco)VALUES
(1, 'Nescau Radical',8.00),
(2, 'Yakult activia',4.00),
(3, 'Alpino amargo', 5.00),
(4, 'Miojo Monica', 1.00);

--Alterar dado da tabela
UPDATE produtos SET preco = 15.00 WHERE id =1;
UPDATE produtos SET nome = 'Toddy' WHERE nome LIKE 'Nescau Radical';
UPDATE produtos SET descricao = 'Armazenar em ambiente gelado'
WHERE id=2 or id=3;

UPDATE produtos SET descricao = 'Promoção'
WHERE preco < 5.00;

--Deletar dado de uma tabela
DELETE FROM produtos WHERE nome LIKE'Alpino amargo'
DELETE FROM produtos WHERE ID = 1;

--Comando DQL (Linguagem de consulta de dados)
--selecionar todos(*) produtos, ordene ascendente
SELECT * FROM produtos ORDER BY id ASC;

--Ordenar pelo preço crescente(menor para maior)
SELECT * FROM produtos ORDER BY preco ASC;
--Ordenar pelo preço decrescente(maior para menor)
SELECT * FROM produtos ORDER BY preco DESC;
--Selecionar Colunas
SELECT nome, preco FROM produto ORDER BY id;
--Selectionar produtos maior que R$4,00
SELECT preco, nome FROM produtos WHERE preco > 4.00;
--Selecionar produto mais caro
SELECT MAX(preco) AS maior_valor FROM produtos;
SELECT preco, nome FROM produtos ORDER BY preco DESC limit 1;

--simulando busca por nome exato
SELECT * FROM produtos WHERE nome LIKE 'Nescau Radical';

--busca pelos primeiros caracteres, o resto não importa
SELECT * FROM produtos WHERE nome LIKE 'Ne%'
--A parte anterior não importa,busca o ultimo
SELECT * FROM produtos WHERE nome LIKE '%Fit';
--busca por qualquer parte do nome
SELECT * FROM produtos WHERE nome LIKE '%cha%';

-- Fase 2 -----------------------------------------
-- Criando tabela Funcionário----------------------

CREATE TABLE funcionario(
	id SERIAL PRIMARY KEY,
	nome VARCHAR(255) NOT NULL,
	data_nasc DATE,
	sexo CHAR(1),
	salario DECIMAL(10, 2),
	ativo BOOLEAN
);

INSERT INTO funcionario (id, nome, data_nasc, sexo, salario, ativo) VALUES
('Bob','1990-05-15', 'M', 2000.00, true),
('Augusto', '1970-04-01', 'M', 1500.00, false),
('Joanir', '2000-01-01', 'F', 1800.00, true),
('Elisa', '1995-10-02', 'F', 1900.00, true);

SELECT * FROM funcionario;

--Seleciona funcionários que nasceram após 01-01-1992
SELECT * FROM funcionario WHERE data_nasc > '1992-01-01';

--Seleciona funcionários em um período
SELECT nome, data_nasc FROM funcionario
WHERE data_nasc BETWEEN '1990-01-01' AND '1997-01-01';

--Contabilizar
SELECT sexo, 
COUNT(*) AS total_pessoas,         --Contar
AVG(salario) AS media_salarial    --Média
FROM funcionario GROUP BY sexo;

--Seleciona funcionario inativo
SELECT * FROM funcionario WHERE ativo = false;
SELECT * FROM clientes;
-- 1.Criar uma tabela "Clientes" com campos para ID, Nome e Email.
CREATE TABLE Clientes (
	id SERIAL PRIMARY KEY,
	nome VARCHAR(60),
	email VARCHAR(60)
); 

-- 2.Adicionar uma coluna "Telefone" à tabela "Clientes" usando ALTER TABLE.
ALTER TABLE clientes ADD telefone VARCHAR(20);

-- 3.Remover a coluna "Email" da tabela "Clientes" usando ALTER TABLE.
ALTER TABLE clientes DROP email;

-- 4.Criar uma tabela "Itens" com campos para ID, Nome e Preço.

CREATE TABLE itens(
	id SERIAL PRIMARY KEY,
	nome VARCHAR(60),
	preco DECIMAL(10,2)
);
SELECT * FROM itens;

-- 5.Inserir um novo cliente na tabela "Clientes" INSERT.
INSERT INTO clientes(nome, telefone) VALUES
('Jucelino','(44)98821-4455'),
('Marcelino','(44)98854-2546'),
('Virgulino','(44)99854-2469');

-- 6.Inserir 3 novos itens na tabela "Itens" INSERT.
INSERT INTO itens(id, nome, preco) VALUES
(2, 'Sabão', 4.00),
(3, 'Red Bull', 8.00),
(4, 'Coca Cola', 10.00);

-- 7.Atualizar o nome de um cliente na tabela "Clientes" UPDATE.
UPDATE clientes SET nome = 'Gilberto Gil' WHERE id=1;
UPDATE clientes SET nome = 'Gilberto Gil' WHERE nome LIKE 'Jucelino';

-- 8.Atualizar o preço de um item na tabela "Itens" UPDATE.
UPDATE itens SET preco = 7.00 WHERE id=3;

-- 9.Excluir um cliente da tabela "Clientes" DELETE.
DELETE FROM clientes WHERE id=2;

-- 10.Excluir um item da tabela "Itens" DELETE.
DELETE FROM itens WHERE id=2;

SELECT * FROM clientes;
SELECT * FROM itens;

DROP TABLE estados;
DROP TABLE cidades;

--Entendendo Relacionamentos entre tabelas no banco de dados

--Criação das tabelas
CREATE TABLE estados(
    id_estado SERIAL PRIMARY KEY,
	nome_estado VARCHAR(50) NOT NULL,
	sigla CHAR(2)
	
);

CREATE TABLE cidades(
    id_cidade SERIAL PRIMARY KEY,      
	nome_cidade VARCHAR(50),
	cep VARCHAR(50),
	id_estado INT REFERENCES estados(id_estado)  
	
);	
 
SELECT * FROM estados;
SELECT * FROM cidades;

-- INSERINDO DADOS NAS TABELAS
INSERT INTO estados(id_estado, nome_estado, sigla) VALUES
(1, 'Paraná','PR'),
(2, 'São Paulo','SP'),
(3,'Minas Gerais','MG');

INSERT INTO cidades(id_cidade, nome_cidade, cep, id_estado ) VALUES
(10, 'Nova Londrina', '87970-000', 1),
(11, 'Marilena', '87960-000', 1),
(12, 'Itaúna', '87980-000', 1),
(13, 'Primavera', '19273-000', 2),
(14, 'Rosana', '19274-000',2),
(15, 'Cachoeira da Prata', '35765-000',3);

SELECT * FROM estados;
SELECT * FROM cidades;

--TRABALHANDO COM RELACIONAMENTOS NO SQL
--2 RELACIONAMENTOS
--SELECIONAR TODAS AS CIDADES E SEUS ESTADOS
SELECT cidades.nome_cidade, estados.nome_estado
FROM estados INNER JOIN cidades
ON cidades.id_estado = estados.id_estado;

--SELECIONA TODAS AS CIDADES DO PARANÁ
SELECT cidades.id_cidade, cidades.nome_cidade, cidades.cep, estados.sigla
FROM estados INNER JOIN cidades
ON cidades.id_estado = estados.id_estado
WHERE estados.sigla LIKE 'SP' OR estados.sigla LIKE 'PR'
ORDER BY cidades.nome_cidade ASC;

--SELECIONA AS ESTADOS COM MAIS CIDADES
SELECT estados.nome_estado, COUNT(cidades.id_cidade) AS total_cidades
FROM estados INNER JOIN cidades
ON estados.id_estado = cidades.id_estado
GROUP BY estados.nome_estado
ORDER BY total_cidades DESC;

--CRIANDO MAIS UMA TABELA PARA O RELACIONAMENTO DE CIDADE-ESTADO
CREATE TABLE pessoa(
     id_pessoa SERIAL PRIMARY KEY,
     nome_pessoa VARCHAR(60),
     data_nascimento DATE,
     sexo CHAR(1),
	 estado_civil VARCHAR(60),
	 profissao VARCHAR(60),
	 id_cidade INT REFERENCES cidades(id_cidade)
);

INSERT INTO pessoa 
(id_pessoa, nome_pessoa, data_nascimento, sexo, estado_civil, profissao, id_cidade)
VALUES
(1, 'Marcele', '2000-01-01', 'f', 'solteira', 'Arquiteta',10),
(2, 'Ananias', '1980-02-20', 'm', 'casado', 'Carpinteiro',10),
(3, 'Silvio', '1950-10-10', 'm', 'casado','dublador', 11),
(4, 'Léo', '2001-01-02', 'm', 'casado','Entregador', 11),
(5, 'Chris', '1990-05-05','m', 'solteiro', 'Fisiculturista', 12),
(6, 'Ana','1970-03-01', 'f', 'solteira','Cantora',13),
(7, 'Jaime', '1970-03-01', 'm', 'casado', 'Carteiro',14),
(8, 'Alice', '2002-01-10', 'f', 'solteira', 'Advogada',15),
(9, 'Valentina', '1995-05-03', 'f','solteira','Zootecnista', 15),
(10, 'Laura', '1998-05-09', 'f', 'solteira', 'Balconista',15);

SELECT * FROM pessoa;

--Selecionar pessoa com sua cidade (relacionando tabela pessoa com cidade)
SELECT pessoa.nome_pessoa, cidade.cidade_nome
FROM pessoa INNER JOIN cidades
ON pessoa.id_cidade = cidades.id_cidade

--Seleciona Estado,Cidade,Pessoa
SELECT pessoa.nome_pessoa, cidades.nome_cidade,estados.nome_estado
FROM pessoa INNER JOIN cidades
ON pessoa.id_cidade = cidades.id_cidade
INNER JOIN estados
ON cidades.id_estado = estados.id_estado;


--Seleciona pessoas do Estado do Paraná
SELECT pessoa.nome_pessoa, estados.nome_estado, cidades.nome_cidade
FROM estados INNER JOIN cidades
ON estados.id_estado = cidades.id_estado
INNER JOIN pessoa
ON cidades.id_cidade = pessoa.id_cidade
WHERE estados.nome_estado LIKE 'Paraná';

--Selecione o nome da pessoa, profissão e qual cidade pertence
SELECT pessoa.nome_pessoa, cidades.nome_cidade, pessoa.profissao
FROM pessoa INNER JOIN cidades
ON pessoa.id_cidade = cidades.id_cidade

--Selecionar cidade com mais mulher/homem
SELECT cidades.nome_cidade, COUNT(*)AS Quantidade
FROM cidades INNER JOIN pessoa
ON cidades.id_cidade = pessoa.id_cidade
WHERE pessoa.sexo = 'f'
GROUP BY cidades.id_cidade
ORDER BY Quantidade desc;

--1 - Selecione todas as pessoas
SELECT pessoa.nome_pessoa FROM pessoa

--2 - Selecione todas as pessoas do sexo Masculino "m";
SELECT pessoa.nome_pessoa FROM pessoa
WHERE pessoa.sexo = 'm';

--3 - Selecione todas as pessoas do estado civil solteiro;
SELECT pessoa.nome_pessoa,pessoa.estado_civil FROM pessoa
WHERE pessoa.estado_civil LIKE 'solteiro'
OR pessoa.estado_civil LIKE 'solteira';


--4 - Selecione as pessoas e suas profissões
SELECT pessoa.nome_pessoa, pessoa.profissao FROM pessoa;

--5 - Selecione as pessoas que nasceram entre 1980-01-01 e 2000-01-01
SELECT pessoa.nome_pessoa, pessoa.data_nascimento
FROM pessoa WHERE data_nascimento BETWEEN '1980-01-01'AND '2000-01-01';

--6 - Selecione as pessoas do Estado de São Paulo
SELECT pessoa.nome_pessoa,estados.nome_estado
FROM pessoa INNER JOIN cidades
ON pessoa.id_cidade = cidades.id_cidade
INNER JOIN estados
ON cidades.id_estado = estados.id_estado
WHERE estados.nome_estado LIKE 'São Paulo';


--Selecione as pessoas de Marilena
SELECT pessoa.nome_pessoa,cidades.nome_cidade
FROM pessoa INNER JOIN cidades
ON pessoa.id_cidade = cidades.id_cidade
WHERE cidades.nome_cidade LIKE 'Marilena';



