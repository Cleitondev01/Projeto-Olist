# Consultas usadas

```sql
-- Estados distintos
SELECT DISTINCT seller_state
FROM olist_sellers_dataset
ORDER BY seller_state;

-- Contagem por estado
SELECT 
  seller_state, 
  COUNT(*) AS qtd
FROM olist_sellers_dataset
GROUP BY seller_state
ORDER BY qtd DESC;

-- Cidades distintas (checagem de escrita)
SELECT DISTINCT seller_city
FROM olist_sellers_dataset
ORDER BY seller_city;

-- Nulos nas principais colunas
SELECT
  SUM(CASE WHEN seller_id IS NULL OR seller_id = '' THEN 1 ELSE 0 END)       AS nulos_seller_id,
  SUM(CASE WHEN seller_zip_code_prefix IS NULL THEN 1 ELSE 0 END)            AS nulos_zip_prefix,
  SUM(CASE WHEN seller_city IS NULL OR seller_city = '' THEN 1 ELSE 0 END)   AS nulos_city,
  SUM(CASE WHEN seller_state IS NULL OR seller_state = '' THEN 1 ELSE 0 END) AS nulos_state
FROM olist_sellers_dataset;

-- Duplicidade de seller (não deveria existir)
SELECT 
  seller_id, 
  COUNT(*) AS linhas
FROM olist_sellers_dataset
GROUP BY seller_id
HAVING COUNT(*) > 1;

-- Sellers que não aparecem em order_items
SELECT COUNT(*) AS sellers_sem_venda
FROM olist_sellers_dataset s
LEFT JOIN olist_order_items_dataset i
  ON i.seller_id = s.seller_id
WHERE i.seller_id IS NULL;

-- CEP prefixo fora de 1..5 dígitos (higiene de dados)
SELECT 
  seller_id, 
  seller_zip_code_prefix
FROM olist_sellers_dataset
WHERE LEN(LTRIM(RTRIM(CAST(seller_zip_code_prefix AS VARCHAR(10))))) NOT BETWEEN 1 AND 5;
