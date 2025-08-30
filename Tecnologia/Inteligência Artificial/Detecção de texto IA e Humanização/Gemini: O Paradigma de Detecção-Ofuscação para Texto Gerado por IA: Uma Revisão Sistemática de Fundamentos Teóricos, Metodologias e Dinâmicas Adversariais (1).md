# Gemini: O Paradigma de Detecção-Ofuscação para Texto Gerado por IA: Uma Revisão Sistemática de Fundamentos Teóricos, Metodologias e Dinâmicas Adversariais

## **Resumo**

Esta revisão fornece uma análise sistemática do panorama técnico e teórico da detecção de texto gerado por IA e suas correspondentes técnicas de evasão. Começamos por desconstruir os fundamentos estatísticos que tornam o texto gerado por máquina detectável, focando em conceitos de distribuição de probabilidade, entropia e perplexidade. Subsequentemente, avaliamos criticamente as principais metodologias de detecção, incluindo marcas d'água estatísticas, classificação *zero-shot* baseada em perplexidade e modelos *transformer* com *fine-tuning*. A análise então se volta para as técnicas de ofuscação, enquadrando a "humanização" de texto como uma forma de ataque adversarial projetado para alterar as propriedades estatísticas do texto gerado para evadir a classificação. Sintetizamos essa interação dinâmica como uma contínua "corrida armamentista", contextualizada dentro de princípios da teoria dos jogos. O relatório culmina em uma discussão sobre as limitações fundamentais dos detectores atuais, como altas taxas de falsos positivos para demografias e gêneros específicos, e explora futuras fronteiras de pesquisa, incluindo análise multimodal e o desenvolvimento de arcabouços éticos robustos. Este trabalho identifica lacunas críticas de pesquisa e ressalta as profundas implicações para a integridade acadêmica e o ecossistema de informação digital mais amplo.

## **1\. Fundamentos Teóricos da Detecção de Texto Gerado por IA**

A capacidade de detectar texto gerado por Inteligência Artificial (IA) não é acidental, mas sim uma consequência direta dos princípios estatísticos que governam os Grandes Modelos de Linguagem (LLMs). A natureza autorregressiva e de maximização de probabilidade desses modelos deixa uma assinatura estatística distinta, que difere da escrita humana, mais variada e menos previsível.

### **1.1. A Assinatura Estatística dos Grandes Modelos de Linguagem (LLMs)**

Fundamentalmente, os LLMs são modelos probabilísticos que geram texto *token* por *token*, amostrando de uma distribuição de probabilidade condicional, P(token∣contexto precedente).1 Este processo, otimizado para maximizar a verossimilhança em vastos corpora de treinamento, resulta em textos que tendem a seguir os caminhos de maior probabilidade.3 Ao contrário de escritores humanos, que introduzem criatividade, idiossincrasias e escolhas lexicais "surpreendentes", os LLMs, por padrão, gravitam em torno da média estatística de seus dados de treinamento.  
Isso leva a um texto com menor entropia (menos aleatoriedade ou surpresa) e uma distribuição de probabilidade mais uniforme em comparação com o texto humano.4 A entropia, no contexto da teoria da informação, quantifica a incerteza na saída de um processo aleatório; uma entropia mais alta indica maior incerteza.5 Como os LLMs são treinados para prever com precisão o próximo  
*token*, seus resultados são inerentemente menos incertos e, portanto, de menor entropia.4 Essa regularidade estatística é a principal vulnerabilidade explorada pelos sistemas de detecção.6 A detectabilidade do texto de IA, portanto, não é uma falha nos LLMs, mas uma consequência direta e inevitável de seu objetivo central de projeto: maximizar a verossimilhança probabilística. Para se tornar "indetectável" por essas medidas estatísticas, um LLM teria que ser fundamentalmente redesenhado para  
*não* escolher os *tokens* mais prováveis, o que contradiria diretamente seu objetivo primário de treinamento e provavelmente degradaria sua coerência e qualidade.

### **1.2. Perplexidade e a Curva de Log-Probabilidade**

A perplexidade é a métrica canônica para avaliar modelos de linguagem, matematicamente definida como a exponencial da média do logaritmo negativo da verossimilhança por *token*.4 Uma perplexidade menor indica que o modelo está menos "surpreso" com o texto, significando que o texto é altamente previsível de acordo com a distribuição de probabilidade do modelo. Textos gerados por LLMs, por serem otimizados para alta probabilidade, exibem consistentemente baixa perplexidade quando avaliados pelo mesmo modelo ou por um similar.7  
O trabalho seminal de Mitchell et al. (2023) sobre o DetectGPT formalizou a hipótese de que o texto gerado por IA reside em máximos locais da função de log-probabilidade do modelo de origem. Perturbar tal texto (por exemplo, substituindo uma palavra) é altamente provável que diminua sua log-probabilidade (aumentando sua perplexidade). Em contrapartida, o texto escrito por humanos, que é menos otimizado para a distribuição do modelo, pode ter sua log-probabilidade aumentada ou diminuída após a perturbação.9 Este conceito de uma "curvatura negativa" no espaço de log-probabilidade serve como um poderoso indicador  
*zero-shot* para detecção.10

### **1.3. O Problema de Classificação Binária e Suas Assimetrias Inerentes**

A tarefa de detectar texto gerado por IA é enquadrada como um problema de classificação binária: classificar um determinado texto como "Humano" ou "IA".12 No entanto, esta não é uma tarefa de classificação padrão e simétrica. Em ambientes de alto risco, como a academia, o custo de um falso positivo (acusar um estudante de má conduta quando ele é inocente) é muito maior do que o custo de um falso negativo (não detectar o uso de IA).14  
Essa assimetria exige uma taxa de falsos positivos excepcionalmente baixa, o que restringe severamente o projeto e o limiar dos modelos de detecção.15 Estudos e relatórios da indústria, incluindo da Turnitin, reconhecem que os detectores têm taxas de falsos positivos não triviais e podem ser tendenciosos.14 A pesquisa mostra que esses sistemas sinalizam desproporcionalmente textos de falantes não nativos de inglês, cuja escrita pode ser mais formulada, e textos em gêneros altamente formais ou técnicos (por exemplo, documentos legais, artigos científicos), que naturalmente exibem menor variação estilística.14 A demanda institucional por uma taxa de falsos positivos quase nula força os desenvolvedores de detectores a adotar limiares de classificação muito conservadores, permitindo deliberadamente que alguns textos gerados por IA passem para minimizar acusações incorretas.18 Essa postura conservadora significa que qualquer texto que não seja  
*flagrantemente* semelhante à IA (ou seja, que tenha sido ligeiramente modificado ou "humanizado") provavelmente será classificado como humano para errar pelo lado da cautela. Assim, as restrições éticas e práticas do domínio de aplicação criam diretamente uma vulnerabilidade explorável para ataques adversariais.

## **2\. Uma Revisão Sistemática das Metodologias de Detecção**

Passando da teoria para a prática, esta seção revisa sistematicamente as principais classes de algoritmos de detecção e suas implementações no mundo real, como as encontradas em ferramentas como GPTZero, ZeroGPT e Turnitin.

### **2.1. Detecção Intrínseca: Marcas d'Água Estatísticas**

Esta abordagem, pioneira de Kirchenbauer et al. (2023), incorpora um sinal estatístico detectável diretamente no texto gerado no momento da criação, exigindo controle sobre o processo de geração.20  
**Mecanismo:** Antes de gerar um *token*, um gerador de números pseudoaleatórios (semeado pelos *tokens* anteriores) particiona o vocabulário do modelo em uma "lista verde" (*greenlist*) e uma "lista vermelha" (*redlist*). O algoritmo de geração é então modificado para promover suavemente os *tokens* da *greenlist*.20 Um detector, conhecendo a semente e o algoritmo, pode então contar o número de  
*tokens* da *greenlist* em um texto. Uma super-representação estatisticamente significativa de *tokens* verdes indica geração por IA, frequentemente medida através de um teste de pontuação-z.21 A pontuação-z quantifica o quão incomum é a contagem observada de  
*tokens* verdes sob a hipótese nula de que o texto foi gerado sem conhecimento da *greenlist*.  
**Análise:** A marca d'água é uma técnica poderosa porque o sinal está embutido no texto principal e pode ser robusto a paráfrases leves.23 No entanto, sua principal fraqueza é que requer a cooperação do provedor do LLM (por exemplo, OpenAI, Google), tornando-a inaplicável para detectar texto de modelos sem marca d'água. Além disso, pesquisas mostram que ela é vulnerável a ataques de paráfrase fortes que reescrevem significativamente o texto 24, e pode degradar a qualidade do texto gerado ao alterar a distribuição de probabilidade natural do modelo.22

### **2.2. Detecção Extrínseca: Classificadores Zero-Shot e Baseados em Características**

Esses métodos analisam o texto como ele é, sem exigir modificações no processo de geração.

#### **2.2.1. Métricas de Perplexidade e Variação (Burstiness) (GPTZero, ZeroGPT)**

**Mecanismo:** Ferramentas como GPTZero operacionalizam os conceitos da Seção 1\. Elas não requerem um classificador com *fine-tuning*, mas dependem de características estatísticas. A **perplexidade** mede a previsibilidade do texto sob um modelo de referência (como o GPT-2), esperando-se que o texto de IA tenha menor perplexidade.26 A  
**variação** (*burstiness*) mede a variância da perplexidade entre as sentenças; a escrita humana tende a ter alta variação (picos de complexidade), enquanto o texto de IA é mais uniforme (baixa variação).26  
**Análise:** Esta abordagem é intuitiva e computacionalmente eficiente. No entanto, sua dependência de limiares estatísticos simples a torna frágil. Ela é propensa a altas taxas de falsos positivos para escrita humana formal ou repetitiva e é facilmente contornada por técnicas de humanização que introduzem intencionalmente variação lexical e estrutural, aumentando assim tanto a perplexidade quanto a variação.30 Ferramentas comerciais como ZeroGPT utilizam uma metodologia subjacente semelhante, combinando análise de aprendizado de máquina com essas métricas estatísticas.32

#### **2.2.2. Características Estilométricas e Baseadas em Embeddings**

**Mecanismo:** Esta abordagem expande a detecção baseada em características para além da perplexidade. A **estilometria** envolve a análise quantitativa do estilo de escrita, incluindo características como diversidade lexical (razão tipo-*token*), distribuição do comprimento das sentenças, uso de palavras de função e complexidade sintática (por exemplo, profundidade da árvore de análise sintática).35 Outra abordagem utiliza o espaço de  
*embedding* de modelos como o RoBERTa para representar sentenças como vetores, capturando padrões semânticos e estilísticos mais profundos que podem ser alimentados a um classificador.6  
**Análise:** Essas características fornecem um sinal mais rico do que a perplexidade isoladamente. No entanto, como qualquer método baseado em características, elas são vulneráveis a serem explicitamente visadas e manipuladas por humanizadores sofisticados.

### **2.3. Detecção Supervisionada: Classificadores Transformer com Fine-Tuning (Turnitin, OpenAI Detector)**

**Mecanismo:** Esta abordagem envolve o *fine-tuning* de um grande modelo pré-treinado, mais comumente uma arquitetura baseada em RoBERTa, em um conjunto de dados massivo de textos pareados escritos por humanos e gerados por IA.39 O modelo aprende a classificar novos textos com base nos padrões sutis que descobriu durante o  
*fine-tuning*. Este é o método empregado pelos primeiros detectores da OpenAI e é um componente central de sistemas comerciais como o da Turnitin.18  
**Análise:** Esses classificadores alcançam uma precisão muito alta em dados que estão *em distribuição* (ou seja, semelhantes aos seus dados de treinamento).39 No entanto, sua principal fraqueza é a falta de generalização. Eles têm um desempenho ruim quando confrontados com textos de LLMs novos e não vistos ou com textos que foram modificados adversarialmente (por exemplo, via paráfrase), pois isso desloca os dados para fora de sua distribuição de treinamento.43

### **Tabela Síntese das Metodologias de Detecção de IA**

| Metodologia (Paper Principal)                                   | Princípio Teórico                                                          | Pontos Fortes                                                                                | Pontos Fracos                                                                                                   | Eficácia contra Humanizadores |
|:--------------------------------------------------------------- |:-------------------------------------------------------------------------- |:-------------------------------------------------------------------------------------------- |:--------------------------------------------------------------------------------------------------------------- |:----------------------------- |
| **Watermarking** (Kirchenbauer et al., 2023\)                   | Teoria da informação, Injeção de sinal estatístico (Tokens "Green-listed") | Detectável mesmo após paráfrase leve; alta precisão teórica; p-valor interpretável.          | Requer controle sobre o modelo gerador; degrada a qualidade do texto; pode ser removido por reescrita profunda. | **Baixa**                     |
| **Análise de Perplexidade/Burstiness** (Mitchell et al., 2023\) | Estatística de Linguagem, Curvatura de Log-Probabilidade                   | Simples, intuitivo, computacionalmente barato; não requer acesso ao modelo.                  | Alta taxa de falsos positivos em textos de não-nativos e formais; facilmente enganado.                          | **Média-Baixa**               |
| **Classificadores Fine-Tuned** (e.g., OpenAI Detector)          | Aprendizado de Máquina Supervisionado, Análise de Padrões em Embeddings    | Alta precisão em dados de treino (in-distribution); pode capturar características complexas. | Pouco generalizável para novos LLMs; frágil a ataques adversariais (paráfrase).                                 | **Média**                     |

## **3\. Evasão e Ofuscação: A Ascensão da Humanização de Texto**

Em resposta direta às tecnologias de detecção, surgiu uma contra-movimentação de ferramentas e técnicas de ofuscação. Esta seção analisa a "humanização" não como uma simples ferramenta de edição, mas como uma forma sofisticada de ataque adversarial projetado para manipular as propriedades estatísticas nas quais os detectores se baseiam.

### **3.1. Paráfrase Profunda como uma Mudança Distribucional**

Ferramentas "humanizadoras" (como BypassGPT e análogos) operam por meio de *paráfrase profunda*. Isso não é meramente substituir palavras por sinônimos, mas envolve reescrever fundamentalmente as sentenças — alterando estruturas sintáticas, mudando a ordem das orações e fazendo substituições lexicais mais abstratas, enquanto se preserva o significado semântico central.46  
O objetivo deste processo é deslocar a distribuição estatística do texto gerado por IA. Ao introduzir estruturas de sentenças mais complexas e variadas e escolhas de palavras menos previsíveis, essas ferramentas aumentam diretamente a perplexidade e a variação (*burstiness*) do texto, afastando-o da "distribuição de IA" e aproximando-o da "distribuição humana" aos olhos de um classificador estatístico.30 O termo "humanizador" é, em certo sentido, um equívoco. Essas ferramentas não tornam necessariamente o texto mais humano em um sentido qualitativo; elas o tornam  
*menos parecido com uma IA estereotipada*. Elas são projetadas para quebrar os padrões estatísticos específicos que os detectores procuram. Isso implica que um detector mais avançado, treinado na saída dos próprios humanizadores, poderia aprender a identificar os artefatos do *processo de humanização* (por exemplo, frases estranhas de substituição de sinônimos), representando o próximo passo na corrida armamentista.43

### **3.2. Humanização como um Ataque Adversarial**

No campo do aprendizado de máquina, a humanização é um exemplo clássico de um ataque adversarial.49 Especificamente, é um ataque de  
*caixa-preta* (*black-box*) onde o adversário (a ferramenta humanizadora) perturba a entrada (o texto gerado por IA) para causar uma classificação incorreta direcionada (de "IA" para "Humano") pelo modelo vítima (o detector), sem precisar de conhecimento da arquitetura interna do detector.49  
A pesquisa demonstrou a eficácia desses ataques. Estudos mostram que até mesmo uma paráfrase simples pode reduzir drasticamente a precisão do detector.43 Ataques mais avançados envolvem a substituição direcionada de sinônimos com base na similaridade de  
*embedding* para minimizar a mudança semântica enquanto maximiza o impacto na pontuação de classificação do detector.53 Os humanizadores mais eficazes podem ser vistos como sistemas que são treinados adversarialmente contra os detectores.43

### **3.3. O Padrão Ouro do Human-in-the-Loop (HITL)**

O método mais eficaz, embora menos escalável, de humanização é a edição manual por um humano após a geração.55 Essa abordagem  
*human-in-the-loop* (HITL) utiliza a IA para o rascunho inicial e o humano para introduzir nuances estilísticas genuínas, criatividade e correção de erros.  
Este método é o padrão ouro para a evasão porque introduz padrões estatísticos humanos verdadeiros em vez de simulados. Ele representa o limite superior teórico de dificuldade para qualquer detector, pois o produto final é uma colaboração genuína humano-IA. O desafio para os detectores é que tal texto não é puramente "IA" ou "Humano", mas ocupa um espaço híbrido.12 Curiosamente, o HITL também é proposto como um método para  
*melhorar* a detecção, onde um especialista humano é auxiliado por ferramentas de IA que destacam padrões suspeitos, criando uma dualidade fascinante no paradigma.57

## **4\. Análise Crítica e Trajetórias Futuras**

Esta seção final sintetiza a análise anterior, enquadrando a dinâmica do campo através de uma lente da teoria dos jogos, discutindo limitações fundamentais e projetando futuras direções de pesquisa com base na literatura.

### **4.1. A Corrida Armamentista Adversarial como um Equilíbrio de Nash**

A dinâmica entre geradores de LLM, detectores e humanizadores pode ser modelada como um jogo de múltiplos jogadores. Um avanço de uma parte (por exemplo, um detector melhor) cria um incentivo para que as outras se adaptem (por exemplo, um humanizador melhor), levando a uma "corrida armamentista" contínua e escalonada.45  
Esta situação se aproxima de um **Equilíbrio de Nash** dinâmico e instável. Neste contexto, um Equilíbrio de Nash é um estado onde nenhum jogador (gerador, detector ou humanizador) pode melhorar seu resultado mudando unilateralmente sua estratégia, dadas as estratégias dos outros.60 Se um detector se torna perfeito, os humanizadores evoluirão para vencê-lo. Se um humanizador se torna perfeito, os detectores evoluirão para identificar seus artefatos. O equilíbrio é instável porque os enormes incentivos comerciais e acadêmicos garantem que os jogadores estejam constantemente investindo em novas estratégias, impedindo que qualquer tecnologia alcance um domínio permanente.58

### **4.2. Limitações Fundamentais e o Problema da Generalização**

Pode haver limites fundamentais, talvez intransponíveis, para a confiabilidade da detecção de texto por IA. O desafio central é que as distribuições estatísticas de texto escrito por humanos e gerado por IA não são disjuntas; elas se sobrepõem significativamente. A escrita humana altamente formulaica (por exemplo, documentos legais, alguns artigos científicos) pode exibir baixa perplexidade, enquanto saídas criativas de IA podem ser intencionalmente variadas.14  
Conforme discutido, os detectores mostram vieses significativos, com taxas mais altas de falsos positivos para falantes não nativos de inglês.16 Além disso, os detectores são frequentemente frágeis, com modelos treinados na saída de um LLM falhando em generalizar para outros.45 À medida que os LLMs melhoram, suas distribuições de saída convergirão inevitavelmente mais de perto com as distribuições humanas, tornando a detecção teoricamente mais difícil com o tempo.43

### **4.3. Fronteiras de Pesquisa Futuras**

Apesar dos desafios, várias direções de pesquisa promissoras estão surgindo.

#### **4.3.1. Detecção Baseada em Resíduos de Pré-treinamento (PRES)**

Uma via promissora é procurar por artefatos não do processo de geração em si, mas dos dados e do processo de *pré-treinamento*. Os LLMs são pré-treinados em texto da web vasto e não filtrado.64 Esses dados de treinamento contêm peculiaridades estatísticas, vieses e inconsistências factuais. Detectores futuros podem ser capazes de identificar esses "resíduos de pré-treinamento" que são sutilmente reproduzidos no texto de saída. Esses resíduos seriam muito mais difíceis para um humanizador remover, pois estão profundamente embutidos nos parâmetros do modelo.

#### **4.3.2. Análise Multimodal**

O futuro da detecção pode estar em ir além do próprio texto. Uma abordagem multimodal incorporaria metadados sobre o processo de criação, como dinâmica de digitação, padrões de edição no histórico de versões de um documento (por exemplo, grandes colagens versus digitação incremental), tempo gasto para escrever e outros sinais comportamentais.66 Isso mudaria o problema de "este texto parece gerado por IA?" para "este texto foi produzido por um processo humano?".

#### **4.3.3. Arcabouços Éticos para Escrita Assistida por IA**

A solução definitiva pode não ser puramente técnica. Uma direção de pesquisa significativa é o desenvolvimento de arcabouços éticos robustos e políticas institucionais para o uso de IA.69 Isso envolve definir níveis aceitáveis de assistência de IA (por exemplo, brainstorming versus geração completa de rascunhos), estabelecer requisitos claros de divulgação e focar a pedagogia em habilidades que a IA não pode replicar, como pensamento crítico e argumentação original. Isso reformula o problema de "detecção de plágio" para "gerenciamento de uma nova ferramenta de escrita". A corrida armamentista provavelmente terminará não com uma vitória técnica para a detecção, mas com uma mudança de paradigma em direção à aceitação e regulamentação da escrita assistida por IA, de forma semelhante à integração de calculadoras na educação matemática.

## **5\. Conclusão: Síntese e Lacunas de Pesquisa**

Esta revisão sistemática analisou o paradigma de detecção de texto gerado por IA e as técnicas de ofuscação correspondentes. A análise revela que o problema de detecção está enraizado na previsibilidade estatística dos LLMs, o que deu origem a um conjunto de métodos de detecção (marcas d'água, baseados em características, supervisionados) que estão em uma luta constante com ataques adversariais cada vez mais sofisticados (humanização). Essa dinâmica é melhor compreendida como uma corrida armamentista instável, sem um vencedor técnico claro no horizonte.  
As principais lacunas de pesquisa e implicações incluem:

* **Lacuna 1: Detectores Robustos e Generalizáveis:** A lacuna técnica mais significativa é a falta de detectores que sejam robustos à paráfrase e possam generalizar entre diferentes LLMs sem retreinamento constante.  
* **Lacuna 2: Compreensão Teórica da Humanização:** Embora saibamos que os humanizadores funcionam, é necessária uma compreensão teórica mais profunda das transformações que eles realizam no espaço de *embedding* de alta dimensão para desenvolver contramedidas mais eficazes.  
* **Lacuna 3: Estudos Longitudinais:** Há uma necessidade de estudos longitudinais sobre o impacto das ferramentas de detecção e humanização de IA nos resultados educacionais e na qualidade da escrita.

A falta de confiabilidade da detecção representa uma ameaça significativa à integridade acadêmica, podendo levar a acusações falsas e à erosão da confiança. A proliferação de conteúdo gerado e humanizado por IA tem profundas implicações para a literacia digital, a desinformação e a própria definição de autoria no século 21\. O caminho a seguir exige uma combinação de inovação técnica, adaptação pedagógica e o desenvolvimento de diretrizes éticas claras.

#### **Referências citadas**

1. N-gram Language Models \- Stanford University, acessado em agosto 19, 2025, [https://web.stanford.edu/\~jurafsky/slp3/3.pdf](https://web.stanford.edu/~jurafsky/slp3/3.pdf)  
2. Language Modeling \- Lena Voita, acessado em agosto 19, 2025, [https://lena-voita.github.io/nlp\_course/language\_modeling.html](https://lena-voita.github.io/nlp_course/language_modeling.html)  
3. An Overview of Large Language Models for Statisticians \- arXiv, acessado em agosto 19, 2025, [https://arxiv.org/html/2502.17814v1](https://arxiv.org/html/2502.17814v1)  
4. Large language model \- Wikipedia, acessado em agosto 19, 2025, [https://en.wikipedia.org/wiki/Large\_language\_model](https://en.wikipedia.org/wiki/Large_language_model)  
5. Evaluating LLMs using semantic entropy | Thoughtworks United States, acessado em agosto 19, 2025, [https://www.thoughtworks.com/en-us/insights/blog/generative-ai/Evaluating-LLM-using-semantic-entropy](https://www.thoughtworks.com/en-us/insights/blog/generative-ai/Evaluating-LLM-using-semantic-entropy)  
6. How Do AI Content Detectors Work? \- Silicon Dales, acessado em agosto 19, 2025, [https://silicondales.com/ai/how-do-ai-content-detectors-work/](https://silicondales.com/ai/how-do-ai-content-detectors-work/)  
7. LLM \- Detect AI Generated Text \- Kaggle, acessado em agosto 19, 2025, [https://www.kaggle.com/c/llm-detect-ai-generated-text/discussion/470224](https://www.kaggle.com/c/llm-detect-ai-generated-text/discussion/470224)  
8. How NLP Powers AI-Generated Text Detection | The AI Journal, acessado em agosto 19, 2025, [https://aijourn.com/how-nlp-powers-ai-generated-text-detection/](https://aijourn.com/how-nlp-powers-ai-generated-text-detection/)  
9. arXiv:2305.17359v2 \[cs.CL\] 4 Oct 2023, acessado em agosto 19, 2025, [https://arxiv.org/pdf/2305.17359](https://arxiv.org/pdf/2305.17359)  
10. Machine-Generated Text Detection using Deep Learning \- arXiv, acessado em agosto 19, 2025, [https://arxiv.org/html/2311.15425v1](https://arxiv.org/html/2311.15425v1)  
11. Detecting AI-Generated Code Assignments ... \- AAAI Publications, acessado em agosto 19, 2025, [https://ojs.aaai.org/index.php/AAAI/article/view/30361/32410](https://ojs.aaai.org/index.php/AAAI/article/view/30361/32410)  
12. Detecting AI-Generated Text: Factors Influencing Detectability with Current Methods \- arXiv, acessado em agosto 19, 2025, [https://arxiv.org/html/2406.15583v1](https://arxiv.org/html/2406.15583v1)  
13. Enhancing Text Authenticity: A Novel Hybrid Approach for AI-Generated Text Detection, acessado em agosto 19, 2025, [https://arxiv.org/html/2406.06558v1](https://arxiv.org/html/2406.06558v1)  
14. What Causes AI Detection False Positives \- Detecting-AI.com, acessado em agosto 19, 2025, [https://detecting-ai.com/blog/what-causes-ai-detection-false-positives](https://detecting-ai.com/blog/what-causes-ai-detection-false-positives)  
15. ChatGPT, and AI Detection Tools: the Challenge of False Positives | JD Supra, acessado em agosto 19, 2025, [https://www.jdsupra.com/legalnews/chatgpt-and-ai-detection-tools-the-1617850/](https://www.jdsupra.com/legalnews/chatgpt-and-ai-detection-tools-the-1617850/)  
16. AI detectors: An ethical minefield \- Center for Innovative Teaching and Learning \-, acessado em agosto 19, 2025, [https://citl.news.niu.edu/2024/12/12/ai-detectors-an-ethical-minefield/](https://citl.news.niu.edu/2024/12/12/ai-detectors-an-ethical-minefield/)  
17. The Challenge of AI Checkers | Center for Transformative Teaching | Nebraska, acessado em agosto 19, 2025, [https://teaching.unl.edu/ai-exchange/challenge-ai-checkers/](https://teaching.unl.edu/ai-exchange/challenge-ai-checkers/)  
18. Testing Turnitin's New AI Detector: How Accurate Is It? \- BestColleges.com, acessado em agosto 19, 2025, [https://www.bestcolleges.com/news/analysis/testing-turnitin-new-ai-detector/](https://www.bestcolleges.com/news/analysis/testing-turnitin-new-ai-detector/)  
19. AI writing detection in the new, enhanced Similarity Report \- Turnitin Guides, acessado em agosto 19, 2025, [https://guides.turnitin.com/hc/en-us/articles/22774058814093-AI-writing-detection-in-the-new-enhanced-Similarity-Report](https://guides.turnitin.com/hc/en-us/articles/22774058814093-AI-writing-detection-in-the-new-enhanced-Similarity-Report)  
20. A Watermark for Large Language Models \- arXiv, acessado em agosto 19, 2025, [https://arxiv.org/abs/2301.10226](https://arxiv.org/abs/2301.10226)  
21. Optimizing watermarks for large language models \- arXiv, acessado em agosto 19, 2025, [https://arxiv.org/html/2312.17295v1](https://arxiv.org/html/2312.17295v1)  
22. arXiv:2311.09816v2 \[cs.CL\] 20 Oct 2024, acessado em agosto 19, 2025, [https://arxiv.org/pdf/2311.09816](https://arxiv.org/pdf/2311.09816)  
23. \[2306.04634\] On the Reliability of Watermarks for Large Language Models \- arXiv, acessado em agosto 19, 2025, [https://arxiv.org/abs/2306.04634](https://arxiv.org/abs/2306.04634)  
24. Watermarks in the Sand: Impossibility of Strong Watermarking for Generative Models \- arXiv, acessado em agosto 19, 2025, [https://arxiv.org/html/2311.04378v5](https://arxiv.org/html/2311.04378v5)  
25. arXiv:2311.09668v1 \[cs.CL\] 16 Nov 2023, acessado em agosto 19, 2025, [https://arxiv.org/pdf/2311.09668](https://arxiv.org/pdf/2311.09668)  
26. Citation: İ. Tarım, A. Onan. Can You Detect the Difference? \- arXiv, acessado em agosto 19, 2025, [https://arxiv.org/html/2507.10475v1](https://arxiv.org/html/2507.10475v1)  
27. Can You Detect the Difference? \- arXiv, acessado em agosto 19, 2025, [https://arxiv.org/pdf/2507.10475?](https://arxiv.org/pdf/2507.10475)  
28. How do I interpret burstiness or perplexity? \- GPTZero, acessado em agosto 19, 2025, [https://support.gptzero.me/hc/en-us/articles/15130070230551-How-do-I-interpret-burstiness-or-perplexity](https://support.gptzero.me/hc/en-us/articles/15130070230551-How-do-I-interpret-burstiness-or-perplexity)  
29. What is perplexity & burstiness for AI detection? \- GPTZero, acessado em agosto 19, 2025, [https://gptzero.me/news/perplexity-and-burstiness-what-is-it/](https://gptzero.me/news/perplexity-and-burstiness-what-is-it/)  
30. How Do Perplexity and Burstiness Make AI Text Undetectable? \- StealthGPT, acessado em agosto 19, 2025, [https://www.stealthgpt.ai/blog/how-do-perplexity-and-burstiness-make-ai-text-undetectable](https://www.stealthgpt.ai/blog/how-do-perplexity-and-burstiness-make-ai-text-undetectable)  
31. Perplexity and Burstiness: Not Just Simple Metrics Anymore \- ShadowGPT, acessado em agosto 19, 2025, [https://humanizeai.now/blog/perplexity-burstiness-2025](https://humanizeai.now/blog/perplexity-burstiness-2025)  
32. ZeroGPT | AI Detector, Chat GPT Zero, AI Text Detector, acessado em agosto 19, 2025, [https://zerogpt.org/](https://zerogpt.org/)  
33. Is ZeroGPT Reliable? A Detailed Review on Its Accuracy \- PopAi, acessado em agosto 19, 2025, [https://www.popai.pro/resources/is-zerogpt-reliable-a-detailed-review-on-its-accuracy/](https://www.popai.pro/resources/is-zerogpt-reliable-a-detailed-review-on-its-accuracy/)  
34. AI Detector \- Trusted AI Checker for ChatGPT, GPT5 & Gemini, acessado em agosto 19, 2025, [https://www.zerogpt.com/](https://www.zerogpt.com/)  
35. HANSEN: Human and AI Spoken Text Benchmark for Authorship ..., acessado em agosto 19, 2025, [https://aclanthology.org/2023.findings-emnlp.916/](https://aclanthology.org/2023.findings-emnlp.916/)  
36. UNCOVER: Identifying AI Generated News Articles by Linguistic Analysis and Visualization \- SciTePress, acessado em agosto 19, 2025, [https://www.scitepress.org/Papers/2023/121633/121633.pdf](https://www.scitepress.org/Papers/2023/121633/121633.pdf)  
37. Stylometric Detection of AI-Generated Text in Twitter Timelines, acessado em agosto 19, 2025, [https://arxiv.org/pdf/2303.03697](https://arxiv.org/pdf/2303.03697)  
38. AI-generated Text Detection: A Multifaceted Approach to Binary and Multiclass Classification, acessado em agosto 19, 2025, [https://arxiv.org/html/2505.11550v1](https://arxiv.org/html/2505.11550v1)  
39. openai-community/roberta-base-openai-detector · Hugging Face, acessado em agosto 19, 2025, [https://huggingface.co/openai-community/roberta-base-openai-detector](https://huggingface.co/openai-community/roberta-base-openai-detector)  
40. Implementing BERT and fine-tuned RobertA to detect AI generated news by ChatGPT \- arXiv, acessado em agosto 19, 2025, [https://arxiv.org/abs/2306.07401](https://arxiv.org/abs/2306.07401)  
41. AI writing detection in the classic report view \- Turnitin Guides, acessado em agosto 19, 2025, [https://guides.turnitin.com/hc/en-us/articles/28457596598925-AI-writing-detection-in-the-classic-report-view](https://guides.turnitin.com/hc/en-us/articles/28457596598925-AI-writing-detection-in-the-classic-report-view)  
42. AI Checker Solutions: Ensure Academic Integrity \- Turnitin, acessado em agosto 19, 2025, [https://www.turnitin.com/solutions/topics/ai-writing/](https://www.turnitin.com/solutions/topics/ai-writing/)  
43. DAMAGE: Detecting Adversarially Modified AI Generated Text \- arXiv, acessado em agosto 19, 2025, [https://arxiv.org/html/2501.03437v1](https://arxiv.org/html/2501.03437v1)  
44. Technical Report on the Checkfor.ai AI-Generated Text Classifier \- arXiv, acessado em agosto 19, 2025, [https://arxiv.org/html/2402.14873v2](https://arxiv.org/html/2402.14873v2)  
45. Detecting Machine-Generated Text: An Arms Race With the ..., acessado em agosto 19, 2025, [https://blog.seas.upenn.edu/detecting-machine-generated-text-an-arms-race-with-the-advancements-of-large-language-models/](https://blog.seas.upenn.edu/detecting-machine-generated-text-an-arms-race-with-the-advancements-of-large-language-models/)  
46. Humanize AI Text: Free AI Humanizer Tool \- QuillBot, acessado em agosto 19, 2025, [https://quillbot.com/ai-humanizer](https://quillbot.com/ai-humanizer)  
47. Do AI Humanizers Work ? How Do They Work ? \- Intellectual Lead, acessado em agosto 19, 2025, [https://intellectualead.com/do-ai-humanizers-work/](https://intellectualead.com/do-ai-humanizers-work/)  
48. arXiv:2501.03437v1 \[cs.CL\] 6 Jan 2025, acessado em agosto 19, 2025, [https://arxiv.org/pdf/2501.03437](https://arxiv.org/pdf/2501.03437)  
49. Adversarial Attacks: The Hidden Risk in AI Security, acessado em agosto 19, 2025, [https://securing.ai/ai-security/adversarial-attacks-ai/](https://securing.ai/ai-security/adversarial-attacks-ai/)  
50. What Is Adversarial AI in Machine Learning? \- Palo Alto Networks, acessado em agosto 19, 2025, [https://www.paloaltonetworks.com/cyberpedia/what-are-adversarial-attacks-on-AI-Machine-Learning](https://www.paloaltonetworks.com/cyberpedia/what-are-adversarial-attacks-on-AI-Machine-Learning)  
51. Explainable Artificial Intelligence with Integrated Gradients for the Detection of Adversarial Attacks on Text Classifiers \- MDPI, acessado em agosto 19, 2025, [https://www.mdpi.com/2571-5577/8/1/17](https://www.mdpi.com/2571-5577/8/1/17)  
52. Humanizing Machine-Generated Content: Evading AI-Text Detection through Adversarial Attack \- arXiv, acessado em agosto 19, 2025, [https://arxiv.org/html/2404.01907v1](https://arxiv.org/html/2404.01907v1)  
53. \[Literature Review\] Adversarial Attacks on AI-Generated Text Detection Models: A Token Probability-Based Approach Using Embeddings \- Moonlight, acessado em agosto 19, 2025, [https://www.themoonlight.io/en/review/adversarial-attacks-on-ai-generated-text-detection-models-a-token-probability-based-approach-using-embeddings](https://www.themoonlight.io/en/review/adversarial-attacks-on-ai-generated-text-detection-models-a-token-probability-based-approach-using-embeddings)  
54. TextHoaxer: Budgeted Hard-Label Adversarial Attacks on Text \- AAAI, acessado em agosto 19, 2025, [https://cdn.aaai.org/ojs/20303/20303-13-24316-1-2-20220628.pdf](https://cdn.aaai.org/ojs/20303/20303-13-24316-1-2-20220628.pdf)  
55. Can AI Paraphrasing Help with Academic Writing? Experiment Results \- Reddit, acessado em agosto 19, 2025, [https://www.reddit.com/r/ArtificialInteligence/comments/1j8mfkf/can\_ai\_paraphrasing\_help\_with\_academic\_writing/](https://www.reddit.com/r/ArtificialInteligence/comments/1j8mfkf/can_ai_paraphrasing_help_with_academic_writing/)  
56. Evaluating the Effectiveness of Turnitin's AI Writing Indicator Model \- Center for the Advancement of Teaching, acessado em agosto 19, 2025, [https://teaching.temple.edu/sites/teaching/files/media/document/Evaluating%20the%20Effectiveness%20of%20Turnitin%E2%80%99s%20AI%20Writing%20Indicator%20Model.pdf](https://teaching.temple.edu/sites/teaching/files/media/document/Evaluating%20the%20Effectiveness%20of%20Turnitin%E2%80%99s%20AI%20Writing%20Indicator%20Model.pdf)  
57. HUMAN-IN-THE-LOOP DETECTION OF AI ... \- OpenReview, acessado em agosto 19, 2025, [https://openreview.net/pdf/6c2a88a2ecba25656924a2468df553f6efeeb214.pdf](https://openreview.net/pdf/6c2a88a2ecba25656924a2468df553f6efeeb214.pdf)  
58. AI Detectors vs. LLMs \+ Humanizers: Who Wins the Arms Race? | ScOp Venture Capital, acessado em agosto 19, 2025, [https://www.scopvc.com/thoughts/2025/6/28/ai-detectors-vs-humanizers-and-llms](https://www.scopvc.com/thoughts/2025/6/28/ai-detectors-vs-humanizers-and-llms)  
59. End the AI detection arms race \- PMC, acessado em agosto 19, 2025, [https://pmc.ncbi.nlm.nih.gov/articles/PMC11573885/](https://pmc.ncbi.nlm.nih.gov/articles/PMC11573885/)  
60. Efficiently Computing Nash Equilibria in Adversarial Team Markov Games \- cs.wisc.edu, acessado em agosto 19, 2025, [https://pages.cs.wisc.edu/\~vlatakis/Papers/Efficiently\_Computing\_Nash\_Equilibria\_in\_Adversarial\_Team\_Markov\_Games.html](https://pages.cs.wisc.edu/~vlatakis/Papers/Efficiently_Computing_Nash_Equilibria_in_Adversarial_Team_Markov_Games.html)  
61. Efficiently Computing Nash Equilibria in Adversarial Team Markov Games \- OpenReview, acessado em agosto 19, 2025, [https://openreview.net/forum?id=mjzm6btqgV](https://openreview.net/forum?id=mjzm6btqgV)  
62. The AI Arms Race: Using AI to avoid AI-detection tools | by Pontus Wärnestål \- Medium, acessado em agosto 19, 2025, [https://drpontus.medium.com/the-ai-arms-race-using-ai-to-avoid-ai-detection-tools-529e17643f45](https://drpontus.medium.com/the-ai-arms-race-using-ai-to-avoid-ai-detection-tools-529e17643f45)  
63. AI detection tools risk losing the generative AI arms race \- ITPro, acessado em agosto 19, 2025, [https://www.itpro.com/technology/artificial-intelligence-ai/370293/ai-detection-tools-vs-generative-ai-arms-race](https://www.itpro.com/technology/artificial-intelligence-ai/370293/ai-detection-tools-vs-generative-ai-arms-race)  
64. Improving Language Understanding by Generative Pre-Training \- OpenAI, acessado em agosto 19, 2025, [https://cdn.openai.com/research-covers/language-unsupervised/language\_understanding\_paper.pdf](https://cdn.openai.com/research-covers/language-unsupervised/language_understanding_paper.pdf)  
65. Detecting AI-Generated Text with Pre-Trained Models using Linguistic Features \- ACL Anthology, acessado em agosto 19, 2025, [https://aclanthology.org/2024.icon-1.21.pdf](https://aclanthology.org/2024.icon-1.21.pdf)  
66. Multimodal sentiment analysis \- Wikipedia, acessado em agosto 19, 2025, [https://en.wikipedia.org/wiki/Multimodal\_sentiment\_analysis](https://en.wikipedia.org/wiki/Multimodal_sentiment_analysis)  
67. What is multimodal AI: Complete overview 2025 \- SuperAnnotate, acessado em agosto 19, 2025, [https://www.superannotate.com/blog/multimodal-ai](https://www.superannotate.com/blog/multimodal-ai)  
68. What is Multimodal data in AI? A Complete Guide for Enterprises \- Kellton, acessado em agosto 19, 2025, [https://www.kellton.com/kellton-tech-blog/the-rise-of-multimodal-data-ai](https://www.kellton.com/kellton-tech-blog/the-rise-of-multimodal-data-ai)  
69. www.tandfonline.com, acessado em agosto 19, 2025, [https://www.tandfonline.com/doi/full/10.1080/10494820.2025.2523382\#:\~:text=The%20proposed%20framework%20for%20the,and%20uphold%20ethical%20data%20practices.](https://www.tandfonline.com/doi/full/10.1080/10494820.2025.2523382#:~:text=The%20proposed%20framework%20for%20the,and%20uphold%20ethical%20data%20practices.)  
70. Full article: Proposing a framework for ethical use of AI in academic writing based on a conceptual review: implications for quality education, acessado em agosto 19, 2025, [https://www.tandfonline.com/doi/full/10.1080/10494820.2025.2523382](https://www.tandfonline.com/doi/full/10.1080/10494820.2025.2523382)  
71. How to Ethically Use AI Essay Tools While Maintaining Academic Integrity \- Yomu AI, acessado em agosto 19, 2025, [https://www.yomu.ai/resources/how-to-ethically-use-ai-essay-tools-while-maintaining-academic-integrity](https://www.yomu.ai/resources/how-to-ethically-use-ai-essay-tools-while-maintaining-academic-integrity)  
72. Ethical Use Cases of AI in Academic Writing: A 2025 Guide for Students and Researchers, acessado em agosto 19, 2025, [https://www.thesify.ai/blog/ethical-use-cases-of-ai-in-academic-writing-a-2025-guide-for-students-and-researchers](https://www.thesify.ai/blog/ethical-use-cases-of-ai-in-academic-writing-a-2025-guide-for-students-and-researchers)