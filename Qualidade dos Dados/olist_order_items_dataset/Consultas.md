# Consultas usadas

```sql
-- Itens com preço/frete inválidos
SELECT *
FROM olist_order_items_dataset
WHERE price <= 0 OR freight_value < 0;

-- Verifica duplicidade de item dentro do mesmo pedido
SELECT 
  order_id, 
  order_item_id, 
  COUNT(*) AS linhas
FROM olist_order_items_dataset
GROUP BY order_id, order_item_id
HAVING COUNT(*) > 1;

-- Frete maior que o preço do produto
SELECT *
FROM olist_order_items_dataset
WHERE freight_value > price;

-- Frete maior que o dobro do preço do produto
SELECT *
FROM olist_order_items_dataset
WHERE freight_value > price * 2;
