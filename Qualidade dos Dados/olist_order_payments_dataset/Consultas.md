## Consultas usadas

```sql
-- Ver se tem algum tipo de pagamento duplicado (variação de escrita)
SELECT DISTINCT payment_type
FROM olist_order_payments_dataset
ORDER BY payment_type;

-- Contar quantos registros tem por tipo de pagamento
SELECT
  payment_type,
  COUNT(*) AS qtd
FROM olist_order_payments_dataset
GROUP BY payment_type
ORDER BY qtd DESC;

-- Checar se tem valores nulos ou vazios nas colunas principais
SELECT
  SUM(CASE WHEN order_id IS NULL OR order_id = '' THEN 1 ELSE 0 END) AS nulos_order_id,
  SUM(CASE WHEN payment_type IS NULL OR payment_type = '' THEN 1 ELSE 0 END) AS nulos_payment_type,
  SUM(CASE WHEN payment_installments IS NULL THEN 1 ELSE 0 END) AS nulos_installments,
  SUM(CASE WHEN payment_value IS NULL THEN 1 ELSE 0 END) AS nulos_value
FROM olist_order_payments_dataset;

-- Conferir se tem número de parcelas esquisito (0 ou muito alto)
SELECT DISTINCT payment_installments
FROM olist_order_payments_dataset
ORDER BY payment_installments;

-- Filtrar registros com parcelas = 0
SELECT
  order_id,
  payment_sequential,
  payment_type,
  payment_installments,
  payment_value
FROM olist_order_payments_dataset
WHERE payment_installments = 0;

-- Procurar valores de pagamento inválidos (<= 0)
SELECT *
FROM olist_order_payments_dataset
WHERE payment_value <= 0;

-- Contar quantos payment_value = 0 por tipo de pagamento
SELECT
  payment_type,
  COUNT(*) AS qtd_valores_zero
FROM olist_order_payments_dataset
WHERE payment_value = 0
GROUP BY payment_type
ORDER BY qtd_valores_zero DESC;

-- Quantos pagamentos tiveram payment_sequential > 5
SELECT COUNT(*) AS qtd_pagamentos_seq_maior_5
FROM olist_order_payments_dataset
WHERE payment_sequential > 5;

-- Detalhar os casos com payment_sequential > 5
SELECT
  order_id,
  payment_sequential,
  payment_type,
  payment_installments,
  payment_value
FROM olist_order_payments_dataset
WHERE payment_sequential > 5;
