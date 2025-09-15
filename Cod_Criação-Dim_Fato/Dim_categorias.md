
---

# Dim_Categorias.md

```markdown
# Dim_Categorias

```sql
-- TBL DIM_CATG
CREATE TABLE dbo.Dim_Categorias (
    sk_categoria INT IDENTITY(1,1) PRIMARY KEY,
    categoria NVARCHAR(120) NOT NULL UNIQUE
);

INSERT INTO dbo.Dim_Categorias (categoria)
SELECT DISTINCT
    COALESCE(NULLIF(LTRIM(RTRIM(LOWER(product_category_name))), ''), 'desconhecida')
FROM dbo.olist_products_dataset
ORDER BY
    COALESCE(NULLIF(LTRIM(RTRIM(LOWER(product_category_name))), ''), 'desconhecida');

SELECT *
FROM Dim_Categorias

Drop table Dim_Categorias
