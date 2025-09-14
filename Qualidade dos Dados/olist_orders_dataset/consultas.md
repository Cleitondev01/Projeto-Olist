# Consultas usadas

```sql
-- Status distintos (pra ver variação de escrita)
SELECT DISTINCT order_status
FROM olist_orders_dataset
ORDER BY order_status;

-- Contagem por status
SELECT 
  order_status, 
  COUNT(*) AS qtd
FROM olist_orders_dataset
GROUP BY order_status
ORDER BY qtd DESC;

-- Pedido DELIVERED sem data de entrega ao cliente
SELECT COUNT(*) AS delivered_sem_customer_date
FROM olist_orders_dataset
WHERE order_status = 'delivered'
  AND order_delivered_customer_date IS NULL;

-- (opcional) listar os casos
SELECT *
FROM olist_orders_dataset
WHERE order_status = 'delivered'
  AND order_delivered_customer_date IS NULL;

-- Pedido SHIPPED sem data de postagem para transportadora
SELECT COUNT(*) AS shipped_sem_carrier_date
FROM olist_orders_dataset
WHERE order_status = 'shipped'
  AND order_delivered_carrier_date IS NULL;

-- Concluídos (delivered/shipped/invoiced) sem aprovação de pagamento
SELECT COUNT(*) AS concluido_sem_aprovacao
FROM olist_orders_dataset
WHERE order_status IN ('delivered','shipped','invoiced')
  AND order_approved_at IS NULL;

-- Datas fora de ordem (aprovação antes da compra)
SELECT COUNT(*) AS aprovacao_antes_da_compra
FROM olist_orders_dataset
WHERE order_approved_at IS NOT NULL
  AND order_purchase_timestamp IS NOT NULL
  AND order_approved_at < order_purchase_timestamp;
