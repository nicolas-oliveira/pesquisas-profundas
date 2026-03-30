# XrandR: Arquitetura, Funcionamento e Configuração Avançada do Sistema de Gerenciamento de Display do X11

## Introdução à Arquitetura do XrandR

O XrandR (X Resize and Rotate extension) representa uma das extensões mais fundamentais do sistema X Window, responsável pelo gerenciamento dinâmico de displays, resoluções e configurações de monitores[^1][^4]. Do ponto de vista da engenharia, o XrandR opera como uma ponte entre as aplicações cliente e o servidor X11, permitindo modificações em tempo real das propriedades da tela sem necessidade de reinicialização do servidor[^8][^9].

A extensão RandR foi desenvolvida em resposta às limitações do protocolo X11 original, que não antecipava a necessidade de alterações dinâmicas nas propriedades da tela[^10][^11]. Originalmente concebido na década de 1980, o X11 não previa cenários como laptops com monitores externos, displays rotativos ou mudanças de resolução durante a execução[^21].

### Arquitetura Interna e Pipeline de Renderização

O funcionamento do XrandR baseia-se em uma arquitetura modular complexa que interage diretamente com o servidor X11[^8]. O sistema opera através de várias estruturas de dados fundamentais:

**Estruturas Centrais do Sistema:**

- **Screen Resources**: Gerencia todos os recursos disponíveis para uma tela específica
- **CRTC (Cathode Ray Tube Controller)**: Controla a saída de vídeo e o timing de sincronização
- **Output**: Representa as conexões físicas (HDMI, VGA, DisplayPort)
- **Mode**: Define resoluções, taxas de atualização e parâmetros de temporização[^15][^31]

### Pipeline de Renderização e Re-rendering

Um dos aspectos mais críticos do XrandR é sua abordagem para o re-rendering da tela[^12]. Quando uma alteração é solicitada, o sistema segue um pipeline específico:

1. **Detecção de Mudança**: O XrandR monitora solicitações de alteração através do protocolo RandR
2. **Validação de Recursos**: Verifica se os recursos necessários (CRTC, modos, outputs) estão disponíveis
3. **Reconfiguração do Buffer**: Modifica o framebuffer principal para acomodar as novas configurações
4. **Sincronização**: Coordena com o driver gráfico para aplicar as mudanças
5. **Notificação**: Informa as aplicações cliente sobre as alterações através de eventos RandR[^16][^17]

O processo de re-rendering no X11 é fundamentalmente diferente dos sistemas modernos, pois todo o conteúdo da tela deve ser reprojetado quando há mudanças significativas na configuração do display[^12].

## Comparação com Wayland: Paradigmas de Renderização

A diferença arquitetural entre X11/XrandR e Wayland representa uma mudança fundamental na filosofia de gerenciamento de displays[^3][^14][^18].

### Tabela Comparativa: X11/XrandR vs Wayland

| Aspecto                         | X11/XrandR                                  | Wayland                                                  |
|:------------------------------- |:------------------------------------------- |:-------------------------------------------------------- |
| **Arquitetura de Renderização** | Server-side rendering centralizado[^12]     | Client-side rendering distribuído[^3]                    |
| **Gerenciamento de DPI**        | DPI global único, resampling posterior[^18] | DPI por monitor, pre-scaling[^18]                        |
| **Controle de Display**         | Ferramenta unificada (xrandr)[^4]           | Compositor-específico (kscreen-doctor, gnome-randr)[^29] |
| **Buffer de Imagem**            | Buffer único redimensionado[^18]            | Buffers individuais por monitor[^18]                     |
| **Compatibilidade**             | Ampla compatibilidade legacy[^9]            | Requer aplicações nativas Wayland[^14]                   |
| **Overhead de Rede**            | Protocolo X11 via rede[^17]                 | Protocolo local otimizado[^3]                            |

### Implicações Técnicas da Diferença Arquitetural

No X11, o servidor mantém controle centralizado sobre a renderização, criando um buffer único que é posteriormente redimensionado para diferentes displays[^18]. Isso pode resultar em perda de qualidade quando há monitores com DPIs diferentes. O Wayland, por outro lado, delega a responsabilidade de renderização para as aplicações, permitindo que cada monitor receba um buffer otimizado[^3][^18].

## Módulos e Componentes Técnicos do XrandR

### Estrutura Modular Detalhada

O XrandR é implementado através de vários módulos interconectados que trabalham em conjunto para fornecer funcionalidade completa de gerenciamento de display[^31]:

#### 1. Módulo de Detecção de Hardware

**Função**: Identifica outputs disponíveis e suas capacidades através do EDID (Extended Display Identification Data)[^13]

**Componentes**:

- Parser EDID para extrair informações do monitor
- Detector de conexão física
- Validador de compatibilidade de modos[^20]

#### 2. Módulo de Gerenciamento de Modos

**Estrutura XRRModeInfo**[^31]:

```c
typedef struct {
    RRMode id;
    unsigned int width, height;
    unsigned long dotClock;
    unsigned int hSyncStart, hSyncEnd, hTotal, hSkew;
    unsigned int vSyncStart, vSyncEnd, vTotal;
    char *name;
    unsigned int nameLength;
    unsigned long modeFlags;
} XRRModeInfo;
```

**Responsabilidades**:

- Calcular parâmetros de temporização
- Validar modos personalizados
- Gerenciar modelines e frequências de atualização[^24][^30]

#### 3. Módulo CRTC (Cathode Ray Tube Controller)

**Estrutura XRRCrtcInfo**[^15][^31]:

```c
typedef struct {
    Time timestamp;
    int x, y;
    unsigned int width, height;
    RRMode mode;
    Rotation rotation;
    int noutput;
    RROutput *outputs;
} XRRCrtcInfo;
```

**Funcionalidades**:

- Controla timing de sincronização
- Gerencia posicionamento espacial dos displays
- Coordena rotação e transformações geométricas[^28]

#### 4. Módulo de Transformação e Escala

O módulo de transformação é responsável pelas operações de escala e posicionamento avançado[^32]. Utiliza matrizes de transformação 3x3 para aplicar:

- Scaling (ampliação/redução)
- Translação (posicionamento)
- Rotação
- Reflexão[^24][^27]

## Comandos Avançados do XrandR

### xrandr --query: Análise Detalhada do Sistema

O comando `xrandr --query` (ou simplesmente `xrandr`) fornece informações completas sobre o estado atual do sistema de display[^2][^5]:

```bash
$ xrandr --query
Screen 0: minimum 320 x 200, current 1920 x 1080, maximum 8192 x 8192
HDMI-1 connected primary 1920x1080+0+0 (normal left inverted right x axis y axis) 531mm x 299mm
   1920x1080     59.93 +  60.00*   50.00    59.94
   1920x1080i    60.00    50.00    59.94
   1680x1050     59.88
```

**Interpretação dos Dados**:

- **Screen 0**: Identificador da tela virtual (conceito legacy do X11)[^2]
- **minimum/maximum**: Limites de resolução suportados pelo driver
- **current**: Resolução ativa atual do desktop virtual
- **connected/disconnected**: Status físico da conexão
- **primary**: Indica o monitor principal do sistema
- **1920x1080+0+0**: Resolução + posição no espaço virtual (x+y offset)[^28]
- **531mm x 299mm**: Dimensões físicas reportadas pelo EDID[^13]

### Configurações Avançadas de Escala e Posicionamento

#### --scale: Escalonamento Inteligente

O parâmetro `--scale` permite criar resoluções virtuais maiores que a capacidade física do monitor[^24][^32]:

```bash
# Criar uma resolução 4K falsa em um monitor 1080p
xrandr --output HDMI-1 --mode 1920x1080 --scale 2x2 --panning 3840x2160
```

**Análise Técnica**:

- `--scale 2x2`: Aplica fator de escala 2:1 em ambas as dimensões
- `--panning 3840x2160`: Define área de navegação virtual
- O resultado é uma área de trabalho 4K simulada em hardware 1080p[^23][^24]

**Implementação Interna**: O scaling é implementado através de transformações matriciais aplicadas ao pipeline de renderização. O X11 cria um buffer virtual maior e depois faz downsampling para a resolução física do monitor[^32].

#### --mode: Gerenciamento de Modos de Vídeo

```bash
# Aplicar modo específico
xrandr --output HDMI-1 --mode 1920x1080

# Criar modo personalizado
cvt 2560 1440 60
xrandr --newmode "2560x1440_60.00" 312.27 2560 2752 3024 3488 1440 1443 1448 1493 -hsync +vsync
xrandr --addmode HDMI-1 "2560x1440_60.00"
```

**Processo de Criação de Modos**:

1. **cvt**: Calcula parâmetros de temporização otimizados
2. **--newmode**: Registra o novo modo no sistema
3. **--addmode**: Associa o modo ao output específico[^25][^30]

#### --primary: Designação de Monitor Principal

```bash
xrandr --output HDMI-1 --primary
```

O conceito de monitor primário no X11 define:

- Local padrão para novos windows
- Posição da barra de tarefas principal
- Monitor de referência para aplicações full-screen[^29]

#### --pos: Posicionamento Espacial Preciso

```bash
# Alinhar bordas inferiores de dois monitores
xrandr --output HDMI-1 --pos 0x0 --output HDMI-2 --pos 1920x-312
```

**Cálculo de Posicionamento**:

- Monitor 1: 1920x1080 na posição (0,0)
- Monitor 2: 1920x1080 na posição (1920,-312)
- Offset negativo -312 = (1080-768) para alinhar bordas inferiores[^28]

### Configuração Multi-Monitor Avançada

```bash
# Configuração complexa: monitor 4K principal + monitor 1080p secundário escalado
xrandr --output DP-1 --mode 3840x2160 --pos 0x0 --primary \
       --output HDMI-1 --mode 1920x1080 --scale 1.5x1.5 --pos 3840x540
```

Esta configuração:

- Define monitor 4K como primário na posição (0,0)
- Escala monitor 1080p para 2880x1620 virtual
- Posiciona o segundo monitor alinhado verticalmente[^27]

## Limitações e Considerações Técnicas

### Problemas de Performance

O XrandR possui limitações inerentes relacionadas à arquitetura do X11:

- Re-rendering completo necessário para mudanças significativas[^16]
- Overhead de comunicação cliente-servidor[^17]
- Limitações de bandwidth para configurações de alta resolução[^12]

### Compatibilidade com Drivers

A funcionalidade do XrandR depende criticamente do suporte do driver gráfico:

- Drivers proprietários podem ter limitações específicas[^22]
- Nem todos os modos calculados são suportados pelo hardware[^20][^26]
- Problemas de sincronização podem ocorrer com múltiplos monitores[^19]

## Conclusão

O XrandR representa uma solução madura e robusta para gerenciamento de displays no ecossistema X11, oferecendo controle granular sobre configurações de monitor através de uma arquitetura modular bem estruturada[^4][^9]. Embora o Wayland apresente vantagens arquiteturais significativas para cenários modernos, o XrandR continua sendo fundamental para sistemas que dependem do X11, fornecendo funcionalidades avançadas como escalonamento virtual, posicionamento preciso e gerenciamento dinâmico de modos de vídeo[^3][^14].

A compreensão profunda dos módulos internos do XrandR - desde as estruturas de dados fundamentais até os algoritmos de transformação - é essencial para administradores de sistema e desenvolvedores que trabalham com configurações complexas de display em ambientes Linux[^13][^31]. As capacidades avançadas do comando xrandr, especialmente as funcionalidades de escala e posicionamento, oferecem soluções práticas para desafios modernos como simulação de resoluções 4K e configurações multi-monitor heterogêneas[^24][^27].

<div style="text-align: center">⁂</div>

[^1]: https://superuser.com/questions/1435053/how-to-use-xrandr-to-crop-part-of-my-monitor

[^2]: https://unix.stackexchange.com/questions/667482/understanding-the-output-of-xrandr-query

[^3]: https://www.cbtnuggets.com/blog/technology/devops/wayland-vs-xorg-wayland-replace-xorg

[^4]: https://xorg-team.pages.debian.net/xorg/howto/use-xrandr.html

[^5]: https://docs.oracle.com/cd/E88353_01/html/E37839/xrandr-1.html

[^6]: https://www.reddit.com/r/linux/comments/k0ihrs/xrandr_scaling_and_arranging/

[^7]: https://www.graphics-muse.org/wp/?p=336

[^8]: https://www.x.org/releases/current/doc/randrproto/randrproto.txt

[^9]: https://wiki.archlinux.org/title/Xrandr

[^10]: https://www.x.org/releases/X11R7.5/doc/randrproto/randrproto.txt

[^11]: https://keithp.com/~keithp/talks/randr/randr.pdf

[^12]: https://docs.oracle.com/javase/8/docs/technotes/guides/troubleshoot/java2dpipeline001.html

[^13]: https://wiki.gentoo.org/wiki/Xrandr

[^14]: https://wiki.archlinux.org/title/Wayland

[^15]: https://stackoverflow.com/questions/13706078/xrandr-related-c-programming

[^16]: https://web.cs.ucdavis.edu/~okreylos/ResDev/Vrui/X11ClusterRendering.html

[^17]: https://serverfault.com/questions/186805/remote-offscreen-rendering

[^18]: https://www.youtube.com/watch?v=YJrSIgtygyo

[^19]: https://community.nxp.com/t5/i-MX-Processors/how-to-switch-from-X11-to-Frame-buffer/m-p/880324

[^20]: https://askubuntu.com/questions/186288/how-to-detect-and-configure-an-output-with-xrandr

[^21]: https://www.usenix.org/conference/2001-usenix-annual-technical-conference/x-resize-and-rotate-extension-–randr

[^22]: https://docs.nvidia.com/jetson/archives/r36.2/DeveloperGuide/SD/WindowingSystems/XWindowSystem.html

[^23]: https://www.reddit.com/r/archlinux/comments/xuqav7/setting_a_fake_x11_resolution_kinda_like_dsr_or/

[^24]: https://www.joeldare.com/wiki/linux:fake_a_higher_resolution_in_linux

[^25]: https://bbs.archlinux.org/viewtopic.php?id=270274

[^26]: https://superuser.com/questions/1257094/cant-change-resolution-with-xrandr

[^27]: https://blog.summercat.com/configuring-mixed-dpi-monitors-with-xrandr.html

[^28]: https://superuser.com/questions/485120/how-do-i-align-the-bottom-edges-of-two-monitors-with-xrandr

[^29]: https://www.baeldung.com/linux/primary-monitor-x-wayland

[^30]: https://www.reddit.com/r/linux_gaming/comments/pnkykr/creating_a_custom_resolution/

[^31]: https://github.com/meehow/python-xrandr/blob/master/xrandr/core.py

[^32]: https://unix.stackexchange.com/questions/774967/can-someone-please-explain-how-xrandr-scale-works

[^33]: https://discuss.kde.org/t/wayland-vs-x-benchmarking-results-updated-july-30/19348

[^34]: https://wilfredwee.github.io/entry/how-to-xrandr/

[^35]: https://askubuntu.com/questions/1174798/xrandr-how-to-shift-output-that-is-lower-resolution-than-the-display-native-res

[^36]: https://www.reddit.com/r/linux/comments/r7d2gj/finding_the_ideal_monitor_rotation_with_xrandr/

[^37]: https://askubuntu.com/questions/1039129/randr-extension-missing

[^38]: https://stackoverflow.com/questions/12644001/how-to-add-the-missing-randr-extension

[^39]: https://www.reddit.com/r/linuxquestions/comments/nnv5cp/how_does_x11_get_hardware_acceleration/

[^40]: https://hero.handmade.network/forums/code-discussion/t/3351-xwindow_system_game_rendering_system

[^41]: https://stackoverflow.com/questions/42388721/x11-graphics-rendering-improvement

[^42]: https://askubuntu.com/questions/491770/ubuntu-14-04-fake-a-higher-resolution

[^43]: https://gitlab.gnome.org/GNOME/gtk/-/issues/2013

[^44]: https://github.com/raboof/xrandr/blob/master/xrandr.c

[^45]: https://github.com/haad/arandr

[^46]: https://github.com/raboof/mod_xrandr-3/blob/master/xrandr.c

[^47]: https://github.com/raboof/xrandr/blob/master/man/xrandr.man

[^48]: https://gist.github.com/hishamhm/3a077df976ce144ffad8b7222f59ca90
