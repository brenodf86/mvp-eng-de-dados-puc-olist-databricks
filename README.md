# MVP — Pipeline de Dados em Nuvem com Databricks (Olist)

# Objetivo

Problema

No e-commerce, atrasos de entrega e experiência do cliente afetam diretamente satisfação (reviews), custos logísticos e performance de vendedores/categorias. Este MVP constrói um pipeline de dados na nuvem (Databricks Community Edition) para organizar os dados do Olist em um modelo analítico (Esquema Estrela) e responder perguntas de negócio relacionadas a logística, atrasos e satisfação do cliente.

# Perguntas de negócio

1- Qual é a taxa de pedidos entregues com atraso por mês e por UF?

2- Quais categorias de produto têm maior atraso médio (em dias) e maior taxa de atraso?

3- Existe relação entre atraso e nota de review (review_score)?

4- Qual o tempo médio entre compra e entrega por UF e por seller?

5- Quais formas de pagamento possuem maior ticket médio e como se relacionam com atraso?

6- Quais sellers concentram mais volume de vendas e qual sua performance (atraso e review)?

# Dataset: 
Brazilian E-Commerce Public Dataset by Olist (Kaggle)

Método: download manual no Kaggle + upload dos CSVs no Databricks.

# Fonte: 
Kaggle — Brazilian E-Commerce Public Dataset by Olist

# Licença: 
CC BY-NC-SA 4.0 https://creativecommons.org/licenses/by-nc-sa/4.0/

# Arquitetura

DBFS (raw) → Bronze (Delta) → Silver (Delta) → Gold (Esquema Estrela)

# Prints: docs/evidencias/

## Modelagem (Esquema Estrela)
![Star Schema](docs1/modelagem_star_schema.drawio.png)

# Diagrama: 
docs1/modelagem_star_schema_drawio.png

# Tabelas Gold: 
dims + fato

# Catálogo de Dados e Qualidade

# Catálogo: 
docs/catalogo_dados.md

# Evidências: 
prints de profiling e validações

# ETL / Carga

# Notebooks:

notebooks/01_bronze_ingestao.*

notebooks/02_silver_transformacoes.*

notebooks/03_gold_modelagem_estrela.*

notebooks/04_data_quality.*

notebooks/05_analises_sql_e_resultados.*

# Resultados (resumo)

P1: … (1 parágrafo com achado)

P2: …

P3: …

# Evidências

# Upload DBFS: 
docs/evidencias/print_dbfs_raw.png

# Tabelas Bronze/Silver/Gold: 
docs/evidencias/print_tables.png

# Qualidade: 
docs/evidencias/print_quality.png

# Respostas: 
docs/evidencias/print_p1.png, …

# Autoavaliação

docs/autoavaliacao.md



# mvp-eng-de-dados-puc-olist-databricks

Fonte: Kaggle https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce?resource=download

Licença: CC BY-NC-SA 4.0 https://creativecommons.org/licenses/by-nc-sa/4.0/

Evidência

Brazilian E-Commerce Public Dataset by Olist
100,000 Orders with product, customer and reviews info

Conjunto de Dados Públicos de E-commerce Brasileiro da Olist: 100.000 Pedidos com informações de produtos, clientes e avaliações

Sobre o Conjunto de Dados: Conjunto de Dados Públicos de E-commerce Brasileiro da Olist Bem-vindo! Este é um conjunto de dados públicos de e-commerce brasileiro com informações de pedidos realizados na Loja Olist. O conjunto de dados contém informações de 100 mil pedidos feitos entre 2016 e 2018 em diversos marketplaces no Brasil. Suas funcionalidades permitem visualizar um pedido sob múltiplas perspectivas: desde o status do pedido, preço, pagamento e desempenho do frete até a localização do cliente, atributos do produto e, finalmente, avaliações escritas pelos clientes. Também disponibilizamos um conjunto de dados de geolocalização que relaciona CEPs brasileiros a coordenadas de latitude e longitude.

Estes são dados comerciais reais, foram anonimizados e as referências às empresas e parceiros no texto das avaliações foram substituídas pelos nomes das grandes casas de Game of Thrones.




