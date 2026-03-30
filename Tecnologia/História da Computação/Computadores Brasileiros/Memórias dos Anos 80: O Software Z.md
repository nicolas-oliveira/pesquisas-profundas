# Memórias dos Anos 80: O Software Z

O software Z era o principal produto da Humana quando eu começa a trabalhar lá em 1986. Neste post vou falar um pouco sobre ele e sobre a experiência de trabalhar no seu desenvolvimento.

**Porque Z**

O nome "Z" veio da seta costumeiramente usada nas figuras de comunicação de dados. Era um nome curto e impactante (embora alguns falam-se no "software Zé"). Para iniciar a execução bastava digitar no prompt do DOS z e enter. Não por acaso para sair era semelhante: o comando "finaliza" era disparado por Alt Z e um enter confirmava a saída.

**O que era o Z**

O Z era um software de comunicação. A grosso modo, sua função era transmitir e receber dados pela interface serial de um micro compatível com o IBM PC. A coisa fica mais interessante (e complicada) quando se pensa em de onde vem e para onde vão os dados.

Uma aplicação típica era a emulação de terminal. Sozinho (e com a configuração padrão) o Z interpretava os dados recebidos como comandos do terminal VT-52 da Digital e os apresentava na tela. Os comandos do VT-52 são bastante básicos e incluem mudar de linha (LF), voltar o cursor para a primeira coluna da linha (CR), voltar o cursor uma coluna (BS) e limpar a tela (FF), entre outros. Estes comandos (LF, CR, etc) são códigos de um caracter, comandos mais complexos eram iniciados por um caracter especial, o ESC. Nessa época eu conhecia de cor a tabela de códigos ASCII.

Em cima deste básico tinha um monte de configurações. Por exemplo, era possível separar uma parte dos dados recebidos através de sequências de início e fim, criando a "recepção secundária" que podia ser dirigida para a impressora ou um arquivo em disco.

Um recurso mais avançado eram os "filtros de comunicação", algo que hoje seria chamado de *plugin*. Os filtros eram programas externos que interagiam com o Z para poder processar os dados a receber e a transmitir. Isso permitia implementar emulações de terminal, transferências de arquivo e outras coisas mais sofisticadas.

**Como o Z era por dentro**

O Z possuia um núcleo de multitarefa cooperativo. Não lembro mais os detalhes, mas acho que eram umas duas dúzias de tarefas se revezando no uso do processador. O fato de ser cooperativo indica que as tarefas tinham que soltar explicitamente o processador. Tipicamente isso era feito através da espera por um recurso ou condição, o que era feito por uma operação **p** em um semáforo. Outra tarefa, ou uma interrupção, liberava a tarefa para execução fazendo uma operação **v** no semáforo. As aulas de teoria de sistemas operacionais da faculdades estavam sendo colocadas em prática. O escalador de tarefas implementava também um conceito de prioridade, as tarefas com prioridade mais alta eram escaladas com maior frequência que as com prioridade mais baixa.

O código, escrito em C com algumas pitadas de assembly, estava organizado em algumas dezenas de fontes (a quantidade de fontes ira aumentar à medida que mais recursos eram colocados). De um modo geral a qualidade do código era mito boa (sou suspeito de falar isso) e razoavelmente comentada.

**Trabalhando no desenvolvimento do Z**

Na época em que eu entrei o Z era compilado com a versão 2 do compilador de C da Lattice (que era também distribuído pela Microsoft).

Micros eram caros nos anos 80. A Humana tinha um Nexus 1600 (parte do acordo de distribuição do Z pela Scopus), com 256K de Ram, dois disquetes e monitor monocromático. Quando entrei tinha acabado de chegar um micro da Monydata (também obtido por escambo). O micro só tinha uma unidade de disquete mas tinha espantosos 704K de Ram* (chupa quem disse que 640K era suficiente). Tinha também um monitor colorido. Este era o micro que eu usava normalmente, porém às vezes tinha que abrir mão para uma demonstração colorida para cliente).

Os compiladores eram compiladores, não IDEs. Ou seja, não incluíam editor (nem depurador). A gente usava o Turbo Pascal como editor. Anos mais tarde tentamos alguns editores mais sofisticados, mas o que acabou sendo usado foi o QuickC que vinha junto com o Microsoft C a partir da versão 5.

Tipicamente uma grande parte da Ram era usada para criar um ramdisk e o compilador era copiado para lá. Não lembro mais mas acho que os fontes eram processados um a um, copiando do disquete para o ramdisk, compilando e depois copiando o objeto de volta para um disquete. Com certeza os fontes todos não cabiam em um único disquete de 360K.

Quando entrei eu ganhei o disquete número 238 do Z (os disquetes eram numerados). A versão atual era a 1.07p. O p era de "preliminar" e já tinham sido geradas várias versões p diferentes e algumas estavam até em cliente. A primeira versão que eu gerei foi a 1.07q e estabeleci a regra que a versão mudava sempre que saísse do desenvolvimento. As versões 1.08 e 1.09 tinham pequenas melhorias.

Não lembro exatamente quando alterei o código para usar a versão 3 do Microsoft C, desta fez desenvolvida do zero pela Microsoft ao invés de licenciada. A principal complicação era uma mudança na convenção de chamada das rotinas, onde os registradores SI e DI não podiam mais ser alterados pelas rotinas (ou não usava ou tinham que ser salvos no início e restaurados no fim). Isso acarretou uma pequena mexida no núcleo multitarefa.

A primeira grande mexida foi na versão 1.10, mas isso fica para o próximo post.

- Para chegar aos 704K eram usados os primeiros 64K reservados para as placas de vídeo. Um jumper permitia desabilitar esta parte de memória no caso de usar uma placa de vídeo que precisasse dela (o que não ocorria com a CGA).

### Memórias dos Anos 80: Zapt

O Z foi um software muito bom. Mas a sua evolução, na minha comprometida visão, foi um software excelente: o Zapt.

|                                                    |
| -------------------------------------------------- |
| A imagem (criada pela agência) não foi unanimidade |

Não lembro exatamente como começaram as conversas sobre o que seria o Zapt. Do ponto de vista técnico, decidimos em duas melhorias importantes: uma linguagem de automação (ZPL) e a operação em background (TSR). Mais importante foi o objetivo destas melhorias, o que a Humana chamou de "processamento cooperativo" (acho que o termo nunca pegou fora da Humana).

De forma resumida, a ideia do processamento cooperativo era que você teria rodando simultaneamente (sobre o coitado do DOS) o Zapt e uma aplicação local. Usando a ZPL, o Zapt seria capaz de fazer automaticamente a troca de dados entre a aplicação local e uma aplicação remota (executada pelo Zapt através de emulação de terminal). Desta forma as duas aplicações "cooperariam" sem precisar ser alteradas.

Nesta época (1988) o processamento em background sobre o DOS era moda e tinham muitos artigos nas revistas a respeito. A rigor era uma gambiarra bem complicada, but não lembro de termos grandes problemas em campo com isso.

A ZPL substituiu o recurso de teclas programáveis do Z. Eu defini a ZPL e implementei a parte de "compilação". À medida que cada linha era digitada ela era processada e convertida em "tokens". No final todo o programa era processado para detectar erros de sintaxe e criar uma tabela de símbolos. A implementação disso se baseou em um livro sobre compiladores de Valdemar Setzer. A execução do programa (mais exatamente uma interpretação) usava basicamente as mesmas estruturas que a compilação. A parte incomum da ZPL é que a saída do programa eram teclas para controlar o Zapt ou a aplicação local. Não existia um comando de gerar teclas, bastava colocar o texto e nomes das teclas especiais:

```
FUNCTION LISTA F3BEGIN "lista" ENTER PGDNENDB
```

Fizemos até um [cartão de referência](https://drive.google.com/file/d/1Fp3kF5Kw64jZLJrVJTuSezSa3ErEhi70/view?usp=sharing "null") com a sintaxe da ZPL e os comandos do Zapt:

Para permitir o processamento cooperativo a tela da aplicação local era acessível dentro do Zapt através de um vídeo especial. No Z e no Zapt os vídeos tinham duas partes: a tela "ativa" e uma "memória de vídeo" com as linhas que já rolaram para fora. Um comando no Z permitia ver e navegar pelo vídeo.

No Zapt isso precisou ser aperfeiçoado, para permitir extrair dados do vídeo. Junte a isso a necessidade de ter uma forma de editar o programa ZPL e chegamos a um editor bem completo (os comandos foram inspirados no WordStar). Faltava braço para fazer o editor e terceirizamos esta parte com uma empresa amiga. O editor foi a parte que mais deu problema no Zapt (pela sua complexidade).

No meu canal tem um [vídeo sobre o Z e o Zapt](https://www.youtube.com/watch?v=lvmhLrZiZV8 "null").
