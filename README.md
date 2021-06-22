# Projeto Robocar

Este projeto foi recriado a partir das vídeo aulas apresentadas no curso ***Formação Engenheiro de Dados: Domine Big Data!*** disponível neste [link](https://www.udemy.com/share/101CKgBEYcdldTRHw=/) ministrado pelo professor Fernando Amaral.

O mini projeto será dividido em 4 etapas.

***1º - A locadora que controlar a locação de veículos.***

Para isso será criado um banco de dados no Postgres onde serão criadas as tabelas.

***2º - Ela deseja fornecer dados analíticos para os gestores.***

Iremos realizar a importação dos dados do banco de dados Postgres para o Hive onde iremos realizar algumas consultar usando o Hue.

***3º - Ter os contratos armazenados digitalmentes em algum banco de dados.***

Iremos simular um ambiente de armazenamento no MongoDB

***4º - Gerenciar diariamente o número de contratos de risco.***

E ao final será executado uma pequena aplicação para buscar contratos de risco.
________________________________________________________________________________________

# 1º Etapa, criação do banco de dados relacional e carga dos dados.

## Modelo relacional projetado

![Desenho do Modelo Relacional](images/postgres_database/relacional.png)

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

### Executando a criação das tabelas

![Criando as tabelas relacionais no Postgres](images/postgres_database/create_table.png)

### Inserindo os dados 

![Inserindo os dados nas tabelas do banco Locadora](images/postgres_database/insert_table.png)

### Consulta na tabela clientes

![Consulta na tabela clientes](images/postgres_database/selects/clientes.png)

### Consulta na tabela despachantes

![Consulta na tabela despachantes](images/postgres_database/selects/despachantes.png)

### Consulta na tabela veiculos

![Consulta na tabela veiculos](images/postgres_database/selects/veiculos.png)

### Consulta na tabela Locacao

![Consulta na tabela locacao](images/postgres_database/selects/locacao.png)

________________________________________________________________________________________

# 2º Etapa, importação dos dados para o Hive.

Nesta etapa, usando o beeline, foi feito uma conexão no banco do Hive da VM e criado um database para receber os dados que estão no Postgresql.

![Conectando-se ao Hive e criando o banco de dados](images/hive_database/create_database.png)

