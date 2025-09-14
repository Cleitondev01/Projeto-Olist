# Consultas usadas

```sql
-- Ver se existe order_id nulo
SELECT COUNT(*) AS qtd_order_id_nulo
FROM olist_order_reviews_dataset
WHERE order_id IS NULL OR order_id = '';

-- Ver se existe review_creation_date nulo
SELECT COUNT(*) AS qtd_creation_date_nulo
FROM olist_order_reviews_dataset
WHERE review_creation_date IS NULL;

-- Ver se existe review_answer_timestamp nulo
SELECT COUNT(*) AS qtd_answer_date_nulo
FROM olist_order_reviews_dataset
WHERE review_answer_timestamp IS NULL;
