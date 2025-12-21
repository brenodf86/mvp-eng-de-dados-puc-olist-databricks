# MVP — Pipeline de Dados em Nuvem com Databricks (Olist)

## Visão Geral
Este MVP constrói um pipeline de dados na nuvem utilizando **Databricks Community/Free Edition** e o dataset público **Olist (Kaggle)**.
O objetivo é organizar os dados em camadas (Raw → Bronze → Silver → Gold) e disponibilizar um **modelo analítico em Esquema Estrela** para responder perguntas de negócio sobre **logística, atrasos e satisfação do cliente (reviews)**.

---

## 1) Objetivo

### Problema
No e-commerce, atrasos de entrega e a experiência do cliente afetam diretamente satisfação (reviews), custos logísticos e performance de vendedores/categorias.
Este MVP organiza os dados do Olist em um DW/Modelo Estrela para analisar atraso de entrega e seus impactos.

### Perguntas de Negócio
1. Qual é a taxa de pedidos entregues com atraso por mês e por UF?  
2. Quais categorias de produto têm maior atraso médio (em dias) e maior taxa de atraso?  
3. Existe relação entre atraso e nota de review (review_score)?  
4. Qual o tempo médio entre compra e entrega por UF e por seller?  
5. Quais formas de pagamento possuem maior ticket médio e como se relacionam com atraso?  
6. Quais sellers concentram mais volume de vendas e qual sua performance (atraso e review)?

---

## 2) Dataset / Fonte e Licença

- **Dataset:** Brazilian E-Commerce Public Dataset by Olist (Kaggle)
- **Coleta:** download manual no Kaggle + upload no Databricks via Data Ingestion
- **Licença:** CC BY-NC-SA 4.0  
  https://creativecommons.org/licenses/by-nc-sa/4.0/

**Evidências:**
- Kaggle: `evidencias/01_kaggle_dataset.png`
- Upload/Ingestion: `evidencias/02_ingestion_upload.png`

---

## 3) Arquitetura e Pipeline (Camadas)

**Arquitetura:**
Raw (tabelas `raw_*`) → Bronze (Delta) → Silver (Delta) → Gold (Esquema Estrela)

- **Raw:** tabelas criadas via upload no Databricks (Data Ingestion)
- **Bronze:** padronização inicial e persistência em Delta
- **Silver:** tratamento de tipos, enriquecimentos e regras (ex.: datas, atrasos)
- **Gold:** dimensões + fato para análises (DW / Star Schema)

**Evidências:**
- Raw: `evidencias/03_raw_tables.png`
- Bronze: `evidencias/04_bronze_tables.png`
- Silver: `evidencias/05_silver_tables.png`
- Gold: `evidencias/06_gold_tables.png`

---

## 4) Modelagem (Esquema Estrela)

**Fato principal:** `gold_fato_pedido_item`  
**Dimensões:** `gold_dim_produto`, `gold_dim_cliente`, `gold_dim_vendedor`, `gold_dim_data`, `gold_dim_pedido`

**Diagrama (Star Schema):**
- `docs1/modelagem_star_schema.png`
- Evidência: `evidencias/11_star_schema.png`

---

## 5) Catálogo de Dados e Qualidade

- Catálogo e profiling: `docs/catalogo_dados.md`
- Evidência (prints): `evidencias/07_quality_profile.png`

Principais verificações:
- Nulos por atributo
- Cardinalidade/distinct por coluna
- Faixas numéricas (min/max/média)
- Checagens de integridade nas chaves (dim ↔ fato) quando aplicável

---

## 6) ETL / Carga (Notebooks)

Os notebooks foram construídos e executados no Databricks, e exportados para este repositório.

- `notebooks/01_bronze_ingestao.*`
- `notebooks/02_silver_transformacoes.*`
- `notebooks/03_gold_modelagem_estrela.*`
- `notebooks/04_data_quality_catalogo.*`
- `notebooks/05_analises_respostas.*`

---

## 7) Resultados (Respostas)

### P1 — Taxa de atraso por mês e UF
**(preencher com 1 parágrafo)**  
Ex.: Identificou-se que a taxa de atraso varia significativamente entre UFs e ao longo dos meses, sugerindo influência de sazonalidade e eficiência logística regional. Estados com maior volume apresentaram padrões mais estáveis, enquanto UFs com menor volume oscilaram mais.

Evidência: `evidencias/08_p1_result.png`

### P2 — Categorias com maior atraso médio e taxa de atraso
**(preencher com 1 parágrafo)**  
Ex.: Algumas categorias apresentaram atraso médio superior e também maior incidência de atraso, indicando possível fragilidade na cadeia logística (peso/volume, origem do seller, etc.). Essas categorias são candidatas prioritárias para ações de melhoria.

Evidência: `evidencias/09_p2_result.png`

### P3 — Relação entre atraso e review_score
**(preencher com 1 parágrafo)**  
Ex.: Observou-se tendência de queda no review_score conforme aumenta o atraso, sugerindo correlação negativa entre pontualidade e satisfação. Embora não prove causalidade, é um indicador operacional relevante.

Evidência: `evidencias/10_p3_result.png`

---

## 8) Discussão de atingimento do objetivo
As perguntas (1), (2) e (3) foram respondidas diretamente via SQL sobre o modelo Gold, permitindo analisar taxa de atraso por período/UF, categorias com maior atraso e a relação entre atraso e review_score.

As perguntas (4) e (6) foram parcialmente respondidas, pois demandam agregações adicionais por seller e validações complementares de integridade/duplicidade em joins por pedido e itens.

A pergunta (5) foi abordada a partir da tabela de pagamentos, porém uma modelagem mais completa para pagamentos como fato separado (com dim de payment_type) é uma evolução desejável.

---

## 9) Autoavaliação
A autoavaliação está em:
- `docs/autoavaliacao.md`

