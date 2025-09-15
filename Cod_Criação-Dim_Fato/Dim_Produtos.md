
---

# Dim_Produtos.md

```markdown
# Dim_Produtos

```sql
--TBL DIM_PRODUTOS
CREATE TABLE dbo.Dim_Produtos (
    sk_produto INT IDENTITY(1,1) PRIMARY KEY,   -- ID sequencial (1,2,3...)
    product_id VARCHAR(64) NOT NULL UNIQUE,     -- ID original do produto
    sk_categoria INT NOT NULL,                  -- chave estrangeira da Dim_Categorias
	qtde_fotos INT NOT NULL,

    peso_g INT NULL,                            -- peso em gramas
    comprimento_cm INT NULL,                    -- comprimento em cm
    altura_cm INT NULL,                         -- altura em cm
    largura_cm INT NULL,                        -- largura em cm

    volume_cm3 AS (                             -- cálculo automático
        ISNULL(comprimento_cm,0) *
        ISNULL(altura_cm,0) *
        ISNULL(largura_cm,0)
    ) PERSISTED,

    categoria_peso AS (                         -- classificação leve/médio/pesado
        CASE 
            WHEN peso_g IS NULL THEN 'desconhecido'
            WHEN peso_g < 500 THEN 'leve'
            WHEN peso_g <= 2000 THEN 'medio'
            ELSE 'pesado'
        END
    ) PERSISTED
);
INSERT INTO dbo.Dim_Produtos (
    product_id, sk_categoria, qtde_fotos,
    peso_g, comprimento_cm, altura_cm, largura_cm
)
SELECT
    p.product_id,
    c.sk_categoria,                -- faz o JOIN pra pegar a categoria certa
	COALESCE(p.product_photos_qty, 0) AS qtde_fotos,
    p.product_weight_g,
    p.product_length_cm,
    p.product_height_cm,
    p.product_width_cm
FROM dbo.olist_products_dataset p
JOIN dbo.Dim_Categorias c
  ON c.categoria = COALESCE(
        NULLIF(LTRIM(RTRIM(LOWER(p.product_category_name))), ''), 
        'desconhecida'
     );


	 SELECT * FROM 
	 Dim_Produtos

	 DROP TABLE Dim_Produtos
