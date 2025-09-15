
---

# Dim_Vendedor.md

```markdown
# Dim_Vendedor

```sql
select * 
from olist_sellers_dataset --ident do vendedor, prefixo do cep, cidade do vendedor e estado (RJ, SP)

Create Table Dim_Vendedor (
	sk_vendedor INT IDENTITY(1,1) PRIMARY KEY,
	seller_id VARCHAR(64) NOT NULL UNIQUE,
	cidade VARCHAR(64),
	estado VARCHAR(2),
	contagem_vendas INT NOT NULL,
	faturamento INT NOT NULL
	)

INSERT INTO Dim_Vendedor (
seller_id,cidade,estado
,contagem_vendas
,faturamento
)
SELECT
	s.seller_id,
	s.seller_city,
	s.seller_state,
	COUNT(DISTINCT c.order_id),
	SUM(c.price) AS faturamento


FROM olist_sellers_dataset s
JOIN olist_order_items_dataset c
	ON s.seller_id = c.seller_id
	GROUP BY
    s.seller_id, s.seller_city, s.seller_state;

	DROP TABLE Dim_Vendedor


	SELECT * FROM 
	Dim_Vendedor

	SELECT COUNT(seller_id)
	FROM olist_sellers_dataset

	select *
FROM olist_order_items_dataset

select COUNT(order_id)
FROM olist_order_items_dataset
WHERE seller_id='0015a82c2db000af6aaaf3ae2ecb0532'

select SUM(price)
FROM olist_order_items_dataset
WHERE seller_id='0015a82c2db000af6aaaf3ae2ecb0532'
