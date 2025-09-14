# Observações – olist_order_items_dataset

- Não foram identificados preços ou fretes inválidos (≤ 0).  
- Não foram encontradas duplicidades na chave `(order_id, order_item_id)`.  
- Há casos de fretes mais caros que o preço do produto, inclusive alguns com valor superior ao dobro do item.  

**Tratativa:**  
Mantidos no dataset e documentados como possíveis *outliers*, já que podem ocorrer em compras de baixo valor enviadas para regiões distantes.

