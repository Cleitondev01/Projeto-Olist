# Consultas usadas – olist_products_dataset

```sql
-- Categorias distintas (ver se tem escrita diferente)
SELECT DISTINCT product_category_name
FROM olist_products_dataset
ORDER BY product_category_name;

-- Quantidade por categoria
SELECT 
  product_category_name, 
  COUNT(*) AS qtd
FROM olist_products_dataset
GROUP BY product_category_name
ORDER BY qtd DESC;

-- Nulos nas principais colunas
SELECT
  SUM(CASE WHEN product_id IS NULL OR product_id = '' THEN 1 ELSE 0 END)                           AS nulos_product_id,
  SUM(CASE WHEN product_category_name IS NULL OR product_category_name = '' THEN 1 ELSE 0 END)     AS nulos_categoria,
  SUM(CASE WHEN product_name_lenght IS NULL THEN 1 ELSE 0 END)                                     AS nulos_tam_nome,
  SUM(CASE WHEN product_description_lenght IS NULL THEN 1 ELSE 0 END)                              AS nulos_tam_desc,
  SUM(CASE WHEN product_photos_qty IS NULL THEN 1 ELSE 0 END)                                      AS nulos_qtd_fotos,
  SUM(CASE WHEN product_weight_g IS NULL THEN 1 ELSE 0 END)                                        AS nulos_peso_g,
  SUM(CASE WHEN product_length_cm IS NULL THEN 1 ELSE 0 END)                                       AS nulos_comp_cm,
  SUM(CASE WHEN product_height_cm IS NULL THEN 1 ELSE 0 END)                                       AS nulos_alt_cm,
  SUM(CASE WHEN product_width_cm IS NULL OR product_width_cm = '' THEN 1 ELSE 0 END)               AS nulos_larg_cm
FROM olist_products_dataset;

-- Listar registros sem nome/descrição (para inspecionar os 610)
SELECT 
    product_id,
    product_category_name,
    product_name_lenght,
    product_description_lenght,
    product_photos_qty,
    product_weight_g,
    product_length_cm,
    product_height_cm,
    product_width_cm
FROM olist_products_dataset
WHERE product_name_lenght IS NULL
   OR product_description_lenght IS NULL;

-- Medidas/fotos/peso inválidos
SELECT *
FROM olist_products_dataset
WHERE COALESCE(product_weight_g,0) <= 0
   OR COALESCE(product_length_cm,0) <= 0
   OR COALESCE(product_height_cm,0) <= 0
   OR COALESCE(product_width_cm,0)  <= 0
   OR COALESCE(product_photos_qty,0) <  0;

-- Medidas muito exageradas (outliers)
SELECT *
FROM olist_products_dataset
WHERE product_weight_g > 30000
   OR product_length_cm > 200
   OR product_height_cm > 200
   OR product_width_cm  > 200;

-- Contagem de outliers “exagerados”
SELECT COUNT(*) AS qtd_outliers_medidas
FROM olist_products_dataset
WHERE product_weight_g > 30000
   OR product_length_cm > 200
   OR product_height_cm > 200
   OR product_width_cm  > 200;
