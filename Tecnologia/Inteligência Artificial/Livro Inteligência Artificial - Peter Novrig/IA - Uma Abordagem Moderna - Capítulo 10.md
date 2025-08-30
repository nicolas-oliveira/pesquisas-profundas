# Capítulo 10

# REPRESENTAÇÃO DO CONHECIMENTO

> *Neste capítulo, mostramos como representar diversos fatos sobre o mundo real de uma forma que pode ser usada para raciocinar e resolver problemas.*

Os capítulos anteriores mostraram como um agente com uma base de conhecimento pode fazer inferências que o habilitem a agir adequadamente. Neste capítulo, abordamos a questão de qual conteúdo inserir na base de conhecimento desse agente — como representar fatos sobre o mundo. Usaremos a lógica de primeira ordem como linguagem de representação, mas capítulos posteriores introduzirão diferentes formalismos de representação, como redes hierárquicas de tarefas para raciocínio sobre planos \(Capítulo 11\), redes bayesianas para raciocínio com incerteza \(Capítulo 13\), modelos de Markov para raciocínio ao longo do tempo \(Capítulo 17\) e redes neurais profundas para raciocínio sobre imagens, sons e outros dados \(Capítulo 21\). Independentemente da representação utilizada, os fatos sobre o mundo ainda precisam ser tratados, e este capítulo oferece uma noção das questões.

A Seção 10.1 introduz a ideia de uma ontologia geral, que organiza tudo no mundo em uma hierarquia de categorias. A Seção 10.2 aborda as categorias básicas de objetos, substâncias e medidas; a Seção 10.3 aborda eventos; e a Seção 10.4 discute o conhecimento sobre crenças. Em seguida, voltamos a considerar a tecnologia para o raciocínio com este conteúdo: a Seção 10.5 discute sistemas de raciocínio projetados para inferência eficiente com categorias, e a Seção 10.6 discute o raciocínio com informação *default*.

## 10.1. Engenharia Ontológica

Em domínios de "miniaturas", a escolha da representação não é tão importante; muitas opções funcionarão. Domínios complexos, como fazer compras na internet ou dirigir no trânsito, exigem representações mais gerais e flexíveis. Este capítulo mostra como criar essas representações, concentrando-se em conceitos gerais — como Eventos, Tempo, Objetos Físicos e Crenças — que ocorrem em muitos domínios diferentes. A representação desses conceitos abstratos às vezes é chamada de **engenharia ontológica**.

Não podemos esperar que seja possível representar *tudo* no mundo, nem mesmo um livro didático de mil páginas, mas deixaremos espaços reservados onde novos conhecimentos para qualquer domínio possam se encaixar. Por exemplo, definiremos o que significa ser um objeto físico, e os detalhes de diferentes tipos de objetos — robôs, televisores, livros ou qualquer outro — podem ser preenchidos posteriormente. Isso é análogo à maneira como os designers de um framework de programação orientada a objetos \(como o framework gráfico Java Swing\) definem conceitos gerais como Janela, esperando que os usuários os utilizem para definir conceitos mais específicos, como Janela de Planilha. O framework geral de conceitos é chamado de **ontologia** **superior** devido à convenção de desenhar gráficos com os conceitos gerais no topo e os conceitos mais específicos abaixo deles, como na Figura 10.1.

Antes de considerar a ontologia mais detalhadamente, devemos fazer uma ressalva importante. Optamos por usar a lógica de primeira ordem (**LPO**) para discutir o conteúdo e a organização do conhecimento, embora certos aspectos do mundo real sejam difíceis de capturar na LPO. A principal dificuldade é que a maioria das generalizações tem exceções ou se mantém apenas até certo ponto. Por exemplo, embora "tomates são vermelhos" seja uma regra útil, alguns tomates são verdes, amarelos ou laranja. Exceções semelhantes podem ser encontradas para quase todas as regras neste capítulo. A capacidade de lidar com exceções e incertezas é extremamente importante, mas é ortogonal à tarefa de compreender a ontologia geral. Por esse motivo, adiamos a discussão de exceções para a Seção 10.5 deste capítulo, e o tópico mais geral de raciocínio com incerteza para o Capítulo 12.

---

![](/home/nicolas/.config/marktext/images/2025-06-06-17-12-22-image.png)

> *Figura 10.1 A ontologia superior do mundo, mostrando os tópicos a serem abordados posteriormente neste capítulo. Cada ligação indica que o conceito inferior é uma especialização do superior. Especializações não são necessariamente disjuntas — um ser humano é tanto um animal quanto um agente. Veremos na Seção 10.3.2 por que objetos físicos se enquadram em eventos generalizados.*

---

Qual a utilidade de uma ontologia superior? Considere a ontologia para circuitos da Seção 8.4.2. Ela faz muitas suposições simplificadoras: o tempo é completamente omitido; os sinais são fixos e não se propagam; a estrutura do circuito permanece constante. Uma ontologia mais geral consideraria sinais em momentos específicos e incluiria os comprimentos dos fios e os atrasos de propagação. Isso nos permitiria simular as propriedades de temporização do circuito e, de fato, tais simulações são frequentemente realizadas por projetistas de circuitos.

Poderíamos também introduzir classes de portas mais interessantes, por exemplo, descrevendo a tecnologia \(TTL, CMOS e assim por diante\), bem como a especificação de entrada-saída. Se quiséssemos discutir confiabilidade ou diagnóstico, incluiríamos a possibilidade de que a estrutura do circuito ou as propriedades das portas pudessem mudar espontaneamente. Para levar em conta capacitâncias parasitas, precisaríamos representar a localização dos fios na placa.

Se observarmos o mundo dos wumpus, considerações semelhantes se aplicam. Embora representemos o tempo, ele tem uma estrutura simples: nada acontece, exceto quando o agente age, e todas as mudanças são instantâneas. Uma ontologia mais geral, mais adequada ao mundo real, permitiria mudanças simultâneas estendidas ao longo do tempo. Também usamos um predicado Pit para dizer quais quadrados têm poços. Poderíamos ter permitido diferentes tipos de poços tendo vários indivíduos pertencentes à classe de poços, cada um com propriedades diferentes. Da mesma forma, poderíamos querer permitir outros animais além dos wumpus. Pode não ser possível determinar a espécie exata a partir das percepções disponíveis, então precisaríamos construir uma taxonomia biológica para ajudar o agente a prever o comportamento dos habitantes das cavernas a partir de pistas escassas.

Para qualquer ontologia de propósito específico, é possível fazer mudanças como essas para avançar em direção a uma maior generalidade. Surge então uma pergunta óbvia: todas essas ontologias convergem para uma ontologia de propósito geral? Após séculos de investigação filosófica e computacional, a resposta é "Talvez". Nesta seção, apresentamos uma ontologia de propósito geral que sintetiza ideias desses séculos. Duas características principais das ontologias de propósito geral as distinguem de coleções de ontologias de propósito especial:

- Uma ontologia de propósito geral deve ser aplicável a praticamente qualquer domínio de propósito específico \(com a adição de axiomas específicos do domínio\). Isso significa que nenhuma questão de representação pode ser amenizada ou varrida para debaixo do tapete.

- Em qualquer domínio suficientemente exigente, diferentes áreas do conhecimento devem ser unificadas, pois o raciocínio e a resolução de problemas podem envolver diversas áreas simultaneamente. Um sistema de reparo de circuitos robóticos, por exemplo, precisa raciocinar sobre circuitos em termos de conectividade elétrica e layout físico, e sobre tempo, tanto para análise de temporização de circuitos quanto para estimativa de custos de mão de obra. As sentenças que descrevem o tempo, portanto, devem ser combináveis com aquelas que descrevem o layout espacial e devem funcionar igualmente bem para nanossegundos e minutos, bem como para angstroms e metros.

Devemos dizer desde já que o empreendimento da engenharia ontológica geral teve até agora apenas sucesso limitado. Nenhuma das principais aplicações de IA \(conforme listadas no Capítulo 1\) utiliza uma ontologia geral — todas utilizam engenharia do conhecimento para fins específicos e aprendizado de máquina. Considerações sociais/políticas podem dificultar o acordo entre partes concorrentes sobre uma ontologia. Como afirma Tom Gruber \(2004\), "Toda ontologia é um tratado — um acordo social — entre pessoas com algum motivo comum de compartilhamento". Quando preocupações conflitantes superam a motivação para o compartilhamento, não pode haver uma ontologia comum. Quanto menor o número de partes interessadas, mais fácil é criar uma ontologia e, portanto, é mais difícil criar uma ontologia de propósito geral do que uma de propósito limitado, como a Ontologia Biomédica Aberta \(Smith et al., 2007\). As ontologias existentes foram criadas seguindo quatro caminhos:

1. Por uma equipe de ontologistas ou lógicos treinados, que arquitetam a ontologia e escrevem axiomas. O sistema **CYC** foi construído principalmente dessa maneira \(Lenat e Guha, 1990\).

2. Importando categorias, atributos e valores de um ou mais bancos de dados existentes. O **DBPEDIA** foi criado importando fatos estruturados da Wikipédia \(Bizer et al., 2007\).

3. Analisando documentos de texto e extraindo informações deles. O **TEXTRUNNER** foi criado pela leitura de um grande corpus de páginas da Web \(Banko e Etzioni, 2008\).

4. Incentivando amadores inexperientes a incorporar o conhecimento de senso comum. O sistema **OPENMIND** foi criado por voluntários que propuseram fatos em inglês \(Singh et al., 2002; Chklovski e Gil, 2005\).

Por exemplo, o Google Knowledge Graph utiliza conteúdo semiestruturado da Wikipédia, combinando-o com outros conteúdos coletados na web sob curadoria humana. Ele contém mais de 70 bilhões de fatos e fornece respostas para cerca de um terço das buscas do Google \(Dong et al., 2014\).

## 10.2. Categorias e Objetos

A organização de objetos em **categorias** é uma parte vital da representação do conhecimento. Embora a interação com o mundo ocorra no nível de objetos individuais, grande parte do raciocínio ocorre no nível de categorias. Por exemplo, um comprador normalmente teria o objetivo de comprar uma bola de basquete, em vez de uma bola de basquete específica, como `BB₉`. As categorias também servem para fazer previsões sobre objetos, uma vez que eles são classificados. Infere-se a presença de certos objetos a partir de informações perceptuais, infere-se a pertença à categoria a partir das propriedades percebidas dos objetos e, em seguida, usa-se a informação da categoria para fazer previsões sobre os objetos. Por exemplo, a partir de sua casca manchada de verde e amarelo, diâmetro de 30 cm, formato ovoide, polpa vermelha, sementes pretas e presença no corredor de frutas, pode-se inferir que um objeto é uma melancia; a partir disso, infere-se que seria útil para salada de frutas.

Há duas opções para representar categorias na lógica de primeira ordem: **predicados** e **objetos**. Ou seja, podemos usar o predicado `BolaDeBasquete(b)` ou podemos **reificar**¹ a categoria como um objeto, *BolasDeBasquete*. Poderíamos então dizer `Membro(b, Basquetebol)`, que abreviaremos como `b ∈ Basquetebol`, para dizer que b é um membro da categoria de basquetebol. Dizemos `Subconjunto(Basquetebol,Bolas)`, abreviado como `Basquetebol ⊂ Bolas`, para dizer que Basquetebol é uma subcategoria de Bolas. Usaremos subcategoria, subclasse e subconjunto indistintamente.

Categorias organizam o conhecimento por meio de herança. Se dissermos que todas as instâncias da categoria Alimento são comestíveis e se afirmarmos que Fruta é uma subclasse de Alimento e Maçãs é uma subclasse de Fruta, podemos inferir que toda maçã é comestível. Dizemos que cada maçã herda a propriedade de comestibilidade, neste caso, de sua participação na categoria Alimento.

As relações de subclasse organizam categorias em uma **hierarquia taxonômica** ou taxonomia. As taxonomias têm sido usadas explicitamente há séculos em áreas técnicas. A maior taxonomia desse tipo organiza cerca de 10 milhões de espécies vivas e extintas, muitas delas besouros,² em uma única hierarquia; a biblioteconomia desenvolveu uma taxonomia para todos os campos do conhecimento, codificada como o sistema decimal de Dewey; e as autoridades fiscais e outros departamentos governamentais desenvolveram extensas taxonomias de ocupações e produtos comerciais.

A lógica de primeira ordem facilita a declaração de fatos sobre categorias, seja relacionando objetos a categorias ou quantificando seus membros. Aqui estão alguns exemplos de fatos:

- Um objeto é um membro de uma categoria.
  
  - $BB₉ ∈ Basquete$

- Uma categoria é uma subclasse de outra categoria.
  
  - $BolasDeBasquete ⊂ Bolas$

- Todos os membros de uma categoria têm algumas propriedades.
  
  - $(x ∈ BolasDeBasQuete)\ ⇒\ Esférico(x)$

- Os membros de uma categoria podem ser reconhecidos por algumas propriedades.
  
  - $Laranja(x)∧Redondo(x)∧Diâmetro(x)=23,75cm ∧x∈Bolas\ ⇒\ x ∈ Bolas de Basquete$

- Uma categoria como um todo tem algumas propriedades.
  
  - $Cães ∈ EspéciesDomesticadas$

Observe que, como *Cães* é uma categoria e membro de *EspéciesDomesticadas*, esta última deve ser uma categoria de categorias. É claro que há exceções a muitas das regras acima \(bolas de basquete furadas não são esféricas\); trataremos dessas exceções mais tarde.

Embora as relações de subclasse e membro sejam as mais importantes para categorias, também queremos ser capazes de estabelecer relações entre categorias que não são subclasses umas das outras. Por exemplo, se dissermos apenas que *`EstudantesDeGraduação`* e `EstudantesDePósGraduação` são subclasses de `Estudantes`, então não teríamos dito que um estudante de graduação não pode também ser um estudante de pós-graduação. 

Dizemos que duas ou mais categorias são **disjuntas** se não tiverem membros em comum. Também podemos querer dizer que as classes *`EstudantesDeGraduação`* e `EstudantesDePósGraduação` formam uma **decomposição exaustiva** de `EstudantesUniversitários`. Uma decomposição exaustiva de conjuntos disjuntos é conhecida como uma partição. Aqui estão mais alguns exemplos desses três conceitos:

$$
Disjuntos(\{Animais, Vegetais\}) \\
DecomposicãoExaustiva(\{Americanos, Canadenses, Mexicanos\}, \\ NorteAmericanos) \\

Particão(\{Animais, Plantas, Fungos, Protistas, Moneras\}, \\ SeresVivos)
$$

(Observe que a Decomposição Exaustiva de Norte-Americanos não é uma Partição, porque algumas pessoas têm dupla cidadania.\) Os três predicados são definidos da seguinte forma:

$$
Disjuntos(s) ⇔ (∀ c_1 , c_2 , c_1 ∈ s ∧ c 2 ∈ s ∧ c 1 ≠ c 2 ⇒ Interseção(c_1,c_2)=\{\}\ )
$$

$$
DecomposiçãoExaustiva(s,c)⇔(∀i\  i∈c⇔∃\ c_2\  c_2 ∈\ s∧i∈ c_2)
$$

$$
Partição(s,c)⇔Disjunto(s)\ ∧\ DecomposiçãoExaustiva(s,c)
$$

As categorias também podem ser definidas fornecendo condições necessárias e suficientes para a filiação. Por exemplo, um solteiro é um homem adulto não casado:

$$
x\ ∈\ Solteiros⇔ NãoCasado(x) ∧ x ∈ Adultos ∧ x ∈ Machos
$$

Como discutimos na barra lateral sobre tipos naturais mais adiante na seção 10.2.2., definições lógicas estritas para categorias geralmente são possíveis apenas para termos formais artificiais, não para objetos comuns. Mas definições nem sempre são necessárias.

## 10.2.1. Composição física

A ideia de que um objeto pode ser parte de outro é familiar. O nariz de alguém é parte da cabeça, a Romênia é parte da Europa e este capítulo faz parte deste livro. Usamos a relação geral `ParteDe` para dizer que uma coisa é parte de outra. Objetos podem ser agrupados em hierarquias de `ParteDe`, que lembram a hierarquia de *Subconjuntos*:

`ParteDe(Bucareste, Romênia)` `ParteDe(Romênia, Europa Oriental)` `ParteDe(Europa Oriental, Europa)` `ParteDe(Europa, Terra)`.

A relação `ParteDe` é transitiva e reflexiva; isto é,

$$
ParteDe(x,y)\ ∧\ ParteDe(y,z)⇒ParteDe(x,z)\\ ParteDe(x,x)
$$

Portanto, podemos concluir `ParteDe(Bucareste, Terra)`. Categorias de objetos compostos são frequentemente caracterizadas por relações estruturais entre partes. Por exemplo, um bípede é um objeto

$$
Bípede(a) ⇒ ∃\ l_1,l_2,\  b\  Perna(l_1) ∧ Perna(l_2) ∧ Corpo(b) ∧   \\ 
ParteDe(l_1 ,a) ∧ ParteDe(l_2, a) ∧ ParteDe(b,a) ∧ \\ 
Presa(l_1,b) ∧ Presa(l_2,b) ∧ \\ 
l_1 ≠ l_2 ∧ [∀ l_3 Perna (l_3 )∧ ParteDe(l_3 ,a) ⇒ (l_3 = l_1 ∨ l_3 = l_2 )]. 
$$

A notação corresponde para "exatamente dois" é um pouco estranha; somos forçados a dizer que existem duas pernas, que elas não são iguais e que, se alguém propõe uma terceira perna, ela deveria ser igual a uma das outras duas. Na Seção 10.5.2, descrevemos um formalismo chamado *lógica de descrições* que facilita a representação de restrições como "exatamente dois".

Podemos definir uma relação `PartiçãoDeParte` análoga à relação `Partição` para categorias. Um objeto é composto pelas partes em sua `PartiçãoDeParte` e pode ser visto como derivando algumas propriedades dessas partes. Por exemplo, a massa de um objeto composto é a soma das massas das partes. Observe que este não é o caso com categorias, que não têm massa, embora seus elementos possam ter.

Também é útil definir objetos compostos com partes definidas, mas sem estrutura específica. Por exemplo, podemos dizer "As maçãs neste saco pesam dois quilos". A tentação seria atribuir esse peso ao *conjunto* de maçãs no saco, mas isso seria um erro, pois o conjunto é um conceito matemático abstrato que possui elementos, mas não possui peso. Em vez disso, precisamos de um novo conceito, que chamaremos de cacho. Por exemplo, se as maçãs são `Maçã₁`, `Maçã₂` e `Maçã₃`, então

$$
GrupoDe([Maçã_1, Maçã2_2, Maçã_3])
$$

denota o objeto composto com as três maçãs como partes \(não elementos\). Podemos então usar o cacho como um objeto normal, embora não estruturado. Observe que  $GrupoDe([x]) = x$. Além disso,  $GrupoDe([Maçãs])$  é o objeto composto que consiste em todas as maçãs. Não deve ser confundido com `Maçãs`, a categoria ou conjunto de todas as maçãs.

Podemos definir `ParteDe` em termos da relação `ParteDe`. Obviamente, cada elemento de s é parte de `GrupoDe(s)`:

$$
∀x\ x∈s⇒ParteDe(x,GrupoDe(s)) .
$$

Além disso, `ParteDe(s)` é o menor objeto que satisfaz essa condição. Em outras palavras`GrupoDe(s)` deve fazer parte de qualquer objeto que tenha todos os elementos de s como partes:

$$
∀y\ [∀x\ x∈s⇒ParteDe(x,y)]⇒ParteDe(GrupoDe(s), y) .
$$

Esses axiomas são um exemplo de uma técnica geral chamada minimização lógica, que significa definir um objeto como o menor que satisfaz certas condições.

## 10.2.2 Medições

Tanto nas teorias científicas quanto nas teorias de senso comum, os objetos têm altura, massa, custo e assim por diante. Os valores que atribuímos a essas propriedades são chamados de ***medidas***. 

Medidas quantitativas comuns são bastante fáceis de representar. Imaginamos que o universo inclui "objetos de medida" abstratos, como o comprimento deste segmento de reta: . Podemos chamar esse comprimento de 1,5 polegada ou 3,81 centímetros. Portanto, o mesmo comprimento tem nomes diferentes em nossa língua. Representamos o comprimento com uma função de unidades que recebe um número como argumento.

### Tipos Naturais

> Algumas categorias têm definições rigorosas: um objeto é um triângulo se, e somente se, for um polígono com três lados. Por outro lado, a maioria das categorias no mundo real não tem uma definição clara; estas são chamadas de categorias de tipos naturais. Por exemplo, os tomates tendem a ser de um escarlate opaco; aproximadamente esféricos; com uma reentrância no topo onde estava o caule; cerca de cinco a dez centímetros de diâmetro; com uma casca fina, mas resistente; e com polpa, sementes e suco dentro. No entanto, há variação: alguns tomates são amarelos ou laranja, tomates verdes são verdes, alguns são menores ou maiores que a média e tomates-cereja são uniformemente pequenos. Em vez de ter uma definição completa de tomates, temos um conjunto de características que serve para identificar objetos que são claramente tomates típicos, mas pode não identificar definitivamente outros objetos. \(Poderia haver um tomate que seja peludo como um pêssego?\)
> 
> Isso representa um problema para um agente lógico. O agente não pode ter certeza de que um objeto que percebeu é um tomate e, mesmo que tivesse certeza, não poderia ter certeza de quais das propriedades típicas de tomates este possui. Esse problema é uma consequência inevitável de operar em ambientes parcialmente observáveis.
> 
> Uma abordagem útil é separar o que é verdadeiro para todas as instâncias de uma categoria do que é verdadeiro apenas para instâncias típicas. Assim, além da categoria Tomates, também teremos a categoria Típico\(Tomates\). Aqui, a função Típico mapeia uma categoria para a subclasse que contém apenas instâncias típicas:
> 
> $Típico(c) ⊆ c.$
> 
> A maior parte do conhecimento sobre espécies naturais será, na verdade, sobre suas ocorrências típicas:
> 
> $x ∈Típico(Tomates)⇒Vermelho(x)∧Redondo(x).$
> 
> Assim, podemos anotar fatos úteis sobre categorias sem definições exatas. A dificuldade de fornecer definições exatas para a maioria das categorias naturais foi explicada em detalhes por Wittgenstein \(1953\). Ele usou o exemplo dos jogos para mostrar que os membros de uma categoria compartilhavam "semelhanças de família" em vez de características necessárias e suficientes: qual definição estrita abrange xadrez, pega-pega, paciência e queimada?
> 
> A utilidade da noção de definição estrita também foi questionada por Quine \(1953\). Ele apontou que mesmo a definição de "solteiro" como um homem adulto solteiro é suspeita; pode-se, por exemplo, questionar uma afirmação como "o Papa é solteiro". Embora não seja estritamente falso, esse uso é certamente infeliz, pois induz inferências não intencionais por parte do ouvinte. A tensão talvez pudesse ser resolvida distinguindo-se entre definições lógicas adequadas para representação de conhecimento interno e os critérios mais sutis para uso linguístico feliz. Este último pode ser alcançado "filtrando" as afirmações derivadas do primeiro. Também é possível que falhas no uso linguístico sirvam como feedback para modificar definições internas, de modo que a filtragem se torne desnecessária.

A conversão entre unidades é feita igualando os múltiplos de uma unidade aos de outra:

$Centímetros (2,54×d) = Polegadas (d).$

A conversão entre unidades é feita igualando múltiplos de uma unidade a outra:

Axiomas semelhantes podem ser escritos para libras e quilogramas, segundos e dias, e dólares e centavos. Medidas podem ser usadas para descrever objetos da seguinte forma:

$$
Diâmetro(BolaDeBasquete_12) = Centímetro(23,75) \\
ListaPreço(BolaDeBasquete_12) = \$(19) \\
Peso(GrupoDe(\{Maçã 1, Maçã 2, Maçã 3\})) = Quilos(2) \\
d ∈ Dias ⇒ Duração(d) = Horas(24).
$$ 

Observe que $\$(1)$ não é uma nota de dólar — é um preço. É possível ter duas notas de dólar, mas existe apenas um objeto chamado $\(1\). Observe também que, embora Polegadas\(0\) e Centímetros\(0\) se refiram ao mesmo comprimento zero, eles não são idênticos a outras medidas zero, como Segundos\(0\).

Medidas quantitativas simples são fáceis de representar. Outras medidas apresentam um problema maior, pois não possuem uma escala de valores consensual. Exercícios apresentam dificuldade, sobremesas são deliciosas e poemas são belos, mas não é possível atribuir números a essas qualidades. Pode-se, em um momento de pura contabilidade, descartar tais propriedades como inúteis para fins de raciocínio lógico; ou, pior ainda, tentar impor uma escala numérica à beleza. Isso seria um erro grave, pois é desnecessário. O aspecto mais importante das medidas não são os valores numéricos específicos, mas o fato de que as medidas podem ser ordenadas.

Embora as medidas não sejam números, ainda podemos compará-las usando um símbolo de ordenação como >. Por exemplo, podemos muito bem acreditar que os exercícios de Norvig são mais difíceis que os de Russell, e que uma pontuação menor em exercícios mais difíceis:

e 1 ∈Exercises ∧ e 2 ∈ Exercises ∧Wrote\(Norvig, e 1 \) ∧Wrote\(Russell, e 2 \)⇒ Difficulty\( e 1 \) > Difficulty\( e 2 \). e 1 ∈Exercises ∧ e 2 ∈ Exercises ∧Difficulty\( e 1 \) >Difficulty\( e 2 \)⇒ ExpectedScore\( e 1 \) < ExpectedScore\( e 2 \).

Isso é suficiente para permitir que se decida quais exercícios fazer, mesmo que nunca tenham sido utilizados valores numéricos para a dificuldade. \(No entanto, é preciso descobrir quem escreveu quais exercícios.\) Esses tipos de relações monótonas entre medidas formam a base para o campo da física qualitativa, um subcampo da IA que investiga como raciocinar sobre sistemas físicos sem mergulhar em equações detalhadas e simulações numéricas. A física qualitativa é discutida na seção de notas históricas.

## 10.2.3Objetos: Coisas e coisas

O mundo real pode ser visto como constituído por objetos primitivos \(por exemplo, partículas atômicas\) e objetos compostos construídos a partir deles. Ao raciocinar no nível de objetos grandes, como maçãs e carros, podemos superar a complexidade envolvida em lidar com um vasto número de objetos primitivos individualmente. Há, no entanto, uma parcela significativa da realidade que parece desafiar qualquer individuação óbvia — divisão em objetos distintos. Damos a essa parcela o nome genérico de Individuação coisas. Por exemplo, suponha que eu tenha um pouco de manteiga e um porco-formigueiro à minha frente. Posso dizer Coisas que há um porco-formigueiro, mas não há um número óbvio de "objetos-manteiga", porque qualquer parte de um objeto-manteiga também é um objeto-manteiga, pelo menos até chegarmos a partes muito pequenas. Esta é a principal distinção entre coisas e coisas. Se cortarmos um porco-formigueiro ao meio, não obteremos dois porcos-formis \(infelizmente\).

A língua inglesa distingue claramente entre coisas e coisas. Dizemos "um porco-formigueiro", mas, exceto em restaurantes pretensiosos da Califórnia, não se pode dizer "uma manteiga". Linguistas distinguem entre substantivos contáveis, como porcos-formis, buracos e teoremas, e substantivos massivos, como manteiga, água e energia. Diversas ontologias concorrentes afirmam lidar com essa distinção. Aqui, descrevemos apenas uma; as outras são abordadas na seção de notas históricas.

Para representar as coisas corretamente, começamos com o óbvio. Precisamos ter como objetos em nossa ontologia pelo menos os "pedaços" grosseiros de coisas com as quais interagimos. Por exemplo, podemos reconhecer um pedaço de manteiga como aquele deixado na mesa na noite anterior; podemos pegá-lo, pesá-lo, vendê-lo ou o que for. Nesses sentidos, é um objeto, assim como o porco-formigueiro. Vamos chamá-lo de Manteiga3. Também definimos a categoria Manteiga. Informalmente, seus elementos serão todas aquelas coisas das quais se poderia dizer "É manteiga", incluindo Manteiga3. Com algumas ressalvas sobre partes muito pequenas que omitiremos por enquanto, qualquer parte de um objeto-manteiga também é um objeto-manteiga:

b∈Manteiga∧ParteDe\(p,b\)⇒p∈Manteiga.

Agora podemos dizer que a manteiga derrete a cerca de 30 graus centígrados:

b∈Manteiga⇒Ponto de Fusão \(b, Centígrados\(30\)\).

Poderíamos continuar dizendo que a manteiga é amarela, menos densa que a água, macia à temperatura ambiente, com alto teor de gordura e assim por diante. Por outro lado, a manteiga não tem tamanho, forma ou peso específicos. Podemos definir categorias mais especializadas de manteiga, como ManteigaSemSal, que também é um tipo de substância. Observe que a categoria LibraDeManteiga, que inclui como membros todos os objetos-manteiga que pesam uma libra, não é um tipo de substância. Se cortarmos uma libra de manteiga ao meio, infelizmente não obteremos duas libras de manteiga.

O que realmente acontece é o seguinte: algumas propriedades são intrínsecas: pertencem à própria substância do objeto, e não ao objeto como um todo. Quando você corta uma instância de stuffing ao meio, os dois pedaços retêm as propriedades intrínsecas — coisas como densidade, ponto de ebulição, sabor, cor, propriedade e assim por diante. Por outro lado, suas propriedades extrínsecas — peso, comprimento, forma e assim por diante — não são mantidas sob subdivisão. Uma categoria de objetos que inclui em sua definição apenas propriedades intrínsecas é então uma substância, ou substantivo de massa; uma classe que inclui quaisquer propriedades extrínsecas em sua definição é um substantivo contável. Stuff e Thing são as categorias mais gerais de substância e objeto, respectivamente.

## 10.3 Eventos

Na Seção 7.7.1, discutimos ações: coisas que acontecem, como Shoott; e fluentes: aspectos do mundo que mudam, como HaveArrowt. Ambas foram representadas como proposições, e usamos axiomas de estado sucessor para dizer que um fluente será verdadeiro no instante t \+ 1 se a ação no instante t o tornou verdadeiro, ou se já era verdadeiro no instante t e a ação não o tornou falso. Isso se aplica a um mundo em que as ações são discretas, instantâneas, acontecem uma de cada vez e não apresentam variação na forma como são executadas \(ou seja, existe apenas um tipo de Shootaction, não há distinção entre atirar rapidamente, lentamente, nervosamente, etc.\).

Mas, à medida que passamos de domínios simplistas para o mundo real, há uma gama muito mais rica de ações ou eventos³ com os quais lidar. Considere uma ação contínua, como encher uma banheira. Um axioma do estado sucessor pode dizer que a banheira está vazia antes da ação e cheia quando a ação é realizada, mas não pode falar sobre o que acontece durante a ação. Também não consegue descrever facilmente duas ações ocorrendo ao mesmo tempo — como escovar os dentes enquanto se espera a banheira encher. Para lidar com esses casos, introduzimos uma abordagem conhecida como cálculo de eventos.

Os objetos do cálculo de eventos são eventos, fluentes e pontos no tempo. At\(Shankar, Berkeley\) é um fluente: um objeto que se refere ao fato de Shankar estar em Berkeley. O evento E1 de Shankar voando de São Francisco para Washington, D.C., é descrito como

E 1 ∈Voos ∧Flyer\( E 1 ,Shankar\)∧Origem\( E 1 ,SF\)∧Destino\( E 1 ,DC\) .

onde Flyings é a categoria de todos os eventos de voo. Ao reificar eventos, tornamos possível adicionar qualquer quantidade de informação arbitrária sobre eles. Por exemplo, podemos dizer que o voo de Shankar foi acidentado com Bumpy \(E1\). Em uma ontologia onde eventos são predicados n-ários, não haveria como adicionar informações extras como esta; mudar para um predicado n \+ 1-ário não é uma solução escalável.

Para afirmar que uma fluência é realmente verdadeira a partir de algum ponto no tempo t1 e continuando até o tempo t2, usamos o predicado T, como em T\(At\(Shankar,Berkeley\),t1,t2. Da mesma forma, usamos Happens\(E1,t1,t2\) para dizer que o evento E1 realmente aconteceu, começando no tempo t1 e terminando no tempo t2. O conjunto completo de predicados para uma versão do cálculo de eventos 4 é:

T\(f, t 1, t 2\) Acontece\(e, t 1, t 2\) Inicia\(e, f, t\) Termina\(e, f, t\) Iniciado\(f, t 1, t 2\) Terminado\(f, t 1, t 2\) t 1 < t 2 F fluente é verdadeiro para todos os tempos entre t 1 e t 2 O evento e começa no tempo t 1 e termina em t 2 O evento e faz com que f fluente se torne verdadeiro no tempo t O evento e faz com que f fluente deixe de ser verdadeiro no tempo t F fluente se torna verdadeiro em algum ponto entre t 1 e t 2 F fluente deixa de ser verdadeiro em algum ponto entre t 1 e t 2 O ponto de tempo t 1 ocorre antes do tempo t 2

Podemos descrever os efeitos de um evento de voo:

E = Voa\(a,aqui,ali\)∧Acontece\(E, t 1 , t 2 \)⇒ Termina\(E,Em\(a,aqui\), t 1 \) ∧Inicia\(E,Em\(a,ali\), t 2 \)

Assumimos um evento distinto, Início, que descreve o estado inicial dizendo quais fluentes são verdadeiros \(usando Iniciados\) ou falsos \(usando Terminados\) no momento inicial. Podemos então descrever quais fluentes são verdadeiros em quais pontos no tempo com um par de axiomas para T e ¬T que seguem o mesmo formato geral dos axiomas do estado sucessor: Suponha que um evento aconteça entre os tempos t1 e t3, e em t2, em algum lugar nesse intervalo de tempo, o evento altera o valor do fluente f, iniciando-o \(tornando-o verdadeiro\) ou terminando-o \(tornando-o falso\). Então, no tempo t4 no futuro, se nenhum outro evento interveniente tiver alterado o fluente \(terminado ou iniciado, respectivamente\), então o fluente terá mantido seu valor. Formalmente, os axiomas são:

Acontece\(e, t 1 , t 3 \)∧Inicia\(e,f, t 2 \)∧¬Terminado \(f, t 2 , t 4 \)∧ t 1 ≤ t 2 ≤ t 3 ≤ t 4 ⇒ T\(f, t 2 , t 4 \) Acontece\(e, t 1 , t 3 \)∧Terminado\(e,f, t 2 \)∧¬Inicia \(f, t 2 , t 4 \)∧ t 1 ≤ t 2 ≤ t 3 ≤ t 4 ⇒ ¬T\(f, t 2 , t 4 \)

onde Terminado e Iniciado são definidos por:

Terminado\(f, t 1 , t 5 \)⇔ ∃e, t 2 , t 3 , t 4 Acontece\(e, t 2 , t 4 \)∧Terminado \(e,f, t 3 \)∧ t 1 ≤ t 2 ≤ t 3 ≤ t 4 ≤ t 5 Iniciado\(f, t 1 , t 5 \)⇔ ∃e, t 2 , t 3 , t 4 Acontece\(e, t 2 , t 4 \)∧Inicia \(e,f, t 3 \)∧ t 1 ≤ t 2 ≤ t 3 ≤ t 4 ≤ t 5

Podemos estender o cálculo de eventos para representar eventos simultâneos \(como duas pessoas sendo necessárias para andar de gangorra\), eventos exógenos \(como o vento movendo um objeto\), eventos contínuos \(como a subida da maré\), eventos não determinísticos \(como lançar uma moeda e ela dar cara ou coroa\) e outras complicações.

## 10.3.1Tempo

O cálculo de eventos nos abre a possibilidade de falar sobre pontos e intervalos de tempo. Consideraremos dois tipos de intervalos de tempo: momentos e intervalos estendidos. A diferença é que apenas os momentos têm duração zero:

Partição\(\{Momentos, IntervalosExtendidos\}, Intervalos\) i∈Momentos⇔Duração\(i\)=Segundos\(0\) .

Em seguida, criamos uma escala de tempo e associamos pontos nessa escala a momentos, obtendo tempos absolutos. A escala de tempo é arbitrária; vamos medi-la em segundos e dizer que o momento à meia-noite \(GMT\) de 1º de janeiro de 1900 tem tempo 0. As funções Begin e End selecionam os primeiros e os últimos momentos em um intervalo, e a função Time fornece o ponto na escala de tempo para um momento. A função Duration fornece a diferença entre o horário final e o horário inicial.

Intervalo\(i\) ⇒ Duração\(i\)=\(Tempo\(Fim\(i\)\)−Tempo\(Início\(i\)\)\). Tempo\(Início\(AD1900\)\)=Segundos\(0\). Tempo\(Início\(AD2001\)\)=Segundos\(3187324800\). Tempo\(Fim\(AD2001\)\)=Segundos\(3218860800\). Duração\(AD2001\)=Segundos\(31536000\).

Para facilitar a leitura desses números, também introduzimos uma função Date, que recebe seis argumentos \(horas, minutos, segundos, dia, mês e ano\) e retorna um ponto no tempo:

Hora\(Início\(AD2001\)\)=Data\(0,0,0,1,Jan,2001\) Data\(0,20,21,24,1,1995\)=Segundos\(30000\).

Dois intervalos se encontram se o tempo final do primeiro for igual ao tempo inicial do segundo. O conjunto completo de relações de intervalo \(Allen, 1983\) é mostrado abaixo e na Figura 10.2:

     Descrição 

As relações de intervalo são representadas usando um conjunto de dois blocos rotulados i e j. Encontro \(i, j\): Os blocos i e j se encontram em um ponto comum. Antes \(i, j\) depois \(j, i\): Os blocos i e j são alinhados horizontalmente em uma sequência, a uma pequena distância. Durante \(i, j\): Os blocos i e j são alinhados verticalmente com o bloco i no topo. O bloco j é maior que o bloco i. Sobreposição \(i, j\): Os blocos i e j são alinhados verticalmente de tal forma que o bloco i é alinhado à esquerda e o bloco j é alinhado ligeiramente à direita. O bloco j é maior que o bloco i. Início \(i, j\): Os blocos i e j são alinhados um abaixo do outro à esquerda. O bloco j é maior que o bloco i. Término \(i, j\): Os blocos i e j são alinhados um abaixo do outro à direita. O bloco j é maior que o bloco i. Igual a \(i, j\): Os blocos i e j são alinhados um abaixo do outro. Ambos os blocos têm o mesmo comprimento.

Figura 10.2 Predicados em intervalos de tempo.

Encontro\(i, j\) Antes\(i, j\) Depois\(j, i\) Durante\(i, j\) Sobreposição\(i, j\) Início\(i, j\) Término\(i, j\) É igual a\(i, j\) ⇔ ⇔ ⇔ ⇔ ⇔ ⇔ ⇔ ⇔ Fim\(i\)=Início\( j\) Fim\(i\) < Início\( j\) Antes\(i, j\) Início\(j\) < Início\(i\) < Fim\(i\) < Fim\(i\) < Início\(j\) < Fim\(i\) < Fim\(j\) Início\(i\) = Início\(j\) Fim\(i\) = Fim\(j\) Início\(i\)=Início\(j\)∧Fim\(i\)=Fim\(j\)

Todas elas têm seu significado intuitivo, com exceção de Overlap: tendemos a pensar em overlap como simétrica \(se i sobrepõe j, então j sobrepõe i\), mas nesta definição, Overlap\(i, j\) só é verdadeira se i começar antes de j. A experiência tem mostrado que esta definição é mais útil para escrever axiomas. Para dizer que o reinado de Elizabeth II seguiu imediatamente o de George VI, e o reinado de Elvis coincidiu com a década de 1950, podemos escrever o seguinte:

Encontra\(ReinadoDe \(George VI\),ReinadoDe \(Elizabeth II\)\). Sobrepõe\(Anos Cinquenta,ReinadoDe \(Elvis\)\). Começa\(Anos Cinquenta\)=Começa\(1950 d.C.\). Termina\(Anos Cinquenta\)=Fim\(1959 d.C.\).

## 10.3.2 Fluentes e objetos

Objetos físicos podem ser vistos como eventos generalizados, no sentido de que um objeto físico é um pedaço de espaço-tempo. Por exemplo, os EUA podem ser considerados um evento que começou em 1776 como uma união de 13 estados e continua em andamento hoje como uma união de 50. Podemos descrever as propriedades mutáveis dos EUA usando fluentes estaduais, como População \(EUA\). Uma propriedade dos EUA que muda a cada quatro ou oito anos, exceto por acidentes, é seu presidente. Pode-se propor que Presidente \(EUA\) seja um termo lógico que denota um objeto diferente em momentos diferentes.

Infelizmente, isso não é possível, pois um termo denota exatamente um objeto em uma determinada estrutura de modelo. \(O termo Presidente\(EUA, t\) pode denotar objetos diferentes, dependendo do valor de t, mas nossa ontologia mantém os índices de tempo separados dos fluentes.\) A única possibilidade é que Presidente \(EUA\) denota um único objeto que consiste em pessoas diferentes em momentos diferentes. É o objeto que é George Washington de 1789 a 1797, John Adams de 1797 a 1801 e assim por diante, como na Figura 10.3. Para dizer que George Washington foi presidente ao longo de 1790, podemos escrever

     Descrição 

O Presidente \(EUA "A"\) denota um único objeto que consiste em diferentes pessoas em diferentes épocas. Este modelo é representado por três blocos dispostos diagonalmente, com o comprimento sendo representado pelos anos. O primeiro bloco é rotulado como Washington de 1789 a 1797, o segundo bloco é rotulado como Adams de 1797 a 1801, e o terceiro bloco é rotulado como Jefferson de 1801.

Figura 10.3 Uma visão esquemática do objeto Presidente \(EUA\) para os primeiros anos

T\(É igual a\(Presidente\(EUA\);GeorgeWashington\),Início\(1790 d.C.\),Fim\(1790 d.C.\)\).

Usamos o símbolo de função Equals em vez do predicado lógico padrão =, porque não podemos ter um predicado como argumento para T e porque a interpretação não é que GeorgeWashington e President\(USA\) são logicamente idênticos em 1790; identidade lógica não é algo que pode mudar ao longo do tempo. A identidade está entre os subeventos dos objetos President\(USA\) e GeorgeWashington que são definidos pelo período 1790.

## 10.4 Objetos Mentais e Lógica Modal

Os agentes que construímos até agora têm crenças e podem deduzir novas crenças. No entanto, nenhum deles tem qualquer conhecimento sobre crenças ou sobre dedução. O conhecimento sobre o próprio conhecimento e processos de raciocínio é útil para controlar a inferência. Por exemplo, suponha que Alice pergunte "qual é a raiz quadrada de 1764" e Bob responda "Eu não sei". Se Alice insistir "pense mais", Bob deve perceber que, com um pouco mais de reflexão, essa questão pode de fato ser respondida. Por outro lado, se a pergunta fosse "O presidente está sentado agora?", Bob deve perceber que pensar mais dificilmente ajudará. O conhecimento sobre o conhecimento de outros agentes também é importante; Bob deve perceber que o presidente sabe.

O que precisamos é de um modelo dos objetos mentais que estão na cabeça de alguém \(ou na base de conhecimento de algo\) e dos processos mentais que manipulam esses objetos mentais. O modelo não precisa ser detalhado. Não precisamos ser capazes de prever quantos milissegundos um agente específico levará para fazer uma dedução. Ficaremos felizes apenas em poder concluir que a mãe sabe se está sentada ou não.

Começamos com as atitudes proposicionais que um agente pode ter em relação a objetos mentais: atitudes como Acredita, Sabe, Quer e Informa. A dificuldade reside no fato de que essas atitudes não se comportam como predicados "normais". Por exemplo, suponhamos que tentamos afirmar que Lois sabe que o Superman pode voar:

Sabe\(Lois,CanFly\(Superman\)\).

Um pequeno problema com isso é que normalmente pensamos em CanFly \(Superman\) como uma frase, mas aqui ele aparece como um termo. Esse problema pode ser corrigido reificando CanFly \(Superman\), tornando-o fluente. Um problema mais sério é que, se for verdade que Superman é Clark Kent, então devemos concluir que Lois sabe que Clark pode voar, o que é errado porque \(na maioria das versões da história\) Lois não sabe que Clark é Superman.

\(Superman=Clark\)∧Sabe\(Lois,PodeVoar\(Superman\)\) ⊨Sabe\(Lois,PodeVoar\(Clark\)\)

Isso é consequência do fato de o raciocínio de igualdade estar embutido na lógica. Normalmente, isso é algo positivo; se nosso agente sabe que 2 \+ 2 = 4 e 4 < 5, então queremos que nosso agente saiba que 2 \+ 2 < 5. Essa propriedade é chamada de transparência referencial — não importa qual termo uma lógica usa para se referir a um objeto, o que importa é o objeto que o termo nomeia. Mas para atitudes proposicionais como "acredita" e "sabe", gostaríamos de ter opacidade referencial — os termos usados importam, porque nem todos os agentes sabem quais termos são correferenciais.

Poderíamos remediar isso com ainda mais reificação: poderíamos ter um objeto para representar Clark/Superman, outro objeto para representar a pessoa que Lois conhece como Clark e ainda outro para a pessoa que Lois conhece como Superman. No entanto, essa proliferação de objetos significa que as frases que queremos escrever rapidamente se tornam prolixas e desajeitadas.

A lógica modal foi projetada para resolver esse problema. A lógica regular se preocupa com uma única modalidade, a modalidade da verdade, permitindo-nos expressar "P é verdadeiro" ou "P é falso". A lógica modal inclui operadores modais especiais que usam sentenças \(em vez de termos\) como argumentos. Por exemplo, "A conhece P" é representado pela notação KAP, onde K é o operador modal para conhecimento. Ela usa dois argumentos: um agente \(escrito como subscrito\) e uma sentença. A sintaxe da lógica modal é a mesma da lógica de primeira ordem, exceto que sentenças também podem ser formadas com operadores modais.

A semântica da lógica modal é mais complexa. Na lógica de primeira ordem, um modelo contém um conjunto de objetos e uma interpretação que mapeia cada nome para o objeto, relação ou função apropriados. Na lógica modal, queremos ser capazes de considerar tanto a possibilidade de a identidade secreta do Superman ser Clark quanto a possibilidade de não ser.

Portanto, precisaremos de um modelo mais complexo, que consista em uma coleção de mundos possíveis, em vez de apenas um mundo verdadeiro. Os mundos são conectados em um grafo por relações de acessibilidade, uma relação para cada operador modal. Dizemos que o mundo w1 é acessível a partir do mundo w0 em relação ao operador modal KA se tudo em w1 for consistente com o que A sabe em w0. Por exemplo, no mundo real, Bucareste é a capital da Romênia, mas para um agente que não soubesse disso, um mundo onde a capital da Romênia é, digamos, Sófia é acessível. Esperançosamente, um mundo onde 2 \+ 2 = 5 não seria acessível a nenhum agente.

Em geral, um átomo de conhecimento KAP é verdadeiro no mundo w se, e somente se, P for verdadeiro em todos os mundos acessíveis a partir de w. A verdade de sentenças mais complexas é derivada pela aplicação recursiva desta regra e das regras normais da lógica de primeira ordem. Isso significa que a lógica modal pode ser usada para raciocinar sobre sentenças de conhecimento aninhadas: o que um agente sabe sobre o conhecimento de outro agente. Por exemplo, podemos dizer que, embora Lois não saiba se a identidade secreta do Superman é Clark Kent, ela sabe que Clark sabe:

K Lois \[ K Clark Identidade\(Superman,Clark\)∨ K Clark ¬Identidade\(Superman;Clark\)\]

A lógica modal resolve alguns problemas complexos com a interação de quantificadores e conhecimento. A frase em inglês "Bond sabe que alguém é um espião" é ambígua. A primeira leitura é que existe um alguém específico que Bond sabe que é um espião; podemos escrever isso como

∃x K Bond Spy\(x\),

o que, na lógica modal, significa que existe um x que, em todos os mundos acessíveis, Bond sabe ser um espião. A segunda leitura é que Bond simplesmente sabe que existe pelo menos um espião:

K Bond ∃x Spy\(x\).

A interpretação da lógica modal é que em cada mundo acessível há um x que é um espião, mas não precisa ser o mesmo x em cada mundo.

Agora que temos um operador modal para conhecimento, podemos escrever axiomas para ele. Primeiro, podemos dizer que os agentes são capazes de tirar conclusões; se um agente conhece P e sabe que P implica Q, então o agente conhece Q:

\( K a P∧ K a \(P⇒Q\)\)⇒ K a Q.

A partir disso \(e de algumas outras regras sobre identidades lógicas\), podemos estabelecer que KA\(P ∨ ¬P\) é uma tautologia; todo agente sabe que toda proposição P é verdadeira ou falsa. Por outro lado, \(KAP\) ∨ \(KA ¬P\) não é uma tautologia; em geral, haverá muitas proposições que um agente desconhece como verdadeiras e desconhece como falsas.

Diz-se \(voltando a Platão\) que o conhecimento é uma crença verdadeira justificada. Ou seja, se for verdade, se você acredita nela e se tiver uma razão inquestionavelmente boa, então você sabe. Isso significa que, se você sabe algo, deve ser verdade, e temos o axioma:

K a P⇒P.

Além disso, agentes lógicos \(mas não todas as pessoas\) são capazes de introspectar seu próprio conhecimento. Se sabem algo, então sabem que sabem:

K a P⇒ K a \( K a P\).

Podemos definir axiomas semelhantes para a crença \(frequentemente denotada por B\) e outras modalidades. No entanto, um problema com a abordagem da lógica modal é que ela pressupõe onisciência lógica por parte dos agentes. Ou seja, se um agente conhece um conjunto de axiomas, então ele conhece todas as consequências desses axiomas. Isso é instável mesmo para a noção um tanto abstrata de conhecimento, mas parece ainda pior para a crença, porque a crença tem mais conotação de se referir a coisas que são fisicamente representadas no agente, não apenas potencialmente deriváveis.

Houve tentativas de definir uma forma de racionalidade limitada para agentes — dizer que os agentes acreditam apenas naquelas afirmações que podem ser derivadas com a aplicação de não mais do que k etapas de raciocínio, ou não mais do que s segundos de computação. Essas tentativas foram geralmente insatisfatórias.

## 10.4.1Outras lógicas modais

Muitas lógicas modais foram propostas, para diferentes modalidades além do conhecimento. Uma proposta é adicionar operadores modais para possibilidade e necessidade: é possivelmente verdade que um dos autores deste livro esteja sentado neste momento, e é necessariamente verdade que 2 \+ 2 = 4.

Como mencionado na Seção 8.1.2, alguns lógicos favorecem modalidades relacionadas ao tempo. Na lógica temporal linear, adicionamos os seguintes operadores modais:

• X P: “P será verdadeiro no próximo passo de tempo”

• F P: “P eventualmente \(finalmente\) será verdadeiro em algum passo de tempo futuro”

• G P: “P é sempre \(globalmente\) verdadeiro”

•P U Q: “P permanece verdadeiro até que Q ocorra”

Às vezes, há operadores adicionais que podem ser derivados destes. Adicionar esses operadores modais torna a lógica em si mais complexa \(e, portanto, dificulta a demonstração por um algoritmo de inferência lógica\). Mas os operadores também nos permitem apresentar certos fatos de forma mais sucinta \(o que torna a inferência lógica mais rápida\). A escolha de qual lógica usar é semelhante à escolha de qual linguagem de programação usar: escolha uma que seja apropriada para sua tarefa, que seja familiar a você e aos outros que compartilharão seu trabalho e que seja eficiente o suficiente para seus propósitos.

## 10.5Sistemas de raciocínio para categorias

Categorias são os principais blocos de construção de esquemas de representação de conhecimento em larga escala. Esta seção descreve sistemas especialmente projetados para organizar e raciocinar com categorias. Existem duas famílias de sistemas intimamente relacionadas: redes semânticas fornecem recursos gráficos para visualizar uma base de conhecimento e algoritmos eficientes para inferir propriedades de um objeto com base em sua associação a uma categoria; e lógicas descritivas fornecem uma linguagem formal para construir e combinar definições de categorias e algoritmos eficientes para decidir relações de subconjuntos e superconjuntos entre categorias.

## 10.5.1Redes semânticas

Em 1909, Charles S. Peirce propôs uma notação gráfica de nós e arestas, chamada de grafos existenciais, que ele chamou de "a lógica do futuro". Assim começou um longo debate entre os defensores da "lógica" e os defensores das "redes semânticas". Infelizmente, o debate obscureceu o fato de que as redes semânticas são uma forma de lógica. A notação que as redes semânticas fornecem para certos tipos de sentenças costuma ser mais conveniente, mas, se eliminarmos as questões da "interface humana", os conceitos subjacentes — objetos, relações, quantificação e assim por diante — são os mesmos.

Existem muitas variantes de redes semânticas, mas todas são capazes de representar objetos individuais, categorias de objetos e relações entre objetos. Uma notação gráfica típica exibe nomes de objetos ou categorias em ovais ou caixas e os conecta com links rotulados. Por exemplo, a Figura 10.4 tem um link MemberOf entre Mary e FemalePersons, correspondendo à asserção lógica Mary ∈ FemalePersons; da mesma forma, o link SisterOf entre Mary e John corresponde à asserção SisterOf \(Mary, John\). Podemos conectar categorias usando links SubsetOf, e assim por diante. É tão divertido desenhar bolhas e setas que podemos nos deixar levar. Por exemplo, sabemos que pessoas têm mães mulheres, então podemos desenhar um link HasMother de Persons para FemalePersons? A resposta é não, porque HasMother é uma relação entre uma pessoa e sua mãe, e categorias não têm mães.5

Por esta razão, usamos uma notação especial — o link de caixa dupla — na Figura 10.4. Este link afirma que

∀x x∈Pessoas⇒\[∀y TemMãe\(x,y\)⇒y∈PessoasMulheres\].

Poderíamos também querer afirmar que as pessoas têm duas pernas, isto é,

∀x x∈Pessoas⇒Pernas\(x,2\).

Como antes, precisamos ter cuidado para não afirmar que uma categoria tem pernas; o link de caixa única na Figura 10.4 é usado para afirmar propriedades de cada membro de uma categoria.

     Descrição 

A rede de herança de baixo para cima é a seguinte. Duas setas rotuladas Membro de apontam de Maria e João para as categorias Pessoas do sexo feminino e Pessoas do sexo masculino, respectivamente. Uma seta rotulada Pernas aponta de João para 1, enquanto outra seta rotulada Irmã de aponta de Maria para João. Duas setas rotuladas Subconjunto de de cada categoria, Pessoas do sexo masculino e Pessoas do sexo feminino, apontam para a categoria rotulada Pessoas. Uma seta rotulada Tem Mãe de Pessoas aponta de volta para Pessoas do sexo feminino. Outra seta rotulada Pernas aponta de Pessoas para 2. Uma seta rotulada Subconjunto de aponta da categoria Pessoas para a categoria rotulada Mamíferos.

Figura 10.4 Uma rede semântica com quatro objetos \(João, Maria, 1 e 2\) e quatro categorias. As relações são indicadas por links rotulados.

A notação de rede semântica facilita a execução de raciocínios de herança do tipo apresentado na Seção 10.2. Por exemplo, por ser uma pessoa, Mary herda a propriedade de ter duas pernas. Assim, para descobrir quantas pernas Mary possui, o algoritmo de herança segue o elo MemberOf de Mary até a categoria à qual ela pertence e, em seguida, segue os elos SubsetOf na hierarquia até encontrar uma categoria para a qual exista um elo Legs em caixa — neste caso, a categoria Persons. A simplicidade e a eficiência desse mecanismo de inferência, em comparação com a demonstração de teoremas lógicos semidecidíveis, têm sido um dos principais atrativos das redes semânticas.

A herança se torna complicada quando um objeto pode pertencer a mais de uma categoria ou quando uma categoria pode ser um subconjunto de mais de uma categoria; isso é chamado de herança múltipla. Nesses casos, o algoritmo de herança pode encontrar dois ou mais valores conflitantes respondendo à consulta. Por esse motivo, a herança múltipla é proibida em algumas linguagens de programação orientada a objetos \(POO\), como Java, que usam herança em uma hierarquia de classes. Geralmente, ela é permitida em redes semânticas, mas adiaremos a discussão sobre isso para a Seção 10.6.

O leitor pode ter notado uma desvantagem óbvia da notação de rede semântica, em comparação com a lógica de primeira ordem: o fato de que as ligações entre bolhas representam apenas relações binárias. Por exemplo, a frase Fly \(Shankar, NewYork, NewDelhi, Yesterday\) não pode ser afirmada diretamente em uma rede semântica. No entanto, podemos obter o efeito de asserções n-árias reificando a própria proposição como um evento pertencente a uma categoria de eventos apropriada. A Figura 10.5 mostra a estrutura da rede semântica para este evento específico. Observe que a restrição a relações binárias força a criação de uma ontologia rica de conceitos reificados.

     Descrição 

Uma seta rotulada "Membro de" aponta da bolha "Voar" \(subscrito 17\) para a bolha "Voar" \(Eventos\). Uma seta rotulada "Agente" \(subscrito 17\) aponta para a bolha "Shankar". Uma seta rotulada "Origem" \(subscrito 17\) aponta da bolha "Voar" \(subscrito 17\) para a bolha "Nova York". Uma seta rotulada "Destino" \(subscrito 17\) aponta para a bolha "Nova Déli". Uma seta rotulada "Durante" \(subscrito 17\) aponta para a bolha "Ontem".

Figura 10.5Um fragmento de uma rede semântica mostrando a representação da afirmação lógica Fly\(Shankar, NewYork, NewDelhi, Yesterday\).

A reificação de proposições torna possível representar cada sentença atômica fundamental e livre de funções da lógica de primeira ordem na notação de rede semântica. Certos tipos de sentenças universalmente quantificadas podem ser afirmadas usando ligações inversas e as setas com caixa simples e dupla aplicadas a categorias, mas isso ainda nos deixa muito aquém da lógica de primeira ordem completa. Negação, disjunção, símbolos de função aninhados e quantificação existencial estão todos ausentes. Agora é possível estender a notação para torná-la equivalente à lógica de primeira ordem — como nos grafos existenciais de Peirce — mas isso nega uma das principais vantagens das redes semânticas, que é a simplicidade e a transparência dos processos de inferência. Os projetistas podem construir uma rede grande e ainda ter uma boa ideia sobre quais consultas serão eficientes, porque \(a\) é fácil visualizar as etapas pelas quais o procedimento de inferência passará e \(b\) em alguns casos a linguagem de consulta é tão simples que consultas difíceis não podem ser propostas.

Nos casos em que o poder expressivo se mostra muito limitado, muitos sistemas de redes semânticas fornecem a anexação procedural para preencher as lacunas. A anexação procedural é uma técnica pela qual uma consulta sobre \(ou, às vezes, uma afirmação sobre\) uma determinada relação resulta em uma chamada a um procedimento especial projetado para essa relação, em vez de um algoritmo de inferência geral.

Um dos aspectos mais importantes das redes semânticas é sua capacidade de representar valores padrão para categorias. Examinando a Figura 10.4 cuidadosamente, percebe-se que John tem uma perna, apesar de ser uma pessoa e todas as pessoas terem duas pernas. Em uma KB estritamente lógica, isso seria uma contradição, mas em uma rede semântica, a afirmação de que todas as pessoas têm duas pernas tem apenas o status padrão; isto é, presume-se que uma pessoa tenha duas pernas, a menos que isso seja contrariado por informações mais específicas. A semântica padrão é imposta naturalmente pelo algoritmo de herança, pois ele segue os links para cima a partir do próprio objeto \(John, neste caso\) e para assim que encontra um valor. Dizemos que o padrão é substituído pelo valor mais específico. Observe que também poderíamos substituir o número padrão de pernas criando uma categoria de OneLeggedPersons, um subconjunto de Persons do qual John é membro.

Podemos manter uma semântica estritamente lógica para a rede se dissermos que a afirmação Legs para Pessoas inclui uma exceção para John:

∀x x∈Pessoas∧x≠João⇒Perna\(x,2\).

Para uma rede fixa, isso é semanticamente adequado, mas será muito menos conciso do que a própria notação de rede se houver muitas exceções. Para uma rede que será atualizada com mais asserções, no entanto, essa abordagem falha — queremos dizer que quaisquer pessoas ainda desconhecidas com uma perna também são exceções. A Seção 10.6 aprofunda essa questão e o raciocínio padrão em geral.

## 10.5.2 Lógicas de descrição

A sintaxe da lógica de primeira ordem foi projetada para facilitar a descrição de objetos. Lógicas descritivas são notações projetadas para facilitar a descrição de definições e propriedades de categorias. Os sistemas de lógica descritiva evoluíram a partir de redes semânticas em resposta à pressão para formalizar o significado das redes, mantendo a ênfase na estrutura taxonômica como princípio organizador.

As principais tarefas de inferência para lógicas descritivas são subsunção \(verificar se uma categoria é um subconjunto de outra comparando suas definições\) e classificação \(verificar se um objeto pertence a uma categoria\). Alguns sistemas também incluem a consistência da definição de uma categoria — se os critérios de associação são logicamente satisfatórios.

A linguagem CLÁSSICA \(Borgida et al., 1989\) é uma lógica descritiva típica. A sintaxe das descrições CLÁSSICAS é mostrada na Figura 10.6.6. Por exemplo, para dizer que solteiros são homens adultos não casados, escreveríamos

Figura 10.6 Sintaxe de descrições em um subconjunto da linguagem CLASSIC.

Solteiro = E \(Solteiro, Adulto, Masculino\).

O equivalente na lógica de primeira ordem seria

Solteiro\(x\) ⇔ Solteiro\(x\)∧Adulto\(x\)∧Masculino\(x\).

Observe que a lógica descritiva possui uma álgebra de operações sobre predicados, o que, obviamente, não podemos fazer na lógica de primeira ordem. Qualquer descrição em CLASSIC pode ser traduzida em uma frase equivalente de primeira ordem, mas algumas descrições são mais diretas em CLASSIC. Por exemplo, para descrever o conjunto de homens com pelo menos três filhos, todos desempregados e casados com médicos, e no máximo duas filhas, todas professoras em departamentos de física ou matemática, usaríamos

E\(Homem, Pelo Menos\(3,Filho\), No Máximo\(2,Filha\), Todos\(Filho,E\(Desempregado,Casado, Todos\(Cônjuge,Médico\)\)\), Todos\(Filha,E\(Professor,Preenche\(Departamento, Física,Matemática\)\)\)\).

Deixamos como exercício traduzir isso para lógica de primeira ordem.

Talvez o aspecto mais importante das lógicas descritivas seja sua ênfase na tratabilidade da inferência. Uma instância problemática é resolvida descrevendo-a e, em seguida, perguntando se ela é subsumida por uma das várias categorias de solução possíveis. Em sistemas lógicos de primeira ordem padrão, prever o tempo de solução é frequentemente impossível. Frequentemente, cabe ao usuário projetar a representação para contornar conjuntos de sentenças que parecem estar fazendo com que o sistema leve várias semanas para resolver um problema. A essência das lógicas descritivas, por outro lado, é garantir que o teste de subsunção possa ser resolvido em tempo polinomial no tamanho das descrições.7

Isso parece maravilhoso em princípio, até que se perceba que só pode ter uma de duas consequências: ou problemas difíceis não podem ser enunciados de forma alguma, ou exigem descrições exponencialmente grandes\! No entanto, os resultados de tratabilidade esclarecem que tipos de construções causam problemas e, portanto, ajudam o usuário a entender como diferentes representações se comportam. Por exemplo, lógicas descritivas geralmente não possuem negação e disjunção. Cada uma força sistemas lógicos de primeira ordem a passar por uma análise de caso potencialmente exponencial para garantir a completude. CLASSIC permite apenas uma forma limitada de disjunção nas construções Fills e OneOf, que permitem disjunção sobre indivíduos explicitamente enumerados, mas não sobre descrições. Com descrições disjuntivas, definições aninhadas podem levar facilmente a um número exponencial de rotas alternativas pelas quais uma categoria pode subsumir outra.

## 10.6 Raciocínio com informações padrão

Na seção anterior, vimos um exemplo simples de uma asserção com status padrão: as pessoas têm duas pernas. Esse padrão pode ser substituído por informações mais específicas, como a de que Long John Silver tem uma perna. Vimos que o mecanismo de herança em redes semânticas implementa a substituição de padrões de forma simples e natural. Nesta seção, estudamos padrões de forma mais geral, com o objetivo de compreender a semântica dos padrões, em vez de apenas fornecer um mecanismo procedural.

## 10.6.1 Circunscrição e lógica padrão

Vimos dois exemplos de processos de raciocínio que violam a propriedade de monotonicidade da lógica, demonstrada no Capítulo 7.8. Neste capítulo, vimos que uma propriedade herdada por todos os membros de uma categoria em uma rede semântica pode ser substituída por informações mais específicas para uma subcategoria. Na Seção 9.4.4, vimos que, sob a suposição de mundo fechado, se uma proposição a não for mencionada em KB, então KB ⊨ ¬α, mas KB ∧ α ⊨ α.

A introspecção simples sugere que essas falhas de monotonicidade são comuns no raciocínio de senso comum. Parece que os humanos frequentemente "tiram conclusões precipitadas". Por exemplo, quando se vê um carro estacionado na rua, normalmente se está disposto a acreditar que ele tem quatro rodas, mesmo que apenas três estejam visíveis. Ora, a teoria da probabilidade pode certamente fornecer uma conclusão de que a quarta roda existe com alta probabilidade; no entanto, para a maioria das pessoas, a possibilidade de o carro não ter quatro rodas não surgirá a menos que alguma nova evidência se apresente. Assim, parece que a conclusão das quatro rodas é alcançada por padrão, na ausência de qualquer razão para duvidar dela. Se novas evidências surgirem — por exemplo, se alguém vir o proprietário carregando uma roda e perceber que o carro está levantado — então a conclusão pode ser retratada. Diz-se que esse tipo de raciocínio exibe não monotonicidade, porque o conjunto de crenças não cresce monotonicamente ao longo do tempo à medida que novas evidências surgem. Lógicas não monotônicas foram concebidas com noções modificadas de verdade e implicação para capturar tal comportamento. Analisaremos duas dessas lógicas que foram extensivamente estudadas: a lógica de circunscrição e a lógica padrão.

A circunscrição pode ser vista como uma versão mais poderosa e precisa da suposição de mundo fechado. A ideia é especificar predicados particulares que se supõe serem "tão falsos quanto possível" — isto é, falsos para todos os objetos, exceto aqueles para os quais se sabe que são verdadeiros. Por exemplo, suponha que queremos afirmar a regra padrão de que os pássaros voam. Introduziríamos um predicado, digamos Abnormal1\(x\), e escreveríamos

Pássaro\(x\)∧¬Anormalidade l 1 \(x\)⇒Moscas\(x\).

Se dissermos que Anormal1 deve ser circunscrito, um raciocinador circunscritivo tem o direito de assumir ¬Anormal1\(x\), a menos que se saiba que Anormal1\(x\) é verdadeiro. Isso permite que a conclusão Voa\(Piu\) seja tirada da premissa Pássaro\(Piu\), mas a conclusão deixa de ser válida se Anormal1\(Piu\) for afirmado.

A circunscrição pode ser vista como um exemplo de lógica de preferência de modelo. Em tais lógicas, uma sentença é implicada \(com status padrão\) se for verdadeira em todos os modelos preferenciais da KB, em oposição ao requisito de verdade em todos os modelos da lógica clássica. Para a circunscrição, um modelo é preferível a outro se tiver menos objetos anormais.9 Vejamos como essa ideia funciona no contexto de herança múltipla em redes semânticas. O exemplo padrão para o qual a herança múltipla é problemática é chamado de "diamante de Nixon". Ele surge da observação de que Richard Nixon era tanto um quaker \(e, portanto, por padrão, um pacifista\) quanto um republicano \(e, portanto, por padrão, não um pacifista\). Podemos escrever isso da seguinte forma:

Republicano\(Nixon\)∧Quaker\(Nixon\). Republicano\(x\)∧¬Anormal l 2 \(x\)⇒¬Pacifista\(x\). Quaker\(x\)∧¬Anormal l 3 \(x\)⇒Pacifista\(x\).

Se circunscrevermos Abnormal2 e Abnormal3, há dois modelos preferenciais: um em que Abnormal2\(Nixon\) e Pacifista\(Nixon\) são verdadeiros e outro em que Abnormal3\(Nixon\) e ¬Pacifista\(Nixon\) são verdadeiros. Assim, o raciocínio circunscritivo permanece apropriadamente agnóstico quanto à possibilidade de Nixon ser pacifista. Se desejarmos, além disso, afirmar que as crenças religiosas têm precedência sobre as crenças políticas, podemos usar um formalismo chamado circunscrição priorizada para dar preferência a modelos em que Abnormal3 é minimizado.

A lógica padrão é um formalismo no qual regras padrão podem ser escritas para gerar conclusões contingentes e não monotônicas. Uma regra padrão se parece com isto:

Pássaro\(x\) : Moscas\(x\)/Moscas\(x\).

Esta regra significa que se Bird\(x\) for verdadeiro e se Flies\(x\) for consistente com a base de conhecimento, então Flies\(x\) pode ser concluído por padrão. Em geral, uma regra padrão tem a forma

P: J 1 ,…, J n /C

onde P é chamado de pré-requisito, C é a conclusão e Ji são as justificativas — se qualquer uma delas puder ser provada falsa, então a conclusão não pode ser tirada. Qualquer variável que apareça em Ji ou C também deve aparecer em P. O exemplo do diamante de Nixon pode ser representado na lógica padrão com um fato e duas regras padrão:

Republicano\(Nixon\)∧Quaker\(Nixon\). Republicano\(x\):¬Pacifista\(x\)/¬Pacifista\(x\). Quaker\(x\):Pacifista\(x\)/Pacifista\(x\).

Para interpretar o significado das regras padrão, definimos a noção de extensão de uma teoria padrão como um conjunto máximo de consequências da teoria. Ou seja, uma extensão S consiste nos fatos originais conhecidos e em um conjunto de conclusões das regras padrão, de modo que nenhuma conclusão adicional possa ser extraída de S, e as justificativas de cada conclusão padrão em S sejam consistentes com S. Como no caso dos modelos preferenciais na circunscrição, temos duas extensões possíveis para o diamante de Nixon: uma em que ele é pacifista e outra em que ele não é. Existem esquemas priorizados nos quais algumas regras padrão podem ter precedência sobre outras, permitindo que algumas ambiguidades sejam resolvidas.

Desde 1980, quando as lógicas não monotônicas foram propostas pela primeira vez, houve um grande progresso na compreensão de suas propriedades matemáticas. No entanto, ainda há questões não resolvidas. Por exemplo, se "Carros têm quatro rodas" é falso, o que significa tê-lo em nossa base de conhecimento? Qual é um bom conjunto de regras padrão para se ter? Se não podemos decidir, para cada regra separadamente, se ela pertence à nossa base de conhecimento, então temos um sério problema de não modularidade. Finalmente, como crenças que têm status padrão podem ser usadas para tomar decisões? Esta é provavelmente a questão mais difícil para o raciocínio padrão.

Decisões frequentemente envolvem compensações e, portanto, é necessário comparar a força da crença nos resultados de diferentes ações e os custos de tomar uma decisão errada. Em casos em que os mesmos tipos de decisões são tomados repetidamente, é possível interpretar regras padrão como declarações de "probabilidade limite". Por exemplo, a regra padrão "Meus freios estão sempre OK" na verdade significa "A probabilidade de que meus freios estejam OK, dada nenhuma outra informação, é suficientemente alta para que a decisão ótima seja dirigir sem verificá-los". Quando o contexto da decisão muda — por exemplo, quando alguém está dirigindo um caminhão muito carregado em uma estrada íngreme na montanha — a regra padrão repentinamente se torna inadequada, mesmo que não haja novas evidências de freios defeituosos. Essas considerações levaram pesquisadores a considerar como incorporar o raciocínio padrão à teoria da probabilidade ou à teoria da utilidade.

## 10.6.2Sistemas de manutenção da verdade

Vimos que muitas das inferências extraídas por um sistema de representação de conhecimento terão apenas o status padrão, em vez de serem absolutamente certas. Inevitavelmente, alguns desses fatos inferidos se revelarão incorretos e terão que ser retratados diante de novas informações. Esse processo é chamado de revisão de crenças.10 Suponha que uma base de conhecimento KB contenha uma sentença P — talvez uma conclusão padrão registrada por um algoritmo de encadeamento progressivo, ou talvez apenas uma afirmação incorreta — e queremos executar TELL\(KB, ¬P\). Para evitar criar uma contradição, devemos primeiro executar RETRACT\(KB, P\). Isso parece bastante fácil. Problemas surgem, no entanto, se quaisquer sentenças adicionais forem inferidas de P e afirmadas na KB. Por exemplo, a implicação P ⇒ Q poderia ter sido usada para adicionar Q. A "solução" óbvia — retirar todas as sentenças inferidas de P — falha porque tais sentenças podem ter outras justificativas além de P. Por exemplo, se R e R ⇒ Q também estiverem na Base de Conhecimento, então Q não precisa ser removido. Sistemas de manutenção da verdade, ou TMSs, são projetados para lidar exatamente com esse tipo de complicação.

Uma abordagem simples para a manutenção da verdade é acompanhar a ordem em que as sentenças são transmitidas à base de conhecimento, numerando-as de P1 a Pn. Quando a chamada RETRACT\(KB, Pi\) é feita, o sistema retorna ao estado imediatamente anterior à adição de Pi, removendo Pi e quaisquer inferências derivadas de Pi. As sentenças de Pi\+1 a Pn podem então ser adicionadas novamente. Isso é simples e garante que a base de conhecimento seja consistente, mas a retração de Pi requer a retração e a reafirmação de n – i sentenças, bem como a desfeita e a refeita de todas as inferências extraídas dessas sentenças. Para sistemas aos quais muitos fatos estão sendo adicionados — como grandes bancos de dados comerciais — isso é impraticável.

Uma abordagem mais eficiente é o sistema de manutenção da verdade baseado em justificação, ou JTMS. Em um JTMS, cada sentença na base de conhecimento é anotada com uma justificação que consiste no conjunto de sentenças das quais foi inferida. Por exemplo, se a base de conhecimento já contiver P ⇒ Q, então TELL\(P\) fará com que Q seja adicionado com a justificação \{P, P ⇒ Q\}. Em geral, uma sentença pode ter qualquer número de justificações. Justificativas tornam a retração eficiente. Dada a chamada RETRACT\(P\), o JTMS excluirá exatamente as sentenças para as quais P é membro de todas as justificações. Portanto, se uma sentença Q tivesse a justificação única \{P, P ⇒ Q\}, ela seria removida; se tivesse a justificação adicional \{P, P ∨ R ⇒ Q\}, ela ainda seria removida; mas se também tivesse a justificação \{R, P ∨ R ⇒ Q\}, então ela seria poupada. Dessa forma, o tempo necessário para a retratação de P depende apenas do número de frases derivadas de P e não do número de frases adicionadas depois de P.

O JTMS pressupõe que sentenças consideradas uma vez provavelmente serão consideradas novamente; portanto, em vez de excluir uma sentença completamente da base de conhecimento quando ela perde todas as justificativas, simplesmente marcamos a sentença como estando fora da base de conhecimento. Se uma afirmação subsequente restaurar uma das justificativas, marcamos a sentença como estando de volta. Dessa forma, o JTMS retém todas as cadeias de inferência que utiliza e não precisa derivar sentenças novamente quando uma justificativa se torna válida novamente.

Além de lidar com a retratação de informações incorretas, os TMSs podem ser usados para acelerar a análise de múltiplas situações hipotéticas. Suponha, por exemplo, que o Comitê Olímpico Romeno esteja escolhendo locais para os eventos de natação, atletismo e hipismo nos Jogos Olímpicos de 2048, que serão realizados na Romênia. Por exemplo, suponha que a primeira hipótese seja Local\(Natação, Pitesti\), Local\(Atletismo, Bucareste\) e Local\(Hipismo, Arad\).

Uma grande quantidade de raciocínio deve ser feita para calcular as consequências logísticas e, portanto, a conveniência dessa seleção. Se, em vez disso, quisermos considerar Site\(Atletismo, Sibiu\), o TMS evita a necessidade de começar do zero novamente. Em vez disso, simplesmente retiramos Site\(Atletismo, Bucareste\) e afirmamos Site\(Atletismo, Sibiu\), e o TMS cuida das revisões necessárias. Cadeias de inferência geradas a partir da escolha de Bucareste podem ser reutilizadas com Sibiu, desde que as conclusões sejam as mesmas.

Um sistema de manutenção da verdade baseado em suposições, ou ATMS, torna esse tipo de troca de contexto entre mundos hipotéticos particularmente eficiente. Em um JTMS, a manutenção de justificativas permite que você se mova rapidamente de um estado para outro fazendo algumas retratações e afirmações, mas a qualquer momento apenas um estado é representado. Um ATMS representa todos os estados que já foram considerados ao mesmo tempo. Enquanto um JTMS simplesmente rotula cada frase como estando dentro ou fora, um ATMS rastreia, para cada frase, quais suposições fariam com que a frase fosse verdadeira. Em outras palavras, cada frase tem um rótulo que consiste em um conjunto de conjuntos de suposições. A frase é verdadeira apenas nos casos em que todas as suposições em um dos conjuntos de suposições são verdadeiras.

Sistemas de manutenção da verdade também fornecem um mecanismo para gerar explicações. Tecnicamente, uma explicação de uma sentença P é um conjunto de sentenças E tais que E implica P. Se as sentenças em E já são conhecidas como verdadeiras, então E simplesmente fornece uma base suficiente para provar que P deve ser o caso. Mas explicações também podem incluir suposições — sentenças que não são conhecidas como verdadeiras, mas que seriam suficientes para provar P se fossem verdadeiras. Por exemplo, se o seu carro não pega, você provavelmente não tem informações suficientes para provar definitivamente a razão do problema. Mas uma explicação razoável pode incluir a suposição de que a bateria está descarregada. Isso, combinado com o conhecimento de como os carros funcionam, explica o não comportamento observado. Na maioria dos casos, preferimos uma explicação E que seja mínima, o que significa que não há um subconjunto apropriado de E que também seja uma explicação. Um ATMS pode gerar explicações para o problema do "carro não pega" fazendo suposições \(como "sem gasolina no carro" ou "bateria descarregada"\) em qualquer ordem que desejarmos, mesmo que algumas suposições sejam contraditórias. Em seguida, olhamos para o rótulo da frase “o carro não pega” para ler os conjuntos de suposições que justificariam a frase.

Os algoritmos exatos usados para implementar sistemas de manutenção da verdade são um pouco complicados e não os abordaremos aqui. A complexidade computacional do problema de manutenção da verdade é pelo menos tão grande quanto a da inferência proposicional — ou seja, NP-difícil. Portanto, não se deve esperar que a manutenção da verdade seja uma panaceia. Quando usado com cuidado, no entanto, um TMS pode proporcionar um aumento substancial na capacidade de um sistema lógico de lidar com ambientes e hipóteses complexos.

## Resumo

Ao nos aprofundarmos nos detalhes de como se representa uma variedade de conhecimento, esperamos ter dado ao leitor uma noção de como as bases de conhecimento reais são construídas e uma noção das interessantes questões filosóficas que surgem. Os pontos principais são os seguintes:

•A representação de conhecimento em larga escala requer uma ontologia de propósito geral para organizar e vincular os vários domínios específicos do conhecimento.

•Uma ontologia de propósito geral precisa cobrir uma ampla variedade de conhecimento e deve ser capaz, em princípio, de lidar com qualquer domínio.

•Construir uma ontologia grande e de propósito geral é um desafio significativo que ainda não foi totalmente concretizado, embora as estruturas atuais pareçam ser bastante robustas.

• Apresentamos uma ontologia superior baseada em categorias e no cálculo de eventos. Abordamos categorias, subcategorias, partes, objetos estruturados, medições, substâncias, eventos, tempo e espaço, mudança e crenças.

•Os tipos naturais não podem ser definidos completamente na lógica, mas propriedades dos tipos naturais podem ser representadas.

•Ações, eventos e tempo podem ser representados com o cálculo de eventos. Tais representações permitem que um agente construa sequências de ações e faça inferências lógicas sobre o que será verdade quando essas ações ocorrerem.

•Sistemas de representação com propósitos específicos, como redes semânticas e lógicas descritivas, foram concebidos para auxiliar na organização de uma hierarquia de categorias. A herança é uma forma importante de inferência, permitindo que as propriedades dos objetos sejam deduzidas a partir de sua participação em categorias.

•A suposição de mundo fechado, conforme implementada em programas lógicos, fornece uma maneira simples de evitar a necessidade de especificar muitas informações negativas. É melhor interpretada como um padrão que pode ser substituído por informações adicionais.

•Lógicas não monotônicas, como a circunscrição e a lógica padrão, visam capturar o raciocínio padrão em geral.

•Os sistemas de manutenção da verdade lidam com atualizações e revisões de conhecimento de forma eficiente.

•É difícil construir grandes ontologias manualmente; extrair conhecimento do texto torna o trabalho mais fácil.

## Notas Bibliográficas e Históricas

Briggs \(1985\) afirma que a pesquisa sobre representação do conhecimento teve início com a teorização indiana sobre a gramática do sânscrito xástrico, no primeiro milênio a.C. Filósofos ocidentais remontam seus trabalhos sobre o assunto a cerca de 300 a.C., na Metafísica de Aristóteles \(literalmente, o que vem depois do livro sobre física\). O desenvolvimento da terminologia técnica em qualquer campo pode ser considerado uma forma de representação do conhecimento.

As primeiras discussões sobre representação em IA tendiam a se concentrar na "representação do problema" em vez da "representação do conhecimento". \(Veja, por exemplo, a discussão de Amarel \(1968\) sobre o problema dos "Missionários e Canibais".\) Na década de 1970, a IA enfatizou o desenvolvimento de "sistemas especialistas" \(também chamados de "sistemas baseados em conhecimento"\) que poderiam, se recebessem o conhecimento de domínio apropriado, igualar ou exceder o desempenho de especialistas humanos em tarefas estritamente definidas. Por exemplo, o primeiro sistema especialista, DENDRAL \(Feigenbaum et al., 1971; Lindsay et al., 1980\), interpretava a saída de um espectrômetro de massas \(um tipo de instrumento usado para analisar a estrutura de compostos químicos orgânicos\) com a mesma precisão de químicos especialistas. Embora o sucesso do DENDRAL tenha sido fundamental para convencer a comunidade de pesquisa em IA da importância da representação do conhecimento, os formalismos representacionais usados no D ENDRAL são altamente específicos para o domínio da química.

Com o tempo, pesquisadores passaram a se interessar por formalismos e ontologias padronizados de representação do conhecimento que pudessem auxiliar na criação de novos sistemas especialistas. Isso os levou a territórios anteriormente explorados por filósofos da ciência e da linguagem. A disciplina imposta à IA pela necessidade de que as teorias "funcionem" levou a um progresso mais rápido e profundo do que quando esses problemas eram domínio exclusivo da filosofia \(embora, por vezes, também tenha levado à reinvenção repetida da roda\).

Mas até que ponto podemos confiar no conhecimento especializado? Já em 1955, Paul Meehl \(ver também Grove e Meehl, 1996\) estudou os processos de tomada de decisão de especialistas treinados em tarefas subjetivas, como prever o sucesso de um aluno em um programa de treinamento ou a reincidência de um criminoso. Em 19 dos 20 estudos que analisou, Meehl descobriu que algoritmos simples de aprendizado estatístico \(como regressão linear ou Bayes ingênuo\) preveem melhor do que os especialistas. Tetlock \(2017\) também estuda o conhecimento especializado e o encontra em falta em casos difíceis. O Educational Testing Service utiliza um programa automatizado para corrigir milhões de questões dissertativas no exame GMAT desde 1999. O programa concorda com os avaliadores humanos em 97% das vezes, aproximadamente o mesmo nível de concordância entre dois avaliadores humanos \(Burstein et al., 2001\). \(Isso não significa que o programa entenda as redações, apenas que ele consegue distinguir as boas das ruins tão bem quanto os avaliadores humanos.\)

A criação de taxonomias ou classificações abrangentes remonta à antiguidade. Aristóteles \(384-322 a.C.\) enfatizou fortemente os esquemas de classificação e categorização. Seu Organon, uma coletânea de obras sobre lógica reunidas por seus alunos após sua morte, incluía um tratado chamado Categorias, no qual ele tentou construir o que hoje chamaríamos de ontologia superior. Ele também introduziu as noções de gênero e espécie para a classificação de nível inferior. Nosso sistema atual de classificação biológica, incluindo o uso da "nomenclatura binomial" \(classificação por meio de gênero e espécie no sentido técnico\), foi inventado pelo biólogo sueco Carolus Linnaeus, ou Carl von Linne \(1707-1778\). Os problemas associados a tipos naturais e limites de categorias inexatos foram abordados por Wittgenstein \(1953\), Quine \(1953\), Lakoff \(1987\) e Schwartz \(1977\), entre outros.

Veja o Capítulo 25 para uma discussão sobre representações de palavras e conceitos em redes neurais profundas que escapam de alguns dos problemas de uma ontologia estrita, mas também sacrificam parte da precisão. Ainda não sabemos a melhor maneira de combinar as vantagens das redes neurais e da semântica lógica para representação.

O interesse em ontologias de larga escala está aumentando, conforme documentado pelo Manual de Ontologias \(Staab, 2004\). O projeto OPENCYC \(Lenat e Guha, 1990; Matuszek et al., 2006\) lançou uma ontologia com 150.000 conceitos, com uma ontologia superior semelhante à da Figura 10.1, bem como conceitos específicos como "tela OLED" e "iPhone", que é um tipo de "telefone celular", que por sua vez é um tipo de "eletrônico de consumo", "telefone", "dispositivo de comunicação sem fio" e outros conceitos. O projeto NEXTKB expande o CYC e outros recursos, incluindo FrameNet e WordNet, para uma base de conhecimento com quase 3 milhões de fatos, e fornece um mecanismo de raciocínio, o FIRE, para acompanhá-lo \(Forbus et al., 2010\).

O projeto DBPEDIA extrai dados estruturados da Wikipédia, especificamente das Infoboxes: os pares atributo/valor que acompanham muitos artigos da Wikipédia \(Wu e Weld, 2008; Bizer et al., 2007\). Em 2015, o DBPEDIA continha 400 milhões de fatos, cerca de 4 milhões de objetos somente na versão em inglês; a contagem de todos os 110 idiomas resulta em 1,5 bilhão de fatos \(Lehmann et al., 2015\).

O grupo de trabalho P1600.1 do IEEE criou o SUMO, a Ontologia Superior Mesclada Sugerida \(Niles e Pease, 2001; Pease e Niles, 2002\), com cerca de 1.000 termos na ontologia superior e links para mais de 20.000 termos específicos de domínio. Stoffel et al. \(1997\) descrevem algoritmos para gerenciar eficientemente uma ontologia muito grande. Etzioni et al. \(2008\) apresenta um levantamento de técnicas para extrair conhecimento de páginas web.

Na Web, linguagens de representação estão emergindo. RDF \(Brickley e Guha, 2004\) permite que asserções sejam feitas na forma de triplas relacionais e fornece alguns meios para evoluir o significado de nomes ao longo do tempo. OWL \(Smith et al., 2004\) é uma lógica descritiva que suporta inferências sobre essas triplas. Até o momento, o uso parece ser inversamente proporcional à complexidade representacional: os formatos tradicionais HTML e CSS representam mais de 99% do conteúdo da Web, seguidos pelos esquemas de representação mais simples, como RDFa \(Adida e Birbeck, 2008\) e microformatos \(Khare, 2006; Patel-Schneider, 2014\), que usam marcação HTML e XHTML para adicionar atributos ao texto em páginas da web. O uso de ontologias sofisticadas de RDF e OWL ainda não é difundido, e a visão completa da Web Semântica \(Berners-Lee et al., 2001\) não foi concretizada. As conferências sobre Ontologia Formal em Sistemas de Informação \(FOIS\) abrangem ontologias gerais e específicas de domínio.

A taxonomia utilizada neste capítulo foi desenvolvida pelos autores e baseia-se, em parte, em sua experiência no projeto CYC e, em parte, no trabalho de Hwang e Schubert \(1993\) e Davis \(1990, 2005\). Uma discussão inspiradora sobre o projeto geral de representação do conhecimento de senso comum aparece no "Manifesto da Física Naif" de Hayes \(1978, 1985b\).

Ontologias profundas bem-sucedidas em um campo específico incluem o projeto Gene Ontology \(Gene Ontology Consortium, 2008\) e a Linguagem de Marcação Química \(Murray-Rust et al., 2003\). Dúvidas sobre a viabilidade de uma ontologia única para todo o conhecimento são expressas por Doctorow \(2001\), Gruber \(2004\), Halevy et al. \(2009\) e Smith \(2004\).

O cálculo de eventos foi introduzido por Kowalski e Sergot \(1986\) para lidar com o tempo contínuo, e houve diversas variações \(Sadri e Kowalski, 1995; Shanahan, 1997\) e visões gerais \(Shanahan, 1999; Mueller, 2006\). James Allen introduziu intervalos de tempo pela mesma razão \(Allen, 1984\), argumentando que intervalos eram muito mais naturais do que situações para o raciocínio sobre eventos prolongados e simultâneos. Em van Lambalgen e Hamm \(2005\), vemos como a lógica dos eventos se mapeia na linguagem que usamos para falar sobre eventos. Uma alternativa ao cálculo de eventos e situações é o cálculo fluente \(Thielscher, 1999\), que reifica os fatos a partir dos quais os estados são compostos.

Peter Ladkin \(1986a, 1986b\) introduziu intervalos de tempo "côncavos" \(intervalos com lacunas — essencialmente, uniões de intervalos de tempo "convexos" comuns\) e aplicou as técnicas da álgebra abstrata matemática à representação do tempo. Allen \(1991\) investiga sistematicamente a ampla variedade de técnicas disponíveis para representação do tempo; van Beek e Manchak \(1996\) analisam algoritmos para raciocínio temporal. Há semelhanças significativas entre a ontologia baseada em eventos apresentada neste capítulo e uma análise de eventos realizada pelo filósofo Donald Davidson \(1980\). As histórias da ontologia de líquidos de Pat Hayes \(1985a\) e as crônicas da teoria dos planos de McDermott \(1985\) também foram influências importantes na área e neste capítulo.

A questão do estatuto ontológico das substâncias tem uma longa história. Platão propôs que as substâncias eram entidades abstratas inteiramente distintas de objetos físicos; ele diria MadeOf\(Butter3, Butter\) em vez de Butter3 ∈ Butter. Isso leva a uma hierarquia de substâncias na qual, por exemplo, UnsaltedButter é uma substância mais específica do que Butter. A posição adotada neste capítulo, na qual as substâncias são categorias de objetos, foi defendida por Richard Montague \(1973\). Também foi adotada no projeto CYC. Copeland \(1993\) lança um ataque sério, mas não invencível.

A abordagem alternativa mencionada no capítulo, na qual a manteiga é um objeto composto por todos os objetos amanteigados do universo, foi proposta originalmente pelo lógico polonês Leśniewski \(1916\). Sua mereologia \(o nome deriva da palavra grega para "parte"\) utilizou a relação parte-todo como substituta da teoria matemática dos conjuntos, com o objetivo de eliminar entidades abstratas como conjuntos. Uma exposição mais compreensível dessas ideias é apresentada por Leonard e Goodman \(1940\), e "A Estrutura da Aparência" \(1977\), de Goodman, aplica as ideias a vários problemas de representação do conhecimento.

Embora alguns aspectos da abordagem mereológica sejam complexos — por exemplo, a necessidade de um mecanismo de herança separado baseado em relações parte-todo —, a abordagem obteve o apoio de Quine \(1960\). Harry Bunt \(1985\) apresentou uma análise abrangente de seu uso na representação do conhecimento. Casati e Varzi \(1999\) abordam partes, todos e uma teoria geral de localizações espaciais.

Existem três abordagens principais para o estudo de objetos mentais. A adotada neste capítulo, baseada na lógica modal e em mundos possíveis, é a abordagem clássica da filosofia \(Hintikka, 1962; Kripke, 1963; Hughes e Cresswell, 1996\). O livro "Raciocínio sobre o Conhecimento" \(Fagin et al., 1995\) fornece uma introdução completa, e Gordon e Hobbs \(2017\) apresentam "Uma Teoria Formal da Psicologia do Senso Comum".

A segunda abordagem é uma teoria de primeira ordem na qual objetos mentais são fluentes. Davis \(2005\) e Davis e Morgenstern \(2005\) descrevem essa abordagem. Ela se baseia no formalismo dos mundos possíveis e se baseia no trabalho de Robert Moore \(1980, 1985\).

A terceira abordagem é uma teoria sintática, na qual objetos mentais são representados por cadeias de caracteres. Uma cadeia de caracteres é apenas um termo complexo que denota uma lista de símbolos, portanto, CanFly\(Clark\) pode ser representado pela lista de símbolos \[C, a, n, F, l, y, \(, C, l, a, r, k,\)\]. A teoria sintática de objetos mentais foi estudada em profundidade pela primeira vez por Kaplan e Montague \(1960\), que demonstraram que ela levava a paradoxos se não fosse tratada com cuidado. Ernie Davis \(1990\) fornece uma excelente comparação entre as teorias sintática e modal do conhecimento. Pnueli \(1977\) descreve uma lógica temporal usada para raciocinar sobre programas, trabalho que lhe rendeu o Prêmio Turing e que foi expandido por Vardi \(1996\). Littman et al. \(2017\) mostram que uma lógica temporal pode ser uma boa linguagem para especificar objetivos para um robô de aprendizado por reforço de uma forma que seja fácil para um humano especificar e que generalize bem para diferentes ambientes.

O filósofo grego Porfírio \(c. 234–305 d.C.\), comentando as categorias de Aristóteles, desenhou o que poderia ser considerado a primeira rede semântica. Charles S. Peirce \(1909\) desenvolveu os grafos existenciais como o primeiro formalismo de rede semântica utilizando a lógica moderna. Ross Quillian \(1961\), motivado por seu interesse na memória humana e no processamento da linguagem, iniciou o trabalho sobre redes semânticas na IA. Um artigo influente de Marvin Minsky \(1975\) apresentou uma versão de redes semânticas chamada frames; um frame era uma representação de um objeto ou categoria, com atributos e relações com outros objetos ou categorias.

A questão da semântica surgiu de forma bastante aguda em relação às redes semânticas de Quillian \(e de outros que seguiram sua abordagem\), com seus onipresentes e muito vagos "links IS-A". O famoso artigo de Bill Woods \(1975\), "What's In a Link?", chamou a atenção de pesquisadores de IA para a necessidade de semântica precisa em formalismos de representação de conhecimento. Ron Brachman \(1979\) elaborou esse ponto e propôs soluções. "The Logic of Frames", de Patrick Hayes \(1979\), foi ainda mais a fundo, afirmando que "A maioria dos 'frames' é apenas uma nova sintaxe para partes da lógica de primeira ordem". "Tarskian Semantics, or, No Notation without Denotation\!", de Drew McDermott \(1978b\), argumentou que a abordagem da teoria dos modelos para a semântica usada na lógica de primeira ordem deveria ser aplicada a todos os formalismos de representação de conhecimento. Esta continua sendo uma ideia controversa; Notavelmente, o próprio McDermott inverteu sua posição em "Uma Crítica da Razão Pura" \(McDermott, 1987\). Selman e Levesque \(1993\) discutem a complexidade da herança com exceções, mostrando que, na maioria das formulações, ela é NP-completa.

As lógicas descritivas foram desenvolvidas como um subconjunto útil da lógica de primeira ordem, para a qual a inferência é computacionalmente tratável. Hector Levesque e Ron Brachman \(1987\) demonstraram que certos usos de disjunção e negação eram os principais responsáveis pela intratabilidade da inferência lógica. Isso levou a uma melhor compreensão da interação entre complexidade e expressividade em sistemas de raciocínio. Calvanese et al. \(1999\) resumem o estado da arte, e Baader et al. \(2007\) apresentam um manual abrangente de lógica descritiva.

Os três principais formalismos para lidar com inferência não monotônica — circunscrição \(McCarthy, 1980\), lógica default \(Reiter, 1980\) e lógica modal não monotônica \(McDermott e Doyle, 1980\) — foram todos introduzidos em uma edição especial do AI Journal. Delgrande e Schaub \(2003\) discutem os méritos das variantes, considerando 25 anos de retrospectiva. A programação de conjuntos de respostas pode ser vista como uma extensão da negação como falha ou como um refinamento da circunscrição; a teoria subjacente da semântica de modelos estáveis foi introduzida por Gelfond e Lifschitz \(1988\), e os principais sistemas de programação de conjuntos de respostas são DLV \(Eiter et al., 1998\) e S MODELS \(Niemela et al., 2000\). Brewka et al. \(1997\) oferecem uma boa visão geral das várias abordagens à lógica não monotônica. Clark \(1978\) aborda a abordagem da negação como falha para programação lógica e completação de Clark. Lifschitz \(2001\) discute a aplicação da programação de conjuntos de respostas ao planejamento. Uma variedade de sistemas de raciocínio não monotônicos baseados em programação lógica estão documentados nos anais das conferências sobre Programação Lógica e Raciocínio Não Monotônico \(LPNMR\).

O estudo de sistemas de manutenção da verdade começou com os sistemas TMS \(Doyle, 1979\) e RUP \(McAllester, 1980\), ambos essencialmente JTMSs. Forbus e de Kleer \(1993\) explicam em detalhes como os TMSs podem ser usados em aplicações de IA. Nayak e Williams \(1997\) mostram como um TMS incremental eficiente, denominado ITMS, torna viável o planejamento das operações de uma nave espacial da NASA em tempo real.

Este capítulo não pôde abordar todas as áreas da representação do conhecimento em profundidade. Os três principais tópicos omitidos são os seguintes:

Física qualitativa: A física qualitativa é um subcampo da representação do conhecimento que se ocupa especificamente da construção de uma teoria lógica e não numérica de objetos e processos físicos. O termo foi cunhado por Johan de Kleer \(1975\), embora se possa dizer que o empreendimento tenha começado com o BUILD de Fahlman \(1974\), um planejador sofisticado para a construção de torres complexas de blocos. Fahlman descobriu, durante o processo de projeto, que a maior parte do esforço \(80%, segundo sua estimativa\) era dedicada à modelagem da física do mundo dos blocos para calcular a estabilidade de vários subconjuntos de blocos, em vez do planejamento em si. Ele esboça um processo hipotético semelhante à física ingênua para explicar por que crianças pequenas conseguem resolver problemas semelhantes ao BUILD sem acesso à aritmética de ponto flutuante de alta velocidade usada na modelagem física do BUILD. Hayes \(1985a\) utiliza "histórias" — fatias quadridimensionais de espaço-tempo semelhantes aos eventos de Davidson — para construir uma física ingênua de líquidos bastante complexa. Davis \(2008\) fornece uma atualização à ontologia de líquidos que descreve o vazamento de líquidos em recipientes.

De Kleer e Brown \(1985\), Ken Forbus \(1985\) e Benjamin Kuipers \(1985\) desenvolveram, de forma independente e quase simultânea, sistemas capazes de raciocinar sobre um sistema físico com base em abstrações qualitativas das equações subjacentes. A física qualitativa logo se desenvolveu a ponto de tornar possível analisar uma impressionante variedade de sistemas físicos complexos \(Yip, 1991\). Técnicas qualitativas têm sido utilizadas para construir novos designs para relógios, limpadores de para-brisa e andadores de seis patas \(Subramanian e Wang, 1994\). A coletânea "Leituras em Raciocínio Qualitativo sobre Sistemas Físicos" \(Weld e de Kleer, 1990\), um artigo de enciclopédia de Kuipers \(2001\) e um artigo de manual de Davis \(2007\) fornecem boas introduções à área.

Raciocínio espacial: O raciocínio necessário para navegar no mundo wumpus é trivial em comparação com a rica estrutura espacial do mundo real. A primeira tentativa séria de capturar o raciocínio de senso comum sobre o espaço aparece no trabalho de Ernest Davis \(1986, 1990\). O cálculo de conexão de regiões de Cohn et al. \(1997\) apoia uma forma de raciocínio espacial qualitativo e levou a novos tipos de sistemas de informação geográfica; veja também \(Davis, 2006\). Assim como na física qualitativa, um agente pode percorrer um longo caminho, por assim dizer, sem recorrer a uma representação métrica completa.

Raciocínio psicológico: O raciocínio psicológico envolve o desenvolvimento de uma psicologia funcional para agentes artificiais usarem no raciocínio sobre si mesmos e sobre outros agentes. Isso frequentemente se baseia na chamada psicologia popular, a teoria que se acredita que os humanos em geral usam para raciocinar sobre si mesmos e sobre outros humanos. Quando pesquisadores de IA fornecem aos seus agentes artificiais teorias psicológicas para raciocinar sobre outros agentes, as teorias frequentemente se baseiam na descrição feita pelos pesquisadores do próprio design dos agentes lógicos. O raciocínio psicológico é atualmente mais útil no contexto da compreensão da linguagem natural, onde adivinhar as intenções do falante é de suma importância.

Minker \(2001\) reúne artigos de pesquisadores líderes em representação do conhecimento, resumindo 40 anos de trabalho na área. Os anais das conferências internacionais sobre Princípios de Representação do Conhecimento e Raciocínio fornecem as fontes mais atualizadas para trabalhos nessa área. Readings in Knowledge Representation \(Brachman e Levesque, 1985\) e Formal Theories of the Commonsense World \(Hobbs e Moore, 1985\) são excelentes antologias sobre representação do conhecimento; a primeira se concentra mais em artigos historicamente importantes em linguagens de representação e formalismos, a segunda na acumulação do conhecimento em si. Davis \(1990\), Stefik \(1995\) e Sowa \(1999\) fornecem introduções de livros didáticos à representação do conhecimento, van Harmelen et al. \(2007\) contribui com um manual, e Davis e Morgenstern \(2004\) editam uma edição especial do AI Journal sobre o tópico. Davis \(2017\) apresenta um panorama da lógica para o raciocínio de senso comum. A conferência bienal sobre Aspectos Teóricos do Raciocínio sobre o Conhecimento \(TARK\) abrange aplicações da teoria do conhecimento em IA, economia e sistemas distribuídos.

1Transformar uma proposição em um objeto é chamado de reificação, do latim res, ou coisa. John McCarthy propôs o termo "coisificação", mas ele nunca pegou.

2Quando perguntado sobre o que se poderia deduzir sobre o Criador a partir do estudo da natureza, o biólogo J. B. S. Haldane disse: “Uma predileção desmedida por besouros”.

3Os termos “evento” e “ação” podem ser usados indistintamente — ambos significam “algo que pode acontecer”.

4Nossa versão é baseada em Shanahan \(1999\), mas com algumas alterações.

5 Vários sistemas antigos falharam em distinguir entre propriedades de membros de uma categoria e propriedades da categoria como um todo. Isso pode levar diretamente a inconsistências, como apontado por Drew McDermott \(1976\) em seu artigo “Artificial Intelligence Meets Natural Stupidity” \(Inteligência Artificial Encontra a Estupidez Natural\). Outro problema comum era o uso de ligações IsA para relações de subconjunto e de pertinência, em correspondência com o uso em inglês: “a cat is a mammal” \(um gato é um mamífero\) e “Fifi is a cat” \(Fifi é um gato\). Veja o Exercício 10.NATS para mais informações sobre essas questões.

6Observe que a linguagem não permite simplesmente afirmar que um conceito, ou categoria, é um subconjunto de outro. Esta é uma política deliberada: a subsunção entre categorias deve ser derivável de alguns aspectos das descrições das categorias. Caso contrário, algo está faltando nas descrições.

O 7CLASSIC fornece testes de subsunção eficientes na prática, mas o pior tempo de execução é exponencial.

8Lembre-se de que a monotonicidade exige que todas as sentenças implicadas permaneçam implicadas após novas sentenças serem adicionadas à KB. Ou seja, se KB ⊨ α, então KB ∧ β ⊨ α.

9Para a hipótese de mundo fechado, um modelo é preferível a outro se tiver menos átomos verdadeiros — ou seja, os modelos preferidos são modelos mínimos. Há uma conexão natural entre a hipótese de mundo fechado e KBs de cláusulas definidas, porque o ponto fixo alcançado pelo encadeamento direto em KBs de cláusulas definidas é o único modelo mínimo. Veja a página 249 para mais informações sobre este ponto.

10A revisão de crenças é frequentemente contrastada com a atualização de crenças, que ocorre quando uma base de conhecimento é revisada para refletir uma mudança no mundo, em vez de novas informações sobre um mundo fixo. A atualização de crenças combina a revisão de crenças com o raciocínio sobre tempo e mudança; também está relacionada ao processo de filtragem descrito no Capítulo 14.
