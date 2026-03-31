# SQL Learning Notes — Anotações e Exemplos Práticos
Este repositório reúne anotações dos principais comandos SQL utilizados em bancos de dados relacionais. O objetivo é servir como material de estudo e consulta rápida com conceitos, exemplos práticos e resultados esperados.

O conteúdo está organizado com:
- Conceito
- Exemplo em SQL
- Resultado esperado

## Base de dados utilizada nos exemplos

### customers

| id | name   | city     |
|----|--------|----------|
| 1  | Ana    | Mineiros |
| 2  | Pedro  | Goiânia  |
| 3  | Carlos | Mineiros |

### products

| id | name     | price | stock | category_id |
|----|----------|-------|-------|-------------|
| 1  | Notebook | 3500  | 5     | 1 |
| 2  | Mouse    | 80    | 30    | 1 |
| 3  | Teclado  | 150   | 20    | 1 |

### categories

| id | name |
|----|------|
| 1 | Eletrônicos |
| 2 | Escritório |

## SELECT

### Conceito
O comando SELECT é utilizado para consultar dados de uma tabela, podendo retornar todas as colunas ou apenas colunas específicas.

### Exemplo
```sql
SELECT * FROM products;

### Resultado

| id | name     | price | stock | category_id |
| -- | -------- | ----- | ----- | ----------- |
| 1  | Notebook | 3500  | 5     | 1           |
| 2  | Mouse    | 80    | 30    | 1           |
| 3  | Teclado  | 150   | 20    | 1           |

### Exemplo

```sql
SELECT name, price FROM products;
```

### Resultado

| name     | price |
| -------- | ----- |
| Notebook | 3500  |
| Mouse    | 80    |
| Teclado  | 150   |

## WHERE

### Conceito

A cláusula WHERE é utilizada para filtrar registros com base em uma condição.

### Exemplo

```sql
SELECT * FROM products
WHERE price > 100;
```

### Resultado

| id | name     | price | stock | category_id |
| -- | -------- | ----- | ----- | ----------- |
| 1  | Notebook | 3500  | 5     | 1           |
| 3  | Teclado  | 150   | 20    | 1           |

## AND / OR

### Conceito

Os operadores AND e OR permitem combinar múltiplas condições.

AND retorna quando todas são verdadeiras.
OR retorna quando ao menos uma é verdadeira.

### Exemplo

```sql
SELECT * FROM products
WHERE price > 100 AND stock > 10;
```

### Resultado

| id | name    | price | stock | category_id |
| -- | ------- | ----- | ----- | ----------- |
| 3  | Teclado | 150   | 20    | 1           |

### Exemplo

```sql
SELECT * FROM products
WHERE price > 100 OR stock > 25;
```

### Resultado

| id | name     | price | stock | category_id |
| -- | -------- | ----- | ----- | ----------- |
| 1  | Notebook | 3500  | 5     | 1           |
| 2  | Mouse    | 80    | 30    | 1           |
| 3  | Teclado  | 150   | 20    | 1           |

## LIKE

### Conceito

O operador LIKE permite buscar padrões em campos de texto.

% representa vários caracteres
_ representa um caractere

### Exemplo

```sql
SELECT * FROM customers
WHERE name LIKE 'P%';
```

### Resultado

| id | name  | city    |
| -- | ----- | ------- |
| 2  | Pedro | Goiânia |

## BETWEEN

### Conceito

O operador BETWEEN filtra valores dentro de um intervalo.

### Exemplo

```sql
SELECT * FROM products
WHERE price BETWEEN 100 AND 2000;
```

### Resultado

| id | name    | price | stock | category_id |
| -- | ------- | ----- | ----- | ----------- |
| 3  | Teclado | 150   | 20    | 1           |

## IN

### Conceito

O operador IN permite filtrar múltiplos valores.

### Exemplo

```sql
SELECT * FROM customers
WHERE city IN ('Mineiros', 'Goiânia');
```

### Resultado

| id | name   | city     |
| -- | ------ | -------- |
| 1  | Ana    | Mineiros |
| 2  | Pedro  | Goiânia  |
| 3  | Carlos | Mineiros |

## ORDER BY

### Conceito

ORDER BY ordena os resultados de forma crescente ou decrescente.

### Exemplo

```sql
SELECT * FROM products
ORDER BY price DESC;
```

### Resultado

| id | name     | price | stock | category_id |
| -- | -------- | ----- | ----- | ----------- |
| 1  | Notebook | 3500  | 5     | 1           |
| 3  | Teclado  | 150   | 20    | 1           |
| 2  | Mouse    | 80    | 30    | 1           |

## LIMIT

### Conceito

LIMIT restringe a quantidade de registros retornados.

### Exemplo

```sql
SELECT * FROM products
LIMIT 2;
```

### Resultado

| id | name     | price | stock | category_id |
| -- | -------- | ----- | ----- | ----------- |
| 1  | Notebook | 3500  | 5     | 1           |
| 2  | Mouse    | 80    | 30    | 1           |

## COUNT

### Conceito

COUNT conta a quantidade de registros.

### Exemplo

```sql
SELECT COUNT(*) FROM products;
```

### Resultado

```
3
```

## AVG

### Conceito

AVG calcula a média dos valores.

### Exemplo

```sql
SELECT AVG(price) FROM products;
```

### Resultado

```
1243.33
```

## SUM

### Conceito

SUM realiza a soma dos valores.

### Exemplo

```sql
SELECT SUM(stock) FROM products;
```

### Resultado

```
55
```

## GROUP BY

### Conceito

GROUP BY agrupa registros para uso com funções agregadas.

### Exemplo

```sql
SELECT category_id, COUNT(*)
FROM products
GROUP BY category_id;
```

### Resultado

| category_id | count |
| ----------- | ----- |
| 1           | 3     |

## Alias (AS)

### Conceito

AS permite renomear colunas ou tabelas.

### Exemplo

```sql
SELECT name AS product_name, price AS product_price
FROM products;
```

### Resultado

| product_name | product_price |
| ------------ | ------------- |
| Notebook     | 3500          |
| Mouse        | 80            |
| Teclado      | 150           |

## Subquery

### Conceito

Subquery é uma consulta dentro de outra consulta.

### Exemplo

```sql
SELECT *
FROM products
WHERE price > (
SELECT AVG(price) FROM products
);
```

### Resultado

| id | name     | price | stock | category_id |
| -- | -------- | ----- | ----- | ----------- |
| 1  | Notebook | 3500  | 5     | 1           |

## INNER JOIN

### Conceito

INNER JOIN retorna apenas registros com correspondência entre tabelas.

### Exemplo

```sql
SELECT products.name, categories.name
FROM products
INNER JOIN categories
ON categories.id = products.category_id;
```

### Resultado

| name     | name        |
| -------- | ----------- |
| Notebook | Eletrônicos |
| Mouse    | Eletrônicos |
| Teclado  | Eletrônicos |

## LEFT JOIN

### Conceito

LEFT JOIN retorna todos da tabela esquerda.

### Exemplo

```sql
SELECT products.name, categories.name
FROM products
LEFT JOIN categories
ON categories.id = products.category_id;
```

## INSERT

### Conceito

INSERT adiciona novos registros.

### Exemplo

```sql
INSERT INTO customers (name, city)
VALUES ('João', 'Mineiros');
```

### Resultado

Novo registro inserido na tabela customers.

## UPDATE

### Conceito

UPDATE altera registros existentes.

### Exemplo

```sql
UPDATE products
SET price = 100
WHERE id = 2;
```

### Resultado

Produto atualizado.

## DELETE

### Conceito

DELETE remove registros.

### Exemplo

```sql
DELETE FROM products
WHERE id = 3;
```

### Resultado

Registro removido.

## Boas práticas

* Evitar SELECT *
* Sempre usar WHERE em UPDATE
* Sempre usar WHERE em DELETE
* Utilizar aliases para melhorar leitura
* Preferir JOIN ao invés de subquery quando possível
* Padronizar nomes de colunas
* Utilizar ORDER BY quando necessário

## Conclusão

Este repositório apresenta os principais comandos SQL utilizados em bancos de dados relacionais, incluindo consultas, filtros, ordenação, agregações, subqueries e manipulação de dados. Os exemplos demonstram o uso prático de cada comando, servindo como material de estudo e consulta rápida para desenvolvimento de aplicações que utilizam SQL.
