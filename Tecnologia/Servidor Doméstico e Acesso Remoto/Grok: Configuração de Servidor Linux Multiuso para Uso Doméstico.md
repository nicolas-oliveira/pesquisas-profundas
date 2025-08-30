# Grok: Configuração de Servidor Linux Multiuso para Uso Doméstico

Este relatório detalha a configuração de um servidor Linux multiuso para desenvolvimento web, Machine Learning (ML) e Inteligência Artificial (IA), com acesso remoto e exposição na internet, conforme especificado. O hardware disponível é analisado, com foco na possibilidade de usar ROCm com a GPU AMD Radeon RX 570, comparações de distribuições Linux e sugestões de upgrades.

## 1. Escolha da Distribuição Linux

### Requisitos

- Distribuição "server" sem interface gráfica.
- Suporte para desenvolvimento web (Nginx, Apache) e ML/AI (TensorFlow, PyTorch).
- Facilidade de configuração para acesso remoto e segurança.

### Análise e Comparação de Distribuições

O hardware inclui:

- **CPU**: Intel Core i5-9400F (Hexa-Core, 2.9GHz base, 4.1GHz turbo).
- **GPU**: AMD Radeon RX 570 (8GB, Polaris, suporte limitado ao ROCm).
- **RAM**: 16GB DDR4 2666MHz.
- **Armazenamento**: NVMe 1TB (Kingston NV3), SSD SATA 240GB (WD Green).
- **Fonte**: EVGA 500W 80 Plus White.

**Opções de Distribuição**:

| Distribuição      | Vantagens                                                                                | Desvantagens                                   | Indicada para                          |
| ----------------- | ---------------------------------------------------------------------------------------- | ---------------------------------------------- | -------------------------------------- |
| **Ubuntu Server** | Amplo suporte, documentação extensa, compatibilidade com ML/AI e web, fácil configuração | Pacotes podem ser menos recentes que Fedora    | Desenvolvimento web, ML/AI, servidores |
| **Debian**        | Estabilidade, confiabilidade, bom para servidores                                        | Pacotes mais antigos, menos suporte para ML/AI | Servidores estáveis                    |
| **CentOS/RHEL**   | Estabilidade empresarial, suporte de longo prazo                                         | Configuração complexa para ML/AI               | Servidores corporativos                |
| **Fedora Server** | Software atualizado, suporte a novas tecnologias                                         | Menos estável, suporte de curto prazo          | Desenvolvimento experimental           |
| **Arch Linux**    | Rolling release, altamente configurável                                                  | Requer conhecimento avançado, menos estável    | Usuários avançados                     |

**Trade-off**:

- **Ubuntu Server 24.04 LTS** é a melhor escolha devido à sua popularidade, suporte para ferramentas de desenvolvimento web (Nginx, Apache) e ML/AI (TensorFlow, PyTorch), além de facilidade de configuração para acesso remoto e segurança.
- Debian é uma alternativa para maior estabilidade, mas menos prática para ML/AI.
- CentOS/RHEL é ideal para ambientes corporativos, mas menos flexível.
- Fedora e Arch são mais indicados para usuários avançados ou experimentais.

**Escolha**: Ubuntu Server 24.04 LTS.

**Citações**:

- TechRadar: Best Linux distro for developers
- Analytics India Magazine: Best Linux distro for machine learning
- KDnuggets: Top 5 Linux Distro for Data Science

## 2. Particionamento

### Requisitos

- `/ (raiz)`: 40GB no NVMe.
- `/home`: 500GB no NVMe.
- `/var (logs)`: 200GB no SSD SATA.
- `Swap`: 8GB (igual à RAM total).

### Análise

- **NVMe (1TB)**: 40GB (`/`) + 500GB (`/home`) = 540GB, sobrando 460GB para datasets ou outros usos.
- **SSD SATA (240GB)**: 200GB (`/var`), sobrando 40GB.
- **Swap**: 8GB é adequado para 16GB de RAM, mas pode ser ajustado com upgrades de RAM.

**Trade-off**:

- O esquema utiliza o NVMe para o sistema e dados de usuários (alta velocidade) e o SSD SATA para logs (menos intensivo).
- O espaço restante no NVMe é útil para datasets de ML/AI.

**Conclusão**: Seguir o esquema de particionamento especificado.

## 3. Configuração do GPU

### Requisitos

- Instalar drivers AMD ROCm para suporte a ML.

### Análise

- A AMD Radeon RX 570 (arquitetura Polaris, gfx803) não é oficialmente suportada pelo ROCm em 2025, conforme documentação (ROCm System Requirements).
- No entanto, métodos não oficiais, como instalar ROCm 3.5 ou 4.3 em Ubuntu 20.04, podem funcionar, embora com limitações de desempenho e estabilidade (GitHub Gist: Installing ROCm on Ubuntu 20.04.4 + RX 570).
- Frameworks como TensorFlow e PyTorch podem não suportar o RX 570 sem compilações personalizadas (Reddit: ROCm support for RX 570).
- O driver open-source `amdgpu` (incluído no kernel Linux) suporta o RX 570 para computação geral, mas não é necessário para um servidor sem GUI.

**Trade-off**:

- Tentar instalar ROCm no RX 570 pode ser instável. O CPU Intel Core i5-9400F é uma alternativa confiável para ML/AI, embora menos eficiente que GPUs.
- Para ML/AI intensivo, considerar GPUs compatíveis com ROCm ou serviços de nuvem.

**Conclusão**:

- Usar o driver `amdgpu` do kernel para o RX 570.
- Configurar ML/AI no CPU, com tentativa de ROCm apenas se necessário.

## 4. Passo a Passo para Configuração

### 4.1 Preparação da Mídia

- Baixar a ISO do Ubuntu Server 24.04 LTS (Ubuntu Downloads).
- Gravar em USB com `balenaEtcher`.

### 4.2 BIOS/UEFI

- Desativar Secure Boot.
- Priorizar inicialização via USB.
- Ativar modo AHCI para SSDs.

### 4.3 Instalação do Sistema Operacional

- Selecionar Ubuntu Server.
- Configurar:
  - **Rede**: IP fixo (ex: 192.168.1.100).
  - **Usuário**: `admin` (com senha forte).
- Particionamento manual:
  - `/`: 40GB (NVMe, ex: /dev/nvme0n1p1).
  - `/home`: 500GB (NVMe, ex: /dev/nvme0n1p2).
  - `/var`: 200GB (SSD SATA, ex: /dev/sda1).
  - `Swap`: 8GB (ex: /dev/nvme0n1p3).
- Instalar pacotes essenciais: OpenSSH, Docker, Python3.

### 4.4 Pós-instalação

- Atualizar sistema:
  
  ```bash
  sudo apt update && sudo apt upgrade -y
  ```

- Verificar TRIM para NVMe:
  
  ```bash
  sudo fstrim -av
  ```

- Configurar Docker:
  
  ```bash
  sudo groupadd docker
  sudo usermod -aG docker admin
  ```

### 4.5 Tentativa de Instalação do ROCm (Opcional)

- **Nota**: O RX 570 não é oficialmente suportado. A instalação pode falhar ou ser instável.

- Tentar instalar ROCm em Ubuntu 24.04:
  
  ```bash
  sudo apt update
  sudo apt install rocm-libs rocminfo
  sudo usermod -a -G render,video $LOGNAME
  ```

- Verificar suporte:
  
  ```bash
  rocminfo
  ```

- Se não funcionar, usar versões mais antigas (ex: ROCm 3.5 em Ubuntu 20.04) ou compilar frameworks manualmente.

### 4.6 Stack ML/AI

- Instalar Miniconda:
  
  ```bash
  wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh
  bash Miniconda3-latest-Linux-x86_64.sh
  ```

- Criar ambiente para TensorFlow (CPU):
  
  ```bash
  conda create -n tf python=3.10
  conda activate tf
  pip install tensorflow
  ```

- Alternativamente, para PyTorch:
  
  ```bash
  conda create -n torch python=3.10
  conda activate torch
  pip install torch
  ```

### 4.7 Serviços Web

- Instalar Nginx:
  
  ```bash
  sudo apt install nginx
  ```

- Habilitar firewall:
  
  ```bash
  sudo ufw allow 'Nginx Full'
  sudo ufw enable
  ```

### 4.8 Acesso Remoto e Segurança

- Configurar SSH com chaves:
  
  ```bash
  ssh-keygen -t rsa -b 4096
  ssh-copy-id admin@192.168.1.100
  ```

- Desativar autenticação por senha no SSH:
  
  ```bash
  sudo nano /etc/ssh/sshd_config
  # Alterar: PasswordAuthentication no
  sudo systemctl restart sshd
  ```

- Configurar port forwarding no roteador para portas 22 (SSH) e 80/443 (web).

- Instalar fail2ban para proteção contra ataques:
  
  ```bash
  sudo apt install fail2ban
  sudo systemctl enable fail2ban
  ```

- Considerar VPN para acesso remoto seguro.

## 5. Sugestões de Upgrade

| Componente        | Atual                   | Recomendado               | Motivo                                     |
| ----------------- | ----------------------- | ------------------------- | ------------------------------------------ |
| **RAM**           | 16GB DDR4 2666MHz       | 32GB DDR4 3200MHz         | Evita swapping em modelos de ML complexos. |
| **GPU**           | RX 570 8GB              | RX 7900 XT ou Instinct MI | Suporte oficial a ROCm para ML/AI.         |
| **Fonte**         | EVGA 500W 80 Plus White | Corsair 750W 80 Plus Gold | Estabilidade com upgrades.                 |
| **Armazenamento** | NVMe 1TB + SSD 240GB    | Adicionar NVMe 1TB        | RAID 0 ou armazenamento de dados.          |

**Justificativa**:

- **RAM**: 16GB pode ser limitante para ML/AI no CPU com datasets grandes.
- **GPU**: RX 7900 XT ou Instinct MI oferecem suporte oficial ao ROCm, melhorando desempenho em ML/AI.
- **Fonte**: 750W suporta expansões futuras.
- **Armazenamento**: Um segundo NVMe melhora desempenho para datasets.

## 6. Considerações Finais

- **Limitações**:
  - O RX 570 tem suporte limitado ao ROCm, podendo causar instabilidade.
  - O Intel Core i5-9400F é capaz para ML/AI, mas menos eficiente que GPUs.
- **Recomendações**:
  - Para ML/AI intensivo, considerar serviços de nuvem (ex: AWS, Google Cloud).
  - Configurar o servidor com foco em segurança para exposição na internet.
- **Distribuição**: Ubuntu Server 24.04 LTS é ideal por sua compatibilidade e suporte.

**Citações**:

- ITProToday: AI-Ready Linux Distributions To Watch in 2025
- GitHub Issue #2071: ROCm on RX 570