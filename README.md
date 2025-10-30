-- ======================
-- Entidades e Atributos.
-- ======================

-- Voluntario
o	IdVoluntario (PK)
o	NomeCompleto
o	Funcao (Ex: Coordenador, Cuidador)

-- Canteiro
IdCanteiro (PK)
Localizacao
Tamanho_m2
Descricao

-- Especie
IdEspecie (PK) (Ex.: Girassol: 01)
NomePopular
NomeCientifico
TempoMedioCiclo_dias

-- Instituicao
IdInstituicao (CNPJ - PK)
Nome
Responsavel

-- Cultivo (Entidade de evento)
IdCultivo (PK) (Ex.: G01)
DataPlantio
QuantidadePlantada
Status (Ex: "Crescendo", "Colhido", "Perdido")

-- Colheita (Entidade de evento)
IdColheita (PK)
DataColheita
QuantidadeColhida_kg

-- Doacao (Entidade de evento)
IdDoacao (PK)
DataDoacao

-- ===================
-- Chaves Primárias (PK).
-- ===================

Voluntario: IdVoluntario
Canteiro: IdCanteiro
Especie: IdEspecie
Instituicao: IdInstituicao
Cultivo: IdCultivo
Colheita: IdColheita
Doacao: IdDoacao

-- =================================
-- Relacionamentos e Cardinalidades.
-- =================================
•	Voluntário (N) --- (N) Canteiro
o	Descrição: Um voluntário pode cuidar de muitos canteiros, e um canteiro pode ser cuidado por muitos voluntários.

•	Voluntário (1) --- (N) Cultivo
o	Descrição: Um voluntário pode realizar muitos cultivos. Cada cultivo é feito por um voluntário.

•	Voluntário (1) --- (N) Colheita
o	Descrição: Um voluntário pode realizar muitas colheitas. Cada colheita é feita por um voluntário.

•	Canteiro (1) --- (N) Cultivo
o	Descrição: Um canteiro pode ter muitos cultivos. Cada cultivo ocorre em um canteiro.

•	Espécie (1) --- (1) Cultivo
o	Descrição: Uma espécie pode ser cultivada muitas vezes. Cada cultivo é de uma espécie.

•	Cultivo (1) --- (N) Colheita
o	Descrição: Um cultivo pode gerar muitas colheitas. Cada colheita vem de um cultivo.

•	Instituicao (1) --- (N) Doacao
o	Descrição: Uma instituição pode receber muitas doações. Cada doação vai para uma instituição.

•	Doacao (N) --- (N) Colheita
o	Descrição: Uma doação pode conter produtos de várias colheitas. Uma colheita pode ser dividida em várias doações.

-- ===================
-- Tabela: Voluntario.
-- ===================
•	IdVoluntario (PK - CPF)
•	NomeCompleto

-- ===================
-- Tabela: Canteiro.
-- ===================
•	IdCanteiro (PK - Ex.: Can01)
•	Localizacao (Ex.: Can01 - Norte Superior)
•	Descricao

-- =================
-- Tabela: Especie.
-- =================
•	IdEspecie (PK)
•	NomePopular
•	NomeCientifico
•	TempoDias

-- ====================
-- Tabela: Instituicao.
-- ===================
•	IdInstituicao (PK - Ex.: CNPJ)
•	Nome
•	Responsavel

-- ===========================================================================
-- Tabela: Cuidado_Canteiro (Tabela Associativa para Voluntario N:N Canteiro).
-- ===========================================================================
•	FK_IdVoluntario (PK, FK referencia Voluntario)
•	FK_IdCanteiro (PK, FK referencia Canteiro)
•	DataInicioCuidado

-- ================
-- Tabela: Cultivo.
-- ================
•	IdCultivo (PK)
•	DataPlantio
•	QuantidadePlantada
•	Status
•	FK_IdVoluntario (FK referencia Voluntario)
•	FK_IdCanteiro (FK referencia Canteiro)
•	FK_IdEspecie (FK referencia Especie)

-- =================
-- Tabela: Colheita.
-- =================
•	IdColheita (PK)
•	DataColheita
•	QuantidadeColhida_kg
•	FK_IdVoluntario (FK referencia Voluntario)
•	FK_IdCultivo (FK referencia Cultivo)
Tabela: Doacao
•	IdDoacao (PK)
•	DataDoacao
•	FK_IdInstituicao (FK referencia Instituicao)
Tabela: Item_Doacao (Tabela Associativa para Doacao N:N Colheita)
•	FK_IdDoacao (PK, FK referencia Doacao)
•	FK_IdColheita (PK, FK referencia Colheita)
•	QuantidadeDoada_kg
________________________________________
Modelo Físico (MySQL)
SQL
-- ======================
-- Criação do Banco de Dados
-- ======================

CREATE DATABASE VerdeViva;
USE VerdeViva;

-- ==============
-- Tabelas principais
-- ==============

CREATE TABLE Voluntario (
    IdVoluntario INT (PK),
    NomeCompleto VARCHAR,
    Email VARCHAR,
    Funcao VARCHAR
);

CREATE TABLE Canteiro (
    IdCanteiro INT (PK),
    Localizacao VARCHAR,
    Descricao VARCHAR
);

CREATE TABLE Especie (
    IdEspecie INT (PK),
    NomePopular VARCHAR,
    NomeCientifico VARCHAR,
    TempoDias INT
);

CREATE TABLE Instituicao (
    ID_Instituicao INT (PK),
    Nome VARCHAR,
    CNPJ VARCHAR,
    Responsavel VARCHAR,
    Telefone VARCHAR
);

-- ==============
-- Tabela Associativa
-- ==============

CREATE TABLE Cuidado_Canteiro (
    FK_IdVoluntario INT,
    FK_IdCanteiro INT,
    DataInicio DATE,
    PK (FK_IdVoluntario, FK_IdCanteiro),
    FK (FK_IdVoluntario),
    FK (FK_IdCanteiro)
);

-- ========================
-- Tabelas de Eventos (com FKs)
-- ========================

CREATE TABLE Cultivo (
    IdCultivo INT (PK),
    DataPlantio DATE,
    QuantidadePlantada VARCHAR,
    Status VARCHAR,
    FK_IdVoluntario INT,
    FK_IdCanteiro INT,
    FK_IdEspecie INT,
    FK (FK_IdVoluntario),
    FK (FK_IdCanteiro),
    FK (FK_IdEspecie)
);

CREATE TABLE Colheita (
    IdColheita INT (PK),
    DataColheita DATETIME NULL,
    QuantidadeColhida_kg NULL,
    FK_IdVoluntario INT,
    FK_IdCultivo INT,
    FK (FK_IdVoluntario),
    FK (FK_IdCultivo)
);

CREATE TABLE Doacao (
    IdDoacao INT (PK),
    DataDoacao NULL,
    FK_IdInstituicao INT,
    Observacoes NULL,
    FK (FK_IdInstituicao)
);

-- ===============
-- Tabela Associativa
-- ===============

CREATE TABLE Item_Doacao (
    FK_IdDoacao INT,
    FK_IdColheita INT,
    QuantidadeDoada_kg NULL,
    PK (FK_IdDoacao, FK_IdColheita),
    FK (FK_IdDoacao),
    FK (FK_IdColheita)
);
________________________________________
Consultas SQL (DML)
1. Liste todos os voluntários cadastrados.
SQL
SELECT NomeCompleto, Funcao 
FROM Voluntario;

2. Mostre as plantas cultivadas.
SQL
SELECT NomePopular, TempoMedioCiclo_dias, NomeCientifico 
FROM Especie;

3. Mostre as plantas cultivadas em cada canteiro.
SQL
SELECT 
    c.Localizacao AS Canteiro,
    e.NomePopular AS Planta,
    cu.DataPlantio
FROM Cultivo AS cu
JOIN Canteiro AS c ON cu.FK_IdCanteiro = c.IdCanteiro
JOIN Especie AS e ON cu.FK_IdEspecie = e.IdEspecie;

4. Exiba os nomes dos voluntários e as plantas que eles cultivam.
SQL
SELECT DISTINCT
    v.NomeCompleto AS Voluntario,
    e.NomePopular AS Planta
FROM Cultivo AS cu
JOIN Voluntario AS v ON cu.FK_IdVoluntario = v.IdVoluntario
JOIN Especie AS e ON cu.FK_IdEspecie = e.IdEspecie
ORDER BY v.NomeCompleto;

5. Liste todas as colheitas realizadas.
SQL
SELECT 
    co.DataColheita,
    ca.Localizacao AS Canteiro,
    co.QuantidadeColhida_kg
FROM Colheita AS co
JOIN Cultivo AS cu ON co.FK_IdCultivo = cu.IdCultivo
JOIN Canteiro AS ca ON cu.FK_IdCanteiro = ca.IdCanteiro;

6. Mostra as instituições que receberam doações.
SQL
SELECT 
    i.Nome AS Instituicao,
    d.DataDoacao,
    e.NomePopular AS Produto,
    id.QuantidadeDoada_kg
FROM Item_Doacao AS id
JOIN Doacao AS d ON id.FK_IdDoacao = d.IdDoacao
JOIN Instituicao AS i ON d.FK_IdInstituicao = i.IdInstituicao
JOIN Colheita AS co ON id.FK_IdColheita = co.IdColheita
JOIN Cultivo AS cu ON co.FK_IdCultivo = cu.IdCultivo
JOIN Especie AS e ON cu.FK_IdEspecie = e.IdEspecie;

7. Liste o total de quilos doados por instituição usando GROUP BY.
SQL
SELECT 
    i.Nome AS Instituicao,
    SUM(id.QuantidadeDoada_kg) AS Total_Doado_kg
FROM Item_Doacao AS id
JOIN Doacao AS d ON id.FK_IdDoacao = d.IdDoacao
JOIN Instituicao AS i ON d.FK_IdInstituicao = i.IdInstituicao
GROUP BY i.Nome
ORDER BY Total_Doado_kg DESC;

8. Mostre os canteiros que ainda não tiveram colheitas usando LEFT JOIN.
SQL
SELECT 
    c.ID_Canteiro,
    c.Localizacao
FROM Canteiro AS c
LEFT JOIN Cultivo AS cu ON c.IdCanteiro = cu.FK_IdCanteiro
LEFT JOIN Colheita AS co ON cu.IdCultivo = co.FK_IdCultivo
WHERE co.IdColheita IS NULL
GROUP BY c.IdCanteiro, c.Localizacao;
-- (Nos nossos dados de exemplo, o Canteiro 3 apareceria aqui)

9. Exiba o voluntário que realizou o maior número de cultivos usando COUNT e ORDER BY.
SQL
SELECT 
    v.NomeCompleto,
    COUNT(cu.IdCultivo) AS Total_Cultivos
FROM Voluntario AS v
JOIN Cultivo AS cu ON v.IdVoluntario = cu.FK_IdVoluntario
GROUP BY v.IdVoluntario
ORDER BY Total_Cultivos DESC
LIMIT 1;

10. Mostra as plantas que ainda não foram colhidas.
SQL
SELECT DISTINCT
    e.NomePopular
FROM Especie AS e
JOIN Cultivo AS cu ON e.IdEspecie = cu.FK_IdEspecie

11. Liste as doações realizadas em setembro de 2025.
SQL
SELECT 
    i.Nome AS Instituicao,
    d.DataDoacao
FROM Doacao AS d
JOIN Instituicao AS i ON d.FK_IdInstituicao = i.IdInstituicao
WHERE MONTH(d.DataDoacao) = 9 AND YEAR(d.DataDoacao) = 2025;


# Horta_Comuitaria_Modulo_03
A Horta Comunitária é administrada por voluntários da escola técnica e tem como objetivo produzir hortaliças e legumes que são doados a instituições sociais da cidade. 
