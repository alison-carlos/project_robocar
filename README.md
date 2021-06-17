# Projeto Robocar

O projeto será dividido em 4 etapas.

1º - A locadora que controlar a locação de veículos.


2º - Ela deseja fornecer dados analíticos para os gestores.


3º - Ter os contratos armazenados digitalmentes em algum banco de dados.


4º - Gerenciar diariamente o número de contratos de risco.

________________________________________________________________________________________

## Modelo relacional projetado

![Desenho do Modelo Relacional](images/relacional.png)

## Scripts - Criação das tabelas relacionas

```sql

/*Deleta o banco de dados caso ele exista.*/

drop database if exists locadora;

/*Cria o banco de dados*/

create database locadora;

-- Conecta no banco 

\c locadora;

-- Altera o datestyle do banco

set datestyle to PostgreSQL, Europena;

-- Dropa as tabelas caso exista

DROP TABLE IF EXISTS locacao;
DROP TABLE IF EXISTS clientes;
DROP TABLE IF EXISTS despachantes;
DROP TABLE IF EXISTS veiculos;

-- Cria as tabelas

create table if not exists clientes 
(
	idcliente integer not null,
	cpf character varying(11) not null,
	cnh character varying(11) not null,
	validadecnh date not null,
	nome character varying(15) not null,
	datacadastro date not null,
	datanascimento date not null,
	telefone character varying(11),
	status character varying(7) not null,
	constraint clientes_pkey primary key (idcliente)
);

create table if not exists despachantes 
(
	iddespachante integer not null,
	nome character varying(150) not null,
	status character varying(7) not null,
	filial character varying(15) not null,
	constraint despachantes_pkey primary key (iddespachante)
);

create table if not exists veiculos
(
	idveiculo integer not null,
	dataaquisicao date not null,
	ano integer not null,
	modelo character varying(150) not null,
	placa character varying (10) not null,
	status character varying(12) not null,
	diaria numeric(10,2) not null,
	constraint veiculos_pkey primary key (idveiculo)
);

create table if not exists locacao
(
	idlocacao integer not null,
	idcliente integer not null references clientes(idcliente),
	iddespachante integer not null references despachantes(iddespachante),
	idveiculo integer not null references veiculos(idveiculo),
	datalocacao date not null,
	dataentrega date,
	total numeric(10,2),
	constraint locacao_pkey primary key (idlocacao, idcliente, iddespachante)
);

```

![Imagem do banco de dados Postgres](images/transaction_database.png)
