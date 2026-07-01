# Análise de Vendas da Amazon

# Problema de Negócio

Contexto da empresa

A Brew Haven é uma rede de cafeterias que começou com uma pequena loja em um bairro movimentado da cidade. Com o passar dos anos, a empresa expandiu e hoje possui diversas unidades espalhadas pela cidade.

A rede vende diversos produtos, como:

- cafés tradicionais
- bebidas geladas
- chás
- chocolates
- produtos de confeitaria

O negócio funciona de forma semelhante a muitas cafeterias modernas:

- grande fluxo de clientes ao longo do dia
- vendas rápidas no balcão
- ticket médio relativamente baixo
- grande volume de transações

## A dúvida da diretoria

Nos últimos meses, os gestores da Brew Haven começaram a perceber que algumas lojas parecem estar **cada vez mais movimentadas**, principalmente em determinados horários do dia.

Diante desse cenário, a Brew Haven decidiu contratar um Analista de Dados para investigar a seguinte questão:

Existe crescimento no faturamento da rede de cafeterias? Se sim, quais são as principais alavancas desse crescimento?

Esse é o **problema de negócio que vamos investigar neste curso**.


# Premissas da análise

Dados de aproximadamente 148.969 transações de uma rede de cafeterias com 3 lojas em Nova York (Astoria, Lower Manhattan, Hell's Kitchen).

Vendas realizadas no período de Janeiro a Junho de 2023.

Cada linha representa um item dentro de um pedido, permitindo análises de mix de produtos, ticket médio e comportamento por horário.

# Estratégia da solução

O método Fato-Dimensão foi usado para desenvolver a análise de dados.

# Passo 1: Resumir o contexto em uma pergunta aberta

As perguntas abertas são um tipo de demanda muito comum em análise de dados nas quais a demanda possui N possíveis soluções e cabe ao analista de dados avaliar as possibilidades e escolher a alternativa com o maior retorno e o menor esforço possível. Para essa análise, foi definida a seguinte pergunta aberta:

Como estão as vendas da rede de cafeterias? O faturamento está crescendo e quais são as principais alavancas por trás desse crescimento?

# Passo 2: Transformar pergunta aberta em fechada

As perguntas fechadas são um tipo de demanda muito comum na área de análise de dados. Essa demanda contém todos os detalhes da análise de dados e direciona o analista exatamente para o que precisa ser feito. Geralmente, a pergunta fechada é a escolha de uma solução entre todas as alternativas possíveis, feita por um profissional mais sênior da área.

Para essa análise, foi definida a seguinte pergunta fechada:

Pergunta Fechada: Analise o crescimento mês a mês do faturamento total e identifique se ele é impulsionado por aumento de volume de clientes ou por aumento do ticket médio. Em seguida, desdobre essa análise por loja, por categoria de produto, por tamanho de bebida, por horário do dia e por dia da semana, para identificar quais fatores estão puxando (ou travando) o crescimento da rede.

# Passo 3: Definição da Coluna Fato

O Fato é a coluna de interesse que representa o ponto focal da análise. Nesse caso, a coluna "transaction_id" mostra quantas vendas (checkouts) foram feitas, quando, e é feita uma contagem (utilizando valores distintos, já que um mesmo transaction_id pode aparecer em várias linhas quando o cliente leva múltiplos produtos) e segmentação por categoria (como loja, mês, dia da semana, tipo de produto e tamanho). O valor associado a esse fato é o Total_Bill do pedido, que deve ser somado por transaction_id para obter o faturamento real de cada venda.

# Passo 4: Identificação das Dimensões

Dimensões Analisadas do dataframe da Brew Haven
Dimensão	Variáveis Representativas
Tempo (Data/Hora do Pedido)	transaction_date (Data), transaction_time (Horário), Hour (Hora do dia), Month (Mês numérico), Month Name (Nome do Mês), Day Name (Nome do Dia), Day of Week (Dia da semana em número)
Loja (Onde Foi Vendido)	store_id (ID da loja), store_location (Bairro/Cidade: Astoria, Lower Manhattan, Hell's Kitchen)
Produto (O que Foi Vendido)	product_id (ID do produto), product_category (Categoria macro), product_type (Subcategoria), product_detail (Descrição detalhada), Size (Tamanho do produto)
Item do Pedido (Linha da Transação)	transaction_id (Cabeçalho do pedido - agrupa os itens da mesma compra), transaction_qty (Quantidade de itens na linha), unit_price (Preço unitário), Total_Bill (Valor total da linha: qty × price)

# Passo 5: Hipóteses Analíticas

H1 – Crescimento vs. Ticket Médio
O crescimento do faturamento é puxado pelo aumento de clientes (volume), não pelo ticket médio.
Teste: Comparar a evolução mensal do número de transações versus o ticket médio.

H2 – Concentração do Crescimento
Apenas uma loja está puxando o crescimento da rede; as demais estão estagnadas ou em queda.
Teste: Visualizar a trajetória de faturamento e volume de transações de cada loja ao longo dos meses.

H3 – Categorias Secundárias
Categorias não-core (Bakery, Branded/Flavours) estão crescendo mais que o café tradicional.
Teste: Calcular a participação percentual (%) dessas categorias no faturamento total por mês e verificar se está aumentando.

H4 – Upsell de Tamanho
Produtos no tamanho "Large" estão ganhando participação de mercado.
Teste: Filtrar bebidas (Café, Chá, Chocolate) e calcular a participação % do tamanho Large no volume de itens vendidos por mês.

H5 – Itens Premium
Produtos de alto valor (top 10% de preço) são os principais responsáveis pelo aumento do gasto médio.
Teste: Comparar o crescimento da receita dos itens mais caros (percentil 90) versus os itens de preço médio/baixo.

H6 – Mudança de Horário de Consumo
As manhãs estão saturadas; o crescimento real está vindo das tardes/noites.
Teste: Calcular a variação percentual do faturamento por hora do dia (comparando faixas como 6h–11h vs. 12h–18h) mês a mês.

H7 – Perfil de Dias Úteis vs. Fim de Semana
O mix de produtos e o crescimento são radicalmente diferentes entre dias úteis e fins de semana.
Teste: Separar a base em Dia Útil (Seg–Sex) e Fim de Semana (Sáb–Dom) e comparar evolução do faturamento e Top 5 tipos de produto.

H8 – Perfil das Lojas (Corporativa vs. Residencial)
"Lower Manhattan" (área corporativa) tem maior ticket médio e picos pela manhã; "Astoria" (área residencial) tem vendas mais constantes e fracionadas.
Teste: Comparar por loja o Ticket Médio, Itens por Transação e distribuição de horários de pico.

H9 – Aderência a Novos Produtos
A loja com pior performance está atrasada na adoção dos novos best-sellers que estão impulsionando as outras lojas.
Teste: Identificar os produtos que mais cresceram na rede nos últimos meses e verificar se a loja com queda vendeu esses itens proporcionalmente menos.




Fiz no início apenas 4 Hipóteses para quebrar o gelo. Isso é uma análise exploratória do dataframe com base na pergunta fechada que o Analista de Dados Sr. (Gustavo Shelby) me sugeriu.

**Hipótese Inicial → Rodar Excel/Análise → Obter Insight → Questionar o Insight → Gerar Nova Hipótese → Rodar Nova Análise.**

# Passo 6: Critérios de Priorização

- **Critério 1:** Dados disponíveis.
- **Critério 2:** Insights acionáveis.

# Passo 7: Priorização das Hipóteses Analíticas

| ID     | Hipótese Analítica                                                                                                       | Mecanismo Acionável                                                                                 | Prioridade |

| **H2** | O crescimento da rede está concentrado em uma única loja, enquanto as demais apresentam baixo crescimento ou estagnação. | 

Intervir nas lojas de baixo desempenho e replicar as boas práticas da unidade com melhor resultado. | 🔴 Alto    |
]


| **H3** | O crescimento das vendas é impulsionado pelas categorias secundárias (Bakery, Branded e Flavours).                       | 

Reforçar exposição dos produtos, criar combos e incentivar vendas adicionais.                       | 🔴 Alto    |
]

| **H4** | O aumento do faturamento está relacionado ao crescimento das vendas do tamanho **Large**.                                | 

Treinar a equipe para realizar upsell de tamanho durante o atendimento.                             | 🔴 Alto    |
]

| **H6** | O crescimento das vendas ocorre principalmente no período da tarde e da noite.                                           | 

Ajustar escalas, estoque e capacidade operacional aos novos horários de maior demanda.              | 🔴 Alto    |
]

| **H7** | Os fins de semana apresentam comportamento de vendas diferente dos dias úteis.                                           | 

Adequar escala, estoque e campanhas para sábado e domingo.                                          | 🔴 Alto    |
]

| **H8** | O perfil da localização da loja (corporativa ou residencial) influencia o desempenho das vendas.                         | 

Adaptar a estratégia operacional às características de cada unidade.                                | 🔴 Alto    |
]

| **H9** | A queda de desempenho de uma loja está associada à baixa adesão aos novos produtos.                                      | 

Reforçar treinamento da equipe e realizar ações de degustação.                                      | 🔴 Alto    |
]

| **H1** | O crescimento da receita é impulsionado pelo aumento do volume de clientes ou pelo aumento do ticket médio.              | 

Definir ações com base no diagnóstico: eficiência operacional ou aumento do ticket médio.           | 🟡 Médio   |
]

| **H5** | Produtos premium representam a principal alavanca de crescimento do faturamento.                                         | 

Incentivar a venda consultiva e garantir disponibilidade dos produtos premium.                      | 🟡 Médio   |
]

# Insights da análise

# Resultados

**📥 Baixe a apresentação em PowerPoint (clique no link e, em seguida, em "Download" ou "View raw"):**  
  [https://docs.google.com/presentation/d/1NhQ-8F8iABrALSBeC_FBwQPg6H6-_DK8/edit?usp=sharing&ouid=114029927907630112086&rtpof=true&sd=true](https://docs.google.com/presentation/d/1NhQ-8F8iABrALSBeC_FBwQPg6H6-_DK8/edit?usp=sharing&ouid=114029927907630112086&rtpof=true&sd=true)

# Próximos passos

Entrar em contato com o engenheiro de dados para entender as fontes de dados da empresa e verificar se os dados coletados estão íntegros e consistentes.

Realizar uma auditoria na base de dados para identificar possíveis falhas de coleta, processamento, integração ou modelagem que possam estar comprometendo as análises.

Comunicar o CEO e a gerência sobre os problemas identificados, destacando que os dados atuais podem não refletir a realidade do negócio.

Recomendar que decisões estratégicas relevantes, como expansão, investimentos ou mudanças operacionais significativas, não sejam tomadas com base na base de dados atual até que sua qualidade e confiabilidade sejam validadas.
