<img src="https://r2cdn.perplexity.ai/pplx-full-logo-primary-dark%402x.png" class="logo" width="120"/>

# Gráfico Floresta (Forest Plot): Definição, História e Protocolos para Construção Correta

## Resumo Executivo

O gráfico floresta é uma ferramenta visual essencial em meta-análises, permitindo a síntese de resultados de múltiplos estudos em uma única imagem[^3][^10][^12]. Desenvolvido na década de 1970, consolidou-se como padrão para representar efeitos combinados e heterogeneidade entre estudos[^2][^11]. Sua construção exige dados estruturados (como tamanhos de efeito, intervalos de confiança e pesos amostrais) e atenção a critérios estatísticos para evitar distorções[^7][^8][^19]. Este artigo detalha sua anatomia, requisitos técnicos, erros comuns e alternativas às métricas tradicionais, baseando-se em fontes acadêmicas e documentações técnicas do Jamovi e R[^4][^5][^9][^15].

---

## Definição e Histórico

### O Que É um Gráfico Floresta?

Um gráfico floresta é uma representação gráfica que combina resultados individuais de estudos científicos e sua síntese estatística em meta-análises[^3][^12]. Cada estudo é representado por:

- **Ponto estimado**: Geralmente um quadrado ou círculo, cujo tamanho reflete o peso do estudo na análise[^1][^10].
- **Intervalo de confiança (IC)**: Uma linha horizontal que atravessa o ponto, indicando a precisão da estimativa[^12][^17].
- **Linha de não efeito**: Uma linha vertical (ex.: valor 1 para razões de chances ou 0 para diferenças de médias), servindo como referência para significância estatística[^3][^19].

No final do gráfico, um diamante ilustra o efeito combinado de todos os estudos, com sua largura representando o IC da meta-análise[^10][^12].

### Origem e Evolução

A primeira versão moderna surgiu em 1982, quando Lewis e Ellis integraram resultados combinados abaixo de estudos individuais[^2]. O termo "floresta" (forest plot) deriva da aparência densa das linhas horizontais, embora mitos sugiram homenagem à pesquisadora Pat Forrest[^2][^11]. Sua popularização ocorreu com a expansão da Cochrane Collaboration na década de 1990, tornando-o padrão em revisões sistemáticas[^2][^12].

---

## Estrutura e Componentes Obligatórios

### Colunas e Elementos Gráficos

1. **Identificação dos estudos** (lado esquerdo):
    - Autores, ano de publicação e/ou identificadores únicos[^3][^10].
    - Em meta-análises com subgrupos, categorias são destacadas (ex.: "Estudos com placebo vs. intervenção")[^8][^12].
2. **Medidas de efeito** (lado direito):
    - **Escala numérica**: Eixo horizontal com a métrica estatística (ex.: *odds ratio*, diferença de médias padronizada)[^3][^17].
    - **Marcadores visuais**:
        - Quadrados proporcional ao peso do estudo[^1][^10].
        - Linhas horizontais representando ICs de 95%[^12][^19].
        - Diamante para o efeito combinado[^3][^10].
3. **Estatísticas de heterogeneidade**:
    - Índices como *I²* (proporção de variação não aleatória) e *Q* (teste de heterogeneidade)[^7][^19].

### Dados Necessários para Construção

| Tipo de Dado | Exemplo de Variáveis | Fonte |
| :-- | :-- | :-- |
| **Estudos individuais** | Tamanho do efeito (ex.: *d* de Cohen), IC 95% | [^3][^10][^12] |
| **Metanálise** | Efeito combinado, modelo (fixo/aleatório) | [^9][^15][^19] |
| **Contexto** | Nomes dos estudos, ano, tamanho amostral | [^8][^12] |

**Exemplo Correto de Estruturação**:

```  
Estudo       | Tamanho do Efeito | IC Inferior | IC Superior | Peso  
-----------|-------------------|-------------|-------------|-----  
Smith 2020 | 0.75              | 0.60        | 0.90        | 30%  
Jones 2021 | 1.10              | 0.85        | 1.35        | 25%  
```


---

## Regras para Construção Adequada

### Passos Técnicos

1. **Preparação dos Dados**:
    - Calcular tamanhos de efeito padronizados (ex.: diferença de médias, *odds ratio*) para cada estudo[^3][^10].
    - **Erro comum**: Usar medianas e intervalos interquartílicos em vez de médias e desvios-padrão para dados contínuos[^7][^8].
        - *Exemplo Incorreto*:

```  
Estudo       | Mediana | Q1-Q3  
-----------|---------|-------  
Silva 2022 | 12      | 10-14  
```

        - *Exemplo Correto*:

```  
Estudo       | Média | DP   | N  
-----------|-------|------|---  
Silva 2022 | 12.3  | 2.1  | 50  
```

2. **Escolha do Modelo Estatístico**:
    - **Modelo de efeitos aleatórios**: Adequado quando há heterogeneidade significativa (*I² > 50%*)[^7][^19].
    - **Modelo de efeitos fixos**: Aplicável apenas se estudos forem homogêneos[^3][^15].
3. **Software e Ferramentas**:
    - **Jamovi + MAJOR**: Módulo gratuito para meta-análises, com opções automatizadas para forest plots[^9][^15].
    - **R + metafor**: Pacote avançado para personalização de gráficos e cálculos complexos[^4][^5].

---

## Erros Frequentes e Como Evitá-los

### 1. Agrupamento Incorreto de Dados

- **Cenário Errado**: Combinar estudos com desfechos diferentes (ex.: mortalidade e qualidade de vida) sem ajustar métricas[^8][^12].
- **Solução**: Usar medidas padronizadas (ex.: *SMD* - diferença de médias padronizada) para harmonizar escalas[^3][^19].


### 2. Interpretação Equivocada da Linha de Não Efeito

- **Erro**: Assumir que estudos cujo IC cruza a linha são "negativos". Na realidade, indicam ausência de significância estatística, não necessariamente ausência de efeito[^10][^12].


### 3. Uso Inapropriado de Medidas de Tendência Central

- **Média vs. Mediana**:
    - **Certo**: Médias e DP para dados normais[^3][^10].
    - **Errado**: Medianas e intervalos interquartílicos em meta-análises de diferenças de médias[^7][^8].


### 4. Ignorar Heterogeneidade

- **Exemplo Incorreto**: Usar modelo de efeitos fixos quando *I² = 80%*, levando a intervalo de confiança artificialmente estreito[^7][^19].

---

## Alternativas à Média, Mediana e Desvio Padrão

### Quando Usar Outras Métricas

| Tipo de Dado | Medida Adequada | Exemplo de Aplicação |
| :-- | :-- | :-- |
| **Dados Binários** | *Odds Ratio*, Risco Relativo | Ensaios clínicos com placebo |
| **Sobrevivência** | Hazard Ratio | Estudos de coorte longitudinal |
| **Proporções** | Razão de Proporções | Prevalência de doenças |

**Exemplo no Jamovi**:
Ao analisar dados binários (ex.: morte vs. sobrevivência), o módulo MAJOR calcula automaticamente *odds ratios* e ICs, sem necessidade de média/DP[^9][^15].

---

## Conclusão

O gráfico floresta é indispensável para sínteses estatísticas transparentes, mas exige rigor metodológico. Dados estruturados (tamanhos de efeito, ICs), escolha adequada de modelos e ferramentas como Jamovi/MAJOR ou R/metafor são essenciais[^9][^15][^19]. Evitar erros como agrupamento inadequado ou uso de medianas em vez de médias garante resultados confiáveis. Alternativas às métricas tradicionais ampliam sua aplicabilidade, desde que alinhadas ao tipo de dado e pergunta de pesquisa[^3][^8][^12].

<div style="text-align: center">⁂</div>

[^1]: https://en.wikipedia.org/wiki/Forest_plot

[^2]: https://pmc.ncbi.nlm.nih.gov/articles/PMC1120528/

[^3]: https://pmc.ncbi.nlm.nih.gov/articles/PMC8119923/

[^4]: https://www.metafor-project.org/doku.php/plots:forest_plot

[^5]: https://search.r-project.org/CRAN/refmans/meta/html/forest.meta.html

[^6]: https://www.reddit.com/r/AskStatistics/comments/1bt01h2/forest_plot_for_metaanalysis_in_jamovi/

[^7]: https://pmc.ncbi.nlm.nih.gov/articles/PMC2432114/

[^8]: https://pharmetheus.com/wp-content/uploads/2023/11/Forest-Plot-Handout_231113.pdf

[^9]: https://github.com/kylehamilton/MAJOR

[^10]: https://s4be.cochrane.org/blog/2016/07/11/tutorial-read-forest-plot/

[^11]: https://www.jdentalpanacea.org/html-article/13952

[^12]: https://www.aapd.org/link/e81a282e618142ab9207a6f233ef8b00.aspx

[^13]: https://wviechtb.github.io/metafor/reference/forest.default.html

[^14]: https://rdrr.io/cran/meta/src/R/forest.R

[^15]: https://forum.jamovi.org/viewtopic.php?t=2286

[^16]: https://journals.lww.com/md-journal/fulltext/2022/07080/using_the_forest_plot_to_compare_citation.30.aspx

[^17]: https://mindthegraph.com/blog/what-is-a-forest-plot/

[^18]: https://stackoverflow.com/questions/73939260/forest-plot-using-metafor-in-r-to-remove-overall-estimate-and-wider-scale

[^19]: https://pmc.ncbi.nlm.nih.gov/articles/PMC11878362/

[^20]: https://rdrr.io/cran/metafor/man/forest.html

[^21]: https://www.sciencedirect.com/topics/medicine-and-dentistry/forest-plot

[^22]: https://pt.wikipedia.org/wiki/Forest_plot

[^23]: https://wviechtb.github.io/metafor/reference/forest.html

[^24]: https://www.metafor-project.org/doku.php/plots

[^25]: https://europepmc.org/article/pmc/pmc3296675

[^26]: https://europepmc.org/article/med/33027562

[^27]: https://www.jeehp.org/journal/view.php?number=488

[^28]: https://www.youtube.com/watch?v=Pxs0gl3hRKE

[^29]: http://exercmed.org/journal/view.php?doi=10.26644%2Fem.2017.005

[^30]: https://cjvanlissa.github.io/Doing-Meta-Analysis-in-R/saving-the-forest-plot.html

[^31]: https://docs.jamovi.org

[^32]: https://forum.jamovi.org/viewtopic.php?t=314

[^33]: https://www.jamovi.org/user-manual.html

[^34]: https://www.jamovi.org/news/2017/11/23/new-modules.html

[^35]: https://www.statsmadeasy.com/jamovi-guides

[^36]: https://www.youtube.com/watch?v=wfsrB6S5cr4

[^37]: https://jamovi.readthedocs.io/ar/latest/jmv/jmvReadWrite_read_omv/

[^38]: https://scispace.com/pdf/jamovi-an-easy-to-use-statistical-software-for-the-social-3n3j7xk54t.pdf

[^39]: https://gamlj.github.io

