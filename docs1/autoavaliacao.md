Autoavaliação

Neste MVP, defini como objetivo construir um pipeline de dados em nuvem para analisar atrasos de entrega e seu impacto na satisfação do cliente utilizando a base pública Olist. O pipeline foi implementado no Databricks Community Edition, estruturado em camadas Bronze/Silver/Gold, com persistência em Delta Lake e modelagem analítica em Esquema Estrela.

Atingimento dos objetivos

As perguntas (1), (2) e (3) foram respondidas diretamente via SQL sobre o modelo Gold, permitindo analisar taxa de atraso por período e UF, categorias com maior atraso e a relação entre atraso e review_score. As perguntas (4) e (6) foram parcialmente respondidas, pois demandam agregações adicionais por seller e validações complementares de integridade/duplicidade em joins por pedido e itens. A pergunta (5) foi abordada a partir da tabela de pagamentos, porém uma modelagem mais completa para pagamentos como fato separado (com dim de payment_type) é uma evolução desejável.

Dificuldades

A principal dificuldade foi garantir tipagem correta de datas e a conciliação de granularidades diferentes (pedido vs item vs pagamento vs review). Também houve atenção para lidar com valores ausentes, especialmente em datas de entrega, e assegurar que análises de atraso considerassem somente pedidos entregues.

Trabalhos futuros

Como evolução, pretendo: (1) criar testes automatizados de qualidade (validações de PK/FK e domínios), (2) orquestrar o pipeline com Jobs/Workflows, (3) construir um dashboard em Databricks SQL, e (4) explorar um modelo preditivo simples para prever probabilidade de atraso e impacto em review.
