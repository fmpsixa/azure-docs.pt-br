---
title: "Criar um par de chaves SSH para máquinas virtuais Linux no Azure | Microsoft Docs"
description: "Crie um par de chaves SSH pública e privada para VMs Linux do Azure com segurança."
services: virtual-machines-linux
documentationcenter: 
author: vlivech
manager: timlt
editor: 
tags: 
ms.assetid: 34ae9482-da3e-4b2d-9d0d-9d672aa42498
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: get-started-article
ms.date: 03/08/2017
ms.author: rasquill
experiment_id: rasquill-ssh-20170308
translationtype: Human Translation
ms.sourcegitcommit: eeb56316b337c90cc83455be11917674eba898a3
ms.openlocfilehash: 19acd4efca7ef043f31b436b96f9129caee9591b
ms.lasthandoff: 04/03/2017



---

# <a name="create-an-ssh-public-and-private-key-pair-for-linux-vms"></a>Criar um par de chaves SSH pública e privada para VMs Linux

Este artigo mostra como gerar um par de arquivos de chave pública e privada RSA do protocolo SSH versão 2 para uso com VMs Linux.  Com um par de chaves SSH, você pode criar máquinas virtuais no Azure que tenham como padrão o uso de chaves SSH para autenticação, eliminando a necessidade de senhas para fazer logon.  As senhas podem ser adivinhadas e podem abrir suas VMs para tentativas de uso contínuo de força bruta para adivinhá-las. As VMs criadas com os Modelos do Azure ou com o `azure-cli` podem incluir sua chave pública SSH como parte da implantação, removendo uma etapa de configuração pós-implantação que consiste na desabilitação de logons com senha para SSH.

## <a name="quick-commands"></a>Comandos rápidos

Execute os comandos a seguir de um shell Bash, substituindo os exemplos por suas próprias escolhas.

O arquivo de chave pública SSH é criado por padrão em `~/.ssh/id_rsa.pub`. Quando receber a solicitação para usar o comando a seguir, crie uma "senha" para proteger sua chave privada. (A senha é usada para criptografar a chave privada.)

```bash
ssh-keygen -t rsa -b 2048 
```

Adicione a chave recém-criada em `ssh-agent`:

```bash
ssh-add ~/.ssh/id_rsa
```

> [!IMPORTANT] 
> Os comandos acima funcionam em sistemas operacionais Linux de quase todas as distribuições, mas não funcionam, necessariamente, em contêineres, pois o ambiente pode ser radicalmente restrito. 

## <a name="detailed-walkthrough"></a>Passo a passo detalhado

Usar as chaves públicas e privadas do SSH é a maneira mais fácil de fazer logon em servidores Linux. [criptografia de chave pública](https://en.wikipedia.org/wiki/Public-key_cryptography) fornece uma maneira muito mais segura de fazer logon na VM BSD ou do Linux no Azure do que as senhas, que podem ser obtidas por força bruta muito mais facilmente.

> [!IMPORTANT]
> Sua chave pública pode ser compartilhada com qualquer pessoa; mas apenas você (ou sua infraestrutura de segurança local) possui sua chave privada.  A chave privada SSH deve ter uma [senha bastante segura](https://www.xkcd.com/936/)(fonte:[xkcd.com](https://xkcd.com)) para protegê-la.  Esta senha é apenas para acessar a chave privada do SSH e **não é** a senha da conta de usuário.  Quando você adiciona uma senha para a chave SSH, ela criptografa a chave privada usando AES de 128 bits, para que a chave privada seja inútil sem a senha para descriptografá-la.  Se um invasor roubasse sua chave privada e se ela não tivesse uma senha, ele poderia usar essa chave privada para fazer logon em qualquer servidor com a chave pública correspondente.  Se uma chave privada for protegida por senha, ela não poderá ser usada por esse invasor, o que é uma camada adicional de segurança para sua infraestrutura no Azure.

Este artigo cria arquivos de chave pública e privada RSA do protocolo SSH versão 2, recomendados para implantações no Resource Manager.  Chaves *SSH-rsa* são necessárias no [portal](https://portal.azure.com) para implantações clássicas e do Gerenciador de Recursos.

## <a name="disable-ssh-passwords-by-using-ssh-keys"></a>Desabilitar senhas SSH usando chaves SSH

O Azure requer chaves públicas e privadas de pelo menos 2048 bits em formato ssh-rsa. Para criar as chaves, use `ssh-keygen`, que faz uma série de perguntas e, em seguida, grava uma chave privada e uma chave pública correspondente. Quando uma VM do Azure é criada, a chave pública é copiada para `~/.ssh/authorized_keys`.  As chaves SSH no `~/.ssh/authorized_keys` são usadas para desafiar o cliente para coincidir com a chave privada correspondente em uma conexão de logon SSH.  Quando uma VM Linux do Azure é criada usando chaves SSH para autenticação, o Azure configura o servidor SSHD para não permitir logons com senha, apenas com chaves SSH.  Portanto, ao criar VMs Linux do Azure com chaves SSH, você pode ajudar a proteger a implantação de VM e evitar a etapa de configuração pós-implantação típica de desabilitar as senhas no arquivo de configuração sshd_config.

## <a name="using-ssh-keygen"></a>Usando ssh-keygen

Esse comando cria um par de chaves SSH (criptografado) protegido por senha usando o RSA de 2048 bits e é comentado para identificá-lo facilmente.  

As chaves SSH são mantidas por padrão no diretório `~/.ssh`.  Se você não tiver um diretório `~/.ssh`, o comando `ssh-keygen` o criará para você com as permissões corretas.

```bash
ssh-keygen \
-t rsa \
-b 2048 
```

*Comando explicado*

`ssh-keygen` = programa usado para criar as chaves

`-t rsa` = tipo de chave a ser criada, que é o formato RSA [wikipedia](https://en.wikipedia.org/wiki/RSA_(cryptosystem)

`-b 2048` = bits da chave


## <a name="classic-portal-and-x509-certs"></a>Portal clássico e certificados x.509

Se você está usando o [portal clássico](https://manage.windowsazure.com/) do Azure, ele requer certificados x.509 para as chaves SSH.  Outros tipos de chaves públicas SSH não são permitidos. Elas *devem* ser certificados x.509.

Para criar um certificado x.509 de sua chave privada do SSH-RSA existente:

```bash
openssl req -x509 \
-key ~/.ssh/id_rsa \
-nodes \
-days 365 \
-newkey rsa:2048 \
-out ~/.ssh/id_rsa.pem
```

## <a name="classic-deploy-using-asm"></a>Implantação clássica usando `asm`

Se estiver usando o modelo de implantação clássica (CLI de gerenciamento de serviços do Azure `asm`), você poderá usar uma chave pública SSH-RSA ou uma chave formatada como RFC4716 em um contêiner **.pem**.  A chave pública SSH-RSA é a que foi criada anteriormente neste artigo usando `ssh-keygen`.

Para criar uma chave formatada RFC4716 com base em uma chave pública SSH existente:

```bash
ssh-keygen \
-f ~/.ssh/id_rsa.pub \
-e \
-m RFC4716 > ~/.ssh/id_ssh2.pem
```

## <a name="example-of-ssh-keygen"></a>Exemplo de ssh-keygen

```bash
ssh-keygen -t rsa -b 2048 -C "ahmet@myserver"
Generating public/private rsa key pair.
Enter file in which to save the key (/home/ahmet/.ssh/id_rsa): 
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in /home/ahmet/.ssh/id_rsa.
Your public key has been saved in /home/ahmet/.ssh/id_rsa.pub.
The key fingerprint is:
14:a3:cb:3e:78:ad:25:cc:55:e9:0c:08:e5:d1:a9:08 ahmet@myserver
The keys randomart image is:
+--[ RSA 2048]----+
|        o o. .   |
|      E. = .o    |
|      ..o...     |
|     . o....     |
|      o S =      |
|     . + O       |
|      + = =      |
|       o +       |
|        .        |
+-----------------+
```

Arquivos de chave salvos:

`Enter file in which to save the key (/home/ahmet/.ssh/id_rsa): ~/.ssh/id_rsa`

O nome do par de chaves para este artigo.  Ter um par de chaves denominado **id_rsa** é o padrão e algumas ferramentas podem esperar o nome de arquivo da chave privada **id_rsa**, portanto, ter um é uma boa ideia. O diretório `~/.ssh/` é o local padrão para os pares de chave SSH e o arquivo de configuração SSH.  Se não for especificado com um caminho completo, `ssh-keygen` criará as chaves no diretório de trabalho atual e não o `~/.ssh` padrão.

Uma listagem do diretório `~/.ssh` .

```bash
ls -al ~/.ssh
-rw------- 1 ahmet staff  1675 Aug 25 18:04 id_rsa
-rw-r--r-- 1 ahmet staff   410 Aug 25 18:04 rsa.pub
```

Senha da chave:

`Enter passphrase (empty for no passphrase):`

`ssh-keygen` refere-se a uma senha usada para criptografar a chave privada como "senha".  É *altamente* recomendável adicionar uma senha a seus pares de chave. Sem uma senha para proteger a chave privada, qualquer pessoa com o arquivo da chave pode usá-lo para fazer logon em qualquer servidor com a chave pública correspondente. Portanto, adicionar uma senha oferece mais proteção no caso de alguém obter acesso a seu arquivo de chave privada, dando-lhe tempo para alterar as chaves usadas para autenticá-lo.

## <a name="using-ssh-agent-to-store-your-private-key-password"></a>Usando ssh-agent para armazenar sua senha de chave privada

Para evitar a digitação da senha do arquivo de chave privada em cada logon do SSH, você poderá usar `ssh-agent` para armazenar em cache a senha do arquivo de chave privada. Se você estiver usando um Mac, a cadeia de chaves do OSX armazena com segurança suas senhas de chave privadas quando `ssh-agent`é chamado.

Verifique e use `ssh-agent` e `ssh-add` para informar o sistema SSH sobre os arquivos de chave, para que a senha não precise ser usada interativamente.

```bash
eval "$(ssh-agent -s)"
```

Agora, adicione a chave privada ao `ssh-agent` usando o comando `ssh-add`.

```bash
ssh-add ~/.ssh/id_rsa
```

Agora, a senha da chave privada é armazenada no `ssh-agent`.

## <a name="using-ssh-copy-id-to-install-the-new-key"></a>Usar `ssh-copy-id` para instalar a nova chave
Se você já tiver criado uma VM, instale a nova chave pública SSH para sua VM Linux com o seguinte comando, substituindo o nome de usuário da VM e o endereço do servidor por seus próprios valores:

```bash
ssh-copy-id -i ~/.ssh/id_rsa.pub ahmet@myserver
```

## <a name="create-and-configure-an-ssh-config-file"></a>Criar e configurar um arquivo de configuração do SSH

É uma prática recomendada criar e configurar um arquivo `~/.ssh/config` para acelerar os logons e otimizar o comportamento do seu cliente SSH.

O exemplo a seguir mostra uma configuração padrão.

### <a name="create-the-file"></a>Criar o arquivo

```bash
touch ~/.ssh/config
```

### <a name="edit-the-file-to-add-the-new-ssh-configuration"></a>Edite o arquivo a fim de adicionar a nova configuração de SSH:

```bash
vim ~/.ssh/config
```

### <a name="example-sshconfig-file"></a>Exemplo de arquivo `~/.ssh/config`:

```bash
# Azure Keys
Host fedora22
  Hostname 102.160.203.241
  User ahmet
# ./Azure Keys
# Default Settings
Host *
  PubkeyAuthentication=yes
  IdentitiesOnly=yes
  ServerAliveInterval=60
  ServerAliveCountMax=30
  ControlMaster auto
  ControlPath ~/.ssh/SSHConnections/ssh-%r@%h:%p
  ControlPersist 4h
  IdentityFile ~/.ssh/id_rsa
```

Essa configuração de SSH fornece seções para cada serviço para habilitar cada um deles com seu próprio par de chaves dedicado. As configurações padrão (`Host *`) são para quaisquer hosts que não correspondam a qualquer um dos hosts específicos acima no arquivo de configuração.

### <a name="config-file-explained"></a>Arquivo de configuração explicado

`Host` = o nome do host sendo chamado no terminal.  `ssh fedora22` instrui `SSH` a usar os valores no bloco de configurações rotulado como `Host fedora22` OBSERVAÇÃO: o host pode ser qualquer rótulo lógico para o uso e não representa o nome de host real de qualquer servidor.

`Hostname 102.160.203.241` = o endereço IP ou o nome DNS do servidor acessado.

`User ahmet` = a conta de usuário remoto a ser usada ao fazer logon no servidor.

`PubKeyAuthentication yes` = instrui o SSH desejado a usar uma chave SSH para fazer logon.

`IdentityFile /home/ahmet/.ssh/id_id_rsa` = a chave privada SSH e a chave pública correspondente a serem usadas para autenticação.

## <a name="ssh-into-linux-without-a-password"></a>SSH no Linux sem uma senha

Agora que tem um par de chaves SSH e um arquivo de configuração do SSH configurado, você pode fazer logon na VM do Linux de forma rápida e segura. Na primeira vez que você fizer logon em um servidor usando uma chave SSH, o comando solicitará a senha para esse arquivo de chave.

```bash
ssh fedora22
```

### <a name="command-explained"></a>Comando explicado

Quando `ssh fedora22` é executado, primeiro o SSH localiza e carrega todas as configurações do bloco `Host fedora22`, em seguida, carrega todas as configurações restantes a partir do último bloco, `Host *`.

## <a name="next-steps"></a>Próximas etapas

A próxima etapa é criar VMs do Linux do Azure usando a nova chave pública SSH.  As VMs do Azure criadas com uma chave pública SSH como o logon estão mais protegidas do que as VMs criadas com as senhas do método de logon padrão.  As VMs do Azure criadas com chaves SSH são por padrão configuradas com senhas desabilitadas, evitando tentativas de adivinhação por força bruta. Se você precisar de mais ajuda com a criação de seu par de chaves SSH, ou precisar de outros certificados, por exemplo, para uso com o portal clássico, confira [Etapas detalhadas para criar pares de chave SSH e certificados](create-ssh-keys-detailed.md).

* [Criar uma VM do Linux segura usando um modelo do Azure](create-ssh-secured-vm-from-template.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Criar uma VM Linux segura usando o Portal do Azure](quick-create-portal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Criar uma VM Linux segura usando a CLI do Azure](quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

