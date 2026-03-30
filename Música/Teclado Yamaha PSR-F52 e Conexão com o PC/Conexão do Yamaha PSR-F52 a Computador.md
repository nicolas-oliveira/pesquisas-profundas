<img src="https://r2cdn.perplexity.ai/pplx-full-logo-primary-dark%402x.png" style="height:64px;margin-right:32px"/>

# Conexão do Yamaha PSR-F52 a Computador

## 1. Análise da Saída

O Yamaha PSR-F52 possui uma saída **PHONES/OUTPUT** com conector **mini jack estéreo de 3,5mm** (também conhecido como P2). Esta saída possui duas funções principais:[^1][^2][^3]

- **Saída de fones de ouvido**: Para monitoramento pessoal
- **Saída de linha externa**: Para envio do sinal de áudio para equipamentos externos[^3][^4]

O sinal de saída possui **nível de linha** adequado para conexão com equipamentos de áudio, sendo estéreo e compatível com conectores TRS de 3,5mm.[^5][^6]

## 2. Requisitos do PC

### Windows:

**Desktop:**

- **Entrada de microfone**: Conector 3,5mm (rosa/pink) - nível de microfone (~-50dB)[^7][^8]
- **Entrada de linha**: Conector 3,5mm (azul/blue) - nível de linha[^8][^7]
- **Porta USB**: Para interfaces de áudio externas[^9]

**Notebook/Laptop:**

- **Entrada combinada**: Conector 3,5mm único para fone/microfone[^10][^8]
- **Porta USB**: Para interfaces de áudio externas


### Linux:

**Portas físicas similares ao Windows, mas com diferentes sistemas de áudio:**

- **ALSA (Advanced Linux Sound Architecture)**: Driver básico do kernel[^11][^12]
- **PulseAudio**: Servidor de áudio padrão na maioria das distribuições[^12][^13]
- **PipeWire**: Sistema moderno que substitui PulseAudio e JACK[^14][^15]
- **JACK**: Para aplicações profissionais de baixa latência[^16][^17]


## 3. Conexão Física

### Cenário 1: Conexão Direta via Entrada P2

**Equipamentos necessários:**

- Cabo TRS 3,5mm estéreo macho-macho[^18][^5]
- Adaptador de nível (recomendado para entrada de microfone)

**Windows:**

1. Conecte o cabo na saída PHONES/OUTPUT do teclado
2. Conecte na entrada de linha (azul) do PC desktop ou entrada combinada do laptop[^7]
3. Configure nas **Configurações de Som** do Windows[^19][^20]

**Linux:**

1. Mesmo procedimento físico
2. Sistema detecta automaticamente via ALSA[^11]
3. PulseAudio/PipeWire gerencia o roteamento[^12][^14]

### Cenário 2: Conexão via Interface de Áudio USB

**Equipamentos recomendados:**

- **Interface de áudio USB** (Focusrite Scarlett Solo, Behringer UCA202, Presonus AudioBox)[^21][^9]
- Cabo TRS 3,5mm para entrada da interface
- Cabo USB para conexão com PC

**Processo universal (Windows/Linux):**

1. Conecte a interface USB ao computador
2. **Windows**: Instale drivers ASIO dedicados ou ASIO4ALL[^22][^23][^24]
3. **Linux**: Sistema detecta automaticamente via ALSA[^25][^11]
4. Conecte o teclado à entrada de linha da interface
5. Configure a interface como dispositivo padrão[^26][^9]

## 4. Configuração de Software

### Windows:

**Configurações Básicas:**

1. **Configurações de Som**:
    - Vá em Configurações > Sistema > Som
    - Selecione dispositivo de entrada correto[^20][^19]
    - Configure como "Dispositivo Padrão"[^26]
2. **Drivers ASIO** (recomendado):
    - Instale driver nativo da interface ou ASIO4ALL[^24][^22]
    - Oferece menor latência e melhor performance[^23]

**FL Studio (Windows):**

1. Pressione **F10** ou vá em **Options > Audio Settings**[^27][^28]
2. Em **Device**, selecione:
    - **FL Studio ASIO** (multi-cliente)[^28]
    - **ASIO4ALL v2** (universal)[^22]
    - **Scarlett USB ASIO** (para interfaces Focusrite)[^29][^30]
3. Ajuste **Buffer Size** conforme necessário[^28][^22]
4. Teste com metrônomo[^30]

### Linux:

**Sistemas de Áudio:**

**PulseAudio (padrão na maioria das distros):**

- Configuração automática via GUI (pavucontrol)[^12]
- Compatível com aplicações desktop padrão[^13]

**PipeWire (moderno, recomendado):**

- Substitui PulseAudio e JACK simultaneamente[^15][^14]
- Baixa latência nativa para aplicações profissionais[^31][^14]
- Configuração através de **pwvucontrol** ou **pavucontrol**[^31]

**JACK (profissional):**

- Para gravação e produção musical[^17][^16]
- Configuração via **qjackctl**[^32][^25][^17]
- Baixa latência configurável[^16][^17]

**LMMS (Linux):**

1. Vá em **Edit > Settings**[^33][^34]
2. Em **Audio Settings**:
    - **ALSA**: Para uso direto[^34][^17]
    - **SDL**: Compatibilidade geral[^34]
    - **PulseAudio**: Integração desktop[^34]
    - **JACK**: Para baixa latência profissional[^35][^34]
3. Configure **Buffer Size** apropriado[^33]
4. **Nota**: LMMS versões recentes suportam entrada de áudio[^36]

### Configuração JACK (Linux Profissional):

**Instalação:**

```bash
# Ubuntu/Debian
sudo apt install jackd2 qjackctl

# Fedora
sudo dnf install jack-audio-connection-kit qjackctl

# Arch
sudo pacman -S jack2 qjackctl
```

**Configuração via qjackctl:**[^32][^25][^17]

1. **Setup > Interface**: Selecione interface (ex: hw:0)
2. **Sample Rate**: 44.1kHz ou 48kHz
3. **Frames/Period**: 128 ou 256 (menor = menos latência)
4. **Realtime**: Habilitado[^17][^32]
5. Iniciar servidor JACK antes de abrir DAW

### Configuração PipeWire (Linux Moderno):

**Para aplicações que requerem baixa latência:**

```bash
# Definir latência no ambiente
export PIPEWIRE_LATENCY=256/48000
```

**Para Reaper/DAWs via PipeWire:**[^31]

- Edite arquivo .desktop da aplicação
- Adicione variável de ambiente PIPEWIRE_LATENCY
- Reconecte dispositivos MIDI se necessário[^31]


## 5. DAW Específicas

### FL Studio:

- **Windows**: Drivers ASIO nativos ou ASIO4ALL[^27][^29][^22]
- **Configuração**: F10 > Device > Selecionar interface[^30][^28]
- **Teste**: Metrônomo para verificar saída de áudio[^30]


### LMMS:

- **Multiplataforma**: ALSA, PulseAudio, JACK, SDL[^33][^34]
- **Linux**: Preferir JACK para baixa latência[^35][^34]
- **Windows**: SDL como padrão[^34]
- **Entrada de áudio**: Disponível em versões nightlies recentes[^36]


### Configurações Otimizadas:

**Latência típica profissional:**

- **JACK**: 5-10ms[^16][^17]
- **ASIO**: 5-15ms[^23]
- **PipeWire**: 5-12ms[^14]

**Buffer sizes recomendados:**

- **Gravação/Monitoramento direto**: 128-256 samples
- **Mixagem**: 512-1024 samples
- **Renderização**: 2048+ samples

A escolha entre conexão direta e interface USB depende da qualidade desejada. Para uso profissional, **interfaces USB com drivers ASIO** (Windows) ou **configuração JACK/PipeWire** (Linux) oferecem resultados superiores em qualidade e latência.[^9][^14][^16]
<span style="display:none">[^37][^38][^39][^40][^41][^42][^43][^44][^45][^46][^47][^48][^49][^50][^51][^52][^53][^54]</span>

<div align="center">⁂</div>

[^1]: https://pt.yamaha.com/pt/products/musical_instruments/keyboards/portable_keyboards/psr-f52/specs.html

[^2]: https://uk.yamaha.com/en/musical-instruments/keyboards/products/portable-keyboards/psr-f52/specs.html

[^3]: https://br.yamaha.com/files/download/other_assets/2/1455812/VEJ1990_PSR-F52_owners_manual_Pt_D0.pdf

[^4]: https://ca.yamaha.com/files/download/other_assets/7/1405077/VEJ1990_PSR-F52_owners_manual_En_D0.pdf

[^5]: https://www.reddit.com/r/WeAreTheMusicMakers/comments/xwdih5/can_i_connect_a_yamaha_psr_f51_keyboard_to_a_pc/

[^6]: https://gzhls.at/blob/ldb/0/4/a/6/380eabb9410989bd1287916033293c01f858.pdf

[^7]: https://www.vcelink.com/blogs/focus/a-brief-guide-to-audio-ports

[^8]: https://manual.audacityteam.org/man/tutorial_connecting_up.html

[^9]: https://www.hollyland.com/blog/tips/connect-audio-interface-to-pc

[^10]: https://www.freesoundrecorder.net/tutorial-how-to-connect-sound-source-to-pc/

[^11]: https://discourse.ardour.org/t/audio-setup-alsa-for-recording-pulseaudio-for-playback/110296

[^12]: https://wiki.archlinux.org/title/PulseAudio

[^13]: https://www.reddit.com/r/linux/comments/coi4dt/a_complete_guide_of_and_debunking_of_audio_on/

[^14]: https://cyberpanel.net/blog/pipewire-linux

[^15]: https://wiki.archlinux.org/title/PipeWire

[^16]: https://wiki.archlinux.org/title/JACK_Audio_Connection_Kit

[^17]: https://linuxaudio.github.io/libremusicproduction/html/articles/demystifying-jack-–-beginners-guide-getting-started-jack.html

[^18]: https://www.youtube.com/watch?v=YR1y-rmjE8E

[^19]: https://www.youtube.com/watch?v=HfERfoeQ8Zo

[^20]: https://support.microsoft.com/en-us/windows/how-to-set-up-and-test-microphones-in-windows-ba9a4aab-35d1-12ee-5835-cccac7ee87a4

[^21]: https://forum.audacityteam.org/t/making-a-direct-recording-from-a-keyboard-to-a-laptop-using-audacity/115036

[^22]: https://www.image-line.com/fl-studio-learning/fl-studio-online-manual/html/envsettings_asio4all.htm

[^23]: https://help.ableton.com/hc/en-us/articles/360003149240-Using-an-ASIO-audio-driver-on-Windows

[^24]: https://www.sweetwater.com/sweetcare/articles/installing-and-using-asio4all-for-windows/

[^25]: https://fedoraproject.org/wiki/JACK_Audio_Connection_Kit

[^26]: https://itsgratuitous.com/best-fl-studio-settings/

[^27]: https://www.image-line.com/fl-studio-learning/fl-studio-online-manual/html/app_wiz2.htm

[^28]: https://www.image-line.com/fl-studio-learning/fl-studio-online-manual/html/envsettings_audio.htm

[^29]: https://www.youtube.com/watch?v=SyQKGLioVYk

[^30]: https://www.youtube.com/watch?v=QotY0GWytlU

[^31]: https://eliasdorneles.com/til/posts/using-pipewire-for-music-on-linux/

[^32]: https://www.youtube.com/watch?v=U48J-HOOJ7c

[^33]: https://docs.lmms.io/user-manual/getting-started/settings

[^34]: https://www.youtube.com/watch?v=iSqQgnUX0wM

[^35]: https://www.youtube.com/watch?v=0fGZ3QTLPMs

[^36]: https://www.reddit.com/r/lmms/comments/1iz4lvj/is_anyone_aware_of_audio_input_in_lmms/

[^37]: https://www.youtube.com/watch?v=7ZFhj55UBOA

[^38]: https://www.youtube.com/watch?v=g6Oz6d7CqCs

[^39]: https://forum.audacityteam.org/t/solved-recording-on-linux-alsa/42496

[^40]: https://www.reddit.com/r/FL_Studio/comments/bjkmiy/how_to_connect_usb_audio_interface_with_fl_studio/

[^41]: https://www.youtube.com/watch?v=V-yMkDv9TIE

[^42]: https://www.youtube.com/watch?v=3qfa9hGJzoY

[^43]: https://www.youtube.com/watch?v=8vfhp8pXUW0\&vl=pt-BR

[^44]: https://www.reddit.com/r/linuxaudio/comments/1knsk3k/the_state_of_audio_production_on_linux_in_2025/

[^45]: https://asio4all.org

[^46]: https://www.xda-developers.com/goodbye-to-linux-audio-headaches-pipewire-simplifies-everything/

[^47]: https://jackaudio.org/downloads/

[^48]: https://www.reddit.com/r/ableton/comments/11iyznc/help_with_setting_up_asio4all_driver_on_windows/

[^49]: https://linuxmusicians.com/viewtopic.php?t=27569

[^50]: https://www.youtube.com/watch?v=4kuZsklrm-0

[^51]: https://sempreupdate.com.br/como-configurar-um-debian-studio-como-instalar-o-jack-o-servidor-de-audio-profissional-para-linux/

[^52]: https://forum.lwks.com/threads/clicks-an-pops-with-2025-1-and-pipewire-1-2-7-linux.252188/

[^53]: https://support.solidstatelogic.com/hc/en-gb/articles/5349706035229-SSL-USB-Audio-ASIO-WDM-Windows-Driver-Downloads

[^54]: https://jackaudio.org/faq/linux_rt_config.html

