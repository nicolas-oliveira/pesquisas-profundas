# **Relatório Técnico Avançado: Engenharia de Firmware e Implementação do CyanogenMod 11 no Jlinksz T1000 (MT6582) em Ambientes Linux**

## **1\. Análise Arquitetural e Perfil de Hardware**

A revitalização de dispositivos legados, especificamente tablets de marca genérica ("white-label") baseados na plataforma MediaTek, constitui um desafio significativo de engenharia de sistemas embarcados. Este relatório disseca a metodologia técnica para a implantação de um firmware customizado, o CyanogenMod 11 (baseado no Android 4.4.4 KitKat), no tablet Jlinksz T1000. A operação é conduzida estritamente através de um ambiente host Linux, exigindo uma compreensão profunda da pilha de drivers do kernel Linux, gerenciamento de dependências de bibliotecas legadas e manipulação direta de partições eMMC via protocolos proprietários de bootloader.  
O Jlinksz T1000 exemplifica a arquitetura predominante de tablets de baixo custo de meados da década de 2010\. A análise forense das propriedades do sistema revela discrepâncias críticas entre o hardware reportado e o hardware físico, uma prática comum em dispositivos OEM dessa categoria para inflar especificações de mercado.

### **1.1 Caracterização do SoC MediaTek MT6582**

O núcleo de processamento do T1000 é o System-on-Chip (SoC) MediaTek MT6582. Diferente de seus sucessores de 64 bits, o MT6582 é uma arquitetura estritamente de 32 bits, baseada no conjunto de instruções ARMv7-A. O processador integra quatro núcleos ARM Cortex-A7 operando a uma frequência nominal de 1.3 GHz.1  
A subsistema gráfico é gerido pela GPU ARM Mali-400 MP2. A relevância desta especificação para a engenharia de ROMs customizadas é crítica: a aceleração de hardware no Android requer bibliotecas proprietárias (blobs) específicas para a Mali-400 que sejam compatíveis com a versão do kernel Linux em execução. No caso do T1000, o kernel nativo é a versão 3.4.67.1 Qualquer tentativa de portar uma ROM deve garantir que as bibliotecas libEGL e libGLES do espaço de usuário (userspace) sejam binariamente compatíveis com os drivers de kernel da Mali presentes na imagem de boot.

### **1.2 Auditoria de Memória e Discrepâncias de Firmware**

Uma análise detalhada das propriedades do sistema via adb shell getprop revela inconsistências intencionais no firmware de fábrica. Embora o dispositivo possa reportar versões de Android mais recentes (como 5.1 ou 6.0) na interface do usuário, a API do SDK real é a 19 (Android 4.4.2 KitKat).1 Além disso, a capacidade de RAM reportada de aproximadamente 4GB é frequentemente uma modificação no build.prop ou no kernel para mascarar a capacidade física real, que geralmente é de 1GB em dispositivos MT6582 desta geração.  
Esta distinção é fundamental para a seleção da ROM customizada. Tentar instalar uma ROM baseada em Android 5.0 ou superior (Lollipop/Marshmallow) exigiria um kernel atualizado (3.10+), o que é inviável sem o código-fonte completo da MediaTek e dos periféricos específicos do Jlinksz (display, touch, sensores). Portanto, o CyanogenMod 11, sendo nativo do Android 4.4, representa o teto técnico estável para este hardware, alinhando-se perfeitamente com o kernel 3.4.67 pré-existente.3

### **1.3 Tabela de Particionamento e Mapa de Memória**

O armazenamento eMMC do dispositivo é organizado em uma estrutura de partições complexa, gerenciada pelos registros MBR (Master Boot Record) e EBR (Extended Boot Record). Diferente de dispositivos modernos que utilizam GPT (GUID Partition Table), o MT6582 depende de um arquivo de dispersão ("Scatter File") para instruir o BootROM sobre onde começar a leitura e escrita de cada bloco de dados.5  
A tabela abaixo ilustra a estrutura típica de particionamento esperada para o MT6582, que deve ser rigorosamente mapeada antes de qualquer operação de escrita para evitar a corrupção do setor de boot ou da partição de calibração de rádio (NVRAM).

| Índice da Partição | Nome Lógico | Função Técnica | Risco de Modificação |
| :---- | :---- | :---- | :---- |
| **PRELOADER** | Preloader | Primeiro estágio do Bootloader. Inicializa a DRAM e o USB DA. | **Crítico (Hard Brick)** |
| **MBR** | MBR | Tabela de Partição Principal. | Alto |
| **EBR1/EBR2** | EBR | Tabelas de Partição Estendida. | Alto |
| **UBOOT** | lk (Little Kernel) | Segundo estágio. Inicializa display e passa cmdline ao Kernel. | Médio |
| **BOOTIMG** | boot | Contém o Kernel Linux (zImage) e o Ramdisk inicial. | Médio (Bootloop) |
| **RECOVERY** | recovery | Ambiente de recuperação e manutenção. | Baixo |
| **ANDROID** | system | Sistema de arquivos do OS (ext4). | Baixo |
| **USRDATA** | data | Dados do usuário. | Baixo |
| **NVRAM** | nvram | Calibração de Rádio, IMEI, MAC Address. | **Crítico (Perda de IMEI)** |

A integridade do arquivo MT6582\_Android\_scatter.txt é o pilar de todo o procedimento. Um endereço físico incorreto neste arquivo pode levar o SP Flash Tool a sobrescrever a partição errada, resultando em danos irreversíveis ao software do dispositivo.7

## **2\. Engenharia do Ambiente de Desenvolvimento Linux**

A interação com o BootROM da MediaTek em sistemas Linux modernos (Ubuntu 22.04 LTS, 24.04 LTS, Debian 12\) apresenta desafios de compatibilidade significativos. As ferramentas proprietárias, como o SP Flash Tool v5, foram compiladas contra bibliotecas que foram depreciadas, enquanto os subsistemas de gerenciamento de dispositivos do Linux evoluíram para bloquear acessos diretos a portas seriais, criando conflitos com o protocolo de handshake da MediaTek.

### **2.1 Resolução de Dependências de Bibliotecas Legadas (libpng12)**

O SP Flash Tool v5 para Linux possui uma dependência hardcoded da biblioteca libpng12.so.0. As distribuições modernas do Linux migraram para libpng16, e o pacote libpng12 foi removido dos repositórios oficiais devido à obsolescência. A tentativa de executar o flasher resulta invariavelmente no erro: error while loading shared libraries: libpng12.so.0: cannot open shared object file.9  
A solução técnica não envolve a instalação forçada de pacotes .deb obsoletos, o que poderia quebrar a integridade do sistema de pacotes apt, mas sim a injeção da biblioteca binária correta no caminho de execução da ferramenta ou no sistema de bibliotecas local.  
A abordagem mais robusta para resolver este impasse no Ubuntu 22.04/24.04 envolve a compilação da biblioteca a partir do código-fonte ou a extração manual de um pacote arquivo e a criação de links simbólicos estratégicos.10

1. **Aquisição do Binário:** É necessário obter o binário libpng12.so.0.54.0 de uma fonte confiável, como os arquivos de pacotes do Ubuntu Xenial.  
2. **Instalação Manual:** O arquivo deve ser movido para /usr/local/lib/ para evitar conflitos com o gerenciador de pacotes do sistema.  
3. **Linkagem Simbólica:** O sistema deve ser instruído a reconhecer esta biblioteca. O comando sudo ln \-s /usr/local/lib/libpng12.so.0.54.0 /usr/lib/x86\_64-linux-gnu/libpng12.so.0 cria a ponte necessária para que o linker dinâmico (ld-linux) satisfaça a dependência do SP Flash Tool.12

### **2.2 Gerenciamento de Conflitos no Subsistema udev**

O kernel Linux utiliza o subsistema udev para gerenciar eventos de dispositivos. Quando o tablet MT6582 é conectado via USB em modo desligado (Preloader Mode), ele se apresenta momentaneamente como uma porta serial virtual (/dev/ttyACM0).  
O serviço ModemManager, padrão em distribuições como Ubuntu e Fedora, monitora agressivamente novas portas seriais para tentar configurar modems 3G/LTE. Este processo de "probing" envia comandos AT para a porta serial. No caso do MT6582, o envio desses caracteres interfere no handshake proprietário da MediaTek, fazendo com que a ferramenta de flash perca a sincronia e falhe com erros de timeout ou "BROM ERROR".13  
Para mitigar isso, é imperativo implementar regras udev que coloquem os IDs de vendedor (VID) da MediaTek em uma "lista negra" para o ModemManager, enquanto garantem permissões de leitura/escrita para o usuário.  
Implementação das Regras de Controle:  
Um arquivo de regras deve ser criado em /etc/udev/rules.d/99-mtk.rules contendo diretivas específicas. Os VIDs críticos para MediaTek são 0e8d (geral) e 6000 (alguns modos de debug).

Snippet de código

\# Regras para impedir a interferência do ModemManager  
ATTRS{idVendor}=="0e8d", ENV{ID\_MM\_DEVICE\_IGNORE}="1"  
ATTRS{idVendor}=="6000", ENV{ID\_MM\_DEVICE\_IGNORE}="1"

\# Regras para permissão de acesso ao usuário (grupo plugdev)  
SUBSYSTEM=="usb", ATTRS{idVendor}=="0e8d", MODE="0666", GROUP="plugdev"  
SUBSYSTEM=="usb", ATTRS{idVendor}=="6000", MODE="0666", GROUP="plugdev"  
KERNEL=="ttyACM\*", ATTRS{idVendor}=="0e8d", MODE="0666", GROUP="plugdev"

A aplicação dessas regras exige o recarregamento do daemon udev (sudo udevadm control \--reload-rules) e o re-trigger dos eventos (sudo udevadm trigger).15

### **2.3 Configuração do Ambiente Python para mtkclient**

Enquanto o SP Flash Tool é a ferramenta oficial, a comunidade de engenharia reversa desenvolveu o mtkclient, uma ferramenta baseada em Python que explora vulnerabilidades no BootROM (como o exploit "kamakiri") para permitir leitura e escrita sem autenticação (DA Auth Bypass). Esta ferramenta é superior para a fase de backup e geração de scatter file em ambientes Linux.17  
A instalação recomendada utiliza um ambiente virtual Python (venv) para isolar as dependências pyusb e pyserial, evitando conflitos com as bibliotecas do sistema. A interação de baixo nível com o barramento USB no Linux exige que o script Python tenha acesso direto aos dispositivos brutos, o que é facilitado pelas regras udev configuradas anteriormente.17

## **3\. Extração Forense de Firmware e Geração de Scatter**

A etapa mais crítica antes de qualquer modificação de escrita é a extração bit-a-bit do firmware existente. Isso serve a um duplo propósito: criar uma imagem de recuperação de desastres ("unbrick") e gerar o mapa de partições (scatter file) exato para a revisão específica da placa do T1000.

### **3.1 Mapeamento de Memória e o Scatter File**

O arquivo de dispersão (scatter file) descreve o layout da memória eMMC. Para o MT6582, ele define endereços lineares e físicos. Utilizar um scatter file genérico baixado da internet é uma prática de alto risco. Fabricantes de tablets genéricos frequentemente alteram o tamanho das partições USRDATA e ANDROID entre lotes de produção. Se o scatter file utilizado para flashar não corresponder à tabela de partição real gravada no MBR/EBR do chip eMMC, o resultado será um dispositivo "brickado" com tabelas de partição corrompidas.5

### **3.2 Protocolo de Extração via mtkclient**

O mtkclient oferece um método automatizado para extrair o firmware e gerar o scatter file simultaneamente, superando as limitações das ferramentas legadas que exigiam cálculos manuais de endereços hexadecimais.  
O procedimento técnico envolve colocar o dispositivo em modo BROM. No MT6582, isso é geralmente alcançado desligando o dispositivo e mantendo pressionado o botão de Volume \+ ou Volume \- enquanto se conecta o cabo USB. O mtkclient detecta a assinatura do dispositivo e injeta um payload (agente de download customizado) na SRAM do SoC, assumindo o controle antes da execução do Preloader original.17  
O comando de execução python mtk.py rl \<diretorio\_destino\> inicia uma leitura recursiva (rl \- read link). O software itera sobre a tabela de partições reportada pelo hardware, despejando cada partição (boot, recovery, system, nvram, etc.) em arquivos binários separados e gerando automaticamente o MT6582\_Android\_scatter.txt correspondente.17  
Esta abordagem mitiga o risco humano de calcular offsets de memória incorretamente, um problema comum no método antigo de "Readback" manual do SP Flash Tool.

## **4\. Arquitetura de Portabilidade de ROM e Custom Recovery**

A instalação do CyanogenMod 11 no Jlinksz T1000 não é uma instalação direta de um pacote padrão, mas sim um exercício de "portabilidade cruzada" (cross-porting). Como não existe uma build oficial do CyanogenMod para este tablet "white-label" específico, a engenharia da solução envolve adaptar uma ROM compilada para um dispositivo com especificações idênticas (mesmo SoC MT6582 e Kernel 3.4.67).

### **4.1 O Paradigma do Kernel 3.4.67**

A compatibilidade do kernel é o fator limitante absoluto. O kernel Linux interage diretamente com o hardware através de drivers. No ecossistema Android da MediaTek dessa era, os drivers para o controlador de display (LCD), digitalizador de toque (Touchscreen), sensores e câmeras são compilados estaticamente no kernel (zImage) ou carregados como módulos que dependem de símbolos específicos do kernel (Magic Version).  
Não é possível simplesmente utilizar o kernel que vem dentro do zip de uma ROM CyanogenMod genérica, pois esse kernel contém drivers para o display e touch do dispositivo "doador", não do T1000. O resultado invariável seria um "bootloop" ou uma tela preta/branca devido à falha na inicialização do driver de vídeo.  
A Solução de Transplante de Kernel:  
A metodologia correta exige a extração do kernel (zImage) da imagem de boot original (boot.img) do T1000 (obtida no backup) e sua inserção na imagem de boot da ROM CyanogenMod e do Custom Recovery.20

### **4.2 Compilação e Adaptação do Custom Recovery (TWRP/CWM)**

Para instalar zips não assinados (como o CM11), o recovery original (stock) deve ser substituído por um Custom Recovery, como o ClockworkMod (CWM) ou Team Win Recovery Project (TWRP).  
Dado que a compilação do TWRP a partir do código-fonte exigiria a árvore de dispositivos (device tree) que não está publicamente disponível para o T1000, a técnica de "porting" de binários é aplicada. Isso envolve tomar um TWRP compilado para outro dispositivo MT6582 com a mesma resolução de tela e arquitetura de partição, e substituir seu kernel pelo kernel original do T1000.  
Fluxo de Trabalho de Engenharia de Imagem (Linux):  
Utilizando ferramentas como mkbootimg e unpackbootimg no Linux:

1. **Desempacotamento:** As imagens recovery.img (Stock) e recovery\_port.img (TWRP doador) são descompactadas, separando o cabeçalho, o kernel e o ramdisk.  
2. **Substituição:** O arquivo zImage (kernel) do TWRP doador é descartado e substituído pelo zImage do Stock Recovery.  
3. **Ajuste de Fstab:** O arquivo recovery.fstab dentro do ramdisk do TWRP deve ser auditado para garantir que os pontos de montagem (/emmc@android, /emmc@usrdata) correspondam aos definidos no scatter file do T1000.22  
4. **Reempacotamento:** A nova imagem é remontada, combinando o kernel original com o ramdisk do TWRP modificado.

### **4.3 Estrutura do CyanogenMod 11 para MT6582**

O CyanogenMod 11 é um sistema operacional completo que substitui a partição /system e /boot. A versão para MT6582 deve ser especificamente construída para a arquitetura armeabi-v7a e kernel 3.4.x. Além do kernel, a adaptação da ROM pode exigir a substituição de módulos proprietários (.ko) em /system/lib/modules para garantir que o Wi-Fi e o Bluetooth funcionem, copiando-os da ROM original para a customizada.23

## **5\. Protocolo de Execução de Flash via Linux**

Com os artefatos preparados (Scatter File, Recovery Portado, ZIP da ROM e GApps), o processo de gravação é iniciado. A utilização do SP Flash Tool no Linux é preferível para esta etapa devido à sua capacidade de lidar com o protocolo de download da MediaTek de forma nativa e estável.

### **5.1 O Processo de Formatação e Riscos Associados**

O termo "formatação" no contexto do SP Flash Tool refere-se à inicialização da tabela de partições. A opção "Format All \+ Download" é extremamente destrutiva e deve ser evitada a menos que a tabela de partições esteja corrompida. Esta opção apaga a partição NVRAM, que contém dados de calibração de fábrica insubstituíveis, incluindo os números IMEI e endereços MAC. A perda desta partição resulta em um dispositivo incapaz de conectar a redes celulares.25  
Para a instalação do CM11, o modo "Download Only" é suficiente e seguro, pois apenas sobrescreve as partições selecionadas (Recovery, System) sem destruir os dados de calibração.

### **5.2 Procedimento de Gravação (Flashing)**

O procedimento no Linux segue uma sequência rigorosa para garantir a sincronização com o Preloader:

1. **Carregamento do Scatter:** O SP Flash Tool carrega o mapa de memória a partir do arquivo gerado pelo mtkclient. Isso configura os endereços de início para cada partição.  
2. **Seleção de Componentes:** Apenas a partição RECOVERY deve ser marcada inicialmente para a gravação da imagem do TWRP modificado. A partição PRELOADER deve ser desmarcada para evitar riscos de bricking, pois a versão instalada já é funcional.  
3. **Ciclo de Handshake:** Ao clicar em "Download", a ferramenta entra em modo de espera. A conexão física do dispositivo (desligado) à porta USB dispara o evento udev. Se as regras estiverem corretas, o kernel Linux atribui permissões, o ModemManager ignora o dispositivo, e o SP Flash Tool detecta o ID do vendedor, iniciando a transferência do DA (Download Agent) para a RAM do tablet, seguido pela gravação da imagem na memória NAND/eMMC.26

### **5.3 Instalação do Sistema Operacional (Sideload)**

Com o Custom Recovery instalado, o tablet é reinicializado em modo de recuperação (Power \+ Volume Up). O ambiente Linux facilita a transferência dos pacotes de instalação (cm11.zip e gapps.zip) através do protocolo ADB Sideload.  
O comando adb sideload \<arquivo.zip\> estabelece uma ponte de dados entre o PC e o recovery, enviando o arquivo para um buffer temporário no dispositivo, onde o script de instalação (updater-script) é executado. Este script formata as partições /system, /cache e /data (Wipe) e descompacta a nova árvore de arquivos do sistema operacional.23

## **6\. Análise Pós-Instalação e Troubleshooting**

A primeira inicialização do CyanogenMod 11 é crítica. O sistema reconstrói o cache Dalvik (pré-compilação JIT de aplicativos), o que pode levar vários minutos.

### **6.1 Diagnóstico de Bootloop**

Se o dispositivo falhar em inicializar (ficar preso na animação de boot), a ferramenta de diagnóstico primária é o adb logcat. No Linux, com o dispositivo conectado durante o bootloop, o comando fornece um fluxo em tempo real de logs do sistema. Erros como "SurfaceFlinger died" ou falhas de "eglSwapBuffers" confirmam incompatibilidade de drivers gráficos (bibliotecas Mali) entre a ROM e o Kernel, exigindo uma nova revisão do processo de portabilidade.29

### **6.2 Restauração de NVRAM**

Caso ocorra a perda de IMEI (geralmente indicada por "Invalid IMEI" na barra de status), a recuperação é realizada restaurando as imagens nvram.bin e nvdata.bin extraídas na fase de backup. O mtkclient no Linux permite a escrita direta destas partições específicas sem a necessidade de re-flashar o sistema inteiro, demonstrando a superioridade da abordagem modular em Linux.19

## **7\. Conclusão**

A implementação do CyanogenMod 11 no Jlinksz T1000 demonstra que, mesmo em hardware legado e genérico, a obsolescência programada pode ser mitigada através de engenharia de software avançada. A utilização do Linux como plataforma de host oferece um ambiente de controle superior, permitindo diagnósticos precisos via logs de kernel (dmesg) e manipulação direta de hardware que seria ofuscada em sistemas Windows.  
A chave para o sucesso desta operação não reside apenas na ferramenta de flash, mas na integridade da preparação dos arquivos: a geração precisa do scatter file, a preservação do kernel nativo através de transplante cirúrgico para as novas imagens, e a proteção rigorosa das partições de calibração não volátil. Este relatório estabelece um protocolo reprodutível para a extensão do ciclo de vida de dispositivos baseados no MT6582, transformando hardware obsoleto em plataformas funcionais para aplicações leves ou leitura digital.

#### **Referências citadas**

1. Tabela Mediatek tablet.txt  
2. KRSDK Debug For Rooting Device | PDF | Java (Programming Language) \- Scribd, acessado em dezembro 31, 2025, [https://www.scribd.com/doc/260576498/Krsdk-Debug-for-Rooting-Device](https://www.scribd.com/doc/260576498/Krsdk-Debug-for-Rooting-Device)  
3. rohantaneja/android\_device\_mediatek\_mt6582: Generic MediaTek MT6582 device configuration. \- GitHub, acessado em dezembro 31, 2025, [https://github.com/rohantaneja/android\_device\_mediatek\_mt6582](https://github.com/rohantaneja/android_device_mediatek_mt6582)  
4. los14mt6582/android\_device\_gionee\_p4: LineageOs 14.1 bootable tree for Gionee P4/MT6582 3.4.67 by rajdeep \- GitHub, acessado em dezembro 31, 2025, [https://github.com/los14mt6582/android\_device\_gionee\_p4](https://github.com/los14mt6582/android_device_gionee_p4)  
5. MT6582 Android Scatter | PDF | Operating System Technology \- Scribd, acessado em dezembro 31, 2025, [https://www.scribd.com/document/345657422/MT6582-Android-Scatter-txt](https://www.scribd.com/document/345657422/MT6582-Android-Scatter-txt)  
6. 1.6.1.2. Create New Scatter File to Locate the Bare Metal Application in the OCRAM \- Intel, acessado em dezembro 31, 2025, [https://www.intel.com/content/www/us/en/docs/programmable/683211/current/create-new-scatter-file-to-locate-the.html](https://www.intel.com/content/www/us/en/docs/programmable/683211/current/create-new-scatter-file-to-locate-the.html)  
7. How To Make a Scatter File MT6595, MT6582, MT6589, MT6592, MT6577, MT6589T, MT6572\! \- GizBeat, acessado em dezembro 31, 2025, [https://gizbeat.com/2917/how-to-make-a-scatter-file-mt6595-mt6582-mt6589-mt6592-mt6577-mt6589t-mt6572/](https://gizbeat.com/2917/how-to-make-a-scatter-file-mt6595-mt6582-mt6589-mt6592-mt6577-mt6589t-mt6572/)  
8. KisMth (Part 7: Creating Scatter File) \- Android Development Kit, acessado em dezembro 31, 2025, [https://androdevkit.wordpress.com/2018/05/15/kismth-part-7-creating-scatter-file/](https://androdevkit.wordpress.com/2018/05/15/kismth-part-7-creating-scatter-file/)  
9. Trying to use SP Flash Tool (Mediatek), latest version requires libpng12-0, which is not available \- Debian Mailing Lists, acessado em dezembro 31, 2025, [https://lists.debian.org/debian-user/2018/09/msg00587.html](https://lists.debian.org/debian-user/2018/09/msg00587.html)  
10. error while loading shared libraries: libpng12.so.0 \- Ask Ubuntu, acessado em dezembro 31, 2025, [https://askubuntu.com/questions/895897/error-while-loading-shared-libraries-libpng12-so-0](https://askubuntu.com/questions/895897/error-while-loading-shared-libraries-libpng12-so-0)  
11. Fix libpng12-0 Missing In Ubuntu 22.10, 22.04, 21.10 Or 20.04 \- Linux Uprising Blog, acessado em dezembro 31, 2025, [https://www.linuxuprising.com/2018/05/fix-libpng12-0-missing-in-ubuntu-1804.html](https://www.linuxuprising.com/2018/05/fix-libpng12-0-missing-in-ubuntu-1804.html)  
12. shared library \- installing libpng12.so on ubuntu 22.04, acessado em dezembro 31, 2025, [https://askubuntu.com/questions/1409363/installing-libpng12-so-on-ubuntu-22-04](https://askubuntu.com/questions/1409363/installing-libpng12-so-on-ubuntu-22-04)  
13. How to root MTK based mobile devices using a Linux PC? \- Android Enthusiasts, acessado em dezembro 31, 2025, [https://android.stackexchange.com/questions/119068/how-to-root-mtk-based-mobile-devices-using-a-linux-pc](https://android.stackexchange.com/questions/119068/how-to-root-mtk-based-mobile-devices-using-a-linux-pc)  
14. Flashing under linux (fighting modem manager resolved\!) / Installing, compiling & flashing questions / Proxmark3 community, acessado em dezembro 31, 2025, [https://www.proxmark.io/www.proxmark.org/forum/viewtopic.php%3Fid=1759.html](https://www.proxmark.io/www.proxmark.org/forum/viewtopic.php%3Fid=1759.html)  
15. gesangtome/SP\_Flash\_Tool\_Linux: MediaTek Smart Phone Download Tool \- GitHub, acessado em dezembro 31, 2025, [https://github.com/gesangtome/SP\_Flash\_Tool\_Linux](https://github.com/gesangtome/SP_Flash_Tool_Linux)  
16. How To Use SP FlashTools in Arch Linux, acessado em dezembro 31, 2025, [https://nullrndtx.github.io/2016/08/02/how-to-use-sp-flashtools-in-arch-linux.html](https://nullrndtx.github.io/2016/08/02/how-to-use-sp-flashtools-in-arch-linux.html)  
17. bkerler/mtkclient: MTK reverse engineering and flash tool \- GitHub, acessado em dezembro 31, 2025, [https://github.com/bkerler/mtkclient](https://github.com/bkerler/mtkclient)  
18. problem entering preloader mode on mt6580 : r/androidroot \- Reddit, acessado em dezembro 31, 2025, [https://www.reddit.com/r/androidroot/comments/1it9o6x/problem\_entering\_preloader\_mode\_on\_mt6580/](https://www.reddit.com/r/androidroot/comments/1it9o6x/problem_entering_preloader_mode_on_mt6580/)  
19. Guide to Create Backup & Restore \- GitHub Gist, acessado em dezembro 31, 2025, [https://gist.github.com/arnabmactavish/df42552e36d4fd0f42a83fb6a4d4bfc9](https://gist.github.com/arnabmactavish/df42552e36d4fd0f42a83fb6a4d4bfc9)  
20. \[Guide\] How to port ROMS for any mt65xx(mt6591,mt6592,mt6582 etc) device 100% Working 2017 \-PART 3 \- YouTube, acessado em dezembro 31, 2025, [https://www.youtube.com/watch?v=\_omTFoh1Y3c](https://www.youtube.com/watch?v=_omTFoh1Y3c)  
21. PORTING GUIDE : How To Port Different ROMs to Your MT65XX Devices(Simplest and Fastest)\[HINDI\] |2018 \- YouTube, acessado em dezembro 31, 2025, [https://www.youtube.com/watch?v=4zMVB0cRn7c](https://www.youtube.com/watch?v=4zMVB0cRn7c)  
22. How To Make Create TWRP MT6592 MT6582 MT6572 MT6589 MT6595 \- GizBeat, acessado em dezembro 31, 2025, [https://gizbeat.com/5786/how-to-make-create-twrp-mt6592-mt6582-mt6572-mt6589-mt6595/](https://gizbeat.com/5786/how-to-make-create-twrp-mt6592-mt6582-mt6572-mt6589-mt6595/)  
23. \[DEV\]\[ROM\] \[25.1\] CyanogenMod 11.0 (Android 4.4.4) \- MoDaCo, acessado em dezembro 31, 2025, [https://www.modaco.com/topic/368013-devrom-251-cyanogenmod-110-android-444/](https://www.modaco.com/topic/368013-devrom-251-cyanogenmod-110-android-444/)  
24. MTK Device ROM Porting Guide | PDF | Zip (File Format) \- Scribd, acessado em dezembro 31, 2025, [https://www.scribd.com/document/360750918/Port-MT6582-to-MT6582](https://www.scribd.com/document/360750918/Port-MT6582-to-MT6582)  
25. SP Flash Tool on GNU/Linux \- rigacci.org, acessado em dezembro 31, 2025, [https://www.rigacci.org/wiki/doku.php/doc/appunti/android/sp\_flash\_tool](https://www.rigacci.org/wiki/doku.php/doc/appunti/android/sp_flash_tool)  
26. How To Use SP Flash Tool (Full Guide) \- YouTube, acessado em dezembro 31, 2025, [https://www.youtube.com/watch?v=LzyYXAflmOk](https://www.youtube.com/watch?v=LzyYXAflmOk)  
27. \[Advanced\] Installing factory Android image w/ SP Flash \- \#16 by Gagan \- Guides, acessado em dezembro 31, 2025, [https://community.myteracube.com/t/advanced-installing-factory-android-image-w-sp-flash/1003/16](https://community.myteracube.com/t/advanced-installing-factory-android-image-w-sp-flash/1003/16)  
28. Samsung Galaxy Tab Pro CM11 rom install \- YouTube, acessado em dezembro 31, 2025, [https://www.youtube.com/watch?v=urB1ihcqkoI](https://www.youtube.com/watch?v=urB1ihcqkoI)  
29. MTK6592 MTK6582 How to make a scatter for China clones / MTK MediaTek devices \- YouTube, acessado em dezembro 31, 2025, [https://www.youtube.com/watch?v=YZFO5WIyES8](https://www.youtube.com/watch?v=YZFO5WIyES8)