Veja essa sequência de comandos, quando eu atualizei o sistema e fiz `sudo modprobe r8169 ` após reinicializar a funcionalidade da Ethernet voltou como se nada tivesse acontecido. Seu objetivo é preencher os comando com texto descrevendo o problema e conforme os comandos são executá-los (Você irá repetí-los e preencher com parágrafos explicativos entre eles) Explicando o que o comando faz e o que é esse erro.

[CONTEXTO]:
Meu Ubuntu 22.04 está sem conexão por cabo o que eu faço? Parece que não tem nenhum driver instalado ou parece que parou de funcionar em alguma atualização e não tem nem sequer a opção mais nas configurações. Eu plugo o cabo e nada acontece.

```bash
uname -a

Linux dell-g15 6.8.0-60-generic #63~22.04.1-Ubuntu SMP PREEMPT_DYNAMIC Tue Apr 22 19:00:15 UTC 2 x86_64 x86_64 x86_64 GNU/Linux
```

```bash
lspci | grep -i eth   

03:00.0 Ethernet controller: Realtek Semiconductor Co., Ltd. RTL8111/8168/8411 PCI Express Gigabit Ethernet Controller (rev 15)
```

```bash
nmcli device status     

DEVICE             TYPE      STATE                   CONNECTION 
wlp0s20f3          wifi      conectado               Brenda_5G  
docker0            bridge    connected (externally)  docker0    
virbr0             bridge    connected (externally)  virbr0     
98:39:8E:D5:0E:2E  bt        desconectado            --         
p2p-dev-wlp0s20f3  wifi-p2p  desconectado            --         
lo                 loopback  não gerenciável         --   
```

Resolvi fazer o upgrade da máquina, porém nada do driver foi listado. Ele já estava instalado desde o começo.

```bash
sudo apt-get update
sudo apt-get upgrade
```

Reiniciei a máquina

```bash
sudo reboot
```

O cabo Ethernet ainda não está sendo reconhecido:

```bash
ip link show                                                                                     
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN mode DEFAULT group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
2: wlp0s20f3: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP mode DORMANT group default qlen 1000
    link/ether 14:75:5b:99:8c:f7 brd ff:ff:ff:ff:ff:ff
3: virbr0: <NO-CARRIER,BROADCAST,MULTICAST,UP> mtu 1500 qdisc noqueue state DOWN mode DEFAULT group default qlen 1000
    link/ether 52:54:00:6b:04:44 brd ff:ff:ff:ff:ff:ff
4: docker0: <NO-CARRIER,BROADCAST,MULTICAST,UP> mtu 1500 qdisc noqueue state DOWN mode DEFAULT group default 
    link/ether 72:fd:35:40:2a:ef brd ff:ff:ff:ff:ff:ff
```

Esse `virbr0` não parece ser relacionado ao problema:

```bash
sudo ip link virbr0 up

Command "virbr0" is unknown, try "ip link help".
```

```bash
ifconfig                                                                                                   

docker0: flags=4099<UP,BROADCAST,MULTICAST>  mtu 1500
        inet 172.17.0.1  netmask 255.255.0.0  broadcast 172.17.255.255
        ether 72:fd:35:40:2a:ef  txqueuelen 0  (Ethernet)
        RX packets 0  bytes 0 (0.0 B)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 0  bytes 0 (0.0 B)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

lo: flags=73<UP,LOOPBACK,RUNNING>  mtu 65536
        inet 127.0.0.1  netmask 255.0.0.0
        inet6 ::1  prefixlen 128  scopeid 0x10<host>
        loop  txqueuelen 1000  (Loopback Local)
        RX packets 4311  bytes 2588104 (2.5 MB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 4311  bytes 2588104 (2.5 MB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

virbr0: flags=4099<UP,BROADCAST,MULTICAST>  mtu 1500
        inet 192.168.122.1  netmask 255.255.255.0  broadcast 192.168.122.255
        ether 52:54:00:6b:04:44  txqueuelen 1000  (Ethernet)
        RX packets 0  bytes 0 (0.0 B)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 0  bytes 0 (0.0 B)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

wlp0s20f3: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 192.168.1.101  netmask 255.255.255.0  broadcast 192.168.1.255
        inet6 fe80::b10:faf6:cf37:c43b  prefixlen 64  scopeid 0x20<link>
        ether 14:75:5b:99:8c:f7  txqueuelen 1000  (Ethernet)
        RX packets 2471841  bytes 2760083951 (2.7 GB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 2257473  bytes 1897374953 (1.8 GB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0
```

O sistema lista que há Ethernet mas ele está sem nome físico:

```bash
sudo lshw -C network       

  *-network                 
       descrição: Interface sem fio
       produto: Alder Lake-P PCH CNVi WiFi
       fabricante: Intel Corporation
       ID físico: 14.3
       informações do barramento: pci@0000:00:14.3
       nome lógico: wlp0s20f3
       versão: 01
       serial: 14:75:5b:99:8c:f7
       largura: 64 bits
       clock: 33MHz
       capacidades: pm msi pciexpress msix bus_master cap_list ethernet physical wireless
       configuração: broadcast=yes driver=iwlwifi driverversion=6.8.0-60-generic firmware=86.fb5c9aeb.0 so-a0-hr-b0-86.uc ip=192.168.1.101 latency=0 link=yes multicast=yes wireless=IEEE 802.11
       recursos: memória de E/S:610-60f irq:16 memória:6103144000-6103147fff
  *-network DISPONÍVEL
       descrição: Ethernet controller
       produto: RTL8111/8168/8411 PCI Express Gigabit Ethernet Controller
       fabricante: Realtek Semiconductor Co., Ltd.
       ID físico: 0
       informações do barramento: pci@0000:03:00.0
       versão: 15
       largura: 64 bits
       clock: 33MHz
       capacidades: pm msi pciexpress msix cap_list
       configuração: latency=0
       recursos: memória de E/S:ffffffff0-fffffffef memória de E/S:ffffffff0-fffffffef porta de E/S:3000(tamanho=256) memória:72104000-72104fff memória:72100000-72103fff
```

De novo esse `virbr0` não parece ter relevância

```bash
  ethtool -i virbr0                                                                                              

  driver: bridge
  version: 2.3
  firmware-version: N/A
  expansion-rom-version: 
  bus-info: N/A
  supports-statistics: no
  supports-test: no
  supports-eeprom-access: no
  supports-register-dump: no
  supports-priv-flags: no
```

Vou fazer novamente o modprobe mesmo já tendo feito antes de reinicializar e atualizar o sistema.

```bash
sudo modprobe r8169 
```

Nada acontece de imediato:

```bash
nmcli device status
DEVICE             TYPE      STATE                   CONNECTION 
wlp0s20f3          wifi      conectado               Brenda_5G  
docker0            bridge    connected (externally)  docker0    
virbr0             bridge    connected (externally)  virbr0     
98:39:8E:D5:0E:2E  bt        desconectado            --         
p2p-dev-wlp0s20f3  wifi-p2p  desconectado            --         
lo                 loopback  não gerenciável         -- 
```

Quando vou ler os erros:

```
sudo dmesg | grep r816                                                                                         

[117301.095883] r8169 0000:03:00.0: error -EIO: PCI read failed
[117301.095896] r8169: probe of 0000:03:00.0 failed with error -5
```

Lendo mais erros:

```bash
sudo dmesg | grep r816                                                                                         qua 20 ago 2025 19:08:58
[  107.398267] r8169 0000:03:00.0: can't disable ASPM; OS doesn't have ASPM control
[  107.408843] r8169 0000:03:00.0 eth0: RTL8168h/8111h, 04:bf:1b:10:d5:34, XID 541, IRQ 170
[  107.408854] r8169 0000:03:00.0 eth0: jumbo features [frames: 9194 bytes, tx checksumming: ko]
[  107.420476] r8169 0000:03:00.0 enp3s0: renamed from eth0
[  107.459455] Generic FE-GE Realtek PHY r8169-0-300:00: attached PHY driver (mii_bus:phy_addr=r8169-0-300:00, irq=MAC)
[  107.606691] r8169 0000:03:00.0 enp3s0: Link is Down
```

Vou buscar por "blacklist" dentro da máquina, parece que não tem nada assim aqui:

```bash
cat /etc/modprobe.d/* | grep r816
# settings for r8168-dkms
# map the specific PCI IDs instead of blacklisting the whole r8169 module
alias    pci:v00001186d00004300sv00001186sd00004B10bc*sc*i*    r8168
alias    pci:v000010ECd00008168sv*sd*bc*sc*i*            r8168
# to blacklist the whole r8169 module
#blacklist r8169
```

Por algum motivo volto a verificar e agora está funcionando. Talvez o NetworkManager demorou iniciar o Daemon. (Minha teoria pode estar errada)

```bash
ip link show
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN mode DEFAULT group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
2: wlp0s20f3: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP mode DORMANT group default qlen 1000
    link/ether 14:75:5b:99:8c:f7 brd ff:ff:ff:ff:ff:ff
3: virbr0: <NO-CARRIER,BROADCAST,MULTICAST,UP> mtu 1500 qdisc noqueue state DOWN mode DEFAULT group default qlen 1000
    link/ether 52:54:00:6b:04:44 brd ff:ff:ff:ff:ff:ff
4: docker0: <NO-CARRIER,BROADCAST,MULTICAST,UP> mtu 1500 qdisc noqueue state DOWN mode DEFAULT group default 
    link/ether 86:83:43:ab:9e:65 brd ff:ff:ff:ff:ff:ff
5: enp3s0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP mode DEFAULT group default qlen 1000
    link/ether 04:bf:1b:10:d5:34 brd ff:ff:ff:ff:ff:ff
```

```bash
DEVICE             TYPE      STATE                   CONNECTION        
enp3s0             ethernet  conectado               Conexão cabeada 1 
wlp0s20f3          wifi      conectado               Brenda_5G         
docker0            bridge    connected (externally)  docker0           
virbr0             bridge    connected (externally)  virbr0            
98:39:8E:D5:0E:2E  bt        desconectado            --                
p2p-dev-wlp0s20f3  wifi-p2p  desconectado            --                
lo                 loopback  não gerenciável         --  
```
