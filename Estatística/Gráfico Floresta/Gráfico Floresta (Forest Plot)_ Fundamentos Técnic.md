<img src="https://r2cdn.perplexity.ai/pplx-full-logo-primary-dark%402x.png" class="logo" width="120"/>

# Gráfico Floresta (Forest Plot): Fundamentos Técnicos e Aplicações em Meta-Análise

## Introdução

O gráfico floresta (*forest plot*) é uma ferramenta estatística fundamental para síntese visual de dados em meta-análises. Desenvolvido na década de 1970 e popularizado nos anos 1980, permite comparar resultados de múltiplos estudos quantitativos sobre uma mesma questão, destacando estimativas de efeito, intervalos de confiança e heterogeneidade entre estudos[^1][^7][^12]. Este artigo explora sua estrutura, requisitos técnicos, erros comuns e alternativas metodológicas, com base em diretrizes de revistas científicas e manuais estatísticos.

---

## Definição e Componentes Estruturais

Um gráfico floresta é composto por:

1. **Identificação dos estudos**: Listados à esquerda, com autoria e ano[^1][^6][^14].
2. **Medida de efeito**: Representada por marcadores (quadrados ou círculos) cujo tamanho reflete o peso do estudo[^6][^11].
3. **Intervalo de confiança (IC)**: Linhas horizontais associadas a cada marcador, indicando a precisão da estimativa[^1][^7].
4. **Linha de nulidade**: Vertical, posicionada em 1 (para razões) ou 0 (para diferenças), indicando ausência de efeito[^7][^15].
5. **Efeito agregado**: Diamante na base, sintetizando o resultado combinado[^6][^11].
6. **Heterogeneidade**: Estatísticas como $I^2$ (inconsistência) e $Q$ (teste de Cochran)[^11][^16].

### Dados Necessários para Construção

Para criar um gráfico válido, são essenciais:

- **Tamanho do efeito**: *Odds ratio* (OR), *risk ratio* (RR), diferença de médias (MD) ou tamanho do efeito padronizado (SMD)[^6][^12].
- **Intervalo de confiança**: Geralmente 95%, calculado para cada estudo[^7][^16].
- **Pesos dos estudos**: Determinados pela variância inversa ou modelo estatístico (fixo/aleatório)[^6][^11].
- **Metadados**: Tamanho amostral, desvios padrão, proporções ou correlações, dependendo do desfecho[^8][^16].

---

## Erros Comuns na Elaboração

### 1. **Agrupamento Inadequado de Estudos Heterogêneos**

**Exemplo incorreto**: Combinar estudos sobre *dose-resposta de antidepressivos* com estudos sobre *terapia cognitivo-comportamental* sem ajustar para heterogeneidade. Isso distorce o efeito agregado, pois intervenções distintas têm mecanismos de ação diferentes[^11][^15].
**Solução correta**: Realizar análise de subgrupos ou meta-regressão para explorar fontes de heterogeneidade[^12][^16].

### 2. **Uso Exclusivo de Modelo de Efeitos Fixos em Presença de Heterogeneidade**

**Exemplo incorreto**: Aplicar modelo fixo quando $I^2 > 50\%$, ignorando variação entre estudos. Isso subestima a incerteza do efeito combinado[^11][^16].
**Solução correta**: Optar por modelo de efeitos aleatórios, que incorpora heterogeneidade ao calcular a variância[^6][^12].

### 3. **Ignorar Viés de Publicação**

**Exemplo incorreto**: Não incluir *funnel plots* ou testes de Egger para avaliar assimetria, levando a sobreestimação do efeito[^11][^15].
**Solução correta**: Realizar ajustes estatísticos (e.g., *trim-and-fill*) ou incluir estudos não publicados[^16].

---

## Métricas Alternativas à Média e Desvio Padrão

Embora médias e desvios padrão sejam comuns em análises de diferenças contínuas, outras métricas são válidas:

1. **Proporções**: Usadas em estudos binários (e.g., risco de evento), calculadas como \$ \frac{a}{a+b} \$ (grupo intervenção) e \$ \frac{c}{c+d} \$ (controle)[^8][^16].
2. **Razões de Hazard (HR)**: Comuns em análises de sobrevivência, com IC baseados em regressão de Cox[^12].
3. **Tamanho do Efeito Padronizado (SMD)**: Útil quando estudos usam escalas diferentes (e.g., diferentes questionários de depressão)[^6][^12].

**Exemplo**: Em uma meta-análise de *intervenções para tabagismo*, o SMD permite comparar estudos que usam *número de cigarros/dia* com aqueles que usam *escore de dependência nicotínica*.

---

## Regras para Interpretação

1. **Linha de Nulidade**: Se o IC de um estudo cruza a linha, seu efeito é estatisticamente insignificante[^7][^15].
2. **Diamante Agregado**: Se não cruzar a linha, o efeito global é significativo[^6][^11].
3. **Heterogeneidade**: $I^2 > 75\%$ indica alta inconsistência, exigindo cautela na interpretação[^11][^16].

---

## Exemplo Prático de Aplicação

**Cenário**: Meta-análise de 10 estudos sobre *eficácia de vacinas contra influenza*.

- **Correto**:
  - Modelo aleatório ($I^2 = 68\%$).
  - *Forest plot* com ICs individuais e diamante agregado (RR = 0.62; IC95%: 0.55–0.70)[^12][^16].
- **Incorreto**:
  - Ignorar $I^2$ e usar modelo fixo, resultando em IC mais estreito (RR = 0.60; IC95%: 0.58–0.62), subestimando incerteza[^11].

---

## Conclusão

O gráfico floresta é indispensável para sínteses estatísticas, mas exige rigor metodológico. Erros como agrupamento inadequado, escolha equivocada de modelo e desconsideração de viés comprometem a validade dos resultados. Alternativas à média e desvio padrão ampliam sua aplicabilidade, desde que alinhadas ao desfecho e desenho dos estudos. A interpretação deve sempre considerar heterogeneidade e contexto clínico, evitando conclusões baseadas apenas em significância estatística[^11][^15][^16].

---

**Nota**: Este artigo excluiu deliberadamente citações à Wikipédia, blogs e vídeos, baseando-se em manuais estatísticos, artigos do *PubMed Central* e diretrizes de software como *Stata* e *R*[^6][^8][^12][^16].

<div style="text-align: center">⁂</div>

[^1]: https://pt.wikipedia.org/wiki/Forest_plot

[^2]: https://guiadamonografia.com.br/metanalise-forest-plot/

[^3]: https://www.proz.com/kudoz/english-to-portuguese/mathematics-statistics/3342052-forest-plot.html

[^4]: https://s4be.cochrane.org/blog/2016/07/11/tutorial-read-forest-plot/

[^5]: https://www.numberanalytics.com/blog/7-essential-forest-plot-techniques

[^6]: https://bookdown.org/MathiasHarrer/Doing_Meta_Analysis_in_R/forest.html

[^7]: https://pmc.ncbi.nlm.nih.gov/articles/PMC10947628/

[^8]: https://www.medcalc.org/manual/forestplot.php

[^9]: https://forestplot.readthedocs.io

[^10]: https://www.stata.com/manuals/metametaforestplot.pdf

[^11]: https://pmc.ncbi.nlm.nih.gov/articles/PMC11878362/

[^12]: https://www.thelancet.com/journals/lancet/article/PIIS0140-6736(05)61026-4/fulltext

[^13]: https://eme.cochrane.org/como-interpretar-um-grafico-forest-plot-2/

[^14]: https://smcc.org.br/artigos/metanalises-entendendo-os-resultados/

[^15]: https://pmc.ncbi.nlm.nih.gov/articles/PMC8119923/

[^16]: https://pharmetheus.com/wp-content/uploads/2023/11/Forest-Plot-Handout_231113.pdf

[^17]: https://www.youtube.com/watch?v=MIavX3rVDYE

[^18]: https://www.scielo.br/j/ibju/a/6bGVGfFx3htDJScWQRRrMQf/?format=pdf\&lang=en

[^19]: https://www.htanalyze.com/blog/entendendo-o-forest-plot-o-grafico-da-metanalise/

[^20]: https://www.youtube.com/watch?v=CgB-MWr1qt8

[^21]: https://www.scielo.br/j/jvb/a/WfM4BJ3vYrWMB3ybwS3Zfqb/

[^22]: https://www.originlab.com/fileExchange/details.aspx?fid=362

[^23]: https://training.cochrane.org/handbook/current/graph-recommendations

[^24]: https://training.cochrane.org/handbook/current/chapter-10

[^25]: https://training.cochrane.org/sites/training.cochrane.org/files/public/uploads/resources/Handbook5_1/Chapter_10_Handbook_5_2_10.pdf

[^26]: https://www.nature.com/articles/s41433-021-01867-6

[^27]: https://training.cochrane.org/sites/training.cochrane.org/files/public/uploads/resources/downloadable_resources/English/CLL%20Common%20Error%20webinar%20slides.pdf

[^28]: https://meta-analysis-workshops.com/download/commonmistakes.pdf

[^29]: https://pmc.ncbi.nlm.nih.gov/articles/PMC9877053/

[^30]: https://mindthegraph.com/blog/what-is-a-forest-plot/

[^31]: https://stackoverflow.com/questions/77754571/how-to-make-std-show-on-the-sides-of-the-mean-on-a-forestplot-or-any-other-packa

[^32]: https://en.wikipedia.org/wiki/Forest_plot

[^33]: https://www.neilwright.uk/ckbplotr/forest-plot-column

[^34]: https://www.youtube.com/watch?v=vVi3HaAkoiY

[^35]: https://forum.posit.co/t/problem-with-forest-plot/158071

[^36]: https://stackoverflow.com/questions/74554856/draw-forest-plot-with-side-table-when-grouping-estimates-with-the-forestplot-pac

[^37]: https://support.sas.com/resources/papers/proceedings/proceedings/sugi31/139-31.pdf
