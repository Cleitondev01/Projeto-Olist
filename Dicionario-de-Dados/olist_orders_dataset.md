# olist_orders_dataset — Dicionário de Dados

## Campos principais
- **order_id**: identificador do pedido.
- **customer_id**: cliente que realizou o pedido (liga com customers).
- **order_status**: status do pedido (created, approved, shipped, delivered, invoiced, canceled, unavailable, processing).
- **order_purchase_timestamp**: data/hora em que o cliente finalizou a compra no site.
- **order_approved_at**: data/hora da aprovação do pagamento.
- **order_delivered_carrier_date**: data/hora em que o pedido foi postado/entregue à transportadora.
- **order_delivered_customer_date**: data/hora da entrega ao cliente.
- **order_estimated_delivery_date**: data estimada/prometida para entrega (SLA).
