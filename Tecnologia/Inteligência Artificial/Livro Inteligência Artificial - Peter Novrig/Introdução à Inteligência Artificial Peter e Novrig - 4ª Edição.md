# Introdução à Inteligência Artificial Peter e Novrig - 4ª Edição

# INTRODUÇÃO

Neste capítulo, tentamos explicar por que consideramos a inteligência artificial um assunto digno de estudo e em que procuramos definir exatamente o que é a inteligência artificial, pois essa definição é importante antes de iniciarmos nosso estudo. 

Denominamos nossa espécie **Homo sapiens** - homem sábio - porque nossa inteligência é muito importante para nós. Durante milhares de anos, procuramos entender como pensamos e agi-mos, isto é, como nosso cérebro, um mero punhado de matéria, pode perceber, compreender, prever e manipular um mundo muito maior e mais complicado que ele próprio. 

O campo da inteligência artificial, ou **IA**, vai ainda mais além: **ele tenta não apenas compreender, mas também construir entidades inteligentes** - máquinas que conseguem computar como agir de modo eficaz e seguro em uma grande variedade de novas situações. Segundo pesquisas, a IA é considerada um dos campos mais interessantes e de mais rápido crescimento, já conseguindo gerar mais de um trilhão de dólares por ano em receitas. Kai-Fu Lee, especialista em IA, prevê que seu impacto será "maior do que tudo na história da humanidade". 

Além disso, as fronteiras intelectuais da IA estão escancaradas. Embora um estudante de uma ciência mais antiga, como a física, possa achar que todas as boas ideias já foram desenvolvidas por Galileu, Newton, Curie, Einstein e demais, a IA ainda tem espaço para vários Einsteins e Edisons em tempo integral.   

Atualmente, a IA abrange uma enorme variedade de subcampos, do mais geral (aprendizagem, raciocínio, percepção etc.) ao mais específico, como:   

- Jogar xadrez   
- Demonstrar teoremas matemáticos   
- Criar poesia   
- Dirigir um carro   
- Diagnosticar doenças   

A IA é relevante para qualquer tarefa intelectual; é verdadeiramente um campo universal.   

## 1.1 O que é lA?

Afirmamos que a lA é interessante, mas não dissemos o que ela é. Historicamente, os pesquisadores têm seguido diversas versões diferentes de IA. Alguns têm definido a inteligência em termos de fidelidade ao desempenho humano, enquanto outros preferem uma definição abstrata e formal da inteligência, chamada de **racionalidade** - em termos gerais, fazer a "coisa certa". O tema em si também varia: alguns consideram a inteligência como uma propriedade dos processos de pensamento e raciocínio internos, enquanto outros enfocam o comportamento inteligente, uma caracterização externa.   

Dessas duas dimensões - **humano contra racional**, **pensamento contra comportamento** -, existem quatro combinações possíveis, com adeptos e programas de pesquisa para todas as quatro. Os métodos usados são necessariamente diferentes:   

- A busca da inteligência semelhante à humana deve ser em parte uma ciência empírica relacionada à psicologia, envolvendo observações e hipóteses sobre o comportamento humano real e os processos de pensamento   
- Uma abordagem racionalista, por outro lado, envolve uma combinação de matemática e engenharia, que se conecta a estatística, teoria de controle e economia   

Cada grupo tem ao mesmo tempo desacreditado e ajudado o outro. Vamos examinar as quatro abordagens com mais detalhes.

---

¹ Aos olhos do público, às vezes há confusão entre os termos "inteligência artificial" e "aprendizado de máquina". O **aprendizado de máquina** é um subcampo da IA que estuda a capacidade de melhorar o desempenho com base na experiência. Alguns sistemas de IA utilizam métodos de aprendizado de máquina para alcançar competência, mas outros não.   
² Não estamos sugerindo que os seres humanos sejam necessariamente "irracionais" no sentido de "desprovidos de clareza mental normal". Simplesmente precisamos observar que as decisões humanas nem sempre são matematicamente perfeitas.   

 --- 

### 1.1.1 Agir de forma humana: abordagem do teste de Turing

O teste de Turing, proposto por Alan Turing (1950), foi projetado como um experimento hipotético que deixaria de lado a vacuidade filosófica da questão "Uma máquina pode pensar?". Um computador passará no teste se um interrogador humano, depois de propor algumas perguntas por escrito, não conseguir descobrir se as respostas escritas vêm de uma pessoa ou de um computador. O Capítulo 27 discute os detalhes do teste e também se um computador seria de fato inteligente se passasse nele. Por enquanto, observamos que programar um computador para passar no teste já nos dá muito no que trabalhar. O computador precisaria ter as seguintes capacidades:   

- **Processamento de linguagem natural** para permitir que ele se comunique com sucesso em uma linguagem humana.   
- **Representação de conhecimento** para armazenar o que sabe ou ouve.   
- **Raciocínio automatizado** para responder a perguntas e tirar novas conclusões.   
- **Aprendizado de máquina** para se adaptar a novas circunstâncias e para detectar e extrapolar padrões.   

Turing enxergava a simulação física de uma pessoa como desnecessária para demonstrar inteligência. Entretanto, outros pesquisadores propuseram o chamado **teste de Turing total**, que exige interação com objetos e pessoas no mundo real. Para ser aprovado no teste de Turing total, um robô precisará de:   

- **Visão computacional** e **reconhecimento de fala** para perceber o mundo.   
- **Robótica** para manipular objetos e mover-se.   

Essas seis disciplinas compõem a maior parte da IA. Ainda assim, os pesquisadores da IA têm dedicado pouco esforço à aprovação no teste de Turing, acreditando que seja mais importante estudar os princípios básicos da inteligência. A busca pelo "voo artificial" teve sucesso quando engenheiros e inventores pararam de imitar os pássaros e começaram a usar túneis de vento para aprender sobre aerodinâmica. Os textos de engenharia aeronáutica não definem como objetivo de seu campo criar "máquinas que voem exatamente como pombos a ponto de poderem enganar até mesmo outros pombos".   

### 1.1.2 Pensar de forma humana: estratégia de modelagem cognitiva

Se pretendemos dizer que dado programa pensa como um ser humano, temos de ter alguma forma de determinar como os seres humanos pensam. Podemos aprender sobre o pensamento humano de três maneiras:   

- **Introspecção** - procurando captar nossos próprios pensamentos à medida que eles se desenvolvem.   
- **Experimentos psicológicos** - observando uma pessoa em ação.   
- **Imagens cerebrais** - observando o cérebro em ação.   

Assim que tivermos uma teoria da mente suficientemente precisa, será possível expressar a teoria como um programa de computador. Se o comportamento de entrada e saída do programa coincidir com o comportamento humano correspondente, teremos evidência de que alguns dos mecanismos do programa também podem operar nos seres humanos.   

Por exemplo, Allen Newell e Herbert Simon, que desenvolveram o GPS, o "**Resolvedor Geral de Problemas**" (do inglês General Problem Solver) (Newell e Simon, 1961), não se contentaram em fazer seu programa resolver problemas de modo correto. Eles estavam mais preocupados em comparar os passos de suas etapas de raciocínio aos passos de indivíduos humanos resolvendo os mesmos problemas. O campo interdisciplinar da **ciência cognitiva** reúne modelos computacionais da IA e técnicas experimentais da psicologia para construir teorias precisas e verificáveis a respeito dos processos de funcionamento da mente humana.   

A ciência cognitiva é um campo fascinante por si só, merecedora de diversos livros e de pelo menos uma enciclopédia (Wilson e Keil, 1999). Ocasionalmente, apresentaremos comentários a respeito de semelhanças ou diferenças entre técnicas de IA e cognição humana. Porém, a ciência cognitiva de verdade se baseia necessariamente na investigação experimental de seres humanos ou animais. Deixaremos esse assunto para outros livros à medida que supomos que o leitor tenha acesso somente a um computador para realizar experimentação.   

Nos primórdios da IA, frequentemente havia confusão entre as abordagens. Um autor argumentava que um algoritmo funcionava bem em uma tarefa e que, portanto, era um bom modelo de desempenho humano, ou vice-versa. Os autores modernos separam os dois tipos de afirmações; essa distinção permitiu que tanto a IA quanto a ciência cognitiva se desenvolvessem com maior rapidez.    

Os dois campos continuam a fertilizar um ao outro, principalmente na visão computacional, que incorpora evidências neurofisiológicas em modelos computacio-nais. Recentemente, a combinação de métodos de neuroimagem combinados a técnicas de aprendizado de máquina para analisar tais dados levou ao início da capacidade de "ler mentes" - isto é, averiguar o conteúdo semântico dos pensamentos íntimos de uma pessoa. Por sua vez, essa capacidade poderia lançar mais luz sobre como funciona a cognição humana.   

 --- 

### 1.1.3 Pensar racionalmente: abordagem das "leis do pensamento"

O filósofo grego **Aristóteles** foi um dos primeiros a tentar codificar o "pensamento correto", isto é, os processos de raciocínio irrefutáveis. Seus **silogismos** forneciam padrões para estruturas de argumentos que sempre resultavam em conclusões corretas ao receberem premissas corretas. O exemplo canônico começa com *Sócrates é um homem* e *todos os homens são mortais* e conclui que *Sócrates é mortal*. (Esse exemplo provavelmente se deve a ***Sextus Empiricus*** e não a Aristóteles.) Essas leis do pensamento deveriam governar a operação da mente; seu estudo deu início ao campo chamado **lógica**.   

Os lógicos do século XIX desenvolveram uma notação precisa para declarações sobre todos os tipos de objetos do mundo e sobre as relações entre eles. (Compare isso com a notação aritmética básica, que fornece apenas declarações a respeito de números.) Por volta de 1965, foram concebidos programas que, teoricamente, são capazes de resolver qualquer problema solucionável descrito em notação lógica. A chamada "**tradição logicista**" dentro da inteligência artificial almeja criar sistemas inteligentes a partir de tais programas.   

A lógica, como convencionalmente compreendida, exige um conhecimento do mundo que é certo - uma condição que, na realidade, raramente é alcançada. Simplesmente não conhecemos as regras, digamos, da política ou da guerra, da mesma forma como conhecemos as regras do xadrez ou da aritmética. A **teoria da probabilidade** preenche essa lacuna, permitindo um raciocínio rigoroso com informações incertas. Em princípio, ela permite a construção de um modelo abrangente de pensamento racional, que vai da informação perceptiva bruta à compreensão de como o mundo funciona e às previsões sobre o futuro. O que isso não faz é gerar um comportamento inteligente. Para isso, precisamos de uma teoria da ação racional.   
O pensamento racional, por si só, é insuficiente.   

### 1.1.4 Agir racionalmente: abordagem de agente racional

Um **agente** é simplesmente algo que age (a palavra agente vem do latim *agere*, que significa "fazer"). Certamente todos os programas de computador realizam alguma coisa, mas espera-se que um agente computacional faça mais: opere autonomamente, perceba seu ambiente, persista por um período de tempo prolongado, adapte-se a mudanças e seja capaz de criar e perseguir metas. Um **agente racional** é aquele que age para alcançar o melhor resultado ou, quando há incerteza, o melhor resultado esperado.   

Na abordagem de "leis do pensamento" para IA, foi dada ênfase a inferências corretas. As vezes, a realização de inferências corretas é uma parte daquilo que caracteriza um agente racional, porque uma das formas de agir racionalmente é raciocinar de modo lógico até a conclusão de que dada ação é a melhor e, depois, agir de acordo com essa conclusão. Por outro lado, existem modos de agir racionalmente que não se pode dizer que envolvem inferências. Por exemplo, afastar-se de um fogão quente é uma ação de reflexo, em geral mais bem-sucedida que uma ação mais lenta executada após cuidadosa deliberação.   
Todas as habilidades necessárias à realização do teste de Turing também permitem que o agente haja racionalmente:   

- **Representação do conhecimento** e **raciocínio** permitem que os agentes alcancem boas decisões   
- Precisamos ter a capacidade de gerar sentenças compreensíveis em **linguagem natural** para podermos participar de uma sociedade complexa   
- Precisamos **aprender** não apenas por erudição, mas também para melhorar nossa habilidade de gerar comportamento efetivo, especialmente em circunstâncias que são novas   

A abordagem do agente racional da IA tem duas vantagens sobre as outras abordagens:   

1. Ela é mais **genérica** que a abordagem de "leis do pensamento", visto que produzir inferência correta é apenas um entre vários mecanismos possíveis para se alcançar a racionalidade   
2. Ela é mais **acessível ao desenvolvimento científico**. O padrão de racionalidade é matematicamente bem definido e completamente geral   

Frequentemente podemos trabalhar a partir dessa especificação para derivar projetos de agentes que comprovadamente a alcançam - algo que é amplamente impossível se o objetivo for imitar o comportamento humano ou os processos de pensamento.   
Por causa disso, a abordagem do agente racional da IA tem prevalecido pela maior parte da história desse campo. Nas primeiras décadas, os agentes racionais eram baseados em alicerces lógicos e formavam planos definidos para se alcançarem objetivos específicos. Posteriormente, métodos baseados na **teoria da probabilidade** e **aprendizado de máquina** permitiram a criação de agentes que pudessem tomar decisões sob incerteza a fim de alcançar o melhor resultado esperado. Em resumo, a IA se concentra no estudo e na construção de agentes que fazem a coisa certa. Aquilo que é considerado a coisa certa é definido pelo objetivo que oferecemos ao agente. Esse paradigma geral é tão difundido que poderíamos chamá-lo de **modelo padrão**. Ele prevalece não apenas na IA, mas também:   

- Na **teoria de controle**, em que um controlador minimiza uma função de custo   
- Na **pesquisa operacional**, em que uma política maximiza uma soma de recompensas   
- Na **estatística**, em que uma regra de decisão minimiza uma função de perda   
- Na **economia**, em que um tomador de decisão maximiza a utilidade ou alguma medida de bem-estar social   

É preciso que se faça uma melhoria importante no modelo padrão para considerar o fato de que a **racionalidade perfeita** - sempre fazer a ação exatamente ótima - não é algo viável em ambientes complicados. As demandas computacionais são simplesmente muito elevadas. Os Capítulos 5 e 17 lidam com a questão da **racionalidade limitada** - agir de forma apropriada quando não existe tempo suficiente para realizar todas as computações que gostaríamos de fazer. No entanto, a racionalidade perfeita continua sendo um bom ponto de partida para a análise teórica.   

### 1.1.5 Máquinas benéficas

O modelo padrão tem constituído um guia útil para a pesquisa de IA desde o princípio, mas provavelmente não é o modelo certo a longo prazo. O motivo é que o modelo padrão pressupõe que daremos à máquina um objetivo totalmente especificado.   
Para uma tarefa definida artificialmente, como no xadrez ou no cálculo do caminho mais curto, a tarefa vem com um objetivo embutido - portanto, o modelo padrão é aplicável. Porém, ao avançarmos para o mundo real, torna-se cada vez mais dificil especificar o objetivo de forma completa e correta. Por exemplo, ao projetar um carro autônomo, pode-se pensar que o objetivo é chegar ao destino com segurança. Mas dirigir em qualquer estrada incorre em riscos de danos causados por outros motoristas errantes, falha de equipamento, e assim por diante; desse modo, uma meta estrita de segurança exigiria a permanência na garagem. Há um certo compromisso entre prosseguir em direção ao destino e incorrer em risco de lesão. Como esse compromisso deve ser feito? Além disso, até que ponto podemos permitir que o carro execute ações que afetariam o comportamento de outros motoristas? Até que ponto o carro deve ser moderado em sua aceleração, direção e frenagem, a fim de evitar sacudir o passageiro? Essas perguntas são dificeis de serem respondidas a priori. São particularmente problemáticas na área geral da interação humano-robô, de que o carro autônomo é um exemplo.   

O problema de chegar a um acordo entre nossas verdadeiras preferências e o objetivo que colocamos na máquina é chamado de **problema de alinhamento de valores**: Os valores ou objetivos colocados na máquina devem ser alinhados aos do ser humano. Se estivermos desenvolvendo um sistema de IA no laboratório ou em um simulador - como tem acontecido na maior parte da história nesse campo -, há uma solução fácil para um objetivo que foi especificado corretamente: reiniciar o sistema, corrigir o objetivo e tentar novamente. À medida que o campo avança em direção a sistemas inteligentes cada vez mais capazes, que são implantados no mundo real, essa abordagem torna-se inviável. Consequências negativas surgirão de um sistema implantado com um objetivo incorreto. Além disso, quanto mais inteligente for o sistema, mais negativas serão as consequências.   

Voltando ao exemplo aparentemente não problemático do xadrez, considere o que acontece se a máquina for inteligente o bastante para raciocinar e agir além dos limites do tabuleiro de xadrez. Nesse caso, ela pode tentar aumentar suas chances de vitória por meio de artimanhas como:   

- Hipnotizar ou chantagear seu oponente   
- Subornar o público para fazer barulho durante o período em que seu oponente está raciocinando **(3)**   
- Tentar adquirir mais poder de computação para si mesma   

*Esses comportamentos não são "não inteligentes" ou "insanos", são uma consequência lógica de definir a vitória como o único objetivo da máquina.*   

É impossível antecipar todas as maneiras pelas quais uma máquina que busca um objetivo fixo pode se comportar mal. Logo, há uma boa razão para pensar que o modelo padrão é inadequado. Não queremos máquinas que sejam inteligentes no sentido de perseguir seus objetivos; queremos que elas busquem os nossos objetivos. Se não pudermos transferir esses objetivos perfeitamente para a máquina, então precisamos de uma nova formulação - uma em que a máquina esteja perseguindo nossos objetivos, mas necessariamente não tenha certeza de quais são eles. Quando uma máquina sabe que não conhece o objetivo completo, ela tem um incentivo para:   

- Agir com cautela   
- Pedir permissão   
- Aprender mais sobre nossas preferências por meio da observação   
- Submeter-se ao controle humano   

Em última análise, queremos agentes comprovadamente benéficos para os humanos. Voltaremos a esse assunto na seção 1.5.

---

**Nota de rodapé 3**: Em um dos primeiros livros sobre o xadrez, Ruy Lopez (1561) escreveu: "Sempre coloque o tabuleiro de modo que o sol esteja contra os olhos do seu oponente."

---

## **1.2 Fundamentos da inteligência artificial**

Nesta seção, apresentaremos um breve histórico das disciplinas que contribuíram com ideias, pontos de vista e técnicas para a IA. Como qualquer histórico, este se concentra em um pequeno número de pessoas, eventos e ideias, ignorando outros que também seriam importantes.   

Organizamos o histórico em torno de uma série de perguntas. Certamente, não desejaríamos dar a impressão de que essas questões são as únicas de que as disciplinas tratam ou de que todas as disciplinas estejam se encaminhando para a IA como sua realização final.   

### 1.2.1 Filosofia

- **Regras formais podem ser usadas para obter conclusões válidas?**   
- **Como a mente (o intelecto) se desenvolve a partir de um cérebro físico?**   
- **De onde vem o conhecimento?**   
- **Como o conhecimento conduz à ação?**   

**Aristóteles** (384-322 a.C.) foi o primeiro a formular um conjunto preciso de leis que governam a parte racional da mente. Ele desenvolveu um sistema informal de silogismos para raciocinio apropriado que, em princípio, permitiam gerar conclusões mecanicamente, dadas as premissas iniciais.   

**Ramon Llull** (1232-1315) apresentou a ideia de um sistema de raciocínio publicado como **Ars Magna**, ou **A Grande Arte** (1305). Llull tentou implementar seu sistema utilizando um artefato mecânico real: um conjunto de rodas de papel que poderiam ser giradas de diferentes formas.   

Por volta de 1500, **Leonardo da Vinci** (1452-1519) projetou, mas não construiu, uma calculadora mecânica; reconstruções recentes mostraram que o projeto era funcional. A primeira máquina de calcular conhecida foi construída em torno de 1623 pelo cientista alemão **Wilhelm Schickard** (1592-1635). **Blaise Pascal** (1623-1662) construiu a pascaline em 1642 e escreveu que ela "produz efeitos que parecem mais próximos ao pensamento que todas as ações dos animais". **Gottfried Wilhelm Leibniz** (1646-1716) construiu um dispositivo mecânico destinado a efetuar operações sobre conceitos, e não sobre números, mas seu escopo era bastante limitado. Em seu livro de 1651, **Leviatã**, **Thomas Hobbes** (1588-1679) sugeriu a ideia de uma máquina pensante, um "animal artificial" em suas palavras, argumentando:   

> "Pois o que é o coração, senão uma mola; e os nervos, senão tantas cordas; e as articulações, senão tantas rodas."   

Ele também sugeriu que o raciocinio era como o cálculo numérico:   

> "Pois 'raciocinar' (...) é nada mais do que 'calcular', ou seja, somar e subtrair."   

Afirmar que a mente opera, pelo menos em parte, de acordo com regras lógicas ou numéricas, e construir sistemas físicos que emulam algumas dessas regras é uma coisa; outra é dizer que a mente em si é esse sistema físico. **René Descartes** (1596-1650) apresentou a primeira discussão clara da distinção entre mente e matéria. Ele observou que uma concepção puramente física da mente parece deixar pouco espaço para o livre-arbitrio. Se a mente é governada inteiramente por leis físicas, então ela não tem mais livre-arbitrio que uma pedra que "decide" cair em direção ao centro da Terra. Descartes era um proponente do **dualismo**. Ele sustentava que há uma parte da mente humana (ou alma, ou espírito) que transcende a natureza, isenta das leis físicas. Por outro lado, os animais não têm essa qualidade dual; eles poderiam ser tratados como máquinas.   

Uma alternativa para o dualismo é o **materialismo**, que sustenta que a operação do cérebro de acordo com as leis da física constitui a mente. O livre-arbitrio é simplesmente o modo como a percepção das escolhas disponíveis se mostra para a entidade que escolhe. Os termos **fisicalismo** e **naturalismo** também são usados para descrever essa visão, que se contrasta com o supernatural.   

Dada uma mente física que manipula o conhecimento, o problema seguinte é estabelecer a origem do conhecimento. O movimento chamado **empirismo**, iniciado a partir da obra de **Francis Bacon** (1561-1626), **Novum Organum**, **(4)** se caracterizou por uma frase de **John Locke** (1632-1704): "Não há nada na compreensão que não estivesse primeiro nos sentidos."   

A obra de **David Hume** (1711-1776) **A Treatise of Human Nature** (Tratado da Natureza Humana) (Hume, 1739) propôs aquilo que se conhece hoje como o **principio da indução**: as regras gerais são adquiridas pela exposição a associações repetidas entre seus elementos.   

Com base no trabalho de **Ludwig Wittgenstein** (1889-1951) e **Bertrand Russell** (1872-1970), o famoso **Círculo de Viena** (Sigmund, 2017), um grupo de filósofos e matemáticos reunidos em Viena nos anos 1920 e 1930, desenvolveu a doutrina do **positivismo lógico**. Essa doutrina sustenta que todo conhecimento pode ser caracterizado por teorias lógicas conectadas, em última análise, a sentenças de observação que correspondem a entradas sensoriais; desse modo, o positivismo lógico combina o racionalismo e o empirismo.   

A teoria da confirmação de **Rudolf Carnap** (1891-1970) e de **Carl Hempel** (1905-1997) tentava analisar a aquisição do conhecimento por meio da experiência, quantificando o grau de crença que deveria ser atribuído a sentenças lógicas com base em sua conexão com observações que as confirmem ou as contrariem. O livro de Carnap, **The Logical Structure of the World** (1928), provavelmente foi a primeira teoria da mente como um processo computacional.   
O último elemento no quadro filosófico da mente é a conexão entre conhecimento e ação. Esta questão é vital para a lA porque a inteligência exige ação, bem como raciocinio. Além disso, apenas pela compreensão de como as ações são justificadas podemos compreender como construir um agente cujas ações sejam justificáveis (ou racionais).   

**Aristóteles** argumentava (em **De Mot Animalium**) que as ações se justificam por uma conexão lógica entre metas e conhecimento do resultado da ação:   

> Porém, como explicar que o pensamento às vezes esteja acompanhado pela ação e às vezes nao de es e di no manhado pedi moviment e as vezes ter as sore objecos imutáveis. Contudo, nesse caso o fim é uma proposição especulativa (...) enquanto aqui a conclusão que resulta das duas premissas é uma ação. (...) Preciso me cobrir; um casaco é uma coberta. Preciso de um casaco. O que eu preciso. tenho de fazer: preciso de um casaco. Tenho de fazer um casaco. E a conclusão, "tenho de fazer um casaco", é uma ação.   

Na obra **Érica a Nicômaco** (Livro III. 3, 1112b), Aristóteles desenvolve esse tópico um pouco mais, sugerindo um algoritmo:
---

**Nota de rodapé 4**: O Novum Organum é uma atualização do Organon de Aristóteles, ou instrumento de pensamento.

---

> Não deliberamos sobre os fins, mas sobre os meios. Um médico não delibera sobre se deve ou não curar, nem um orador sobre se deve ou não persuadir (...). Eles dão a finalidade por estabelecida e procuram saber a maneira de alcançá-la; se lhes parece poder ser alcançada por vários meios, procuram saber o mais fácil e o mais eficaz; e se há apenas um meio para alcançá-la, procuram saber como será alcançada por esse meio e por que outro meio alcançar esse primeiro, até chegar ao primeiro principio, que é o último na ordem de descoberta, (...) e o que vem em último lugar na ordem da análise parece ser o primeiro na ordem da execução. E, se chegarmos a uma impossibilidade, abandonamos a busca; por exemplo, se precisarmos de dinheiro e não for possível consegui-lo; porém, se algo parecer possível, tentaremos realizá-lo.   

O algoritmo de **Aristóteles** foi implementado 2300 anos mais tarde, por **Newell** e **Simon**, em seu programa **General Problem Solver** (Resolvedor Geral de Problemas). Agora, poderíamos denominá-lo sistema de planejamento regressivo guloso (ver Capítulo 11). Os métodos baseados no planejamento lógico para se chegar a metas definidas dominaram as primeiras poucas décadas da pesquisa teórica em IA.   
Pensar apenas em termos de ações para atingir metas costuma ser útil, mas às vezes inaplicável. Por exemplo, se houver várias maneiras diferentes de atingir uma meta, deve haver alguma maneira de escolher entre elas. Mais importante, pode não ser possível atingir uma meta com certeza, mas ainda assim alguma ação deve ser tomada. Como então se deve decidir? **Antoine Arnauld** (1662), analisando a noção de decisões racionais em jogos de azar, propôs uma fórmula quantitativa para maximizar o valor monetário esperado do resultado. Mais tarde, **Daniel Bernoulli** (1738) introduziu a noção mais geral de utilidade para capturar o valor interno e subjetivo de um resultado. A noção moderna de tomada de decisão racional sob incerteza envolve a maximização da utilidade esperada, conforme explicado no Capítulo 16.   

Em questões de ética e políticas públicas, um tomador de decisão precisa considerar os interesses de vários indivíduos. **Jeremy Bentham** (1823) e **John Stuart Mill** (1863) promoveram a ideia do **utilitarismo**: que a tomada de decisão racional, baseada na maximização da utilidade, deve se aplicar a todas as esferas da atividade humana, incluindo as decisões de política pública feitas em nome de muitos indivíduos. O utilitarismo é um tipo específico de **consequencialismo**: a ideia de que o que é certo e errado é determinado pelos resultados esperados de uma ação.  

Por outro lado, **Immanuel Kant** propôs em 1875 uma teoria da ética baseada em regras ou **ética deontológica**, em que "fazer a coisa certa" é determinado não por resultados, mas por leis sociais universais que regem as ações permitidas, como "não mentir" ou "não matar". Assim, um utilitarista poderia contar uma mentira inocente se o bem esperado superasse o mal, mas um kantiano não o faria, porque mentir é inerentemente errado. Mill reconheceu o valor das regras, mas as entendeu como procedimentos de decisão eficientes, compilados a partir do raciocínio dos princípios sobre as consequências. Muitos sistemas modernos de IA adotam exatamente essa abordagem.   

### 1.2.2 Matemática

- **Quais são as regras formais para obter conclusões válidas?**   
- **O que pode ser computado?**   
- **Como raciocinamos com informações incertas?**   

Os filósofos demarcaram algumas das ideias fundamentais sobre IA, mas o salto para uma ciência formal exigiu certo nível de formalização matemática da lógica e da probabilidade e a introdução de um novo ramo da matemática: a computação.   
A ideia de **lógica formal** pode ser ligada aos filósofos da Grécia antiga, Índia e China, mas seu desenvolvimento matemático começou realmente com o trabalho de **George Boole** (1815-1864), que definiu os detalhes da **lógica proposicional** ou **lógica booleana** (Boole, 1847). Em 1879, **Gottlob Frege** (1848-1925) estendeu a lógica de Boole para incluir objetos e relações, criando a **lógica de primeira ordem** que é utilizada hoje. **(5)**   
Além do seu papel central no periodo inicial da pesquisa em IA, a lógica de primeira ordem motivou o trabalho de Gödel e Turing, que sustentou a própria computação, conforme explicamos a seguir.   

A **teoria da probabilidade** pode ser vista como uma lógica generalizadora para situações com informações incertas - uma consideração de grande importância para a lA. **Gerolamo Cardano** (1501-1576) formulou inicialmente a ideia de probabilidade, descrevendo-a em termos dos resultados possíveis de eventos de jogos de azar. Em 1654, **Blaise Pascal** (1623-1662), em uma carta para **Pierre Fermat** (1601-1665), mostrou como predizer o futuro de um jogo de azar inacabado e atribuir recompensas médias aos jogadores. A probabilidade se transformou rapidamente em uma parte valiosa de todas as ciências quantitativas, ajudando a lidar com medidas incertas e teorias incompletas. **Jacob Bernoulli** (1654-1705, tio de Daniel), **Pierre Laplace** (1749-1827) e outros pesquisadores aperfeiçoaram a teoria e introduziram novos métodos estatísticos. **Thomas Bayes** (1702-1761) propôs uma regra para atualizar probabilidades à luz de novas evidências; a **regra de Bayes** é uma ferramenta fundamental para os sistemas de IA.   

A formalização da probabilidade, combinada com a disponibilidade de dados, levou ao surgimento da estatística como um campo. Um dos primeiros usos foi a análise de **John Graunt** dos dados do censo de Londres, em 1662. **Ronald Fisher** é considerado o primeiro estatístico moderno (Fisher, 1922). Ele reuniu as ideias de probabilidade, planejamento de experimentos, análise de dados e computação - em 1919, ele insistiu que não poderia fazer seu trabalho sem uma calculadora mecânica chamada **MILLIONAIRE** (a primeira calculadora que podia fazer multiplicação), embora o custo da calculadora fosse maior do que seu salário anual (Ross, 2012).   
A história da computação é tão antiga quanto a história dos números, mas acredita-se que o primeiro algoritmo não trivial tenha sido o **algoritmo de Euclides** para calcular o maior divisor comum. A palavra **algoritmo** vem de **Muhammad ibn Musa al-Khwarizmi**, um matemático do século IX, cujos escritos também introduziram os numerais arábicos e a álgebra na Europa. Boole e outros discutiram algoritmos para dedução lógica e, no fim do século XIX, foram empreendidos esforços para formalizar o raciocínio matemático geral como dedução lógica.   

**Kurt Gödel** (1906-1978) mostrou que existe um procedimento efetivo para provar qualquer afirmação verdadeira na lógica de primeira ordem de Frege e Russell, mas que essa lógica não poderia capturar o princípio de indução matemática necessário para caracterizar os números naturais. Em 1931, Gödel mostrou que existem de fato limites sobre a dedução. Seu **teorema da incompletude** mostrou que, em qualquer teoria formal tão forte como a aritmética de Peano (a teoria elementar dos números naturais), existem afirmações necessariamente verdadeiras que não têm provas na teoria.   

Esse resultado fundamental também pode ser interpretado como a demonstração de que existem funções sobre os inteiros que não podem ser representadas por um algoritmo, isto é, não podem ser calculadas. Isso motivou **Alan Turing** (1912-1954) a tentar caracterizar exatamente que funções são computáveis - capazes de ser calculadas por um procedimento efetivo.   

A **tese de Church-Turing** propõe identificar a noção geral da computabilidade com funções calculadas por uma **máquina de Turing** (Turing, 1936). Turing também mostrou que existiam algumas funções que nenhuma máquina de Turing poderia calcular. Por exemplo, nenhuma máquina pode determinar, de forma geral, se dado programa retornará uma resposta sobre certa entrada ou se continuará sendo executado para sempre.   

Embora a computabilidade seja importante para a compreensão da computação, a noção de **tratabilidade** teve um impacto muito maior sobre a IA. Em termos gerais, um problema é chamado de **intratável** se o tempo necessário para resolver instâncias dele cresce exponencialmente com o tamanho das instâncias. A distinção entre crescimento polinomial e exponencial da complexidade foi enfatizada primeiro em meados da década de 1960 (Cobham, 1964; Edmonds, 1965). Ela é importante porque o crescimento exponencial significa que até mesmo instâncias moderadamente grandes não podem ser resolvidas em qualquer tempo razoável.   

A teoria da **NP-completude**, apresentada primeiro por Cook (1971) e Karp (1972), fornece uma base para analisar a tratabilidade dos problemas: qualquer classe de problemas à qual a classe de problemas NP-completos pode ser reduzida provavelmente é intratável. (Embora não tenha sido provado que problemas NP-completos são necessariamente intratáveis, a maioria dos teóricos acredita nisso.) Esses resultados contrastam com o otimismo com que a imprensa popular saudou os primeiros computadores - "Supercérebros eletrônicos" que eram "Mais rápidos que Einstein!". Apesar da crescente velocidade dos computadores, o uso parcimonioso de recursos e a necessidade de imperfeição é que caracterizarão os sistemas inteligentes.   

**Grosso modo, o mundo é uma instância de um problema extremamente grande!**

 --- 

**Nota de rodapé 5**: A notação proposta por Frege para a lógica de primeira ordem - uma combinação enigmática de aspectos textuais e geométricos - nunca se tornou popular.

---

### 1.2.3 Economia

- Como devemos tomar decisões de acordo com nossas preferências?   
- Como devemos fazer isso quando outros não podem nos acompanhar?   
- Como devemos fazer isso quando a recompensa pode estar distante no futuro?   

A ciência da economia teve início em 1776, quando Adam Smith (1723-1790) publicou **An Inquiry into the Nature and Causes of the Wealth of Nations** (Uma Investigação sobre a Natureza e as Causas da Riqueza das Nações). Smith propôs que as economias consistiam em agentes individuais atendendo aos seus próprios interesses. Porém, Smith não defendia a ganância financeira como uma posição moral: seu livro anterior (1759), **The Theory of Moral Sentiments** (A Teoria dos Sentimentos Morais) começa indicando que a preocupação com o bem-estar de outros é um componente essencial dos interesses de cada indivíduo.   

A maioria das pessoas pensa que a economia trata de dinheiro e, de fato, a primeira analise matemática de decisões sob incerteza, a fórmula do valor máximo esperado de Arnauld (1662), tratava do valor monetário de apostas. Daniel Bernoulli (1738) notou que essa formula não parecia funcionar bem para grandes quantias de dinheiro, como investimentos em expedições comerciais marítimas. Em vez disso, ele propôs um princípio baseado na maximização da utilidade esperada e explicou as escolhas de investimento humano ao propor que a utilidade marginal de uma quantidade adicional de dinheiro diminuía à medida que se adquiria mais dinheiro.   

Léon Walras (pronuncia-se "Valrasse") (1834-1910) deu à teoria da utilidade uma base mais genérica em termos de preferências entre apostas sobre quaisquer resultados (não apenas resultados monetários). A teoria foi aperfeiçoada por Ramsey (1931) e, mais tarde, por John von Neumann e Oskar Morgenstern em seu livro **The Theory of Games and Economic Behavior** (A Teoria dos Jogos e o Comportamento Econômico) (1944). A economia não é mais o estudo do dinheiro; antes, é o estudo dos desejos e das preferências.   

A teoria da decisão, que combina a teoria da probabilidade com a teoria da utilidade, fornece uma estrutura formal e completa para decisões (econômicas ou outras) tomadas sob incerteza, ou seja, em casos nos quais as descrições probabilísticas captam de forma apropriada o ambiente do tomador de decisões. Isso é adequado para "grandes" economias em que cada agente não precisa levar em conta as ações de outros agentes como indivíduos. No caso das "pequenas" economias, a situação é muito mais parecida com um jogo: as ações de um jogador podem afetar de forma significativa a utilidade de outro (positiva ou negativamente). O desenvolvimento da teoria dos jogos por Von Neumann e Morgenstern (consulte também Luce e Raiffa, 1957) incluiu o surpreendente resultado de que, em alguns jogos, um agente racional deve adotar políticas que são (ou pelo menos parecem ser) aleatórias. Ao contrário da teoria da decisão, a teoria dos jogos não oferece uma receita inequívoca para a seleção de ações. Na IA, as decisões que envolvem vários agentes são estudadas sob o título de sistemas multiagentes (Capítulo 18).   

Os economistas, com algumas exceções, não trataram a terceira questão da listagem anterior, ou seja, como tomar decisões racionais quando as recompensas das ações não são imediatas, mas resultam de várias ações executadas em sequência. Esse tópico foi adotado no campo de pesquisa operacional, que emergiu na Segunda Guerra Mundial dos esforços britânicos para otimizar instalações de radar e, mais tarde, encontrou inúmeras aplicações civis. O trabalho de Richard Bellman (1957) formalizou uma classe de problemas de decisões sequenciais chamados de processos de decisão markovianos, que estudaremos no Capítulo 17 e, sob o título de aprendizagem por reforço, no Capítulo 22.   

O trabalho em economia e pesquisa operacional contribuiu muito para nossa noção de agentes racionais, ainda que por muitos anos a pesquisa em IA se desenvolvesse ao longo de caminhos inteiramente separados. Uma razão para isso era a aparente complexidade da tomada de decisões racionais. Herbert Simon (1916-2001), pesquisador pioneiro da lA, ganhou o Prêmio Nobel de Economia em 1978 por seu trabalho inicial demonstrando que modelos baseados em satisfação (do inglês **satisficing**, também traduzido como satisfazimento) - a tomada de decisões "boas o suficiente", em vez de calcular laboriosamente uma decisão ótima - forneciam uma descrição melhor do comportamento humano real (Simon, 1947). Desde os anos 1990, ressurgiu o interesse pelas técnicas da teoria da decisão para sistemas de IA.   

### 1.2.4 Neurociência

- Como o cérebro processa informações?   

Neurociência é o estudo do sistema nervoso, em particular do cérebro. Apesar de o modo exato como o cérebro habilita o pensamento ser um dos grandes mistérios da ciência, o fato de ele habilitar o pensamento foi avaliado por milhares de anos devido à evidência de que pancadas fortes na cabeça podem levar à incapacitação mental. Também se sabe há muito tempo que o cérebro dos seres humanos tem algumas características diferentes; em aproximadamente 335 a.C., Aristóteles escreveu: "De todos os animais, o homem é o que tem o maior cérebro em proporção ao seu tamanho." **(6)** Ainda assim, apenas em meados do século XVIII o cérebro foi amplamente reconhecido como a sede da consciência. Antes disso, acreditava-se que a sede da consciência poderia estar localizada no coração e no baço.   

O estudo da afasia (deficiência da fala) feito por Paul Broca (1824-1880) em 1861, com pacientes cujo cérebro foi danificado, iniciou a pesquisa da organização funcional do cérebro, identificando uma área localizada no hemisfério cerebral esquerdo - agora chamada de "área de Broca" - responsável pela produção da fala². Nessa época, sabia-se que o cérebro consistia em grande parte de células nervosas ou neurônios, mas apenas em 1873 Camillo Golgi (1843-1926) desenvolveu uma técnica de coloração que permitiu a observação de neurônios individuais no cérebro (Figura1.1). Essa técnica foi usada por Santiago Ramon y Cajal (1852-1934) em seus estudos pioneiros da organização de neurônios no cérebro. **(8)**    
Agora, aceita-se que as funções cognitivas resultam da operação eletroquímica dessas estruturas. Ou seja, uma coleção de células simples pode levar ao pensamento, à ação e à consciência. Nas palavras enérgicas de John Searle (1992), os cérebros causam mentes.   

Atualmente, temos alguns dados sobre o mapeamento entre áreas do cérebro e as partes do corpo que elas controlam ou das quais recebem entrada sensorial. Tais mapeamentos podem mudar radicalmente no curso de algumas semanas, e alguns animais parecem ter vários mapas. Além disso, não compreendemos inteiramente como outras áreas do cérebro podem assumir o comando de certas funções quando uma area é danificada. Praticamente não há teoria que explique como a memória de um indivíduo é armazenada ou como as funções cognitivas de nível mais alto operam.   

A medição da atividade de cérebros intactos teve início em 1929, com a invenção do eletroencefalógrafo (EEG) por Hans Berger. O desenvolvimento recente da técnica de ressonância magnética funcional (fMRI - functional magnetic resonance imaging) (Ogawa et al., 1990; Cabeza e Nyberg, 2001) está dando aos neurocientistas imagens sem precedentes de detalhes da atividade cerebral, tornando possíveis medições que correspondem, em aspectos interessantes, a processos cognitivos em ação. Essas medições são ampliadas por avanços na gravação da atividade dos neurônios em uma única célula e pelos métodos de optogenética (Crick, 1999; Zemelman et al., 2002; Han e Boyden, 2007), permitindo a medição e o controle de neurônios individuais modificados para que sejam sensíveis à luz.   

O desenvolvimento de interfaces cérebro-máquina (Lebedev e Nicolelis, 2006) para detecção e controle motor não apenas promete restaurar a função de indivíduos com deficiência, mas também esclarece muitos aspectos dos sistemas neurais. Uma descoberta importante desse trabalho é que o cérebro consegue se adaptara para interagir com sucesso com um dispositivo externo, tratando-o por fim como se fosse outro órgão ou membro sensorial.   

 --- 

**Nota de rodapé 6:** Desde então, foi descoberto que o musaranho (Scandentia) e algumas espécies de pássaros têm alta proporção de cérebro em relação à massa corporal.   
**Nota de rodapé 7:** Muitos citam Alexander Hood (1824) como possível fonte anterior.   
**Nota de rodapé 8:** Golgi persistiu em sua convicção de que as funções do cérebro eram executadas principalmente em um meio contínuo no qual os neurônios estavam incorporados, enquanto Cajal propunha a "doutrina neuronal". Os dois compartilharam o Prêmio Nobel em 1906, mas pronunciaram discursos mutuamente antagônicos ao aceitá-lo.   

 --- 

<img src="https://raw.githubusercontent.com/nicolas-oliveira/roadmap-IA-paradigmas/refs/heads/main/week00-introduction/4%C2%AA%20Edi%C3%A7%C3%A3o%20-%20Introdu%C3%A7%C3%A3o%20%C3%A0%20Intelig%C3%AAncia%20Artificial%20Peter%20e%20Novrig/files/a59556c96fb335bc9d1d188d3846d2d8838e047fec07e5.png" title="" alt="a59556c96fb335bc9d1d188d3846d2d8838e047fec07e507428d4001c2e48d6d" data-align="center">   

> Figura 1.2 Partes de uma célula nervosa ou neurônio. Cada neurônio consiste em um corpo celular ou soma, que contém um núcleo celular. Ramificando-se a partir do corpo celular, há uma série de fibras chamadas dendritos e uma única fibra longa chamada axônio. O axônio se estende por uma longa distância, muito mais longa do que indica a escala desse diagrama. Em geral, um axônio têm 1cm de comprimento (100 vezes o diâmetro do corpo celular), mas pode alcançar até 1 metro. Um neurônio faz conexões com 10-100.000 outros neurônios, em junções chamadas sinapses. 

> Os sinais se propagam de um neurônio para outro por meio de uma complicada reação eletroquímica. Os sinais controlam a atividade cerebral em curto prazo e também permitem mudanças a longo prazo na posição e na conectividade dos neurônios. Acredita-se que esses mecanismos formem a base para o aprendizado no cérebro. A maior parte do processamento de informações ocorre no córtex cerebral,a camada exterior do cérebro. A unidade organizacional básica parece ser uma coluna de tecido com aproximadamente 0,5 mm de diâmetro, contendo cerca de 20.000 neurônios e estendendo-se por toda a profundidade do córtex, cerca de 4 mm nos seres humanos.   

De certa maneira, cérebros e computadores digitais têm propriedades diferentes. A Figura 1.2 mostra que os computadores têm um tempo de ciclo que é 1 milhão de vezes mais rápido que o cérebro. O cérebro compensa isso tendo muito mais capacidade de armazenamento e interconexões que um computador pessoal de última geração, apesar de os maiores supercomputadores apresentarem capacidade similar à do cérebro em algumas métricas. Os futuristas enaltecem demais esses números, apontando para a proximidade de uma singularidade em que os computadores alcançariam um nível sobre-humano de desempenho (Vinge, 1993; Kurzweil, 2005; Doctorow e Stross, 2012), e então rapidamente se melhorariam ainda mais. Porém, as comparações numéricas cruas não são especialmente informativas. Mesmo com um computador de capacidade virtualmente ilimitada, ainda precisamos de mais avanços conceituais em nossa compreensão da inteligência (ver Capítulo 28). Colocado de forma grosseira, sem a teoria certa, máquinas mais rápidas apenas dão a resposta errada mais rapidamente.   

### 1.2.5 Psicologia

- Como os seres humanos e os animais pensam e agem?   

Normalmente, considera-se que as origens da psicologia científica remontam ao trabalho do físico alemão Hermann von Helmholtz (1821-1894) e de seu aluno Wilhelm Wundt (1832-1920). Helmholtz aplicou o método científico ao estudo da visão humana, e seu **Handbook of Physiological Optics** (Manual de Óptica Fisiológica) é descrito até hoje como "o mais importante tratado sobre a física e a fisiologia da visão humana" (Nalwa, 1993, p. 15). Em 1879, Wundt abriu o primeiro laboratório de psicologia experimental na Universidade de Leipzig. Ele insistia em experimentos cuidadosamente controlados, nos quais seus colaboradores executariam uma tarefa perceptiva ou associativa enquanto refletiam sobre seus processos de pensamento.   
O controle cuidadoso percorreu um longo caminho para transformar a psicologia em ciência, mas a natureza subjetiva dos dados tornava improvável que um pesquisador divergisse de suas próprias teorias.   

|                             | Supercomputador                | Computador pessoal             | Mente humana                  |
| --------------------------- | ------------------------------ | ------------------------------ | ----------------------------- |
| Unidades computacionais     | 10⁴ CPUs, 10¹² transistores    | 4 CPUs, 10⁹ transistores       | 10¹¹ neurônios                |
| Unidades de armazenamento   | 10¹⁴ bits RAM, 10¹⁵ bits disco | 10¹¹ bits, 10¹³ RAM bits disco | 10¹¹ neurônios, 10¹⁴ sinapses |
| Tempo de ciclo              | 10⁻⁹ seg                       | 10⁻⁹ seg                       | 10⁻³ seg                      |
| Operações/seg               | 10¹⁵                           | 10¹⁰                           | 10¹⁷                          |
| Atualizações de memória/seg | 10¹⁴                           | 10¹⁰                           | 10¹⁴                          |

> Figura 1.2 Comparação grosseira dos recursos computacionais de um supercomputador, o Summit (Feld-man, 2017), um computador pessoal típico de 2019 e o cérebro humano. A potência do cérebro humano não mudou muito em milhares de anos, enquanto os supercomputadores passaram de megaLOPs nos anos 1960 para gigaFLOPs nos anos 1980, teraFLOPs nos anos 1990, petaFLOPs em 2008 e exaFLOPs em 2018   

> (1 exaFLOP =10^18 operações de ponto flutuante por segundo).   

Por outro lado, os biólogos que estudavam o comportamento animal careciam de dados introspectivos e desenvolveram uma metodologia objetiva, conforme descreveu H. S. Jennings (1906) em seu influente trabalho **Behavior of the Lower Organisms** (Comportamento dos Organismos Inferiores). Aplicando esse ponto de vista aos seres humanos, o movimento chamado behaviorismo, liderado por John Watson (1878-1958), rejeitava qualquer teoria que envolvesse processos mentais com a premissa de que a introspecção não poderia fornecer evidência confiável. Os behavioristas insistiam em estudar apenas medidas objetivas dos ***perceptos*** (ou estímulos) dados a um animal e suas ações resultantes (ou respostas). O behaviorismo descobriu muito sobre ratos e pombos, mas teve menos sucesso na compreensão dos seres humanos.   

A visão do cérebro como um dispositivo de processamento de informações, uma característica importante da psicologia cognitiva, tem suas origens nos trabalhos de William James (1842-1910). Helmholtz também insistiu em que a percepção envolvia uma forma de inferência lógica inconsciente. O ponto de vista cognitivo foi em grande parte eclipsado pelo behaviorismo nos EUA, mas na Unidade de Psicologia Aplicada de Cambridge, dirigida por Frederic Bartlett (1886-1969), a modelagem cognitiva foi capaz de florescer. **The Nature of Explanation** (A Natureza da Explicação), de Kenneth Craik (1943), aluno e sucessor de Bartlett, restabeleceu com vigor a legitimidade de termos "mentais" como crenças e objetivos, argumentando que eles são tão científicos quanto, digamos, usar a pressão e a temperatura ao falar sobre gases, apesar de estes serem constituídos por moléculas que não têm nenhuma dessas duas propriedades.   
Craik especificou os três passos fundamentais de um agente baseado em conhecimento:   

1. o estímulo deve ser traduzido em uma representação interna,   
2. a representação é manipulada por processos cognitivos para derivar novas representações internas, e   
3. por sua vez, essas representações são de novo traduzidas em ações.   

Craik explicou com clareza por que esse era um bom projeto de um agente:   

> Se o organismo transporta um "modelo em escala reduzida" da realidade externa e de suas próprias ações possíveis dentro de sua cabeça, ele é capaz de experimentar várias alternativas, concluir qual a melhor delas, reagir a situações futuras antes que elas surjam, utilizar o conhecimento de eventos passados para lidar com o presente e o futuro e, em todos os sentidos, reagir de maneira muito mais completa, segura e competente às emergências que enfrenta (Craik, 1943).   

Após a morte de Craik em um acidente de bicicleta em 1945, seu trabalho teve continuidade com Donald Broadbent, cujo livro **Perception and Communication** (Percepção e Comunicação), de 1958, foi um dos primeiros trabalhos a modelar fenômenos psicológicos como processamento de informações. Enquanto isso, nos EUA, o desenvolvimento da modelagem computacional levou à criação do campo da ciência cognitiva. Pode-se dizer que o campo teve início em um seminário em setembro de 1956 no MIT - apenas 2 meses após a conferência em que a própria IA "nasceu".   

No seminário, George Miller apresentou **The Magic Number Seven** (O Número Mágico Sete), Noam Chomsky apresentou **Three Models of Language** (Três Modelos de Linguagem) e Allen Newell e Herbert Simon apresentaram **The Logic Theory Machine** (A Máquina de Teoria Lógica). Esses três artigos influentes mostraram como modelos computacionais poderiam ser usados para tratar a psicologia da memória, a linguagem e o pensamento lógico, respectivamente. Agora é comum entre os psicólogos (embora não de forma universal) a visão de que "uma teoria cognitiva deve ser como um programa de computador" (Anderson, 1980); ou seja, ela deve descrever a operação de uma função cognitiva em termos de processamento de informações.   

Para os fins desta revisão, contaremos o campo da interação homem-computador (IHC) como subcampo da psicologia. Doug Engelbart, um dos pioneiros da IHC, defendeu a ideia de **aumento de inteligência** - AI em vez de IA. Ele acreditava que os computadores deveriam aumentar as habilidades humanas em vez de automatizar as tarefas humanas. Em 1968, a "mãe de todas as demonstrações" de Engelbart exibiu pela primeira vez o mouse do computador, um sistema de janelas, hipertexto e videoconferência - tudo em um esforço para demonstrar o que os trabalhadores do conhecimento humano poderiam coletivamente realizar com algum aumento de inteligência.   
Hoje, é mais provável que vejamos AI e IA como dois lados da mesma moeda, com o primeiro enfatizando o controle humano e o último enfatizando o comportamento inteligente por parte da máquina. Ambos são necessários para que as máquinas sejam úteis aos humanos.   

Claro! Abaixo está o conteúdo transformado em formato Markdown, com negrito, listas e subtítulos, conforme solicitado — sem alterar nem adicionar ou remover informações:   

 --- 

### **1.2.6 Engenharia de computadores**

• Como podemos construir um computador eficiente?   
O computador eletrônico digital moderno foi criado independentemente e quase ao mesmo tempo por cientistas de três países que participavam da Segunda Guerra Mundial. O primeiro computador ***operacional*** foi a máquina eletromecânica de Heath Robinson, **(9)** construída em 1943 pela equipe de Alan Turing com um único propósito: decifrar mensagens alemãs. Em 1943, o mesmo grupo desenvolveu o Colossus, uma poderosa máquina de uso geral baseada em válvulas eletrônicas. **(10)**   

O primeiro computador ***programável*** operacional foi o Z-3, criado por Konrad Zuse na Alemanha, em 1941. Zuse também inventou os números de ponto flutuante e a primeira linguagem de programação de alto nível, denominada Plankalkül.    
O primeiro computador ***eletrônico***, o ABC, foi montado por John Atanasoff e por seu aluno Clifford Berry, entre 1940 e 1942, na Universidade Estadual de Iowa. A pesquisa de Atanasoff recebeu pouco apoio ou reconhecimento; foi o ENIAC, desenvolvido como parte de um projeto militar secreto na Universidade da Pensilvânia por uma equipe que incluía John Mauchly e J. Presper Eckert, que provou ser o precursor mais influente dos computadores modernos.   
Desde aquele tempo, cada geração de ***hardware*** de computador trouxe aumento em velocidade e capacidade, e redução no preço – uma tendência explicada pela lei de Moore. O desempenho duplicou a cada 18 meses aproximadamente, até por volta de 2005, quando os problemas de dissipação de energia levaram os fabricantes a começar a multiplicação do número de núcleos de CPU e não a velocidade de clock. Espera-se, atualmente, que futuros aumentos de funcionalidade venham de um paralelismo maciço – uma convergência curiosa com as propriedades do cérebro. Também vemos novos projetos de ***hardware*** baseados na ideia de que, ao lidar com um mundo incerto, não necessitamos de 64 bits de precisão em nossos números; apenas 16 bits (como no formato `bfloat 16`) ou mesmo 8 bits serão suficientes e permitirão um processamento mais rápido.   
Estamos apenas começando a ver hardware ajustado para aplicativos de IA, como a unidade de processamento gráfico (GPU), a unidade de processamento tensorial (TPU), o motor em escala de wafer (WSE).   
Desde a década de 1960 até cerca de 2012, a quantidade de capacidade de computação usada para treinar as principais aplicações de aprendizado de máquina seguiu a lei de Moore. A partir de 2012, as coisas mudaram: de 2012 a 2018, houve um aumento de 300 mil vezes, o que significa uma duplicação a cada 100 dias, mais ou menos (Amodei e Hernandez, 2018). Um modelo de aprendizado de máquina que levava um dia inteiro para treinar em 2014 precisava de apenas dois minutos em 2018 (Ying et al., 2018).   

Embora ainda não seja prática, a computação quântica oferece a promessa de acelerações muito maiores para algumas subclasses importantes de algoritmos de IA.   
É claro que existiam dispositivos de cálculo antes do computador eletrônico. As primeiras máquinas automatizadas, datando do século XVII, foram descritas anteriormente. A primeira máquina programável foi um tear criado em 1805 por Joseph Marie Jacquard (1752–1834), que utilizava cartões perfurados para armazenar instruções relativas ao padrão a ser tecido.   

Na metade do século XIX, Charles Babbage (1792–1871) projetou duas máquinas de cálculo, mas não concluiu nenhuma delas:   

- A “máquina diferencial” se destinava a calcular tabelas matemáticas para projetos de engenharia e científicos. Ela foi finalmente construída e se mostrou funcional em 1991 (Swade, 2000).   
- A “Máquina Analítica” de Babbage era bem mais ambiciosa: ela incluía memória endereçável, programas armazenados baseados nos cartões perfurados de Jacquard e saltos condicionais. Foi a primeira máquina capaz de executar computação universal.   

A colega de Babbage, Ada Lovelace, filha do poeta Lord Byron, compreendeu seu potencial, descrevendo-a como:   

> “uma máquina de pensar ou (…) raciocinar”, capaz de raciocinar sobre “todos os assuntos no universo” (Lovelace, 1843).   

Ela também antecipou as ondas de “oba-oba” da IA, escrevendo:   

> “É preciso prevenir-se contra a possibilidade de ideias exageradas que possam surgir quanto aos poderes da Máquina Analítica.”   

Infelizmente, as máquinas de Babbage e as ideias de Lovelace foram, em grande parte, esquecidas.   

A IA também tem uma dívida com a área de software da ciência da computação, que forneceu os sistemas operacionais, as linguagens de programação e as ferramentas necessárias para escrever programas modernos (e artigos sobre eles). Porém, essa é uma área em que a dívida foi paga: o trabalho em IA foi pioneiro em muitas ideias que foram aproveitadas posteriormente na ciência da computação em geral, incluindo:   

- compartilhamento de tempo,   
- interpretadores interativos,   
- computadores pessoais com janelas e mouse,   
- ambientes de desenvolvimento rápido,   
- listas ligadas,   
- gerenciamento automático de armazenamento,   
- conceitos fundamentais de programação simbólica, funcional, declarativa e orientada a objetos.

---

### **1.2.7 Teoria de controle e cibernética**

• Como os artefatos podem operar sob seu próprio controle?   

Ctesíbio de Alexandria (cerca de 250 a.C.) construiu a primeira máquina autocontrolada: um relógio de água com um regulador que mantinha uma taxa de fluxo constante. Essa invenção mudou a definição do que um artefato poderia fazer. Antes, somente os seres vivos podiam modificar seu comportamento em resposta a mudanças no ambiente.   

Outros exemplos de sistemas de controle retroalimentados autorreguláveis incluem:   

- o regulador do motor a vapor, criado por James Watt (1736–1819),   
- o termostato, criado por Cornelis Drebbel (1572–1633), que também inventou o submarino.   

James Clerk Maxwell (1868) iniciou a teoria matemática dos sistemas de controle.   

A figura central no desenvolvimento pós-guerra da teoria de controle foi Norbert Wiener (1894–1964). Wiener foi um matemático brilhante que trabalhou com Bertrand Russell, entre outros, antes de se interessar por sistemas de controle biológico e mecânico e sua conexão com a cognição.   

Como Craik (que também utilizou sistemas de controle como modelos psicológicos), Wiener e seus colegas Arturo Rosenblueth e Julian Bigelow desafiaram a ortodoxia behaviorista (Rosenblueth et al., 1943). Eles viram o comportamento consciente como o resultado de um mecanismo regulador tentando minimizar o “erro” – a diferença entre o estado atual e o estado objetivo.   

No fim da década de 1940, Wiener, juntamente com Warren McCulloch, Walter Pitts, John von Neumann, organizou uma série de conferências que influenciou os novos modelos matemáticos e computacionais da cognição.   

O livro de Wiener, Cybernetics (Cibernética – 1948), tornou-se bestseller e despertou o público para a possibilidade de máquinas dotadas de inteligência artificial.   

Enquanto isso, na Grã-Bretanha, W. Ross Ashby (Ashby, 1940) foi pioneiro em ideias semelhantes. Ashby, Alan Turing, Grey Walter e outros formaram o Ratio Club para “aqueles que tinham as ideias de Wiener antes de surgir o livro de Wiener”.   
Design for a Brain (Projeto de um Cérebro – 1948, 1952), de Ashby, elaborava a sua ideia de que a mente poderia ser criada com a utilização de mecanismos homeostáticos contendo laços de retroalimentação para atingir comportamento adaptável estável.   

A teoria de controle moderna, em especial o ramo conhecido como controle estocástico ótimo, tem como objetivo o projeto de sistemas que maximizam uma função custo sobre o tempo.   

Isso corresponde aproximadamente ao modelo padrão da IA: projetar sistemas que se comportem de maneira ótima.   

Então, por que a IA e a teoria de controle são dois campos diferentes, apesar das conexões estreitas entre seus fundadores?   

A resposta reside no acoplamento estrito entre as técnicas matemáticas familiares aos participantes e os conjuntos de problemas correspondentes que foram incluídos em cada visão do mundo. O cálculo e a álgebra matricial, as ferramentas da teoria de controle, eram adequados para sistemas que podem ser descritos por conjuntos fixos de variáveis contínuas, enquanto a IA foi criada em parte como um meio de escapar das limitações percebidas.   

As ferramentas de inferência lógica e computação permitiram que os pesquisadores da IA considerassem problemas como a linguagem, a visão, o planejamento simbólico, que ficavam completamente fora do campo de ação da teoria de controle.

---

### **1.2.8 Linguística**

• Como a linguagem se relaciona com o pensamento?   

Em 1957, B. F. Skinner publicou Verbal Behavior (Comportamento Verbal). Essa obra foi uma descrição completa e detalhada da abordagem behaviorista para o aprendizado da linguagem, escrita pelo mais proeminente especialista no campo. Porém, curiosamente, uma crítica do livro se tornou tão conhecida quanto o próprio livro e serviu para aniquilar o interesse pelo behaviorismo. O autor da resenha foi o linguista Noam Chomsky, que tinha acabado de publicar um livro sobre sua própria teoria, Syntactic Structures (Estruturas Sintáticas). Chomsky chamou a atenção para o fato de que a teoria behaviorista não tratava da noção de criatividade na linguagem - ela não explicava como as crianças podiam compreender e formar frases que nunca tinham ouvido antes. A teoria de Chomsky - baseada em modelos sintáticos criados pelo linguista indiano Panini (cerca de 350 a.C.) - podia explicar esse fato e, diferentemente das teorias anteriores, era formal o bastante para poder, em princípio, ser programada.   

Portanto, a linguística moderna e a IA “nasceram” aproximadamente na mesma época e cresceram juntas, cruzando-se em um campo híbrido chamado linguística computacional ou processamento de linguagem natural. O problema de compreender a linguagem logo se tornou consideravelmente mais complexo do que parecia em 1957. A compreensão da linguagem exige a compreensão do assunto e do contexto, não apenas a compreensão da estrutura das frases. Isso pode parecer óbvio, mas só foi amplamente avaliado na década de 1960. Grande parte do trabalho inicial em representação do conhecimento (o estudo de como colocar o conhecimento em uma forma que um computador possa utilizar) estava vinculado à linguagem e era suprido com informações da pesquisa em linguística que, por sua vez, estava conectada a décadas de pesquisa sobre a análise filosófica da linguagem.   

 --- 

## 1.3 História da inteligência artificial

Uma forma rápida de resumir os marcos na história da IA é listar os vencedores do Prêmio Turing:   

- Marvin Minsky (1969) e John McCarthy (1971) pela definição dos fundamentos do campo com base na representação e no raciocínio;   
- Ed Feigenbaum e Raj Reddy (1994) pelo desenvolvimento de sistemas especialistas, que codificam o conhecimento humano para resolver problemas do mundo real;   
- Judea Pearl (2011) pelo desenvolvimento de técnicas de raciocínio probabilístico que lidam com a incerteza de um modo baseado em princípios;   
- Finalmente, Yoshua Bengio, Geoffrey Hinton e Yann LeCun (2019) por tornar o “aprendizado profundo” (redes neurais multicamadas) uma parte crítica da computação moderna.   

O restante desta seção apresenta mais detalhes sobre cada fase da história da IA.
---

### 1.3.1 Gestação da inteligência artificial (1943–1956)

O primeiro trabalho agora reconhecido como IA foi realizado por Warren McCulloch e Walter Pitts (1943). Inspirados pelo trabalho de modelagem matemática de Nicolas Rashevsky (1936, 1938), orientador de Pitts, eles se basearam em três fontes:   

- o conhecimento da fisiologia básica e da função dos neurônios no cérebro;   
- uma análise formal da lógica proposicional criada por Russell e Whitehead;   
- a teoria da computação de Turing.   

Esses dois pesquisadores propuseram um modelo de neurônios artificiais, no qual cada neurônio se caracteriza por estar “ligado” ou “desligado”, com a troca para “ligado” ocorrendo em resposta à estimulação por um número suficiente de neurônios vizinhos. O estado de um neurônio era considerado “equivalente, em termos concretos, a uma proposição que definia seu estímulo adequado”. Por exemplo, eles mostraram que qualquer função computável podia ser calculada por alguma rede de neurônios conectados e que todos os conectivos lógicos (E, OU, NÃO etc.) podiam ser implementados por estruturas de redes simples. McCulloch e Pitts também sugeriram que redes definidas adequadamente seriam capazes de aprender.   
Donald Hebb (1949) demonstrou uma regra de atualização simples para modificar as intensidades de conexão entre neurônios. Sua regra, agora chamada aprendizado hebbiano, continua a ser um modelo influente até hoje.   
Dois alunos de Harvard, Marvin Minsky (1927–2016) e Dean Edmonds, construíram o primeiro computador de rede neural em 1950. O SNARC, como foi chamado, usava 3 mil válvulas eletrônicas e um mecanismo de piloto automático retirado de um bombardeiro B-24 para simular uma rede de 40 neurônios. Mais tarde, em Princeton, Minsky estudou computação universal em redes neurais. A banca examinadora de seu doutorado mostrou-se cética em relação a esse tipo de trabalho ser classificado como matemática, porém, segundo contam, von Neumann teria dito: “Se não é agora, será algum dia.”   

Surgiram vários exemplos de trabalhos que hoje podem ser caracterizados como IA, incluindo dois programas de jogo de damas desenvolvidos independentemente em 1952 por Christopher Strachey, na Universidade de Manchester, e por Arthur Samuel, na IBM. Mas a visão de Alan Turing foi talvez a mais influente. Já em 1947, ele proferia palestras sobre o tema na Sociedade Matemática de Londres e articulou um programa de pesquisa persuasivo em seu artigo de 1950, “Computing Machinery and Intelligence” (Maquinário de Computação e Inteligência). Nesse artigo, ele apresentou:   

- o teste de Turing,   
- aprendizado de máquina,   
- algoritmos genéticos   
- e aprendizado por reforço.   

Alan Turing tratou de muitas das objeções levantadas à possibilidade de IA, conforme descrito no Capítulo 27. Também sugeriu que seria mais fácil criar IA em nível humano desenvolvendo algoritmos de aprendizado e, em seguida, ensinando a máquina, em vez de programar sua inteligência manualmente. Em palestras subsequentes, ele advertiu que alcançar esse objetivo pode não ser o melhor para a raça humana.   

Em 1955, John McCarthy, do Dartmouth College, convenceu Minsky, Claude Shannon e Nathaniel Rochester a ajudá-lo a reunir pesquisadores dos EUA interessados em teoria de autômatos, redes neurais e estudo da inteligência. Eles organizaram um seminário de 2 meses em Dartmouth, no verão de 1956. Havia 10 participantes, incluindo Allen Newell e Herbert Simon, do Carnegie Tech, **(11)** Trenchard More, de Princeton, Arthur Samuel, da IBM, e Ray Solomonoff e Oliver Selfridge, do MIT.   

A proposta dizia: **(12)**   

> Propomos que um estudo de 2 meses e 10 homens sobre inteligência artificial seja realizado durante o verão de 1956 no Dartmouth College, em Hanover, New Hampshire. O estudo deve ser conduzido com a conjetura básica de que cada aspecto da aprendizagem ou qualquer outra característica da inteligência pode, em princípio, ser descrita tão precisamente a ponto de ser construída uma máquina para simulá-la. Será realizada uma tentativa para descobrir como fazer com que as máquinas usem a linguagem, formem abstrações e conceitos, resolvam os tipos de problemas hoje reservados aos seres humanos e se aperfeiçoem.   

> Achamos que poderá haver avanço significativo em um ou mais desses problemas se um grupo cuidadosamente selecionado de cientistas trabalhar em conjunto durante o verão.   

Apesar da previsão otimista, o seminário em Dartmouth não trouxe nenhuma grande inovação. Newell e Simon apresentaram talvez o trabalho mais amadurecido, um sistema de construção de provas de teoremas matemáticos, o Logic Theorist (LT), sobre o qual Simon afirmou:   

> “Criamos um programa de computador capaz de pensar não numericamente e, assim, resolvemos o antigo dilema mente-corpo.” (13)   

Logo após o seminário, o programa foi capaz de demonstrar a maioria dos teoremas do Capítulo 2 do livro Principia Mathematica, de Russell e Whitehead. Dizem que Russell ficou encantado quando lhe contaram que o programa havia criado uma prova de um teorema que era mais curta que a do livro. Os editores do Journal of Symbolic Logic (Revista de Lógica Simbólica) ficaram menos impressionados; eles rejeitaram um artigo escrito em parceria por Newell, Simon e pelo Logic Theorist.

---

**Notas de Rodapé (11): **Atualmente, Carnegie Mellon University (CMU).   
**Notas de Rodapé (12): **Esse foi o primeiro uso oficial do termo inteligência artificial feito por McCarthy. Talvez “racionalidade computacional” tivesse sido mais preciso e menos ameaçador, mas “IA” pegou. No 50º aniversário da conferência de Dartmouth, McCarthy declarou que resistiu aos termos “computador” ou “computacional” em consideração a Norbert Wiener, que estava promovendo dispositivos cibernéticos analógicos em vez de computadores digitais.   
**Notas de Rodapé (13): **Newell e Simon também criaram uma linguagem de processamento de listas, a IPL, para escrever o LT. Eles não tinham nenhum compilador e fizeram a conversão para código de máquina à mão. Para evitar erros, trabalharam em paralelo, gritando números binários um ao outro à medida que escreviam cada instrução, a fim de terem certeza de que os números eram os mesmos.   

 --- 

### **1.3.2 Entusiasmo inicial, grandes expectativas (1952-1969)**

Em geral, a classe intelectual dos anos 1950 preferia acreditar que “uma máquina nunca poderá realizar X” (ver, no Capítulo 27, uma longa lista de X reunidos por Turing). Os pesquisadores da IA respondiam naturalmente demonstrando um X após outro. Particularmente, eles estavam focados em tarefas que consideravam indicar a inteligência nos humanos, incluindo jogos, quebra-cabeças, matemática e testes de QI. John McCarthy se referiu a esse período como a era do “Olhe, mamãe, sem as mãos!”.   

O sucesso inicial do LT de Newell e Simon prosseguiu com o General Problem Solver (Resolvedor Geral de Problemas) ou GPS. Diferentemente do LT, esse programa foi projetado desde o início para imitar protocolos humanos para a resolução de problemas. Dentro da classe limitada de quebra-cabeças com a qual podia lidar, verificou-se que a ordem em que o programa considerava submetas e ações possíveis era semelhante à ordem em que os seres humanos abordavam os mesmos problemas. Desse modo, o GPS talvez tenha sido o primeiro programa a incorporar a abordagem de “pensar de forma humana”.   

O sucesso do GPS e de programas subsequentes como modelos de cognição levou Newell e Simon (1976) a formularem a famosa hipótese do **sistema de símbolos físicos,** que afirma que:   

> “um sistema de símbolos físicos tem os meios necessários e suficientes para uma ação inteligente geral”.   

O que eles queriam dizer é que qualquer sistema (humano ou máquina) que demonstre inteligência deve operar manipulando estruturas de dados compostas por símbolos. Veremos, mais adiante, que essa hipótese foi contestada a partir de várias direções.   

Na IBM, Nathaniel Rochester e seus colegas produziram alguns dos primeiros programas de IA. Herbert Gelernter (1959) construiu o Geometry Theorem Prover (Provador de Teoremas de Geometria), que podia demonstrar teoremas que seriam considerados bastante complicados por muitos alunos de matemática. Esse trabalho foi precursor dos modernos provadores de teoremas matemáticos.   
De todo o trabalho exploratório realizado durante esse período, talvez o mais influente a longo prazo tenha sido o de Arthur Samuel para o jogo de damas. Usando métodos que agora chamamos de aprendizagem por reforço (ver Capítulo 22), os programas de Samuel aprenderam a jogar em um nível amador elevado. Com isso, ele contestou a ideia de que os computadores só podem realizar as atividades para as quais foram programados: seu programa aprendeu rapidamente a jogar melhor que seu criador.   

O programa foi demonstrado na televisão em fevereiro de 1956, causando impressão muito forte. Como Turing, Samuel teve dificuldades para conseguir um horário em que pudesse utilizar os computadores. Trabalhando à noite, ele usou máquinas que ainda estavam na bancada de testes na fábrica da IBM. O programa de Samuel foi o precursor de sistemas posteriores, como:   

- TD-GAMMON (Tesauro, 1992), que estava entre os melhores jogadores de gamão do mundo, e   
- ALPHAGO (Silver et al., 2016), que chocou o mundo ao derrotar o campeão mundial humano em Go (ver Capítulo 5).

---

### 1.3.3 Dose de realidade (1966-1973)

Desde o início, os pesquisadores da IA eram ousados nos prognósticos de seus sucessos futuros. Esta declaração de Herbert Simon em 1957 é citada com frequência:   

> Não é meu objetivo surpreendê-los ou chocá-los, mas o modo mais simples de resumir tudo isso é dizer que agora existem no mundo máquinas que pensam, aprendem e criam.   

> Além disso, sua capacidade de realizar essas atividades está crescendo rapidamente até o ponto - em um futuro visível - no qual a variedade de problemas com que elas poderão lidar será correspondente à variedade de problemas com os quais lida a mente humana.   

O termo “futuro visível” é muito vago, mas Simon também fez predições mais concretas: que dentro de 10 anos um computador seria campeão de xadrez e que um teorema matemático significativo seria provado por uma máquina. Essas previsões se realizaram (ou quase) no prazo de 40 anos, em vez de 10. O excesso de confiança de Simon se devia ao desempenho promissor dos primeiros sistemas de IA em exemplos simples. Contudo, em quase todos os casos, esses primeiros sistemas acabaram falhando em problemas mais difíceis.   

Houve dois motivos principais para essa falha:   

1. O primeiro foi que muitos dos primeiros sistemas de IA eram baseados principalmente em “introspecção informada” sobre o modo como os seres humanos realizam uma tarefa, em vez de uma análise cuidadosa da tarefa, o que ela significa para ser uma solução e o que um algoritmo precisaria fazer para produzir tais soluções de modo confiável.   
2. O segundo tipo da falha foi uma falta de apreciação da impossibilidade de tratar muitos dos problemas que a IA estava tentando resolver.   

A maior parte dos primeiros sistemas de solução de problemas funcionava experimentando diferentes combinações de passos até encontrar a solução. Essa estratégia funcionou inicialmente porque os micromundos continham pouquíssimos objetos e, consequentemente, um número muito pequeno de ações possíveis e sequências de soluções muito curtas. Antes do desenvolvimento da teoria de complexidade computacional, era crença geral que o “aumento da escala” para problemas maiores era apenas uma questão de haver hardware mais rápido e maior capacidade de memória. Por exemplo, o otimismo que acompanhou o desenvolvimento da prova de teoremas por resolução logo foi ofuscado quando os pesquisadores não conseguiram provar teoremas que envolviam mais que algumas dezenas de fatos. O fato de um programa poder encontrar uma solução em princípio não significa que o programa contenha quaisquer dos mecanismos necessários para encontrá-la na prática.   

<img title="" src="https://raw.githubusercontent.com/nicolas-oliveira/roadmap-IA-paradigmas/refs/heads/main/week00-introduction/4%C2%AA%20Edi%C3%A7%C3%A3o%20-%20Introdu%C3%A7%C3%A3o%20%C3%A0%20Intelig%C3%AAncia%20Artificial%20Peter%20e%20Novrig/files/1f399e546e7d942760bcbd83aea7ef490dc1bd6f30ab30.png" alt="1f399e546e7d942760bcbd83aea7ef490dc1bd6f30ab30737aa87757e4deab1c" data-align="center" width="562">    

> Figura 1.3 Uma cena do mundo de blocos. O programa SHRDLU (Winograd, 1972) tinha acabado de completar o comando: “Encontre um bloco mais alto que o bloco que você está segurando e coloque- o na caixa.”   

A ilusão do poder computacional ilimitado não ficou confinada aos programas de resolução de problemas. Os primeiros experimentos de evolução de máquina (agora chamada de programação genética) (Friedberg, 1958; Friedberg et al., 1959) se baseavam na convicção — sem dúvida correta — de que, realizando-se uma série apropriada de pequenas mutações em um programa em código de máquina, seria possível gerar um programa com bom desempenho para qualquer tarefa simples. Então, a ideia era experimentar mutações aleatórias com um processo de seleção para preservar mutações que parecessem úteis. Apesar de milhares de horas de tempo de CPU, quase nenhum progresso foi demonstrado.   

A incapacidade de conviver com a “explosão combinatória” foi uma das principais críticas à IA contidas no relatório de Lighthill (Lighthill, 1973), que formou a base para a decisão do governo britânico de encerrar o apoio à pesquisa da IA em todas as universidades, com exceção de duas (a tradição oral pinta um quadro um pouco diferente e mais colorido, com ambições políticas e hostilidades pessoais, cuja descrição não nos interessa aqui).   

Uma terceira dificuldade surgiu devido a algumas limitações fundamentais nas estruturas básicas que estavam sendo utilizadas para gerar o comportamento inteligente. Por exemplo, o livro de Minsky e Papert, Perceptrons (1969), provou que, embora os perceptrons (uma forma simples de rede neural) pudessem aprender tudo o que eram capazes de representar, eles podiam representar muito pouco. Em particular, um perceptron de duas entradas não podia ser treinado para reconhecer quando suas duas entradas eram diferentes.   

Embora seus resultados não se aplicassem a redes mais complexas de várias camadas, o financiamento para pesquisas relacionadas a redes neurais logo se reduziu a quase nada. Ironicamente, os novos algoritmos de aprendizado por retropropagação, que acabariam provocando um enorme renascimento na pesquisa de redes neurais no fim da década de 1980 e novamente na década de 2010, foram, na verdade, desenvolvidos em outros contextos já no início da década de 1960 (Kelley, 1960; Bryson, 1962).   

 --- 

### 1.3.4 Sistemas especialistas (1969–1986)

O panorama da resolução de problemas que havia surgido durante a primeira década de pesquisas em IA foi o de um mecanismo de busca de uso geral que procurava reunir passos elementares de raciocínio para encontrar soluções completas. Tais abordagens foram chamadas de métodos fracos porque, embora gerais, não podiam ter aumento de escala para instâncias grandes ou difíceis. A alternativa para métodos fracos é usar um conhecimento mais poderoso e específico de um domínio, que permita passos de raciocínio maiores e que possa tratar com mais facilidade casos que ocorrem tipicamente em áreas de especialidades menos abrangentes. Podemos dizer que, para resolver um problema difícil, praticamente é necessário já saber a resposta.   

O programa DENDRAL (Buchanan et al., 1969) foi um exemplo inicial dessa abordagem. Ele foi desenvolvido em Stanford, onde Ed Feigenbaum (um antigo aluno de Herbert Simon), Bruce Buchanan (um filósofo que se tornou cientista da computação) e Joshua Lederberg (um geneticista laureado com um Prêmio Nobel) formaram uma equipe para resolver o problema de inferir a estrutura molecular a partir das informações fornecidas por um espectrômetro de massa.   
A entrada para o programa consiste na fórmula elementar da molécula (p. ex., C,H,,NO,) e no espectro de massa que fornece as massas dos diversos fragmentos da molécula gerada quando ela é bombardeada por um feixe de elétrons. Por exemplo, o espectro de massa poderia conter um pico em m = 15, correspondendo à massa de um fragmento metil (CH₃).   

A versão ingênua do programa gerou todas as estruturas possíveis consistentes com a fórmula e depois previu qual seria o espectro de massa observado para cada uma, comparando esse espectro com o espectro real. Como se poderia esperar, esse é um problema intratável, mesmo para moléculas de tamanho moderado. Os pesquisadores do DENDRAL consultaram especialistas em química analítica e descobriram que eles trabalhavam procurando padrões conhecidos de picos no espectro que sugerissem subestruturas comuns na molécula.   

Por exemplo, a regra a seguir é usada para reconhecer um subgrupo cetona (C=O), que pesa 28 unidades de massa:   

- Se M é a massa da molécula inteira e existem dois picos em x₁ e x₂ tais que:   
    (a) x₁ + x₂ = M + 28;   
    (b) x₁ - 28 é um pico;   
    (c) x₂ - 28 é um pico; e   
    (d) No mínimo, um entre x₁ e x₂ é alto;   
    então, existe um subgrupo cetona.   

O reconhecimento de que a molécula contém uma subestrutura específica reduz enormemente o número de possíveis candidatos. O DENDRAL era poderoso porque incorporava o conhecimento relevante de espectroscopia de massa não na forma dos princípios básicos, mas em eficientes “receitas de bolo” (Feigenbaum et al., 1971). O DENDRAL foi importante porque representou o primeiro sistema bem-sucedido de conhecimento intensivo: sua habilidade derivava de um grande número de regras de propósito específico.   

Em 1971, Feigenbaum e outros pesquisadores de Stanford iniciaram o Heuristic Programming Project (HPP) para investigar até que ponto a nova metodologia de sistemas especialistas poderia ser aplicada a outras áreas.   
O maior esforço seguinte foi o sistema MYCIN, que diagnosticava infecções no sangue. Com cerca de 450 regras, o MYCIN era capaz de se sair tão bem quanto alguns especialistas e muito melhor do que médicos em início de carreira. Ele também apresentava duas diferenças importantes em relação ao DENDRAL:   

1. Diferentemente das regras do DENDRAL, não havia nenhum modelo teórico geral a partir do qual as regras do MYCIN pudessem ser deduzidas. Elas tinham de ser adquiridas a partir de extensas entrevistas com especialistas.   
2. As regras tinham de refletir a incerteza associada ao conhecimento médico.   

O MYCIN incorporava um cálculo de incerteza chamado de fatores de certeza (consulte o Capítulo 13), que pareciam (na época) se adequar bem à forma como os médicos avaliavam o impacto das evidências sobre o diagnóstico.   
O primeiro sistema especialista comercial bem-sucedido, o R1, iniciou sua operação na Digital Equipment Corporation (McDermott, 1982). O programa ajudou a configurar pedidos de novos sistemas de computadores; em 1986, ele estava fazendo a empresa economizar cerca de 40 milhões de dólares por ano. Em 1988, o grupo de IA da DEC tinha 40 sistemas especialistas entregues e outros a caminho. A DuPont tinha 100 desses sistemas em uso e 500 em desenvolvimento. Quase todas as corporações importantes dos EUA tinham seu próprio grupo de IA e estavam usando ou investigando sistemas especialistas.   

A importância do conhecimento de domínio também ficou aparente na área da compreensão da linguagem natural. Apesar do sucesso do sistema SHRDLU de Winograd, seus métodos não se estendiam para tarefas mais genéricas: para problemas como resolução de ambiguidade, ele usava regras simples, que contavam com o minúsculo escopo do mundo de blocos.   

Diversos pesquisadores, entre eles Eugene Charniak no MIT e Roger Schank em Yale, sugeriram que uma compreensão robusta da linguagem exigiria conhecimentos gerais sobre o mundo e um método genérico para utilizar esses conhecimentos. (Schank foi ainda mais longe, afirmando: “Não existe essa coisa de sintaxe.” Isso irritou muitos linguistas, mas serviu para dar início a uma discussão útil.)   

Schank e seus alunos construíram uma série de programas (Schank e Abelson, 1977; Wilensky, 1978; Schank e Riesbeck, 1981), todos com a tarefa de entender a linguagem natural. Porém, a ênfase foi menos na linguagem em si e mais nos problemas de representação e raciocínio com o conhecimento exigido para compreensão da linguagem.   

O enorme crescimento das aplicações para resolução de problemas reais levou ao desenvolvimento de diversas ferramentas de representação e raciocínio. Algumas se baseavam na lógica — por exemplo, a linguagem Prolog se tornou popular na Europa e no Japão, e a família PLANNER, nos EUA. Outras, seguindo a ideia de frames de Minsky (1975), adotaram uma abordagem mais estruturada, reunindo fatos sobre tipos específicos de objetos e eventos, e organizando os tipos em uma grande hierarquia taxonômica, semelhante a uma taxonomia biológica.   

Em 1981, os japoneses anunciaram o projeto “Fifth Generation”, um plano de 10 anos para montar computadores inteligentes e com forte paralelismo, que rodassem Prolog. O orçamento deveria ultrapassar 1,3 bilhão de dólares em valores atuais. Em resposta, os EUA formaram a Microelectronics and Computer Technology Corporation (MCC), um consórcio projetado para assegurar a competitividade nacional. Em ambos os casos, a IA fazia parte de um amplo esforço, incluindo o projeto de chips e a pesquisa de interface com humanos.   

Na Grã-Bretanha, o relatório Alvey reabilitou o subsídio que havia sido cortado em consequência do relatório Lighthill. No entanto, nenhum desses projetos alcançou seus objetivos ambiciosos em termos de novas capacidades de IA ou impacto econômico.   

De modo geral, a indústria da lA se expandiu de alguns milhões de dólares em 1980 para bilhões de dólares em 1988, incluindo centenas de empresas construindo sistemas especialistas, sistemas de visão, robôs, e software e hardware especializados para esses propósitos.   

Logo depois, veio um período chamado de **"inverno da IA"**, em que muitas empresas caíram no esquecimento à medida que deixaram de cumprir promessas extravagantes. Tornou-se dificil construir e manter sistemas especialistas para domínios complexos, em parte porque os métodos de raciocínio usados pelos sistemas falhavam frente à incerteza e em parte porque os sistemas não aprendiam com a experiência.   

### 1.3.5 Retorno das redes neurais (1986 até a atualidade)

Em meados dos anos 1980, pelo menos quatro grupos diferentes reinventaram o algoritmo de aprendizado por retropropagação, desenvolvido primeiramente no início da década de 1960.   

O algoritmo foi aplicado a muitos problemas de aprendizado em ciência da computação e psicologia, e a ampla disseminação dos resultados na coletânea **Parallel Distributed Processing** (Processamento Distribuído Paralelo) (Rumelhart e McClelland, 1986) causou grande alvoroço.   

Os chamados **"modelos conexionistas"** eram vistos por alguns como concorrentes diretos dos modelos simbólicos promovidos por Newell e Simon e da abordagem logicista de McCarthy e outros pesquisadores. Pode parecer óbvio que, em certo nível, os seres humanos manipulam simbolos - de fato, o livro do antropólogo Terrence Deacon, **The Symbolic Species** (A Espécie Simbólica - 1997), sugere que essa é a característica que define os seres humanos.   

Contra isso, Geoff Hinton, uma figura importante no ressurgimento das redes neurais nas décadas de 1980 e 2010, descreveu os símbolos como o **"éter luminífero da IA"** - uma referência ao meio inexistente através do qual muitos físicos do século XIX acreditavam que as ondas eletromagnéticas se propagavam. Certamente, olhando mais de perto, muitos conceitos que temos na linguagem não têm o tipo de condições logicamente necessárias e suficientes que os primeiros pesquisadores de IA esperavam capturar de forma axiomática. Pode ser que os modelos conexionistas formem conceitos internos de um modo mais fluido e impreciso, mais adequado à confusão do mundo real. Eles também têm a capacidade de aprender com os exemplos - eles podem comparar seu valor de saída previsto com o valor real em um problema e modificar seus parâmetros para reduzir a diferença, tornando-os mais propensos a ter melhor desempenho em exemplos futuros.   

### 1.3.6 Raciocínio probabilístico e aprendizado de máquina (1987 até a atualidade)

A fragilidade dos sistemas especialistas levou a uma abordagem nova e mais científica que incorpora probabilidade em vez de lógica booleana, aprendizado de máquina em vez de programação manual e resultados experimentais em vez de afirmações filosóficas.¹⁴ Agora, é mais comum tomar as teorias existentes como bases, em vez de propor teorias inteiramente novas, fundamentar as afirmações em teoremas rigorosos ou em metodologia experimental consolidada (Cohen, 1995), em vez de utilizar como base a intuição, e destacar a relevância para aplicações reais no lugar de exemplos fabricados simples.   

Conjuntos de problemas de benchmark compartilhados tornaram-se a norma para demonstrar progresso, incluindo:   

- O repositório da UC em Irvine para conjuntos de dados de aprendizado de máquina   
- A International Planning Competition para algoritmos de planejamento   
- O corpo de reconhecimento de fala LibriSpeech   
- O conjunto de dados MNIST para o reconhecimento de digitos manuscritos   
- ImageNet e COCO para reconhecimentos de objetos por imagem   
- SQUAD para respostas a perguntas em linguagem natural   
- A competição WMT para tradução de máquina   
- As competições internacionais para resolvedores de satisfatibilidade booleana (SAT)   

¹⁴ Alguns caracterizaram essa mudança como uma vitória dos **puros** - aqueles que pensam que as teorias da LA devem se fundamentar no rigor matemático - sobre os **impuros** - aqueles que preferem experimentar muitas ideias, escrever alguns programas e depois avaliar o que parece estar funcionando. As duas abordagens são importantes. Um deslocamento em direção à pureza implica que o campo alcançou um nivel de estabilidade e maturidade. A ênfase atual no aprendizado profundo pode representar um ressurgimento da impureza.   

Em parte, a lA surgiu como uma rebelião contra as limitações de áreas existentes como a teoria de controle e a estatística, mas nesse período ela adotou os resultados positivos desses campos. Conforme afirmou David McAllester (1998):   

> ciência da computação. Atualmente, esse isolacionismo está sendo abandonado. Existe o reconhecimento de que o aprendizado da máquina não deve ser isolado da teoria da informação, de que o raciocínio incerto não deve ser isolado da modelagem estocástica, de que a busca não deve ser isolada da otimização clássica e do controle, e de que o raciocínio automatizado não deve ser isolado dos métodos formais e da análise estática.

O campo do reconhecimento de fala ilustra o padrão. Nos anos 1970, foi experimentada ampla variedade de arquiteturas e abordagens. Muitas delas eram bastante ocasionais e frágeis, e funcionavam apenas em alguns exemplos cuidadosamente selecionados. Nos anos 1980, abordagens baseadas em **modelos ocultos de Markov** (HMMs, do inglês Hidden Markov Models) passaram a dominar a área. Dois aspectos de HMMs são relevantes:   

1. Eles se baseiam em uma teoria matemática rigorosa. Isso permitiu que os cientistas de reconhecimento de fala se baseassem em várias décadas de resultados matemáticos desenvolvidos em outros campos.   
2. Eles são gerados por um processo de treinamento em um grande conjunto de dados reais de fala. Isso assegura um desempenho robusto e, em testes cegos rigorosos, os HMMs têm melhorado continuamente suas pontuações.   

Como resultado, a tecnologia da fala e o campo inter-relacionado de reconhecimento de caracteres manuscritos fizeram a transição para aplicações industriais e de consumo em larga escala. Observe que não há nenhuma afirmação científica de que os humanos utilizam HMMs para reconhecer a fala, mas apenas de que HMMs fornecem um arcabouço matemático para a compreensão e solução do problema. Na seção 1.3.8, veremos que o aprendizado profundo atrapalhou um pouco essa cômoda narrativa.   

O ano de 1988 foi importante para a conexão entre IA e outros campos, entre eles a estatística, a pesquisa operacional, a teoria da decisão e a teoria de controle. A obra de Judea Pearl, **Probabilistic Reasoning in Intelligent Systems** (Raciocínio Probabilístico em Sistemas Inteligentes - 1988), levou a uma nova aceitação da probabilidade e da teoria da decisão na IA. O desenvolvimento de Pearl quanto às redes bayesianas ocasionou um formalismo rigoroso para a representação eficiente do conhecimento incerto, bem como algoritmos práticos para o raciocínio probabilístico. Os Capítulos 12 a 16 examinam essa área, além de desenvolvimentos mais recentes que aumentaram muito o poder expressivo dos formalismos probabilísticos. O Capítulo 20 descreve métodos para o aprendizado de redes bayesianas e de modelos relacionados a partir de dados.   

Ainda em 1988, uma segunda contribuição importante foi o trabalho de Rich Sutton na conexão da aprendizagem por reforço - que tinha sido usado no programa de jogo de damas de Arthur Samuel nos anos 1950 - com a teoria dos **processos de decisão markovianos** (MDPs, do inglês Markov Decision Processes), desenvolvida no campo da pesquisa operacional. Diversos trabalhos surgiram conectando a pesquisa em planejamento em IA aos MDPs, e o campo de aprendizado por reforço encontrou aplicações na robótica e no controle de processos, além de adquirir profundos alicerces teóricos.   

Uma consequência da recente apreciação da IA por dados, modelagem estatística, otimização e aprendizado de máquina foi a reunificação gradual de subcampos, como:   

- Visão computacional   
- Robótica   
- Reconhecimento de fala   
- Sistemas multiagentes   
- Processamento de linguagem natural   

que se tornaram um tanto separados do núcleo da IA. O processo de reintegração gerou benefícios significativos tanto em termos de aplicações - por exemplo, a implantação de robôs na prática se expandiu muito durante esse período - quanto em uma melhor compreensão teórica dos problemas centrais da IA.   

### 1.3.7 Big data (2001 até a atualidade)

Avanços notáveis no poder da computação e na criação da World Wide Web facilitaram a criação de enormes conjuntos de dados - um fenômeno às vezes conhecido como **big data**. Esses conjuntos de dados incluem:   

- Trilhões de palavras de texto   
- Bilhões de imagens   
- Bilhões de horas de áudio e video   
- Grandes quantidades de dados genômicos   
- Dados de rastreamento de veículos   
- Dados de sequências de cliques   
- Dados de redes sociais   
- E assim por diante   

Isso ocasionou o desenvolvimento de algoritmos de aprendizado projetados especialmente para tirar proveito desses enormes conjuntos de dados. Quase sempre, a grande maioria dos exemplos nesses conjuntos de dados são não rotulados; por exemplo, em um artigo influente de Yarowsky (1995) sobre desambiguação de sentido de palavras, as ocorrências de uma palavra como "planta" não são rotuladas no conjunto de dados para indicar se ela se refere a flora ou a uma fábrica. Porém, com conjuntos de dados com tamanho suficiente, algoritmos de aprendizado adequados podem conseguir acurácia superior a 96% na tarefa de identificar o sentido desejado da palavra. Além disso, Banko e Brill (2001) afirmaram que a melhoria no desempenho obtida pelo aumento do tamanho do conjunto de dados por duas ou três ordens de grandeza supera qualquer melhoria alcançada pela modificação do algoritmo. 

Parece haver um fenômeno semelhante em tarefas de visão por computador, como no problema do preenchimento de lacunas em fotografias - lacunas causadas por danos ou pela remoção de um ex-amigo. Hays e Efros (2007) desenvolveram um método mais inteligente para fazer isso, mesclando pixels de imagens semelhantes; eles descobriram que a técnica não funcionava bem com um banco de dados de milhares de imagens, mas que excedia um limiar de qualidade com milhões de imagens. Pouco depois, a disponibilidade de dezenas de milhões de imagens no banco de dados ImageNet (Deng et al., 2009) gerou uma revolução no campo da visão computacional.   

A disponibilidade de big data e a mudança para o aprendizado de máquina ajudaram a IA a recuperar a atratividade comercial (Havenstein, 2005; Halevy et al., 2009). Big data foi um fator fundamental na vitória de 2011 do sistema Watson da IBM sobre os campeões humanos no jogo de perguntas Jeopardy! - evento que teve grande impacto na percepção do público sobre a IA.   

### 1.3.8 Aprendizado profundo (2011 até a atualidade)

O termo **aprendizado profundo** refere-se ao aprendizado de máquina usando várias camadas de elementos de computação simples e configuráveis. Já na década de 1970 foram realizados experimentos com essas redes e, na forma de redes neurais convolucionais, encontraram algum sucesso no reconhecimento de dígitos manuscritos na década de 1990 (LeCun et al., 1995).   
Porém, só em 2011 é que os métodos de aprendizagem profunda realmente ganharam força.

Isso ocorreu primeiro no reconhecimento de fala e, em seguida, no reconhecimento visual de objetos.   

Em 2012, na competição ImageNet, que exigia a classificação de imagens em uma entre mil categorias (tatu, prateleira, saca-rolhas etc.), um sistema de aprendizado profundo criado pelo grupo de Geoffrey Hinton na Universidade de Toronto (Krizhevsky et al., 2013) demonstrou uma fantástica melhoria em relação aos sistemas anteriores, baseados em grande parte em atributos projetados manualmente. Desde então, sistemas de aprendizado profundo superaram o desempenho humano em algumas tarefas de visão (e ficaram para trás em algumas outras tarefas). Ganhos desse tipo também foram relatados no reconhecimento de fala, tradução de máquina, diagnóstico médico e jogos recreativos. O uso de uma rede profunda para representar a função de avaliação contribuiu para as vitórias do **ALPHAGO** sobre os melhores jogadores humanos de Go (Silver et al., 2016, 2017, 2018).   

Esses sucessos notáveis levaram a um ressurgimento do interesse pela IA entre estudantes, empresas, investidores, governos, a mídia e o público em geral. Praticamente toda semana aparecem notícias de uma nova aplicação de IA se aproximando ou superando o desempenho humano, muitas vezes acompanhada por especulações de sucesso acelerado ou de um novo inverno da IA.   

Um hardware poderoso é essencial para realizar aprendizado profundo. Enquanto uma CPU básica de um computador pode fazer 10⁹ ou 10¹⁰ operações por segundo, um algoritmo de aprendizado profundo sendo executado em hardware especializado (p. ex., GPU, TPU ou FPGA) pode consumir entre 10¹⁴ e 10¹⁷ operações por segundo, a maior parte na forma de operações com matrizes e vetores usando um alto grau de paralelismo. Obviamente, o aprendizado profundo também depende da disponibilidade de grandes quantidades de dados de treino e de alguns truques algoritmicos (ver Capítulo 21).   

### 1.4 Estado da arte

O **One Hundred Year Study** (Estudo de Cem Anos) sobre a IA (também conhecido como AI 100), da Universidade de Stanford, reúne painéis de especialistas para fornecer relatórios sobre o estado da arte em IA. Seu relatório de 2016 (Stone et al., 2016; Grosz e Stone, 2018) conclui que "Podem ser esperados aumentos substanciais nos usos futuros das aplicações de IA, incluindo carros mais autônomos, diagnósticos de saúde e tratamento direcionado e assistência física para idosos" e que "A sociedade está agora em um momento crucial para determinar como implantar tecnologias baseadas em IA visando promover, em vez de impedir, valores democráticos como liberdade, igualdade e transparência". O estudo AI100 também produz um Indice de IA em aiindex.org para ajudar a monitorar o progresso. Esses são alguns destaques dos relatórios de 2018 e 2019 (em comparação com uma linha de base do ano 2000, a menos que indicado de outra forma):   

- **Publicações**: os artigos sobre IA aumentaram 20 vezes entre 2010 e 2019, para cerca de 20 mil por ano. A categoria mais popular foi o aprendizado de máquina. (Os artigos sobre aprendizado de máquina em arXiv.org dobraram a cada ano entre 2009 e 2017.) Visão computacional e processamento de linguagem natural foram os próximos em popularidade.   
- **Sentimento**: cerca de 70% dos novos artigos sobre IA são neutros, mas os artigos com tom positivo aumentaram de 12% em 2016 para 30% em 2018. As questões mais comuns são éticas: privacidade de dados e discriminação por algoritmos.   
- **Estudantes**: as inscrições no curso aumentaram cinco vezes nos EUA e 16 vezes mundialmente a partir de uma linha de base de 2010. IA é a especialização mais popular na Ciência da Computação.   
- **Diversidade**: os professores de IA em todo o mundo são cerca de 80% homens e 20% mulheres. Números semelhantes são encontrados entre doutorandos e em contratações na indústria.   
- **Conferências**: a audiência na NeurIPS aumentou 800% desde 2012, para 13.500 inscritos. Outras conferências estão passando por um aumento anual de aproximadamente 30%.   
- **Indústria**: as startups ligadas a IA nos EUA aumentaram 20 vezes, passando a mais de 800.   
- **Internacionalização**: a China publica mais artigos por ano do que os EUA e quase o mesmo que toda a Europa. No entanto, no impacto ponderado por citação, os autores dos EUA estão 50% à frente dos autores chineses. Cingapura, Brasil, Austrália, Canadá e Índia são os países que mais crescem em termos de número de contratações relacionadas à IA.   
- **Visão**: as taxas de erro para detecção de objetos (conforme alcançado no LSVRC, o Desafio de Reconhecimento Visual em Grande Escala) melhoraram de 28% em 2010 para 2% em 2017, ultrapassando o desempenho humano. A precisão na resposta a perguntas visuais abertas (VQA) melhorou de 55% para 68% desde 2015, mas fica atrás do desempenho humano em 83%.   
- **Velocidade**: O tempo de treinamento para a tarefa de reconhecimento de imagem diminuiu por um fator de 100 apenas nos 2 últimos anos. A quantidade de capacidade de computação usada nas principais aplicações de IA está dobrando a cada 3,4 meses.   
- **Linguagem**: a precisão nas respostas às perguntas, medida pela pontuação F1 no Stanford Question Answer Dataset (SQUAD), aumentou de 60 para 95, de 2015 a 2019; na variante SQUAD 2, o progresso foi mais rápido, passando de 62 para 90 em apenas 1 ano. Ambas as pontuações superam o desempenho de nível humano.   
- **Comparação com humanos**: segundo relatos, em 2019, os sistemas de IA atingiram ou superaram o desempenho de nível humano em xadrez, Go, pôquer, Pac-Man, Jeopardy!, detecção de objetos na ImageNet, reconhecimento de fala em um dominio limitado, tradução de chinês para inglês em um domínio restrito, Quake III, Dota 2, StarCraft II, diversos jogos de Atari, detecção de câncer de pele, detecção de câncer de próstata, enovelamento de proteína e diagnóstico de retinopatia diabética.   

Quando (se for o caso) os sistemas de IA atingirão desempenho de nivel humano em uma grande variedade de tarefas? Ford (2018) entrevista especialistas em IA e encontra uma grande variedade de anos como resposta, variando de 2029 a 2200, com 2099 na média. Em uma pesquisa semelhante (Grace et al., 2017), 50% dos entrevistados acharam que isso poderia acontecer até 2066, embora 10% tenham pensado que isso poderia acontecer já em 2025 e alguns tenham dito "nunca". Os especialistas também ficaram divididos quanto à necessidade de novos avanços fundamentais ou apenas de melhorias nas técnicas atuais. Mas não leve essas previsões muito a sério; como Philip Tetlock (2017) demonstra na area de previsão de eventos mundiais, os especialistas não são melhores do que os amadores.   

Como os futuros sistemas de IA funcionarão? Ainda não podemos dizer. Conforme detalhamos nesta seção, o campo tem adotado várias histórias sobre si mesmo - primeiro a ideia ousada de que a inteligência por uma máquina seria possível e, então, que ela poderia ser alcançada codificando em lógica o conhecimento especialista; mais adiante, que modelos probabilísticos do mundo seriam a ferramenta principal e, mais recentemente, que o aprendizado de máquina induziria modelos que podem não ser baseados em qualquer teoria bem compreendida. O futuro revelará o modelo a seguir.   

O que a IA pode fazer hoje? Talvez não tanto quanto alguns dos artigos mais otimistas da mídia poderiam levar alguém a acreditar, mas ainda assim muito. Aqui, mostramos alguns exemplos:   
**Veículos robóticos**: a história dos veículos robóticos remonta aos carros radiocontrolados dos anos 1920, porém as primeiras demonstrações de veículos autônomos em estradas sem guias especiais aconteceram na década de 1980 (Kanade et al., 1986; Dickmanns e Zapp, 1987). Depois das demonstrações bem-sucedidas de direção em estradas sem asfalto, no desafio de percorrer 132 milhas conhecido como DARPA Grand Challenge em 2005 (Thrun, 2006), e depois em ruas com trânsito no Urban Challenge, em 2007, a corrida para o desenvolvimento de carros autônomos começou para valer. Em 2018, veículos de teste da Waymo passaram da marca de 10 milhões de milhas dirigidas em estradas públicas sem um acidente sério, requisitando controle do motorista humano somente uma vez a cada 6 mil milhas. Logo após, a empresa começou a oferecer um serviço comercial de táxi robótico.   
No ar, drones autônomos de asa fixa têm realizado entregas de sangue de uma ponta a outra do país em Ruanda desde 2016. Quadricópteros realizam manobras aéreas incríveis, exploram prédios enquanto criam mapas 3D e se reúnem em formações autônomas.   

**Robôs com pernas**: BigDog, um robô quadrúpede de Raibert et al. (2008), alterou nossas noções de como os robôs se movem - não mais o andar lento, com as pernas rígidas, de um lado para o outro dos robôs de filmes de Hollywood, mas algo muito semelhante a um animal e capaz de se recuperar quando empurrado ou escorregando em uma poça de gelo. Atlas, um robô humanoide, não apenas anda em terreno irregular, mas salta sobre caixas e dá saltos mortais (Ackerman e Guizzo, 2016).   
**Planejamento autônomo e escalonamento**: a uma centena de milhões de quilômetros da Terra, o programa Remote Agent, da Nasa, se tornou o primeiro programa de planejamento autônomo de bordo a controlar o escalonamento de operações de uma nave espacial (Jonsson et al., 2000). O Remote Agent gerou planos de metas de alto nível especificadas a partir do solo e monitorou a execução daqueles planos - efetuando a detecção, o diagnóstico e a recuperação de problemas conforme eles ocorriam. Hoje, o toolkit de planejamento EUROPA (Barreiro et al., 2012) é usado para as operações diárias dos robôs de Marte da NASA, e o sistema SEXTANT (Winternitz, 2017) permite a navegação autônoma no espaço profundo, além do sistema de GPS global.   

Durante a crise do Golfo Pérsico em 1991, as forças armadas dos EUA distribuíram uma ferramenta de análise dinâmica e replanejamento, denominada DART (do inglês Dynamic Analysis and Replanning Tool - Cross e Walker, 1994), a fim de realizar o planejamento logístico automatizado e a programação de execução do transporte. Isso envolveu até 50 mil veículos, transporte de carga aérea e pessoal simultaneamente, e teve de levar em conta pontos de partida, destinos, rotas, capacidades de transporte e resolução de conflitos entre todos os parâmetros. A Defense Advanced Research Project Agency (DARPA), agência estatal de defesa dos EUA, declarou que essa única aplicação compensou com folga os 30 anos de investimento da DARPA em IA.   

Todos os dias, empresas de transporte por aplicativo como Uber e serviços de mapeamento como o Google Maps oferecem instruções de direção para centenas de milhões de usuários, planejando rapidamente a melhor rota que considere as condições de tráfego atuais e previstas.   

**Tradução de máquina**: sistemas de tradução automática online agora permitem a leitura de documentos em mais de 100 idiomas, incluindo os idiomas nativos de mais de 99% da população mundial, e traduzem centenas de bilhões de palavras por dia para centenas de milhões de usuários. Embora não sejam perfeitos, geralmente são adequados para a compreensão.   

Para idiomas intimamente relacionados com uma grande quantidade de dados de treinamento (como francês e inglês), as traduções dentro de um domínio restrito estão próximas do nível de um ser humano (Wu et al., 2016b).   
**Reconhecimento de fala**: em 2017, a Microsoft mostrou que seu Sistema de Reconhecimento de Fala Conversacional atingiu uma taxa de erro de palavras de 5,1%, correspondendo ao desempenho humano na tarefa Switchboard, que envolve a transcrição de conversas telefônicas (Xiong et al., 2017). Cerca de um terço da interação com o computador em todo o mundo agora é feita por voz, em vez de pelo teclado; o Skype oferece tradução de fala em tempo real em 10 idiomas. Alexa, Siri, Cortana e Google oferecem assistentes que podem responder a perguntas e realizar tarefas para o usuário; por exemplo, o serviço Duplex da Google usa reconhecimento e síntese de fala para fazer reservas em restaurantes para os usuários, mantendo uma conversa fluente em nome deles.   

**Recomendações**: empresas como Amazon, Facebook, Netflix, Spotify, YouTube, Walmart e outras usam o aprendizado de máquina para recomendar aquilo que você poderia gostar com base em suas experiências anteriores e nas de outras pessoas com gosto parecido ao seu. O campo dos sistemas de recomendação tem uma longa história (Resnick e Varian, 1997), mas vem mudando rapidamente devido aos novos métodos de aprendizado profundo que analisam conteúdo (texto, música, vídeo), bem como histórico e metadados (van den Oord et al., 2014; Zhang et al., 2017). A filtragem de spams também pode ser considerada uma forma de recomendação (ou contraindicação); as técnicas atuais de IA filtram mais de 99,9% dos spams, e os serviços de e-mail também podem recomendar destinatários em potencial, bem como um possível texto de resposta.   

**Jogos**: quando o Deep Blue venceu o campeão mundial de xadrez Garry Kasparov em 1997, os defensores da supremacia humana colocaram suas esperanças no Go. Piet Hut, astrofísico e entusiasta do Go, previu que seriam necessários "cem anos para que um computador vencesse os humanos no Go - talvez mais ainda". Porém, apenas 20 anos depois, o ALPHAGO superou todos os jogadores humanos (Silver et al., 2017). Ke Jie, o campeão mundial, disse: "No ano passado, ele ainda era bastante humano quando jogava. Mas esse ano tornou-se como um deus do Go." ALPHAGO se beneficiou do estudo de centenas de milhares de jogos anteriores de jogadores humanos de Go e do conhecimento apurado de jogadores experientes de Go que trabalharam na equipe.   

Um programa sucessor, o **ALPHAZERO**, não utilizou nenhuma entrada de humanos (exceto para as regras do jogo) e foi capaz de aprender, jogando consigo mesmo, como derrotar todos os oponentes, humanos e máquinas, em Go, xadrez e shogi (Silver et al., 2018).   

Enquanto isso, campeões humanos foram derrotados por sistemas de IA em jogos tão diversos quanto o jogo de perguntas Jeopardy! (Ferrucci et al., 2010), pôquer (Bowling et al., 2015; Moraveik et al., 2017; Brown e Sandholm, 2019) e os videogames Dota 2 (Fernandez e Mahlmann, 2018), Star Craft II (Vinyals et al., 2019) e Quake III (Jaderberg et al., 2019).   

**Compreensão de imagens**: não contentes por exceder a acurácia humana na tarefa desafiadora de reconhecimento de objetos do ImageNet, os pesquisadores da visão computacional enfrentaram o problema mais difícil de legendagem de imagens. Alguns exemplos impressionantes incluem "Uma pessoa andando de moto em uma estrada de terra", "Duas pizzas repousando em cima de um fogão" e "Um grupo de jovens jogando frisbee" (Vinyals et al., 2017b).   

No entanto, os sistemas atuais estão longe da perfeição: uma "geladeira cheia de comida e bebida" se mostrou sendo uma placa de estacionamento proibido parcialmente ocultada por vários adesivos pequenos.   

**Medicina**: atualmente, algoritmos de IA igualam ou superam os médicos especialistas no diagnóstico de muitas condições, particularmente quando esse diagnóstico é baseado em imagens. Alguns exemplos incluem doença de Alzheimer (Ding et al., 2018), câncer metastático (Liu et al., 2017; Esteva et al., 2017), doença oftálmica (Gulshan et al., 2016) e doenças de pele (Liu et al., 2019c). Uma revisão sistemática e meta-análise (Liu et al., 2019a) constatou que, em média, o desempenho dos programas de IA era equivalente ao dos profissionais de saúde. Uma ênfase atual na IA médica é a facilitação de parcerias homem-máquina. Por exemplo, o sistema LYNA atinge 99,6% de acurácia geral no diagnóstico de câncer de mama metastático - melhor do que um especialista humano sem ajuda -, mas a combinação do sistema com um humano funciona ainda melhor (Liu et al., 2018; Steiner et al., 2018).   

A adoção ampla dessas técnicas agora é limitada não pela precisão do diagnóstico, mas pela necessidade de demonstrar melhora nos resultados clínicos e garantir transparência, imparcialidade e privacidade de dados (Topol, 2019). Em 2017, somente duas aplicações médicas de IA foram aprovadas pela FDA, mas passaram para 12 em 2018 e o número continua a subir.   

**Ciência climática**: um grupo de cientistas ganhou o Prêmio Gordon Bell de 2018 por um modelo de aprendizado profundo que descobre informações detalhadas sobre eventos climáticos extremos que, anteriormente, ficavam ocultos nos dados climáticos. Eles usaram um supercomputador com hardware de GPU especializado para ultrapassar o nível de exaops (10¹⁸ operações por segundo), o primeiro programa de aprendizado de máquina a conseguir isso (Kurth et al., 2018). Rolnick et al. (2019) apresentaram um catálogo de 60 páginas com maneiras como o aprendizado de máquina pode ser usado para o enfrentamento da mudança climática.   

Esses são apenas alguns exemplos de sistemas de inteligência artificial que existem hoje em dia. Não é mágica nem ficção científica, mas ciência, engenharia e matemática, e este livro apresenta uma introdução a tudo isso.   

# 1.5 Riscos e benefícios da IA

Francis Bacon, um filósofo considerado o criador do método científico, observou em *The Wisdom of the Ancients* (A Sabedoria dos Ancestrais) (1609) que as "artes mecânicas são de uso ambíguo, servindo tanto para ferir quanto para remediar". Como a IA desempenha um papel cada vez mais importante nas esferas econômica, social, científica, médica, financeira e militar, seria adequado considerar as feridas e os remédios - na linguagem moderna, os riscos e benefícios - que ela pode ocasionar. Os tópicos resumidos aqui são abordados com mais detalhes nos Capítulos 27 e 28.   

Vamos começar com os benefícios: Em termos simples, toda a nossa civilização é o resultado da nossa inteligência humana. Se tivermos acesso a uma inteligência mecânica substancialmente maior, o teto de nossas ambições será substancialmente elevado. O potencial da IA e da robótica para libertar a humanidade do trabalho braçal e repetitivo, aumentando drasticamente a produção de bens e serviços, pode ser o presságio de uma era de paz e abundância.   
A capacidade de acelerar a pesquisa científica pode resultar em curas para doenças e soluções para as mudanças climáticas e a escassez de recursos. Como sugeriu Demis Hassabis, CEO do Google DeepMind: "Primeiro resolva a IA, depois use a IA para resolver tudo o mais."   

Muito antes de termos a oportunidade de "resolver a IA", porém, incorreremos em riscos pelo uso indevido da IA, inadvertidamente ou não. Alguns deles já são aparentes, enquanto outros parecem prováveis com base nas tendências atuais:   

- **Armas autônomas letais**: são definidas pelas Nações Unidas como armas que podem localizar, selecionar e eliminar alvos humanos sem a supervisão humana. Isto significa que um pequeno grupo pode implantar uma grande quantidade de armas contra alvos humanos definidos por qualquer critério de reconhecimento viável. As tecnologias necessárias para armas autônomas são semelhantes àquelas necessárias para carros autônomos. Em 2014, a ONU iniciou as discussões informais de especialistas sobre os riscos em potencial das armas autônomas letais, passando em 2017 para a fase formal de pré-tratado.
- **Vigilância e persuasão**: embora seja caro, tedioso e, às vezes, legalmente questionável para o pessoal de segurança monitorar linhas telefônicas, câmeras de vídeo, e-mails e outros canais de mensagens, a IA (reconhecimento de fala, visão computacional e compreensão de linguagem natural) pode ser usada de forma escalável para realizar vigilância em massa de indivíduos e detectar atividades de interesse. Ao adaptar fluxos de informações a indivíduos por meio da mídia social, com base em técnicas de aprendizado de máquina, o comportamento político pode ser modificado e controlado de alguma forma - uma preocupação que se tornou evidente nas eleições norte-americanas a partir de 2016.   
- **Tomada de decisão tendenciosa**: o uso indevido ou deliberado de algoritmos de aprendizado de máquina para tarefas como avaliação de liberdade condicional e pedidos de empréstimo pode resultar em decisões discriminatórias por raça, sexo ou outras categorias protegidas. Muitas vezes, os próprios dados refletem um preconceito infiltrado na sociedade.   
- **Impacto no emprego**: as preocupações sobre a eliminação de empregos pelas máquinas ocorrem há séculos. A história nunca é simples: as máquinas realizam algumas das tarefas que os humanos fariam de outra forma, mas também tornam os humanos mais produtivos e, portanto, mais empregáveis; e tornam as empresas mais lucrativas e, portanto, capazes de pagar salários mais altos. Elas podem viabilizar economicamente algumas atividades que, de outra forma, seriam inviáveis. Seu uso geralmente resulta no aumento da riqueza, mas costuma ter o efeito de transferir a riqueza do trabalho para o capital, aumentando ainda mais as desigualdades. Avanços anteriores em tecnologia - como a invenção de teares mecânicos - resultaram em sérias perdas de emprego, mas, por fim, as pessoas encontram novas formas de trabalho para fazer. Por outro lado, é possível que a IA também realize essas novas formas de trabalho. Esse tópico está rapidamente se tornando um grande foco para economistas e governos do mundo inteiro.   
- **Aplicações de segurança crítica**: à medida que as técnicas de IA avançam, elas são cada vez mais usadas em aplicações em que a segurança é crítica, como dirigir carros e gerenciar o abastecimento de água das cidades. Já houve acidentes fatais que destacam a dificuldade de verificação formal e análise estatística de risco para sistemas desenvolvidos com técnicas de aprendizado de máquina. O campo da IA precisará desenvolver padrões técnicos e éticos, pelo menos comparáveis aos prevalentes em outras disciplinas de engenharia e saúde, nas quais a vida de pessoas está em risco.   
- **Cibersegurança**: as técnicas de IA são úteis na defesa contra ataques cibernéticos, por exemplo, detectando padrões de comportamento incomuns, mas também contribuem para a potência, capacidade de sobrevivência e capacidade de proliferação de software malicioso (malware). Por exemplo, métodos de aprendizagem por reforço têm sido usados para criar ferramentas altamente eficazes para ataques de extorsão e phishing automatizados e personalizados.   

Veremos esses assuntos novamente, em mais profundidade, na seção 27.3. À medida que os sistemas de IA se tornam mais capazes, eles passam a assumir uma parcela maior de funções sociais anteriormente desempenhadas por seres humanos. Assim como os humanos usaram essas funções no passado para causar prejuízos, podemos esperar que os humanos usem mal os sistemas de IA nessas funções para causar ainda mais prejuízo. Todos os exemplos mostrados anteriormente indicam a importância de controle e, ultimamente, regulamentação.   

Atualmente, a comunidade de pesquisa e as principais corporações envolvidas na pesquisa em IA têm desenvolvido princípios voluntários de autogoverno para as atividades relacionadas a IA (ver seção 27.3). Governos e organizações internacionais estão preparando agências reguladoras para desenhar regulamentações adequadas a cada caso de uso especifico, para preparar-se para os impactos econômicos e sociais e aproveitar as capacidades da IA para enfrentar os principais problemas da sociedade.   

E a longo prazo? Será que alcançaremos o objetivo de longa data, ou seja, a criação de inteligência comparável ou mais capaz do que a inteligência humana? E, se for o caso, o que acontecerá?   

Durante grande parte da história da IA, essas questões foram ofuscadas pela rotina diária de fazer os sistemas de IA realizarem qualquer coisa, ainda que remotamente inteligente. Como acontece com qualquer disciplina com tal amplitude, a grande maioria dos pesquisadores de lA se especializou em um subcampo especifico, como jogos, representação do conhecimento, visão ou compreensão da linguagem natural - muitas vezes supondo que o progresso nesses subcampos contribuiria para os objetivos mais amplos da IA. Nils Nilsson (1995), um dos líderes originais do projeto Shakey no SRI, relembrou esses objetivos mais amplos e advertiu que os subcampos corriam o risco de se tornarem fins em si mesmos. Mais tarde, alguns fundadores influentes da IA, incluindo John McCarthy (2007), Marvin Minsky (2007) e Patrick Winston (Beal e Winston, 2009), concordaram com os avisos de Nilson, sugerindo que, em vez de focar no desempenho mensurável em aplicações específicas, a IA deve retornar às suas raízes de lutar, nas palavras de Herb Simon, por "máquinas que pensam, que aprendem e que criam". Eles chamaram o esforço de lA de nivel humano ou HLA (Human-Level Al) - uma máquina deveria ser capaz de aprender a fazer qualquer coisa que um humano possa fazer. Seu primeiro simpósio foi em 2004 (Minsky et al., 2004). Outro esforço com objetivos similares, o movimento de inteligência artificial geral (IAG) (Goertzel e Pennachin, 2007), realizou sua primeira conferência e organizou o Journal of Artificial General Intelligence em 2008.   

Por volta da mesma época, surgiram preocupações de que a criação de uma superinteligência artificial ou SIA - inteligência muito superior à capacidade humana - poderia ser uma má ideia (Yudkowsky, 2008; Omohundro, 2008). O próprio Turing (1996) argumentou a mesma coisa em uma palestra de 1951 realizada em Manchester, baseando-se nas mesmas ideias de Samuel Butler (1863):   

> Parece provável que, quando o método de raciocínio de máquina for iniciado, não levará muito tempo para que ele ultrapasse nossos fracos poderes. (...) Portanto, em algum estágio poderemos esperar que as máquinas tomem o controle, da forma como é mencionado no livro Erewhon, de Samuel Butler.   

Essas preocupações só se tornaram mais difundidas com os avanços recentes no aprendizado profundo, a publicação de livros como *Superintelligence*, de Nick Bostrom (2014), e os pronunciamentos públicos de Stephen Hawking, Bill Gates, Martin Rees e Elon Musk.   

Exibir um sentido geral de inquietação com a ideia da criação de máquinas superinteligentes é apenas natural. Poderíamos chamar isso de **problema do gorila**: há cerca de 7 milhões de anos, um primata agora extinto evoluiu, com uma vertente levando aos gorilas e outra aos humanos. Hoje, os gorilas não estão muito contentes com a vertente humana; eles basicamente não têm controle sobre seu futuro. Se esse for o resultado do sucesso da criação da lA super-humana - que os humanos cederão o controle sobre seu futuro -, então talvez devamos parar de trabalhar com IA e, como um corolário, abrir mão dos benefícios que isso poderia trazer.   
Essa é a essência da advertência de Turing: não é óbvio que poderemos controlar máquinas mais inteligentes do que nós.   

Se a IA super-humana fosse uma caixa-preta vinda do espaço sideral, então realmente seria sensato ter cuidado ao abrir a caixa. Mas não é assim que acontece: nós projetamos os sistemas de lA, de modo que, se eles acabassem "tomando o controle", como Turing sugere, isso seria o resultado de uma falha de projeto.   

Para evitar tal resultado, precisamos compreender a origem da falha em potencial.   

Norbert Wiener (1960), que foi motivado a considerar o futuro a longo prazo da IA depois de ver o programa que jogava damas de Arthur Samuel aprendendo a ganhar de seu criador, afirmou isto:   

> Se, para alcançar nossos propósitos, usarmos um agente mecânico em cuja operação não podemos efetivamente interferir (...) é melhor termos certeza de que o propósito implementado na máquina é o propósito que realmente desejamos.   

Muitas culturas têm mitos sobre humanos que pedem algo a deuses, gênios, mágicos ou demônios. Invariavelmente, nessas histórias, eles obtêm o que literalmente pedem, para depois se arrependerem. O terceiro desejo, se houver, é desfazer os dois primeiros.    

Chamaremos isso de problema do **problema do Rei Midas**: Midas, um rei lendário na mitologia grega, pediu que tudo o que tocasse se transformasse em ouro, mas se arrependeu depois de tocar em sua comida, bebida e membros da sua família.**(16)**   

A seção 1.1.5 tocou nessa questão, na qual apontamos a necessidade de uma mudança significativa no modelo padrão de colocar objetivos fixos na máquina. A solução para a situação indesejável posta por Wiener é não ter, de forma alguma, um "propósito definido na máquina". Em vez disso, queremos máquinas que se esforçam para alcançar os objetivos humanos, mas sabendo que não conhecem exatamente quais são esses objetivos.   
Talvez seja lamentável que quase toda a pesquisa em IA até agora tenha sido realizada dentro do modelo padrão, o que significa que quase todo o material técnico desta edição reflete esse arcabouço intelectual. Mas existem alguns resultados iniciais dentro do novo arcabouço.   

No Capítulo 16, mostramos que uma máquina tem um incentivo positivo para permitir que seja desligada se, e somente se, não tiver certeza acerca do objetivo humano. No Capítulo 18, formulamos e estudamos jogos de assistência, que descrevem matematicamente a situação em que um ser humano tem um objetivo e uma máquina tenta alcançá-lo, mas inicialmente não o conhece com certeza. No Capítulo 22, explicamos os métodos de aprendizado por reforço inverso, permitindo que as máquinas aprendam mais sobre as preferências humanas observando as escolhas que os humanos fazem. No Capítulo 27, exploramos duas das principais dificuldades: primeiro, que nossas escolhas dependem de nossas preferências, por meio de uma arquitetura cognitiva muito complexa e difícil de ser invertida; em segundo lugar, que nós, humanos, podemos não ter preferências consistentes em primeiro lugar - seja individualmente ou em grupo -, de modo que pode não ser claro o que os sistemas de IA deveriam estar fazendo por nós.   

## Resumo

Este capítulo define a IA e estabelece os fundamentos culturais sobre os quais ela se desenvolveu. Alguns pontos importantes são:   

- Pessoas diferentes abordam a IA com objetivos diferentes em mente. Duas questões importantes são: Você se preocupa com o raciocínio ou com o comportamento? Você quer modelar seres humanos ou tentar obter resultados ótimos?   

- De acordo com o que chamamos de modelo padrão, a IA se ocupa principalmente da ação racional. No caso ideal, um agente inteligente adota a melhor ação possível em uma situação. Estudaremos o problema da criação de agentes que são inteligentes nesse sentido.   

- É preciso aperfeiçoar duas coisas nessa ideia simples: primeiro, a capacidade de qualquer agente, seja ele humano ou não, de escolher ações racionais é limitada pela intratabilidade computacional de fazê-lo; segundo, o conceito de uma máquina que busca um objetivo definido precisa ser substituído pelo de uma máquina que busca objetivos que beneficiam os humanos, sem que ela saiba ao certo quais são esses objetivos.   

- Os filósofos (desde 400 a.C) tornaram a IA concebível sugerindo que a mente é, em alguns aspectos, semelhante a uma máquina, no sentido de que ela opera sobre o conhecimento codificado em alguma linguagem interna e que o pensamento pode ser usado para escolher quais ações tomar.   

- Os matemáticos forneceram as ferramentas para manipular declarações de certeza lógica, bem como declarações incertas e probabilísticas. Eles também definiram a base para a compreensão da computação e do raciocínio sobre algoritmos.   

- Os economistas formalizaram o problema de tomar decisões que maximizam a utilidade esperada para o tomador de decisões.   

- Os neurocientistas descobriram alguns fatos sobre como o cérebro trabalha e a forma como ele se assemelha e se diferencia dos computadores.   

- Os psicólogos adotaram a ideia de que os seres humanos e os animais podem ser considerados máquinas de processamento de informações. Os linguistas mostraram que o uso da linguagem se ajusta a esse modelo.
  
  --- 

**Nota de rodapé (15)**: 15 Ainda antes disso, em 1847, Richard Thornton, editor do *Primitive* *Expounder*, havia criticado as calculadoras mecanicas: "*A mente (…) ultrapassa a si mesma e elimina a necessidade de sua própria existência, inventando máquinas com pensamento pro prio. (…) Mas quem sabe que tais máquinas, quando aperfeiçoadas, podem não pensar em um plano para remediar todos os seus próprios defeitos e então produzir ideias além do alcance da mente mor*tal!"   
**Nota de rodapé (16)**: Midas teria se saído melhor se tivesse seguido os princípios básicos da segurança e incluído um botão "desfazer" e um botão "pausar" em seu desejo.   

 --- 


