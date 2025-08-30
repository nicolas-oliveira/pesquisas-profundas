# Perplexity: Identificação de Texto Gerado por IA e Humanização: Fundamentação Teórica e embasamento matemático

Desde sua expansão com modelos como GPT-3/4 e LLaMA, a geração automática de texto baseia-se em distribuições de probabilidade sobre o vocabulário, onde cada token $w$ é amostrado conforme

$$
P(w \mid \mathbf{c}) = \frac{\exp (z_w)}{\sum_{v} \exp(z_v)},
$$

com $\mathbf{c}$ representando o contexto e $z_w$ o logit do token $w$. A **entropia** dessa distribuição, definida por

$$
H = -\sum_{w} P(w \mid \mathbf{c}) \log P(w \mid \mathbf{c}),
$$

é usada para quantificar a aleatoriedade do modelo e distingue-lo de padrões humanos, cuja entropia tende a ser maior e mais variável. A **perplexidade** ($\mathrm{PPL}$) de um texto de comprimento $N$ com probabilidade $P(x_{1:N})$ é

$$
\mathrm{PPL} = \exp\Bigl(-\tfrac{1}{N}\sum_{i=1}^N \log P(x_i \mid x_{<i})\Bigr),
$$

e sua **curva de perplexidade** (variação de PPL por sentença) evidencia esta diferença de entropia entre humano e IA, servindo como ferramenta de detecção inicial (perplexity curve) (Kirchenbauer et al., 2023).[^1]

O problema de distinguir texto humano de IA é formulado como uma **classificação binária** (Humano vs. IA), enfrentando desafios como a baixa taxa de falsos positivos em gêneros formais ou escritos por não-nativos, onde padrões estatísticos podem imitar saídas de LLMs.

# 2. Metodologias de Detecção

## 2.1 Watermarking Estatístico

Kirchenbauer et al. (2023) propuseram um watermarking suave para LLMs: a cada passo de amostragem, um gerador pseudo-aleatório seleciona uma lista “verde” de tokens e aumenta seu logit em $\delta$, de modo a elevar a fração esperada $\gamma$ de tokens verdes. A detecção é feita via teste $z$ sobre a contagem observada $g$ de tokens verdes em $L$ tokens:

$$
z = \frac{g - \gamma L}{\sqrt{\gamma(1-\gamma)L}}.
$$

Se $z$ excede um limiar, rejeita-se $H_0$ de ausência de watermark.[^2][^1]

## 2.2 Perplexidade e Burstiness

Ferramentas como GPTZero baseiam-se em estatísticas de linguagem:

- **Perplexidade média** por sentença, comparando $E_{\mathrm{perp}} = H_{\mathrm{human}} - H_{\mathrm{AI}}\ge0$.
- **Burstiness**, medida da variação de perplexidade entre sentenças, cuja entropia associada

$$
E_{\mathrm{burst}} = \log p\bigl(\sum_k|s_k - s_{k+1}|\bigr)_{\mathrm{AI}} - \log p(\dots)_{\mathrm{human}} \ge0
$$

não se mostrou robusta para LLMs maiores (Mitchell et al., 2023).[^3]

## 2.3 Classificadores Fine-Tuned

Classificadores baseados em RoBERTa fine-tuned em corpora humano vs. IA usam features estilométricas (profundidade sintática, escolha lexical, métrica de embedding). Estudos recentes reportam acurácia próxima a 95% no conjunto de treino, mas fraca generalização a domínios não vistos (Wang et al., 2024).[^4]

# 3. Técnicas de Humanização e Ofuscação

## 3.1 Paraphrase Profundo

**Mecanismos de humanização** empregam *deep paraphrasing* como técnica central para reescrever sintaticamente o texto preservando  sua semântica, reduzindo a perplexidade a níveis compatíveis com a escrita humana. Esta abordagem fundamenta-se na teoria da **equivalência semântica por entailment mútuo**:  duas sentenças são consideradas semanticamente equivalentes quando cada uma implica logicamente na outra, mantendo suas condições de verdade .[(Dagan et al., 2013)](https://aclanthology.org/2023.findings-acl.179.pdf)

A paráfrase profunda opera através de múltiplas transformações linguísticas coordenadas: **substituição lexical controlada** por sinônimos contextuais que preservam a coerência semântica mediante thresholds de similaridade cossenoidal (tipicamente ≥0.85), **transformações sintáticas** que alteram a estrutura arbórea das sentenças sem modificar relações semânticas subjacentes, e **reorganização discursiva** que modifica a ordem de apresentação de informações mantendo a lógica argumentativa. Estudos demonstram que quando aplicadas sistematicamente,  essas transformações reduzem a perplexidade média de textos gerados por IA de ~15-20 para ~8-12, aproximando-se dos valores típicos da escrita humana (5-10). [(Burkett et al, 2012)](https://aclanthology.org/D12-1079.pdfhttps://aclanthology.org/D12-1079.pdf) [(Berant et al, 2014)](https://aclanthology.org/P14-1133.pdf) [(Omarov et al, 2023)](https://aclanthology.org/2023.findings-acl.179.pdf)

**Exemplo prático:** O texto "*The artificial intelligence system demonstrated remarkable performance in natural language processing tasks*" pode ser transformado via paráfrase profunda em "*The AI-based framework exhibited exceptional capabilities across various linguistic computational challenges*", preservando o significado central enquanto altera significativamente a estrutura sintática e escolhas lexicais que servem como marcadores estatísticos para detectores automatizados.

## 3.2 Ataques Adversariais

Adversarial Paraphrasing representa uma abordagem sistemática para contornar detectores de texto gerado por IA, funcionando como um **ataque adversarial otimizado** contra classificadores neurais. A metodologia proposta por Cheng et al. (2025) implementa um sistema de paráfrase guiado por gradiente que utiliza o próprio detector como função de custo: para cada posição textual, o sistema seleciona o candidato de substituição que minimiza o score de detecção $D(y)$, onde DD representa a função do detector e yy o texto modificado.[^5]

O processo operacional segue uma **estratégia de busca gulosa**: (1) **Identificação de tokens vulneráveis** através da análise de gradientes do detector, priorizando palavras que mais contribuem para a classificação "IA-gerado"; (2) **Geração de candidatos adversariais** via modelos de substituição semântica que mantêm coerência contextual; (3) **Seleção ótima** baseada na minimização da função $L=D(y)+λ⋅sim(x,y)$, onde $λ$ pondera o trade-off entre evasão e preservação semântica; (4) **Aplicação iterativa** até atingir threshold de detecção ou limite de modificações. [(BERT, Word2Vec)](https://proceedings.mlr.press/v180/wang22b/wang22b.pdf)

## 3.3 Geração Híbrida (Humano-no-Loop)

**Métodos Human-in-the-Loop (HITL)** representam a abordagem mais sofisticada para humanização de textos, integrando **supervisão humana inteligente** no processo de revisão e refinamento pós-geração automatizada. Esta metodologia híbrida combina a eficiência generativa da IA com a capacidade humana de **julgamento contextual, correção estilística e validação semântica**, resultando em textos que são intrinsecamente mais difíceis de detectar por métodos automatizados.[(Zeng et al, 2024)](https://arxiv.org/html/2403.03506v1) (Lokna et al., 2024).[^6]

**Arquitetura operacional**: O sistema HITL implementa um **workflow iterativo** onde: (1) **Geração inicial** por LLM seguindo prompt estruturado; (2) **Análise automatizada** identificando seções de alta perplexidade e marcadores estatísticos típicos de IA; (3) **Intervenção humana direcionada** focalizando modificações em trechos flagrados como suspeitos; (4) **Verificação de coerência** garantindo que edições locais não comprometam fluxo narrativo global; (5) **Validação final** via detector secundário para confirmar redução do score de IA-detecção.[(Yang et al,  2022)](https://ceur-ws.org/Vol-3124/paper6.pdf)

**Limitações de escalabilidade**: Embora altamente efetiva, a abordagem HITL enfrenta **constrangimentos práticos significativos**: (1) **Custo temporal** - cada ciclo de revisão demanda 3-8 minutos por parágrafo, limitando throughput; (2) **Variabilidade inter-humana** - diferentes revisores introduzem padrões estilísticos distintos, complicando detecção mas também consistência; (3) **Fadiga cognitiva** - sessões prolongadas resultam em degradação da qualidade de revisão; (4) **Escalabilidade econômica** - custos de mão-de-obra qualificada tornam o método inviável para aplicações de massa.

# 4. Análise Crítica e Desafios Futuros

## 4.1 Corrida Armamentista

Geradores (OpenAI, Anthropic), detectores (GPTZero) e humanizadores (BypassGPT) formam um ciclo adversarial. O equilíbrio corrente assemelha-se a um **Equilíbrio de Nash**, onde melhorias unilaterais são imediatamente contrabalançadas.

## 4.2 Limitações Fundamentais

Detectores estatísticos apresentam alta taxa de falsos positivos em textos jurídicos ou de não-nativos, enquanto ML-classifiers sofrem com *distribution shift* e ataques adversariais, limitando a generalizabilidade dos modelos.

## 4.3 Direções Futuras

Tendências apontam para:

- **Detecção baseada em resíduos de pré-treinamento** (PRES), explorando assinaturas de treino do LLM;
- **Análise multimodal**, unindo texto a metadados de criação;
- **Frameworks éticos** para uso responsável de IA na escrita.

# 5. Tabela Síntese

| Metodologia (Paper Principal)                              | Princípio Teórico                                   | Pontos Fortes                        | Pontos Fracos                                                      | Eficácia contra Humanizadores |
|:---------------------------------------------------------- |:--------------------------------------------------- |:------------------------------------ |:------------------------------------------------------------------ |:----------------------------- |
| Watermarking (Kirchenbauer et al., 2023)                   | Teoria da informação; green-list pseudo-aleatória   | Detectável mesmo após paráfrase leve | Requer controle sobre o gerador; vulnerável a reescrita profunda   | Baixa                         |
| Análise de Perplexidade/Burstiness (Mitchell et al., 2023) | Estatística de linguagem; entropia de perplexidade  | Simples e intuitiva                  | Alta taxa de falsos positivos; ineficaz contra textos pós-editados | Média-Baixa                   |
| Classificadores Fine-Tuned (Wang et al., 2024)             | Aprendizado supervisionado; features estilométricas | Alta precisão em dados de treino     | Pouco generalizável; fácil de enganar com ataques adversariais     | Média                         |

# 6. Conclusão

Apesar de avanços teóricos e práticos, detectores de texto gerado por IA enfrentam limitações de robustez e generalização, especialmente diante de técnicas de humanização adversarial. A próxima geração de pesquisa deve explorar assinaturas intrínsecas de pré-treinamento, fusão multimodal e diretrizes éticas, buscando balancear detecção eficaz e respeito à integridade acadêmica.
<span style="display:none">[^10][^11][^12][^13][^14][^15][^16][^17][^18][^19][^20][^21][^22][^7][^8][^9]</span>

<div style="text-align: center">⁂</div>

[^1]: https://arxiv.org/abs/2301.10226

[^2]: https://aclanthology.org/2024.findings-naacl.40.pdf

[^3]: https://aclanthology.org/2023.emnlp-main.136.pdf

[^4]: https://aclanthology.org/2024.semeval-1.132.pdf

[^5]: https://www.themoonlight.io/en/review/adversarial-paraphrasing-a-universal-attack-for-humanizing-ai-generated-text

[^6]: https://openreview.net/forum?id=UZS6D7GfP1

[^7]: https://dataloop.ai/library/model/openai-community_roberta-base-openai-detector/

[^8]: https://nationalcentreforai.jiscinvolve.org/wp/2023/03/17/ai-writing-detectors/

[^9]: https://github.com/jwkirchenbauer/lm-watermarking

[^10]: https://gptzero.me/news/perplexity-and-burstiness-what-is-it/

[^11]: https://huggingface.co/fakespot-ai/roberta-base-ai-text-detection-v1

[^12]: https://llmrisks.github.io/week11/

[^13]: http://arxiv.org/pdf/2301.10226.pdf

[^14]: https://openaccess.thecvf.com/content/CVPR2022/papers/Bar_DETReg_Unsupervised_Pretraining_With_Region_Priors_for_Object_Detection_CVPR_2022_paper.pdf

[^15]: https://botpress.com/blog/human-in-the-loop

[^16]: https://www.arxiv.org/abs/2506.07001

[^17]: http://proceedings.mlr.press/v97/hendrycks19a/hendrycks19a.pdf

[^18]: https://cloud.google.com/discover/human-in-the-loop

[^19]: https://github.com/chengez/Adversarial-Paraphrasing

[^20]: https://openaccess.thecvf.com/content/ICCV2023/papers/Li_AlignDet_Aligning_Pre-training_and_Fine-tuning_in_Object_Detection_ICCV_2023_paper.pdf

[^21]: https://arxiv.org/abs/2403.14711

[^22]: https://userway.org/blog/human-in-the-loop/
