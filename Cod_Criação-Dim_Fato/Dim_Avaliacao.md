# Dim_Avaliacao.md

```markdown
# Dim_Avaliação

```sql
Create Table Dim_Avaliação
(
sk_comentarios INT IDENTITY(1,1) PRIMARY KEY,
id_avaliação	VARCHAR(64),
order_id		VARCHAR(64),
nota_avaliação	INT,
titulo_comentario Varchar(64),
mensagem_comentario VARCHAR(2000),
data_comentario DATE,
data_resposta_loja DATE
)
INSERT INTO Dim_Avaliação (
id_avaliação	,
order_id		,
nota_avaliação,
titulo_comentario,
mensagem_comentario,
data_comentario,
data_resposta_loja
)
Select
	review_id,
	order_id,
	review_score,
	review_comment_title,
	review_comment_message,
	review_creation_date,
	review_answer_timestamp

FROM olist_order_reviews_dataset


select * from Dim_Avaliação

SELECT COLUMN_NAME
FROM INFORMATION_SCHEMA.COLUMNS
WHERE TABLE_NAME = 'olist_order_reviews_dataset';


select *
FROM olist_order_reviews_dataset -- reviews dos produtos, contem order_id, estrelas e etc.
