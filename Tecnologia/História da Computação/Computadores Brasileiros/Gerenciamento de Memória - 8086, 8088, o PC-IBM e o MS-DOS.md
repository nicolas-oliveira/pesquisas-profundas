# Gerenciamento de Memória - 8086, 8088, o PC-IBM e o MS-DOS

Com o sucesso dos microprocessadores de 8 bits e o constante avanço da micro-eletrônica, os fabricantes partiram para projetos mais ousados.

A Motorola, por exemplo, abandonou a arquitetura de 8 bits do [6800](http://en.wikipedia.org/wiki/6800 "null") e desenvolveu o [68000](http://en.wikipedia.org/wiki/68000 "null"), com 16 registradores de 32 bits de uso geral e modos usuário e supervisor para proteção do sistema operacional. A Zilog partiu para o sofisticado [Z8000](http://en.wikipedia.org/wiki/Z8000 "null").

A Intel, entretanto, se mostrou bastante conservadora com o 8086. Os registradores de uso geral do 8086 (AX, BX, CS e DX) são uma expansão direta dos registradores do 8080. Inclusive, cada registrador ?X pode ser acessado como dois registrados de 8 bit, ?H e ?L.

Isto, mais alguns cuidados na definição do conjunto de instruções, permitiu a conversão automática de código fonte assembler do 8080 para o 8086. Vários programas de sucesso no CPM/80, como [WordStar](http://en.wikipedia.org/wiki/WordStar "null") e [Dbase II](http://en.wikipedia.org/wiki/DBASE "null") utilizaram este processo para sair na frente no lançamento do PC IBM (por outro lado, estes programas convertidos ficaram em desvantagem quando concorrentes feitos especificamente para o PC ficaram disponíveis).

Como [vimos anteriormente](http://dqsoft.blogspot.com/2006/09/gerenciamento-de-memria-8080-e-cpm80.html "null"), embora o 8080 seja um processador de 8 bits, ele possui algumas instruções para manipular 16 bits, tipicamente endereços. O 8086, entretanto, manipula quase que exclusivamente 16 bits. Isto criou um problema, pois, mesmo no final dos anos 70, era evidente que 16 bits de endereçamento (64Kbytes) era pouco. A gamb^H^H^H^B adaptação técnica (TM [Rodrigo Strauss](http://www.1bit.com.br "null")) adotada pela Intel custou muitas noites de sono para os desenvolvedores de aplicações e compiladores. As instruções normais trabalham com endereços de 16 bits, estes endereços são convertidos para 20 bits somando com o valor de registradores especiais também de 16 bits, porém deslocados de 4 bits para a esquerda. Parece confuso? É confuso mesmo.

Estes registradores especiais são chamados registradores de segmentos. O 8086 tem 4 destes registradores, chamados CS, DS, SS e ES. Por default, estes registradores são usados, respectivamente para acesso a código, dados e pilha (o registrador ES só é usado implicitamente por algumas instruções especiais). É possível (com certas restrições) usar explicitamente um registrador de segmento diferente colocando um byte de prefixo na frente da instrução.

Vamos tentar esclarecer com um exemplo. Vamos supor que os registradores BX, DS e ES contenham, respectivamente, 0x1234, 0x9000 e 0x4321 (0x indica número hexadecimal, se você não sabia disso provavelmente está no blog errado). A instrução assembler MOV AX,[BX] carrega em AX o valor na posição de memória de endereço DS*16 + BX (ou seja, 0x91234). A instrução MOV AX,ES:[BX] usa o registrador ES no lugar do DS, carregando em AX o valor na posição de memória de endereço 0x44444.

É costume no 8086 falar em endereços na forma segmento:offset, onde segmento e offset são valores de 16 bits. No nosso exemplo anterior, o endereço 0x44444 poderia ser referenciado como 0x4321:0x1234. Ou 0x4000:0x4444. Ou outras 64K-2 formas diferentes (o 8086 ignora o "vai um" quando soma o segmento deslocado com o offset, guarde esta informação para quando formos falar do 286).

O que tudo isto significa na prática? Em primeiro lugar, que o 8086 é capaz de endereçar até 1M de memória. Em segundo lugar, que uma aplicação precisa fazer uma certa ginástica para usar mais que 64K de código ou dados. No caso do código, existem o JUMP e CALL intersegmento, mas são instruções mais longas e mais demoradas. No caso de dados, para acessar uma estrutura que ocupe mais de 64K é preciso fazer umas contas medonhas. Os compiladores C para o 8086 costumam suportar alguns modelos de endereçamento:

- **Tiny:** Tudo fica em um único segmento de 64K. Os quatro registradores de segmento apontam para o início do segmento no início da execução e não são mais alterados. Altamente eficiente e simples, até que o seu programa não caiba em 64K.

- **Small:** Um segmento de código (apontado por CS) e um segmento de dados (apontado por DS e SS). Novamente os registradores não precisam ser alterados durante a execução.

- **Medium:** Um segmento de código para cada rotina e um segmento único de dados. Cada chamada de rotina passa de 3 para 5 bytes; o seu código que não cabia por pouco em 64K agora ocupa 70K.

- **Large:** Múltiplos segmentos de código e dados, mas nenhuma estrutura passa de 64K. Os registradores CS e DS mudam a toda instante, o tamanho do código cresce e o desempenho cai.

- **Huge:** Múltiplos segmentos de código e dados, permite estruturas com mais de 64K. Avançar um ponteiro de dados passa a ser uma operação lenta.

Neste altura vocês devem estar achando que a Intel fez uma grande besteira. Entretanto, simplificando o processador ela simplificou o processo de fabricação, saindo na frente dos concorrentes e obtendo mais chips bons por waffer de silício e portanto chips mais baratos. Além disso, a compatibilidade parcial com o 8080 permitiu ter rapidamente uma boa oferta de software. Enquanto os outros processadores de 16 bits eram usados apenas em projetos mais sofisticados, o 8086 foi ganhando terreno. Quando a IBM foi ouvir os conselhos da Microsoft (que aliás tinha muito software para 8080 e CPM/80, a ponto de ter desenvolvido uma placa de expansão com Z80 para ter penetração no mercado Apple) a estratégia deu frutos.

Um ponto interessante no interior do 8086 é a divisão do processador em uma unidade de execução e uma unidade de acesso de memória. Esta segunda unidade é a responsável por acionar os pinos de endereçamento do chip e faz algumas coisas especiais:

- no caso de um acesso a uma valor de 16 bits que está em um endereço impar, gera dois acessos à memória. Desta forma existe uma penalidade de tempo, mas o software é executado quase que normalmente.

- aproveita quando a unidade de execução está processando a instrução para pegar as instruções seguintes e colocá-las numa pequena fila. Não chega a ser um cache, mas dá um pequeno ganho de performance e permite usar memórias mais lentas.

A existência da unidade de acesso à memória simplificou criar o 8088, que é identico ao 8086 porém usa uma memória de 8 bits. Do ponto de vista de software, nada muda. Do ponto de vista de hardware, o custo de um sistema simples cai, pois os chips de memória típicos eram de 16K ou 64K posições de 1 bit. Com o 8088, dava para fazer um sistema usando 8 chips de memória, com o 8086 era preciso no mínimo 16 chips.

Ao contrário do 8080, o 8086 inicia a execução no final da memória, no endereço 0xFFFF0. Novamente, isto simplifica o projeto, pois pode-se colocar uma memória não volátil no fim da memória e Ram no começo. No início da memória existe uma tabela importante o vetor de interrupções. Esta tabela contém os endereços das rotinas que tratam as interrupções de hardware e software.

## O PC-IBM

Por sugestão da Microsoft, a IBM utilizou o 8088 no PC IBM. Na placa principal havia lugar para até 64K de Ram, na forma de 32 chips de 16Kx1. No PC XT eram usados chips de 64Kx1, permitindo 256K na placa mãe.

O 1M de endereçamento foi dividido em três grandes partes: os primeiros 640K (0x00000 a 0x9FFFF) para a Ram, os últimos 64K (0xF0000 a 0xFFFFF) para a memória não volátil com o BASIC e o BIOS e o resto (0xA0000 a 0xEFFFF) para uso pelas placas de expansão (notadamente a Ram das placas de vídeo em 0xB0000 e 0xB8000 e o BIOS da controladora de HD em 0xC0000).

Uma curiosidade é que o chip de DMA da Intel (responsável entre outras coisas por transferir para a memória os dados das controladoras de disco) também era limitado a 16 bits. Para endereçar toda a memória, a IBM precisou acrescentar um registrador externo de 4 bits. Isto criou uma nova barreira de 64K: não era possível ler diretamente do disco para um trecho de memória que contivesse uma mudança nos 4 bits mais significativos do endereço. O BIOS se limitava a recusar este tipo de transferências, deixando por conta do sistema operacional ou aplicação contornar esta limitação.

O BIOS possui as rotinas de iniciação do hardware e rotinas básicas de entrada e saída. A memória de 0x00000 a 0x003FF contem os vetores de interrupção, de 0x00400 a 0x004FF estão as variáveis do BIOS.

## O MS-DOS

A Seattle Computer foi uma das primeiras empresas a fazer um micro usando o 8086. Entretanto o único software disponível era o BASIC da Microsoft. Cansados de esperar pelo CP/M-86, eles desenvolveram o [QDOS](http://en.wikipedia.org/wiki/QDOS "null") (Quick and Dirty Operationg System). Quando a IBM também se cansou de esperar pelo CP/M-86, a Microsoft licenciou o QDOS e o ofereceu para a IBM. O resto, como dizem, é história. (Recomendo para quem quiser os detalhes o livro Gates de Stephen Manes e Paul Andrews).

Apesar das inúmeras diferenças em relação ao CP/M-80, o MS-DOS suporta uma interface de programação muito semelhante. Como vimos na parte anterior, no CP/M-80 a interface entre a aplicação e o sistema operacional é feita através de uma estrutura no início da memória. No MS-DOS esta estrutura é simulada no Prefixo de Segmento de Programa (PSP). Lá estão a linha de comando, as estruturas de acesso a arquivos (FCBs) e até mesmo a chamada ao SO. Nas versões futuras do MS-DOS algumas destas características foram perdendo importâncio, mas continuam lá e sobrevivem até no Windows.

Ao contrário do CP/M-80, o MS-DOS reside nos endereços baixos. A memória disponível para os programas começa após o final do SO (mais rigorosamente, após a parte residente do COMMAND.COM) e vai até o fim da Ram (o final da Ram é também usado pelo parte transiente do COMMAND.COM). Na versão 1 o DOS não oferecia nenhuma função de gerenciamento de memória, exceto o Terminate and Stay Resident, através da qual um programa podia encerrar deixando uma parte de si na memória (ou seja, controle de memória do DOS se limitava ao endereço ondem os programas eram carregados). A partir da versão 2, o DOS passou a oferecer funções simples de alocação de memória, colocando um pequeno prefixo no final de cada bloco alocado.

O MS-DOS suporta dois formatos de arquivos executáveis: COM e EXE.

Um arquivo COM é uma imagem binária de um programa. Esta imagem é carregada no offset 0x100 de um segmento, os registradores CS, DS, ES e SS apontam para o início do segmento e a execução é iniciada em CS:0x100. No começo do segmento é colocado o PSP. Este esquema, muito semelhante ao CP/M-80, é apropriado para programas no modelo tiny.

O arquivo EXE suporta programas com múltiplos segmentos. Para isto ele possui um cabeçalho para para acertar as referências inter-segmentos conforme o segmento em que o programa foi carregado. O PSP é criado na memória antes do primeiro segmento do programa. No início da execução, os registradores DS e ES apontam para o PSP, os registradores CS, IP, SS e SP são carregados com informações obtidas do cabeçalho do EXE.

Do ponto de vista de gerenciamento de memória, o 8086 e o MS-DOS não trazem grandes vantagens em relação ao 8080 e CP/M-80 (exceto por suportarem uma memória maior). Isto ficou para a geração seguinte. Infelizmente, o 286 seguiu na direção errada, como veremos no próximo post desta série.
