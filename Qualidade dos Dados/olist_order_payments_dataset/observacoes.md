# Observações – olist_order_payments_dataset

- **Parcelas zeradas:** 2 registros com `payment_installments = 0`.  
  **Interpretação:** possivelmente pagamentos à vista.  
  **Ação:** mantidos no dataset e documentados para validação futura.

- **Valores de pagamento zerados:** 9 registros com `payment_value = 0`  
  (6 = `voucher`, 3 = `not_defined`).  
  **Interpretação:** podem estar ligados a promoções/cupons ou registros incompletos.  
  **Ação:** documentados como anomalias.

- **Sequência de pagamentos anormal:** 458 registros com `payment_sequential > 5`, todos em `voucher`.  
  **Interpretação:** pode indicar múltiplas tentativas de uso do mesmo voucher.  
  **Ação:** documentados como comportamento atípico, mas não removidos.

**Conclusão:** as anomalias não comprometem a consistência geral da tabela; foram mantidas e registradas como *outliers* de negócio (promoções, descontos ou pagamentos à vista).
