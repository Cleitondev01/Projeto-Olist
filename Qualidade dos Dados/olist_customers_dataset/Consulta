## Consultas usadas

```sql
-- Conta a quantidade de estados únicos
SELECT COUNT(DISTINCT customer_state) AS qtd_estados
FROM olist_customers_dataset;

-- Lista todos os estados distintos
SELECT DISTINCT customer_state
FROM olist_customers_dataset
ORDER BY customer_state;

-- Linhas por estado (desc)
SELECT customer_state, COUNT(*) AS qtde_linhas
FROM olist_customers_dataset
GROUP BY customer_state
ORDER BY qtde_linhas DESC;
