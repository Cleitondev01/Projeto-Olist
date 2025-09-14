# Observações – olist_orders_dataset

- **Delivered sem data de entrega ao cliente:** 8 registros.  
  *Pode estar desatualizado (pedido ainda em trânsito / atualização pendente).*  
  **Próximo passo sugerido:** JOIN com reviews para ver reclamações de não entrega.

- **Concluídos (delivered/shipped/invoiced) sem `order_approved_at`:** 14 registros.  
  *Estranho, pois o pedido normalmente só prossegue após aprovação.*  
  **Possíveis causas:** falha no registro do timestamp ou exceções de negócio (ex.: promocional).  
  **Próximo passo sugerido:** JOIN com `olist_order_payments_dataset` para checar tipo e valor de pagamento.

- **Shipped sem `order_delivered_carrier_date`:** 0 registros.  
- **Datas fora de ordem (aprovação antes da compra):** 0 registros.

**Conclusão:** demais checagens consistentes; anomalias documentadas para validação.
