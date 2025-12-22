# MVP — Pipeline de Dados em Nuvem com Databricks (Olist)

Repositório (para avaliação): https://github.com/brenodf86/mvp-eng-de-dados-puc-olist-databricks

## Visão Geral
Este MVP constrói um pipeline de dados na nuvem utilizando **Databricks Community/Free Edition** e o dataset público **Olist (Kaggle)**. O objetivo é organizar os dados em camadas (**Raw → Bronze → Silver → Gold**) e disponibilizar um modelo analítico em **Esquema Estrela** para responder perguntas de negócio sobre logística, atrasos e satisfação do cliente (reviews).

---

## 1) Objetivo e Problema
No e-commerce, atrasos de entrega e a experiência do cliente afetam diretamente a satisfação (reviews), custos logísticos e performance por categoria/UF. Este MVP organiza os dados do Olist em um modelo estrela para analisar **atraso de entrega** e seus impactos.

### Perguntas de Negócio
1. Qual é a taxa de pedidos entregues com atraso por mês e por UF?
2. Quais categorias de produto têm maior atraso médio (em dias) e maior taxa de atraso?
3. Existe relação entre atraso e nota de review (review_score)?

---

## 2) Dataset / Fonte e Licença
- Dataset: **Brazilian E-Commerce Public Dataset by Olist (Kaggle)**
- Coleta: download manual no Kaggle + upload no Databricks via Data Ingestion
- Licença: [CC BY-NC-SA 4.0](https://creativecommons.org/licenses/by-nc-sa/4.0/)

**Evidências:**
- Kaggle: [evidencias/01_kaggle_dataset.png](evidencias/01_kaggle_dataset.png)
- Upload/Ingestion: [evidencias/02_ingestion_upload.png](evidencias/02_ingestion_upload.png)

---

## 3) Arquitetura e Pipeline (Camadas)
Arquitetura: **Raw (tabelas raw_*) → Bronze (Delta) → Silver (Delta) → Gold (Esquema Estrela)**

- Raw: tabelas criadas via upload no Databricks (Data Ingestion)
- Bronze: padronização inicial e persistência em Delta
- Silver: tratamento de tipos, enriquecimentos e regras (datas, métricas de atraso)
- Gold: dimensões + fato para análises (DW / Star Schema)

**Evidências:**
- Raw: [evidencias/03_raw_tables.png](evidencias/03_raw_tables.png)
- Bronze: [evidencias/04_bronze_tables.png](evidencias/04_bronze_tables.png)
- Silver: [evidencias/05_silver_tables.png](evidencias/05_silver_tables.png)
- Gold: [evidencias/06_gold_tables.png](evidencias/06_gold_tables.png)

---

## 4) Modelagem (Esquema Estrela)
- Fato principal: `gold_fato_pedido_item`
- Dimensões: `gold_dim_produto`, `gold_dim_cliente`, `gold_dim_vendedor`, `gold_dim_data`, `gold_dim_pedido`

**Diagrama (Star Schema):**
- Imagem: [docs1/modelagem_star_schema.drawio.png](docs1/modelagem_star_schema.drawio.png)

(Se você preferir embedar a imagem no README:)
![Star Schema](docs1/modelagem_star_schema.drawio.png)

---

## 5) Catálogo de Dados e Qualidade
- Catálogo/profiling: **(adicione aqui o arquivo quando commitar)**  
  Ex.: [docs1/catalogo_dados.md](docs1/catalogo_dados.md)

**Evidência:**
- Profiling: [evidencias/07_quality_profile.png](evidencias/07_quality_profile.png)

Principais verificações:
- Nulos por atributo
- Cardinalidade/distinct por coluna
- Faixas numéricas (min/max/média)
- Checagens de integridade (quando aplicável) nas chaves dim ↔ fato

---

## 6) ETL / Carga (Notebooks)
Os notebooks foram construídos e executados no Databricks, e exportados para este repositório:

- [notebooks/01_bronze_ingestao.ipynb](notebooks/01_bronze_ingestao.ipynb)
- [notebooks/02_silver_transformacoes.ipynb](notebooks/02_silver_transformacoes.ipynb)
- [notebooks/03_gold_modelagem_estrela.ipynb](notebooks/03_gold_modelagem_estrela.ipynb)
- [notebooks/04_data_quality_catalogo.ipynb](notebooks/04_data_quality_catalogo.ipynb)
- [notebooks/05_analises_respostas.ipynb](notebooks/05_analises_respostas.ipynb)

---

## 7) Resultados (Respostas)

### P1 — Taxa de atraso por mês e UF
**Achado (resumo):**  
Observou-se variação da taxa de atraso por UF e mês. Em recortes com baixo volume de itens, a taxa pode aparentar picos (ex.: 100% com poucos registros), portanto a análise deve considerar também o volume (itens) para interpretar corretamente.

**SQL (executado):**
```sql
%sql
SELECT
  d.ano, d.mes,
  c.customer_state AS uf,
  COUNT(*) AS itens,
  SUM(CASE WHEN f.atraso_dias > 0 THEN 1 ELSE 0 END) AS itens_atrasados,
  (SUM(CASE WHEN f.atraso_dias > 0 THEN 1 ELSE 0 END) / COUNT(*)) AS taxa_atraso
FROM gold_fato_pedido_item f
JOIN gold_dim_data d ON f.date_key_compra = d.date_key
JOIN gold_dim_cliente c ON f.customer_id = c.customer_id
WHERE f.entrega_dias IS NOT NULL
GROUP BY d.ano, d.mes, c.customer_state
ORDER BY d.ano, d.mes, taxa_atraso DESC
LIMIT 20;

