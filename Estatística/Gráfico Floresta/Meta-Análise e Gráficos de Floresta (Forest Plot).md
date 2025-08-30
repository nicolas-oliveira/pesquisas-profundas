<img src="https://r2cdn.perplexity.ai/pplx-full-logo-primary-dark%402x.png" class="logo" width="120"/>

# Meta-Análise e Gráficos de Floresta (Forest Plot) no Jamovi: Um Guia Passo a Passo

O Jamovi é uma ferramenta estatística gratuita e de código aberto que oferece uma alternativa acessível para análises estatísticas complexas, incluindo meta-análise. Este guia apresenta como realizar meta-análises e criar gráficos de floresta (forest plots) usando o Jamovi, com foco na facilidade de uso e aplicação prática.

## O que é o Jamovi e por que usá-lo para meta-análise?

O Jamovi é um software estatístico com interface gráfica que permite pesquisadores e analistas realizarem análises estatísticas de forma intuitiva. É uma alternativa gratuita aos softwares proprietários como SPSS e SAS[^1]. Entre as opções gratuitas de programas estatísticos com interface gráfica, o Jamovi se destaca pela simplicidade de aprendizado e uso, permitindo que usuários se concentrem na análise estatística dos dados em vez de se preocuparem com aspectos técnicos do programa[^1].

Uma das grandes vantagens do Jamovi para meta-análise é sua estrutura modular, que permite adicionar funcionalidades específicas ao programa base. Para realizar meta-análises no Jamovi, utilizamos o módulo MAJOR (Meta-Analysis JamOvi R), desenvolvido por Kyle Hamilton[^6]. Este módulo funciona como um wrapper da popular biblioteca metaphor do R, mas com interface gráfica amigável[^5].

## Instalando o módulo MAJOR no Jamovi

Antes de começar a realizar meta-análises no Jamovi, você precisa instalar o módulo MAJOR. Siga estes passos:

1. Abra o Jamovi no seu computador
2. Clique no ícone "+" localizado no canto superior direito da tela[^13]
3. Escolha "Jamovi Library" no menu que se abre[^13]
4. Digite "MAJOR" na barra de pesquisa
5. Clique em "Install" ao lado do módulo MAJOR[^4]

Uma vez instalado, o módulo MAJOR estará disponível para uso no menu de análises do Jamovi.

## Entendendo a Meta-Análise no Jamovi

A meta-análise é uma técnica estatística que combina os resultados de múltiplos estudos independentes para fornecer uma estimativa mais precisa do efeito de interesse[^10]. O módulo MAJOR no Jamovi permite realizar meta-análises com diferentes tipos de dados de entrada, incluindo:

- Coeficientes de correlação
- Alfa de Cronbach
- Resultados dicotômicos
- Diferenças de médias
- Proporções
- Tamanhos de efeito e variâncias amostrais[^4]


## Realizando uma Meta-Análise no Jamovi

Para realizar uma meta-análise básica no Jamovi usando o módulo MAJOR, siga estes passos:

### 1. Preparação dos dados

Primeiro, você precisa organizar seus dados em um formato adequado para meta-análise:

1. Abra o Jamovi e crie uma nova planilha de dados
2. Configure colunas para cada estudo incluído na meta-análise
3. Insira os dados relevantes para cada estudo (dependendo do tipo de meta-análise)[^9]

Por exemplo, para uma meta-análise de diferenças médias, você precisará das seguintes informações para cada estudo:

- Nome ou identificador do estudo
- Média do grupo experimental
- Desvio padrão do grupo experimental
- Tamanho amostral do grupo experimental
- Média do grupo controle
- Desvio padrão do grupo controle
- Tamanho amostral do grupo controle[^5]


### 2. Acessando o módulo MAJOR

Com os dados prontos:

1. Clique em "Analyses" na barra superior do Jamovi
2. Procure por "MAJOR" na lista de módulos
3. Escolha o tipo apropriado de meta-análise (ex.: "Mean Differences" para diferenças de médias)[^5][^9]

### 3. Configurando a meta-análise

Na janela que se abre:

1. Selecione as variáveis correspondentes aos dados dos seus estudos
2. Escolha o modelo de efeitos (fixo ou aleatório) - o modelo de efeitos aleatórios é geralmente recomendado quando há heterogeneidade entre os estudos
3. Selecione o estimador apropriado (por padrão, o módulo usa o estimador de máxima verossimilhança restrita, mas você também pode escolher o método DerSimonian-Laird)[^5]
4. Configure outras opções conforme necessário para sua análise específica

## Criando um Gráfico de Floresta (Forest Plot) no Jamovi

O gráfico de floresta é uma representação visual essencial em meta-análise, mostrando os resultados de estudos individuais e a estimativa combinada do efeito[^10]. Para criar um forest plot no Jamovi:

### 1. Na mesma janela de configuração da meta-análise:

1. Role para baixo até encontrar as opções de gráfico (geralmente uma aba chamada "Plots")
2. Marque a opção "Forest plot"
3. Configure as opções do gráfico conforme desejado:
    - Tamanho do gráfico (é possível aumentar o tamanho para melhorar a legibilidade)
    - Configurações de eixos
    - Elementos gráficos[^9]

### 2. Personalizando seu Forest Plot:

O módulo MAJOR permite personalizar aspectos do forest plot para melhorar sua apresentação:

- Ajustar o tamanho e formato do gráfico
- Modificar as escalas dos eixos
- Alterar os elementos gráficos utilizados[^9]


## Interpretando o Forest Plot

Um forest plot típico gerado pelo Jamovi mostra:

1. **Estudos individuais**: Representados por quadrados (ou outros símbolos), cujo tamanho geralmente corresponde ao peso do estudo na meta-análise
2. **Intervalos de confiança**: Representados por linhas horizontais que se estendem a partir de cada estudo
3. **Linha de não-efeito**: Geralmente uma linha vertical no valor zero (para diferenças de médias) ou um (para razões de chances)
4. **Estimativa combinada do efeito**: Representada por um diamante na parte inferior do gráfico
5. **Heterogeneidade**: Os resultados do teste de heterogeneidade (Q, I², etc.) são geralmente apresentados junto ao gráfico[^10][^5]

O forest plot permite visualizar facilmente:

- A direção e magnitude do efeito de cada estudo
- A precisão de cada estimativa (através do intervalo de confiança)
- A consistência entre os resultados dos estudos
- O efeito combinado global e sua precisão[^10]


## Análise de Viés de Publicação

Além do forest plot, o módulo MAJOR do Jamovi também oferece ferramentas para avaliar o viés de publicação:

1. **Funnel Plot**: Um gráfico que ajuda a detectar viés de publicação
2. **Testes estatísticos**: Incluindo o teste de Egger, teste PET e o teste de assimetria de Begg[^9][^5]

Para criar um funnel plot:

1. Na mesma janela de configuração da meta-análise, procure a opção "Funnel plot"
2. Marque esta opção para incluir o gráfico na saída

## Exemplo Prático com o Dataset de Normand1999

O módulo MAJOR inclui datasets de exemplo para praticar. Um deles é o dataset Normand1999, que compara tratamentos especializados para AVC versus tratamentos de rotina[^5]:

1. Para acessar este dataset:
    - Clique em "Open"
    - Selecione "Data Library"
    - Escolha "Meta-analysis for Jamovi"
    - Selecione "Normand1999 meta-analysis of mean differences"[^5]
2. Com este dataset aberto:
    - Vá para "Analyses" > "MAJOR" > "Mean Differences"
    - O software configurará automaticamente as variáveis
    - Execute a análise e observe os resultados, incluindo o forest plot

Este exemplo mostra nove estudos comparando tratamentos especializados para AVC versus tratamento de rotina, onde o desfecho é o número de dias de hospitalização (menos dias é considerado melhor)[^5].

## Considerações Importantes para Meta-Análise no Jamovi

1. **Escolha do modelo**: O modelo de efeitos aleatórios é geralmente recomendado quando há heterogeneidade entre os estudos
2. **Sensibilidade aos estimadores**: É importante testar diferentes estimadores (como DerSimonian-Laird vs. máxima verossimilhança restrita) para verificar a robustez dos resultados[^5]
3. **Análise de sensibilidade**: Considere realizar uma análise de "leave-one-out" (remover um estudo por vez) para verificar se os resultados dependem fortemente de um único estudo[^5]
4. **Interpretação da heterogeneidade**: Utilize as estatísticas Q e I² para avaliar a heterogeneidade entre os estudos

## Conclusão

O Jamovi, com seu módulo MAJOR, oferece uma ferramenta gratuita e acessível para realizar meta-análises e criar gráficos de floresta de alta qualidade. Sua interface gráfica intuitiva torna o processo mais simples, mesmo para usuários sem experiência em programação.

Para pesquisadores e estudantes que buscam uma alternativa gratuita aos softwares estatísticos comerciais, o Jamovi representa uma excelente opção, combinando facilidade de uso com recursos estatísticos avançados. A capacidade de realizar meta-análises e criar forest plots de forma intuitiva torna o software particularmente valioso para revisões sistemáticas e sínteses de evidências.

A abordagem modular do Jamovi também significa que novas funcionalidades continuam sendo adicionadas pela comunidade, tornando-o uma ferramenta em constante evolução para análises estatísticas avançadas, incluindo meta-análise.

<div style="text-align: center">⁂</div>

[^1]: https://balaiocientifico.com/jamovi/introducao-ao-jamovi/

[^2]: https://inscricoes.ufsc.br/curso-de-ma

[^3]: https://wcea.education/portfolio-item/meta-analysis-using-jamovi-software-hands-on-experience/

[^4]: https://github.com/kylehamilton/MAJOR

[^5]: https://www.youtube.com/watch?v=wfsrB6S5cr4

[^6]: https://www.jamovi.org/news/2017/11/23/new-modules.html

[^7]: https://www.youtube.com/watch?v=qBeRMNldzgs

[^8]: https://www.scielo.br/j/rbef/a/HHCwMGhjGmS8rcDB3qrhmbg/?lang=pt

[^9]: https://www.youtube.com/watch?v=kNNS-yZ8P70

[^10]: https://www.youtube.com/watch?v=DFr6udB4o4w

[^11]: https://www.youtube.com/watch?v=JrsRZqRFpq8

[^12]: https://balaiocientifico.com/jamovi/apostila-de-jamovi/

[^13]: https://docs.jamovi.org/_pages/Install_modules.html

[^14]: https://cursosextensao.usp.br/course/view.php?id=3211

[^15]: https://www.youtube.com/watch?v=21Bbd8WLR_A

[^16]: https://www.youtube.com/watch?v=LtQ-fAWtVb4

[^17]: https://pedroejoaoeditores.com.br/produto/metanalise-na-area-do-exercicio-fisico-e-da-saude-o-que-e-como-fazer-como-redigir-como-ler-e-interpretar/

[^18]: https://www.statsmadeasy.com/jamovi-guides/the-basics/4-importing-exporting-saving-data

[^19]: https://lsj.readthedocs.io/pt/latest/

[^20]: https://www.youtube.com/watch?v=LsbrcQaaI4o

[^21]: https://www.youtube.com/watch?v=PmisCPKznw8

[^22]: https://pt.scribd.com/document/784584468/Jamovi

[^23]: https://www.jamovi.org

[^24]: https://www.reddit.com/r/AskStatistics/comments/1bt01h2/forest_plot_for_metaanalysis_in_jamovi/?tl=pt-br

[^25]: https://pedroejoaoeditores.com.br/?arquivo_download=6546

[^26]: https://www.youtube.com/watch?v=9j0li3_yNco

[^27]: https://www.youtube.com/watch?v=cN-LzCY88wg

[^28]: https://www.reddit.com/r/AskStatistics/comments/1bt01h2/forest_plot_for_metaanalysis_in_jamovi/

[^29]: https://www.youtube.com/watch?v=9YdaXNCVX7o

[^30]: https://www.youtube.com/watch?v=wZX3TPIhKTE

[^31]: https://www.futurejournal.org/FSRJ/article/download/921/557

[^32]: https://uspdigital.usp.br/janus/Disciplina?sgldis=RNP5797

[^33]: https://bdtd.ibict.br/vufind/Record/UFV_b6829ef5fa4963839e9a9c47e3550998

[^34]: https://www.youtube.com/watch?v=Ec5HhZcwoMw

[^35]: https://forum.jamovi.org/viewtopic.php?t=837

[^36]: https://docs.jamovi.org/_pages/tut_0103-creating-an-analysis.html

[^37]: https://www.jamovi.org/user-manual.html

[^38]: https://www.rensvandeschoot.com/tutorials/jamovi-for-beginners/

[^39]: https://osf.io/zm6yp/

[^40]: https://www.youtube.com/live/FKCCf5m5ShA

[^41]: https://diegoariel.com.br/post/aula-05-como-realizar-uma-metanalise-forest-plot

[^42]: https://diegoariel.com.br/post/aula-09-como-realizar-uma-metanalise-criando-o-forest-plot

