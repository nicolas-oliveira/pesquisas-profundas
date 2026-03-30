# Memórias dos Anos 80: O Nexus 1600

## Parte 1

Chegamos a uma parte interessante das minhas memórias: minha participação no projeto de um dos primeiros computadores brasileiros compatíveis com o PC IBM, o Nexus 1600. Este assunto vai ocupar vários posts, neste primeiro vou tentar dar um pouco do contexto deste projeto dentro da Scopus.

|                                  |
| -------------------------------- |
| Olha eu aí na Folha Informática! |

Como mencionei em algum post anterior, a Scopus foi fundada por três colegas de faculdade. A Scopus cresceu rápido seguindo um modelo vertical (como a maioria das empresas da época): quase tudo era feito internamente.

Este crescimento rápido teve um forte efeito sobre a estrutura da empresa e a ocupação de cargos de chefia. Quando inicie o estágio, vários dos primeiros engenheiros (que eu considero a "geração heroica") já tinham avançados para cargos de chefia. A Engenharia era organizada nos departamentos de software e hardware (que inclusive ficavam em andares diferentes no prédio).

Durante os meus primeiros meses na Scopus os gerentes destes departamentos foram promovidos para um novo grupo, o "Comitê de Tecnologia", com o objetivo de dar mais coesão interdisciplinar aos projetos. A ideia era que este comitê se envolveria na especificação dos projetos e no seu anteprojeto técnico. Algum tempo depois foi criado um grupo de Gerenciamento de Projeto, com o objetivo de formalizar a estrutura matricial dos projetos. Os projetos que mencionei nos posts anteriores (Lepus 200, TVA 2052 e o TVG 4001), seguiram à risca esta estrutura.

A diretoria da Engenharia era ocupada por Edson Fregni, que acumulava ainda o cargo de Presidente e tinha uma atuação forte na defesa da Reserva de Mercado. Reconhecendo o problema deste acumulo de funções, em 1982 um novo Diretor de Engenharia foi contratado. Por sua vez ele trouxe um engenheiro que viria a ser o líder do projeto do Nexus (o fato de ser alguém de fora criou inicialmente um certo atrito).

A minha primeira ligação com o projeto do Nexus foi quando um dos sócios apareceu na Engenharia com um PC-IBM (só a CPU e teclado). Ele foi ligado a um TVA-1000 alterado para funcionar como monitor e logo um monte de gente (eu inclusive) se aglomerou em torno. Minha lembrança deste evento foram as piscadas do vídeo quanto a tela rolava, o que gerou reclamações quanto ao monitor improvisado, até que eu examinei o Technical Reference e verifiquei que o BIOS apagava a tela enquanto fazia a rolagem (isto se deve ao fato da CGA original não suportar o acesso simultâneo da memória de vídeo pelo processador e o hardware de geração dos sinais de vídeo).

Um outro efeito do crescimento rápido foi a necessidade de financiamento, o que levou a dívidas. Em 1983 a Scopus sofria com a concorrência no mercado de terminais de vídeo e o seu micro de 8 bits não decolava. O projeto do Nexus passou a ser visto como a taboa de salvação. No início do projeto a Scopus realizou o seu primeiro corte de funcionários. Atendendo à sugestão de um funcionário, parte das lâmpadas de iluminação foram retiradas para economizar na cona de luz. Não sei quanto isto representou em termos de economia, mas certamente foi um baque na moral.

No próximo post vou falar um pouco no projeto do hardware.

### Memórias dos Anos 80: O Nexus 1600, parte 2 (Hardware)

Continuando de onde paramos no post anterior, o projeto do Nexus 1600 fugiu um pouco da estrutura tradicional da Scopus, com uma interferência menor do "Comitê de Tecnologia" e uma autonomia maior do líder do projeto. Neste post vou falar um pouco sobre os acertos e erros (na minha visão) no projeto do hardware.

A linha básica do projeto era ser algo compatível com o PC IBM, mas melhor. No início do projeto tivemos algumas reuniões com o marketing, e a principal recomendação deles foi acrescentar o suporte aos disquetes de 8", pois nenhuma empresa aceitaria os disquetes "de brinquedo" de 5 1/4".

Os projetistas eram, em retrospecto, bem inexperientes. Os principais projetistas da parte digital foram um colega de turma (meu companheiro no projeto de formatura) e um de uma turma anterior. A placa de vídeo foi projetada por um engenheiro formado na turma seguinte à minha (portanto recém formado). As ferramentas disponíveis eram precárias (por exemplo, os osciloscópios disponíveis eram de 25MHz).

Na placa de sistema, a principal decisão foi suportar a operação a 8MHz, além dos 4.77MHz do PC original. Potencialmente isto seria uma grande vantagem sobre os concorrentes, mas a implementação disso foi através de jumpers na placa. Ou seja, para mudar a velocidade era preciso desligar o micro e retirar a tampa. Some a isso que muitos softwares eram sensíveis à velocidade e dá para começar a entender a fama de "incompatível" que os concorrentes iriam colar ao Nexus.

Uma decisão certeira, antecipando ao PC XT, foi o uso de chips de 64 Kbits de Ram, permitindo a expansão para 256K na placa mãe.

Uma decisão esdrúxula (em retrospecto) foi usar uma USART 8251 para a interface com o teclado. Embora isso fosse mais eficiente que o "bit banging" feito pelo PC IBM, isso foi mais uma fonte de incompatibilidade para softwares que interagiam diretamente com o teclado, como alguns sistemas operacionais. A situação não era totalmente grave, pois a solução adotada era compatível com aplicativos que assumiam a interrupção de teclado e liam as teclas diretamente do hardware. Mas operações como comandar o teclado eram feitas de forma diferente.

O teclado, aliás, era muito bom. Feito com as mesmas teclas indutivas que o TVA-1000 e o micro Scopus original, tinha pequenos aperfeiçoamentos no layout.

A interface de disquete tinha duas diferenças notáveis em relação à do PC-IBM. A primeira era seguir na parte analógica o circuito e layout da interface de disquete do micro Scopus (devido aos traumas com ela). A segunda era a presença de um conector imenso na parte traseira, para permitir conectar um gabinete com os disquetes de 8" (acho que este gabinete nunca chegou a ser produzido, todo mundo ficou contente com os disquetes "de brinquedo").

Uma questão de honra na placa de vídeo era ser capaz de conciliar os acessos à memória feitos pelo processador com os feitos pelo hardware de vídeo. Isto se mostrou bem complicado e o circuito ocupou duas placas ao invés de uma. O resultado foi muito bom, desde que o software tirasse proveito disso (como a BIOS do Nexus fazia).

Uma outra antecipação ao XT foi incluir uma interface de disco rígido. Entretanto, o HD era montado em um gabinete externo.

Um protótipo foi montado usando a técnica de wire-wrap (como no meu projeto de formatura). Algo bem impressionante, dada a quantidade de chips e ligações. Uma certa quantidade de softwares (incluindo alguns jogos) foram comprados para teste de compatibilidade. Este protótipo ficava numa sala no último andar do prédio e várias vezes eu ia lá no final do dia confirmar a compatibilidade dos jogos...

Com o protótipo funcionando, o passo seguinte era a placa de circuito impresso. Trabalho feito à mão por um "layoutista".

Quando as placas chegaram, é claro que não funcionaram. Nada de estranho até aí, já era costuma um trabalho paciente de conferir a placa com o protótipo atrás de pequenos erros. O problema é que a placa insistia em não funcionar.

O lançamento do Nexus (o projeto que ia salvar a Scopus!) ia ser feito em uma feira. A data ia se aproximando, a placa não funcionava e o pânico foi se instalando. Foram feitas algumas tentativas muito loucas de montar uma mistura da placa com o protótipo para tentar identificar onde esta problema, e nada. A Scopus tinha um emulador (ICE) do 8088, quando fomos tentar usar ele acabamos fazendo uma ligação ao contrário e pifamos o ICE...

Finalmente um engenheiro mais experiente (que não participava do projeto) fez o diagnóstico e achou uma solução. O problema era de ruído nos sinais. A velocidade "estonteante" de 4.77MHz era demais para a placa dupla face com layout feito a mão. A solução para a feira foi extremamente artesanal. Um pino foi soldado em cada pino de terra dos CIs e uma placa de cobre com a furação da placa mãe foi soldada por baixo, fazendo um "plano terra". Um monte de capacitores adicionais forma colocados em pontos estratégicos. Resistores foram soldados nos conectores de expansão para reduzir a reflexão de sinais. Acho que pelo menos uma dúzia de micros foram montados desta forma.

A solução para a produção original não era muito melhor. A principal diferença é que o plano terra era um PCI fina colocada por cima da placa normal (eliminando a necessidade de soldar os pinos). Os resistores para a via de expansão foram montados em um placa espetada no último slot. Micros destes chegaram a ser entregues para clientes.

A solução mais definitiva foi contratar um serviço de CAD (algo extremamente caro na época) para reprojetar a placa. Esta nova placa funcionava perfeitamente, mas esqueceram de avisar os requisitos mecânicos. O resultado é que, se colocada no lugar da placa original, os slots ficavam defasados em relação à carcaça. No meu trabalho seguinte tinha um micro da produção original. Quando ele pifou o técnico precisou pegar uma furadeira e fazer novos furos de fixação para colocar a placa nova.

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

### Memórias dos Anos 80 - Nexus 1600, parte 5 (consequências)

Apesar de todos os percalços, o Nexus 1600 foi apresentado na "Feira Internacional de Informática" (mais conhecida como SUCESU) de outubro de 1983 e "salvou" financeiramente a Scopus (pelo menos por alguns anos). Todos viveram felizes para sempre? Não exatamente...

|                                        |
| -------------------------------------- |
| O Nexus 2600, o sucessor do Nexus 1600 |

Uma primeira consequência (que eu considero positiva, mas isso é discutível) foi uma mudança organizacional na Engenharia, com a divisão por linhas de produtos (microcomputadores, terminais e dispositivos para ligação a mainframes IBM, outros terminais).

A Scopus teve dificuldades em promover as vantagens do Nexus 1600 e os concorrentes conseguiram colar nele o rótulo de "incompatível". Em pouco tempo a liderança em vendas deste mercado era da Microtec, uma empresa nova cujos equipamentos não tinham grande inovações mas eram bastante confiáveis e compatíveis (na feira onde o Nexus 1600 foi lançado a Scopus tinha um estande grande com uma dezena de micros enquanto que a Microtec tinha um estande onde só cabia uma mesa onde estava um único micro - e desconfio que o teclado era importado).

Pouco menos de um ano após o lançamento do Nexus 1600 a IBM lançava o PC-AT, dando um pulo no desempenho (vou falar sobre o 80286 e o PC-AT no próximo post). A Scopus entrou uma fase de indefinições sobre o futuro. Se comentava que a Intel teria oferecido um projeto completo de uma placa mãe compatível com o PC-AT e muitos eram favoráveis a seguir esta linha ao invés de investir em um projeto próprio.

A maioria das pessoas não levava o SISNE a sério e as ideias de licenciá-lo para outros fabricantes nunca se concretizou. Alguns fabricantes anunciaram os seus próprios sistemas operacionais. Eu tive oportunidade de examinar somente um destes e o desassembly era muito igual ao do MS-DOS 2.0 (curiosamente, as diferenças eram pequenas otimizações, como retirar os NOPs gerados pelo assembler quando determinava no segundo passo que um desvio podia ser feito com a variante mais curta do JMP).

A grande equipe montada para o desenvolvimento do SISNE foi se desmontando e acabei me vendo praticamente sozinho tentando matar os bugs mais feios.

Apesar de todos os problemas com o hardware do Nexus 1600, somente em 1985 foi lançado um sucessor, o Nexus 2600 (apesar do 2, era um PC XT não um PC AT). O projeto foi tocado de forma bem discreta pelo meu colega do projeto de formatura, agora veterano das desventuras com o 1600. A primeira providência dele ao ser alocado para o projeto foi colocar na prateleira em cima da mesa um livro de ondas e linhas... A placa de sistema não teve os problemas de ruído e tinha um grande aperfeiçoamento: a seleção de clock entre 4.77 e 8MHz podia ser comandada por software, sem interromper o processamento. O BIOS permitia fazer essa mudança através de uma combinação de teclas. Somado a uma maturidade maior no desenvolvimento de aplicações, isto reduzia em muito as problemas de incompatibilidade. Uma melhoria invisível (mas polêmica, devido aos traumas com o MicroScopus) foi o reprojeto da interface de disquete. Além de eliminar o conector para os drives de 8", a parte analógica usava componentes mais novos que simplificavam enormemente a calibração na fábrica. Do ponto de vista externo, o que se observava era a adoção de unidades "slim" (ou meia-altura), o que permitira acomodar até quatro drives (entre disquetes, HD e fita) onde no Nexus 1600 só podiam ser colocados dois drives de disquete.

Não foi uma época muito agradável para mim no trabalho, o que acabou me levando a procurar outra opção. Na vida particular as coisas foram muito melhores: em 1984 eu me casei e, alguns meses depois, "ficamos grávidos".
