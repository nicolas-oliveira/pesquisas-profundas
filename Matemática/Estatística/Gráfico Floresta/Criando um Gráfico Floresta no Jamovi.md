# Criando um Gráfico Floresta no Jamovi

## O que é Meta-análise?

A **meta-análise** é um **processo estatístico** que **combina os resultados obtidos em dois ou mais estudos** para gerar uma **síntese** mais robusta, aumentando o **tamanho da amostra** e, assim, produzindo uma **estimativa precisa do tamanho do efeito**. Ao **analisar dados de estudos já realizados**, esse método calcula médias ponderadas dos efeitos individuais e seus **intervalos de confiança**, permitindo avaliar a **variabilidade** entre os resultados e identificar padrões comuns ([Himmelfarb Library Guides](https://guides.himmelfarb.gwu.edu/studydesign101/metaanalysis?utm_source=chatgpt.com "Study Design 101: Meta-Analysis - Research Guides")).

Para executar uma meta-análise, é necessário ter pelo menos dois estudos com **metodologias quantitativas semelhantes** e todas as **medidas necessárias para o cálculo** disponíveis, embora nem toda revisão sistemática inclua meta-análise ([Eventos Estatísticos](https://events.stat.uconn.edu/metapack/intro-to-meta-analysis/?utm_source=chatgpt.com "Introduction to Meta-analysis")). Os resultados são apresentados em um **gráfico de floresta (forest plot)**, onde cada estudo é mostrado com um quadrado (tamanho do efeito) e uma linha (intervalo de confiança), e a síntese geral aparece como um diamante ([Students 4 Best Evidence](https://s4be.cochrane.org/blog/2016/07/11/tutorial-read-forest-plot/?utm_source=chatgpt.com "Tutorial: How to read a forest plot - Students 4 Best Evidence")). 

A **heterogeneidade** é quantificada pelo **I-quadrado**, que indica a proporção da variabilidade atribuída a diferenças reais entre estudos (em vez de erro amostral) e orienta na escolha entre **modelo de efeito fixo** ou **modelo de efeito randômico**; em caso de alta heterogeneidade, recomenda-se também realizar **análises de subgrupos** e **análise de sensibilidade** para explicar discrepâncias ([pmc.ncbi.nlm.nih.gov](https://pmc.ncbi.nlm.nih.gov/articles/PMC3405079/?utm_source=chatgpt.com "Evolution of Heterogeneity (I2) Estimates and Their 95% Confidence ...")).

## O que é necessário para fazer um gráfico floresta?

O Gráfico Floresta exige o **número de participantes**, a **média** e o **desvio-padrão** porque como dito anteriormente a meta-análise é a combinação de dois ou mais estudos com amostragens semelhantes. 

Por isso,  cada estudo vai calcular o **tamanho do efeito** individual, pois a diferença de médias ou o desvio-padrão padronizado (como Cohen’s d ou Hedges’ g) depende diretamente desses parâmetros estatísticos. 

Esses valores também definem a **precisão** de cada estimativa, uma vez que amostras maiores (N) e desvios-padrão menores geram **intervalos de confiança** mais estreitos, refletidos como linhas horizontais menores em cada estudo no **forest plot**. 

# O que é Jamovi?

A **jamovi** é um software de **código aberto** que serve como uma **interface gráfica** (front-end) para a linguagem **R**, oferecendo uma **planilha** à esquerda para inserção ou importação de **dados** (CSV, Excel, SPSS, R, Stata, SAS) e, à direita, a exibição imediata dos **resultados das análises estatísticas**.

Com módulos adicionais, como o “major” para **meta-análise**, e suporte a **Estatísticas Descritivas**, **Testes t**, **ANOVA**, **Regressão Linear e Logística**, o jamovi gera **tabelas formatadas** (estilo APA) e **gráficos atrativos**, além de expor o **R Syntax Mode** e o **Rj Editor** para quem deseja ver ou executar diretamente o código R subjacente.

O fato de ser **Open Source** torna o jamovi **100% gratuito** e **acessível** a estudantes e pesquisadores, eliminando barreiras financeiras e permitindo que toda a comunidade contribua para seu desenvolvimento, compartilhe novos módulos e garanta que a plataforma evolua continuamente.

# Como criar um Gráfico Floresta?

Para criar um gráfico floresta (forest plot) no jamovi para uma metanálise, siga estes passos baseados nas informações fornecidas:

## 1. Instale o software **jamovi**.

[jamovi desktop - jamovi](https://www.jamovi.org/download.html)

## 2. Prepare seus dados em uma planilha.

Para dados de metanálise, você precisará de colunas para o nome de cada estudo e, para cada estudo, dados separados para o grupo experimental/tratamento e o grupo controle. Dependendo do tipo de variável de resultado.

- Para variáveis contínuas, inclua o **número de participantes**, a **média** e o **desvio padrão** para cada grupo. ***Exemplo**: Sejam  $x_1,x_2,...,x_n$ os valores observados em um conjunto de dados com $N$ participantes.*

- **Número de participantes**:
  
  $$
  N = \sum_{i=1}^{N} 1
  $$
  
  **Média aritmética**:
  
  $$
  \bar{x} = \frac{1}{N} \sum_{i=1}^{N} x_i
  $$
  
  **Desvio-padrão amostral**:
  
  $$
  s = \sqrt{\frac{1}{N-1} \sum_{i=1}^{N} \bigl(x_i - \bar{x}\bigr)^2}
  $$
  
  - Para variáveis dicotômicas (binárias), inclua o **número de eventos** e o **número total de participantes** para cada grupo.
  
  Abra ou insira seus dados no jamovi. Certifique-se de que as variáveis numéricas (como contagens, médias, desvios padrão) estejam definidas com o tipo de medida **Continuous**. O nome do estudo pode ser definido como **Nominal** ou **Text**.
  
  <img src="file:///home/nicolas/Imagens/Capturas%20de%20tela/Captura%20de%20tela%20de%202025-05-20%2015-19-24.png" title="" alt="Captura de tela de 2025-05-20 15-19-24.png" data-align="center">

## 2.1 Para calcular média e desvio-padrão no jamovi

---

### 2.1.2. Importe os dados para o jamovi

1. Abra o jamovi e selecione **Arquivo ▸ Abrir** para carregar sua planilha (.csv, .xlsx, .sav etc.).

2. Verifique na planilha interna se todas as variáveis numéricas foram reconhecidas corretamente (sem símbolos ou texto em colunas numéricas).

---

### 2.1.3. Acesse a estatística descritiva

- No menu superior, clique em **Exploração**.

- Em seguida, selecione **Estatística Descritiva**.

<img title="" src="file:///home/nicolas/.config/marktext/images/2025-05-20-16-31-32-image.png" alt="" data-align="center" width="175">

---

### 2.1.4. Selecione as variáveis de interesse

1. No painel esquerdo (“Variáveis”), marque as colunas que contêm os dados que deseja analisar.

<img title="" src="file:///home/nicolas/.config/marktext/images/2025-05-20-16-33-01-image.png" alt="" width="441" data-align="center">

<img src="file:///home/nicolas/.config/marktext/images/2025-05-20-16-34-16-image.png" title="" alt="" data-align="center">

---

### 2.1.5. Escolha as estatísticas a serem exibidas

1. No painel direito, expanda a seção **Estatísticas**.

2. Marque **Média (Mean)** e **Desvio-padrão (Standard Deviation)**.

3. Caso queira o desvio-padrão populacional em vez do amostral, clique no ícone de engrenagem ao lado de “Standard Deviation” e altere a opção conforme necessário.

<img title="" src="file:///home/nicolas/.config/marktext/images/2025-05-20-16-35-53-image.png" alt="" data-align="center" width="422">

---

### 2.1.6. Visualize e interprete os resultados

1. Os valores de média e desvio-padrão aparecerão imediatamente na tabela de saída à direita.

2. Cada linha corresponderá a uma variável, exibindo **Mean** e **SD** (desvio-padrão) lado a lado.

---

### 2.1.7. Exporte ou copie os resultados

1. Para copiar a tabela, clique com o botão direito sobre ela e escolha **Copiar**.

2. Você também pode exportar todo o output com **Arquivo ▸ Exportar**, selecionando o formato desejado (HTML, PDF, DOCX).

---

**Dica final**: utilize o menu **Visualize** na mesma aba de Estatística Descritiva para gerar gráficos com barras de erro (%) ou desvio-padrão, facilitando a apresentação dos resultados em relatórios ou apresentações.

## 3. Mudando as Colunas para os dados corretos

Para fazer o gráfico floresta é necessário que os dados estejam corretamente adequados para o gráfico; Para fazer a modificação da variável de "nominal" para "Decimal" basta clicar na coluna duas vezes.![Captura de tela de 2025-05-20 15-36-59.png](/home/nicolas/Imagens/Capturas%20de%20tela/Captura%20de%20tela%20de%202025-05-20%2015-36-59.png)

## 4. Instale o módulo de metanálise, se ainda não o tiver.

Clique no botão **Modules** (ou **+ Modules**) no topo da tela. Na **jamovi library**, encontre o módulo chamado **major** (ou **meta-analysis**) e clique em **Install**.

<img title="" src="file:///home/nicolas/Imagens/Capturas%20de%20tela/Captura%20de%20tela%20de%202025-05-20%2015-20-09.png" alt="Captura de tela de 2025-05-20 15-20-09.png" data-align="center" width="235">

<img title="" src="file:///home/nicolas/Imagens/Capturas%20de%20tela/Captura%20de%20tela%20de%202025-05-20%2015-21-24.png" alt="Captura de tela de 2025-05-20 15-21-24.png" data-align="center" width="499">

Após a instalação, o botão **major** aparecerá na barra de análises. Clique neste botão para acessar as opções de metanálise.

<img title="" src="file:///home/nicolas/Imagens/Capturas%20de%20tela/Captura%20de%20tela%20de%202025-05-20%2015-22-26.png" alt="Captura de tela de 2025-05-20 15-22-26.png" width="482" data-align="center">

## 5. Selecionar a Análise

Dentro do menu do módulo major, selecione o tipo de análise apropriado para os seus dados. Por exemplo, escolha **Dichotomous** para resultados binários (como número de eventos) ou **Mean differences** para dados contínuos (com médias e desvios padrão).

<img src="file:///home/nicolas/Imagens/Capturas%20de%20tela/Captura%20de%20tela%20de%202025-05-20%2015-23-22.png" title="" alt="Captura de tela de 2025-05-20 15-23-22.png" data-align="center">

Uma janela de análise será aberta. Arraste e solte as variáveis das colunas da sua planilha para as caixas correspondentes na janela de análise. Atribua o nome do estudo à caixa **Study label**. Atribua os dados de cada grupo (por exemplo, Sample Size, Mean, Standard Deviation, Incidents, Total) às caixas do **Group 1** (grupo experimental/tratamento) e **Group 2** (grupo controle), conforme o tipo de análise selecionado.

![Captura de tela de 2025-05-20 15-24-00.png](/home/nicolas/Imagens/Capturas%20de%20tela/Captura%20de%20tela%20de%202025-05-20%2015-24-00.png)

Ao atribuir as variáveis, o jamovi processará automaticamente os dados e exibirá os resultados no painel à direita. Os resultados incluirão o **Forest plot**.

Role para baixo na área de resultados à direita para visualizar o **Forest plot** gerado. Este gráfico ilustrará os resultados individuais de cada estudo (geralmente representados por quadrados e seus intervalos de confiança) e o resultado combinado da metanálise (geralmente representado por um diamante).

![Captura de tela de 2025-05-20 15-43-53.png](/home/nicolas/Imagens/Capturas%20de%20tela/Captura%20de%20tela%20de%202025-05-20%2015-43-53.png)

![Captura de tela de 2025-05-20 15-43-59.png](/home/nicolas/Imagens/Capturas%20de%20tela/Captura%20de%20tela%20de%202025-05-20%2015-43-59.png)

![Captura de tela de 2025-05-20 15-44-04.png](/home/nicolas/Imagens/Capturas%20de%20tela/Captura%20de%20tela%20de%202025-05-20%2015-44-04.png)

# Conclusão

O gráfico floresta é uma representação visual padrão dos resultados de uma metanálise. O jamovi o gera automaticamente uma vez que os dados e o tipo de análise são configurados corretamente no módulo **major**.
