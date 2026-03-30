# Memórias dos Anos 80: Estágio na Scopus Tecnologia

## Parte I

E saímos dos anos 70! Estamos em 1981, o último ano da faculdade. Hora de encerrar o estágio no IPT e ir procurar um estágio mais focado com o que eu pretendia seguir como emprego.

A Scopus Tecnologia era uma "figurinha carimbada". Fundada por três colegas de turma (Edson Fregni, Josef Manasterski e Celso Ikeda) tinha se tornado um símbolo (e um alvo) dos defensores e inimigos da reserva de mercado. Inicialmente desenvolvia e produzia terminais de vídeo (vou falar um pouco sobre o que é isso aí adiante) mas recentemente lançara um microcomputador CP/M (vou falar mais sobre isso). Um dos meus colegas (e amigo) já estagiava lá.

O presidente da Scopus era o Edson Fregni. Além do trabalho na Scopus e da função política de defensor da reserva de mercado ele ainda achava tempo para dar aulas na Poli. Um dia reuni coragem e fui pedir para ele um estágio. Quando disse que meu interesse era em software, ele me disse para conversar com o diretor da área, que também era professor da Poli. Descobri que ele estava dando aula naquele momento e fiquei esperando a aula acabar para ir conversar. Após uma longa conversa sobre software eu tinha obtido não só um estágio, mas também um amigo (e futuro patrão, mas isso é mais para frente).

O que é um terminal de vídeo? De forma bem simplificada (vocês podem ver mais na [wikipedia](https://en.wikipedia.org/wiki/Computer_terminal "null")), é um dispositivo de entrada e saída para computadores onde a parte de saída é uma tela. Tipicamente a parte de entrada é um teclado e a conexão ao computador é serial (assíncrona ou assíncrona). Nos mais simples os dados trafegam "nus", os mainframes normalmente tinham protocolos onde os dados são agrupados em pacotes e precisam ser confirmados pelo outro lado (através do chamado ACK).

O primeiro terminal da Scopus era puro hardware, mas logo passaram a usar o microcontrolador 8080, o que convergiu para a linha TVA 1000 (Terminal de Vídeo Alfanumérico) com as seguintes características:

- Microprocessador 8080 rodando a um pouco mais de 3 MHz (a frequência foi escolhida para a geração do vídeo)

- Memória EPROM para o firmware (inicialmente a 2708 de 1Kbyte, depois a 2716 de 2Kbytes)

- Memória Ram dinâmica (inicialmente usando chips de 4Kbits depois chips de 16Kbits)

- Vídeo mapeado na memória, usando DMA (8257) e o controlador de vídeo 8275

- Interface serial feita com a USART 8051

- Teclado com teclas indutivas (um luxo!)

Toda a parte lógica estava em uma única placa imensa, que ficava na diagonal por cima do tubo da tela.

|                                        |
| -------------------------------------- |
| A foto é ruim, mas é a única que achei |

Na época em que entrei a Scopus estava finalizando uma primeira "mudança de geração". Os projetistas originais (que eu chamaria de "geração heroica") foram sendo promovidos para cargos de gerência (ou saíram da empresa). A "nova geração" (principalmente na área de software) era um pouco mais preocupada com alguns aspectos mais formais de qualidade.

Uma historinha ilustra um pouco esta transição deste período heroico. Originalmente o desenvolvimento do software era feito em algo chamado "Gepeto" (não cheguei a ver isso), que usa uma *teletype* como terminal (uma *teletype* é um terminal eletromecânico, lento e barulhento). Os fontes eram salvos em fita perfurada. Só tinham um destes equipamentos e o processo todo de edição e compilação eram lentos e incômodos. Daí que no aperto de correção de bugs muitos engenheiros alteravam alteravam direto o código executável no gravador de EProm. Era a chamada "costura": o código novo era escrito por cima do velho. Se o código novo era menor que o velho, bastava completar com NOPs. Se era maior, o jeito era colocar um JMP para uma área não usada, colocar o novo código lá e depois fazer um JMP para depois do código antigo. Neste processo era comum sobrarem alguns restos de código antigo depois do JMP. Pois bem, algum tempo antes de entrar um dos projetistas recebeu uma caixa de disquetes e um saco de EPROMs com a missão de editar os fontes nos disquetes para que gerassem exatamente o conteúdo das EPROMs (que eram cópias das matrizes usadas pela fábrica). O resultado é que os fontes ficavam com JMPs inesperado, definições de dados aparentemente aleatório no meio do código (resto das instruções sobrepostas) e trechos de códigos em endereços esquisitos.

Quando entrei o grosso do desenvolvimento para os terminais era feito em dois equipamentos de desenvolvimento da Intel ([Intellec](https://en.wikipedia.org/wiki/Intellec "null")). Um era totalmente original, o outro só tinha a CPU da Intel com monitor e unidade de disquete adaptados pela Scopus. Aliás, esta era outra característica comum desta época: desenvolvimento próprio de ferramentas, com nível variado de qualidade e acabamento.

O meu estágio foi principalmente desenvolvendo um aplicativo para o MicroScopus (como vou falar em um post futuro), mas acabei colocando um pouco a mão em firmware de terminais ainda como estagiário. É claro que estamos falando na correção de bugs.

O terminal envolvido era o TVA1210, que emulava um terminal da Univac. Era um projeto da era heroica (com as devidas costuras registradas no código fonte). A Univac usava um protocolo complicado, tinha até o ACK do ACK. O desenvolvimento acabou extrapolando a capacidade do hardware em termos de desempenho e (em parte) de memória Ram.

No lado da memória, a previsão era usar 4K, mas isso se mostrou insuficiente. Eu mencionei lá atrás que o chip utilizado era 4Kbits, isto significa 4K posições de 1 bit. Para fazer 4Kbytes são necessários 8 chips. Quando estourou os 4K previstos, o projetista teve a ideia de montar só 1 chip da região seguinte. O resultado é que nesta área só podiam ser colocadas variáveis lógicas (*flags*).

O problema de processamento era mais sério. O Univac dava um tempo curto para o terminal processar os dados enviados e responder com um ACK. Não dava para o terminal da Scopus receber todos os dados, processar e depois responder. O jeito era ir processando os dados à medida em que eram recebidos. Só que existia o risco dos dados serem corrompidos na comunicação. Para isso existia um CRC no final de cada pacote. Ao final da recepção o terminal precisava conferir este CRC e, em caso de erro, ignorar o pacote e enviar um NAK pedindo a retransmissão. Só que o terminal Scopus estava processando os dados à medida em que chegavam... Daí surgiu que existiam dois conjuntos de flags: os "falsos" e os "verdadeiros" (nomenclatura horrível, mas era isso que estava nos fontes). O firmware ia processando os dados e atualizando a tela e os flags "falsos". No final, se o CRC estava OK os flags "falsos" eram copiados por cima dos flags "verdadeiros". Se o CRC dava errado, a tela era apagada e os flags "verdadeiros" eram copiados por cima dos "falsos", revertendo o processamento.

Eu resolvi alguns bugs simples, mas não teve como solucionar o pior deles: o terminal tinha a opção de comandar uma impressora. Quando vinha um pacote errado o terminal imprimia besteiras e apagar a tela e descartar os flags não fazia sumir a cópia impressa!

### Memórias dos Anos 80: Estágio na Scopus Tecnologia - parte II - O "Laboratório"

Ao começar a escrever sobre o meu trabalho na Scopus, percebi a necessidade de explicar algo difícil de entender atualmente: o "Laboratório".

Da mesma forma como computadores pessoais ainda eram raros no começo dos anos 80, também não era comum os desenvolvedores terem um computador de uso exclusivo na sua mesa. Os microcomputadores ficavam em uma área "comunitária" - o Laboratório.

Os projetistas passavam um certo tempo em suas mesas sem micro. Era possível escrever código em papel e passar para o pessoal de apoio digitar, mas nesta altura já era comum o próprio desenvolvedor digitar o código. Para estudar o código, a gente tinha listagens impressas.

No post anterior eu mencionei os dois Intel Intellect. Além deles, na época em que comecei a estagiar, o laboratório tinha uns tantos micro Scopus (micros CP-M/80 que vão ser o assunto do próximo post). A maioria dos equipamentos tinham algum tipo de gambiarra, dificilmente um equipamento novinho vinha da fábrica para a engenharia.

Um outro equipamento do laboratório era o gravador de EProm, uma gambiarra montada a partir do hardware de um terminal. Anos mais tarde, seria substituído por outro projeto interno mais confiável (tinha até uma placa de circuito impresso própria, ao invés de usar uma montagem *wire-wrap*).

Rede local ainda era um sonho (durante todos os anos 80 era comum artigos dizendo que aquele seria o "ano das redes"). Mas todos os micros tinham interface serial e elas eram ligadas ao ConCon - Concentrador de Comunicação. Sim, uma outra gambiarra montada a partir do hardware de um terminal. Ele funcionava como uma espécie de central telefônica: você ia lá e digitava os números dos dois equipamentos a serem conectados. Uma funcionalidade especial era poder conectar ao próprio ConCon, que funcionava como *spooler* de impressão: os dados recebidos eram salvos temporariamente em disquete e enviados para a impressora. Impressora "de linha" (mais rápida e barulhenta que as impressoras matriciais), que ficava fechada em uma salinha.

O ConCon era prático, mas (anos depois) colaborou para um pequeno desastre. Os Intellect dispunham do ICE (*In-Circuito Emulator*) um cordão umbilical que podia ser espetado no lugar do 8080 em um circuito e permitia depurar o firmware (não era assim tão prático como soa, vários desenvolvedores preferiam deduzir logicamente os problemas e só usavam o ICE como último recurso). Estava então o desenvolvedor usando o ICE, com um modelo novo de terminal. É claro que o terminal estava todo aberto para dar acesso ao soquete do processador. Por algum motivo, a placa auxiliar para converter os sinais seriais para o padrão RS232 estava solta. E aí a placa bateu no lado interno do soquete de alimentação (não isolado), jogando 110V na trilha de terra da placa. Os 110V foram pela cordão umbilical até o Intellect. E pela serial do Intellect até o ConCon. E do ConCon para todos os outros equipamentos... Demorou semanas para ressuscitar todos os equipamentos.

### Memórias dos Anos 80: Estágio na Scopus Tecnologia - parte III, o MicroScopus

Baseada no microprocessador 8080, a linha de terminais TVA1000 estava a um passo de ser um microcomputador. Este passo foi dado em 1980 com o lançamento do MicroScopus, um micro de 8 bits com sistema CP/M-80.

|                                       |
| ------------------------------------- |
| O uC20 à esquerda e o uC200 à direita |

Olhando para trás, agora é claro que este projeto ilustra o conflito entre o prático e o filosófico e as limitações da equipe de desenvolvimento da Scopus.

**O Hardware**

O que faltava para um TVA1000 virar um micro? Era preciso aumentar a memória Ram para 64K (usando 32 chips de 16Kx1), acrescentar um recurso de chaveamento de memória para permitir colocar e tirar a EPROM do mapa da memória, e ligar uma interface de disquete.

A Scopus já tinha uma interface de disquete pronta, graças a um projeto feito para a Buroughs/Unisys: o DE-1500. O DE-1500 era um terminal de entrada de dados (*data entry*), composto de um terminal TVA1000 acrescido de uma ou duas unidades de disquete 8 polegadas. Infelizmente tive pouco contato com ele e não sei detalhes sobre o software, mas sei que incluía algum rudimento de sistema operacional e uma linguagem para definição das telas de entrada de dados.

O MicroScopus (uC10) original padecia de algumas limitações de desempenho. A primeira era a própria CPU, um 8080 rodando alguma coisa acima de 3MHz. Os concorrentes usavam um Z80 rodando a pelo menos 4MHz. Por motivos que nunca me foram claros, a Scopus utilizava somente processadores Intel. O resultado foi uma nova versão usando o 8085 a 5MHz, o uC20 com as unidades de disquete no "carrinho de chá".

Tinha somente um destes modelos com 8085 no laboratório e um fato me chamou a atenção: uma série de fios de wire-wrap percorrendo a placa de ponta a ponta. Comentando com um dos meus amigos do departamento de hardware, ele disse que era a via de dados do processador. As trilhas na placa sofriam interferência demais do resto do circuito, impedindo o funcionamento confiável. Todos os MicroScopus com 8085 estavam sendo produzidos com estes fios soldados.

O outro problema era a capacidade dos disquetes. Trabalhando com face simples e densidade simples, ela era limitada a 250Kbytes. Como [falei anos atrás](https://dqsoft.blogspot.com/2010/05/historias-do-tempo-dos-disquetes-oito.html "null"), a Scopus teve dificuldade em fazer uma interface densidade dupla confiável. A solução foi copiar o layout da placa do projeto de referência do fabricante (detalhes no link).

(Estes problemas de layout e ruído se manifestariam de forma mais forte na hora do projeto do Nexus 1600, o micro de 16 bits, mas vou falar sobre isso bem mais para frente).

Um último comentário sobre o hardware diz respeito ao teclado e ao vídeo. Esta parte foi influenciada pelo contato com um terminal de vídeo da HP, particularmente o uso de teclas de função. As últimas linhas da tela eram normalmente usadas para indicar as funções destas tecla e caracteres semi-gráficos e vídeo reverso eram usados para montar formulários nas telas.

Anos mais tarde seria lançado o uC200, inspirado na nova linha de terminais TVA2000 e com a (tardia) opção de disquetes de 5 1/4". Pessoalmente nunca gostei muito do visual dele, a foto no começo não dá uma ideia real do tamanho dele.

**O Software**

O software era um calcanhar de Aquiles na política de reserva de mercado. A regra era a pirataria, variando apenas o nível da cara de pau. Aqui a Scopus fez algo impressionante: licenciou o CP/M-80 e várias linguagens da Microsoft, com direito aos fontes. Até onde sei, foi a única fazer isso. (Quem estiver curioso sobre estes fontes, uma boa parte deles está hoje em dia na internet).

O núcleo do CP-M/80 era escrito em Assembler e os utilitários em PL/M-80 (uma linguagem criada pela Intel). Apesar de ter os fontes, existiam alguns empecilhos técnicos em compilá-los e, cedendo à pressão para o lançamento rápido, a Scopus simplesmente editou os executáveis para traduzir as mensagens.

A Scopus trabalhou durante muito tempo no projeto de um sistema operacional de rede para o MicroScopus (o Multiplus), mas acho que isso só foi disponibilizado para um cliente.

Um colega talentoso, por pura farra, passou os fontes do CP-M/80 por um tradutor de Assembly de 8080 para 8086 (que foi a estratégia usada por muitos softwares conhecidos, como o WordStar e DBase) e conseguiu rodar parcialmente em um equipamento da Scopus com processador 8086.

Os fontes das linguagens foram bem mais usados. Uma coisa que me surpreendeu quando eu vi um primeiro micro com CP-M/80 foi o COBOL-80, um compilador de COBOL distribuído pela Microsoft (digo distribuído porque os fontes revelavam que tinha sido feito por outra empresa). A Scopus tinha vários clientes que usavam isso e logo descobriram que ele tinha vários bugs... Pelo que me contaram o compilador não estava escritos diretamente em Assembly. Os recursos de macros e compilação condicional eram usados e abusados para criar uma linguagem própria (isso era bem comum na época, a própria Scopus tinha uma linguagem para implementar protocolos de comunicação). Os fontes do BASIC seriam usados para fazer um interpretador BASIC para o Nexus, o micro de 16 bits.

Durante parte do meu estágio eu estudei o fonte do utilitário PIP, que era o equivalente ao COPY do MSDOS (mas com uma sintaxe terrível). Do alto da minha arrogância juvenil estava certo que conseguiria fazer algo muito mais eficiente (escrevendo em Assembly, é claro). Mas esse projeto não foi em frente.

O principal projeto do meu estágio foi um utilitário (cujo nome não lembro) para o MicroScopus, mas isso será assunto do próximo post.

### Memórias dos Anos 80: Nexus 1600 , parte 4 (SISNE)

Uma das partes mais polêmicas do projeto do Nexus 1600 foi o desenvolvimento de um sistema operacional compatível com o MS-DOS, o SISNE (SIStema do NExus). Muitas pessoas não acreditavam (e não acreditam) que o SISNE foi desenvolvido pela Scopus. O eu dizia na época era: "foi desenvolvido sim, e temos os bugs para provar...".

|                                   |
| --------------------------------- |
| Texto no livreto "Scopus 30 anos" |

Um ponto fraco da reserva de mercado era o software, com a pirataria correndo solta. Mesmo entre os apoiadores da política de reserva existia uma ênfase no hardware, a preocupação com o número de máquinas produzidas e a visão do software como sendo uma atividade secundária que podia ficar para uma segunda etapa. Foi uma surpresa o governo ser convencido a estabelecer uma regra obrigando os micros de 16 bits a serem comercializados com sistema operacional desenvolvido no Brasil (*spoiler*: esta regra não deu o resultado esperado).

Não que desenvolver um sistema operacional seja uma tarefa impossível. É parte do nosso complexo de vira-latas achar que certas coisas só pode se feitas por empresas estrangeiras.

O MS-DOS 1.x, em particular, não era um sistema complexo. Conforme relatado em vários lugares ([aqui](https://en.wikipedia.org/wiki/MS-DOS#History "null") por exemplo), o desenvolvimento inicial foi feito por uma pessoa (Tim Paterson) em seis semanas. A interface de programação era baseada no CP/M-80, com cerca de duas dúzias de funções. A maior parte do "núcleo" era a manipulação do sistema de arquivos que se baseava na FAT, criada pela Microsoft para o Standalone Disk BASIC. Suportando apenas disquetes com baixa capacidade, a FAT ocupava apenas 512 bytes e podia ser mantida continuamente na memória.

Além do núcleo, o MS DOS 1.x incluía o interpretador de comando (COMMAND, que implementava internamente os comandos DIR, DEL, COPY, TYPE, RENAME, PAUSE e REM) e os utilitários CHKDSK, DISKCOMP, DISKCOPY, FORMAT, MODE e SYS.

O grande trunfo da Scopus para desenvolver o SISNE era usar uma linguagem de alto nível ao invés de Assembly. A oferta de linguagens neste tempo não era muito grande e acabou sendo escolhido um compilador de PASCAL comercializado pela IBM para a primeira versão do SISNE. Este compilador se mostrou bem adequado, pelo menos em termos de facilidade em programar e estabilidade. É claro que o resultado ficou bem maior que o MS-DOS, mas o Nexus vinha com incríveis 256K bytes na placa de sistema...

Para fazer o SISNE no prazo necessário foi feita uma grande força tarefa. Não lembro mais os detalhes, mas acho que foram duas ou três pessoas fazendo o núcleo (eu uma delas), um pessoa fazendo o COMMAND (sem o COPY, que foi transformado em utilitário externo) e mais duas ou três pessoas para fazer os utilitários (incluindo o COPY).

O SISNE funcionava e (até onde me lembro) não tinha muitos problemas de compatibilidade. O problema mesmo eram os bugs. Nos meses seguintes eu tive bastante trabalho tentando resolvê-los e tive a desilusão de descobrir que um dos desenvolvedores era bem fraco (e isto era bastante conhecido, só eu na minha inocência não sabia).

Em um post futuro eu vou falar sobre a segunda versão do SISNE. Mas eu acho que aqui é um bom ponto para avançar alguns anos, para depois que eu já tinha saído da Scopus e o SISNE estava na versão 3, quando a Scopus foi acusada de ter copiado o MS DOS, com direito a matéria na Veja. Eu procurei a Scopus para saber os detalhes e, na verdade, a situação era boba. O trecho de código que sai estampado na Veja (e em outros lugares) era parte do utilitário SUBST. Este utilitário, pouco usado, permitia converter acessos a uma unidade de disco para outra (para poder rodar aplicativos que tivessem a unidade fixa no código). A lógica deste utilitário é simples: ele intercepta as chamadas ao DOS e troca a letra da unidade na entrada e destroca na volta. A única complicação é que os locais onde pode estar a unidade variam conforme a função. O desenvolvedor da versão Scopus, preocupado com compatibilidade, disassemblou o utilitário original e anotou os passos feitos (basicamente testar um a um os códigos de função e fazer os ajustes conforme ele). Na hora de programar, ele seguiu diretamente estas anotações e acabou fazendo os mesmos testes na mesma ordem... O curioso é que eu acho que dificilmente faria isso pois a minha arrogância juvenil me faria inventar um forma "mais eficiente" de fazer a mesma coisa. Provavelmente usaria uma tabela, que é o que foi feito pela Scopus para gerar uma versão diferente. Mas neste ponto a reserva de mercado já estava no fim e pouco tempo depois a Microsoft foi autorizada a comercializar o MS DOS 3.3 porque as versões nacionais só seriam compatíveis dom a versão 3.2 (ou outra besteira parecida).

### Memórias dos Anos 80: Estágio na Scopus Tecnologia - parte IV, o Utilitário para o MicroScopus

É com vergonha que admito não lembrar o nome do programa que consumiu a maior parte do meu tempo como estagiário. Mas, felizmente, lembro de algumas lições.

|                                                       |
| ----------------------------------------------------- |
| Fonte: Revista Micro Sistemas número 6, março de 1982 |

Como mencionei na parte anterior, o MicroScopus teve a tela e teclado influenciados por um terminal HP e a linguagem preferida pelos desenvolvedores de "aplicativos comerciais" era o COBOL. Daí o interesse em fornecer ferramentas para que estes desenvolvedores pudessem tirar proveito dos recursos da tela e teclado.

A primeira ferramenta foi o utilitário MASCARA. O vídeo do MicroScopus não era gráfico nem colorido, mas tinha alguns caracteres apropriados para o desenho de linhas e caixas (como o PC-IBM também teria) e um recurso de vídeo reverso, sublinhado e piscante (os chamados "atributos de vídeo"). O MASCARA era um editor que permitia desenhar de forma simples uma tela combinando texto, caracteres semigráficos e atributos. O resultado era salvo como um arquivo objeto. O desenvolvedor criava a(s) tela(s) e depois "linkava" com o programa COBOL e com uma biblioteca que tinha a rotina para limpar a tela e apresentar a "máscara". O programa COBOL então escrevia os dados variáveis e colocava os campos de entrada de dados nas posições corretas. (Para ser mais preciso, as linguagens compiladas da Microsoft usavam todas o mesmo formato de objeto e biblioteca, portanto as telas podiam ser usadas com qualquer uma delas).

A minha missão como estagiário era fazer algo "parecido" para facilitar o uso das teclas de função. Podemos dizer que era um editor de macros, onde era editado o conjunto de teclas a serem geradas quando cada tecla de função fosse pressionada.

A parte mais complicada para mim foi a apresentação e navegação na tela de forma sincronizada com a representação interna. Algo que complicou ainda mais quando eu fui apresentar a minha primeira versão e uma das primeiras coisas que fizeram foi pressionar Control + alguma coisa. O meu programa não fazia tratamento especial nenhum e o que apareceu na tela foi o que tinha no gerador de caracteres para o código obtido. O desejado era que o utilitário apresentasse na tela ^X (onde X é a "alguma coisa"). Beleza, agora tinha horas em que o operador andava uma posição para o lado e na tela o cursor tinha que andar duas... e não vou nem tentar relembrar a flecha para baixo! Aqui veio uma primeira lição: detalhar os requisitos antes de iniciar a codificação.

A base para a minha codificação foi o MASCARA. Ambos, é claro, escritos em Assembly. O autor do MASCARA era uma pessoa bastante meticulosa e organizada (além de um grande fotógrafo, mas isto não vem ao caso). Cada função do programa, como "mover cursor uma posição para direita" estava codificado em um fonte separado. Talvez fosse um pouco exagerado, mas o código do MASCARA era bem legível.

Em retrospecto, esse era um típico projeto para ser desenvolvido em linguagem de alto nível. Na época (1981) a linguagem C ainda era pouco conhecida fora do mundo Unix, mas existiam outras linguagens que poderiam ter sido usadas. O uso de linguagens de alto nível na Scopus aumentaria nos anos seguintes. Com um pouco de exagero eu considero isso uma vitoria da "minha geração".

Para quem quiser ler a entrevista da qual eu extraí a foto, a revista (em PDF) pode ser baixada do [Datassette](https://datassette.nyc3.cdn.digitaloceanspaces.com/revistas/micro_sistemas_006.pdf "null").
