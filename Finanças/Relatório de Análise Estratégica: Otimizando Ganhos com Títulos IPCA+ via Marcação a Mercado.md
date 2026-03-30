# **Relatório de Análise Estratégica: Otimizando Ganhos com Títulos IPCA+ via Marcação a Mercado**

## **Sumário Executivo**

Este relatório apresenta uma análise aprofundada sobre a estratégia de gestão ativa de títulos públicos federais, com foco específico na venda antecipada de títulos Tesouro IPCA+ de longo prazo para capitalizar sobre o fenômeno da marcação a mercado. A tese central demonstra que a Renda Fixa, quando abordada taticamente, pode gerar ganhos de capital expressivos, potencialmente superando os retornos de ativos de renda variável em determinados ciclos econômicos.  
A análise histórica do ciclo de afrouxamento monetário ocorrido no Brasil entre 2016 e 2018 serve como um estudo de caso robusto, validando o potencial da estratégia. Durante este período, a taxa Selic foi reduzida de 14,25% para 6,5% ao ano, resultando em uma valorização extraordinária para os detentores de títulos IPCA+ longos, com o Tesouro IPCA+ 2035 acumulando um retorno de 181% entre 2016 e 2019\.1  
Uma simulação matemática detalhada, baseada em aportes mensais de R$ 100,00, quantifica o potencial de ganhos. Em um cenário otimista, que projeta uma queda de 400 pontos-base nos juros reais, o portfólio simulado apresenta um retorno líquido de aproximadamente 55% sobre o capital aportado, evidenciando a assimetria positiva da estratégia.  
A conclusão principal é que a venda antecipada de títulos IPCA+ de longo prazo representa uma estratégia viável e potente para investidores com conhecimento intermediário, horizonte de investimento de médio prazo (superior a dois anos para otimização fiscal) e tolerância à volatilidade de curto prazo. O sucesso da operação depende fundamentalmente do monitoramento ativo de indicadores macroeconômicos, como a curva de juros e as projeções do Boletim Focus, para identificar os momentos ótimos de compra e venda.  

---

## **1\. Fundamentos da Marcação a Mercado em Títulos Públicos**

### **1.1. O Mecanismo de Precificação Diária: Do Valor na Curva ao Valor de Mercado**

A marcação a mercado (MaM) é o processo de atualização diária do preço de um ativo financeiro para refletir o seu valor de negociação corrente no mercado secundário.2 Em vez de apresentar ao investidor um valor que evolui linearmente com base na taxa contratada (conhecido como "marcação na curva"), a MaM mostra por quanto aquele ativo poderia ser vendido ou comprado naquele exato momento.4 Desde 2023, por determinação da Associação Brasileira das Entidades dos Mercados Financeiro e de Capitais (ANBIMA), bancos e corretoras são obrigados a divulgar diariamente a marcação a mercado de ativos de renda fixa, incluindo títulos públicos, aumentando a transparência para o investidor.2  
Este mecanismo é o que introduz volatilidade — e, consequentemente, oportunidade — em uma classe de ativos tradicionalmente vista como "fixa".6 É crucial entender que a MaM afeta apenas o investidor que decide vender seu título antes da data de vencimento. Para aquele que mantém o ativo em carteira até o resgate final, a rentabilidade acordada no momento da aplicação está garantida (assumindo a ausência de um evento de crédito do emissor).3 A flutuação diária no extrato, portanto, não representa uma perda ou ganho efetivo, mas sim uma indicação do valor de liquidação imediata do ativo.  
Essa mudança regulatória representa mais do que uma simples alteração na forma de exibição dos saldos; ela catalisa uma mudança de paradigma. Ao expor a volatilidade diária, a MaM incentiva o investidor de varejo a evoluir de uma postura passiva de "comprar e esquecer" para uma gestão mais ativa e tática de seu portfólio de renda fixa. A ansiedade inicial gerada pela flutuação pode ser convertida em consciência estratégica, onde uma "perda" momentânea durante um ciclo de alta de juros é corretamente interpretada como uma oportunidade de compra a preços mais baixos, e um "ganho" durante um ciclo de queda de juros se revela uma oportunidade de realização de lucros.

### **1.2. A Relação Inversa: Como as Taxas de Juros Futuras Determinam o Preço Presente dos Títulos**

O princípio fundamental que rege a marcação a mercado de títulos prefixados e híbridos é a relação inversa entre o preço do título e as taxas de juros praticadas no mercado.8 Quando as taxas de juros futuras sobem, o preço dos títulos já emitidos cai. Quando as taxas de juros futuras caem, o preço dos títulos já emitidos sobe.  
A lógica intuitiva por trás desse comportamento é baseada na competitividade. Imagine que um investidor adquiriu um Tesouro IPCA+ que paga uma taxa real de IPCA \+ 6% ao ano. Se, meses depois, devido a uma melhora no cenário econômico, o Tesouro passa a emitir novos títulos com a mesma data de vencimento, mas pagando uma taxa real de IPCA \+ 5%, o título antigo, que paga 1 p.p. a mais, torna-se mais valioso. No mercado secundário, outros investidores estarão dispostos a pagar um valor superior ao que foi pago originalmente (ágio) para adquirir essa rentabilidade maior.9 O inverso ocorre se as taxas de mercado subirem para IPCA \+ 7%; o título antigo se torna menos atrativo e seu preço de negociação cai (deságio).6  
Matematicamente, essa relação é explicada pela fórmula de valor presente. O Preço Unitário (PU) de um título é o valor presente de todos os seus fluxos de caixa futuros (pagamentos de juros e principal), descontados a uma taxa que reflete as condições atuais de mercado. Para um título sem cupons (como o Tesouro IPCA+ Principal), a fórmula simplificada é:  
PU=(1+i)nVF​  
Onde VF é o valor de face no vencimento, i é a taxa de juros de mercado e n é o prazo até o vencimento. Fica evidente que, se a taxa i (o denominador) aumenta, o PU (o resultado) necessariamente diminui, e vice-versa.6

### **1.3. Análise Comparativa dos Títulos do Tesouro Direto**

A sensibilidade de um título à marcação a mercado varia drasticamente conforme sua natureza (pós-fixado, prefixado ou híbrido).

* **Tesouro Selic (LFT):** Sendo um título pós-fixado, sua rentabilidade é atrelada à variação diária da taxa Selic. Como seu rendimento já reflete a taxa de juros corrente da economia, as *expectativas futuras* sobre os juros têm um impacto mínimo em seu preço. Sua volatilidade é extremamente baixa, tornando-o o ativo mais seguro para resgates antecipados e o veículo ideal para a construção de reservas de emergência.7  
* **Tesouro Prefixado (LTN):** Com uma taxa de juros nominal totalmente fixa, definida no momento da compra, este título é o mais sensível às oscilações das expectativas de juros futuros. Qualquer variação na curva de juros impacta diretamente sua competitividade, resultando em variações de preço significativas. Em um cenário de queda de juros, sua valorização tende a ser a mais expressiva entre os títulos públicos.3  
* **Tesouro IPCA+ (NTN-B Principal):** Como um título híbrido, sua rentabilidade é composta pela variação da inflação (IPCA) mais uma taxa de juro real prefixada. Sua sensibilidade à marcação a mercado é dupla: ele é afetado pelas expectativas de inflação e, de forma mais contundente, pelas variações na curva de *juros reais*. É essa segunda componente que oferece a oportunidade de ganhos de capital em ciclos de queda da Selic, ao mesmo tempo em que a primeira componente protege o poder de compra do investidor.14

A tabela abaixo resume as principais características e sensibilidades de cada título.  
**Tabela 1: Comparativo das Características dos Títulos do Tesouro Direto**

| Título                | Indexador         | Principal Fator de Risco de Mercado                 | Sensibilidade à Marcação a Mercado | Cenário Ideal para Venda Antecipada |
|:--------------------- |:----------------- |:--------------------------------------------------- |:---------------------------------- |:----------------------------------- |
| **Tesouro Selic**     | Taxa Selic        | Variação da Selic diária                            | Muito Baixa                        | Não se aplica para ganho de capital |
| **Tesouro Prefixado** | Taxa Fixa         | Variação das expectativas de juros nominais futuros | Alta                               | Queda das taxas de juros nominais   |
| **Tesouro IPCA+**     | IPCA \+ Taxa Fixa | Variação das expectativas de juros reais futuros    | Moderada a Alta                    | Queda das taxas de juros reais      |

---

## **2\. Análise Histórica: Lições dos Ciclos de Queda da Selic**

### **2.1. Estudo de Caso: O Ciclo de Afrouxamento Monetário de 2016-2018**

O período entre 2016 e 2018 oferece um exemplo paradigmático do potencial da estratégia de venda antecipada de títulos IPCA+.

* **Contexto Macroeconômico:** O Brasil vivia um momento de profunda transição política e econômica. O impeachment da ex-presidente Dilma Rousseff e o início do governo de Michel Temer trouxeram uma nova equipe econômica com uma agenda focada em reformas estruturais e controle fiscal, como a aprovação do Teto de Gastos. Essa mudança de rota reancorou as expectativas de inflação e reduziu drasticamente a percepção de risco-país, criando as condições necessárias para que o Banco Central (BC) iniciasse um dos mais agressivos ciclos de corte de juros da história recente.17  
* **Trajetória da Selic:** O ciclo de afrouxamento monetário teve início na reunião do Comitê de Política Monetária (Copom) de 19 de outubro de 2016, quando a taxa Selic foi reduzida de 14,25% para 14,00% ao ano.18 A partir daí, seguiram-se 12 cortes consecutivos, que levaram a taxa à sua mínima histórica até então, de 6,5% ao ano, em março de 2018\. Este patamar foi mantido até o final daquele ano, em 12 de dezembro de 2018\.19  
* **Performance dos Títulos IPCA+ Longos:** Este cenário macroeconômico foi extremamente benéfico para os detentores de títulos públicos de longo prazo. As taxas de juro real exigidas pelo mercado, que estavam em patamares muito elevados devido à crise fiscal anterior (superiores a 6% a.a.), sofreram uma forte compressão.20 Como resultado, os preços dos títulos IPCA+ longos dispararam. O Tesouro IPCA+ 2035, por exemplo, registrou uma rentabilidade de 52% apenas no ano de 2016\. Analisando o ciclo completo, um investidor que adquiriu este título no início de 2016 e o vendeu no final de 2019 obteve um retorno acumulado de 181%, superando com folga o Ibovespa (171%) e o CDI (aproximadamente 40%) no mesmo período.1

**Tabela 2: Evolução da Meta da Taxa Selic (Reuniões do Copom, 2016-2018)**

| Data da Reunião                                        | Período de Vigência (Início) | Meta Selic (% a.a.) |     |
|:------------------------------------------------------ |:---------------------------- |:------------------- |:--- |
| 31/08/2016                                             | 01/09/2016                   | 14,25               |     |
| **19/10/2016**                                         | **20/10/2016**               | **14,00**           |     |
| 30/11/2016                                             | 01/12/2016                   | 13,75               |     |
| 11/01/2017                                             | 12/01/2017                   | 13,00               |     |
| 22/02/2017                                             | 23/02/2017                   | 12,25               |     |
| 12/04/2017                                             | 13/04/2017                   | 11,25               |     |
| 31/05/2017                                             | 01/06/2017                   | 10,25               |     |
| 26/07/2017                                             | 27/07/2017                   | 9,25                |     |
| 06/09/2017                                             | 07/09/2017                   | 8,25                |     |
| 25/10/2017                                             | 26/10/2017                   | 7,50                |     |
| 06/12/2017                                             | 07/12/2017                   | 7,00                |     |
| 07/02/2018                                             | 08/02/2018                   | 6,75                |     |
| 21/03/2018                                             | 22/03/2018                   | 6,50                |     |
| **12/12/2018**                                         | **13/12/2018**               | **6,50**            |     |
| Fonte: Banco Central do Brasil. Dados compilados de.18 |                              |                     |     |

### **2.2. Padrões em Ciclos de Corte de Juros: O Impacto da Compressão do Prêmio de Risco**

A análise do ciclo de 2016-2018 revela que a valorização dos títulos não se deve apenas à queda da taxa básica de juros, mas, fundamentalmente, à queda da *taxa de juro real de longo prazo* exigida pelo mercado. Esta taxa é composta pela expectativa da Selic real futura acrescida de um prêmio de risco, que reflete incertezas, principalmente de natureza fiscal.  
O que se observa é que o gatilho para os ganhos mais expressivos não é o primeiro corte da Selic em si, mas a percepção do mercado de que o ciclo de alta chegou ao fim e que um ciclo de queda é iminente. O preço de um título reflete as expectativas futuras.3 Portanto, o investidor que se posiciona durante o "pico dos juros", quando o pessimismo é maior e as taxas reais estão mais elevadas, é quem captura a maior parte da valorização. A venda, por sua vez, deve ser planejada para quando o ciclo de cortes já está maduro e as taxas reais comprimiram significativamente. A estratégia, portanto, não é reativa (agir após o corte), mas proativa, baseando-se em indicadores que sinalizam uma virada no ciclo macroeconômico.  

---

## **3\. Simulação de Carteira: Construção de Patrimônio e Potencial de Ganho**

Para ilustrar o impacto da marcação a mercado de forma prática, foi desenvolvida uma simulação de aportes mensais em um título Tesouro IPCA+ de longo prazo, seguida por uma venda antecipada em diferentes cenários de mercado.

### **3.1. Premissas e Metodologia de Cálculo**

* **Cenário de Acumulação:** Aportes mensais de R$ 100,00 durante 24 meses.  
* **Ativo Selecionado:** Título hipotético Tesouro IPCA+ Principal com vencimento em 18 anos no início da simulação, para manter uma *duration* média de aproximadamente 15 anos ao longo do período.  
* **Taxa de Compra:** Assume-se uma taxa de juro real constante de IPCA \+ 5,5% a.a. durante os 24 meses de aportes.  
* **Metodologia:** O Preço Unitário (PU) do título é calculado a cada mês com base na taxa de juros real e no prazo remanescente. O aporte mensal compra uma fração do título. O valor de mercado da carteira é o produto do total de frações acumuladas pelo PU vigente.  
* **Fórmula de Precificação:** A simulação utiliza a fórmula padrão para o Tesouro IPCA+ Principal 21:  
  PU=(1+TaxaReal)252DU​VNA​  
  Para fins de simplificação, o VNA (Valor Nominal Atualizado) é mantido constante em R$ 1.000,00, isolando o efeito da variação da taxa de juros real no PU.

### **3.2. Evolução do Investimento ao Longo de 24 Meses**

A tabela a seguir detalha a acumulação do patrimônio ao longo dos 24 meses, sob a premissa de taxas de juros estáveis.  
**Tabela 3: Simulação de Aportes Mensais em Tesouro IPCA+ (Taxa Real de 5,5% a.a.)**

| Mês | Aporte (R$) | Total Aportado (R$) | PU de Compra (R$) | Frações Adquiridas | Frações Totais | Valor de Mercado (R$) |
|:--- |:----------- |:------------------- |:----------------- |:------------------ |:-------------- |:--------------------- |
| 1   | 100,00      | 100,00              | 442,18            | 0,22615            | 0,22615        | 100,00                |
| 6   | 100,00      | 600,00              | 454,41            | 0,22006            | 1,34433        | 610,87                |
| 12  | 100,00      | 1.200,00            | 479,79            | 0,20842            | 2,64223        | 1.267,71              |
| 18  | 100,00      | 1.800,00            | 506,53            | 0,19742            | 3,89675        | 1.973,86              |
| 24  | 100,00      | 2.400,00            | 534,70            | 0,18702            | 5,11080        | 2.732,75              |

Ao final de 24 meses, o investidor teria aportado um total de R$ 2.400,00. Devido ao rendimento ("carrego") da taxa de 5,5% a.a., o valor de mercado da sua posição seria de R$ 2.732,75.  
**Gráfico 1: Crescimento do Valor Aportado vs. Valor de Mercado do Portfólio**  
*(Um gráfico de linhas ilustraria os dados da Tabela 3, mostrando a linha do "Total Aportado" crescendo linearmente de R$ 100 a R$ 2.400, e a linha do "Valor de Mercado" crescendo de forma ligeiramente exponencial, partindo de R$ 100 e terminando em R$ 2.732,75, sempre acima da linha de aportes.)*

### **3.3. O Evento de Liquidez: Calculando o Ganho de Capital na Venda Antecipada**

No 25º mês, simulamos a venda de toda a posição acumulada (5,11080 frações do título). O preço de venda (PU) dependerá do cenário de juros naquele momento. No cenário otimista, a taxa real de mercado cai de 5,5% para 1,5% (uma compressão de 400 pontos-base).

* **Cálculo do Novo PU (Cenário Otimista):**  
  * Prazo remanescente: 16 anos (4032 dias úteis)  
  * Nova Taxa Real: 1,5% a.a.  
  * PUvenda​=(1+0,015)2524032​1000​=R$788,31  
* **Valor Final de Mercado:**  
  * Valorfinal​=5,11080×R$788,31=R$4.029,20  
* **Ganho de Capital Bruto:**  
  * Ganho=R$4.029,20−R$2.400,00=R$1.629,20

Esta simulação demonstra que o retorno da estratégia não é linear. O ganho de capital de R$ 1.629,20, gerado pelo evento único de queda dos juros, é muito superior ao rendimento acumulado de R$ 332,75 durante os 24 meses de aportes. Isso evidencia a natureza da estratégia: o objetivo principal não é o rendimento contínuo da taxa ("carrego"), mas sim a captura de um evento de re-precificação de capital.
---

## **4\. Análise de Cenários e Sensibilidade**

### **4.1. Projeção de Resultados para o Portfólio Simulado**

Partindo do portfólio acumulado de R$ 2.400,00, os resultados da venda antecipada são projetados para três cenários macroeconômicos distintos.

* **Cenário Base (Estável):** A taxa de juro real de mercado permanece em 5,5%. O PU de venda é R$ 534,70. O valor bruto da carteira é de R$ 2.732,75.  
* **Cenário Otimista (Queda de Juros):** A taxa de juro real cai para 1,5% (-400 bps). O PU de venda sobe para R$ 788,31. O valor bruto da carteira é de R$ 4.029,20.  
* **Cenário Pessimista (Alta de Juros):** A taxa de juro real sobe para 7,5% (+200 bps). O PU de venda cai para R$ 419,00. O valor bruto da carteira é de R$ 2.141,43.

### **4.2. Tabela Comparativa de Resultados Projetados**

A tabela a seguir consolida os resultados, incluindo o impacto tributário, para cada cenário. Assume-se que a venda ocorre após 720 dias, enquadrando-se na alíquota mínima de 15% de Imposto de Renda.  
**Tabela 4: Análise Comparativa de Cenários (Base, Otimista, Pessimista)**

| Métrica                          | Cenário Base (Taxa 5,5%) | Cenário Otimista (Taxa 1,5%) | Cenário Pessimista (Taxa 7,5%) |
|:-------------------------------- |:------------------------ |:---------------------------- |:------------------------------ |
| **PU de Venda (R$)**             | 534,70                   | 788,31                       | 419,00                         |
| **Valor Bruto da Carteira (R$)** | 2.732,75                 | 4.029,20                     | 2.141,43                       |
| **Ganho/Perda Bruto (R$)**       | 332,75                   | 1.629,20                     | \-258,57                       |
| **Alíquota de IR (15%)**         | 15,0%                    | 15,0%                        | N/A (Prejuízo)                 |
| **IR a Pagar (R$)**              | 49,91                    | 244,38                       | 0,00                           |
| **Valor Líquido Final (R$)**     | **2.682,84**             | **3.784,82**                 | **2.141,43**                   |
| **Retorno Líquido**              | **\+11,79%**             | **\+57,70%**                 | **\-10,77%**                   |

### **4.3. Análise de Sensibilidade: O Impacto de Variações de \+/- 2 p.p. nas Taxas**

Para fornecer uma visão mais granular, a matriz de sensibilidade abaixo demonstra o impacto de variações incrementais na taxa de juros real sobre o valor final do portfólio.  
**Tabela 5: Matriz de Sensibilidade do Valor do Portfólio**

| Variação na Taxa Real | Taxa Real Final | Valor Líquido Final (R$) | Retorno Líquido |
|:--------------------- |:--------------- |:------------------------ |:--------------- |
| **\+200 bps**         | 7,50%           | 2.141,43                 | \-10,77%        |
| **\+100 bps**         | 6,50%           | 2.418,97                 | \+0,79%         |
| **0**                 | 5,50%           | 2.682,84                 | \+11,79%        |
| **\-100 bps**         | 4,50%           | 3.064,21                 | \+27,68%        |
| **\-200 bps**         | 3,50%           | 3.407,25                 | \+41,97%        |

Esta análise revela a assimetria positiva da estratégia. No cenário pessimista, a perda de \-10,77% só é realizada se o investidor for forçado a vender. Se tiver flexibilidade de prazo, ele pode manter o título, que continua rendendo a taxa contratada, e aguardar a reversão do ciclo de juros ou simplesmente carregá-lo até o vencimento, onde o principal corrigido e a rentabilidade acordada são garantidos pelo Tesouro Nacional.7 A perda, portanto, é opcional para o investidor paciente. O ganho no cenário otimista, por outro lado, é uma oportunidade real e tática de antecipar lucros e realocar o capital. O risco principal não é a perda permanente de capital, mas sim o risco de liquidez e de custo de oportunidade.
---

## **5\. Estratégias Táticas para Venda Antecipada**

### **5.1. Indicadores-Chave para o Timing de Venda**

Identificar o momento ideal para vender um título IPCA+ e realizar o lucro da marcação a mercado requer o acompanhamento de indicadores que reflitam as expectativas do mercado.

* **Análise da Curva de Juros (Yield Curve):** A curva de juros é um gráfico que mostra as taxas de juros dos títulos para diferentes vencimentos.22 Sua inclinação é um termômetro das expectativas econômicas.23  
  * **Curva Inclinada:** Juros de longo prazo significativamente mais altos que os de curto prazo. Geralmente sinaliza expectativas de crescimento e inflação no futuro, ou um elevado prêmio por risco fiscal. Este é o cenário ideal para *comprar* títulos longos, travando taxas elevadas.  
  * **Curva Achatada (Flattening):** A diferença entre os juros longos e curtos diminui. Este movimento frequentemente antecede ciclos de corte na taxa Selic, pois o mercado passa a projetar uma desaceleração econômica e, consequentemente, juros mais baixos no futuro. O achatamento da curva é um **forte indicador de que o ciclo de alta de juros pode estar no fim**, servindo como um gatilho para se preparar para a venda do título.  
* **Monitoramento do Boletim Focus:** Publicado semanalmente pelo Banco Central, o Boletim Focus consolida as projeções de dezenas de instituições financeiras para indicadores-chave como IPCA, Selic, PIB e câmbio.25  
  * **Gatilho de Venda:** Uma tendência clara e persistente de revisões para baixo nas projeções da Selic e do IPCA para os horizontes de 12 e 24 meses. Quando a mediana das expectativas do mercado aponta para um ciclo de afrouxamento monetário, isso valida a tese de valorização dos títulos prefixados e híbridos, indicando que o momento de máxima compressão das taxas pode estar se aproximando.25

É importante notar que esses indicadores não são ferramentas de previsão infalíveis, mas sim de confirmação de narrativa. A estratégia funciona quando uma narrativa macroeconômica clara se consolida (ex: "o ajuste fiscal permitirá a queda dos juros"). A curva de juros e o Boletim Focus são as formas como o mercado expressa e quantifica sua crença nessa narrativa. O momento ideal de venda ocorre no "pico da euforia", quando as taxas futuras atingem suas mínimas e antes que uma nova narrativa contrária comece a se formar.

### **5.2. Métodos de Proteção (Hedge) para o Investidor de Varejo**

Dado o risco de volatilidade, é prudente adotar estratégias de proteção (*hedge*) para mitigar perdas em caso de movimentos adversos e inesperados nas taxas de juros.27

* **Diversificação de Vencimentos (*Laddering*):** Em vez de concentrar todo o capital em um único título de vencimento muito longo (ex: IPCA+ 2055), o investidor pode construir uma "escada" de vencimentos, alocando em títulos como IPCA+ 2035, 2045 e 2055\. Títulos com prazo menor possuem menor *duration* e, portanto, menor sensibilidade a variações nos juros. Essa diversificação suaviza a volatilidade geral da carteira.  
* **Alocação em Títulos Pós-Fixados (Tesouro Selic):** Manter uma parcela estratégica da carteira de renda fixa em Tesouro Selic funciona como um hedge eficaz. Em um cenário de alta inesperada dos juros, que causaria desvalorização nos títulos IPCA+, o Tesouro Selic se beneficiaria imediatamente da nova taxa mais elevada, compensando parte das perdas. Além disso, a liquidez e estabilidade do Tesouro Selic fornecem "pólvora seca" para aproveitar os preços mais baixos dos títulos longos durante esses períodos de estresse.

---

## **6\. Recomendações Técnicas e Considerações Finais**

### **6.1. Seleção de Ativos**

* **Tipos de Títulos:** Para a estratégia de ganho de capital via marcação a mercado, os títulos mais indicados são os **Tesouro Prefixado** e **Tesouro IPCA+**, devido à sua alta sensibilidade às expectativas de juros.14 O Tesouro IPCA+ oferece uma vantagem sobre o Prefixado puro, pois sua componente de correção inflacionária protege o investidor contra surpresas no IPCA durante o período de investimento.  
* **O Papel da *Duration* e do Prazo:** A sensibilidade de um título a uma variação na taxa de juros é diretamente proporcional à sua *duration* (prazo médio ponderado dos fluxos de caixa). Em termos práticos, quanto mais longo o prazo de vencimento de um título, maior sua *duration* e, consequentemente, maior será a variação de seu preço para uma mesma mudança nos juros de mercado.11 Portanto, para maximizar o potencial de ganho com a marcação a mercado, o investidor deve priorizar os títulos com os vencimentos mais longos disponíveis na plataforma do Tesouro Direto (ex: Tesouro IPCA+ 2045, Tesouro IPCA+ 2055).

### **6.2. Fatores Críticos de Custo e Tributação**

Uma análise completa da estratégia deve incluir os custos operacionais e a tributação, que impactam diretamente o retorno líquido.

* **Imposto de Renda (IR):** A tributação sobre os rendimentos de títulos públicos segue uma tabela regressiva, que beneficia investimentos de longo prazo. A alíquota incide apenas sobre o ganho de capital.31 Para otimizar o retorno líquido, a venda antecipada deve ser planejada para ocorrer, idealmente, após 720 dias (2 anos) do aporte inicial, garantindo a aplicação da alíquota mínima de 15%.32

**Tabela 6: Alíquotas da Tabela Regressiva de Imposto de Renda**

| Prazo da Aplicação        | Alíquota de IR |     |
|:------------------------- |:-------------- |:--- |
| Até 180 dias              | 22,5%          |     |
| De 181 a 360 dias         | 20,0%          |     |
| De 361 a 720 dias         | 17,5%          |     |
| Acima de 720 dias         | 15,0%          |     |
| Fonte: Receita Federal.33 |                |     |

* **Taxa de Custódia da B3:** Há uma taxa de custódia de 0,20% ao ano, calculada sobre o valor total dos títulos.35 A forma de cobrança foi alterada recentemente: a taxa não é mais debitada semestralmente. Agora, o valor acumulado é cobrado em um evento de liquidez, seja na venda antecipada, no pagamento de juros semestrais ou no vencimento do título.37 Existe uma isenção para investimentos no Tesouro Selic de até R$ 10.000,00 por CPF.36 A maioria das corretoras, como a Clear e a XP, isenta os investidores de taxas de administração para o Tesouro Direto.38

### **6.3. Conclusão: Perfil do Investidor e Disciplina Estratégica**

A estratégia de capitalizar sobre a marcação a mercado em títulos IPCA+ não é adequada para todos os perfis de investidor. Ela exige:

1. **Tolerância à Volatilidade:** O investidor deve estar confortável em ver o valor de sua carteira flutuar no curto prazo, entendendo que a "perda" no extrato é temporária e representa uma oportunidade.  
2. **Horizonte de Médio a Longo Prazo:** É necessário um prazo de investimento de, no mínimo, dois a três anos para permitir a maturação de um ciclo de juros e para se beneficiar da alíquota mínima de Imposto de Renda.  
3. **Disciplina e Acompanhamento:** O sucesso depende da disciplina para seguir a estratégia e do acompanhamento regular do cenário macroeconômico e dos indicadores-chave.

O principal risco da estratégia não é a perda de principal, que é garantida pelo Tesouro Nacional no vencimento, mas sim o **risco de liquidez** — a necessidade de resgatar os recursos em um momento desfavorável de mercado, forçando a realização de um prejuízo.  
Como recomendação final, sugere-se que investidores interessados em aplicar esta estratégia comecem com uma alocação pequena de seu portfólio de renda fixa. Essa abordagem gradual permite ganhar experiência prática e confiança na dinâmica da marcação a mercado antes de comprometer um capital mais substancial.

#### **Referências citadas**

1. Tesouro IPCA+: a hora da virada? | VOCÊ S/A, acessado em setembro 11, 2025, [https://vocesa.abril.com.br/mercado-financeiro/tesouro-ipca-a-hora-da-virada/](https://vocesa.abril.com.br/mercado-financeiro/tesouro-ipca-a-hora-da-virada/)  
2. O que é marcação a mercado? | Educação Financeira \- Serasa, acessado em setembro 11, 2025, [https://www.serasa.com.br/blog/investimentos-o-que-marcacao-mercado/](https://www.serasa.com.br/blog/investimentos-o-que-marcacao-mercado/)  
3. Marcação a mercado: o que é e como funciona na Renda Fixa?, acessado em setembro 11, 2025, [https://blog.toroinvestimentos.com.br/renda-fixa/marcacao-a-mercado/](https://blog.toroinvestimentos.com.br/renda-fixa/marcacao-a-mercado/)  
4. Entenda de uma vez por todas o que é marcação a mercado e como ganhar dinheiro com ela \- Suno, acessado em setembro 11, 2025, [https://www.suno.com.br/noticias/colunas/guilherme-puim/o-que-e-marcacao-a-mercado-como-ganhar-dinheiro/](https://www.suno.com.br/noticias/colunas/guilherme-puim/o-que-e-marcacao-a-mercado-como-ganhar-dinheiro/)  
5. Documento tira dúvidas sobre marcação a mercado para títulos de renda fixa; regra já entrou em vigor \- Anbima, acessado em setembro 11, 2025, [https://www.anbima.com.br/pt\_br/noticias/documento-tira-duvidas-sobre-marcacao-a-mercado-para-titulos-de-renda-fixa-regra-ja-entrou-em-vigor.htm](https://www.anbima.com.br/pt_br/noticias/documento-tira-duvidas-sobre-marcacao-a-mercado-para-titulos-de-renda-fixa-regra-ja-entrou-em-vigor.htm)  
6. Entendendo a relação entre juros e preço dos títulos (marcação a mercado) \- Expert XP, acessado em setembro 11, 2025, [https://conteudos.xpi.com.br/aprenda-a-investir/relatorios/entendendo-a-relacao-entre-juros-e-preco/](https://conteudos.xpi.com.br/aprenda-a-investir/relatorios/entendendo-a-relacao-entre-juros-e-preco/)  
7. Marcação a mercado: o que é e como funciona \- Fala, Nubank, acessado em setembro 11, 2025, [https://blog.nubank.com.br/marcacao-a-mercado/](https://blog.nubank.com.br/marcacao-a-mercado/)  
8. Marcação a Mercado: entenda como proteger e potencializar seus investimentos \- íon Itaú, acessado em setembro 11, 2025, [https://www.ion.itau/news/marcacao-a-mercado-entenda-como-proteger-e-potencializar-seus-investimentos/](https://www.ion.itau/news/marcacao-a-mercado-entenda-como-proteger-e-potencializar-seus-investimentos/)  
9. Como as taxas de juros afetam os títulos? \- MAPFRE, acessado em setembro 11, 2025, [https://www.mapfre.com/pt-br/comunicacao/economia-comunicacao/taxas-juros-titulos/](https://www.mapfre.com/pt-br/comunicacao/economia-comunicacao/taxas-juros-titulos/)  
10. Relação entre preços de títulos e taxas de juros (vídeo) \- Khan Academy, acessado em setembro 11, 2025, [https://pt.khanacademy.org/economics-finance-domain/macroeconomics/monetary-system-topic/macro-financial-assets/v/relationship-between-bond-prices-and-interest-rates](https://pt.khanacademy.org/economics-finance-domain/macroeconomics/monetary-system-topic/macro-financial-assets/v/relationship-between-bond-prices-and-interest-rates)  
11. Qual é a relação entre taxa de juros e os preços dos títulos? \- TopInvest, acessado em setembro 11, 2025, [https://www.topinvest.com.br/qual-e-a-relacao-entre-taxa-de-juros-e-os-precos-dos-titulos/](https://www.topinvest.com.br/qual-e-a-relacao-entre-taxa-de-juros-e-os-precos-dos-titulos/)  
12. Marcação a mercado: o que é, como funciona e vantagens \- BTG Content, acessado em setembro 11, 2025, [https://content.btgpactual.com/blog/investimentos/marcacao-a-mercado-o-que-e-como-funciona-e-vantagens](https://content.btgpactual.com/blog/investimentos/marcacao-a-mercado-o-que-e-como-funciona-e-vantagens)  
13. Prefixado ou IPCA+? O Que Fazer com os Títulos do Tesouro com a Selic em Alta, acessado em setembro 11, 2025, [https://forbes.com.br/forbes-money/2024/10/prefixado-ou-ipca-o-que-fazer-com-os-titulos-do-tesouro-com-a-selic-em-alta/](https://forbes.com.br/forbes-money/2024/10/prefixado-ou-ipca-o-que-fazer-com-os-titulos-do-tesouro-com-a-selic-em-alta/)  
14. O que é marcação a mercado e como funciona esse 'segredo' da ..., acessado em setembro 11, 2025, [https://www.nordinvestimentos.com.br/blog/marcacao-a-mercado-o-que-e-como-funciona/](https://www.nordinvestimentos.com.br/blog/marcacao-a-mercado-o-que-e-como-funciona/)  
15. Tesouro Prefixado dispara: ainda dá para lucrar com a marcação a mercado? \- E-Investidor, acessado em setembro 11, 2025, [https://einvestidor.estadao.com.br/investimentos/tesouro-prefixado-marcacao-a-mercado/](https://einvestidor.estadao.com.br/investimentos/tesouro-prefixado-marcacao-a-mercado/)  
16. Tesouro IPCA+ x Tesouro Selic: qual a melhor opção com inflação e ..., acessado em setembro 11, 2025, [https://www.infomoney.com.br/onde-investir/tesouro-ipca-x-tesouro-selic-qual-a-melhor-opcao-com-inflacao-e-juros-em-alta/](https://www.infomoney.com.br/onde-investir/tesouro-ipca-x-tesouro-selic-qual-a-melhor-opcao-com-inflacao-e-juros-em-alta/)  
17. Mercado brasileiro evolui com liquidez, ciclos políticos e impacto global \- InfoMoney, acessado em setembro 11, 2025, [https://www.infomoney.com.br/advisor/mercado-brasileiro-evolui-com-liquidez-ciclos-politicos-e-impacto-global/](https://www.infomoney.com.br/advisor/mercado-brasileiro-evolui-com-liquidez-ciclos-politicos-e-impacto-global/)  
18. Taxa Selic | 2016 \- ADVFN, acessado em setembro 11, 2025, [https://br.advfn.com/indicadores/taxa-selic/2016](https://br.advfn.com/indicadores/taxa-selic/2016)  
19. Copom mantém Selic em 6,5% ao ano na primeira reunião após ..., acessado em setembro 11, 2025, [https://agenciabrasil.ebc.com.br/economia/noticia/2018-10/copom-mantem-juros-basicos-em-65-ao-ano-apos-eleicoes](https://agenciabrasil.ebc.com.br/economia/noticia/2018-10/copom-mantem-juros-basicos-em-65-ao-ano-apos-eleicoes)  
20. Tesouro Direto: após taxas atingirem em julho os maiores níveis desde 2016, gestores sugerem cautela \- TradeMap, acessado em setembro 11, 2025, [https://trademap.com.br/agencia/mercados/tesouro-direto-apos-taxas-atingirem-em-julho-os-maiores-niveis-desde-2016-gestores-sugerem-cautela](https://trademap.com.br/agencia/mercados/tesouro-direto-apos-taxas-atingirem-em-julho-os-maiores-niveis-desde-2016-gestores-sugerem-cautela)  
21. E-book de tópicos avançados MÓDULO 03, acessado em setembro 11, 2025, [https://repositorio.enap.gov.br/bitstream/1/6248/8/T%C3%B3picos%20Avan%C3%A7ados%20Cap%C3%ADtulo%203%20TD.pdf](https://repositorio.enap.gov.br/bitstream/1/6248/8/T%C3%B3picos%20Avan%C3%A7ados%20Cap%C3%ADtulo%203%20TD.pdf)  
22. Renda fixa 102: Curva de juros \- PIMCO, acessado em setembro 11, 2025, [https://www.pimco.com/br/pt/resources/education/bonds-102-understanding-the-yield-curve](https://www.pimco.com/br/pt/resources/education/bonds-102-understanding-the-yield-curve)  
23. Curva de juros: o que é, gráfico, tipos e como afeta os investimentos ..., acessado em setembro 11, 2025, [https://cmcapital.com.br/blog/curva-de-juros/](https://cmcapital.com.br/blog/curva-de-juros/)  
24. Curva de rendimento: guia abrangente sobre yield curve | StoneX, acessado em setembro 11, 2025, [https://www.stonex.com/pt-br/glossario-financeiro/curva-de-rendimento/](https://www.stonex.com/pt-br/glossario-financeiro/curva-de-rendimento/)  
25. Como usar o Boletim Focus nos seus investimentos \- Blog Ouro ..., acessado em setembro 11, 2025, [https://www.ouropretoinvestimentos.com.br/blog/boletim-focus-nos-seus-investimentos/](https://www.ouropretoinvestimentos.com.br/blog/boletim-focus-nos-seus-investimentos/)  
26. Boletim Focus o que é Sua importância e como consultar \- Suno, acessado em setembro 11, 2025, [https://www.suno.com.br/guias/boletim-focus-o-que-e-sua-importancia-e-como-consultar/](https://www.suno.com.br/guias/boletim-focus-o-que-e-sua-importancia-e-como-consultar/)  
27. Hedge: entenda como protege a sua carteira de investimentos \- Funds Explorer, acessado em setembro 11, 2025, [https://www.fundsexplorer.com.br/artigos/hedge/](https://www.fundsexplorer.com.br/artigos/hedge/)  
28. Hedge: para reduzir os riscos dos seus investimentos \- Modalmais, acessado em setembro 11, 2025, [https://www.modalmais.com.br/blog/hedge-investimentos/](https://www.modalmais.com.br/blog/hedge-investimentos/)  
29. Nova “marcação a mercado” na renda fixa: O que muda para o investidor? \- Expert XP, acessado em setembro 11, 2025, [https://conteudos.xpi.com.br/renda-fixa/relatorios/nova-marcacao-a-mercado-na-renda-fixa-o-que-muda-para-o-investidor/](https://conteudos.xpi.com.br/renda-fixa/relatorios/nova-marcacao-a-mercado-na-renda-fixa-o-que-muda-para-o-investidor/)  
30. O que é marcação a mercado e como ela afeta os investimentos?, acessado em setembro 11, 2025, [https://blog.picpay.com/marcacao-a-mercado/](https://blog.picpay.com/marcacao-a-mercado/)  
31. Imposto de Renda no Tesouro Direto: tributação e como calcular \- Onze, acessado em setembro 11, 2025, [https://www.onze.com.br/blog/imposto-de-renda-no-tesouro-direto/](https://www.onze.com.br/blog/imposto-de-renda-no-tesouro-direto/)  
32. Qual o Imposto cobrado sobre o Tesouro Direto? \- Atendimento XP | Tire suas dúvidas, acessado em setembro 11, 2025, [https://atendimento.xpi.com.br/artigo/1409-qual-o-imposto-cobrado-sobre-o-tesouro-direto](https://atendimento.xpi.com.br/artigo/1409-qual-o-imposto-cobrado-sobre-o-tesouro-direto)  
33. Qual é a tabela regressiva da Renda Fixa? – Blog Santander, acessado em setembro 11, 2025, [https://www.santander.com.br/blog/tabela-regressiva](https://www.santander.com.br/blog/tabela-regressiva)  
34. Tributação de Investimentos Financeiros | NuInvest, acessado em setembro 11, 2025, [https://www.nuinvest.com.br/tributacao-de-renda-fixa.html](https://www.nuinvest.com.br/tributacao-de-renda-fixa.html)  
35. Custos | Toro Investimentos, acessado em setembro 11, 2025, [https://www.toroinvestimentos.com.br/info/custos](https://www.toroinvestimentos.com.br/info/custos)  
36. Regras e Regulamento \- Tesouro Direto, acessado em setembro 11, 2025, [https://www.tesourodireto.com.br/en/sobre-o-tesouro/regras-e-regulamento](https://www.tesourodireto.com.br/en/sobre-o-tesouro/regras-e-regulamento)  
37. Taxa de custódia do Tesouro Direto deixa de ser cobrada semestralmente | Agência Brasil, acessado em setembro 11, 2025, [https://agenciabrasil.ebc.com.br/economia/noticia/2024-12/taxa-de-custodia-do-tesouro-direto-deixa-de-ser-cobrada-semestralmente](https://agenciabrasil.ebc.com.br/economia/noticia/2024-12/taxa-de-custodia-do-tesouro-direto-deixa-de-ser-cobrada-semestralmente)  
38. Quais os custos do Tesouro Direto? \- Atendimento Clear | Tire suas dúvidas, acessado em setembro 11, 2025, [https://atendimento.clear.com.br/artigo/4328-quais-os-custos-do-tesouro-direto](https://atendimento.clear.com.br/artigo/4328-quais-os-custos-do-tesouro-direto)  
39. Custos Operacionais: Conheça as taxas do mercado \- XP Investimentos, acessado em setembro 11, 2025, [https://www.xpi.com.br/custos-operacionais/](https://www.xpi.com.br/custos-operacionais/)