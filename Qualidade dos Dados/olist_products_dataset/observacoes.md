# Observações – olist_products_dataset

- **Produtos sem nome/descrição:** 610 registros.  
- **Produto sem cadastro completo:** 1 registro com apenas `product_id`.  
- **Produto sem medidas/peso:** 1 registro sem `weight_g`, `length_cm`, `height_cm`, `width_cm`.  
- **Produtos com peso zerado:** existem casos com `product_weight_g = 0`, considerados plausíveis.  
- **Produto com peso exagerado:** 1 registro com `product_weight_g = 40425` (≈ 40,4 kg), provavelmente erro de digitação.

**Tratativa:**  
Todos os registros foram mantidos no dataset e documentados como **cadastro incompleto ou outliers** para referência.
