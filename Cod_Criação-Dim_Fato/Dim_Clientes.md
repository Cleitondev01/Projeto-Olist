# Modelagem – Código SQL (Dims e Fato)

---

## Dim_clientes

```sql
CREATE TABLE dbo.Dim_clientes (
    sk_cliente INT IDENTITY(1,1) PRIMARY KEY, -- surrogate key sequencial
    cliente_id_unico VARCHAR(64) NOT NULL,
    compras_total INT,
    venda_remunerada_total INT,
    classe_cliente VARCHAR(32),
    cidade VARCHAR(64),
    estado VARCHAR(2),
    dt_primeira_compra DATE
);

-- Popula a tabela com a lógica da view original
INSERT INTO dbo.Dim_clientes (
    cliente_id_unico,
    compras_total,
    venda_remunerada_total,
    classe_cliente,
    cidade,
    estado,
    dt_primeira_compra
)
SELECT
    c.customer_unique_id AS cliente_id_unico,
    COUNT(o.order_id) AS compras_total,
    SUM(CASE WHEN LOWER(o.order_status) NOT IN ('canceled','unavailable') THEN 1 ELSE 0 END) AS venda_remunerada_total,
    CASE
        WHEN SUM(CASE WHEN LOWER(o.order_status) NOT IN ('canceled','unavailable') THEN 1 ELSE 0 END) >= 6 THEN 'Fiel'
        WHEN SUM(CASE WHEN LOWER(o.order_status) NOT IN ('canceled','unavailable') THEN 1 ELSE 0 END) BETWEEN 2 AND 5 THEN 'Recorrente'
        WHEN SUM(CASE WHEN LOWER(o.order_status) NOT IN ('canceled','unavailable') THEN 1 ELSE 0 END) = 1 THEN 'Novo'
        ELSE 'Sem compra remunerada'
    END AS classe_cliente,
    MIN(c.customer_city)  AS cidade,
    MIN(c.customer_state) AS estado,
    MIN(o.order_purchase_timestamp) AS dt_primeira_compra
FROM dbo.olist_orders_dataset AS o
JOIN dbo.olist_customers_dataset AS c
    ON o.customer_id = c.customer_id
GROUP BY c.customer_unique_id;

SELECT
    sk_cliente,
    cliente_id_unico,
    dt_primeira_compra
FROM dbo.Dim_clientes
WHERE dt_primeira_compra = (
    SELECT MIN(dt_primeira_compra) FROM dbo.Dim_clientes
);

select *
from Dim_clientes
/* CONSIDERAMOS POR QTDE DE COMPRAS:
1 = Novo
2–5 = Recorrente
6+ = Fiel
0 = Sem compra remunerada
*/
-- 1) 1 linha por cliente?
SELECT COUNT(*) AS linhas, COUNT(DISTINCT cliente_id_unico) AS clientes FROM dbo.Dim_clientes;

-- 2) venda_remunerada_total nunca > compras_total?
SELECT * FROM dbo.Dim_clientes WHERE venda_remunerada_total > compras_total;

-- 3) cliente_num único?
SELECT cliente_num, COUNT(*) qtd FROM dbo.Dim_clientes GROUP BY cliente_num HAVING COUNT(*) > 1;

-- 4) classificação coerente?
SELECT classe_cliente, MIN(venda_remunerada_total) min_v, MAX(venda_remunerada_total) max_v
FROM dbo.Dim_clientes GROUP BY classe_cliente;

DROP VIEW Dim_clientes
