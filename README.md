
# Configurando EC2, Anexando Volume EBS e Criando Arquivo nele via SSH

Este tutorial cobre o processo de configuração de uma instância EC2, anexação de um volume EBS, montagem do volume, e criação de um arquivo nele via SSH.

## Pré-requisitos

- Conta na AWS
- Chave PEM para acesso SSH
- Cliente SSH (Terminal, PuTTY, etc.)

## Passo 1: Configuração da Instância EC2

1. **Acessar o Console da AWS**
   - Acesse [AWS Management Console](https://aws.amazon.com/console/) e faça login.

2. **Navegar até o Serviço EC2**
   - No painel da AWS, procure por "EC2" e selecione o serviço.

3. **Criar uma Nova Instância EC2**
   - Clique em "Launch Instance" (Iniciar Instância).
   - Dê um nome à instância.
   - Selecione a AMI "Amazon Linux 2".
   - Escolha o tipo de instância "t2.micro".
   - Crie ou selecione uma chave PEM para acesso SSH.
   - Configure a rede e selecione "Enable" para "Auto-assign Public IP".
   - Crie ou selecione um grupo de segurança que permita acesso SSH na porta 22.
   - Clique em "Launch Instance".

## Passo 2: Conectar à Instância via SSH

1. **Modificar Permissões da Chave PEM**
   ```sh
   chmod 400 chave-nova2.pem
   ```

2. **Conectar à Instância**
   ```sh
   ssh -i "chave.pem" ec2-user@seu-endereco-ipv4-publico
   ```

## Passo 3: Anexar e Montar um Volume EBS

1. **Criar um Novo Volume EBS**
   - No console da AWS, vá para "Elastic Block Store" > "Volumes".
   - Clique em "Create Volume".
   - Selecione o tamanho e a zona de disponibilidade (mesma da instância EC2).
   - Clique em "Create".

2. **Anexar o Volume à Instância**
   - Selecione o volume EBS criado.
   - Clique em "Actions" > "Attach Volume".
   - Selecione a instância EC2 e clique em "Attach".

3. **Formatar e Montar o Volume**
   - Liste os dispositivos de bloco:
     ```sh
     lsblk
     ```
   - Formate o volume (se necessário):
     ```sh
     sudo mkfs -t ext4 /dev/xvdf
     ```
   - Crie um ponto de montagem e monte o volume:
     ```sh
     sudo mkdir /mnt/ebs-volume
     sudo mount /dev/xvdf /mnt/ebs-volume
     ```

## Passo 4: Criar e Editar um Arquivo

1. **Navegar até o Diretório Montado**
   ```sh
   cd /mnt/ebs-volume
   ```

2. **Criar um Arquivo de Texto**
   ```sh
   echo "Olá, Banda do Miguel\!" > arquivo.txt
   ```

3. **Listar o Conteúdo do Diretório**
   ```sh
   ls
   ```

4. **Verificar o Conteúdo do Arquivo**
   ```sh
   cat arquivo.txt
   ```

## Passo 5: Encerrar a Instância

1. **Encerrar a Instância**
   - No console EC2, selecione a instância.
   - Clique em "Instance State" > "Terminate Instance".

## Passo 6: Eliminar o Volume EBS

1. **Desmontar o Volume**
   ```sh
   sudo umount /mnt/ebs-volume
   ```

2. **Excluir o Volume EBS**
   - No console da AWS, vá para "Elastic Block Store" > "Volumes".
   - Selecione o volume EBS que você criou.
   - Clique em "Actions" > "Delete Volume" (Excluir Volume).
   - Confirme a exclusão.

## Conclusão

NÃO SE ESQUEÇA DE EXCLUIR O VOLUME EBS NOVO!!!!!
