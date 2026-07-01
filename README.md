# Análise de Vendas da Amazon

# Problema de Negócio

A Amazon é o maior marketplace do mundo, com milhões de produtos em seu catálogo. No entanto, manter um portfólio extenso gera custos significativos de armazenagem, logística e gestão de fornecedores.

# Premissas da análise

Dados de 100.000 vendas feitas pela Amazon.

Vendas realizadas nos Estados Unidos, Canadá, Índia, Reino Unido e Austrália.

Os dados analisados correspondem ao período de 2020 a 2024.

# Estratégia da solução

O método Fato-Dimensão foi usado para desenvolver a análise de dados.

# Passo 1: Resumir o contexto em uma pergunta aberta

As perguntas abertas são um tipo de demanda muito comum em análise de dados nas quais a demanda possui N possíveis soluções e cabe ao analista de dados avaliar as possibilidades e escolher a alternativa com o maior retorno e o menor esforço possível. Para essa análise, foi definida a seguinte pergunta aberta:

Como estão as vendas da Amazon? A empresa tem muitos produtos bons ou é simplesmente um armazém de brechó?

# Passo 2: Transformar pergunta aberta em fechada

As perguntas fechadas são um tipo de demanda muito comum na área de análise de dados. Essa demanda contém todos os detalhes da análise de dados e direciona o analista exatamente para o que precisa ser feito. Geralmente, a pergunta fechada é a escolha de uma solução entre todas as alternativas possíveis, feita por um profissional mais sênior da área.

Para essa análise, foi definida a seguinte pergunta fechada:

Pergunta Fechada: Faça um gráfico de Pareto mostrando todas as categorias da Amazon. Veja quais categorias pagam as contas da empresa e quais são simplesmente peso morto no estoque.

# Passo 3: Definição da Coluna Fato

O Fato é a coluna de interesse que representa o ponto focal da análise. Nesse caso, a coluna "OrderID" mostra quantas vendas foram feitas, quando, e é feita uma contagem e segmentação por categoria...

# Passo 4: Identificação das Dimensões

As colunas foram agrupadas em dimensões comuns que fornecem mais detalhes sobre o Fato que será analisado. Foram organizadas as seguintes dimensões:

**Tempo:** OrderDate. (Base cronológica para análises de sazonalidade e tendências).

**Produto e Marca:** ProductID, ProductName, Category, Brand. (Detalhamento do item vendido para análise de portfólio e desempenho por categoria).

**Cliente e Vendedor:** CustomerID, CustomerName, SellerID. (Identificação das partes envolvidas na transação).

**Valores e Custos:** Quantity, UnitPrice, Discount, Tax, ShippingCost, TotalAmount. (Métricas financeiras da venda; TotalAmount é a métrica central de faturamento).

**Pagamento e Status:** PaymentMethod, OrderStatus. (OrderStatus é a variável-alvo para classificação do resultado do pedido).

**Localização:** City, State, Country. (Base geográfica para análises regionais de desempenho).

**Controle:** OrderID. (Identificador único para rastreabilidade do registro).

# Passo 5: Hipóteses Analíticas

**H1:** Não existe a Lei de Pareto na Amazon. As categorias vendem de forma relativamente equilibrada.

**H2:** Apenas duas categorias não apresentam vendas. As demais vendem de forma uniforme.

**H3:** O faturamento por categoria é baixo na maior parte das categorias, mostrando que as receitas estão distribuídas entre diversos segmentos.

**H4:** O lucro de muitas categorias é negativo, indicando que os custos de tráfego e aquisição de clientes superam o ganho obtido por venda.

H5: quais marcas tiveram maior participação nas vendas?

Fiz no início apenas 4 Hipóteses para quebrar o gelo. Isso é uma análise exploratória do dataframe com base na pergunta fechada que o Analista de Dados Sr. (Gustavo Shelby) me sugeriu.

**Hipótese Inicial → Rodar Excel/Análise → Obter Insight → Questionar o Insight → Gerar Nova Hipótese → Rodar Nova Análise.**

# Passo 6: Critérios de Priorização

- **Critério 1:** Dados disponíveis.
- **Critério 2:** Insights acionáveis.

# Passo 7: Priorização das Hipóteses Analíticas

| ID     | Hipótese Analítica                                                                                                       | Mecanismo Acionável                                                                                 | Prioridade |

| **H2** | O crescimento da rede está concentrado em uma única loja, enquanto as demais apresentam baixo crescimento ou estagnação. | 

Intervir nas lojas de baixo desempenho e replicar as boas práticas da unidade com melhor resultado. | 🔴 Alto    |


| **H3** | O crescimento das vendas é impulsionado pelas categorias secundárias (Bakery, Branded e Flavours).                       | 

Reforçar exposição dos produtos, criar combos e incentivar vendas adicionais.                       | 🔴 Alto    |


| **H4** | O aumento do faturamento está relacionado ao crescimento das vendas do tamanho **Large**.                                | 

Treinar a equipe para realizar upsell de tamanho durante o atendimento.                             | 🔴 Alto    |


| **H6** | O crescimento das vendas ocorre principalmente no período da tarde e da noite.                                           | 

Ajustar escalas, estoque e capacidade operacional aos novos horários de maior demanda.              | 🔴 Alto    |


| **H7** | Os fins de semana apresentam comportamento de vendas diferente dos dias úteis.                                           | 

Adequar escala, estoque e campanhas para sábado e domingo.                                          | 🔴 Alto    |


| **H8** | O perfil da localização da loja (corporativa ou residencial) influencia o desempenho das vendas.                         | 

Adaptar a estratégia operacional às características de cada unidade.                                | 🔴 Alto    |


| **H9** | A queda de desempenho de uma loja está associada à baixa adesão aos novos produtos.                                      | 

Reforçar treinamento da equipe e realizar ações de degustação.                                      | 🔴 Alto    |


| **H1** | O crescimento da receita é impulsionado pelo aumento do volume de clientes ou pelo aumento do ticket médio.              | 

Definir ações com base no diagnóstico: eficiência operacional ou aumento do ticket médio.           | 🟡 Médio   |

| **H5** | Produtos premium representam a principal alavanca de crescimento do faturamento.                                         | 

Incentivar a venda consultiva e garantir disponibilidade dos produtos premium.                      | 🟡 Médio   |


# Insights da análise

# Resultados

**📥 Baixe a apresentação em PowerPoint (clique no link e, em seguida, em "Download" ou "View raw"):**  
  [https://docs.google.com/presentation/d/1NhQ-8F8iABrALSBeC_FBwQPg6H6-_DK8/edit?usp=sharing&ouid=114029927907630112086&rtpof=true&sd=true](https://docs.google.com/presentation/d/1NhQ-8F8iABrALSBeC_FBwQPg6H6-_DK8/edit?usp=sharing&ouid=114029927907630112086&rtpof=true&sd=true)

# Próximos passos

Entrar em contato com o engenheiro de dados para entender as fontes de dados da empresa e verificar se os dados coletados estão íntegros e consistentes.

Realizar uma auditoria na base de dados para identificar possíveis falhas de coleta, processamento, integração ou modelagem que possam estar comprometendo as análises.

Comunicar o CEO e a gerência sobre os problemas identificados, destacando que os dados atuais podem não refletir a realidade do negócio.

Recomendar que decisões estratégicas relevantes, como expansão, investimentos ou mudanças operacionais significativas, não sejam tomadas com base na base de dados atual até que sua qualidade e confiabilidade sejam validadas.
