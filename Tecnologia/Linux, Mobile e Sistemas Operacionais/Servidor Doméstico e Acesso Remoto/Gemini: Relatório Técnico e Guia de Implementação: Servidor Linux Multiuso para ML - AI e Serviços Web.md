# Gemini: Relatório Técnico e Guia de Implementação: Servidor Linux Multiuso para ML/AI e Serviços Web

## Seção 1: Análise Estratégica da Distribuição do Sistema Operacional

A seleção de um sistema operacional (SO) é a fundação sobre a qual toda a infraestrutura de servidor será construída. Para um ambiente multiuso que deve equilibrar estabilidade para serviços web e compatibilidade de ponta para cargas de trabalho de Machine Learning (ML), a escolha da distribuição Linux não é trivial. A análise a seguir compara três candidatos proeminentes — Ubuntu Server LTS, Debian Stable e Rocky Linux — com base nos requisitos técnicos do projeto, culminando em uma recomendação fundamentada.

### 1.1. Cenário de Decisão: Ubuntu Server vs. Debian vs. Rocky Linux

Cada distribuição possui uma filosofia e um conjunto de pontos fortes que a tornam mais ou menos adequada para diferentes casos de uso. A avaliação deve ponderar a estabilidade, o ecossistema de software, o suporte da comunidade e, crucialmente, a compatibilidade com o stack de aceleração de GPU AMD ROCm.

- **Ubuntu Server LTS (Long-Term Support)**: Esta distribuição, mantida pela Canonical, é amplamente reconhecida por sua facilidade de uso e um vasto repositório de pacotes de software, o que a torna uma escolha popular para desenvolvedores e novas implementações. Seu ciclo de lançamento previsível, com uma nova versão LTS a cada dois anos, oferece um equilíbrio entre software moderno e estabilidade de longo prazo. O suporte da comunidade é massivo, e a documentação é extensa. De forma mais crítica para este projeto, o Ubuntu é a plataforma primária para o desenvolvimento e suporte oficial do AMD ROCm, garantindo a melhor compatibilidade e a maior quantidade de guias de instalação disponíveis.5  
- **Debian Stable**: O projeto Debian é a base para muitas outras distribuições, incluindo o Ubuntu, e é reverenciado por sua estabilidade rochosa e compromisso com o software livre.3 Seu ciclo de lançamento "quando estiver pronto" garante que os pacotes sejam exaustivamente testados, resultando em um sistema extremamente confiável, ideal para servidores críticos. No entanto, isso implica que as versões dos pacotes podem ser mais antigas em comparação com o Ubuntu.3 Embora seja geralmente mais leve em recursos, a curva de aprendizado pode ser um pouco mais acentuada, e o suporte oficial do ROCm para Debian é mais recente e menos maduro do que para o Ubuntu.6  
- **Rocky Linux**: Como um clone binário-compatível do Red Hat Enterprise Linux (RHEL), o Rocky Linux é projetado para ambientes corporativos que exigem alta segurança (com SELinux por padrão), confiabilidade e um longo ciclo de vida alinhado ao RHEL.1 É uma escolha excelente para virtualização e sistemas que precisam de compatibilidade com o ecossistema RHEL. Contudo, seu repositório de software é menos extenso que o do ecossistema Debian/Ubuntu, e a configuração pode ser mais complexa para usuários não familiarizados com RHEL.1 O suporte ao ROCm existe, refletindo o suporte ao RHEL, mas a documentação e os recursos da comunidade são menos abundantes do que para o Ubuntu.5

Para visualizar esses trade-offs, a seguinte matriz de decisão foi elaborada.

| Critério                   | Ubuntu Server LTS                                                    | Debian Stable                                                   | Rocky Linux                                                   |
|:-------------------------- |:-------------------------------------------------------------------- |:--------------------------------------------------------------- |:------------------------------------------------------------- |
| Estabilidade               | Alta (LTS com atualizações de segurança por 10 anos) 4               | Excepcional (pacotes mais antigos e exaustivamente testados) 3  | Excepcional (compatível com RHEL, focado em empresa) 1        |
| Disponibilidade de Pacotes | Excelente (repositórios vastos, PPAs) 2                              | Muito Boa (repositórios vastos, mas com versões mais antigas) 3 | Boa (focado em pacotes enterprise, menor que Debian/Ubuntu) 2 |
| Suporte a ML/ROCm          | Excelente (plataforma de referência para AMD ROCm) 5                 | Bom (suporte oficial recente, mas menos maduro) 6               | Bom (suporte alinhado ao RHEL, menos documentação) 5          |
| Curva de Aprendizado       | Baixa (amigável para iniciantes, vasta documentação) 1               | Média (requer mais conhecimento de sistema) 3                   | Alta (voltado para administradores de sistemas RHEL) 1        |
| Suporte Comunitário        | Excelente (maior base de usuários e fóruns) 1                        | Muito Bom (comunidade forte e experiente) 8                     | Bom (crescendo, mas mais focado no nicho enterprise) 8        |
| Filosofia                  | Equilíbrio entre modernidade e estabilidade, foco no desenvolvedor 1 | Estabilidade máxima, aderência estrita ao software livre 3      | Estabilidade empresarial, segurança e compatibilidade RHEL 1  |

### 1.2. Veredito e Justificativa Técnica para a Escolha da Distribuição

Recomendação Final: **Ubuntu Server 22.04 LTS.** 

A decisão de selecionar o Ubuntu Server 22.04 LTS é estratégica e pragmática, ditada pelo requisito mais complexo e de maior risco do projeto: a configuração da aceleração de ML na GPU AMD Radeon RX 570 via ROCm.  

O fator decisivo é a compatibilidade com o ROCm. A documentação oficial da AMD e a vasta quantidade de tutoriais e discussões da comunidade demonstram um foco primário e um suporte muito mais maduro para o Ubuntu. 

A GPU em questão, uma Radeon RX 570 com arquitetura Polaris, já não é oficialmente suportada pelas versões recentes do ROCm, o que exigirá soluções alternativas e potencialmente frágeis. Tentar implementar essas soluções em uma distribuição com suporte de segunda linha (como Debian ou Rocky Linux) adicionaria uma camada desnecessária de complexidade e incerteza a um problema já desafiador. A escolha do Ubuntu maximiza a probabilidade de sucesso ao alinhar o ambiente do SO com a plataforma onde a maioria dos testes, desenvolvimento e soluções de contorno para o ROCm ocorrem.  

Embora a estabilidade do Debian e o foco empresarial do Rocky Linux sejam admiráveis, essas vantagens são ofuscadas pela necessidade prática de garantir que o componente mais crítico do stack de ML funcione. Adicionalmente, o ecossistema robusto do Ubuntu garante ampla disponibilidade de pacotes e suporte para os outros componentes do servidor, como Docker e Nginx, simplificando o restante da configuração.3

## Seção 2: Arquitetura de Armazenamento e Particionamento de Disco

Um esquema de particionamento bem planejado é fundamental para o desempenho, a resiliência e a manutenibilidade de um servidor. A solicitação do usuário para um particionamento manual em múltiplos discos não é apenas uma preferência, mas uma implementação de uma arquitetura de armazenamento inteligente que visa otimizar as operações de entrada/saída (I/O) e proteger o sistema contra falhas comuns.

### 2.1. Racional Técnico do Esquema de Particionamento Proposto

O esquema de particionamento solicitado distribui os diretórios do sistema de arquivos Linux em dois dispositivos de armazenamento físico com características de desempenho diferentes: um NVMe de alta velocidade e um SSD SATA. Esta abordagem oferece três benefícios principais:

1. **Isolamento de I/O (Input/Output)**: Ao alocar o diretório /var no SSD SATA, as operações de escrita intensiva, como a geração de logs por serviços web (Nginx), logs do sistema, operações de banco de dados e o armazenamento de camadas de imagens Docker, são fisicamente separadas do disco principal.10 Isso evita que essas atividades concorram por largura de banda de I/O com o sistema operacional (em  `/`) e os dados de trabalho do usuário (em `/home`), que residem no NVMe. O resultado é uma melhor responsividade geral do sistema, especialmente sob carga.  
2. **Resiliência e Prevenção de Falhas**: Uma causa comum de falha em servidores é o esgotamento do espaço em disco na partição raiz (`/`) devido ao crescimento descontrolado de arquivos de log. Ao isolar /var em sua própria partição em um disco separado, um cenário de "log-runaway" preencheria o SSD SATA, mas não afetaria o NVMe. Isso impede que a partição raiz fique sem espaço, uma condição que normalmente levaria a uma falha total do sistema, tornando-o incapaz de inicializar. Esta é uma prática de *hardening- de servidores estabelecida.
3. **Hierarquia de Desempenho**: A estratégia aloca os dados com base na necessidade de velocidade. Os componentes mais críticos para a latência, como os arquivos do sistema operacional em `/` (que afetam o tempo de boot e a velocidade de carregamento de programas) e os dados de trabalho do usuário em `/home` (código-fonte, datasets de ML), são colocados no dispositivo mais rápido, o NVMe. Dados variáveis e menos sensíveis à latência, como os logs em `/var`, são adequadamente servidos pelo SSD SATA, que ainda oferece um desempenho excelente em comparação com discos rígidos tradicionais.

#### 2.1.1 Decisão sobre a Partição de Swap:

A partição de swap (espaço de troca) será alocada no SSD SATA. Embora o NVMe seja tecnicamente mais rápido, o swapping é uma operação de "último recurso" que indica que o sistema está ficando sem RAM. Alocar a swap no SSD SATA libera o NVMe para suas tarefas primárias de alta prioridade. Além disso, essa abordagem ajuda a distribuir o desgaste de escrita (um fator na vida útil dos SSDs) entre os dois dispositivos, em vez de concentrá-lo apenas no drive principal.

### 2.2. Guia de Implementação do Particionamento Manual

Para executar esta arquitetura durante a instalação do Ubuntu Server, é necessário utilizar a opção de particionamento manual. A tabela a seguir serve como um plano de execução preciso.

| Dispositivo Físico | Ponto de Montagem | Tamanho  | Tipo de Partição | Sistema de Arquivos  | Justificativa                                                                       |
|:------------------ |:----------------- |:-------- |:---------------- |:-------------------- |:----------------------------------------------------------------------------------- |
| `/dev/nvme0n1p1`   | /boot/efi         | 512 MB   | Primária         | EFI System Partition | Partição de boot necessária para sistemas UEFI.                                     |
| `/dev/nvme0n1p2`   | / (raiz)          | 40 GB    | Primária         | Ext4                 | Sistema operacional, bibliotecas e binários. No drive mais rápido para performance. |
| `/dev/nvme0n1p3`   | /home             | \~960 GB | Primária         | Ext4                 | Dados de usuário, código-fonte, datasets. Máximo de espaço no drive rápido.         |
| `/dev/sda1`        | /var              | 200 GB   | Primária         | Ext4                 | Logs, imagens Docker, bancos de dados. Isolado para resiliência e I/O.              |
| `/dev/sda2`        | swap              | 8 GB     | Primária         | swap                 | Espaço de troca. No SSD secundário para liberar o NVMe e distribuir o desgaste.     |
| `/dev/sda3`        | (Não alocado)     | \~32 GB  | \-               | \-                   | Espaço livre no SSD SATA para flexibilidade futura.                                 |

Passo a Passo no Instalador do Ubuntu Server:

1. Na etapa "Configure storage", selecione a opção de particionamento manual ("Something else" ou "Manual partitioning"). 
2. O instalador exibirá os dois discos: `/dev/nvme0n1` (o NVMe de 1TB) e `/dev/sda` (o SSD SATA de 240GB).  
3. Particionamento do NVMe (`/dev/nvme0n1`):  
   - Selecione o dispositivo /dev/nvme0n1 e crie uma nova tabela de partição (se estiver vazio).  
   - Selecione o espaço livre e clique em "Add Partition" (ou "+").  
   - Crie a partição EFI: Tamanho 512MB, Use as EFI System Partition, Ponto de Montagem /boot/efi.  
   - Selecione o espaço livre restante e crie a partição raiz: Tamanho 40GB, Use as Ext4 journaling file system, Ponto de Montagem `/`.  
   - Selecione o espaço livre restante e crie a partição home: Use todo o espaço restante, Use as Ext4 journaling file system, Ponto de Montagem `/home`.  
4. Particionamento do SSD SATA (`/dev/sda`):  
   - Selecione o dispositivo `/dev/sda` e crie uma nova tabela de partição (se estiver vazio).  
   - Selecione o espaço livre e crie a partição /var: Tamanho 200GB, Use as Ext4 journaling file system, Ponto de Montagem /var.  
   - Selecione o espaço livre restante e crie a partição swap: Tamanho 8GB, Use as swap area.  
5. Após criar todas as partições, verifique se o "Device for boot loader installation" está definido para o disco principal, /dev/nvme0n1.  
6. Prossiga com a instalação. O instalador formatará e montará as partições conforme especificado.

## Seção 3: Preparação do Ambiente e Instalação do Sistema Base

Com a estratégia de SO e armazenamento definida, a próxima fase é a preparação do hardware e a execução da instalação do sistema. A configuração correta do BIOS/UEFI é um pré-requisito indispensável para garantir que o hardware seja corretamente reconhecido e que o instalador possa operar sem restrições.

### 3.1. Configuração de BIOS/UEFI e Preparação da Mídia de Instalação

Preparação da Mídia de Instalação:

1. Download da Imagem ISO: Obtenha a imagem de instalação oficial do Ubuntu Server 22.04 LTS a partir do site do Ubuntu.
2. Criação do USB Bootável: Utilize a ferramenta balenaEtcher, conforme solicitado, para gravar a imagem ISO em um pendrive USB. Esta ferramenta garante uma gravação correta, bit a bit, que é diferente de simplesmente copiar o arquivo para o drive.15

Configuração do BIOS/UEFI na Placa-Mãe ASUS TUF B360M-PLUS GAMING/BR:  

A configuração do firmware da placa-mãe é um passo crítico. As instruções a seguir são específicas para o modelo fornecido.

1. **Acesso ao BIOS**: Reinicie o computador e pressione repetidamente a tecla `<F2>` ou `<Del>` durante a inicialização para entrar na interface de configuração do BIOS.
2. **Modo Avançado**: Uma vez na interface do BIOS, pressione a tecla \<F7\> para alternar para o "Advanced Mode". Este modo expõe todas as opções de configuração necessárias.16  
3. **Desativar o Secure Boot**: O Secure Boot é um recurso de segurança que impede a inicialização de sistemas operacionais não assinados. Para instalar Linux e, posteriormente, carregar módulos de kernel de terceiros (como os do ROCm), ele deve ser desativado.  
   - Navegue até a aba Boot.  
   - Selecione o menu Secure Boot.  
   - Localize a opção OS Type. O padrão é "Windows UEFI mode". Altere esta configuração para "Other OS". Esta ação desativa efetivamente o Secure Boot.
   - Em algumas versões de firmware, a abordagem alternativa é navegar até Key Management dentro do menu Secure Boot e selecionar a opção Clear Secure Boot Keys. Isso também desativará o recurso, mudando seu status para "Disabled" ou "Setup".
4. **Verificar o Modo AHCI para SATA**: O modo AHCI (Advanced Host Controller Interface) é essencial para o desempenho ideal de SSDs, permitindo recursos como o Native Command Queuing (NCQ).  
   - Navegue até a aba Advanced.  
   - Procure por um submenu como PCH Storage Configuration ou similar.  
   - Verifique se a opção SATA Mode Selection está configurada como AHCI. Geralmente, este é o padrão em placas-mãe modernas, mas a verificação é uma etapa de devida diligência crucial.20  
5. **Definir a Ordem de Boot**: Para que o sistema inicie a partir da mídia de instalação.  
   - Navegue até a aba Boot.  
   - Vá para a seção Boot Option Priorities.  
   - Defina o pendrive USB (geralmente identificado pelo nome do fabricante) como Boot Option #1.  
6. **Salvar e Sair**: Pressione a tecla \<F10\>, confirme a gravação das alterações e saia do BIOS. O sistema será reiniciado e deverá iniciar a partir do pendrive USB.

### 3.2. Roteiro Detalhado de Instalação do Sistema Operacional

Com o BIOS/UEFI configurado e a mídia pronta, a instalação do Ubuntu Server pode começar.

1. **Inicialização e Seleção de Idioma/Teclado**: O sistema inicializará a partir do pendrive, apresentando o menu do instalador. Selecione o idioma e o layout de teclado desejados.  
2. **Configuração de Rede**: O instalador tentará configurar a rede via DHCP. É neste ponto que se deve configurar um endereço IP estático para o servidor. Selecione a interface de rede (ex: eth0), escolha a opção de configuração manual e insira os detalhes de rede:  
   - Endereço IP: 192.168.1.100 (ou outro IP livre na sua rede)  
   - Máscara de Sub-rede: Ex:` 255.255.255.0  `
   - Gateway: Ex: `192.168.1.1`
   - Servidores DNS: Ex: `8.8.8.8`, `1.1.1.1 ` 
3. **Configuração de Proxy e Mirror**: Deixe em branco, a menos que sua rede exija um proxy para acesso à internet.  : Nesta etapa crucial, selecione a opção de particionamento manual e execute o plano detalhado na Seção 2.2.  
4. **Criação de Usuário**: Configure o perfil do usuário principal.  
   - Nome: Seu nome  
   - Nome do servidor (hostname): multiserver (ou um nome descritivo)  
   - Nome de usuário: admin  
   - Senha: Escolha uma senha forte e segura.  
5. **Instalação de Software Adicional**: O instalador oferecerá a opção de instalar pacotes populares ("snaps").  
   - Marque a opção para instalar o OpenSSH server. Isso é essencial para permitir o acesso remoto ao servidor via SSH imediatamente após a conclusão da instalação.  
   - Marque a opção para instalar o Docker. Isso simplifica a configuração, pois o ambiente de containerização já estará disponível no primeiro boot.  
6. Conclusão e Reinicialização: A instalação prosseguirá, copiando os arquivos e configurando os pacotes. Ao final, remova o pendrive USB quando solicitado e reinicie o sistema.

## Seção 4: Configuração Pós-Instalação e Validação do Núcleo do Servidor

Após a instalação bem-sucedida do sistema operacional, a próxima fase concentra-se em solidificar a base do servidor. Isso inclui a atualização de pacotes, a verificação da integridade do hardware e, mais criticamente, a implementação de uma estratégia robusta para a configuração da GPU AMD, que representa o maior desafio técnico deste projeto.

### 4.1. Procedimentos de Atualização, Segurança Inicial e Validação de Hardware

1. Primeiro Acesso e Atualização do Sistema:  
   
   - Conecte-se ao servidor remotamente a partir de outra máquina na mesma rede usando o cliente SSH:  
     
     ```bash
     ssh admin@192.168.1.100
     ```
   
   - Imediatamente após o login, execute uma atualização completa do sistema para garantir que todos os pacotes estejam na versão mais recente e com os últimos patches de segurança:  
     
     ```bash
     sudo apt update && sudo apt upgrade \-y
     ```

2. Validação de Hardware e Sistema de Arquivos:  
   
   - Verifique se o sistema operacional reconhece corretamente os componentes de hardware:  
     
     - CPU: `lscpu` (deve listar 6 núcleos para o i5-9400F).  
     - RAM: `free -h` (deve mostrar aproximadamente 16GB de memória total).  
     - Armazenamento: `lsblk` e `df -h` (devem mostrar a estrutura de discos e partições conforme planejado na Seção 2.2, com os pontos de montagem corretos).  
   
   - Verificação do TRIM para SSDs: O comando TRIM é vital para manter o desempenho dos SSDs ao longo do tempo. Execute-o manualmente para garantir que está funcional:  
     
     ```bash
     sudo fstrim -av
     ```
     
     O comando deve reportar a quantidade de dados "trimados" em cada partição montada nos SSDs.

### 4.2. O Desafio da GPU: Estratégia e Implementação do AMD ROCm para a RX 570 (Polaris)

Esta é a seção mais crítica do guia. A GPU Radeon RX 570, baseada na arquitetura Polaris (`gfx803`), não é oficialmente suportada pelas versões modernas da plataforma de computação AMD ROCm. Uma abordagem de engenharia cuidadosa é necessária para contornar essa limitação sem comprometer a estabilidade do servidor.

#### 4.2.1. O Conflito Central: Suporte Oficial vs. Realidade do Hardware

A documentação oficial do ROCm para as versões 5.x e 6.x lista explicitamente as GPUs suportadas, que se concentram nas arquiteturas mais recentes como CDNA e RDNA.5 A arquitetura Polaris (GCN 4.0), da qual a RX 570 faz parte, foi preterida. A AMD removeu ativamente o suporte para essas GPUs "legacy" a partir de versões como a ROCm 4.5, tornando uma instalação direta com pacotes recentes impossível.23 Tentar instalar o ROCm moderno resultará em falhas, pois o software não contém os binários ou o código necessário para a `gfx803`.

#### 4.2.2. A Estratégia de Engenharia Superior: Isolamento via Containerização

Tentar forçar uma instalação nativa de uma versão antiga e não suportada do ROCm em um servidor de produção multiuso é uma abordagem de alto risco. Uma simples atualização do sistema (sudo apt upgrade), especialmente uma atualização do kernel Linux, tem uma alta probabilidade de quebrar os drivers da AMD e tornar todo o ambiente de ML inutilizável, exigindo uma manutenção complexa e demorada.

A solução de engenharia correta para este problema é o isolamento. O servidor tem dois papéis distintos: um host estável para serviços web e um ambiente de execução para ML. A melhor prática, alinhada com os princípios de MLOps (Machine Learning Operations), é encapsular o ambiente de ML, com suas dependências frágeis e específicas, dentro de um contêiner Docker.  

Esta abordagem oferece vantagens decisivas:

- **Estabilidade**: O sistema operacional host (Ubuntu Server 22.04) permanece limpo e estável. Ele pode ser atualizado e mantido com segurança, sem risco de impactar o stack de ML.  
- **Reprodutibilidade**: O ambiente de ML, contido no Docker, é definido por um Dockerfile ou uma imagem pré-construída. Ele é imutável e portátil, podendo ser recriado de forma idêntica a qualquer momento.  
- **Manutenibilidade**: Se o ambiente de ML quebrar, basta destruir e recriar o contêiner, sem afetar o sistema host ou os serviços web que estão rodando. A complexidade é contida, não espalhada por todo o sistema.

#### 4.2.3. Método Primário (Recomendado): ROCm via Docker Especializado

Esta é a abordagem recomendada para um ambiente funcional e de baixo risco.

1. **Pré-requisitos no Host**: O driver amdgpu do kernel Linux, que vem com o Ubuntu moderno, já fornece a interface de baixo nível necessária. O Docker já foi instalado durante a configuração do SO e configurado na seção seguinte.  

2. **Execução do Contêiner ROCm para Polaris**: Utilize uma imagem Docker mantida pela comunidade, que foi especificamente construída para suportar a arquitetura gfx803. Essas imagens contêm uma versão mais antiga e compatível do ROCm e bibliotecas pré-compiladas. A imagem firstbober/rocm-pytorch-gfx803-docker é um bom exemplo.27  
   
   ```bash
   # Comando para iniciar um contêiner interativo com acesso à GPU  
   docker run -it --device=/dev/kfd --device=/dev/dri --group-add video --group-add render --security-opt seccomp=unconfined --name ml-container firstbober/rocm-pytorch-gfx803-docker
   ```
   
   ### Análise do Comando:
   
   - `--device=/dev/kfd`: Mapeia o driver de computação do kernel (KFD) para dentro do contêiner. Essencial para ROCm.28  
   - `--device=/dev/dri`: Mapeia os dispositivos da Direct Rendering Interface, que representam a GPU física.28  
   - `--group-add video` e `--group-add render`: Concede ao usuário dentro do contêiner as permissões de grupo necessárias para acessar o hardware da GPU.7  
   - `--security-opt seccomp=unconfined`: Desativa o filtro de chamadas de sistema padrão do Docker. É recomendado para cargas de trabalho de HPC e ML para permitir o mapeamento de memória e melhorar o desempenho.28  
   - `--name ml-container`: Atribui um nome fácil de lembrar ao contêiner.  

3. Verificação da GPU no Contêiner:  
   
   - Após o comando acima, você estará em um shell dentro do contêiner. Para verificar se a GPU foi detectada corretamente *dentro deste ambiente isolado*, execute:  
     
     ```bash
     rocminfo
     ```
   
   - A saída deve listar um agente (Agent) com o nome gfx803, confirmando o sucesso. O comando clinfo também pode ser usado para verificar o suporte a OpenCL.

#### 4.2.4. Método Alternativo (Experimental e Não Recomendado)

Para fins de completude, uma instalação nativa é teoricamente possível, mas altamente desaconselhada para este caso de uso. O processo envolveria:

1. Localizar e baixar o instalador amdgpu-install de uma versão antiga do ROCm que ainda suportava Polaris, como a 4.5.2.24  
2. Forçar a instalação, potencialmente editando scripts e resolvendo dependências manualmente.31  
3. "Prender" as versões dos pacotes do ROCm e do kernel (sudo apt-mark hold \<package-name\>) para impedir que atualizações automáticas quebrem a instalação.  
   Esta abordagem cria um sistema frágil e difícil de manter, que vai contra os princípios de um servidor estável.

### 4.3. Orquestração de Serviços com Docker: Instalação e Configuração

O Docker foi selecionado para instalação durante a configuração do SO. O passo final é configurar as permissões para que o usuário admin possa gerenciar contêineres sem precisar usar sudo para cada comando.

1. Adicionar Usuário ao Grupo Docker:  
   
   ```bash
   # O grupo docker deve ter sido criado durante a instalação.  
   # Adicione o usuário 'admin' ao grupo.  
   sudo usermod \-aG docker admin
   ```

2. Aplicar as Alterações: As alterações de grupo só têm efeito em uma nova sessão de login. Saia da sessão SSH e conecte-se novamente.  
   
   ```bash
    exit  
    ssh admin@192.168.1.100
   ```

3. Verificação: Execute o comando a seguir sem sudo. Ele deve funcionar sem erros de permissão.  
   
   ```bash
   docker ps
   ```

# Seção 5: Implementação dos Stacks de Aplicação

Com o sistema base e a estratégia de containerização da GPU definidos, o foco agora se volta para a construção dos ambientes de aplicação específicos para Machine Learning e serviços web. O ambiente de ML será construído dentro do contêiner Docker, enquanto o servidor web será executado diretamente no host.

## 5.1. Construção do Ambiente de Machine Learning com Miniconda e TensorFlow-ROCm

Todas as ações nesta subseção devem ser executadas dentro do contêiner Docker (ml-container) criado anteriormente. Para acessar o shell do contêiner, use o comando: 

```bash
docker exec \-it ml-container
```

1. **Instalação do Miniconda**: Miniconda é um instalador mínimo para o gerenciador de pacotes Conda, ideal para criar ambientes Python isolados e gerenciar dependências complexas.  
   
   ```bash
   # Dentro do contêiner  
   # Baixar o instalador do Miniconda  
   wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86\_64.sh
   ```
   
   ```bash
   # Executar o script de instalação (aceite os padrões)  
    Miniconda3-latest-Linux-x86\_64.sh
   
   # Feche e reabra o shell do contêiner para que o PATH seja atualizado  
   exit  
   docker exec \-it ml-container
   ```

2. **Criação do Ambiente Conda**: Crie um ambiente dedicado para o TensorFlow para evitar conflitos de dependência.  
   
   ```bash
   # Criar um ambiente chamado 'tf' com Python 3.10  
   conda create \-n tf python=3.10
   
   # Ativar o novo ambiente  
   conda activate tf
   ```

3. **Instalação do TensorFlow-ROCm:** Este é um passo de precisão cirúrgica. A versão do pacote tensorflow-rocm deve ser estritamente compatível com a versão do ROCm presente na imagem Docker. Uma incompatibilidade resultará em erros de biblioteca (symbol not found) ou falha na inicialização da GPU.  
   As imagens Docker comunitárias para gfx803 geralmente utilizam versões mais antigas do ROCm, como 4.5.2 ou 5.2.x. A análise da documentação de compatibilidade revela as seguintes correspondências cruciais 33:  
   
   - Para ROCm 4.5.2, a versão compatível é tensorflow-rocm==2.7.0.  
   - Para ROCm 5.2.0, a versão compatível é tensorflow-rocm==2.10.0.520.

A tabela a seguir formaliza a "receita" de software a ser usada, eliminando ambiguidades e garantindo a compatibilidade.

| Componente          | Versão Selecionada      | Fonte/Repositório         | Justificativa de Compatibilidade                                                                             |
|:------------------- |:----------------------- |:------------------------- |:------------------------------------------------------------------------------------------------------------ |
| ROCm (no contêiner) | \~4.5.2 / 5.2.x         | Imagem Docker Comunitária | Versão antiga que mantém o suporte para a arquitetura Polaris gfx803.24                                      |
| Python              | 3.10                    | Conda Forge               | Versão estável e amplamente compatível com bibliotecas de ML e com as versões relevantes do TensorFlow.34    |
| tensorflow-rocm     | 2.7.0 (para ROCm 4.5.2) | PyPI                      | Pacote compilado especificamente contra as bibliotecas do ROCm 4.5.2, garantindo a compatibilidade de ABI.33 |

Com o ambiente `tf` ativado, execute o comando de instalação preciso:  

```bash
# Instalar a versão exata do tensorflow-rocm compatível com ROCm 4.5.2  
pip install tensorflow-rocm==2.7.0  
```

4. **Verificação da Aceleração por GPU**: Para confirmar que o TensorFlow está utilizando a GPU AMD, execute um pequeno script Python dentro do ambiente Conda.  
   
   ```bash
   # Dentro do contêiner, com o ambiente 'tf' ativado  
   python \-c "import tensorflow as tf; print(tf.config.list\_physical\_devices('GPU'))"
   ```
   
   A saída esperada deve ser uma lista contendo um dispositivo GPU, algo como: ``. Uma lista vazia indica que a GPU não foi reconhecida pelo TensorFlow.

### 5.2. Implantação do Servidor Web com Nginx e Hardening de Firewall (UFW)

Estas ações são realizadas no sistema operacional host, não dentro do contêiner, para expor os serviços web à rede.

1. **Instalação do Nginx**: Nginx é um servidor web de alto desempenho, comumente usado como proxy reverso, balanceador de carga e para servir conteúdo estático.  
   
   ```bash
   # Executar no terminal do host (Ubuntu Server)  
   sudo apt install nginx
   ```

2. **Configuração do Firewall (UFW \- Uncomplicated Firewall)**: É essencial proteger o servidor, permitindo apenas o tráfego necessário.  
   
   ```bash
   # Permitir conexões SSH existentes e futuras  
   sudo ufw allow 'OpenSSH'
   ```
   
   ```bash
   # Permitir tráfego HTTP (porta 80\) e HTTPS (porta 443\) para o Nginx  
   sudo ufw allow 'Nginx Full'
   ```
   
   ```bash
   # Ativar o firewall  
   sudo ufw enable  
   # O sistema pedirá confirmação. Digite 'y' e pressione Enter.
   ```
   
   ```bash
   # Verificar o status e as regras ativas  
   sudo ufw status
   ```
   
   Após esses passos, o servidor Nginx estará rodando e acessível através do endereço IP http://192.168.1.100 em um navegador na mesma rede, e o firewall estará bloqueando todas as outras portas não autorizadas.

## Seção 6: Análise de Desempenho e Recomendações Estratégicas de Upgrade

Um servidor é um sistema dinâmico, e a avaliação contínua de seu desempenho é crucial para identificar gargalos e planejar evoluções. Esta seção analisa a capacidade do hardware atual para as cargas de trabalho propostas e delineia um plano de upgrade estratégico para mitigar as limitações identificadas.

### 6.1. Avaliação de Desempenho e Identificação de Gargalos do Sistema Atual

- **Análise da CPU (Intel Core i5-9400F)**: Lançado no primeiro trimestre de 2019, este processador de 9ª geração possui 6 núcleos e 6 threads, com uma frequência base de 2.9 GHz e turbo de 4.1 GHz.35  
  - Para Serviços Web: O desempenho é mais do que adequado. Cargas de trabalho típicas de um servidor web como o Nginx são geralmente limitadas pela velocidade de I/O ou pela memória, não pela CPU. O bom desempenho single-thread do i5-9400F para sua época é suficiente para lidar com requisições e lógica de aplicação leve.37  
  - Para Machine Learning: A CPU representa um gargalo significativo. A ausência de Hyper-Threading (apenas 6 threads) limita o paralelismo em tarefas de pré-processamento de dados, que são intensivas em CPU. Em comparação com processadores modernos com SMT (Simultaneous Multi-Threading), como o AMD Ryzen 5 5600 (6 núcleos/12 threads), o desempenho multi-thread do i5-9400F é drasticamente inferior, chegando a ser mais de 100% mais lento em benchmarks sintéticos.39 Isso se traduzirá em tempos muito mais longos para preparar grandes datasets antes que eles possam ser enviados para a GPU.  
- **Análise da GPU (AMD Radeon RX 570 8GB)**: Esta placa, baseada na arquitetura Polaris de 2017, é o principal gargalo para qualquer tarefa séria de ML.22  
  - Desempenho em ML: Além dos complexos desafios de compatibilidade com o ROCm, seu poder de cômputo bruto (medido em TFLOPS) é muito baixo para os padrões de treinamento de modelos modernos. Embora os 8GB de VRAM sejam um ponto positivo, permitindo carregar modelos de tamanho moderado, a velocidade com que a GPU pode processar os dados será extremamente lenta.41 Ela é funcional para aprendizado e experimentação em pequena escala, mas inviável para treinamento produtivo.  
- **Análise da RAM (16GB DDR4 2666MHz)**: A capacidade de 16GB é um ponto de partida razoável, mas se tornará rapidamente um fator limitante. O treinamento de modelos de ML, especialmente com datasets maiores, e a execução de múltiplos serviços em contêineres Docker podem consumir essa quantidade de memória rapidamente. Quando a RAM se esgota, o sistema recorre à partição de swap no disco, o que causa uma degradação massiva de desempenho, pois o acesso ao disco é ordens de magnitude mais lento que o acesso à RAM.  
- **Análise do Armazenamento**: A arquitetura de armazenamento com um NVMe para o sistema e um SSD SATA para dados variáveis é excelente e não constitui um gargalo imediato. Ela está bem dimensionada para as tarefas propostas.

### 6.2. Proposta Detalhada de Upgrade de Hardware

O caminho de upgrade mais impactante e lógico é aquele que aborda o gargalo mais severo primeiro. Neste caso, a GPU é a limitação primária, e sua substituição desencadeia uma cascata de upgrades necessários para manter o equilíbrio do sistema.  

A mudança mais estratégica não é apenas um upgrade de hardware, mas uma mudança de ecossistema: migrar da plataforma AMD ROCm para a plataforma NVIDIA CUDA. CUDA é o padrão de fato na indústria de ML, com um suporte de software imensamente mais maduro, estável e universalmente compatível com todos os principais frameworks (TensorFlow, PyTorch). Esta mudança elimina completamente os desafios de compatibilidade enfrentados com a RX 570\.  

A tabela a seguir detalha um plano de upgrade faseado e justificado.

| Componente                   | Atual                    | Recomendado                                             | Análise de Custo-Benefício e Impacto no Desempenho                                                                                                                                                                                                                                                                                                                                                                                      |                                                                                                                                                                                |
|:---------------------------- |:------------------------ |:------------------------------------------------------- |:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| GPU                          | AMD RX 570 8GB           | NVIDIA RTX 3060 12GB                                    | Impacto: Transformacional. Salto quântico no desempenho de ML e eliminação total dos problemas de compatibilidade do ROCm. A RTX 3060 oferece 12GB de VRAM, ideal para modelos maiores, e uma largura de banda de memória superior (192-bit).42 Para LLMs e modelos que exigem ainda mais memória, a                                                                                                                                    | RTX 4060 Ti 16GB é uma alternativa superior, embora com uma largura de banda de memória menor (128-bit).44 A mudança para o ecossistema CUDA é o benefício mais significativo. |
| Fonte de Alimentação         | EVGA 500W 80+ White      | 750W 80+ Gold (ex: Corsair RM750e, SeaSonic FOCUS Gold) | Impacto: Essencial para a estabilidade. A fonte de 500W é insuficiente para uma GPU como a RTX 3060 (\~170W TDP) somada ao resto do sistema (\~100W), operando perigosamente perto de seu limite.45 Uma fonte de 750W 80+ Gold oferece uma margem de segurança ampla, maior eficiência energética (menos calor e menor custo de eletricidade) e energia mais limpa e estável para os componentes, prevenindo instabilidades e falhas.45 |                                                                                                                                                                                |
| RAM                          | 16GB DDR4 2666MHz        | 32GB DDR4 2666/3200MHz                                  | Impacto: Significativo. Dobrar a RAM para 32GB evitará o uso da partição de swap em cenários de treinamento de modelos mais complexos e permitirá a execução de mais serviços em paralelo sem degradação de desempenho. A placa-mãe B360M limita a velocidade a 2666MHz, mas comprar um kit de 3200MHz pode ser mais econômico e é à prova de futuro para um eventual upgrade de plataforma.                                            |                                                                                                                                                                                |
| CPU/Plataforma (Longo Prazo) | Intel i5-9400F (LGA1151) | AMD Ryzen 5 5600 (AM4) \+ Placa-mãe B550                | Impacto: Alto. Uma vez que a GPU e a RAM são atualizadas, a CPU se tornará o gargalo principal. A migração para uma plataforma AM4 com um Ryzen 5 5600 oferece o dobro de threads (12 vs 6\) e um desempenho multi-core mais de 100% superior.39 Isso aceleraria drasticamente o pré-processamento de dados, a compilação de software e a responsividade geral do sistema sob carga pesada.                                             |                                                                                                                                                                                |

## Conclusão: Sumário Executivo e Próximos Passos

Este relatório detalhou a configuração de um servidor Linux multiuso, abordando cada requisito com uma análise técnica aprofundada. A recomendação pelo Ubuntu Server 22.04 LTS foi feita pragmaticamente para maximizar a compatibilidade com o stack de ML. A arquitetura de armazenamento proposta, com partições distribuídas entre um NVMe e um SSD SATA, foi projetada para otimizar o desempenho de I/O e aumentar a resiliência do sistema.  

O desafio mais significativo — a utilização da GPU AMD Radeon RX 570 com ROCm — foi resolvido através de uma estratégia de engenharia robusta: a containerização. Ao isolar o ambiente de ML em um contêiner Docker especializado, garantimos a funcionalidade da GPU sem comprometer a estabilidade do sistema operacional host, que pode ser mantido e atualizado de forma independente. Esta abordagem não é apenas uma solução alternativa, mas uma implementação das melhores práticas de MLOps para criar ambientes reprodutíveis e de fácil manutenção.  

O servidor, conforme configurado, representa um ponto de partida funcional e bem arquitetado. No entanto, a análise de desempenho revela gargalos claros, principalmente na capacidade de computação da GPU RX 570 e na quantidade de RAM, que limitarão a escala e a complexidade dos projetos de Machine Learning.  

O plano de upgrade estratégico apresentado é o caminho para transformar este sistema de um *homelab- experimental em uma estação de trabalho de ML de médio porte. A recomendação prioritária é a substituição da GPU por uma NVIDIA RTX 3060 12GB (ou superior) e o upgrade da fonte de alimentação para uma unidade de 750W 80+ Gold. Esta mudança não apenas proporciona um aumento massivo de desempenho, mas também migra o sistema para o ecossistema CUDA, padrão da indústria, eliminando os desafios de compatibilidade e abrindo a porta para um desenvolvimento de ML mais eficiente e sem atritos. Subsequentemente, um upgrade de RAM para 32GB e, a longo prazo, uma migração de plataforma para um sistema baseado em AMD Ryzen, solidificarão o servidor como uma ferramenta poderosa e versátil para os próximos anos.

#### Referências citadas

1. Rocky Linux vs. Ubuntu: Differences Explained | Knowledge Base by phoenixNAP, acessado em julho 29, 2025, [https://phoenixnap.com/kb/rocky-linux-vs-ubuntu](https://phoenixnap.com/kb/rocky-linux-vs-ubuntu)  
2. Rocky Linux Vs Ubuntu: Breaking Down 14 Features \- RedSwitches, acessado em julho 29, 2025, [https://www.redswitches.com/blog/rocky-linux-vs-ubuntu/](https://www.redswitches.com/blog/rocky-linux-vs-ubuntu/)  
3. Debian vs Ubuntu: Which Is the Best Linux Distribution? \- LightNode VPS, acessado em julho 29, 2025, [https://go.lightnode.com/tech/debian-vs-ubuntu](https://go.lightnode.com/tech/debian-vs-ubuntu)  
4. What's better Debian stable or ubuntu LTS (for servers)?? : r/linuxquestions \- Reddit, acessado em julho 29, 2025, [https://www.reddit.com/r/linuxquestions/comments/16gsnc4/whats\_better\_debian\_stable\_or\_ubuntu\_lts\_for/](https://www.reddit.com/r/linuxquestions/comments/16gsnc4/whats_better_debian_stable_or_ubuntu_lts_for/)  
5. Compatibility matrix \- ROCm Documentation \- AMD, acessado em julho 29, 2025, [https://rocm.docs.amd.com/en/latest/compatibility/compatibility-matrix.html](https://rocm.docs.amd.com/en/latest/compatibility/compatibility-matrix.html)  
6. System requirements (Linux) \- ROCm Documentation, acessado em julho 29, 2025, [https://rocm.docs.amd.com/projects/install-on-linux/en/latest/reference/system-requirements.html](https://rocm.docs.amd.com/projects/install-on-linux/en/latest/reference/system-requirements.html)  
7. Install Radeon software for Linux with ROCm, acessado em julho 29, 2025, [https://rocm.docs.amd.com/projects/radeon/en/latest/docs/install/native\_linux/install-radeon.html](https://rocm.docs.amd.com/projects/radeon/en/latest/docs/install/native_linux/install-radeon.html)  
8. Debian vs Rocky Linux for my server : r/homelab \- Reddit, acessado em julho 29, 2025, [https://www.reddit.com/r/homelab/comments/1cscznh/debian\_vs\_rocky\_linux\_for\_my\_server/](https://www.reddit.com/r/homelab/comments/1cscznh/debian_vs_rocky_linux_for_my_server/)  
9. How to install AMD Radeon Driver on Ubuntu 22.04.3 LTS | by Tilak Mudgal \- Medium, acessado em julho 29, 2025, [https://medium.com/@tilak559/streamlining-amd-gpu-installation-on-ubuntu-e7a22cf63118](https://medium.com/@tilak559/streamlining-amd-gpu-installation-on-ubuntu-e7a22cf63118)  
10. Partitioning recommendations for installing Ubuntu 22.04 LTS on SSD/HDD, acessado em julho 29, 2025, [https://askubuntu.com/questions/1493701/partitioning-recommendations-for-installing-ubuntu-22-04-lts-on-ssd-hdd](https://askubuntu.com/questions/1493701/partitioning-recommendations-for-installing-ubuntu-22-04-lts-on-ssd-hdd)  
11. How To Install Ubuntu 22.04 LTS {Step By Step} With Screenshots | LinuxTeck, acessado em julho 29, 2025, [https://www.linuxteck.com/how-to-install-ubuntu-22-04-lts-step-by-step//](https://www.linuxteck.com/how-to-install-ubuntu-22-04-lts-step-by-step//)  
12. Install Ubuntu Server, acessado em julho 29, 2025, [https://ubuntu.com/tutorials/install-ubuntu-server](https://ubuntu.com/tutorials/install-ubuntu-server)  
13. How to use manual partitioning during installation? \- Ask Ubuntu, acessado em julho 29, 2025, [https://askubuntu.com/questions/343268/how-to-use-manual-partitioning-during-installation](https://askubuntu.com/questions/343268/how-to-use-manual-partitioning-during-installation)  
14. Basic installation \- Ubuntu Server documentation, acessado em julho 29, 2025, [https://documentation.ubuntu.com/server/tutorial/basic-installation/](https://documentation.ubuntu.com/server/tutorial/basic-installation/)  
15. Install Ubuntu Desktop, acessado em julho 29, 2025, [https://ubuntu.com/tutorials/install-ubuntu-desktop](https://ubuntu.com/tutorials/install-ubuntu-desktop)  
16. How to Enable/Disable Secure Boot | Official Support | ASUS USA, acessado em julho 29, 2025, [https://www.asus.com/us/support/faq/1050047/](https://www.asus.com/us/support/faq/1050047/)  
17. Disabiling ASUS Bios Secure Boot \- YouTube, acessado em julho 29, 2025, [https://www.youtube.com/watch?v=tnOHi0w77bU\&pp=0gcJCfwAo7VqN5tD](https://www.youtube.com/watch?v=tnOHi0w77bU&pp=0gcJCfwAo7VqN5tD)  
18. \[Motherboard\] How to enable or disable Secure Boot ? | Official Support | ASUS USA, acessado em julho 29, 2025, [https://www.asus.com/us/support/faq/1049829/](https://www.asus.com/us/support/faq/1049829/)  
19. How do I turn secure boot off in UEFI BIOS on an ASUS TUF Gaming board? \- Super User, acessado em julho 29, 2025, [https://superuser.com/questions/1734315/how-do-i-turn-secure-boot-off-in-uefi-bios-on-an-asus-tuf-gaming-board](https://superuser.com/questions/1734315/how-do-i-turn-secure-boot-off-in-uefi-bios-on-an-asus-tuf-gaming-board)  
20. TUF B360M-PLUS GAMING/BR \- ASUS, acessado em julho 29, 2025, [https://dlcdnets.asus.com/pub/ASUS/mb/LGA1151/TUF\_B360M-PLUS\_GAMING\_BR/E14070\_TUF\_B360M-PLUS\_GAMING\_BR\_UM\_web.pdf](https://dlcdnets.asus.com/pub/ASUS/mb/LGA1151/TUF_B360M-PLUS_GAMING_BR/E14070_TUF_B360M-PLUS_GAMING_BR_UM_web.pdf)  
21. ROCM Device Support Wishlist #4276 \- GitHub, acessado em julho 29, 2025, [https://github.com/ROCm/ROCm/discussions/4276](https://github.com/ROCm/ROCm/discussions/4276)  
22. ROCm \- Wikipedia, acessado em julho 29, 2025, [https://en.wikipedia.org/wiki/ROCm](https://en.wikipedia.org/wiki/ROCm)  
23. \[Issue\]: ROCm completely stopped working after version 6.14.0-202 on Linux. #4919, acessado em julho 29, 2025, [https://github.com/ROCm/ROCm/issues/4919](https://github.com/ROCm/ROCm/issues/4919)  
24. Installing ROCM in TW and Leap \- Hardware \- openSUSE Forums, acessado em julho 29, 2025, [https://forums.opensuse.org/t/installing-rocm-in-tw-and-leap/151618](https://forums.opensuse.org/t/installing-rocm-in-tw-and-leap/151618)  
25. Minimal requirements for ROCm support of AMD GPUs \- Reddit, acessado em julho 29, 2025, [https://www.reddit.com/r/ROCm/comments/1ahcf38/minimal\_requirements\_for\_rocm\_support\_of\_amd\_gpus/](https://www.reddit.com/r/ROCm/comments/1ahcf38/minimal_requirements_for_rocm_support_of_amd_gpus/)  
26. Unfortunately that support is via ROCm, which doesn't support the last three gen... | Hacker News, acessado em julho 29, 2025, [https://news.ycombinator.com/item?id=28200477](https://news.ycombinator.com/item?id=28200477)  
27. Firstbober/rocm-pytorch-gfx803-docker \- GitHub, acessado em julho 29, 2025, [https://github.com/Firstbober/rocm-pytorch-gfx803-docker](https://github.com/Firstbober/rocm-pytorch-gfx803-docker)  
28. Running ROCm Docker containers — ROCm installation (Linux), acessado em julho 29, 2025, [https://rocm.docs.amd.com/projects/install-on-linux/en/latest/how-to/docker.html](https://rocm.docs.amd.com/projects/install-on-linux/en/latest/how-to/docker.html)  
29. ROCm for "Old" AMD GPU \- Jingbo (Eric) Yang, acessado em julho 29, 2025, [https://jingboyang.github.io/rocm\_rx580\_pytorch.html](https://jingboyang.github.io/rocm_rx580_pytorch.html)  
30. ROCm 4.5 is the last release to support Vega 10 (Radeon Instinct MI25) \- Reddit, acessado em julho 29, 2025, [https://www.reddit.com/r/hardware/comments/qrni4d/rocm\_45\_is\_the\_last\_release\_to\_support\_vega\_10/](https://www.reddit.com/r/hardware/comments/qrni4d/rocm_45_is_the_last_release_to_support_vega_10/)  
31. How can I install AMD ROCm 5+ on Ubuntu 22.04?, acessado em julho 29, 2025, [https://askubuntu.com/questions/1429376/how-can-i-install-amd-rocm-5-on-ubuntu-22-04](https://askubuntu.com/questions/1429376/how-can-i-install-amd-rocm-5-on-ubuntu-22-04)  
32. AMD ROCm™ installation \- AMD GPUOpen, acessado em julho 29, 2025, [https://gpuopen.com/learn/amd-lab-notes/amd-lab-notes-rocm-installation-readme/](https://gpuopen.com/learn/amd-lab-notes/amd-lab-notes-rocm-installation-readme/)  
33. tensorflow-upstream/rocm\_docs/tensorflow-rocm-release.md at develop-upstream \- GitHub, acessado em julho 29, 2025, [https://github.com/ROCmSoftwarePlatform/tensorflow-upstream/blob/develop-upstream/rocm\_docs/tensorflow-rocm-release.md](https://github.com/ROCmSoftwarePlatform/tensorflow-upstream/blob/develop-upstream/rocm_docs/tensorflow-rocm-release.md)  
34. TensorFlow compatibility \- ROCm Documentation \- AMD, acessado em julho 29, 2025, [https://rocm.docs.amd.com/en/latest/compatibility/ml-compatibility/tensorflow-compatibility.html](https://rocm.docs.amd.com/en/latest/compatibility/ml-compatibility/tensorflow-compatibility.html)  
35. Intel Core i5-9400F Benchmarks \- Geekbench Browser, acessado em julho 29, 2025, [https://browser.geekbench.com/processors/intel-core-i5-9400f](https://browser.geekbench.com/processors/intel-core-i5-9400f)  
36. Intel® Core™ i5-9400F Processor (9M Cache, up to 4.10 GHz) \- Product Specifications, acessado em julho 29, 2025, [https://www.intel.com/content/www/us/en/products/sku/190883/intel-core-i59400f-processor-9m-cache-up-to-4-10-ghz/specifications.html](https://www.intel.com/content/www/us/en/products/sku/190883/intel-core-i59400f-processor-9m-cache-up-to-4-10-ghz/specifications.html)  
37. Intel Core i5-9400F @ 2.90GHz \- CPU Benchmarks, acessado em julho 29, 2025, [https://www.cpubenchmark.net/cpu.php?cpu=Intel+Core+i5-9400F+%40+2.90GHz\&id=3397](https://www.cpubenchmark.net/cpu.php?cpu=Intel+Core+i5-9400F+@+2.90GHz&id=3397)  
38. Intel i5-9400 vs i5-9400F \[cpubenchmark.net\] by PassMark Software \- CPU Benchmarks, acessado em julho 29, 2025, [https://www.cpubenchmark.net/compare/Intel-Core-i5-9400-vs-Intel-Core-i5-9400F/3414vs3397](https://www.cpubenchmark.net/compare/Intel-Core-i5-9400-vs-Intel-Core-i5-9400F/3414vs3397)  
39. Intel i5-9400F vs AMD Ryzen 5 5600 \[cpubenchmark.net\] by PassMark Software, acessado em julho 29, 2025, [https://www.cpubenchmark.net/compare/3397vs4811/Intel-i5-9400F-vs-AMD-Ryzen-5-5600](https://www.cpubenchmark.net/compare/3397vs4811/Intel-i5-9400F-vs-AMD-Ryzen-5-5600)  
40. The 8GB RX 570 is still pretty good\! \- YouTube, acessado em julho 29, 2025, [https://www.youtube.com/watch?v=fbxmtv8lLBk](https://www.youtube.com/watch?v=fbxmtv8lLBk)  
41. AMD Radeon RX 570 X2: Detailed Specifications and Benchmark Ratings \- CpuTronic.com, acessado em julho 29, 2025, [https://cputronic.com/gpu/amd-radeon-rx-570-x2](https://cputronic.com/gpu/amd-radeon-rx-570-x2)  
42. Help Me Decide: RTX 3060 12GB vs. RTX 4060 Ti 16GB for ML and Occasional Gaming : r/LocalLLaMA \- Reddit, acessado em julho 29, 2025, [https://www.reddit.com/r/LocalLLaMA/comments/1hx6utz/help\_me\_decide\_rtx\_3060\_12gb\_vs\_rtx\_4060\_ti\_16gb/](https://www.reddit.com/r/LocalLLaMA/comments/1hx6utz/help_me_decide_rtx_3060_12gb_vs_rtx_4060_ti_16gb/)  
43. Help Me Decide: RTX 3060 12GB vs. RTX 4060 Ti 16GB for ML and Occasional Gaming : r/StableDiffusion \- Reddit, acessado em julho 29, 2025, [https://www.reddit.com/r/StableDiffusion/comments/1hx6wg8/help\_me\_decide\_rtx\_3060\_12gb\_vs\_rtx\_4060\_ti\_16gb/](https://www.reddit.com/r/StableDiffusion/comments/1hx6wg8/help_me_decide_rtx_3060_12gb_vs_rtx_4060_ti_16gb/)  
44. Deep Dive into GeForce RTX 3060 Ti and RTX 4060 Ti: Performance and Value, acessado em julho 29, 2025, [https://blog.spheron.network/deep-dive-into-geforce-rtx-3060-ti-and-rtx-4060-ti-performance-and-value](https://blog.spheron.network/deep-dive-into-geforce-rtx-3060-ti-and-rtx-4060-ti-performance-and-value)  
45. Part List \- Intel Core i5-9400F, GeForce RTX 3060 Ti \- PCPartPicker Australia, acessado em julho 29, 2025, [https://au.pcpartpicker.com/list/4Tkcwz](https://au.pcpartpicker.com/list/4Tkcwz)