<img src="https://r2cdn.perplexity.ai/pplx-full-logo-primary-dark%402x.png" style="height:64px;margin-right:32px"/>

# Tutorial Completo de Instalação do Ardour no Ubuntu 22.04

**Principais recomendações**
Para a maior estabilidade e atualizações automáticas, recomenda-se instalar via repositório oficial Ubuntu Studio Backports (APT) ou Flatpak. Usuários que desejam controle máximo podem usar AppImage ou compilação a partir do código-fonte.

***

## 1. Pré-requisitos

Antes de qualquer método, abra um terminal (Ctrl +Alt +T) e atualize os pacotes:

```bash
sudo apt update && sudo apt upgrade -y
```


***

## 2. Instalação via APT (Ubuntu Studio Backports)

1. Adicione o PPA do Ubuntu Studio Backports:

```bash
sudo add-apt-repository ppa:ubuntustudio-ppa/backports
```

2. Atualize o índice de pacotes:

```bash
sudo apt update
```

3. Instale o Ardour:

```bash
sudo apt install ardour
```

4. Execute:

```bash
ardour
```


Esse método fornece integração com gerenciador de pacotes e recebe futuras atualizações automaticamente.[^1]

***

## 3. Instalação via Flatpak

1. Instale o Flatpak (se necessário):

```bash
sudo apt install flatpak
```

2. Adicione o repositório Flathub:

```bash
flatpak remote-add --if-not-exists flathub https://flathub.org/repo/flathub.flatpakrepo
```

3. Instale o Ardour:

```bash
flatpak install flathub org.ardour.Ardour
```

4. Execute:

```bash
flatpak run org.ardour.Ardour
```


Recomendado para acesso às versões mais recentes sem alterar bibliotecas do sistema.[^2]

***

## 4. Uso de AppImage

1. Baixe o AppImage em https://ardour.org/download.html.
2. Torne-o executável:

```bash
chmod +x Ardour-*.AppImage
```

3. Execute:

```bash
./Ardour-*.AppImage
```


Versão portátil, sem dependências do sistema, mas sem atualizações automáticas.[^2]

***

## 5. Compilação a partir do Código-Fonte

Use quando precisar de otimizações específicas ou recursos em desenvolvimento.

1. Instale dependências básicas:

```bash
sudo apt install build-essential git python3 waf \
  libjack-jackd2-dev libboost-all-dev libasound2-dev \
  libcurl4-gnutls-dev libglibmm-2.4-dev libsndfile1-dev \
  librubberband-dev libfftw3-dev libaubio-dev libxml2-dev \
  vamp-plugin-sdk libtag1-dev liblo-dev libcwiid-dev
```

2. Clone o repositório Ardour:

```bash
git clone git://git.ardour.org/ardour/ardour.git ~/src/ardour
cd ~/src/ardour
```

3. Configure e compile:

```bash
./waf configure
./waf
```

4. Instale (opcional):

```bash
sudo ./waf install
```

5. Execute:

```bash
ardour5   # ou ardour6, conforme versão compilada
```


Detalhes de dependências e flags em: Ardour Build Docs.[^3]

***

## 6. Dicas e Solução de Problemas

- Se houver erros de JACK ou ALSA, instale/atualize o PipeWire e pulse support:

```bash
sudo apt install pipewire pipewire-pulse
```

- Para permissões de áudio em tempo real:

```bash
sudo usermod -aG audio $USER
```

Depois, reinicie a sessão.
- Use `pw-jack ardour` se o PipeWire não for detectado automaticamente.

***

**Observação sobre fontes**
Devido a limitações de ferramentas de pesquisa, foram utilizadas as principais referências disponíveis dos repositórios oficiais, blogs técnicos e documentação Ardour.[^1][^3][^2]
<span style="display:none">[^10][^11][^12][^13][^14][^15][^16][^17][^18][^19][^20][^4][^5][^6][^7][^8][^9]</span>

<div align="center">⁂</div>

[^1]: https://www.geeksforgeeks.org/installation-guide/how-to-install-ardour-on-ubuntu/

[^2]: https://www.ubuntufree.com/ardour-for-ubuntu/

[^3]: https://discourse.ardour.org/t/compile-and-run-ardour-6-9-ubuntu-22-04/107487

[^4]: https://ardour.org/building_linux.html

[^5]: https://www.reddit.com/r/Ardour/comments/ydt67f/howto_get_ardour_7_running_on_ubuntu_2210_with/

[^6]: https://www.youtube.com/watch?v=XBTQKm74M7w

[^7]: http://linuxaudioproduction.blogspot.com/2015/09/how-to-build-ardour-from-git.html

[^8]: http://www.leftclicker.net/doku.php?id=compiling_ardour_8_on_ubuntu_23

[^9]: http://elinuxbook.com/install-ardour-linux-audio-workstation-in-ubuntu-16-04-a-best-linux-audio-editor/

[^10]: https://www.reddit.com/r/Ardour/comments/17wh0kb/how_do_i_get_ardour_running_on_ubuntu/?tl=es-419

[^11]: https://dev.to/ericsonwillians/building-ardour-from-source-on-linux-a-comprehensive-guide-561d

[^12]: https://discourse.ardour.org/t/ardour-in-ppa-repos/104436

[^13]: https://ardour.org/first_time_linux.html

[^14]: https://guysherman.com/2015/08/12/building-ardour-on-ubuntu/

[^15]: https://www.youtube.com/watch?v=aPQi1P1so1c

[^16]: https://www.edivaldobrito.com.br/digital-audio-workstation-ardour/

[^17]: https://www.youtube.com/watch?v=-cREeP5-pR8

[^18]: http://linuxmao.org/Ardour+2+-+installation

[^19]: https://www.reddit.com/r/linuxaudio/comments/zvfxbs/how_to_setup_ardour_on_fedora_silverblue_and_any/

[^20]: https://guysherman.com/2016/06/21/building-ardour-on-arch/

