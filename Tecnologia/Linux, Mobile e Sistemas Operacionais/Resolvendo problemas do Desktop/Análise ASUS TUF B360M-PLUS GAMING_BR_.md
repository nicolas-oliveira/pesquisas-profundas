

# **Relatório Técnico de Diagnóstico e Manutenção – Placa-Mãe ASUS TUF B360M-PLUS GAMING/BR**

## **Introdução**

Este relatório apresenta uma análise técnica exaustiva da placa-mãe ASUS TUF B360M-PLUS GAMING/BR, um componente de hardware projetado para o soquete LGA1151 e compatível com processadores Intel Core de 8ª e 9ª Geração.1 Posicionada dentro da aliança The Ultimate Force (TUF) Gaming, esta placa-mãe Micro-ATX visa oferecer uma plataforma durável e confiável para o segmento de jogos de entrada e intermediário. Sua construção se destaca pelo uso de componentes com certificação de padrão militar, como TUF Chokes, capacitores e MOSFETs, além de um conjunto de salvaguardas de hardware conhecido como TUF Protection.3  
A estrutura deste documento foi concebida para servir como um guia definitivo, consolidando dados de manuais oficiais, bases de conhecimento técnico e uma análise aprofundada de relatos empíricos de comunidades de hardware e canais de reparo. A análise inicia-se com os métodos de diagnóstico primário, interpretando os códigos de bip do sistema durante o Power-On Self-Test (POST). Em seguida, avança para a identificação e o detalhamento de falhas crônicas e problemas recorrentes observados por usuários e técnicos. A terceira seção foca em procedimentos de manutenção preventiva e cuidados técnicos essenciais, extraídos da documentação oficial. Por fim, o relatório culmina em guias práticos para a solução de falhas críticas, oferecendo fluxogramas de diagnóstico e metodologias de reparo que se estendem até o nível de componente, capacitando o técnico a realizar intervenções precisas e eficazes.  
---

## **Seção 1: Diagnóstico por Códigos de Bip (POST Beep Codes)**

### **1.1. O Processo POST e a Função do Speaker**

O Power-On Self-Test (POST) é a rotina de diagnóstico fundamental executada pelo firmware BIOS/UEFI imediatamente após o sistema ser ligado. Sua função é verificar a presença, a conexão e a integridade funcional de componentes de hardware essenciais, como a Unidade Central de Processamento (CPU), a memória de acesso aleatório (RAM) e a placa de vídeo (GPU), antes de iniciar o processo de boot do sistema operacional.4  
A placa-mãe TUF B360M-PLUS GAMING/BR, sendo um modelo de gama média, não possui um display de diagnóstico integrado (comumente conhecido como Q-Code), que exibe códigos alfanuméricos para indicar o status do POST. Na ausência deste recurso, o diagnóstico de falhas de inicialização depende inteiramente dos códigos sonoros, ou "beep codes", emitidos pelo pequeno alto-falante (speaker) do gabinete. Este speaker deve ser conectado ao header de 4 pinos dedicado no painel de sistema da placa-mãe, conforme indicado no manual.3 A interpretação correta das sequências de bipes curtos e longos é, portanto, a primeira e mais crucial etapa para diagnosticar problemas que impedem o sistema de exibir vídeo.

### **1.2. Tabela de Códigos de Bip Críticos (AMI BIOS)**

As placas-mãe da ASUS, incluindo a série TUF, utilizam um BIOS desenvolvido pela American Megatrends Inc. (AMI), o que resulta em um conjunto padronizado de códigos de bip.4 Embora o padrão seja consistente, a experiência prática de técnicos e usuários em fóruns revela nuances que não são abordadas na documentação oficial. A tabela a seguir consolida os códigos de erro mais críticos, combinando a informação oficial da ASUS com interpretações práticas para um diagnóstico mais preciso.5

| Código de Bip (Padrão) | Significado Oficial (ASUS FAQ) | Componente(s) Suspeito(s) | Interpretação Prática e Ações Recomendadas |
| :---- | :---- | :---- | :---- |
| **1 Bip Longo \+ 2 Bips Curtos** | Anomalia detectada na memória.5 | Módulos de RAM, Slots de RAM, CPU (Controlador de Memória Integrado). | Falha de detecção ou inicialização da RAM. **Ações:** 1\. Desligue o PC e desconecte da tomada. 2\. Reinstale os módulos de RAM, garantindo que estejam firmemente encaixados nos slots recomendados (A2 e B2 para dual-channel).3 3\. Limpe os contatos dourados dos módulos com uma borracha branca macia e remova os resíduos com álcool isopropílico. 4\. Teste com apenas um módulo de RAM de cada vez, em cada um dos slots. 5\. Verifique se os módulos de RAM estão na Qualified Vendor List (QVL) da placa-mãe. |
| **1 Bip Longo \+ 3 Bips Curtos** | Anomalia detectada na placa gráfica.5 | Placa de Vídeo (GPU), Slot PCI Express x16, Fonte de Alimentação (conectores de energia da GPU). | Falha na detecção ou inicialização da GPU. **Ações:** 1\. Verifique se a GPU está corretamente inserida no slot PCIe x16 principal. 2\. Confirme que todos os cabos de alimentação PCIe (6 ou 8 pinos) necessários estão conectados firmemente à GPU e à fonte. 3\. Limpe os contatos dourados da GPU. 4\. Teste a GPU em outro slot PCIe (se disponível) ou em outro sistema. 5\. Garanta que o monitor esteja ligado *antes* de ligar o PC e teste diferentes cabos/portas de vídeo (HDMI, DVI).8 |
| **1 Bip Longo \+ 4 Bips Curtos** | Anomalia em componente de hardware ou erro de CPU.5 | CPU, Cooler da CPU, Placa-mãe. | Erro crítico de hardware, frequentemente relacionado à CPU ou seu subsistema. **Ações:** 1\. Verifique se o cooler da CPU está corretamente instalado e conectado ao header CPU\_FAN. 2\. Reinstale a CPU, verificando se há pinos tortos no soquete LGA1151 e aplicando nova pasta térmica. 3\. Execute um reset completo do CMOS. 4\. Se o problema persistir, pode indicar uma falha na própria CPU ou na placa-mãe. |
| **5 Bips Curtos** | Erro de Processador (CPU).4 | CPU, Placa-mãe (soquete ou VRM). | Falha irrecuperável na detecção ou inicialização da CPU. **Ações:** Este é um erro grave. 1\. Siga os mesmos passos do código 1 longo \+ 4 curtos. 2\. Verifique a compatibilidade da CPU com a versão do BIOS instalada (especialmente relevante para CPUs de 9ª Geração). 3\. Teste com uma CPU sabidamente funcional e compatível para isolar a falha entre o processador e a placa-mãe. |
| **Nenhum Bip (com LEDs da placa acesos)** | Falha de CPU, BIOS ou alimentação primária.5 | CPU, BIOS (corrompido), Placa-mãe (curto-circuito), Fonte de Alimentação. | O sistema recebe energia (standby), mas falha em uma etapa muito inicial do POST, antes mesmo da inicialização da CPU ou da memória. **Ações:** 1\. Verifique se o conector de alimentação da CPU de 8 pinos (EATX12V) está conectado. 2\. Realize um teste de bancada, removendo a placa-mãe do gabinete e conectando apenas o essencial (CPU, 1 RAM, fonte). 3\. Tente um reset de CMOS. 4\. Se a falha persistir, suspeite de um BIOS corrompido, uma falha na CPU ou um curto-circuito na placa-mãe. |

### **1.3. Análise de Casos Reais e Discrepâncias**

A interpretação literal dos códigos de bip, embora útil, pode levar a diagnósticos incorretos se não for contextualizada com a experiência prática. A análise de discussões em fóruns técnicos revela que a causa raiz de um erro pode ser mais sutil do que o manual sugere, muitas vezes relacionada à interação entre componentes em vez de uma falha isolada.  
Um exemplo notável é o código 1 bip longo \+ 3 bips curtos, que oficialmente aponta para uma falha na placa de vídeo.5 No entanto, múltiplos relatos de usuários descrevem cenários em que o sistema emite este código, mas funciona perfeitamente.6 Em uma discussão detalhada, um usuário descobriu que o erro só ocorria se o monitor estivesse desligado ou em modo de espera durante a inicialização do PC. Ao ligar o monitor após ouvir os bipes, o sistema operacional carregava normalmente.8  
Este comportamento expõe uma nuance crítica do processo POST: o firmware não está apenas verificando a presença física da GPU, mas também tentando estabelecer uma comunicação com um dispositivo de exibição. Se o monitor não responder rapidamente ao "handshake" inicial, seja por um tempo de inicialização lento, um cabo de vídeo de baixa qualidade ou uma incompatibilidade momentânea com a porta (especialmente DisplayPort), o BIOS pode interpretar isso como uma ausência de dispositivo de vídeo e registrar o erro correspondente. A implicação prática para um técnico é clara: antes de condenar uma placa de vídeo cara, deve-se instruir o usuário a realizar testes simples, como ligar o monitor antes do PC, experimentar diferentes cabos e portas de vídeo, e, se possível, testar com um monitor diferente. Este conhecimento, derivado da experiência coletiva da comunidade, transcende a documentação oficial e previne a substituição desnecessária de hardware funcional.  
---

## **Seção 2: Análise de Problemas Comuns e Falhas Recorrentes**

### **2.1. Mapeamento de Falhas Crônicas**

Além dos erros genéricos indicados pelos códigos de bip, a placa-mãe TUF B360M-PLUS GAMING/BR exibe um padrão de falhas recorrentes que foram identificadas através da compilação de dados de fontes de alta credibilidade na comunidade de hardware, como os fóruns Linus Tech Tips 9, discussões no Reddit 10 e demonstrações de reparo em canais técnicos do YouTube.11 Estes problemas específicos apontam para sensibilidades no firmware, no design elétrico e na interação com softwares de controle, oferecendo um panorama mais profundo dos potenciais pontos de falha desta placa.

### **2.2. Matriz de Problemas Recorrentes**

A tabela a seguir serve como um índice de referência rápida para os problemas mais complexos e específicos relatados para este modelo. Ela permite que um técnico identifique rapidamente uma falha com base nos sintomas e localize as fontes de discussão para um aprofundamento.

| Problema | Sintomas | Causa Provável (Análise) | Fonte(s) Principal(is) de Relato |
| :---- | :---- | :---- | :---- |
| **Falha de Inicialização Pós-Desligamento Remoto** | LEDs da placa acendem, mas não há resposta ao jumper do botão de power. Fans não giram. O problema desaparece após um ciclo de energia completo (desconectar da tomada). | Estado de "lock-up" do firmware, possivelmente no controlador Super I/O, causado por um comando ACPI não padrão enviado por software de gerenciamento remoto (ex: HiveOS), agravado por conflitos com software ASUS (AI Suite/Armoury Crate). | Linus Tech Tips 9 |
| **Sem Vídeo Após Desativar CSM no BIOS** | O sistema liga (fans, LEDs, periféricos), mas não há sinal de vídeo em nenhuma saída (onboard ou GPU). Não é possível acessar a tela do BIOS. | O sistema foi forçado para o modo UEFI puro. A falha ocorre porque o SO foi instalado em um disco com partição MBR (não inicializável em UEFI) ou a GPU não possui um driver de firmware UEFI (GOP). | Reddit 10 |
| **Incompatibilidade de CPU (9ª Geração)** | O sistema não posta (sem bipes ou com bipes de erro de CPU) ao instalar um processador Intel de 9ª Geração (ex: i5-9400F). | A versão do BIOS de fábrica é muito antiga e não contém o microcódigo necessário para suportar CPUs de 9ª Geração. | ASUS CPU QVL 13, PCPartPicker/Reddit 15 |
| **Curto-Circuito no VRM da CPU** | O sistema não liga (morte súbita). A fonte de alimentação pode tentar ligar e desligar imediatamente (proteção contra curto-circuito). | Falha de um ou mais MOSFETs no Módulo Regulador de Tensão (VRM), causando um curto entre a linha de 12V e o terra. Frequentemente associado ao estresse térmico em CPUs de TDP mais alto. | Canais de reparo no YouTube 11, Fóruns de eletrônica 17 |

### **2.3. Análise Aprofundada das Falhas**

#### **2.3.1. Falha de Inicialização (No POST / No Power)**

**Cenário A: Falha ao Ligar Após Desligamento Remoto (Relato LTT)**  
Um dos problemas mais intrigantes e específicos documentados para esta linha de placas-mãe é a falha em ligar após um desligamento iniciado por software remoto, como o sistema operacional de mineração HiveOS.9 Os sintomas são consistentes: a placa recebe energia standby (indicada pelos LEDs RGB acesos), mas ignora completamente as tentativas de inicialização via jumper do botão de power. Não há rotação dos fans nem tentativa de POST. O comportamento é resolvido temporariamente ao desconectar fisicamente a fonte de alimentação da tomada por alguns minutos, o que sugere um problema de estado lógico em vez de uma falha de hardware permanente.9  
A investigação deste comportamento aponta para uma complexa interação entre o firmware da placa, os estados de energia ACPI (Advanced Configuration and Power Interface) e os softwares de controle da ASUS, como o AI Suite e o Armoury Crate. Estes softwares são conhecidos por se integrarem profundamente ao sistema para gerenciar voltagens, ventoinhas e estados de energia.19 O Armoury Crate, em particular, pode ser configurado para se instalar automaticamente a partir de uma opção no próprio BIOS, criando uma camada de software persistente que pode entrar em conflito com outros sistemas de gerenciamento.20  
A hipótese mais provável é que o comando de desligamento enviado pelo HiveOS utiliza um sinal ACPI que o firmware da B360M, possivelmente interferido por hooks ou serviços residuais do AI Suite, interpreta de forma incorreta. Isso pode levar um dos controladores principais da placa, como o chip Super I/O (responsável por monitoramento e inicialização), a entrar em um estado de proteção ou "limbo". Neste estado, ele para de responder aos sinais de inicialização do painel frontal. Como este controlador é alimentado pela linha de 5V standby, apenas um ciclo de energia completo (desconectar da tomada) é capaz de reiniciá-lo e restaurar a funcionalidade. A solução definitiva, portanto, não reside na substituição de hardware, mas na erradicação completa de todos os softwares de utilidade da ASUS e na desativação de qualquer opção de instalação automática no BIOS.  
**Cenário B: Sem Vídeo Após Desativar o CSM no BIOS**  
Outra falha recorrente e de fácil reprodução é a perda total de sinal de vídeo após desativar o "Compatibility Support Module" (CSM) nas configurações do BIOS.10 O usuário relata que o computador liga, com fans e LEDs funcionando, mas o monitor permanece preto, sem exibir a tela do POST ou permitir o acesso ao BIOS.10  
Este problema é uma consequência direta do que a desativação do CSM implica. O CSM é uma camada de compatibilidade que permite a um firmware UEFI moderno emular um ambiente de BIOS legado. Isso é necessário para inicializar sistemas operacionais instalados em discos com um esquema de partição Master Boot Record (MBR) e para usar placas de expansão mais antigas que não possuem firmware compatível com UEFI.  
Ao desativar o CSM, a placa-mãe é forçada a operar em modo UEFI puro. Isso impõe duas condições estritas:

1. O dispositivo de boot deve usar o esquema de partição GUID Partition Table (GPT).  
2. A placa de vídeo deve ter um driver de firmware UEFI, conhecido como Graphics Output Protocol (GOP), para poder exibir a interface do BIOS/UEFI.

A falha ocorre porque a maioria dos usuários que encontra esse problema possui um sistema operacional (como o Windows) instalado em um disco MBR. Ao desativar o CSM, a placa-mãe não consegue mais encontrar um sistema inicializável e trava o processo de boot antes mesmo de carregar os drivers de vídeo do sistema operacional. No caso de um usuário com uma GTX 1060 10, embora a placa tenha um driver GOP, o problema principal é a partição MBR do disco de boot. A solução primária e mais simples é realizar um reset completo do CMOS para reverter a configuração e reativar o CSM. Para usuários que desejam operar em modo UEFI puro (necessário para recursos como Secure Boot ou Re-Size BAR), a solução definitiva é converter o disco de MBR para GPT ou realizar uma instalação limpa do sistema operacional com o CSM já desativado.

#### **2.3.2. Incompatibilidade de CPU por BIOS Desatualizado**

A TUF B360M-PLUS GAMING/BR foi lançada primariamente para suportar a 8ª Geração de processadores Intel (Coffee Lake). O suporte para a 9ª Geração (Coffee Lake Refresh), que inclui modelos extremamente populares como o Core i5-9400 e o i5-9400F, foi adicionado posteriormente através de atualizações de BIOS.13 Consequentemente, unidades da placa-mãe fabricadas antes da disponibilização desses updates e que permaneceram em estoque por um longo período não serão capazes de inicializar com uma CPU de 9ª Geração instalada.15  
A verificação da página de suporte de CPU da ASUS é crítica. Para o processador Intel Core i5-9400, existem diferentes requisitos de BIOS dependendo da revisão (stepping) do processador 14:

* **Core i5-9400 (revisões P0 e U0):** Requer no mínimo a versão de BIOS **1404** (lançada em 28/09/2018).13  
* **Core i5-9400 (revisão R0):** Requer no mínimo a versão de BIOS **2201**.13

Isso cria um dilema técnico significativo, frequentemente chamado de "problema do ovo e da galinha". Um usuário que adquire a placa e uma CPU de 9ª Geração pode se encontrar em uma situação onde não consegue ligar o computador para atualizar o BIOS, pois a CPU instalada não é reconhecida pelo firmware antigo. A TUF B360M-PLUS GAMING/BR não possui o recurso ASUS BIOS FlashBack, que permite a atualização do BIOS sem a necessidade de uma CPU ou RAM instalada. Portanto, a única solução viável para o usuário final é obter temporariamente uma CPU compatível da 8ª Geração (como um Celeron G4900, Pentium G5400 ou Core i3-8100) 13, instalá-la apenas para realizar a atualização do BIOS e, em seguida, instalar a CPU de 9ª Geração desejada. Este é um obstáculo logístico e de custo que pode frustrar montadores de primeira viagem.

#### **2.3.3. Curto-Circuito no Circuito de Alimentação (MOSFETs/VRM)**

Relatos de canais de reparo e fóruns de eletrônica indicam que uma causa comum para a "morte súbita" de placas-mãe como a TUF B360M-PLUS GAMING/BR é a falha no Módulo Regulador de Tensão (VRM).11 O VRM é o circuito responsável por converter a tensão de \+12V fornecida pelo conector de 8 pinos da fonte de alimentação para a tensão muito mais baixa (geralmente entre 1.0V e 1.4V) e de alta corrente que a CPU requer para operar (Vcore).24 Este circuito é composto principalmente por MOSFETs (transistores), indutores (chokes) e capacitores.  
Uma análise visual do layout da placa no manual 3 e o conhecimento de mercado sobre o chipset B360 indicam que este modelo utiliza um design de VRM de 5 fases (provavelmente 4 fases para o Vcore e 1 para os gráficos integrados). Embora os componentes sejam da linha "TUF", este é um design modesto. Ele é perfeitamente adequado para operar com CPUs de 65W de TDP, como o i5-8400 ou i5-9400F, em suas configurações de fábrica. No entanto, quando pareado com CPUs de 95W desbloqueadas (como o i7-8700K ou i5-9600K), mesmo sem overclock, o VRM pode operar sob estresse térmico significativo durante cargas de trabalho prolongadas, como jogos ou renderização.25  
O superaquecimento contínuo pode levar à degradação dos MOSFETs, culminando em uma falha catastrófica onde o transistor entra em curto-circuito, conectando diretamente a linha de 12V ao terra. Isso aciona a proteção contra curto-circuito (SCP) da fonte de alimentação, que desliga imediatamente para evitar danos maiores, resultando em um sistema que não liga.17 Um fator agravante é que este modelo de placa-mãe, como muitos em sua categoria, não possui um sensor de temperatura dedicado para o VRM que seja legível por softwares de monitoramento como o HWInfo64.25 Isso impede que o usuário monitore proativamente as temperaturas do VRM e tome medidas preventivas, como melhorar o fluxo de ar no gabinete.  
---

## **Seção 3: Manutenção e Cuidados Técnicos Essenciais**

### **3.1. Gerenciamento e Atualização de BIOS**

A manutenção adequada do BIOS é fundamental para a estabilidade, compatibilidade e segurança do sistema. A ASUS fornece ferramentas robustas para gerenciamento, mas seu uso correto é imperativo.  
**Procedimento de Atualização (ASUS EZ Flash 3\)**  
A atualização do BIOS deve ser realizada com cautela para evitar a corrupção do firmware. O método recomendado é através da ferramenta EZ Flash 3, integrada ao próprio BIOS.3

1. **Download:** Acesse a página de suporte oficial da ASUS para a TUF B360M-PLUS GAMING/BR e baixe o arquivo de BIOS mais recente.23  
2. **Preparação do Pendrive:** Extraia o arquivo .zip baixado. Localize o arquivo de BIOS, que terá a extensão .CAP. Renomeie este arquivo para TB360MP.CAP.3 Copie este arquivo renomeado para a raiz de um pendrive formatado com o sistema de arquivos FAT32.  
3. **Acesso ao BIOS:** Reinicie o computador e pressione a tecla \<Delete\> ou \<F2\> repetidamente durante o POST para entrar no menu do BIOS.3  
4. **Execução do EZ Flash 3:** Uma vez no BIOS, pressione \<F7\> para entrar no "Advanced Mode". Navegue até a aba "Tool" e selecione a opção "ASUS EZ Flash 3 Utility".  
5. **Atualização:** Na interface do EZ Flash, selecione a opção "via Storage Device(s)". Navegue até o seu pendrive e selecione o arquivo TB360MP.CAP. O sistema solicitará confirmação para ler e depois para atualizar o BIOS. Confirme ambas as etapas.  
6. **Finalização:** O processo de atualização começará. É absolutamente crítico que o sistema não seja desligado ou reiniciado durante este procedimento. Após a conclusão, o sistema reiniciará automaticamente. Recomenda-se entrar novamente no BIOS e carregar as configurações padrão otimizadas ("Load Optimized Defaults") pressionando \<F5\>.

**Procedimento de Reset de CMOS (Clear RTC RAM)**  
Um reset do CMOS apaga todas as configurações personalizadas do BIOS e as reverte para os padrões de fábrica. Este é um passo essencial de troubleshooting para resolver problemas de instabilidade ou falhas de boot causadas por configurações incorretas, como a desativação do CSM.10

* **Método 1 (Jumper CLRTC):**  
  1. Desligue completamente o computador e desconecte o cabo de alimentação da tomada.  
  2. Localize o header de 2 pinos chamado CLRTC na placa-mãe, consultando o diagrama no manual.3  
  3. Use um objeto metálico, como a ponta de uma chave de fenda, para tocar e manter contato com ambos os pinos simultaneamente por cerca de 5 a 10 segundos.  
  4. Remova o objeto, reconecte o cabo de alimentação e ligue o PC. Será necessário reconfigurar o BIOS.  
* **Método 2 (Remoção da Bateria):**  
  1. Desligue o computador e desconecte o cabo de alimentação.  
  2. Localize a bateria de lítio CR2032, que se parece com uma moeda, na placa-mãe.  
  3. Com cuidado, remova a bateria de seu soquete.  
  4. Aguarde de 5 a 10 minutes para garantir que todos os capacitores que mantêm a memória CMOS energizada se descarreguem completamente.10  
  5. Recoloque a bateria no soquete, reconecte a energia e ligue o sistema.

### **3.2. Verificação de Compatibilidade de Hardware (QVL)**

A estabilidade de um sistema depende fundamentalmente da compatibilidade entre a placa-mãe, a CPU e a memória RAM. A ASUS mantém listas de fornecedores qualificados (Qualified Vendor Lists \- QVLs) que detalham os componentes exatos testados e validados para cada placa-mãe.13  
Regras de Instalação de Memória 3:

* **Capacidade e Tipo:** A placa suporta um máximo de 64 GB de memória RAM DDR4, do tipo "unbuffered" e "non-ECC", distribuídos em seus quatro slots DIMM.1  
* **Velocidade:** A velocidade de memória oficialmente suportada vai até 2666 MHz. Módulos com velocidades superiores (anunciados com perfis XMP) funcionarão no máximo a 2666 MHz, a menos que se trate de um processador Intel de 8ª Geração com menos de 6 núcleos, caso em que o limite pode ser inferior.2  
* **Arquitetura Dual-Channel:** Para obter o melhor desempenho, a memória deve ser instalada em modo dual-channel. Para isso, os módulos devem ser inseridos nos slots de mesma cor, que, segundo o padrão da ASUS, são o segundo e o quarto a partir do soquete da CPU (slots **A2** e **B2**).3  
* **Compatibilidade:** Para máxima estabilidade, é altamente recomendado instalar módulos de memória com a mesma latência CAS (CAS Latency) e, idealmente, que sejam do mesmo kit (vendidos juntos em um pacote) e do mesmo fabricante. Consultar a QVL de memória no site da ASUS antes da compra é a prática mais segura.3

### **3.3. Práticas de Manutenção Preventiva**

* **Limpeza de Contatos:** Com o tempo, a poeira e a oxidação podem se acumular nos contatos dourados dos módulos de RAM e das placas de vídeo, levando a mau contato e erros de POST.5 Uma manutenção preventiva eficaz envolve a remoção periódica desses componentes (com o PC desligado e sem energia) e a limpeza cuidadosa de seus contatos. Utilize uma borracha escolar branca e macia para esfregar suavemente os contatos, removendo a camada de oxidação. Em seguida, use um cotonete ou pano sem fiapos umedecido com álcool isopropílico para limpar qualquer resíduo da borracha e garantir uma superfície de contato limpa.  
* **Remoção de Software Bloatware (AI Suite):** Conforme detalhado na Seção 2, softwares como o AI Suite 3 podem introduzir instabilidade e conflitos de gerenciamento de energia. A prática recomendada é evitar sua instalação. Caso já esteja instalado, sua remoção deve ser completa. A ASUS fornece uma ferramenta dedicada, o "AI Suite 3 Cleaner", para este fim.19 Após a desinstalação, é crucial entrar no BIOS e desativar qualquer opção que promova a instalação automática de softwares como o Armoury Crate, garantindo que o sistema operacional permaneça livre de interferências de utilitários da ASUS.20

---

## **Seção 4: Guia Prático de Solução de Falhas Críticas**

Esta seção fornece fluxogramas e procedimentos passo a passo para resolver os problemas recorrentes identificados na Seção 2, oferecendo uma abordagem prática e acionável para o diagnóstico e o reparo.

### **4.1. Solucionando Falhas de Inicialização**

**Fluxograma de Diagnóstico "Não Liga / Não Posta":**

1. **Verificação de Energia Primária:** Observe a placa-mãe. O LED laranja de standby, próximo aos slots de RAM, está aceso?  
   * **Não:** O problema está na alimentação externa. Verifique o cabo de força, a tomada, o interruptor da fonte de alimentação e a própria fonte.  
   * **Sim:** A placa está recebendo energia standby. Prossiga para o próximo passo.  
2. **Diagnóstico Sonoro:** Com um speaker de gabinete devidamente conectado, tente ligar o sistema. Você ouve algum código de bip?  
   * **Sim:** Consulte a Tabela 1 na Seção 1.2. O código de bip indicará o componente com falha (RAM, GPU, CPU). Prossiga com as ações recomendadas para aquele código.  
   * **Não (Nenhum Bip):** A falha é crítica e ocorre muito cedo no POST. Prossiga para o isolamento de componentes.  
3. **Teste de Bancada (Isolamento):**  
   * Desligue e desconecte tudo da energia. Remova a placa-mãe do gabinete e coloque-a sobre uma superfície não condutiva (como a caixa da própria placa).  
   * Desconecte todos os componentes e cabos, exceto o essencial: a CPU (com cooler), um único módulo de RAM (instalado no slot A2), o conector de alimentação principal de 24 pinos e o conector de alimentação da CPU de 8 pinos.  
   * Tente ligar o sistema curto-circuitando momentaneamente os dois pinos PWR\_SW no header do painel frontal.  
   * Se o sistema agora ligar e emitir um código de bip (provavelmente de ausência de GPU), um dos componentes desconectados (HD/SSD, outra RAM, placa USB, etc.) ou o próprio gabinete (curto-circuito com a placa) é o culpado. Reconecte um componente de cada vez para identificar o causador.  
4. **Falha Persistente em Bancada:** Se o sistema ainda não der sinal de vida (sem bipes, sem rotação de fans), os suspeitos restantes são a fonte de alimentação, a placa-mãe ou a CPU.  
   * Teste com uma fonte de alimentação diferente que seja sabidamente funcional.  
   * Se a falha persistir, a probabilidade aponta para um defeito na placa-mãe (ex: VRM em curto) ou na própria CPU.

**Guia de Solução para "Falha Pós-Desligamento Remoto":**

1. **Recuperação Imediata:** Desconecte o cabo de força da fonte de alimentação. Pressione e segure o botão de ligar do gabinete por 15 a 20 segundos para descarregar completamente os capacitores. Aguarde de 5 a 10 minutos antes de reconectar a energia e tentar ligar novamente. Isso deve resetar o estado de "lock-up" do controlador de energia.  
2. **Solução Preventiva (Software):** Inicialize o sistema operacional e realize uma desinstalação completa de todos os utilitários ASUS. Utilize o "AI Suite 3 Cleaner" 19 e o "Armoury Crate Uninstall Tool" 31 disponíveis no site da ASUS. Para uma limpeza mais profunda, utilize ferramentas de terceiros como o Revo Uninstaller no modo avançado para remover arquivos e entradas de registro residuais.32  
3. **Solução Preventiva (BIOS):** Entre no BIOS, navegue até a aba "Tool" ou "Advanced" e localize a opção "Download & Install Armoury Crate" ou similar. Defina-a como "Disabled" para impedir que o firmware tente reinstalar o software no sistema operacional.20

**Guia de Recuperação para "Sem Vídeo Após Desativar CSM":**

1. **Ação Primária:** Realize um reset completo do CMOS, preferencialmente utilizando o método do jumper CLRTC, conforme descrito na Seção 3.1. Isso reverterá todas as configurações do BIOS para o padrão de fábrica, reativando o CSM.  
2. **Verificação de Saída de Vídeo:** Se o reset do CMOS não resolver, e sua CPU possuir gráficos integrados (modelos que não terminam em "F", como o i5-8400), remova a placa de vídeo dedicada e conecte o monitor diretamente à porta HDMI ou DVI da placa-mãe para tentar obter imagem.10  
3. **Ação Definitiva (para operar sem CSM):** Se o objetivo é manter o CSM desativado, uma reinstalação do sistema operacional é necessária. Antes de mudar a configuração no BIOS, é possível tentar converter o disco de boot de MBR para GPT usando a ferramenta MBR2GPT.exe fornecida no Windows. A abordagem mais segura, no entanto, é fazer backup dos dados, entrar no BIOS, desativar o CSM, e então realizar uma instalação limpa do Windows, garantindo que o instalador formate o disco de destino como GPT.

### **4.2. Resolvendo Incompatibilidade de CPU (9ª Geração)**

1. **Diagnóstico:** O sistema não posta, não emite bipes ou apresenta um código de erro de CPU (como 5 bipes curtos) após a instalação de um processador Intel de 9ª Geração.  
2. **Requisito de Hardware:** A solução exige acesso temporário a um processador de 8ª Geração compatível com as versões de BIOS mais antigas (ex: Core i3-8100, Celeron G4900, Pentium G5400).13  
3. Procedimento de Atualização:  
   a. Desligue o sistema e instale a CPU de 8ª Geração.  
   b. Ligue o sistema. Ele deve agora postar e permitir o acesso ao BIOS.  
   c. Siga o procedimento completo de atualização do BIOS via EZ Flash 3, conforme detalhado na Seção 3.1, para instalar a versão mais recente disponível.  
   d. Após a atualização ser concluída e o sistema reiniciar, desligue-o completamente.  
   e. Substitua a CPU de 8ª Geração pela CPU de 9ª Geração desejada.  
   f. Ligue o sistema. Ele agora deve reconhecer a nova CPU e postar normalmente. É uma boa prática entrar no BIOS e carregar as configurações padrão otimizadas (Load Optimized Defaults) após uma troca de hardware tão significativa.

### **4.3. Reparo em Nível de Componente: Diagnóstico de MOSFETs em Curto**

**Aviso de Risco:** Este procedimento é destinado a técnicos com experiência em eletrônica e soldagem de componentes de montagem em superfície (SMD). A execução incorreta pode causar danos permanentes e irreparáveis à placa-mãe. Prossiga com total conhecimento dos riscos envolvidos.  
**Localização:** Os MOSFETs do circuito VRM da CPU estão agrupados ao redor do soquete do processador, geralmente sob um dissipador de calor metálico que precisa ser removido para acesso.3  
**Guia Prático de Teste (com a placa desligada e sem energia):**

1. **Configuração do Multímetro:** Coloque o multímetro digital na função de teste de diodo (símbolo de diodo) ou na função de continuidade (que emite um som em caso de curto-circuito).  
2. **Identificação dos Pinos:** Um MOSFET de 8 pinos típico em placas-mãe (formato SO-8) possui 4 pinos de Source (geralmente interligados), 3 pinos de Drain (interligados e conectados à aba metálica superior) e 1 pino de Gate.33 Consulte o datasheet do componente ou um boardview para confirmação.  
3. **Teste de Curto (Drain-Source):** Este é o teste principal para identificar uma falha catastrófica.  
   * Coloque a ponta de prova preta (negativa) do multímetro em qualquer um dos pinos do Drain.  
   * Coloque a ponta de prova vermelha (positiva) em qualquer um dos pinos do Source.  
   * **Leitura Esperada (MOSFET bom):** O multímetro deve exibir uma leitura de queda de tensão, tipicamente entre 0.3V e 0.7V, que corresponde ao diodo de corpo interno do MOSFET.35  
   * Inverta as pontas (vermelha no Drain, preta no Source). A leitura esperada é de circuito aberto (o multímetro exibirá "OL" ou "1").  
   * **Leitura de Falha (MOSFET em curto):** Se o multímetro apitar ou exibir uma leitura muito baixa (próxima de 0.00V) em ambas as direções do teste, o MOSFET está em curto-circuito e precisa ser substituído.18  
4. **Teste de Fuga no Gate:** Não deve haver continuidade (nenhum apito, leitura "OL") entre o pino do Gate e os pinos de Source ou Drain. Se houver qualquer leitura de continuidade, o MOSFET está com defeito.  
5. **Reparo:** A substituição de um MOSFET SMD requer uma estação de solda de ar quente para remover o componente defeituoso e soldar o novo. É crucial usar um MOSFET de substituição com especificações elétricas idênticas ou superiores (VDS, ID, RDS(on)). A identificação precisa do componente pode exigir a consulta de esquemas elétricos (boardview) específicos para a TUF B360M-PLUS GAMING.36

---

## **Conclusão**

### **Sumário Técnico**

A análise aprofundada da ASUS TUF B360M-PLUS GAMING/BR revela que, embora seja uma placa-mãe robusta e confiável para sua faixa de preço, ela possui vulnerabilidades específicas que técnicos e usuários avançados devem conhecer. A sua estabilidade é notavelmente impactada por interações de firmware com softwares de gerenciamento, especialmente em cenários de energia não-padrão, como desligamentos remotos. A ausência do recurso BIOS FlashBack impõe uma barreira logística significativa para a compatibilidade com hardware mais recente (CPUs de 9ª Geração), exigindo uma CPU mais antiga para atualizações de BIOS. Adicionalmente, seu circuito VRM, embora funcional para as especificações do chipset, representa um potencial ponto de falha quando submetido a estresse térmico prolongado por CPUs de TDP mais alto, uma vulnerabilidade agravada pela falta de monitoramento de temperatura acessível ao usuário.

### **Recomendações Finais**

Para maximizar a estabilidade, a longevidade e a facilidade de diagnóstico da TUF B360M-PLUS GAMING/BR, as seguintes práticas são fortemente recomendadas:

1. **Manter o BIOS Atualizado:** Verifique e instale regularmente a versão mais recente do BIOS disponível na página de suporte oficial da ASUS. Isso garante a máxima compatibilidade de hardware, correções de segurança e melhorias de estabilidade.  
2. **Evitar Softwares de Controle da ASUS:** Para evitar conflitos de software e problemas de gerenciamento de energia, recomenda-se não instalar utilitários como o AI Suite 3 ou o Armoury Crate. Se a funcionalidade for necessária, utilize o BIOS para controle de ventoinhas (Q-Fan Control) e softwares de terceiros de boa reputação (como HWInfo64) para monitoramento. Desative a instalação automática do Armoury Crate no BIOS.  
3. **Garantir Ventilação Adequada:** Assegure um bom fluxo de ar dentro do gabinete, direcionando ar fresco sobre a área do VRM, especialmente se estiver utilizando um processador com TDP de 95W. Isso ajuda a mitigar o estresse térmico e a prolongar a vida útil dos componentes de alimentação.  
4. **Consultar a QVL:** Antes de adquirir novos módulos de RAM, sempre verifique a Qualified Vendor List (QVL) no site da ASUS para garantir a compatibilidade validada e evitar problemas de instabilidade ou falhas de POST.  
5. **Adotar uma Abordagem Metódica de Diagnóstico:** Em caso de falha de inicialização, siga um processo lógico de troubleshooting. Comece com a interpretação dos códigos de bip, prossiga com o isolamento de componentes (teste de bancada) e a verificação de configurações de BIOS antes de assumir uma falha de hardware catastrófica.

#### **Referências citadas**

1. ASUS TUF B360M-PLUS GAMING S | Overview, Specs, Details | SHI, acessado em julho 29, 2025, [https://www.shi.com/product/35781294/ASUS-TUF-B360M-PLUS-GAMING-S](https://www.shi.com/product/35781294/ASUS-TUF-B360M-PLUS-GAMING-S)  
2. T. Madre ASUS TUF B360M-PLUS GAMING, Chipset Intel B360, Soporta: Intel Core i7 / i5 / i3 de 8va Gen., Socket 1151, Memoria: DDR4 2666/2400/2133 MHz, 64GB Max, Integrado: Audio HD, Red, USB 3.1 y SATA 3.0, M.2, Micro- \- PCEL, acessado em julho 29, 2025, [https://www.pcel.com/ASUS-TUF-B360M-PLUSGAMING-T-Madre-ASUS-TUF-B360M-PLUS-GAMING-Chipset-Intel-B360-Soporta-Intel-Core-i7-i5-i3-de-8va-Gen-Socket-1151-Memoria-DDR4-2666-2400-2133-MHz-64GB-Max-Integra-192940](https://www.pcel.com/ASUS-TUF-B360M-PLUSGAMING-T-Madre-ASUS-TUF-B360M-PLUS-GAMING-Chipset-Intel-B360-Soporta-Intel-Core-i7-i5-i3-de-8va-Gen-Socket-1151-Memoria-DDR4-2666-2400-2133-MHz-64GB-Max-Integra-192940)  
3. TUF B360M-PLUS GAMING/BR \- ASUS, acessado em julho 29, 2025, [https://dlcdnets.asus.com/pub/ASUS/mb/LGA1151/TUF\_B360M-PLUS\_GAMING\_BR/E14070\_TUF\_B360M-PLUS\_GAMING\_BR\_UM\_web.pdf](https://dlcdnets.asus.com/pub/ASUS/mb/LGA1151/TUF_B360M-PLUS_GAMING_BR/E14070_TUF_B360M-PLUS_GAMING_BR_UM_web.pdf)  
4. What Are ASUS Beep Codes and How To Identify Them | SoftwareKeep, acessado em julho 29, 2025, [https://softwarekeep.com/blogs/news/what-are-asus-beep-codes-and-how-to-identify-them](https://softwarekeep.com/blogs/news/what-are-asus-beep-codes-and-how-to-identify-them)  
5. \[Motherboard\] How to use buzzer to troubleshoot monitor display issues | Official Support, acessado em julho 29, 2025, [https://www.asus.com/us/support/faq/1029959/](https://www.asus.com/us/support/faq/1029959/)  
6. ASUS Beep Codes Help : r/buildapc \- Reddit, acessado em julho 29, 2025, [https://www.reddit.com/r/buildapc/comments/jbgqa7/asus\_beep\_codes\_help/](https://www.reddit.com/r/buildapc/comments/jbgqa7/asus_beep_codes_help/)  
7. hardware failure \- Beep-Codes definition for ASUS motherboard \- Super User, acessado em julho 29, 2025, [https://superuser.com/questions/783082/beep-codes-definition-for-asus-motherboard](https://superuser.com/questions/783082/beep-codes-definition-for-asus-motherboard)  
8. 1 long and 3 short signals \- Republic of Gamers Forum \- 674645, acessado em julho 29, 2025, [https://rog-forum.asus.com/t5/x99/1-long-and-3-short-signals/td-p/674645](https://rog-forum.asus.com/t5/x99/1-long-and-3-short-signals/td-p/674645)  
9. ASUS TUF B360-Plus Gaming Not Turning On \- Troubleshooting \- Linus Tech Tips, acessado em julho 29, 2025, [https://linustechtips.com/topic/1397313-asus-tuf-b360-plus-gaming-not-turning-on/](https://linustechtips.com/topic/1397313-asus-tuf-b360-plus-gaming-not-turning-on/)  
10. I disabled the CSM option in BIOS and my computer don't turn video on anymore \- Reddit, acessado em julho 29, 2025, [https://www.reddit.com/r/ASUS/comments/n3d44o/i\_disabled\_the\_csm\_option\_in\_bios\_and\_my\_computer/](https://www.reddit.com/r/ASUS/comments/n3d44o/i_disabled_the_csm_option_in_bios_and_my_computer/)  
11. TUF B360 PRO GAMING WIFI NO DISPLAY FIXED \- YouTube, acessado em julho 29, 2025, [https://www.youtube.com/watch?v=V5bR-wARpow](https://www.youtube.com/watch?v=V5bR-wARpow)  
12. TUF B360M-PLUS GAMING EM CURTO COMO RESOLVER \- YouTube, acessado em julho 29, 2025, [https://www.youtube.com/watch?v=p5YfTHUPS14](https://www.youtube.com/watch?v=p5YfTHUPS14)  
13. TUF B360M-PLUS GAMING/BR \- Support \- ASUS, acessado em julho 29, 2025, [https://www.asus.com/sg/supportonly/tuf%20b360m-plus%20gamingbr/helpdesk\_cpu/](https://www.asus.com/sg/supportonly/tuf%20b360m-plus%20gamingbr/helpdesk_cpu/)  
14. TUF B360M-PLUS GAMING/BR \- Support \- ASUS, acessado em julho 29, 2025, [https://www.asus.com/supportonly/tuf%20b360m-plus%20gamingbr/helpdesk\_cpu/](https://www.asus.com/supportonly/tuf%20b360m-plus%20gamingbr/helpdesk_cpu/)  
15. Does the mother board (asus tuf b360-plus gaming) work with Intel core-i5 9400F \- Reddit, acessado em julho 29, 2025, [https://www.reddit.com/r/buildapc/comments/drx85a/does\_the\_mother\_board\_asus\_tuf\_b360plus\_gaming/](https://www.reddit.com/r/buildapc/comments/drx85a/does_the_mother_board_asus_tuf_b360plus_gaming/)  
16. Is BIOS update needed? Asus TUF B360M-Plus \+ Intel Core i5-9400F Coffee Lake \- Reddit, acessado em julho 29, 2025, [https://www.reddit.com/r/techsupport/comments/g6wjnk/is\_bios\_update\_needed\_asus\_tuf\_b360mplus\_intel/](https://www.reddit.com/r/techsupport/comments/g6wjnk/is_bios_update_needed_asus_tuf_b360mplus_intel/)  
17. Bad mosfet on this motherboard?? \- EEVblog, acessado em julho 29, 2025, [https://www.eevblog.com/forum/beginners/bad-mosfet-on-this-motherboard/](https://www.eevblog.com/forum/beginners/bad-mosfet-on-this-motherboard/)  
18. Help IDing replacement MOSFET \- Asus tuf gaming laptop f15 : r/AskElectronics \- Reddit, acessado em julho 29, 2025, [https://www.reddit.com/r/AskElectronics/comments/10w09rm/help\_iding\_replacement\_mosfet\_asus\_tuf\_gaming/](https://www.reddit.com/r/AskElectronics/comments/10w09rm/help_iding_replacement_mosfet_asus_tuf_gaming/)  
19. \[Motherboard\] AI Suite 3/AI Suite 3 Cleaner- Introduction and troubleshooting \- ASUS, acessado em julho 29, 2025, [https://www.asus.com/support/faq/1012780/](https://www.asus.com/support/faq/1012780/)  
20. AI Suite III: How do I get rid of this god-awful thing? : r/ASUS \- Reddit, acessado em julho 29, 2025, [https://www.reddit.com/r/ASUS/comments/156bze3/ai\_suite\_iii\_how\_do\_i\_get\_rid\_of\_this\_godawful/](https://www.reddit.com/r/ASUS/comments/156bze3/ai_suite_iii_how_do_i_get_rid_of_this_godawful/)  
21. AI Suite 3 won't go away\! \- ASUS ROG forums, acessado em julho 29, 2025, [https://rog-forum.asus.com/t5/asus-software/ai-suite-3-won-t-go-away/td-p/854386](https://rog-forum.asus.com/t5/asus-software/ai-suite-3-won-t-go-away/td-p/854386)  
22. Asus Tuf B360M-Plus Gaming CPU Support \- Softwareg.com.au, acessado em julho 29, 2025, [https://softwareg.com.au/blogs/computer-hardware/asus-tuf-b360m-plus-gaming-cpu-support](https://softwareg.com.au/blogs/computer-hardware/asus-tuf-b360m-plus-gaming-cpu-support)  
23. TUF B360M-PLUS GAMING \- Support \- ASUS, acessado em julho 29, 2025, [https://www.asus.com/me-en/supportonly/tuf%20b360m-plus%20gaming/helpdesk\_bios/](https://www.asus.com/me-en/supportonly/tuf%20b360m-plus%20gaming/helpdesk_bios/)  
24. What happens if VRM is damaged?Motherboard VRM Damage Symptoms \- YouTube, acessado em julho 29, 2025, [https://www.youtube.com/watch?v=dJhTy7CP8\_c](https://www.youtube.com/watch?v=dJhTy7CP8_c)  
25. Worried about VRM. Running 8700k on B360m-e TUF . : r/intel \- Reddit, acessado em julho 29, 2025, [https://www.reddit.com/r/intel/comments/lufe3w/worried\_about\_vrm\_running\_8700k\_on\_b360me\_tuf/](https://www.reddit.com/r/intel/comments/lufe3w/worried_about_vrm_running_8700k_on_b360me_tuf/)  
26. Manual \- TUF B360M-PLUS GAMING/BR Id: 10178, acessado em julho 29, 2025, [https://www.asus.com/br/motherboards-components/motherboards/tuf-gaming/tuf-b360m-plus-gaming-br/helpdesk\_manual?model2Name=TUF-B360M-PLUS-GAMING-BR](https://www.asus.com/br/motherboards-components/motherboards/tuf-gaming/tuf-b360m-plus-gaming-br/helpdesk_manual?model2Name=TUF-B360M-PLUS-GAMING-BR)  
27. TUF B360M-PLUS GAMING/BR \- Support \- ASUS, acessado em julho 29, 2025, [https://www.asus.com/sg/supportonly/tuf%20b360m-plus%20gamingbr/helpdesk\_knowledge/](https://www.asus.com/sg/supportonly/tuf%20b360m-plus%20gamingbr/helpdesk_knowledge/)  
28. Asus TUF B360M-PLUS GAMING S Micro ATX LGA1151 Motherboard \- PCPartPicker, acessado em julho 29, 2025, [https://pcpartpicker.com/product/tYcRsY/asus-tuf-b360m-plus-gaming-s-micro-atx-lga1151-motherboard-tuf-b360m-plus-gaming-s](https://pcpartpicker.com/product/tYcRsY/asus-tuf-b360m-plus-gaming-s-micro-atx-lga1151-motherboard-tuf-b360m-plus-gaming-s)  
29. How To Completely Uninstall AI Suite 3 On ASUS Motherboards \- YouTube, acessado em julho 29, 2025, [https://www.youtube.com/watch?v=TTQbJaBZHbM](https://www.youtube.com/watch?v=TTQbJaBZHbM)  
30. How To Completely Remove ASUS AI Suite 3 With AIsuite Cleaner \- YouTube, acessado em julho 29, 2025, [https://www.youtube.com/watch?v=DM4akP2shjE](https://www.youtube.com/watch?v=DM4akP2shjE)  
31. Uninstalling AI Suite 3 Completely \- Cleaner tool gone? : r/ASUS \- Reddit, acessado em julho 29, 2025, [https://www.reddit.com/r/ASUS/comments/t9m79l/uninstalling\_ai\_suite\_3\_completely\_cleaner\_tool/](https://www.reddit.com/r/ASUS/comments/t9m79l/uninstalling_ai_suite_3_completely_cleaner_tool/)  
32. AI Suite III complete removal : r/ASUS \- Reddit, acessado em julho 29, 2025, [https://www.reddit.com/r/ASUS/comments/kddyw5/ai\_suite\_iii\_complete\_removal/](https://www.reddit.com/r/ASUS/comments/kddyw5/ai_suite_iii_complete_removal/)  
33. DICA RÁPIDA: TESTANDO MOSFET NA PLACA MÃE \- YouTube, acessado em julho 29, 2025, [https://www.youtube.com/watch?v=Vm2HNY2TACY](https://www.youtube.com/watch?v=Vm2HNY2TACY)  
34. Testando Mosfet Direto Na Placa Mãe \- YouTube, acessado em julho 29, 2025, [https://www.youtube.com/watch?v=9n-gLlQkjkI](https://www.youtube.com/watch?v=9n-gLlQkjkI)  
35. Como testar Transistor MOSFET utilizando Multímetro Digital \- VeRSis Tecnologia, acessado em julho 29, 2025, [https://versis.com.br/como-testar-transistor-mosfet-utilizando-multimetro-digital/](https://versis.com.br/como-testar-transistor-mosfet-utilizando-multimetro-digital/)  
36. ASUS TUF B360M-PLUS GAMING RG \[DIAGRAMAS.COM.BR\] | PDF \- Scribd, acessado em julho 29, 2025, [https://www.scribd.com/document/829327074/ASUS-TUF-B360M-PLUS-GAMING-RG-DIAGRAMAS-COM-BR](https://www.scribd.com/document/829327074/ASUS-TUF-B360M-PLUS-GAMING-RG-DIAGRAMAS-COM-BR)