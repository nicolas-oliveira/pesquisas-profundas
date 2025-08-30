# Perplexity: Criando um Servidor Ubuntu Server 22.04 LTS com Acesso Remoto e Multiprojetos

## Instalação do Ubuntu Server 22.04 LTS

### Download e Preparação da Mídia

Para criar um servidor Ubuntu Server 22.04 LTS, primeiro baixe a imagem ISO oficial do Ubuntu Server. Esta versão é uma LTS (Long Term Support) que oferece cinco anos de suporte e atualizações gratuitas, sendo ideal para ambientes de produção.[^1][^2]

**Requisitos mínimos do sistema:**

- Processador Dual Core (2 GHz)
- 4 GB de RAM física
- 25 GB ou mais de espaço em disco
- Mídia inicializável (USB)
- Conectividade com a Internet (opcional)

### Processo de Instalação

1. **Criação da mídia inicializável**: Grave a imagem ISO em um pendrive usando ferramentas como balenaEtcher[^1]
2. **Boot do sistema**: Configure o BIOS para inicializar pela mídia USB
3. **Instalação**: Siga o processo interativo de instalação, selecionando idioma (recomenda-se manter em inglês), layout de teclado, e configurações de rede[^1]
4. **Particionamento**: Use as configurações padrão para simplificar o processo[^3]
5. **Configuração de usuário**: Defina nome do servidor, usuário e senha[^3]

## Configuração SSH para Acesso Remoto

### Instalação e Configuração do OpenSSH

Por padrão, o Ubuntu Server não vem com o SSH habilitado. Para permitir acesso remoto seguro, instale e configure o OpenSSH:

```bash
# Instalar o servidor SSH
sudo apt install openssh-server

# Habilitar e iniciar o serviço
sudo systemctl enable ssh
sudo systemctl start ssh

# Verificar status
sudo systemctl status ssh
```

### Configuração de Segurança SSH

Para maior segurança, edite o arquivo de configuração do SSH:[^4][^5]

```bash
sudo nano /etc/ssh/sshd_config
```

**Configurações recomendadas:**

- Alterar a porta padrão (opcional): `Port 2222`
- Desabilitar login root: `PermitRootLogin no`
- Configurar autenticação por chave: `PubkeyAuthentication yes`
- Limitar usuários: `AllowUsers seuusuario`

Após as alterações, reinicie o serviço:

```bash
sudo systemctl restart ssh
```

## Configuração do Firewall UFW

### Instalação e Configuração Básica

O Ubuntu vem com o UFW (Uncomplicated Firewall) pré-instalado, mas desabilitado:[^6][^7]

```bash
# Verificar status
sudo ufw status

# Configurar políticas padrão
sudo ufw default deny incoming
sudo ufw default allow outgoing

# Permitir SSH antes de habilitar o firewall
sudo ufw allow ssh
# ou especificar porta personalizada
sudo ufw allow 2222

# Habilitar o firewall
sudo ufw enable
```

### Liberação de Portas para Serviços Web

Para multiprojetos web, libere as portas necessárias:[^8][^6]

```bash
# HTTP
sudo ufw allow 80

# HTTPS
sudo ufw allow 443

# Portas personalizadas para diferentes projetos
sudo ufw allow 3000
sudo ufw allow 8080

# Verificar regras
sudo ufw status verbose
```

## Configuração do Servidor Web

### Apache + PHP-FPM

Para hospedar múltiplos projetos web, instale o Apache com PHP-FPM:[^9][^10]

```bash
# Atualizar sistema
sudo apt update && sudo apt upgrade

# Instalar Apache e PHP-FPM
sudo apt install apache2 php-fpm

# Instalar módulos necessários
sudo apt install libapache2-mod-fcgid

# Habilitar módulos
sudo a2enmod mpm_event
sudo a2enmod proxy
sudo a2enmod proxy_fcgi
sudo a2enconf php8.1-fpm

# Reiniciar Apache
sudo systemctl restart apache2
```

### Virtual Hosts para Multiprojetos

Para hospedar múltiplos projetos, configure Virtual Hosts:[^11][^12]

```bash
# Criar diretórios para projetos
sudo mkdir -p /var/www/projeto1.com/public_html
sudo mkdir -p /var/www/projeto2.com/public_html

# Configurar permissões
sudo chown -R $USER:$USER /var/www/projeto1.com/public_html
sudo chown -R $USER:$USER /var/www/projeto2.com/public_html
```

**Criar arquivo de configuração do Virtual Host:**

```bash
sudo nano /etc/apache2/sites-available/projeto1.com.conf
```

**Conteúdo do arquivo:**

```apache
<VirtualHost *:80>
    ServerName projeto1.com
    ServerAlias www.projeto1.com
    DocumentRoot /var/www/projeto1.com/public_html

    <FilesMatch \.php$>
        SetHandler "proxy:unix:/run/php/php8.1-fpm.sock|fcgi://localhost/"
    </FilesMatch>

    ErrorLog ${APACHE_LOG_DIR}/projeto1_error.log
    CustomLog ${APACHE_LOG_DIR}/projeto1_access.log combined
</VirtualHost>
```

**Habilitar o site:**

```bash
sudo a2ensite projeto1.com.conf
sudo systemctl reload apache2
```

### Alternativa com Nginx

O Nginx é uma alternativa mais eficiente para alto tráfego:[^13][^14]

```bash
# Instalar Nginx
sudo apt install nginx

# Habilitar no firewall
sudo ufw allow 'Nginx Full'

# Gerenciar o serviço
sudo systemctl start nginx
sudo systemctl enable nginx
```

## Docker para Multiprojetos

### Instalação do Docker

Para maior flexibilidade na hospedagem de multiprojetos, instale o Docker:[^15][^16]

```bash
# Instalar dependências
sudo apt install ca-certificates curl gnupg lsb-release

# Adicionar chave GPG oficial do Docker
sudo mkdir -p /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg

# Adicionar repositório
echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

# Instalar Docker
sudo apt update
sudo apt install docker-ce docker-ce-cli containerd.io docker-compose-plugin

# Adicionar usuário ao grupo docker
sudo usermod -aG docker $USER
```

### Docker Compose para Multiprojetos

Use Docker Compose para gerenciar múltiplos projetos facilmente:[^17]

```yaml
# docker-compose.yml
version: '3.8'
services:
  webapp1:
    image: nginx:alpine
    ports:
      - "8001:80"
    volumes:
      - ./projeto1:/usr/share/nginx/html

  webapp2:
    image: nginx:alpine
    ports:
      - "8002:80"
    volumes:
      - ./projeto2:/usr/share/nginx/html

  database:
    image: mysql:8.0
    environment:
      MYSQL_ROOT_PASSWORD: senha_segura
    ports:
      - "3306:3306"
```

## Armazenamento de Arquivos

### Servidor Samba para Compartilhamento

Configure um servidor de arquivos usando Samba:[^18][^19]

```bash
# Instalar Samba
sudo apt install samba

# Criar diretório compartilhado
sudo mkdir /mnt/arquivos
sudo chown nobody:nogroup /mnt/arquivos

# Configurar Samba
sudo nano /etc/samba/smb.conf
```

**Adicionar ao final do arquivo:**

```ini
[arquivos]
    comment = Arquivos Compartilhados
    path = /mnt/arquivos
    browseable = yes
    read only = no
    guest ok = yes
    create mask = 0777
    directory mask = 0777
```

**Reiniciar Samba:**

```bash
sudo systemctl restart smbd
sudo ufw allow samba
```

### Backup Automático

Configure backups automáticos usando rsync e cron:[^20][^21]

```bash
# Criar script de backup
sudo nano /usr/local/bin/backup.sh
```

**Conteúdo do script:**

```bash
#!/bin/bash
BACKUP_DIR="/backup"
SOURCE_DIR="/var/www"
DATE=$(date +%Y-%m-%d_%H-%M-%S)

# Criar backup incremental
rsync -av --delete $SOURCE_DIR $BACKUP_DIR/backup_$DATE/

# Manter apenas últimos 7 backups
find $BACKUP_DIR -type d -name "backup_*" -mtime +7 -exec rm -rf {} \;
```

**Tornar executável e agendar:**

```bash
sudo chmod +x /usr/local/bin/backup.sh

# Adicionar ao crontab para executar diariamente às 2h
sudo crontab -e
# Adicionar linha: 0 2 * * * /usr/local/bin/backup.sh
```

## Acesso Remoto de Qualquer Lugar

### Configuração de DNS

Para acesso remoto de qualquer lugar, configure um nome de domínio apontando para seu IP público. Se usar IP dinâmico, configure um serviço de DNS dinâmico.[^22][^23]

### Certificados SSL/TLS

Use Let's Encrypt para HTTPS gratuito:

```bash
# Instalar Certbot
sudo apt install certbot python3-certbot-apache

# Obter certificado
sudo certbot --apache -d seudominio.com

# Renovação automática
sudo crontab -e
# Adicionar: 0 12 * * * /usr/bin/certbot renew --quiet
```

### Considerações de Segurança

1. **Mantenha o sistema atualizado:**

```bash
sudo apt update && sudo apt upgrade
```

2. **Configure fail2ban para proteção contra ataques de força bruta:**

```bash
sudo apt install fail2ban
```

3. **Use autenticação por chaves SSH** em vez de senhas[^4]
4. **Monitore logs regularmente:**

```bash
sudo tail -f /var/log/auth.log
```

5. **Configure backups offsite** para proteção adicional[^20]

## Conclusão

Com essa configuração, você terá um servidor Ubuntu Server 22.04 LTS robusto e seguro, capaz de hospedar múltiplos projetos web/ML/IA, com acesso remoto seguro via SSH e HTTP/HTTPS, além de capacidades de armazenamento e backup de arquivos. O servidor poderá ser acessado remotamente de qualquer lugar do mundo, mantendo altos padrões de segurança através do firewall UFW e outras medidas de proteção implementadas.

<div style="text-align: center">⁂</div>

[^1]: https://tiparaleigo.wordpress.com/2022/05/11/como-instalar-o-ubuntu-server-22-04-lts-passo-a-passo/

[^2]: https://ubuntu.com/download/server

[^3]: https://www.youtube.com/watch?v=oRjGHtZeEno

[^4]: https://guidetux.com.br/como-instalar-ssh-ubuntu-24-04-22-04-ou-20-04/

[^5]: https://www.edivaldobrito.com.br/como-habilitar-o-servico-ssh-no-ubuntu-22-04-lts/

[^6]: https://www.digitalocean.com/community/tutorials/how-to-set-up-a-firewall-with-ufw-on-ubuntu-20-04-pt

[^7]: https://www.hostinger.com/br/tutoriais/firewall-ubuntu-ufw

[^8]: https://www.locaweb.com.br/ajuda/wiki/firewall-ubuntu/

[^9]: https://othhost.com.br/como-criar-um-servidor-web-com-apache-ou-nginx-em-uma-vps-passo-a-passo-para-rodar-sites-e-aplicacoes/

[^10]: https://www.digitalocean.com/community/tutorials/how-to-configure-apache-http-with-mpm-event-and-php-fpm-on-ubuntu-18-04-pt

[^11]: https://brasilcloud.com.br/tutorial/como-configurar-apache-virtual-hosts-multi-dominio-no-ubuntu-14-04-lts/

[^12]: https://www.atlantic.net/vps-hosting/host-multiple-websites-on-a-single-server-with-apache-on-ubuntu/

[^13]: https://www.digitalocean.com/community/tutorials/how-to-configure-nginx-as-a-web-server-and-reverse-proxy-for-apache-on-one-ubuntu-18-04-server-pt

[^14]: https://www.edivaldobrito.com.br/servidor-nginx-no-ubuntu/

[^15]: https://kinsta.com/pt/blog/instalacao-docker-no-ubuntu/

[^16]: https://plus.diolinux.com.br/t/como-instalar-docker-no-ubuntu-server-guia-rapido/55581

[^17]: https://www.youtube.com/watch?v=7Y5ZEmDZlNA

[^18]: https://www.dio.me/articles/servidores-de-arquivos-com-linux-introducao-ao-servidor-de-arquivos

[^19]: https://www.youtube.com/watch?v=jn4idPyCzvE

[^20]: https://blog.storagehouse.com.br/post/como-fazer-backup-de-servidor-linux-ubuntu

[^21]: https://www.storcenter.com.br/post/como-fazer-backup-de-um-servidor-ubuntu

[^22]: https://serverspace.com.br/support/help/bind9-as-a-primary-dns-server-on-ubuntu/

[^23]: https://coffops.com/como-alterar-servidor-de-dns-no-ubuntu-20-04-via-terminal/

[^24]: https://www.youtube.com/watch?v=qnMol8-wvq8

[^25]: https://aprendendoprogramar.com.br/tutoriais/linux/02_config_ssh_ubuntu/

[^26]: https://brunocosta.dev/instalar-e-ativar-o-ssh-no-linux/

[^27]: https://releases.ubuntu.com/jammy

[^28]: https://kinsta.com/pt/blog/nginx-vs-apache/

[^29]: https://ubuntu.com/server/docs/service-openssh

[^30]: https://intranet.tecsolve.com.br/knowledge-base/article/como-configurar-o-nginx-para-apache-no-ubuntu

[^31]: https://www.youtube.com/watch?v=XNys1t06jyU

[^32]: https://www.vivaolinux.com.br/dica/Instalando-e-configurando-servidor-SSH-(Ubuntu)

[^33]: https://www.digitalocean.com/community/tutorials/how-to-install-the-apache-web-server-on-ubuntu-20-04-pt

[^34]: https://www.youtube.com/watch?v=EnvbteTXZGE

[^35]: https://www.youtube.com/watch?v=-cforvm_oV0

[^36]: https://expressvps.com.br/como-configurar-um-ambiente-de-desenvolvimento-remoto-com-vps-guia-pratico/

[^37]: https://blog.rubfy.com.br/acessar-vps-windows/

[^38]: https://bhservers.com.br/cliente/knowledgebase/155/Como-abrir-porta-no-Firewall-do-Ubuntu-com-facilidade.html

[^39]: https://adiantiframework.com.br/forum/view_7397

[^40]: https://othhost.com.br/como-acessar-servidor-vps/

[^41]: https://www.youtube.com/watch?v=INbfIShsRr0

[^42]: https://www.homehost.com.br/blog/vps/como-acessar-um-servidor-vps/

[^43]: https://gist.github.com/fabianogoes/cbfd62664965fbd87e70

[^44]: https://www.reddit.com/r/networking/comments/17cn3dm/adding_an_ubuntu_fileapp_server_to_a_windows/?tl=pt-br

[^45]: https://www.locaweb.com.br/ajuda/wiki/como-acessar-o-vps-windows-2016-via-conexao-de-area-de-trabalho-remota-rdc/

[^46]: https://www.digitalocean.com/community/tutorials/how-to-use-ssh-to-connect-to-a-remote-server-pt

[^47]: https://docs.lacunasoftware.com/pt-br/articles/amplia-reg/on-premises/linux/install-ubuntu.html

[^48]: https://bhservers.com.br/cliente/knowledgebase/127/Como-acessar-o-VPS-Windows-desde-um-PC-com-Windows.html

[^49]: https://belenos.me/blog/ssh-ubuntu-debian/

[^50]: https://www.youtube.com/watch?v=130Xdztvm0I

[^51]: https://www.macoratti.net/19/10/net_climp1.htm

[^52]: https://www.youtube.com/watch?v=AjRVR9Wjnto

[^53]: https://arthurpedroti.com.br/criando-um-servidor-ubuntu-no-docker/

[^54]: https://www.digitalocean.com/community/tutorials/how-to-configure-bind-as-a-private-network-dns-server-on-ubuntu-18-04-pt

[^55]: https://www.reddit.com/r/linux4noobs/comments/1j86315/cant_access_multiple_linux_websites/?tl=pt-br

[^56]: https://www.youtube.com/watch?v=0SSSfyy7bO4

[^57]: https://pt.stackoverflow.com/questions/439849/ngix-com-multiplos-projetos-angular

[^58]: https://www.digitalocean.com/community/tutorials/how-to-install-and-use-docker-on-ubuntu-20-04-pt

[^59]: https://serverspace.com.br/support/help/bind9-as-a-secondary-dns-server-on-ubuntu/

[^60]: https://viniciuspaes.com/linux/tutorial-instalar-configurar-servidor-web-http-ubuntu-apache-mysql-php-ftp-tls-php-fpm/

[^61]: https://masterdaweb.com/blog/como-instalar-e-subir-um-container-com-docker-no-ubuntu-server-24-04

[^62]: https://king.host/wiki/artigo/como-alterar-o-dns-recursivo-no-ubuntu/

[^63]: https://www.reddit.com/r/Ubuntu/comments/1fkkska/2404_lts_multiple_session_rdp_where_is_the_rdp/?tl=pt-br

[^64]: https://www.youtube.com/watch?v=56m308eEMZQ

[^65]: https://serverspace.com.br/support/help/backup-ubuntu-server-20-04-with-bacula/

[^66]: https://www.vinchin.com/pt/linux-backup/backup-e-restore-servidor-linux.html

[^67]: https://diolinux.com.br/tutoriais/servidor-de-arquivos-na-nuvem-nextcloud.html

[^68]: https://www.datastorage.com.br/post/backup-de-ubuntu-linux

[^69]: https://www.reddit.com/r/HomeServer/comments/sp7gjl/storage_configuration_ubuntu_server_nas_mergerfs/?tl=pt-br

[^70]: https://help.ubuntu.com/stable/ubuntu-help/backup-how.html.pt-BR

[^71]: https://www.youtube.com/watch?v=wlrYEsT5MrQ

[^72]: https://www.youtube.com/watch?v=St7vd-Iw5pg

[^73]: https://plus.diolinux.com.br/t/backup-do-ubuntu/60850

[^74]: https://documentation.ubuntu.com/server/how-to/security/firewalls/

[^75]: https://www.reddit.com/r/HomeServer/comments/1kyq0aq/ubuntu_server_nas_best_practices/?tl=pt-br

[^76]: https://www.reddit.com/r/selfhosted/comments/1bro9r7/backing_up_data_on_ubuntu_server/?tl=pt-br
