---
title: 'Saiba mais: gerenciamento de senhas do Azure Active Directory | Microsoft Docs'
description: "Tópicos avançados sobre o gerenciamento de senha do Azure AD, incluindo como funciona o write-back de senha, segurança de write-back de senha, como funciona o portal de redefinição de senha e quais dados são usados na redefinição de senha."
services: active-directory
documentationcenter: 
author: MicrosoftGuyJFlo
manager: femila
editor: curtand
ms.assetid: d3be2912-76c8-40a0-9507-b7b1a7ce2f8f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/28/2017
ms.author: joflore
translationtype: Human Translation
ms.sourcegitcommit: 07635b0eb4650f0c30898ea1600697dacb33477c
ms.openlocfilehash: dca6f5189693fc98cec4f92eac81b6985e691889
ms.lasthandoff: 03/28/2017


---
# <a name="learn-more-about-password-management"></a>Saiba mais sobre Gerenciamento de Senha
> [!IMPORTANT]
> **Você está aqui por que está enfrentando problemas para iniciar sessão?** Se sim, [veja aqui como alterar e redefinir sua senha](active-directory-passwords-update-your-own-password.md#reset-your-password).
>
>

Se você já tiver implantado o Gerenciamento de Senha ou estiver somente procurando saber mais sobre a parte técnica antes de implantá-lo, esta seção lhe dará uma boa visão geral dos conceitos técnicos por trás do serviço. Abordaremos o seguinte:

* [**Como funciona o portal de redefinição de senha?**](#how-does-the-password-reset-portal-work)
* [**Visão geral de write-back de senha**](#password-writeback-overview)
 * [Como funciona o write-back de senha](#how-password-writeback-works)
* [**Cenários com suporte para write-back de senha**](#scenarios-supported-for-password-writeback)
 * [Cliente Azure AD Connect, Azure AD Sync e DirSync com suporte](#supported-clients)
 * [Licenças necessárias para write-back de senha](#licenses-required-for-password-writeback)
 * [Modos de autenticação local com suporte para write-back de senha](#on-premises-authentication-modes-supported-for-password-writeback)
 * [Operações do Usuário e do Administrador com suporte para write-back de senha](#user-and-admin-operations-supported-for-password-writeback)
 * [Operações do Usuário e do Administrador sem suporte para write-back de senha](#user-and-admin-operations-not-supported-for-password-writeback)
* [**Modelo de segurança de write-back de senha**](#password-writeback-security-model)
 * [Detalhes de criptografia de write-back de senha](#password-writeback-encryption-details)
 * [Uso de largura de banda de write-back de senha](#password-writeback-bandwidth-usage)
* [**Implantando, gerenciando e acessando dados de redefinição de senha para seus usuários**](#deploying-managing-and-accessing-password-reset-data-for-your-users)
 * [Quais dados são usados na redefinição de senha?](#what-data-is-used-by-password-reset)
 * [Implantação de redefinição de senha sem a necessidade de registro do usuário final](#deploying-password-reset-without-requiring-end-user-registration)
 * [O que acontece quando um usuário se registra para redefinição de senha?](#what-happens-when-a-user-registers)
 * [Como acessar os dados de redefinição de senha de seus usuários](#how-to-access-password-reset-data-for-your-users)
 * [Configurando dados de redefinição de senha com o PowerShell](#setting-password-reset-data-with-powershell)
 * [Lendo dados de redefinição de senha com o PowerShell](#reading-password-reset-data-with-powershell)
* [**Como a redefinição de senha funciona para usuários B2B?**](#how-does-password-reset-work-for-b2b-users)

## <a name="how-does-the-password-reset-portal-work"></a>Como funciona o portal de redefinição de senha?
Quando um usuário navega até o portal de redefinição de senha, um fluxo de trabalho é iniciado para determinar se essa conta de usuário é válida, a que organização o usuário pertence, onde a senha do usuário é gerenciada e se o usuário tem licença para usar o recurso ou não.  Ler as etapas abaixo para saber mais sobre a lógica por trás da página de redefinição de senha.

1. O usuário clica no link Não consegue acessar a sua conta ou vai diretamente para [https://passwordreset.microsoftonline.com](https://passwordreset.microsoftonline.com).
2. O usuário insere uma id de usuário e passa  por um captcha.
3. O AD do Azure verifica se o usuário é capaz de usar esse recurso fazendo o seguinte:
   * Verifica se o usuário tem esse recurso habilitado e uma licença do Azure AD atribuída.
     * Se o usuário não tiver esse recurso habilitado ou uma licença atribuída, ele é aconselhado a entrar em contato com seu administrador para redefinir a senha.
   * Verifica se o usuário tem os dados de desafio corretos definidos na sua conta de acordo com a política do administrador.
     * Se a política exige apenas um desafio, fica garantido que o usuário tem os dados apropriados definidos em pelo menos um dos desafios habilitados pela política do administrador.
       * Se o usuário não estiver configurado, ele é aconselhado a entrar em contato com seu administrador para redefinir a senha.
     * Se a política exige dois desafios, fica garantido que o usuário tem os dados apropriados definidos em pelo menos dois dos desafios habilitados pela política do administrador.
       * Se o usuário não estiver configurado, ele é aconselhado a entrar em contato com seu administrador para redefinir a senha.
   * Verifica se a senha do usuário é gerenciada no local (federada ou com sincronização de hash de senha).
     * Se o write-back estiver implantado e a senha do usuário for gerenciada no local, o usuário tem permissão para prosseguir, autenticar e redefinir a senha.
     * Se o write-back não estiver implantado e a senha do usuário for gerenciada no local, o usuário é aconselhado a entrar em contato com seu administrador para redefinir a senha.
4. Se ficar estabelecido que o usuário é capaz de redefinir a senha com êxito, ele será guiado através do processo de redefinição.

Saiba mais sobre como implantar o write-back de senha em [Introdução: gerenciamento de senhas do Azure AD](active-directory-passwords-getting-started.md).

## <a name="password-writeback-overview"></a>Visão geral de write-back de senha
O write-back de senha é um componente do [Azure Active Directory Connect](connect/active-directory-aadconnect.md) que pode ser habilitado e usado pelos assinantes atuais do Active Directory Premium do Azure. Para saber mais, confira [Edições do Active Directory do Azure](active-directory-editions.md).

O write-back de senha permite que você configure o locatário de nuvem para gravar senhas de volta no seu Active Directory local.  Ele evita que você precise configurar e gerenciar uma solução de redefinição de senha de autoatendimento complicado no local e fornece uma maneira conveniente baseada em nuvem para que os usuários redefinam suas senhas locais onde quer que estejam.  Leia sobre alguns dos principais recursos de write-back de senha:

* **Feedback sem atraso.**  O write-back de senha é uma operação síncrona.  Os usuários serão notificados imediatamente se a senha não atender à política ou não puder ser redefinida ou alterada por qualquer motivo.
* **Com suporte à redefinição de senha para os usuários do AD FS ou outras tecnologias de federação.**  Com o write-back de senha, desde que as contas de usuário federado sejam sincronizadas com seu locatário do Azure AD, eles poderão gerenciar suas senhas do AD locais da nuvem.
* **Com suporte à redefinição de senhas de usuários usando a sincronização de hash de senha.** Quando o serviço de redefinição de senha detecta que uma conta de usuário sincronizada está habilitada para sincronização de hash de senha, redefinimos o local dessa conta e a senha da nuvem ao mesmo tempo.
* **Com suporte à alteração de senhas do painel de acesso e do Office 365.**  Quando usuários federados ou sincronizados com senha alteram suas senhas expiradas ou não expiradas, gravamos as senhas novamente em seu ambiente do AD local.
* **Oferece suporte à gravação de senhas quando um administrador as redefine no** [**Portal de Gerenciamento do Azure**](https://manage.windowsazure.com).  Sempre que um administrador redefine uma senha de usuário no [Portal de Gerenciamento do Azure](https://manage.windowsazure.com), se esse usuário é federado ou sincronizado com senha, vamos definir a senha que o administrador selecionar no AD local também.  Isso não é suportado atualmente no Portal de Administração do Office.
* **impõe as suas políticas de senha do AD locais.**  Quando um usuário redefine a sua senha, verificamos se ele atende às suas políticas do AD locais antes de confirmá-lo nesse diretório.  Isso inclui histórico, complexidade, tempo, filtros de senha e qualquer outra restrição de senha que você tenha definido no AD local.
* **Não requer regras de firewall de entrada.**  O write-back de senha usa uma retransmissão do barramento de serviço do Azure como um canal de comunicação subjacente, o que significa que você não precisa abrir todas as portas de entrada no firewall para que esse recurso funcione.
* **Sem suporte para contas de usuário que existam em grupos protegidos no seu Active Directory local.** Para saber mais sobre grupos protegidos, confira [Contas e grupos protegidos do Active Directory](https://technet.microsoft.com/library/dn535499.aspx).

### <a name="how-password-writeback-works"></a>Como funciona o write-back de senha
O write-back de senha tem três componentes principais:

* O serviço de nuvem de redefinição de senha (também integrado às páginas de alteração de senha do Azure AD)
* A retransmissão do barramento de serviço do Azure específica de locatário
* O ponto de extremidade de redefinição de senha local

Elas se encaixam conforme descrito no diagrama abaixo:

  ![][001]

Quando um usuário federado ou com sincronização de hash de senha redefine ou altera a sua senha na nuvem, ocorre o seguinte:

1. Verificamos que tipo de senha o usuário tem.  Se virmos que a senha é gerenciada no local, garantimos a execução do serviço de write-back.  Se estiver em execução, permitimos que o usuário continue; se não estiver, informamos ao usuário que a senha não pode ser redefinida aqui.
2. Em seguida, o usuário passa pelas entradas de autenticação pertinentes e alcança a tela de redefinição de senha.
3. O usuário seleciona uma nova senha e a confirma.
4. Ao clicar em enviar, criptografamos a senha em texto sem formatação com uma chave simétrica que foi criada durante o processo de instalação de write-back.
5. Após criptografar a senha, nós a incluímos em uma carga que seja enviada por um canal HTTPS para a retransmissão do barramento de serviço específica do seu locatário (que também configuramos para você durante o processo de instalação de write-back).  Esta retransmissão é protegida por uma senha gerada aleatoriamente que somente a sua instalação local sabe.
6. Depois que a mensagem chega ao barramento de serviço, o ponto de extremidade de redefinição de senha é automaticamente reativado e vê que há uma solicitação de redefinição pendente.
7. O serviço procurará o usuário em questão usando o atributo de âncora de nuvem.  Para que essa pesquisa seja bem-sucedida, o objeto de usuário deve existir no espaço do conector do AD, deve ser vinculado ao objeto de MV correspondente e deve ser vinculado ao objeto de conector AAD correspondente. Finalmente, para a sincronização localizar essa conta de usuário, o link do objeto de conector do AD para MV deve ter a regra de sincronização `Microsoft.InfromADUserAccountEnabled.xxx` no link.  Isso é necessário porque quando a chamada vem da nuvem, o mecanismo de sincronização usa o atributo cloudAnchor para pesquisar o objeto de espaço do conector AAD, segue o link de volta para o objeto de MV e segue o link de volta para o objeto do AD. Como pode haver vários objetos do AD (várias florestas) para o mesmo usuário, o mecanismo de sincronização depende do `Microsoft.InfromADUserAccountEnabled.xxx` link para escolher o correto. Observe que, como resultado dessa lógica, é necessário conectar o Azure AD Connect ao Controlador de Domínio Primário para que o write-back de senha funcione.  Se precisar fazer isso, poderá configurar o Azure AD Connect para que ele use um Emulador de Controlador de Domínio Primário clicando com o botão direito do mouse nas **propriedades** do conector de sincronização do Active Directory e selecionando **Configurar partições de diretório**. Nesse local, procure a seção **Configurações de conexão do controlador de domínio** e marque a caixa intitulada **Usar somente controladores de domínio preferenciais**. Observação: se o controlador de domínio preferencial não for um emulador de PDC, o Azure AD Connect ainda tentará acessar o PDC para executar o write-back de senha.
8. Depois que a conta de usuário é encontrada, tentamos redefinir a senha diretamente na floresta do AD pertinente.
9. Se a operação de definição de senha for bem-sucedida, dizemos ao usuário que sua senha foi modificada e que ele pode seguir o seu caminho tranquilamente.
10. Se a operação de definição de senha falhar, retornamos o erro para o usuário e o deixamos tentar novamente.  A operação pode falhar porque o serviço estava inoperante, porque a senha selecionada não atende às políticas da organização, porque não foi possível encontrar o usuário no AD local ou por vários outros motivos.  Temos uma mensagem específica para muitos desses casos e informamos ao usuário o que podem fazer para resolver o problema.

## <a name="scenarios-supported-for-password-writeback"></a>Cenários com suporte para write-back de senha
A seção a seguir descreve quais os cenários com suporte para quais versões de nossos recursos de sincronização.  Em geral, é sempre recomendável usar o recurso de atualização automática do Azure AD Connect ou instalar a versão mais recente do [Azure AD Connect](connect/active-directory-aadconnect.md#install-azure-ad-connect) se você quer usar o write-back de senha.

* [**Clientes Azure AD Connect, Azure AD Sync e DirSync com suporte**](#supported-clients)
* [**Licenças necessárias para write-back de senha**](#licenses-required-for-password-writeback)
* [**Modos de autenticação local com suporte para write-back de senha**](#on-premises-authentication-modes-supported-for-password-writeback)
* [**Operações do Usuário e do Administrador com suporte para write-back de senha**](#user-and-admin-operations-supported-for-password-writeback)
* [**Operações do Usuário e do Administrador sem suporte para write-back de senha**](#user-and-admin-operations-not-supported-for-password-writeback)

### <a name="supported-clients"></a>Clientes com suporte
É recomendável usar o recurso de atualização automática do Azure AD Connect ou instalar a versão mais recente do [Azure AD Connect](connect/active-directory-aadconnect.md#install-azure-ad-connect) se você quer usar o write-back de senha.

* **DirSync (qualquer versão > 1.0.6862)** - _SEM SUPORTE_ - dá suporte somente Aos recursos básicos de write-back e não tem mais suporte no grupo de produtos
* **Azure AD Sync** - _PRETERIDO_ - dá suporte somente aos recursos básicos de write-back e não tem recursos de desbloqueio de conta, registro em log avançado e melhorias de confiabilidade feitas no Azure AD Connect. Assim, é **altamente** recomendável fazer a atualização.
* **Azure AD Connect** - _SUPORTE TOTAL_ - dá suporte a todos os recursos de write-back - atualize para a versão mais recente para obter os melhores recursos novos e a maior estabilidade/confiabilidade possível

### <a name="licenses-required-for-password-writeback"></a>Licenças necessárias para write-back de senha
Para usar o write-back de senha, você deve ter uma das licenças a seguir atribuídas no locatário.

* **Azure AD Premium P1** - não há limitações quanto ao uso de write-back de senha
* **Azure AD Premium P2** - não há limitações quanto ao uso de write-back de senha
* **Enterprise Moblity Suite** - não há limitações quanto ao uso de write-back de senha
* **Enterprise Cloud Suite** - não há limitações quanto ao uso de write-back de senha

Você não pode usar o write-back de senha com qualquer plano de licenciamento do Office 365, seja ele de avaliação ou pago. Você deve atualizar para um dos planos acima para usar esse recurso.

Não temos planos para habilitar o write-back de senha para nenhuma SKU do Office 365.

### <a name="on-premises-authentication-modes-supported-for-password-writeback"></a>Modos de autenticação locais com suporte para write-back de senha
O write-back de senha funciona para os seguintes tipos de senha de usuário:

* **Usuários somente de nuvem**: o write-back de senha não se aplica nesse caso, pois não há uma senha local
* **Usuários com sincronização de senha**: suporte para write-back de senha
* **Usuários federados**: suporte para write-back de senha
* **Usuários de autenticação de passagem**: suporte para write-back de senha

### <a name="user-and-admin-operations-supported-for-password-writeback"></a>Operações de administrador e usuário com suporte para write-back de senha
As senhas são gravadas novamente em todas as seguintes situações:

* **Operações do usuário final com suporte**
 * Qualquer operação de alteração de senha voluntária de autoatendimento do usuário final
 * Qualquer operação para forçar alteração de senha de autoatendimento do usuário final (por exemplo, expiração de senha)
 * Qualquer redefinição de senha de autoatendimento do usuário final do [Portal de Redefinição de Senha](https://passwordreset.microsoftonline.com)
* **Operações do administrador com suporte**
 * Qualquer operação de alteração de senha voluntária de autoatendimento do administrador
 * Qualquer operação para forçar alteração de senha de autoatendimento do administrador (por exemplo, expiração de senha)
 * Qualquer redefinição de senha de autoatendimento do administrador do [Portal de Redefinição de Senha](https://passwordreset.microsoftonline.com)
 * Qualquer redefinição de senha do usuário final iniciada pelo administrador do [Portal de Gerenciamento Clássico](https://manage.windowsazure.com)
 * Qualquer redefinição de senha do usuário final iniciada pelo administrador do [Portal do Azure](https://portal.azure.com)

### <a name="user-and-admin-operations-not-supported-for-password-writeback"></a>Operações de administrador e usuário sem suporte para write-back de senha
As senhas não são gravadas em nenhuma das seguintes situações:

* **Operações do usuário final sem suporte**
 * Qualquer usuário final que redefine sua senha usando o PowerShell v1, v2 ou a API do Graph do Azure AD
* **Operações do administrador sem suporte**
 * Qualquer redefinição de senha do usuário final iniciada pelo administrador do [Portal de Gerenciamento do Office](https://portal.office.com)
 * Qualquer redefinição de senha do usuário final iniciada pelo administrador do PowerShell v1, v2 ou da API do Graph do Azure AD

Embora estejamos trabalhando para remover essas limitações, não temos ainda uma linha do tempo específica que possamos divulgar.

## <a name="password-writeback-security-model"></a>Modelo de segurança de write-back de senha
O write-back de senha é um serviço altamente seguro e sólido.  Para garantir que as suas informações estarão protegidas, habilitamos um modelo de segurança com quatro camadas que é descrito abaixo.

* **Retransmissão de barramento de serviço específica do locatário** : quando você configurar o serviço, configuramos uma retransmissão do barramento de serviço específica do locatário que é protegida por uma senha forte gerada aleatoriamente a qual a Microsoft nunca tem acesso.
* **Chave de criptografia de senha criptograficamente forte e bloqueada** : depois que a retransmissão do barramento de serviço é criada, podemos criar uma chave simétrica forte que usamos para criptografar a senha quando ela é transmitida.  Essa chave existe apenas no repositório secreto de sua empresa na nuvem, que é amplamente bloqueado e auditado, assim como qualquer senha no diretório.
* **TLS padrão da indústria** : quando uma redefinição de senha ou a operação de alteração ocorre na nuvem, podemos levar a senha de texto sem formatação e criptografá-la com sua chave pública.  Vamos então colocá-la em uma mensagem HTTPS que é enviada por um canal criptografado usando certificados SSL da Microsoft para a sua retransmissão do barramento de serviço.  Depois que essa mensagem é recebida no Barramento de Serviço, o agente local acorda, autentica para o barramento de serviço usando a senha forte que foi gerada, recebe a mensagem criptografada, descriptografa usando a chave particular que nós geramos e tenta definir a senha por meio da API do AD DS SetPassword.  Essa etapa é o que nos permite impor a política de senha local do AD (complexidade, idade, histórico, filtros, etc.) na nuvem.
* **Políticas de expiração de mensagem** : por fim, se por algum motivo a mensagem ficar no barramento de serviço porque o serviço local está inativo, o tempo limite terá sido atingido e ela será removida após alguns minutos para aumentar ainda mais a segurança.

### <a name="password-writeback-encryption-details"></a>Detalhes de criptografia de write-back de senha
A seguir são descritas as etapas de criptografia pelas quais uma solicitação de redefinição de senha passa depois que um usuário a envia, mas antes que ela chegue a seu ambiente local, para garantir a segurança e a confiabilidade máximas do serviço.

* **Etapa 1 - criptografia de senha com chave RSA de 2048 bits** - depois que um usuário envia uma senha para ser gravada no local, primeiro, a senha enviada em si é criptografada com uma chave RSA de 2048 bits.

* **Etapa 2 - criptografia em nível de pacote com AES-GCM** - em seguida, todo o pacote (senha + metadados necessários) é criptografado usando AES-GCM. Isso impede que qualquer pessoa com acesso direto ao canal do Barramento de Serviço subjacente exiba/viole o conteúdo.

* **Etapa 3 - toda a comunicação ocorre via TLS/SSL** – além disso, toda a comunicação com o ServiceBus ocorre em um canal SSL/TLS. Isso protege o conteúdo de terceiros não autorizados.

* **Etapa 4 - Substituição de Chave Automática a cada seis meses** - por fim, automaticamente a cada seis meses ou sempre que o write-back de senha é desabilitado/reabilitado no Azure AD Connect, substituímos todas essas chaves para garantir a segurança máxima do serviço.

### <a name="password-writeback-bandwidth-usage"></a>Uso de largura de banda de write-back de senha

O write-back de senha é um serviço de largura de banda extremamente baixa que envia solicitações de volta para o agente local somente nas seguintes circunstâncias:

1. Duas mensagens são enviadas quando o recurso é habilitado ou desabilitado por meio do Azure AD Connect.
2. Uma mensagem é enviada a cada 5 minutos como uma pulsação de serviço para desde que o serviço está em execução.
3. Duas mensagens são enviadas sempre que uma nova senha é enviada, uma mensagem como solicitação para executar a operação e uma mensagem subsequente com o resultado da operação. Essas mensagens são enviadas nas seguintes circunstâncias.
4. Sempre que uma nova senha é enviada durante uma redefinição de senha de autoatendimento do usuário.
5. Sempre que uma nova senha é enviada durante uma operação de alteração de senha do usuário.
6. Sempre que uma nova senha é enviada durante uma redefinição de usuário iniciada pelo administrador (somente dos portais de administração do Azure)

#### <a name="message-size-and-bandwidth-considerations"></a>Considerações sobre a largura de banda e o tamanho da mensagem

O tamanho de cada uma das mensagens descritas acima normalmente é de 1kb, o que significa que, mesmo sob carga extrema, o próprio serviço de write-back de senha estará consumindo no máximo alguns quilobits por segundo de largura de banda. Como cada mensagem é enviada em tempo real, somente quando for solicitado por uma operação de atualização de senha, e como o tamanho da mensagem é bem pequeno, o uso da largura de banda da capacidade de write-back será efetivamente muito pequeno para ter qualquer impacto real mensurável.

## <a name="deploying-managing-and-accessing-password-reset-data-for-your-users"></a>Implantando, gerenciando e acessando dados de redefinição de senha para seus usuários
Você pode gerenciar e acessar dados de redefinição de senha para os usuários por meio das experiências do Azure AD Connect, PowerShell, Graph ou registro.  Você pode até mesmo implantar a redefinição de senha para toda a empresa sem exigir que os usuários se registrem para ela, aproveitando as opções descritas abaixo.

  * [Quais dados são usados na redefinição de senha?](#what-data-is-used-by-password-reset)
  * [Implantação de redefinição de senha sem a necessidade de registro do usuário final](#deploying-password-reset-without-requiring-end-user-registration)
  * [O que acontece quando um usuário se registra para redefinição de senha?](#what-happens-when-a-user-registers)
  * [Como acessar os dados de redefinição de senha de seus usuários](#how-to-access-password-reset-data-for-your-users)
  * [Configurando dados de redefinição de senha com o PowerShell](#setting-password-reset-data-with-powershell)
  * [Lendo dados de redefinição de senha com o PowerShell](#reading-password-reset-data-with-powershell)

### <a name="what-data-is-used-by-password-reset"></a>Quais dados são usados na redefinição de senha?
A tabela a seguir descreve onde e como esses dados são usados durante a redefinição de senha e foi projetada para ajudá-lo a decidir quais opções de autenticação são adequadas para a sua organização. Esta tabela também mostra os requisitos de formatação quando você fornecer dados em nome dos usuários de caminhos de entrada que não validarem esses dados.

> [!NOTE]
> O Telefone comercial não aparece no portal de registro porque os usuários atualmente não podem editá-lo no diretório. Somente administradores podem definir esse valor.
>
>

<table>
          <tbody><tr>
            <td>
              <p>
                <strong>Nome do Método de Contato</strong>
              </p>
            </td>
            <td>
              <p>
                <strong>Elemento de Dados do Active Directory</strong>
              </p>
            </td>
            <td>
              <p>
                <strong>Elemento de Dados do Azure Active Directory</strong>
              </p>
            </td>
            <td>
              <p>
                <strong>Usados/Definíveis Onde?</strong>
              </p>
            </td>
            <td>
              <p>
                <strong>Requisitos do formato</strong>
              </p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Telefone comercial</p>
            </td>
            <td>
              <p>telephoneNumber</p>
              <p>Essa propriedade pode ser sincronizada com o atributo PhoneNumber no Azure Active Directory e usada imediatamente para a redefinição de senha, sem exigir que o usuário se registre primeiro.</p>
            </td>
            <td>
              <p>PhoneNumber</p>
              <p>por exemplo, Set-MsolUser -UserPrincipalName JWarner@contoso.com -PhoneNumber "+1 1234567890x1234"</p>
            </td>
            <td>
              <p>Usado em:</p>
              <p>Portal de redefinição de senha</p>
              <p>Configurável de:</p>
              <p>PhoneNumber é configurável do PowerShell, do DirSync, do Portal de Gerenciamento do Azure e do Portal de administração do Office</p>
            </td>
            <td>
              <p>+ ccc xxxyyyzzzz (por exemplo, +1 1234567890)</p>
              <ul>
                <li class="unordered">
Forneça um código de país<br><br></li>
              </ul>
              <ul>
                <li class="unordered">
Forneça um código de área (quando aplicável)<br><br></li>
              </ul>
              <ul>
                <li class="unordered">
Coloque um + antes do país de código<br><br></li>
              </ul>
              <ul>
                <li class="unordered">
Deve haver um espaço entre o código do país e o resto do número<br><br></li>
              </ul>
              <ul>
                <li class="unordered">
Não há suporte para extensões; se você especificar extensões, nós a removeremos do número antes de expedir a chamada.<br><br></li>
              </ul>
            </td>
          </tr>
          <tr>
            <td>
              <p>Telefone celular</p>
            </td>
            <td>
              <p>Móvel</p>
              <p>Essa propriedade pode ser sincronizada com o atributo MobilePhone no Azure Active Directory e usada imediatamente para a redefinição de senha, sem exigir que o usuário se registre primeiro.</p>
              <p>Não é possível sincronizar essa propriedade para AuthenticationPhone no momento.</p>
            </td>
            <td>
              <p>AuthenticationPhone</p>
              <p>OU</p>
              <p>MobilePhone</p>
              <p>(O telefone de autenticação é usado se não houver dados presentes; caso contrário, ele reverterá para o campo do telefone celular.)</p>
              <p>por exemplo, Set-MsolUser -UserPrincipalName JWarner@contoso.com -MobilePhone "+1 1234567890x1234"</p>
            </td>
            <td>
              <p>Usado em:</p>
              <p>Portal de redefinição de senha</p>
              <p>Portal de registro</p>
              <p>Configurável de: </p>
              <p>AuthenticationPhone é configurável do portal de registro de redefinição de senha ou do portal de registro MFA.</p>
              <p>MobilePhone é configurável do PowerShell, do Azure AD Connect, do Portal de Gerenciamento do Azure e do Portal de administração do Office</p>
            </td>
            <td>
              <p>+ ccc xxxyyyzzzz (por exemplo, +1 1234567890)</p>
              <ul>
                <li class="unordered">
Forneça um código de país.<br><br></li>
              </ul>
              <ul>
                <li class="unordered">
Forneça um código de área (quando aplicável).<br><br></li>
              </ul>
              <ul>
                <li class="unordered">
Coloque um + antes do país de código.<br><br></li>
              </ul>
              <ul>
                <li class="unordered">
Deve ter um espaço entre o código do país e o resto do número.<br><br></li>
              </ul>
              <ul>
                <li class="unordered">
Não há suporte para extensões; se você especificar extensões, nós a ignoraremos ao expedir a chamada.<br><br></li>
              </ul>
            </td>
          </tr>
          <tr>
            <td>
              <p>Email alternativo</p>
            </td>
            <td>
              <p>Não disponível</p>
              <p>Não é possível sincronizar os valores do Active Directory para a propriedade AuthenticationEmail ou AlternateEmailAddresses [0] no momento. </p>
              <p>Você pode usar o PowerShell para definir AlternateEmailAddresses[0]. Instruções para isso estão na seção logo abaixo dessa tabela.</p>
            </td>
            <td>
              <p>AuthenticationEmail</p>
              <p>OU</p>
              <p>AlternateEmailAddresses[0] </p>
              <p>(A autenticação de que email é usada se não houver dados presentes; caso contrário, ele reverterá para o campo de email alternativo.)</p>
              <p>Observação: o campo de email alternativo é especificado como uma matriz de cadeias de caracteres no diretório.  Usamos a primeira entrada nessa matriz.</p>
              <p>por exemplo, Set-MsolUser -UserPrincipalName JWarner@contoso.com -AlternateEmailAddresses "email@live.com"</p>
            </td>
            <td>
              <p>Usado em:</p>
              <p>Portal de redefinição de senha</p>
              <p>Portal de registro</p>
              <p>Configurável de: </p>
              <p>AuthenticationEmail é configurável do portal de registro de redefinição de senha ou do portal de registro MFA.</p>
              <p>AlternateEmail é configurável do PowerShell, do Portal de Gerenciamento do Azure e do Portal de administração do Office</p>
            </td>
            <td>
              <p>
                <a href="mailto:user@domain.com">user@domain.com</a> ou 甲斐@黒川.日本</p>
              <ul>
                <li class="unordered">
Os emails devem seguir a formatação padrão de acordo com .<br><br></li>
              </ul>
              <ul>
                <li class="unordered">
Os emails Unicode têm suporte.<br><br></li>
              </ul>
            </td>
          </tr>
          <tr>
            <td>
              <p>Perguntas de segurança e respostas</p>
            </td>
            <td>
              <p>Não disponível</p>
              <p>Não é possível sincronizar as Perguntas de Segurança ou as Respostas do Active Directory no momento.</p>
            </td>
            <td>
              <p>Não disponível para modificação diretamente no diretório.</p>
              <p>Só pode ser definido durante o processo de registro de usuário final de redefinição de senha.</p>
            </td>
            <td>
              <p>Usado em:</p>
              <p>Portal de redefinição de senha</p>
              <p>Portal de registro </p>
              <p>Configurável de: </p>
              <p>A única maneira de definir perguntas de segurança é através do Portal de Gerenciamento do Azure.</p>
              <p>A única maneira de definir respostas para perguntas de segurança para um determinado usuário é através do Portal de Registro.</p>
            </td>
            <td>
              <p>As perguntas de segurança têm um máximo de 200 e um mínimo de três caracteres</p>
              <p>As respostas têm um máximo de 40 e um mínimo de três caracteres</p>
            </td>
          </tr>
        </tbody></table>


### <a name="deploying-password-reset-without-requiring-end-user-registration"></a>Implantando a redefinição de senha sem a necessidade de registro do usuário final
Se desejar implantar a redefinição de senha sem solicitar que os usuários se registrem nela, você poderá fazer isso facilmente seguindo uma das duas opções abaixo. Isso pode ser uma maneira útil de desbloquear grandes números de usuários para usar SSPR e ainda permitir que os usuários validem essas informações durante o processo de registro.

Muitos de nossos maiores clientes usam isso hoje para começar muito rapidamente a redefinição de senha.

#### <a name="synchronize-phone-numbers-with-azure-ad-connect"></a>Sincronizar números de telefone com o Azure AD Connect
Se você sincronizar dados para um ou ambos os campos abaixo, eles poderão ser usados imediatamente para redefinição de senha, sem exigir que os usuários se registrem primeiro:

* Telefone celular
* Telefone comercial

Para obter informações sobre quais propriedades precisam ser atualizadas no local, acesse a seção [Quais dados são usados pela redefinição de senha?](#what-data-is-used-by-password-reset) e pesquise os campos mencionados acima.  

Verifique se os números de telefone estão no formato "+1 1234567890" para que eles funcionem corretamente com nosso sistema.

#### <a name="set-phone-numbers-or-emails-with-powershell"></a>Definir números de telefone ou emails com o PowerShell
Se você definir um ou mais desses campos, eles poderão ser usados imediatamente para redefinição de senha, sem exigir que os usuários se registrem primeiro:

* Telefone celular
* Telefone comercial
* Email alternativo

Para saber como definir essas propriedades usando o PowerShell, acesse a seção [Definindo dados de redefinição de senha com o PowerShell](#setting-password-reset-data-with-powershell).

Verifique se os números de telefone estão no formato "+1 1234567890" para que eles funcionem corretamente com nosso sistema.

### <a name="what-happens-when-a-user-registers"></a>O que acontece quando um usuário se registra?
Quando um usuário se registrar, a página de registro **sempre** definirá os seguintes campos:

* Telefone de autenticação
* E-mail de autenticação
* Perguntas de segurança e respostas

Se você forneceu um valor para **Celular** ou **Email alternativo**, os usuários poderão usar isso imediatamente para redefinir suas senhas, mesmo que não tenham registrado o serviço.  Além disso, os usuários verão esses valores ao se registrar pela primeira vez e poderão modificá-los caso queiram.  No entanto, depois deles terem registrado com êxito, esses valores serão mantidos nos campos **Telefone de Autenticação** e **Email de Autenticação**, respectivamente.

### <a name="how-to-access-password-reset-data-for-your-users"></a>Como acessar os dados de redefinição de senha de seus usuários
#### <a name="data-settable-via-synchronization"></a>Dados configuráveis por meio da sincronização
Os campos a seguir podem ser sincronizados a partir do ambiente local:

* Telefone celular
* Telefone comercial

#### <a name="data-settable-with-azure-ad-powershell--azure-ad-graph"></a>Dados configuráveis com o Azure AD PowerShell e o Azure AD Graph
Os seguintes campos podem ser definidos usando o Azure AD PowerShell e a API do Graph do Azure AD:

* Email alternativo
* Telefone celular
* Telefone comercial

#### <a name="data-settable-with-registration-ui-only"></a>Dados configuráveis somente com a interface do usuário de registros
Os seguintes campos só são acessíveis por meio da IU do registro SSPR (https://aka.ms/ssprsetup):

* Perguntas de segurança e respostas

#### <a name="data-readable-with-azure-ad-powershell--azure-ad-graph"></a>Dados legíveis com o Azure AD PowerShell e o Azure AD Graph
Os seguintes campos são acessíveis com o Azure AD PowerShell e a API do Graph do Azure AD:

* Email alternativo
* Telefone celular
* Telefone comercial
* Telefone de autenticação
* E-mail de autenticação

### <a name="setting-password-reset-data-with-powershell"></a>Configurando dados de redefinição de senha com o PowerShell
Você pode definir valores para os seguintes campos com o Azure AD PowerShell.

* Email alternativo
* Telefone celular
* Telefone comercial

**_PowerShell V1_**

Para começar, você precisa primeiro [baixar e instalar o módulo do Azure AD PowerShell](https://msdn.microsoft.com/library/azure/jj151815.aspx#bkmk_installmodule).  Depois que você tiver instalado, pode seguir as etapas abaixo para configurar cada campo.

**_PowerShell V2_**

Para começar, você precisa primeiro [baixar e instalar o módulo Azure AD V2 PowerShell](https://github.com/Azure/azure-docs-powershell-azuread/blob/master/Azure%20AD%20Cmdlets/AzureAD/index.md). Depois que você tiver instalado, pode seguir as etapas abaixo para configurar cada campo.

Para instalar rapidamente de versões recentes do PowerShell que dão suporte a Install-Module, execute estes comandos (a primeira linha simplesmente verifica se ele já está instalado):

```
Get-Module AzureADPreview
Install-Module AzureADPreview
Connect-AzureAD
```

#### <a name="alternate-email---how-to-set-alternate-email-with-powershell"></a>Email alternativo - como definir o email alternativo com o PowerShell
**_PowerShell V1_**

```
Connect-MsolService
Set-MsolUser -UserPrincipalName user@domain.com -AlternateEmailAddresses @("email@domain.com")
```

**_PowerShell V2_**

```
Connect-AzureAD
Set-AzureADUser -ObjectId user@domain.com -OtherMails @("email@domain.com")
```

#### <a name="mobile-phone---how-to-set-mobile-phone-with-powershell"></a>Celular - como definir um telefone celular com o PowerShell
**_PowerShell V1_**

```
Connect-MsolService
Set-MsolUser -UserPrincipalName user@domain.com -MobilePhone "+1 1234567890"
```

**_PowerShell V2_**

```
Connect-AzureAD
Set-AzureADUser -ObjectId user@domain.com -Mobile "+1 1234567890"
```

#### <a name="office-phone---how-to-set-office-phone-with-powershell"></a>Telefone comercial - como configurar o telefone comercial com o PowerShell
**_PowerShell V1_**

```
Connect-MsolService
Set-MsolUser -UserPrincipalName user@domain.com -PhoneNumber "+1 1234567890"
```

**_PowerShell V2_**

```
Connect-AzureAD
Set-AzureADUser -ObjectId user@domain.com -TelephoneNumber "+1 1234567890"
```

### <a name="reading-password-reset-data-with-powershell"></a>Lendo dados de redefinição de senha com o PowerShell
Você pode ler os valores dos campos a seguir com o Azure AD PowerShell.

* Email alternativo
* Telefone celular
* Telefone comercial
* Telefone de autenticação
* E-mail de autenticação

#### <a name="alternate-email---how-to-read-alternate-email-with-powershell"></a>Email alternativo - como ler o email alternativo com o PowerShell
**_PowerShell V1_**

```
Connect-MsolService
Get-MsolUser -UserPrincipalName user@domain.com | select AlternateEmailAddresses
```

**_PowerShell V2_**

```
Connect-AzureAD
Get-AzureADUser -ObjectID user@domain.com | select otherMails
```

#### <a name="mobile-phone---how-to-read-mobile-phone-with-powershell"></a>Celular - como ler um telefone celular com o PowerShell
**_PowerShell V1_**

```
Connect-MsolService
Get-MsolUser -UserPrincipalName user@domain.com | select MobilePhone
```

**_PowerShell v2_**

```
Connect-AzureAD
Get-AzureADUser -ObjectID user@domain.com | select Mobile
```

#### <a name="office-phone---how-to-read-office-phone-with-powershell"></a>Telefone comercial - como ler o telefone comercial com o PowerShell
**_PowerShell V1_**

```
Connect-MsolService
Get-MsolUser -UserPrincipalName user@domain.com | select PhoneNumber
```

**_PowerShell V2_**

```
Connect-AzureAD
Get-AzureADUser -ObjectID user@domain.com | select TelephoneNumber
```

#### <a name="authentication-phone---how-to-read-authentication-phone-with-powershell"></a>Telefone de autenticação - como ler o telefone de autenticação com o PowerShell
**_PowerShell V1_**

```
Connect-MsolService
Get-MsolUser -UserPrincipalName user@domain.com | select -Expand StrongAuthenticationUserDetails | select PhoneNumber
```

**_PowerShell V2_**

```
Not possible in PowerShell V2
```

#### <a name="authentication-email---how-to-read-authentication-email-with-powershell"></a>Email de autenticação - como ler o email de autenticação com o PowerShell
**_PowerShell V1_**

```
Connect-MsolService
Get-MsolUser -UserPrincipalName user@domain.com | select -Expand StrongAuthenticationUserDetails | select Email
```

**_PowerShell V2_**

```
Not possible in PowerShell V2
```
## <a name="how-does-password-reset-work-for-b2b-users"></a>Como a redefinição de senha funciona para usuários B2B?
A redefinição e a alteração de senhas têm suporte completo em todas as configurações B2B.  Leia abaixo os três casos B2B explícitos com suporte para redefinição de senha.

1. **Usuários de uma organização parceira com um locatário existente do Azure AD** - se a organização com a qual você está fazendo uma parceria tiver um locatário existente do Azure AD, **respeitaremos quaisquer políticas de redefinição de senha habilitadas no locatário**. Para que a redefinição de senha funcione, a organização parceira só precisa garantie que o Azure AD SSPR esteja habilitado, o que não acarreta custo adicional para os clientes do O365 e pode ser habilitado seguindo as etapas em nosso guia de [Introdução ao gerenciamento de senhas](https://azure.microsoft.com/documentation/articles/active-directory-passwords-getting-started/#enable-users-to-reset-or-change-their-aad-passwords).
2. **Usuários que se inscreveram usando a [assinatura de autoatendimento](https://docs.microsoft.com/azure/active-directory/active-directory-self-service-signup)** - se a organização com a qual você está fazendo uma parcerias tiver usado o recurso de [assinatura de autoatendimento](https://docs.microsoft.com/azure/active-directory/active-directory-self-service-signup) para obter um locatário, permitiremos que eles redefinam desde o início com o email com o qual se registraram.
3. **Usuários B2B** - novos usuários B2B criados usando os novos [recursos do Azure AD B2B](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-b2b-what-is-azure-ad-b2b) também poderão redefinir suas senhas desde o início com o email com o qual se registraram durante o processo de convite.

Para testar isso, acesse http://passwordreset.microsoftonline.com com um desses usuários parceiros.  Contanto que eles tenham um email alternativo ou um email de autenticação definido, a redefinição de senha funcionará conforme o esperado.  Mais informações sobre os dados usados por sspr aqui podem ser encontradas na visão geral [Quais dados são usados pela redefinição de senha](https://azure.microsoft.com/en-us/documentation/articles/active-directory-passwords-learn-more/#what-data-is-used-by-password-reset).

## <a name="next-steps"></a>Próximas etapas
Veja abaixo links para todas as páginas de documentação sobre Redefinição de Senha do AD do Azure:

* **Você está aqui por que está enfrentando problemas para iniciar sessão?** Se sim, [veja aqui como alterar e redefinir sua senha](active-directory-passwords-update-your-own-password.md#reset-your-password).
* [**Como funciona**](active-directory-passwords-how-it-works.md) – saiba mais sobre os seis diferentes componentes do serviço e o que cada um deles faz
* [**Introdução**](active-directory-passwords-getting-started.md) – saiba como permitir que os usuários redefinam e alterem suas senhas na nuvem ou no local
* [**Personalizar**](active-directory-passwords-customize.md)- aprenda a personalizar a aparência e o comportamento do serviço de acordo com as necessidades de sua organização
* [**Práticas recomendadas**](active-directory-passwords-best-practices.md) - aprenda a implantar rapidamente e gerenciar com eficiência as senhas em sua organização
* [**Obter percepções**](active-directory-passwords-get-insights.md) – saiba mais sobre nossos recursos integrados de relatórios
* [**Perguntas frequentes**](active-directory-passwords-faq.md) - obtenha respostas para perguntas frequentes
* [**Solução de problemas**](active-directory-passwords-troubleshoot.md) – aprenda a solucionar rapidamente os problemas com o serviço

[001]: ./media/active-directory-passwords-learn-more/001.jpg "Image_001.jpg"
[002]: ./media/active-directory-passwords-learn-more/002.jpg "Image_002.jpg"

