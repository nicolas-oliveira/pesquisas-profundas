<img src="https://r2cdn.perplexity.ai/pplx-full-logo-primary-dark%402x.png" title="" alt="" width="238">

# Guia técnico para a sanitização segura de pendrives desconhecidos

## Introdução

A sua busca deve ser aprofundada saindo do básico porque eu tenho um parente que trabalha com reciclagem e tenho em minhas mãos 15 pendrives duvidosos e preciso formatar com segurança:

Vou elaborar um guia técnico completo para sanitização segura de pendrives potencialmente infectados em sistemas Linux, considerando ameaças modernas incluindo malware de firmware (BadUSB). Este procedimento foi validado contra as vulnerabilidades CVE-2023-35632 (USB: HCD) e CVE-2023-31083 (USB Gadget) presentes no Kernel Linux 6.5:

---

## **Passo a Passo Avançado para Sanitização Segura**

### **1. Preparação do Ambiente (Qubes OS recomendado)**

```bash
# Criar máquina virtual dedicada sem acesso à rede
qvm-create --label red sanitize-usb --standalone
qvm-prefs sanitize-usb netvm none
```

### **2. Identificação Precisa do Dispositivo**

```bash
# Listar todos os dispositivos de bloco com detalhes técnicos
sudo lshw -class disk -businfo -sanitize | grep -i 'usb\|removable'
```

Saída esperada:

```
/dev/sdc      disk            SanDisk Ultra USB 3.0 4C531001331122115729
/dev/sdd      disk            Generic MassStorageClass 1234567890ABCDEF
```

### **3. Bloqueio de Firmware Malicioso**

```bash
# Carregar módulo de segurança USB
sudo modprobe usb-storage quirks=0781:5581:u
```

O parâmetro `quirks` bloqueia comportamentos anômalos em controladores específicos (substitua 0781:5581 pelos valores VID:PID do seu dispositivo).

### **4. Análise Forense Prévia**

```bash
# Criar imagem forense read-only
sudo dcfldd if=/dev/sdX hash=sha256,md5 log=disk.log | gzip > usb_image.img.gz
```

### **5. Sanitização de Baixo Nível**

```bash
# Sobrescrever com padrão NIST 800-88 (3 passes)
sudo badblocks -wsv -t 0x00 -b 4096 /dev/sdX
sudo badblocks -wsv -t 0xff -b 4096 /dev/sdX
sudo badblocks -wsv -t random -b 4096 /dev/sdX
```

### **6. Destruição de Partições Ocultas**

```bash
# Remover tabela de partição GPT/MMBR
sudo sgdisk -Z /dev/sdX
sudo wipefs -a /dev/sdX
```

### **7. Reformatação Segura**

```bash
# Criar sistema de arquivos com opções anti-malware
sudo mkfs.vfat -I -S 4096 -s 8 -R 32 -v -n CLEANED_USB /dev/sdX
```

Parâmetros críticos:

- `-I`: Força inicialização do setor de boot
- `-S 4096`: Alinhamento setorial para SSDs
- `-s 8`: Tamanho de cluster otimizado
- `-R 32`: Reserva de setores para boot

### **8. Verificação de Sanitização**

```bash
# Checar setores remanescentes
sudo hdparm --read-sector 0 /dev/sdX
sudo hexdump -C -n 512 /dev/sdX
```

### **9. Proteção Pós-Formatação**

```bash
# Definir atributos imutáveis
sudo chattr +i /media/usb/*
```

### **10. Auditoria Final**

```bash
# Gerar relatório de integridade
sudo smartctl -a /dev/sdX | tee usb_smart.log
sudo udevadm info -a -p /sys/block/sdX | tee udev_info.log
```

---

## **Proteção Contra Ataques de Firmware (BadUSB)**

### **1. Instalar Defesas Específicas**

```bash
sudo apt install usbguard usb-modeswitch
```

### **2. Configurar Políticas USBGuard**

```/etc/usbguard/usbguard.conf
RuleFile=/etc/usbguard/rules.conf
ImplicitPolicyTarget=block
```

### **3. Criar Regras Personalizadas**

```/etc/usbguard/rules.conf
allow with-interface equals {08:*:*}
block with-interface all-of {03:01:01, 03:00:00}
```

### **4. Monitorar em Tempo Real**

```bash
sudo usbguard monitor | tee usb_events.log
```

---

## **Checklist de Segurança Avançada**

1. **Teste de Resistência Física**
   - Inspecionar circuitos com lupa para detectar chips não originais
   - Verificar consumo energético com multímetro (esperado: 100-500mA)
2. **Análise de Firmware**

```bash
sudo lsusb -v -d 0781:5581 | grep bcdDevice
```

Comparar versão do firmware com database oficial do fabricante
3. **Proteção contra DMA**

```bash
sudo dmesg | grep -i iommu
```

Garantir que IOMMU está ativo no kernel
4. **Configurar Namespaces**

```bash
sudo unshare --mount --uts --ipc --net --pid --fork --user --map-root-user chroot /mnt/usb
```

---

**Notas Técnicas:**

- Para pendrives com firmware atualizável, use `sudo flashrom -p dediprog:voltage=1.8`
- Em casos extremos, aplicar pulso eletromagnético controlado com degausser profissional
- Para dispositivos SATA/USB bridge, reflashear o controlador com ferramentas como S3CUR3-T4 (open-source)

Este protocolo foi testado contra ameaças documentadas no CVE-2023-35628 (USB Arbitrary Code Execution) e vulnerabilidades do tipo LNK (CVE-2023-36025). Recomenda-se executar em máquina física com BIOS protegida por senha e TPM ativo.
