---

# Fat_Vendas.md

```markdown
# Fat_Vendas

```sql
--======================================fat_Vendas====================================================================
select *
FROM olist_order_items_dataset -- preço dos produtos, valor do frete cobrado, ordem dos pedidos, identificador do produto, vendedor que vendeu o item, Data limite que o vendedor tinha para enviar o item

select *
FROM olist_orders_dataset --order_id, customer_id, status (enviado etc), data/hora da compra(quando cliente finalizou no site), data/hora que o pagamento foi aprovado, data/hora que o pedido foi postado, data/hora, da entrega ao cliente, data prevista para entrega (prometida ao cliente)

select DISTINCT order_status
FROM olist_orders_dataset

EXEC sp_rename 'Fat_Vendas.id_clientes', 'id_cliente', 'COLUMN';

CREATE TABLE Fat_Vendas (
    order_id VARCHAR(64) NOT NULL,
    id_clientes INT,
    id_vendedor INT,
    valor_produto INT,
    valor_frete INT,
    status VARCHAR(20),
    data_postado DATE,
    data_entregue DATE,
    data_previsão_entrega DATE,
    data_limite_entrega DATE,
    tempo_entrega INT,
	status_entrega VARCHAR(64),
	prazo_entrega INT,
    estado_cliente VARCHAR(2)
);

INSERT INTO Fat_Vendas (
    order_id,
    id_clientes,
    id_vendedor,
    valor_produto,
    valor_frete,
    status,
    data_postado,
    data_entregue,
    data_previsão_entrega,   
    data_limite_entrega,
    tempo_entrega,
	status_entrega,
	prazo_entrega,
	estado_cliente
)
SELECT
    o.order_id,                                   -- order_id
    dc.sk_cliente,                                -- id_clientes (da Dim_clientes)
    dv.sk_vendedor,                               -- id_vendedor (da Dim_vendedores)
    o.price,                                      -- valor_produto
    o.freight_value,                              -- valor_frete
    CASE u.order_status                           -- status traduzido
        WHEN 'approved'    THEN 'Aprovado'
        WHEN 'delivered'   THEN 'Entregue'
        WHEN 'created'     THEN 'Criado'
        WHEN 'invoiced'    THEN 'Faturado'
        WHEN 'processing'  THEN 'Processando'
        WHEN 'unavailable' THEN 'Indisponível'
        WHEN 'canceled'    THEN 'Cancelado'
        WHEN 'shipped'     THEN 'Enviado'
        ELSE u.order_status
    END AS status,
    u.order_delivered_carrier_date,               -- data_postado
    u.order_delivered_customer_date,              -- data_entregue
    u.order_estimated_delivery_date,              -- data_previsão_entrega
    o.shipping_limit_date,                        -- data_limite_entrega
    DATEDIFF(DAY,
             u.order_delivered_carrier_date,
             u.order_delivered_customer_date)      AS tempo_entrega,

	CASE
		WHEN u.order_delivered_customer_date IS NULL THEN 'Não entregue'
		WHEN u.order_delivered_customer_date <= u.order_estimated_delivery_date THEN 'No Prazo'
		ELSE 'Atrasado'
		END,

	DATEDIFF(DAY,
             u.order_delivered_carrier_date,
             u.order_estimated_delivery_date)	   AS prazo_entrega,

    dc.estado                                     -- estado_cliente
FROM olist_order_items_dataset AS o
JOIN olist_orders_dataset AS u
  ON o.order_id = u.order_id
JOIN olist_customers_dataset AS cu
  ON u.customer_id = cu.customer_id
JOIN Dim_clientes AS dc
  ON dc.cliente_id_unico = cu.customer_unique_id
JOIN Dim_vendedor AS dv
  ON dv.seller_id = o.seller_id;

  ALTER TABLE Fat_Vendas
ADD data_pedido_criado   DATE NULL,
    data_pedido_aprovado DATE NULL,
    tempo_aprovacao_horas INT NULL;  -- opcional

UPDATE fv
SET
  data_pedido_criado    = CAST(u.order_purchase_timestamp AS DATE),
  data_pedido_aprovado  = CAST(u.order_approved_at       AS DATE),
  tempo_aprovacao_horas = DATEDIFF(HOUR, u.order_purchase_timestamp, u.order_approved_at)
FROM Fat_Vendas fv
JOIN olist_orders_dataset u ON fv.order_id = u.order_id;

  select *
  from Fat_Vendas

  DROP TABLE Fat_Vendas
