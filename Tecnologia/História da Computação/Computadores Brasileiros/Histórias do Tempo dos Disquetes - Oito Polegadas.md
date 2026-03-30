# Histórias do Tempo dos Disquetes - Oito Polegadas

No mês passado a Sony anunciou que vai parar de produzir disquetes, o que parece ser o fim de uma era. Nesta série de posts vou lembrar a história dos disquetes e algumas histórias que aconteceram comigo.

## O Início

O disquete foi inventado pela IBM, como uma forma simples e barata de carga de microcódigo no mainframe 370. Começou a ser comercializado em 1971, na forma de um disco de 8 polegadas dentro de um envelope ligeiramente flexível. A capacidade inicial era 80 Kbytes e os drives no mainframe eram apenas de leitura.

Em 1973 a IBM introduziu o formato que seria o padrão para os disquetes de 8 polegadas, com capacidade de 250 Kbytes. Este formato seria retroativamente chamado de face simples densidade simples (SSSD), com o disco organizado em 77 trilhas, cada uma com 22 setores de 128 bytes. A versão face simples densidade dupla (setores de 256 bytes) foi lançada em 1976 e no ano seguinte a versão dupla face dupla densidade (DSDD), chegando a 1 Mbyte de capacidade.

Os disquetes de 8 polegadas foram as unidades de "armazenamento em massa" dos primeiros microcomputadores, com um drive custando mais de US$1000,00.

## O Cobra 400

Num dos primeiros posts eu me referi ao "[meu primeiro computador pessoal](http://dqsoft.blogspot.com/2006/01/meu-primeiro-computador-pessoal.html "null")", o Cobra 400 (que não era meu nem pessoal, era um equipamento do IPT ao qual eu tinha acesso como estagiário). Esta estranha criatura tinha uma unidade de disquete 8 polegadas SSSD, para backup e transferência de dados (o Cobra 400 era primariamente uma máquina para digitação de dados). No IPT o disquete era pouco usado, dava-se preferência à unidade de fita magnética (o computador principal, um B1700, tinha unidade de fita mas não tinha drive de disquete).

## Dupla Densidade na Scopus

O meu segundo estágio (e primeiro emprego) foi na Scopus, que era uma fabricante de terminais de vídeo e micro-computadores. Na época que comecei a estagiar ela tinha acabado de lançar o MicroScopus, um micro de 8 bits com sistema operacional CP/M-80 - e drives de 8 polegadas.

Inicialmente os drives eram SSSD, mas rapidamente foram lançados drives DSDD. Entretanto, o circuito para dupla densidade era muito mais complicado que para densidade simples. Simplificando bastante, na densidade simples cada bit era gravado como dois pulsos, um indicando o valor do bit e outro de temporização (clock). Desta forma um circuito relativamente simples (algo como um mono-estável disparado pelo pulso de clock) permitia localizar os pulsos de dados. A densidade dupla é obtida, digamos assim, tirando os pulsos de clock. O circuito precisava conter um PLL para se manter sincronizado com os dados. Nestes tempos "do onça" isto exigia umas duas dúzias de componentes críticos.

O resultado prático é que as primeiras controladoras DSDD da Scopus não funcionavam de forma confiável. Eu fui uma vítima delas, perdendo a única cópia que eu tinha dos fontes do meu projeto de formatura (naquele tempo era difícil achar disquete em lojas e eu fiquei com vergonha de pegar mais de um disquete "emprestado"; felizmente eu tinha uma listagem bem atualizada).

A solução foi copiar uma placa exemplo do fabricante do drive - não somente o circuito eletrônico em si, mas o próprio layout da placa. Isto criou uma paranoia que afetou o projeto do Nexus 1600 (o PC compatível da Scopus) - a controladora de disquete ficou com praticamente o mesmo layout.

## O Duplicador de Disquetes

Nos meus tempos de estagiário de disquete eu tinha uma grande inveja de um outro estagiário, que recebeu um trabalho muito mais importante que o meu - fazer um programa duplicador de disquetes. Era comum sentarmos lado a lado no laboratório e eu acompanhava os testes por um efeito colateral - o ruído. Os drives de oito polegadas eram particularmente ruidosos e nestes testes estavam sendo usado unidades com dois drives. Cada vez que uma unidade era selecionada ouvia-se um claque bem alto. Como o programa lia uma trilha de uma unidade e em seguida a gravava na outra, os primeiros testes pareciam uma metralhadora: claque-claque-claque...

Entretanto, à medida em que os testes do duplicador de disquetes avançavam, "pequenos detalhes" iam surgindo: era preciso formatar a trilha se não estivesse formatada, verificar após a escrita, repetir em caso de erro e por aí vai. O resultado é que à medida que o programa ia sendo acertado a velocidade de cópia ia caindo. A metralhadora do começo acabou virando um trote cauteloso: claque-------claque------claque...

## Disquete de 8 Polegadas no Nexus 1600

Quando a IBM lançou o PC (em 81) o disquete de 8 polegadas já tinha perdido terreno para a versão de 5 1/4 (que veremos nos próximos posts). O projeto do PC compatível da Scopus, o Nexus 1600, seguiu um caminho todo "especial", coincidindo com a entrada de algumas novas pessoas na Engenharia. Quando o projeto foi apresentado para o Marketing, a maioria das decisões já estavam tomadas. O marketing fez poucas exigências, uma dela foi o suporte a disquetes de 8 polegadas, pois "os clientes não confiavam nos disquinhos de 5 1/4".

Como os drives não cabiam no gabinete, a placa controladora de disquete recebeu um conector imenso na parte traseira, no qual era conectado um cabo que ia até um gabinete externo (com fonte própria) onde ficariam as unidades. Foi projetado e testado todo o hardware e software necessário, porém nenhuma unidade de 8 polegadas chegou a ser vendida.

Enquanto isto, foi projetado "a toque de caixa" uma nova versão do MicroScopus com suporte aos tais disquinhos em que ninguém confiava.
