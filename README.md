# passo-a-passo-vm

# Guia de Instalação do Debian no Oracle VirtualBox

Este guia irá ensinar como instalar o **Debian** em uma máquina virtual no **Oracle VirtualBox** e configurá-lo para atuar como um servidor SSH, com **Apache**, **PHP**, **MySQL**, e **phpMyAdmin**.

## Requisitos

- **Oracle VirtualBox** instalado.
- **Imagem ISO do Debian** (pode ser baixada em [Debian Downloads](https://www.debian.org/download)).

---

## Passo 1: Criação da Máquina Virtual no Oracle VirtualBox

1. **Abrir o Oracle VirtualBox**.
2. **Criar uma nova máquina virtual**:
   - Clique em **Novo**.
   - **Nome**: Escolha um nome para a máquina virtual (ex: Debian-Server). Evite usar número 0. Exemplo Linux01.
   - **Tipo**: Selecione **Linux** e **Versão**: **Debian (64-bit)**.
3. **Configuração de Memória**:
   - A memória recomendada para o Debian é **1GB ou mais**. Ajuste conforme o desempenho desejado.
4. **Criar um Disco Rígido Virtual**:
   - Selecione **Criar um disco rígido virtual agora**.
   - Escolha o tipo de arquivo **VDI (VirtualBox Disk Image)**.
   - Defina a **tamanho do disco rígido** (recomendado: **20 GB ou mais**).
   - Escolha a opção **Dinamicamente alocado**.
5. **Finalizar a criação da máquina virtual**.

---

## Passo 2: Configurar a Máquina Virtual

1. **Selecionar a máquina virtual** criada e clicar em **Configurações**.
2. **Configuração de Sistema**:
   - Na guia **Sistema**, deixe a opção **Floppy** desmarcada (não é necessário).
   - Acesse a guia **Processador** e configure o número de núcleos de CPU (recomendado: **2 núcleos**).
3. **Configuração de Armazenamento**:
   - Na guia **Armazenamento**, selecione **Vazio** na árvore de armazenamento.
   - Clique no ícone de **disquete** à direita e selecione a imagem ISO do Debian.
4. **Configuração de Rede**:
   - Na guia **Rede**, selecione **Adaptador 1** como **Conectado à Rede NAT** (para permitir a conexão com a internet).
   - Se necessário, configure como **Rede interna** ou **Bridge** para diferentes tipos de acesso de rede.

---

## Passo 3: Instalação do Debian

1. **Iniciar a Máquina Virtual**:
   - Clique em **Iniciar** para iniciar a instalação do Debian.
2. **Instalação do Debian**:
   - Selecione o idioma, a localização, e o layout do teclado.
   - Configure o nome da máquina e o domínio.
   - Configure a senha do **root**.
   - Crie um **usuário** normal.
   - Prossiga com a instalação do sistema base.
3. **Particionamento**:
   - Selecione **Usar o disco inteiro** para o particionamento automático.
   - Selecione **GUI** ou **Ambiente de trabalho** para a instalação do sistema.
4. **Escolha do Espelho de Pacotes**:
   - Selecione o espelho de pacotes mais próximo para baixar os pacotes necessários.
5. **Instalar o Grub**:
   - Selecione **sim** para instalar o carregador de inicialização **GRUB**.

Após a instalação ser concluída, o Debian será iniciado. Por padrão no Oracle Virtual Box 7 tem uma opção que cria o usuário vboxuser com senha changeme e usuário root com mesma senha

---

## Passo 4: Atualizar o Sistema

1. Após o login inicial, atualize os pacotes com os seguintes comandos:

   ```bash
   sudo apt update
   sudo apt upgrade
   ```	


2. Reinicie o sistema:
   ```bash
   sudo reboot
   ```

---

## Passo 5: Configuração do Servidor SSH

1. **Instalar o OpenSSH Server**:
   ```bash
   sudo apt install openssh-server
   ```

2. **Verificar o status do serviço SSH**:
   ```bash
   sudo systemctl status ssh
   ```

3. **Habilitar o SSH para iniciar automaticamente**:
   ```bash
   sudo systemctl enable ssh
   ```

4. **Verificar o IP da Máquina Virtual**:
   ```bash
   ip a
   ```
Ou 
   ```bash
   curl ifconfig.me
   ```
Ou 
   ```bash
   ifconfig
   ```

---

## Passo 6: Instalar o Apache com PHP

1. **Instalar o Apache**:
   ```bash
   sudo apt install apache2
   ```

2. **Instalar o PHP**:
   ```bash
   sudo apt install php libapache2-mod-php
   ```

3. **Testar o Apache e PHP**:
   - Crie um arquivo de teste em `/var/www/html/info.php`:
   ```bash
     sudo nano /var/www/html/info.php
   ```

   - Adicione o seguinte conteúdo:
   ```bash
     <?php
     phpinfo();
     ?>
   ```

   - Acesse no navegador: `http://<IP-da-VirtualBox>/info.php`. Você deve ver a página de informações do PHP.

---

## Passo 7: Instalar o MySQL

1. **Instalar o MySQL Server**:
   ```bash
   sudo apt install mysql-server
   ```

2. **Segurança do MySQL**:
   ```bash
   sudo mysql_secure_installation
   ```

3. **Verificar o status do MySQL**:
   ```bash
   sudo systemctl status mysql
   ```

---

## Passo 8: Instalar o phpMyAdmin

1. **Instalar o phpMyAdmin**:
   ```bash
   sudo apt install phpmyadmin
   ```

2. Durante a instalação, selecione o **Apache2** como servidor web e configure o banco de dados com **dbconfig-common**.

3. **Ativar o phpMyAdmin no Apache**:
   ```bash
   sudo ln -s /usr/share/phpmyadmin /var/www/html/phpmyadmin
   ```

4. **Reiniciar o Apache**:
   ```bash
   sudo systemctl restart apache2
   ```

---

## Passo 9: Acessar o phpMyAdmin

- Abra um navegador e acesse: `http://<IP-da-VirtualBox>/phpmyadmin`.
- Faça login com o **usuário root** ou qualquer outro usuário configurado no MySQL.
  
Para simplificar o trabalho podemos fazer o redirecionamento de Portas

 Rule | Protocolo | IP Hospedeiro/Host | Porta Hospedeiro | IP Convidado | Porta Convidado |
|------|-----------|--------------------|------------------|--------------|-----------------|
| 1    | TCP       | 127.0.0.1          | 223333           | 10.0.2.15    | 22              |
| 2    | TCP       | 127.0.0.1          | 8080             | 10.0.2.15    | 80              |
| 3    | TCP       | 127.0.0.1          | 330777           | 10.0.2.15    | 3306            |
  

