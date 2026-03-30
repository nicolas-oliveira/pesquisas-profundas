# Gemini: Análise de Causa Raiz de Falha Intermitente de Ethernet no Ubuntu 22.04 com NIC Realtek RTL8111/8168

## Resumo Executivo

Este relatório apresenta uma análise técnica detalhada de uma falha intermitente de conexão Ethernet em um laptop Dell G15 executando Ubuntu 22.04 com o kernel Linux 6.8.0-60. A causa raiz do problema foi identificada como uma falha de comunicação de hardware durante a sequência de inicialização do driver do kernel, especificamente uma condição de corrida exacerbada por uma implementação instável do recurso PCI Express Active State Power Management (ASPM) no controlador de interface de rede (NIC) Realtek RTL8111/8168/8411. A análise dos logs do kernel revelou que a tentativa inicial de carregar o módulo `r8169` falhou com um erro de I/O (`-EIO: PCI read failed`), indicando que o dispositivo de hardware estava em um estado de baixo consumo de energia não responsivo. A resolução do problema não foi um resultado direto da atualização de software realizada, mas sim do ciclo de energia completo (`reboot`) subsequente, que reiniciou o estado do hardware da NIC, permitindo que a função de sondagem (`probe`) do driver fosse executada com sucesso na inicialização seguinte. O aparecimento tardio e "mágico" da interface após a reinicialização é atribuído à temporização dos serviços de espaço de usuário, como o NetworkManager, que efetivamente reavaliou ou reconheceu o dispositivo após este ter tido tempo suficiente para se estabilizar. As recomendações proativas se concentram na desativação permanente do recurso ASPM defeituoso por meio de um parâmetro de kernel e na manutenção da higiene do sistema, removendo configurações de drivers conflitantes latentes.

## Seção 1: Estado do Sistema e Análise da Falha Inicial

Esta seção estabelece o ambiente de linha de base e realiza uma análise hierárquica da falha, começando pelo sistema operacional e avançando até a interface kernel-hardware onde a falha ocorreu.

### 1.1 Perfil do Sistema e Identificação do Hardware

- **Sistema Operacional:** O sistema em questão é um Ubuntu 22.04, utilizando o kernel Linux `6.8.0-60-generic`. Esta é uma versão de kernel moderna na qual recursos agressivos de economia de energia, incluindo políticas de PCIe ASPM, são habilitados por padrão.

- **Plataforma de Hardware:** O hardware é um laptop Dell G15. Este contexto é crítico, pois o firmware de laptops (BIOS/UEFI) frequentemente possui implementações de gerenciamento de energia únicas e, por vezes, problemáticas.1

- **Dispositivo Alvo:** A saída do comando `lspci` identifica o dispositivo como um `Realtek Semiconductor Co., Ltd RTL8111/8168/8411 PCI Express Gigabit Ethernet Controller (rev 15)`. Esta família específica de controladores tem um histórico longo e bem documentado de problemas relacionados a drivers no Linux, particularmente no que diz respeito ao driver incluído no kernel, `r8169`, em comparação com o driver fornecido pelo fabricante, `r8168`.4

### 1.2 Manifestação Inicial da Falha: Ausência de Interface de Rede

As saídas iniciais dos comandos `nmcli device status` e `ip link show` confirmam que a pilha de gerenciamento de rede do sistema operacional estava completamente inconsciente do dispositivo Ethernet, que deveria ser nomeado `enp3s0`. A interface simplesmente não existia do ponto de vista do sistema operacional.

A saída do comando `sudo lshw -C network` é uma evidência chave. Ela mostra o controlador Ethernet como "DISPONÍVEL", o que significa que o subsistema PCI do kernel enumerou com sucesso o dispositivo no barramento. No entanto, a ausência de um nome lógico (como `enp3s0`) e de detalhes de configuração indica que nenhum driver conseguiu se vincular ou "reivindicar" o dispositivo com sucesso. Isso isola a falha na fase de vinculação/inicialização do driver.

### 1.3 Diagnóstico em Nível de Kernel: A Falha na Sondagem do Driver

A tentativa manual de carregar o driver via `sudo modprobe r8169` representa a primeira interação direta com o módulo do kernel responsável pelo dispositivo. A saída subsequente do `dmesg` fornece a evidência conclusiva da falha inicial:

```
[117301.095883] r8169 0000:03:00.0: error -EIO: PCI read failed
[117301.095896] r8169: probe of 0000:03:00.0 failed with error -5
```

A análise dessas mensagens é direta: o código de erro `-5` corresponde a `EIO` (Erro de Entrada/Saída). O kernel está explicitamente declarando que o driver, durante sua rotina de inicialização (a função `probe`), foi incapaz de ler os registradores do hardware através do barramento PCI Express. Isso não é um bug de software no sentido tradicional, mas uma falha fundamental de comunicação entre a CPU e o dispositivo periférico. Indica que o dispositivo está presente no barramento, mas não está respondendo aos comandos.

### 1.4 Investigando Configurações Latentes: O Fator `r8168-dkms`

A verificação dos diretórios de configuração do `modprobe` (`/etc/modprobe.d/*`) revelou arquivos de configuração relacionados ao `r8168-dkms`. Esta é uma pista histórica crítica. O `r8168-dkms` é o pacote do driver fornecido pelo fabricante, frequentemente instalado para contornar problemas com o driver `r8169` nativo do kernel. Sua instalação normalmente coloca o `r8169` em uma lista negra para evitar conflitos.7

A presença desses arquivos sugere fortemente que o pacote `r8168-dkms` foi instalado neste sistema em algum momento. O comando `apt-get upgrade`, que instalou um novo kernel, pode ter causado a falha no processo de compilação do DKMS para o `r8168` — um problema comum quando os cabeçalhos do kernel não correspondem ou o código do driver é incompatível com a nova versão do kernel.12 Isso deixaria o sistema sem seu driver funcional anterior, forçando um fallback para o

`r8169` nativo do kernel e, assim, expondo a incompatibilidade de hardware subjacente que o `r8168` poderia estar mascarando. Embora não seja a causa direta do erro `-EIO`, este é um fator contribuinte significativo para o estado instável do sistema.

## Seção 2: O Caminho da Resolução e a Correlação Temporal

Esta seção analisa a sequência de eventos que levaram à resolução, focando no papel crítico da reinicialização do sistema e no subsequente aparecimento tardio da interface de rede.

### 2.1 O Efeito da Atualização do Sistema e do Ciclo de Energia

O usuário executou `sudo apt-get update && sudo apt-get upgrade` seguido por `sudo reboot`. Esta ação teve dois efeitos distintos e importantes:

1. **Mudança no Estado do Software:** O kernel e outros pacotes do sistema foram atualizados. Embora isso pudesse introduzir novos drivers ou correções, é improvável que tenha sido o mecanismo de resolução direto, dado que o problema persistiu imediatamente após a reinicialização.

2. **Mudança no Estado do Hardware:** Um `reboot` completo realiza um ciclo de energia nos componentes do sistema. Isso é fundamentalmente diferente de remover e reinserir um módulo do kernel (`rmmod`/`modprobe`). Um ciclo de energia força todos os periféricos, incluindo a NIC Realtek, a reiniciarem suas máquinas de estado internas, limparem seu status de gerenciamento de energia e retornarem a um estado padrão e totalmente ligado (estado de link PCIe `L0`).

### 2.2 Estado Pós-Reinicialização: Um Sucesso Atrasado

Imediatamente após a reinicialização, o comando `ip link show` ainda mostrava a ausência de `enp3s0`. Esta é uma observação crucial: a reinicialização por si só não foi uma condição suficiente para que a interface aparecesse *imediatamente* no momento da inicialização.

A interface então apareceu "magicamente após tempo de espera". Isso afasta a possibilidade de uma simples correção de configuração estática e aponta para um processo dinâmico ou dependente do tempo. A hipótese mais provável é que a sondagem inicial e rápida do dispositivo pelo kernel durante o boot ainda tenha sido muito rápida para que a NIC se estabilizasse, potencialmente levando a outra falha silenciosa na sondagem. No entanto, uma vez que o sistema atingiu o espaço de usuário e a tela de login, serviços como o **NetworkManager** iniciaram seu próprio trabalho. O NetworkManager verifica periodicamente por mudanças de hardware e tenta gerenciar novos dispositivos.15 Ele possui lógica de repetição embutida para conexões (

`connection.autoconnect-retries`).17 É altamente provável que uma verificação atrasada pelo NetworkManager, ou um evento

`udev` relacionado, tenha acionado uma reavaliação do dispositivo após ele ter se estabilizado completamente, levando a uma sondagem de driver bem-sucedida e ao subsequente aparecimento da interface `enp3s0`.

### 2.3 Identificando o Momento da Restauração

A conexão foi restaurada no momento em que o `ip link show` listou pela primeira vez a interface `enp3s0` com o estado `UP`. Isso foi seguido pelo `nmcli device` mostrando o dispositivo como "conectada". Isso confirma a seguinte sequência de eventos:

1. O driver do kernel (`r8169`) sonda e inicializa com sucesso o hardware.

2. O kernel registra uma nova interface de rede (`enp3s0`).

3. O `udev` recebe o evento do kernel e cria o nó do dispositivo.

4. O NetworkManager detecta o novo dispositivo não gerenciado.

5. O NetworkManager aplica um perfil de conexão adequado (provavelmente um perfil padrão para conexões cabeadas) e leva a interface a um estado totalmente conectado.

A tabela a seguir fornece uma correlação cronológica clara entre os comandos do usuário, as saídas do sistema e o estado subjacente do kernel/hardware.

| Passo Lógico | Comando Executado           | Saída Chave                   | Estado Observado do Sistema              | Estado Inferido do Kernel/Hardware                                               |
| ------------ | --------------------------- | ----------------------------- | ---------------------------------------- | -------------------------------------------------------------------------------- |
| T0           | `nmcli device status`       | Sem `enp3s0`                  | Camada de rede não ciente do dispositivo | NIC em estado de energia instável; Driver não vinculado                          |
| T1           | `sudo modprobe r8169`       | (Sem saída)                   | Sem alteração                            | Sondagem do `r8169` iniciada                                                     |
| T2           | `sudo dmesg \| grep r816`   | `probe failed with error -5`  | Sem alteração                            | Sondagem falha devido a `EIO` (falha de leitura PCI)                             |
| T3           | `sudo reboot`               | (Sistema reinicia)            | Sistema offline                          | **Evento Crítico: Reinicialização por Ciclo de Energia do Hardware**             |
| T4           | `ip link show` (pós-reboot) | Sem `enp3s0`                  | Dispositivo ainda não visível para o SO  | NIC reiniciada, mas a sondagem inicial pode ter falhado (condição de corrida)    |
| T5           | (Tempo de espera)           | N/A                           | N/A                                      | Hardware se estabiliza; Serviços de espaço de usuário (NM) iniciam a verificação |
| T6           | `ip link show`              | `enp3s0` aparece, estado `UP` | **Conexão Restaurada**                   | **Driver `r8169` sonda e se vincula com sucesso**                                |
| T7           | `nmcli device`              | `enp3s0` está "conectada"     | NetworkManager gerencia o dispositivo    | Interface totalmente configurada e operacional                                   |

## Seção 3: Análise Profunda da Causa Raiz: Falha na Sondagem do Driver e ASPM

Esta seção fornece a explicação técnica central, desconstruindo o erro do kernel e ligando-o diretamente aos problemas conhecidos com o PCIe Active State Power Management (ASPM) neste hardware específico.

### 3.1 Anatomia de uma Sondagem de Driver do Kernel Linux

O modelo de driver do Linux opera da seguinte forma: dispositivos em um barramento (como o PCIe) são descobertos pelo driver do barramento. O kernel então tenta encontrar um driver adequado para esse dispositivo com base em seus identificadores de fornecedor e produto. A função `probe` é o ponto de entrada para a inicialização do driver. Seu trabalho é verificar a presença do dispositivo, alocar recursos (memória, IRQs), inicializar o hardware e registrá-lo no subsistema apropriado (neste caso, a pilha de rede). Se a função `probe` retornar um erro, o kernel considera o driver incompatível ou o dispositivo defeituoso, e não vinculará o driver ao dispositivo. Foi precisamente isso que aconteceu, deixando o dispositivo "DISPONÍVEL" mas "NÃO REIVINDICADO".

### 3.2 Desconstruindo o Erro: `probe failed with error -5 (-EIO)`

Como estabelecido, `-5` é `-EIO`, um erro de I/O genérico, mas severo. A mensagem acompanhante `PCI read failed` é a causa específica. Isso significa que quando a função `probe` do driver `r8169` tentou uma operação de baixo nível — provavelmente lendo o endereço MAC do dispositivo ou registradores de configuração de seu espaço de I/O mapeado em memória — o barramento PCIe retornou um erro. O hardware simplesmente não estava respondendo.

### 3.3 O Principal Suspeito: Active State Power Management (ASPM)

O Active State Power Management (ASPM) é um recurso crítico de economia de energia para computadores modernos, especialmente laptops. Ele permite que um link PCIe seja colocado em estados de baixo consumo de energia (`L0s`, `L1`) quando ocioso.19 O estado

`L1` é um estado de sono mais profundo que oferece economias de energia significativas, mas requer uma latência maior para despertar.

Existe uma vasta evidência de instabilidade entre o driver `r8169` e as NICs Realtek relacionada ao ASPM. Este tem sido um problema recorrente por muitos anos e através de várias versões do kernel.6 Os sintomas variam de baixo desempenho e quedas de link até a indisponibilidade completa do dispositivo — exatamente como observado aqui.

A hipótese causal é que a NIC, sob o controle da política de gerenciamento de energia do SO e do BIOS do sistema, entrou em um estado ASPM (`L1`). Devido a um bug no firmware da NIC ou na implementação do BIOS da placa-mãe, ela falhou em despertar corretamente desse estado quando a função `probe` do driver tentou acessá-la. Essa falha na transição de volta para o estado ativo `L0` resultou na `PCI read failed` e no erro `-EIO`. O controle do ASPM é uma interação complexa entre o BIOS e o SO; o BIOS pode definir políticas, e o SO pode tentar sobrepô-las. Se o BIOS tiver uma implementação com bugs, o controle pelo SO pode expor esses bugs.24

### 3.4 Análise Comparativa: Por Que a Reinicialização Funcionou

A chave para o sucesso após a reinicialização foi o **estado do hardware**. O `reboot` funcionou como um "reset de hardware" para a unidade de gerenciamento de energia da NIC. A tabela a seguir contrasta as condições que levaram à falha com as condições que levaram ao sucesso, provando que o fator determinante foi o estado de energia do hardware, não uma mudança na configuração do software.

| Parâmetro                     | Estado Antes da Reinicialização     | Estado Após a Reinicialização    | Análise                                                                              |
| ----------------------------- | ----------------------------------- | -------------------------------- | ------------------------------------------------------------------------------------ |
| **Estado do Hardware da NIC** | Instável de Baixo Consumo (ASPM L1) | Limpo, Totalmente Ligado (L0)    | A reinicialização forçou um reset do estado do hardware, eliminando a falha do ASPM. |
| **Versão do Kernel**          | 6.8.0-60-generic                    | 6.8.0-60-generic                 | Sem alteração. O kernel em si não foi a variável.                                    |
| **Módulo do Driver**          | `r8169`                             | `r8169`                          | Sem alteração. O mesmo driver foi usado em ambas as tentativas.                      |
| **`modprobe r8169`**          | Falha                               | Sucesso (implicitamente no boot) | O sucesso depende do estado do hardware estar responsivo.                            |
| **Saída do `dmesg`**          | `-EIO: PCI read failed`             | (Sem erro)                       | A ausência do erro pós-reboot confirma que o caminho de I/O estava livre.            |
| **Resultado**                 | Sem interface `enp3s0`              | Interface `enp3s0` criada        | A sondagem bem-sucedida permite que o driver registre o dispositivo de rede.         |

## Seção 4: Síntese e Recomendações Proativas

Esta seção final resume a cadeia causal e fornece passos claros e acionáveis para prevenir a recorrência do problema.

### 4.1 A Cadeia Causal Resumida

1. **Condição Subjacente:** Uma implementação de ASPM com bugs no firmware da NIC Realtek e/ou no BIOS do sistema Dell cria um potencial de instabilidade com o gerenciamento de energia do kernel Linux.

2. **Gatilho:** A NIC entra em um estado de sono profundo do ASPM (`L1`).

3. **Falha:** A NIC falha em despertar corretamente quando acessada pela CPU.

4. **Sintoma:** A função `probe` do driver `r8169` tenta uma `PCI read`, que falha com um erro `EIO`.

5. **Resultado:** O driver falha na inicialização, e o kernel não cria a interface de rede `enp3s0`.

6. **Resolução:** Uma reinicialização do sistema (`reboot`) realiza um ciclo de energia na NIC, reiniciando seu estado de energia defeituoso e permitindo que a sondagem subsequente do driver seja bem-sucedida.

### 4.2 Recomendações para Mitigação Proativa

- **Recomendação 1 (Solução Primária): Desativar o ASPM via Parâmetro de Kernel.** Esta é a solução mais direta e confiável para prevenir a condição de gatilho.
  
  - **Ação:** Editar o arquivo de configuração do GRUB (`/etc/default/grub`).
  
  - **Detalhe:** Adicionar `pcie_aspm=off` à string `GRUB_CMDLINE_LINUX_DEFAULT`.
  
  - **Finalizar:** Executar `sudo update-grub` e reiniciar.
  
  - **Justificativa:** Este comando instrui o kernel Linux a evitar completamente o gerenciamento do PCIe ASPM, impedindo que a NIC entre no estado de baixo consumo instável que causa a falha na sondagem.20 Isso aborda a causa raiz na camada de interação SO-hardware.

- **Recomendação 2 (Higiene do Sistema): Remover Pacotes DKMS Conflitantes.**
  
  - **Ação:** Remover completamente o pacote `r8168-dkms` e seus arquivos de configuração residuais.
  
  - **Detalhe:** Executar `sudo apt-get purge r8168-dkms`. Inspecionar manualmente `/etc/modprobe.d/` em busca de arquivos remanescentes que possam colocar o `r8169` na lista negra e removê-los.8
  
  - **Justificativa:** Isso previne conflitos futuros entre os drivers nativos do kernel e os do fabricante, especialmente durante atualizações de kernel. Garante que o sistema dependa de um único driver consistente (`r8169`), simplificando futuras soluções de problemas.

- **Recomendação 3 (Melhores Práticas de Firmware): Atualizar o BIOS/UEFI do Sistema.**
  
  - **Ação:** Verificar o site de suporte da Dell para o modelo G15 e aplicar quaisquer atualizações de BIOS disponíveis.2
  
  - **Justificativa:** As atualizações de BIOS frequentemente contêm atualizações de microcódigo e correções para bugs de gerenciamento de energia e inicialização de hardware. Uma futura atualização de BIOS da Dell pode resolver a instabilidade subjacente do ASPM no nível do firmware, tornando potencialmente desnecessário o workaround `pcie_aspm=off`. Esta é a correção mais fundamental, embora dependa do fornecedor disponibilizá-la.

# Referências citadas:

1. Marco Civil da Internet completa dez anos — Agência Nacional de ..., acessado em agosto 20, 2025, [https://www.gov.br/anatel/pt-br/assuntos/noticias/marco-civil-da-internet-completa-dez-anos](https://www.gov.br/anatel/pt-br/assuntos/noticias/marco-civil-da-internet-completa-dez-anos)  
2. MARCO CIVIL DA INTERNET \- NEUTRALIDADE DE REDE E ..., acessado em agosto 20, 2025, [https://www.cidp.pt/revistas/rjlb/2019/1/2019\_01\_0079\_0098.pdf](https://www.cidp.pt/revistas/rjlb/2019/1/2019_01_0079_0098.pdf)  
3. MARCO CIVIL DA INTERNET E NEUTRALIDADE DA REDE: ASPECTOS JURÍDICOS E TECNOLÓGICOS | Revista Eletrônica do Curso de Direito da UFSM, acessado em agosto 20, 2025, [https://periodicos.ufsm.br/revistadireito/article/view/23288](https://periodicos.ufsm.br/revistadireito/article/view/23288)  
4. Princípios fundamentais do Marco Civil da Internet | Portal Fiocruz, acessado em agosto 20, 2025, [https://fiocruz.br/documento/2014/04/principios-fundamentais-do-marco-civil-da-internet](https://fiocruz.br/documento/2014/04/principios-fundamentais-do-marco-civil-da-internet)  
5. Anatel proíbe redução na velocidade de internet fixa por tempo indeterminado, acessado em agosto 20, 2025, [https://agenciabrasil.ebc.com.br/geral/noticia/2016-04/anatel-proibe-reducao-na-velocidade-de-internet-fixa-por-tempo-indeterminado](https://agenciabrasil.ebc.com.br/geral/noticia/2016-04/anatel-proibe-reducao-na-velocidade-de-internet-fixa-por-tempo-indeterminado)  
6. Limitação de largura de banda – Wikipédia, a enciclopédia livre, acessado em agosto 20, 2025, [https://pt.wikipedia.org/wiki/Limita%C3%A7%C3%A3o\_de\_largura\_de\_banda](https://pt.wikipedia.org/wiki/Limita%C3%A7%C3%A3o_de_largura_de_banda)  
7. O que é: Divisão de Banda (Bandwidth Throttling) • Napoleon, acessado em agosto 20, 2025, [https://napoleon.com.br/glossario/o-que-e-divisao-de-banda-bandwidth-throttling/](https://napoleon.com.br/glossario/o-que-e-divisao-de-banda-bandwidth-throttling/)  
8. Token bucket \- Wikipedia, acessado em agosto 20, 2025, [https://en.wikipedia.org/wiki/Token\_bucket](https://en.wikipedia.org/wiki/Token_bucket)  
9. Leaky bucket \- Wikipedia, acessado em agosto 20, 2025, [https://en.wikipedia.org/wiki/Leaky\_bucket](https://en.wikipedia.org/wiki/Leaky_bucket)  
10. Saiba o que muda nos direitos do consumidor de telecomunicações ..., acessado em agosto 20, 2025, [https://agenciabrasil.ebc.com.br/geral/noticia/2023-11/saiba-o-que-muda-nos-direitos-do-consumidor-de-telecomunicacoes](https://agenciabrasil.ebc.com.br/geral/noticia/2023-11/saiba-o-que-muda-nos-direitos-do-consumidor-de-telecomunicacoes)  
11. Anatel aprova regulamento de qualidade dos serviços de telecomunicações \- JM Online, acessado em agosto 20, 2025, [https://jmonline.com.br/geral/anatel-aprova-regulamento-de-qualidade-dos-servicos-de-telecomunicac-es-1.65376](https://jmonline.com.br/geral/anatel-aprova-regulamento-de-qualidade-dos-servicos-de-telecomunicac-es-1.65376)  
12. regulamento de qualidade dos serviços de telecomunicações (rqual) \- Portal Gov.br, acessado em agosto 20, 2025, [https://www.gov.br/anatel/pt-br/consumidor/participe-dos-debates/conselhos-de-usuarios/foruns/RQUALCDUST10112022.pdf](https://www.gov.br/anatel/pt-br/consumidor/participe-dos-debates/conselhos-de-usuarios/foruns/RQUALCDUST10112022.pdf)  
13. Resolução nº 717, de 23 de dezembro de 2019 \- Anatel, acessado em agosto 20, 2025, [https://informacoes.anatel.gov.br/legislacao/resolucoes/2019/1371-resolucao-717](https://informacoes.anatel.gov.br/legislacao/resolucoes/2019/1371-resolucao-717)  
14. Understanding Oversubscription – POTs and PANs, acessado em agosto 20, 2025, [https://potsandpansbyccg.com/2020/12/04/understanding-oversubscription/](https://potsandpansbyccg.com/2020/12/04/understanding-oversubscription/)  
15. Why Dedicated Bandwidth Beats Oversubscription: Our Solution \- Metanet Hosting, acessado em agosto 20, 2025, [https://metanethosting.com/blog/dedicated-bandwidth-vs-oversubscription/](https://metanethosting.com/blog/dedicated-bandwidth-vs-oversubscription/)  
16. Understanding Broadband Performance Factors: All Mbps Are Not Created Equal \- \- ctc technology & energy, acessado em agosto 20, 2025, [https://www.ctcnet.us/wp-content/uploads/2014/02/CTC-ConnectivityPerformanceFactorsBrief0213141.pdf](https://www.ctcnet.us/wp-content/uploads/2014/02/CTC-ConnectivityPerformanceFactorsBrief0213141.pdf)  
17. What is Statistical Multiplexing | IGI Global Scientific Publishing, acessado em agosto 20, 2025, [https://www.igi-global.com/dictionary/statistical-multiplexing/28237](https://www.igi-global.com/dictionary/statistical-multiplexing/28237)  
18. Multiplexing \- Wikipedia, acessado em agosto 20, 2025, [https://en.wikipedia.org/wiki/Multiplexing](https://en.wikipedia.org/wiki/Multiplexing)  
19. Statistical multiplexing – Knowledge and References \- Taylor & Francis, acessado em agosto 20, 2025, [https://taylorandfrancis.com/knowledge/Engineering\_and\_technology/Computer\_science/Statistical\_multiplexing/](https://taylorandfrancis.com/knowledge/Engineering_and_technology/Computer_science/Statistical_multiplexing/)  
20. Sobre o RQUAL — Agência Nacional de Telecomunicações \- Portal Gov.br, acessado em agosto 20, 2025, [https://www.gov.br/anatel/pt-br/dados/qualidade/qualidade-dos-servicos/sobre-o-rqual](https://www.gov.br/anatel/pt-br/dados/qualidade/qualidade-dos-servicos/sobre-o-rqual)  
21. O que é QoS (Quality of Service) em redes? | Fortinet, acessado em agosto 20, 2025, [https://www.fortinet.com/br/resources/cyberglossary/qos-quality-of-service](https://www.fortinet.com/br/resources/cyberglossary/qos-quality-of-service)  
22. Implementar o Quality of Service (QoS) no Microsoft Teams, acessado em agosto 20, 2025, [https://learn.microsoft.com/pt-br/microsoftteams/qos-in-teams](https://learn.microsoft.com/pt-br/microsoftteams/qos-in-teams)  
23. Differentiated services \- Wikipedia, acessado em agosto 20, 2025, [https://en.wikipedia.org/wiki/Differentiated\_services](https://en.wikipedia.org/wiki/Differentiated_services)  
24. RFC 3670 \- Information Model for Describing Network Device QoS Datapath Mechanisms, acessado em agosto 20, 2025, [https://datatracker.ietf.org/doc/rfc3670/](https://datatracker.ietf.org/doc/rfc3670/)  
25. Regulamento de Uso do Espectro de Radiofrequências \- ANATELlegis \- Datalegis, acessado em agosto 20, 2025, [https://anatellegis.datalegis.net/action/TematicaAction.php?acao=abrirVinculos\&cotematica=15725021\&cod\_menu=8306\&cod\_modulo=502](https://anatellegis.datalegis.net/action/TematicaAction.php?acao=abrirVinculos&cotematica=15725021&cod_menu=8306&cod_modulo=502)  
26. Resolução nº 671, de 3 de novembro de 2016 \- Anatel, acessado em agosto 20, 2025, [https://www.anatel.gov.br/legislacao/resolucoes/2016/911-resolucao-671](https://www.anatel.gov.br/legislacao/resolucoes/2016/911-resolucao-671)  
27. Radiação Restrita — Agência Nacional de Telecomunicações \- Portal Gov.br, acessado em agosto 20, 2025, [https://www.gov.br/anatel/pt-br/regulado/radiofrequencia/radiacao-restrita](https://www.gov.br/anatel/pt-br/regulado/radiofrequencia/radiacao-restrita)  
28. Beamforming | PDF | Wi-Fi | Roteador (informática) \- Scribd, acessado em agosto 20, 2025, [https://pt.scribd.com/document/553809427/Beamforming](https://pt.scribd.com/document/553809427/Beamforming)  
29. Beamforming \- Agecom Telecom, acessado em agosto 20, 2025, [https://agecomtelecom.com.br/beamforming-a-tecnologia/](https://agecomtelecom.com.br/beamforming-a-tecnologia/)  
30. UFSM e ALGcom fecham acordo para desenvolvimento de provas de conceito de novas soluções na área de antenas 5G, acessado em agosto 20, 2025, [https://www.ufsm.br/2022/08/22/ufsm-e-algcom-fecham-acordo-para-desenvolvimento-de-provas-de-conceito-de-novas-solucoes-na-area-de-antenas-5g](https://www.ufsm.br/2022/08/22/ufsm-e-algcom-fecham-acordo-para-desenvolvimento-de-provas-de-conceito-de-novas-solucoes-na-area-de-antenas-5g)  
31. BEAMFORMING POR MEIO DE CODEBOOK PARA ... \- BDM UnB, acessado em agosto 20, 2025, [https://bdm.unb.br/bitstream/10483/29061/1/2019\_LucasTahHsinScherrerMa\_tcc.pdf](https://bdm.unb.br/bitstream/10483/29061/1/2019_LucasTahHsinScherrerMa_tcc.pdf)  
32. Espectro — Agência Nacional de Telecomunicações \- Portal Gov.br, acessado em agosto 20, 2025, [https://www.gov.br/anatel/pt-br/regulado/espectro](https://www.gov.br/anatel/pt-br/regulado/espectro)  
33. Anatel aprova destinação de 120 MHz para 5G na faixa de 4,9 GHz ..., acessado em agosto 20, 2025, [https://teletime.com.br/26/10/2023/anatel-aprova-destinacao-de-120-mhz-para-5g-na-faixa-de-49-ghz/](https://teletime.com.br/26/10/2023/anatel-aprova-destinacao-de-120-mhz-para-5g-na-faixa-de-49-ghz/)  
34. Anatel destina 120 MHz para o 5G na faixa de 4,9 GHz \- Agência Gov \- EBC, acessado em agosto 20, 2025, [https://agenciagov.ebc.com.br/noticias/202310/anatel-destina-120-mhz-para-o-5g-na-faixa-de-4-9-ghz](https://agenciagov.ebc.com.br/noticias/202310/anatel-destina-120-mhz-para-o-5g-na-faixa-de-4-9-ghz)  
35. Anatel define se dará 1,2 GHz da faixa de 6 GHz para serviços não licenciados \- Abranet | Notícias, acessado em agosto 20, 2025, [https://www.abranet.org.br/publicacoes/noticias/3411](https://www.abranet.org.br/publicacoes/noticias/3411)  
36. Which WiFi channels should I use? \- Datto, acessado em agosto 20, 2025, [https://networkinghelp.datto.com/help/Content/kb/Networking/General%20Information/KB115005589863.htm](https://networkinghelp.datto.com/help/Content/kb/Networking/General%20Information/KB115005589863.htm)  
37. When to use 20 MHz vs 40 MHz vs 80 MHz Wi-Fi channel widths ..., acessado em agosto 20, 2025, [https://www.icstech.com.au/when-to-use-20-mhz-vs-40-mhz-vs-80-mhz-wi-fi-channel-widths/](https://www.icstech.com.au/when-to-use-20-mhz-vs-40-mhz-vs-80-mhz-wi-fi-channel-widths/)  
38. Best WiFi Channel Width For 5GHz (20, 40, 80, 160 MHz) \- Router Freak, acessado em agosto 20, 2025, [https://routerfreak.com/best-wifi-channel-width-for-5ghz/](https://routerfreak.com/best-wifi-channel-width-for-5ghz/)  
39. WiFi 5GHz: Existe algum motivo para não selecionar 20/40/80 MHz ..., acessado em agosto 20, 2025, [https://www.reddit.com/r/HomeNetworking/comments/k0qsff/wifi\_5ghz\_any\_reason\_not\_to\_select\_204080\_mhz\_vs/?tl=pt-br](https://www.reddit.com/r/HomeNetworking/comments/k0qsff/wifi_5ghz_any_reason_not_to_select_204080_mhz_vs/?tl=pt-br)  
40. Planeamento e Otimização de Redes 5G Evódia Eunice Monteiro ..., acessado em agosto 20, 2025, [https://repositorio.iscte-iul.pt/bitstream/10071/29827/1/master\_evodia\_monteiro\_medina.pdf](https://repositorio.iscte-iul.pt/bitstream/10071/29827/1/master_evodia_monteiro_medina.pdf)  
41. Frequências destinadas ao 5G têm novo ato e consulta sobre ..., acessado em agosto 20, 2025, [https://www.gov.br/anatel/pt-br/assuntos/noticias/frequencias-destinadas-ao-5g-tem-novo-ato-e-consulta-sobre-requisitos-tecnicos](https://www.gov.br/anatel/pt-br/assuntos/noticias/frequencias-destinadas-ao-5g-tem-novo-ato-e-consulta-sobre-requisitos-tecnicos)  
42. Ato nº 915, de 01 de fevereiro de 2024 \- Anatel, acessado em agosto 20, 2025, [https://informacoes.anatel.gov.br/legislacao/atos-de-requisitos-tecnicos-de-gestao-do-espectro/2024/1920-ato-915](https://informacoes.anatel.gov.br/legislacao/atos-de-requisitos-tecnicos-de-gestao-do-espectro/2024/1920-ato-915)  
43. 5G Frequências destinadas ao 5G têm novo ato e consulta sobre requisitos técnicos, acessado em agosto 20, 2025, [https://pedbrasil.org.br/5g-frequencias-destinadas-ao-5g-tem-novo-ato-e-consulta-sobre-requisitos-tecnicos/](https://pedbrasil.org.br/5g-frequencias-destinadas-ao-5g-tem-novo-ato-e-consulta-sobre-requisitos-tecnicos/)  
44. Anatel aprova requisitos técnicos para a faixa de 3,7-3,8 GHz \- Portal Gov.br, acessado em agosto 20, 2025, [https://www.gov.br/anatel/pt-br/assuntos/noticias/anatel-aprova-requisitos-tecnicos-para-a-faixa-de-3-7-3-8-ghz](https://www.gov.br/anatel/pt-br/assuntos/noticias/anatel-aprova-requisitos-tecnicos-para-a-faixa-de-3-7-3-8-ghz)  
45. PARECER SEI Nº 200/2023/MF Ementa: Consulta Pública ANATEL nº 72/2022, para definição de Procedimentos de Ensaios para Aval \- Portal Gov.br, acessado em agosto 20, 2025, [https://www.gov.br/fazenda/pt-br/composicao/orgaos/secretaria-de-reformas-economicas/manifestacoes-em-consultas-publicas-de-orgaos-reguladores/2023/agencia-nacional-de-telecomunicacoes-anatel/parecer-sei-no-200-2023](https://www.gov.br/fazenda/pt-br/composicao/orgaos/secretaria-de-reformas-economicas/manifestacoes-em-consultas-publicas-de-orgaos-reguladores/2023/agencia-nacional-de-telecomunicacoes-anatel/parecer-sei-no-200-2023)  
46. 5G-Based Transmission Power Control Mechanism in Fog ... \- MDPI, acessado em agosto 20, 2025, [https://www.mdpi.com/2071-1050/10/4/1258](https://www.mdpi.com/2071-1050/10/4/1258)  
47. what does transmit power means and what is the best? \- Ubiquiti Community, acessado em agosto 20, 2025, [https://community.ui.com/questions/what-does-transmit-power-means-and-what-is-the-best/cddd0320-92bf-4936-9e3d-8c4e7d44a6cb](https://community.ui.com/questions/what-does-transmit-power-means-and-what-is-the-best/cddd0320-92bf-4936-9e3d-8c4e7d44a6cb)  
48. Técnicas para melhorar a eficiência do sensoriamento colaborativo em redes 5G para áreas remotas \- Repositório Institucional da UnB, acessado em agosto 20, 2025, [https://repositorio.unb.br/handle/10482/39058](https://repositorio.unb.br/handle/10482/39058)  
49. 5G/NR \- Power Class \- ShareTechnote, acessado em agosto 20, 2025, [https://www.sharetechnote.com/html/5G/5G\_PowerClass.html](https://www.sharetechnote.com/html/5G/5G_PowerClass.html)  
50. Anatel abre caminho (e espectro) para a tecnologia WiFi 6 no Brasil \- TELETIME News, acessado em agosto 20, 2025, [https://teletime.com.br/05/05/2020/anatel-abre-caminho-e-espectro-para-a-tecnologia-wifi-6-no-brasil/](https://teletime.com.br/05/05/2020/anatel-abre-caminho-e-espectro-para-a-tecnologia-wifi-6-no-brasil/)  
51. Regulamentação de Wi-Fi: Conformidade com as Normas \- DT Network, acessado em agosto 20, 2025, [https://dtnetwork.com.br/regulamentacao-de-wi-fi-conformidade-com-as-norma/](https://dtnetwork.com.br/regulamentacao-de-wi-fi-conformidade-com-as-norma/)  
52. Anatel vai liberar faixa de WiFi para ser compartilhada por ..., acessado em agosto 20, 2025, [https://telesintese.com.br/anatel-vai-liberar-faixa-de-wifi-para-ser-compartilhada-por-operadoras-de-celular/](https://telesintese.com.br/anatel-vai-liberar-faixa-de-wifi-para-ser-compartilhada-por-operadoras-de-celular/)  
53. Anatel publica requisitos técnicos para equipamentos WiFi 6E, acessado em agosto 20, 2025, [https://telesintese.com.br/anatel-publica-requisitos-tecnicos-para-equipamentos-wifi-6e/](https://telesintese.com.br/anatel-publica-requisitos-tecnicos-para-equipamentos-wifi-6e/)  
54. Anatel revisa regras para a faixa de 6 GHz e PPPs criticam \- TELETIME News, acessado em agosto 20, 2025, [https://teletime.com.br/13/01/2025/ppps-criticam-nova-decisao-da-anatel-que-divide-faixa-de-6-ghz/](https://teletime.com.br/13/01/2025/ppps-criticam-nova-decisao-da-anatel-que-divide-faixa-de-6-ghz/)  
55. Como Evitar Overlapping em Redes Wi-Fi – UNITEELCOM, acessado em agosto 20, 2025, [https://uniteelcom.com.br/como-evitar-overlapping-em-redes-wifi/](https://uniteelcom.com.br/como-evitar-overlapping-em-redes-wifi/)  
56. WiFi Advisor Co-Channel vs Adjacent Channel Interfence | VIAVI Solutions Inc., acessado em agosto 20, 2025, [https://www.viavisolutions.com/en-uk/support/knowledge-base/faq/wifi-advisor-co-channel-vs-adjacent-channel-interfence](https://www.viavisolutions.com/en-uk/support/knowledge-base/faq/wifi-advisor-co-channel-vs-adjacent-channel-interfence)  
57. Co-Channel and Adjacent Channel Interference in Mobile ..., acessado em agosto 20, 2025, [https://www.geeksforgeeks.org/mobile-computing/co-channel-and-adjacent-channel-interference-in-mobile-computing/](https://www.geeksforgeeks.org/mobile-computing/co-channel-and-adjacent-channel-interference-in-mobile-computing/)  
58. CSMA/CA with and without RTS/CTS \- YouTube, acessado em agosto 20, 2025, [https://www.youtube.com/watch?v=My4VDzviiNg](https://www.youtube.com/watch?v=My4VDzviiNg)  
59. CSMA/CA activity avoiding interference. | Download Scientific Diagram, acessado em agosto 20, 2025, [https://www.researchgate.net/figure/CSMA-CA-activity-avoiding-interference\_fig1\_4312248](https://www.researchgate.net/figure/CSMA-CA-activity-avoiding-interference_fig1_4312248)
