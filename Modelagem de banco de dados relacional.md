# Modelagem de banco de dados relacional: entendendo SQL

# Link do Curso

[Curso Online Modelagem de banco de dados relacional: entendendo SQL | Alura](https://cursos.alura.com.br/course/modelagem-banco-dados-relacional-sql)

# Materiais de Apoio

## Principais tipo de dados em SQL

| Categoria | Descrição | Exemplo | Comando |
| --- | --- | --- | --- |
| Numéricos exatos | Inteiros de vários tamanhos que podem ser formatados | 9.78 pode ser definida como decimal(3,2) 9 é número inteiro é do tipo int. | int, smallint, decimal(i,j), numeric(i,j), dec(i,j) |
| Numéricos aproximados | Números de ponto flutuante com precisão | 7.90 é do tipo float | float ou real, double precision |
| Cadeias de caracteres | Textos de tamanhos fixos | “modelagem” é char(9) | char(n) ou character(n) |
|  | Texto de tamanho variável |  | varchar(n) ou char varying(n) ou character varying(n) |
| Valores lógicos | Termos que representa verdadeiro ou falso |  | true, false ou unknown |
| Datas | Datas, dias, mês, anos. | Calendário lunar, calendário comercial | Date DD-MM-YYYY ou YYYY-MM-DD- |
|  | Tempo | 10:59:13 é tipo HH:MM:SS | HH:MM:SS, timestamp |

### Scripts

[Scripts_BD_ALURA.zip](Modelagem%20de%20banco%20de%20dados%20relacional%20entendendo%20%208e12b4997fe8463db3203c785b08cf14/Scripts_BD_ALURA.zip)

[sql_clube_do_livro_alura_AMANDA.sql](Modelagem%20de%20banco%20de%20dados%20relacional%20entendendo%20%208e12b4997fe8463db3203c785b08cf14/sql_clube_do_livro_alura.sql)

### Funções de agregação

[Trabalhando com funções de agregação | Alura](https://www.alura.com.br/artigos/trabalhando-funcoes-de-agregacao)

### Diagrama Venn SQL Joins

![Untitled](Modelagem%20de%20banco%20de%20dados%20relacional%20entendendo%20%208e12b4997fe8463db3203c785b08cf14/Untitled.png)

[SQL JOIN: Aprenda INNER, LEFT, RIGHT, FULL e CROSS | Alura](https://www.alura.com.br/artigos/join-em-sql)

## Esqueleto para a criação de tabelas

```sql
CREATE TABLE NOME_TABELA (
	NOME_CAMPO_1 TIPO_CAMPO_1,
	NOME_CAMPO_2 TIPO_CAMPO_1,
PRIMARY KEY (NOME_CAMPO_1)
);
```

# Aulas

## Introdução e Instalação:

Modelo relacional do projeto/problema:

![Untitled](Modelagem%20de%20banco%20de%20dados%20relacional%20entendendo%20%208e12b4997fe8463db3203c785b08cf14/Untitled%201.png)

> SQL é uma sigla em inglês para **Structured Query Language** que pode ser livremente traduzida para **Linguagem de Consulta Estruturada** e é uma **linguagem padrão**
 para trabalhar com **bancos de dados relacionais.**
> 

### Instalação

O *Uniform server* é um conjunto de servidores que possui o MySQL e simplifica a instalação. Entretanto, no site do [MySQL](https://dev.mysql.com/downloads/installer/) é possível instalar de forma autônoma se preferir.

[Uniform Server](https://sourceforge.net/projects/miniserver/)

[https://sourceforge.net/projects/miniserver/files/Uniform Server ZeroXIV/14_0_3_ZeroXIV/14_0_3_ZeroXIV.exe/download](https://sourceforge.net/projects/miniserver/files/Uniform%20Server%20ZeroXIV/14_0_3_ZeroXIV/14_0_3_ZeroXIV.exe/download)

## Esquemas e Tabelas:

<aside>
❗ obs: 
- Para encerrar um comando, usa-se ponto e vígula (;);
- o símbolo de raio ⚡ executa o comando selecionado (ou CTRL + ENTER);

</aside>

![Untitled](Modelagem%20de%20banco%20de%20dados%20relacional%20entendendo%20%208e12b4997fe8463db3203c785b08cf14/Untitled%202.png)

Atualize a lista de esquemas e aparecerá o esquema do clube do livro:

![Untitled](Modelagem%20de%20banco%20de%20dados%20relacional%20entendendo%20%208e12b4997fe8463db3203c785b08cf14/Untitled%203.png)

Com o ambiente criado, podemos iniciar a criação das tabelas:

### Criando tabela

- Livros

```sql
CREATE TABLE LIVROS (
	ID_LIVRO INT NOT NULL,
	NOME_LIVRO VARCHAR(100) NOT NULL,
	AUTORIA VARCHAR(100) NOT NULL,
	EDITORA VARCHAR(100) NOT NULL,
	CATEGORIA VARCHAR(100) NOT NULL,
	PREÇO DECIMAL(5,2) NOT NULL,#5 DÍGITOS, SENDO 2 DEPOIS DA VÍGULA

PRIMARY KEY (ID_LIVRO)
);
```

- Estoque

```sql
CREATE TABLE ESTOQUE (
    ID_LIVRO INT NOT NULL,
    QTD_ESTOQUE INT NOT NULL,
 PRIMARY KEY (ID_LIVRO)
);
```

- Vendas

```sql
CREATE TABLE VENDAS (
    ID_PEDIDO INT NOT NULL,
    ID_VENDEDOR INT NOT NULL,
    ID_LIVRO INT NOT NULL,
    QTD_VENDIDA INT NOT NULL,
    DATA_VENDA DATE NOT NULL,
 PRIMARY KEY (ID_VENDEDOR,ID_PEDIDO)
);
```

- Vendedores

```sql
CREATE TABLE VENDEDORES (
    ID_VENDEDOR INT NOT NULL,
    NOME_VENDEDOR VARCHAR(255) NOT NULL,
 PRIMARY KEY (ID_VENDEDOR)
);
```

### Integridades referencias

Alterando a tabela estoque, inserindo uma chave estrangeira e declarando uma restrição.

> ADD CONSTRAINT    CE_ESTOQUE_LIVROS
adicionar restrição      nome_da_restrição
> 
- relação entre a tabela estoque e livros

```sql
ALTER TABLE ESTOQUE ADD CONSTRAINT CE_ESTOQUE_LIVROS
FOREIGN KEY (ID_LIVRO) # campo da tabela estoque que é a chave estrangeira (CE) 
REFERENCES LIVROS (ID_LIVRO) # essa CE referencia ID_LIVRO da tabela LIVROS

# Comandos para criar um erro na tentativa de alterar um livro
 # na tabela estoque que não está na tabela livros.
ON DELETE NO ACTION
ON UPDATE NO ACTION;
```

- relação entre a vendas estoque e livros

```sql
ALTER TABLE VENDAS ADD CONSTRAINT CE_VENDAS_LIVROS
FOREIGN KEY (ID_LIVRO)
REFERENCES LIVROS (ID_LIVRO)
ON DELETE NO ACTION
ON UPDATE NO ACTION;
```

- relação entre a tabela vendedores e vendas

```sql
ALTER TABLE VENDAS ADD CONSTRAINT CE_VENDAS_VENDEDORES
FOREIGN KEY (ID_VENDEDOR)
REFERENCES VENDEDORES (ID_VENDEDOR)
ON DELETE NO ACTION
ON UPDATE NO ACTION;
```

## Inserindo dados:

Para inserir dados, é necessário desativar as chaves estrangeiras pelo comando:

```sql
SET FOREIGN_KEY_CHECKS = 0;
```

A inserção de dados deve seguir a mesma ordem de criação dos atributos da tabela, como a imagem abaixo:

![Untitled](Modelagem%20de%20banco%20de%20dados%20relacional%20entendendo%20%208e12b4997fe8463db3203c785b08cf14/Untitled%204.png)

Assim, foi possível cadastrar o primeiro livro ao banco de dados. Como todos os campos foram registrados com o NOT NULL, não pode ter campos vazios na tabela LIVROS.

```sql
INSERT INTO LIVROS VALUES (
 1,
'Percy Jackson e o Ladrão de Raios',
'Rick Riordan',
'Intrínseca',
'Aventura',
34.45 #sepadador decimal é o ponto.
);
```

É possível cadastrar os dados em blocos, separando-os por vírgula, da seguinte forma:

- livros:

```sql
INSERT INTO LIVROS VALUES
(2, 'A Volta ao Mundo em 80 Dias',     'Júlio Verne',        'Principis',     'Aventura', 21.99),
(3, 'O Cortiço',                       'Aluísio de Azevedo', 'Panda Books',   'Romance',  47.8),
(4, 'Dom Casmurro',                    'Machado de Assis',   'Via Leitura',   'Romance',  19.90),
(5, 'Memórias Póstumas de Brás Cubas', 'Machado de Assis',   'Antofágica',    'Romance',  45),
(6, 'Quincas Borba',                   'Machado de Assis',   'L&PM Editores', 'Romance',  48.5),
(7, 'Ícaro',                           'Gabriel Pedrosa',    'Ateliê',        'Poesia',   36),
(8, 'Os Lusíadas',                     'Luís Vaz de Camões', 'Montecristo',   'Poesia',   18.79),
(9, 'Outros Jeitos de Usar a Boca',    'Rupi Kaur',          'Planeta',        'Poesia',  34.8);
```

- vendendores:

```sql
INSERT INTO VENDEDORES VALUES
(1,'Paula Rabelo'),
(2,'Juliana Macedo'),
(3,'Roberto Barros'),
(4,'Barbara Jales');
```

- vendas:

```sql
INSERT INTO VENDAS VALUES 
(1, 3, 7, 1, '2020-11-02'),
(2, 4, 8, 2, '2020-11-02'),
(3, 4, 4, 3, '2020-11-02'),
(4, 1, 7, 1, '2020-11-03'),
(5, 1, 6, 3, '2020-11-03'),
(6, 1, 9, 2, '2020-11-04'),
(7, 4, 1, 3, '2020-11-04'),
(8, 1, 5, 2, '2020-11-05'),
(9, 1, 2, 1, '2020-11-05'),
(10, 3, 8, 2, '2020-11-11'),
(11, 1, 1, 4, '2020-11-11'),
(12, 2, 10, 10, '2020-11-11'),
(13, 1, 12, 5, '2020-11-18'),
(14, 2, 4, 1, '2020-11-25'),
(15, 3, 13, 2,'2021-01-05'),
(16, 4, 13, 1, '2021-01-05'),
(17, 4, 4, 3, '2021-01-06'),
(18, 2, 12, 2, '2021-01-06');
```

- estoque:

```sql
INSERT INTO ESTOQUE VALUES
(1,  7),
(2,  10),
(3,  2),
(8,  4),
(10, 5),
(11, 3),
(12, 3);
```

### Informações fora de ordem:

É necessário informar a ordem dos dados que serão inseridos.

```sql
#Inserindo valores fora de ordem
INSERT INTO LIVROS
(CATEGORIA, AUTORIA, NOME_LIVRO, EDITORA, ID_LIVRO, PREÇO)
VALUES
('Biografia' ,    'Malala Yousafzai', 'Eu sou Malala'       , 'Companhia das Letras', 11, 22.32),
('Biografia' ,    'Michelle Obama'  , 'Minha história'      , 'Objetiva'            , 12, 57.90),
('Biografia' ,    'Anne Frank'      , 'Diário de Anne Frank', 'Pe Da Letra'         , 13, 34.90);
```

### Excluindo algum valor

```sql
# Excluindo uma tabela
DROP TABLE VENDEDORES;
```

## Consultando e alterando dados:

Para selecionar todas as linhas e colunas de uma tabela:

```sql
SELECT * FROM LIVROS;
```

Executando o código, teremos:

![Untitled](Modelagem%20de%20banco%20de%20dados%20relacional%20entendendo%20%208e12b4997fe8463db3203c785b08cf14/Untitled%205.png)

Para selecionar apenas uma coluna, é necessário informar o nome da coluna:

```sql
SELECT NOME_LIVRO FROM LIVROS;
```

É possível também, apelidar uma coluna, da seguinte forma:

![Untitled](Modelagem%20de%20banco%20de%20dados%20relacional%20entendendo%20%208e12b4997fe8463db3203c785b08cf14/Untitled%206.png)

### Filtros:

Para selecionar apenas as biografias:

![Untitled](Modelagem%20de%20banco%20de%20dados%20relacional%20entendendo%20%208e12b4997fe8463db3203c785b08cf14/Untitled%207.png)

Para colocar mais de um restrição, usa o comando AND:

```sql
SELECT * FROM LIVROS
WHERE CATEGORIA = "ROMANCE" AND PREÇO <48;
```

```sql
SELECT * FROM LIVROS
WHERE CATEGORIA = "POESIA" AND NOT (AUTORIA = "Luís Vaz de Camões" OR AUTORIA = "Gabriel Pedrosa" );
```

Podemos fazer pesquisa indicando parte de uma palavra, como por exemplo: 

- Retornar todos os nomes de cidades que terminam com ‘lândia’:

```sql
SELECT CIDADE, ESTADO
FROM MAPA
WHERE CIDADE LIKE '%LÂNDIA';
```

- Retornar todos os nomes de cidades que começam com ‘A’:

```sql
SELECT CIDADE, ESTADO
FROM MAPA
WHERE CIDADE LIKE 'A%';
```

- Retornar todos os nomes de cidades que tenham a sílaba ‘ma’:

```sql
SELECT CIDADE, ESTADO
FROM MAPA
WHERE CIDADE LIKE '%MA%';
```

- Para não mostrar valores repetidos, podemos usar o DISTINCT e ORDER BY para ordenar:
    - O seguinte exemplo mostra todos os livros vendidos pelo vendedor 1.

```sql
SELECT DISTINCT ID_LIVRO FROM VENDAS
WHERE ID_VENDEDOR = 1;
ORDER BY ID_LIVRO;
```

- Deletando informação

```sql
DELETE FROM LIVROS WHERE ID_LIVRO = 8; # TIRANDO OS LUSIADAS DO BANCO DE DADOS
```

- alterando valores

```sql
UPDATE LIVROS SET PREÇO = 0.9*PRECO;
```

## Unindo Tabelas

```sql
SELECT VENDAS.ID_VENDEDOR, VENDEDORES.NOME_VENDEDOR, SUM(VENDAS.QTD_VENDIDA)
FROM VENDAS, VENDEDORES
WHERE VENDAS.ID_VENDEDOR = VENDEDORES.ID_VENDEDOR
GROUP BY VENDAS.ID_VENDEDOR;
```

- Funções de agregação - métrica
    - `MAX`: a partir de um conjunto de valores é retornado o maior entre eles;
    - `MIN`: analisa um grupo de valores e retorna o menor entre eles;
    - `SUM`: calcula o somatório dos valores de um campo específico;
    - `AVG`: realiza a média aritmética dos valores de uma determinada coluna; e
    - `COUNT`: contabiliza a quantidade de linhas selecionadas.

### Inner Join

O `INNER JOIN` é a **interseção** entre duas tabelas, ou seja, na consulta aparecerá todas as informações de um determinado campo da tabela A que também foi encontrado na tabela B.

![https://caelum-online-public.s3.amazonaws.com/2462-entendendo-sql/05/aula5-img1.png](https://caelum-online-public.s3.amazonaws.com/2462-entendendo-sql/05/aula5-img1.png)

```sql
#esquema de uso
SELECT <CAMPOS>
FROM TABELA_A
INNERJOIN TABELA_B
ON TABELA_A.CAMPO = TABELA_B.CAMPOCOPIAR CÓDIGO
```

```sql
#exemplo
SELECT VENDAS.ID_VENDEDOR, VENDEDORES.NOME_VENDEDOR, SUM(VENDAS.QTD_VENDIDA)
FROM VENDAS INNER JOIN VENDEDORES
ON VENDAS.ID_VENDEDOR = VENDEDORES.ID_VENDEDOR
GROUP BY VENDAS.ID_VENDEDOR;
```

### Left Join

O `LEFT JOIN` baseia-se nas informações da tabela declarada à esquerda do comando ao se juntar com outra tabela.

![https://caelum-online-public.s3.amazonaws.com/2462-entendendo-sql/05/aula5-img2.png](https://caelum-online-public.s3.amazonaws.com/2462-entendendo-sql/05/aula5-img2.png)

```sql
#esquema de uso
SELECT <CAMPOS>
FROM TABELA_A
LEFTJOIN TABELA_B
ON TABELA_A.CAMPO = TABELA_B.CAMPOCOPIAR CÓDIGO
```

```sql
#exemplo
SELECT LIVROS.NOME_LIVRO, VENDAS.QTD_VENDIDA
FROM LIVROS LEFT JOIN VENDAS
ON LIVROS.ID_LIVRO = VENDAS.ID_LIVRO;
```

### **Right Join**

![https://caelum-online-public.s3.amazonaws.com/2462-entendendo-sql/05/aula5-img3.png](https://caelum-online-public.s3.amazonaws.com/2462-entendendo-sql/05/aula5-img3.png)

```sql
#esquema de uso
SELECT <CAMPOS>
FROM TABELA_A
RIGHTJOIN TABELA_B
ON TABELA_A.CAMPO = TABELA_B.CAMPOCOPIAR CÓDIGO
```

```sql
#exemplo
SELECT LIVROS.NOME_LIVRO, VENDAS.QTD_VENDIDA
FROM LIVROS RIGHT JOIN VENDAS
ON LIVROS.ID_LIVRO = VENDAS.ID_LIVRO;
```

### **Full Outer Join**

Esse comando apresenta a união entre duas tabelas.

1. Incluindo a interseção

![https://caelum-online-public.s3.amazonaws.com/2462-entendendo-sql/05/aula5-img4.png](https://caelum-online-public.s3.amazonaws.com/2462-entendendo-sql/05/aula5-img4.png)

```sql
#esquema de uso
SELECT <CAMPOS>
FROM TABELA_A
FULLOUTERJOIN TABELA_B
ON TABELA_A.CAMPO = TABELA_B.CAMPOCOPIAR CÓDIGO
```

ou

```sql
#esquema de uso
SELECT <CAMPOS>
FROM TABELA_A
LEFTJOIN TABELA_B
ON TABELA_A.CAMPO = TABELA_B.CAMPO

UNIONSELECT <CAMPOS>
FROM TABELA_A
RIGHTJOIN TABELA_B
ON TABELA_A.CAMPO = TABELA_B.CAMPOCOPIAR CÓDIGO
```

### Excluindo a interseção

![https://caelum-online-public.s3.amazonaws.com/2462-entendendo-sql/05/aula5-img5.png](https://caelum-online-public.s3.amazonaws.com/2462-entendendo-sql/05/aula5-img5.png)

```sql
#esquema de uso
SELECT <CAMPOS>
FROM TABELA_A
INNERJOIN TABELA_B
ON TABELA_A.CAMPO = TABELA_B.CAMPO
WHERE TABELA_A.CAMPOISNULLOR TABELA_B.CAMPOISNULLCOPIAR CÓDIGO
```