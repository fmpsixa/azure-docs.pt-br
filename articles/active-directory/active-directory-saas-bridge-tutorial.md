---
title: "Tutorial: Integração do Azure Active Directory ao Bridge | Microsoft Docs"
description: "Saiba como configurar o logon único entre o Azure Active Directory e o Bridge."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 3dbb6499-80c1-4d00-a0b4-e0ad5522cf0f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 3/10/2017
ms.author: jeedes
translationtype: Human Translation
ms.sourcegitcommit: 07635b0eb4650f0c30898ea1600697dacb33477c
ms.openlocfilehash: 2250fbe0b7451d875ccdd319af9b77cb4e24fe35
ms.lasthandoff: 03/28/2017


---
# <a name="tutorial-azure-active-directory-integration-with-bridge"></a>Tutorial: Integração do Azure Active Directory ao Bridge

Neste tutorial, você aprende a integrar o Bridge ao Azure AD (Azure Active Directory).

A integração do Bridge ao Azure AD oferece os seguintes benefícios:

- No Azure AD, é possível controlar quem tem acesso ao Bridge
- É possível permitir que os usuários sejam conectados automaticamente ao Bridge usando SSO (logon único) com suas contas do Azure AD
- Gerenciar suas contas em um único local: o Portal clássico do Azure

Para conhecer mais detalhadamente a integração de aplicativos de SaaS ao AD do Azure, consulte [O que é o acesso a aplicativos e logon único com o Active Directory do Azure](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD ao Bridge, você precisa dos seguintes itens:

- Uma assinatura do AD do Azure
- Uma assinatura do Bridge habilitada para SSO


>[!NOTE]
>Para testar as etapas deste tutorial, nós não recomendamos o uso de um ambiente de produção.
>

Para testar as etapas deste tutorial, você deve seguir estas recomendações:

- Não use o ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, você pode obter uma [versão de avaliação de um mês](https://azure.microsoft.com/pricing/free-trial/).


## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, você testa o SSO do Azure AD em um ambiente de teste. 

O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionando o Bridge por meio da galeria
2. Configurar e testar o SSO do Azure AD


## <a name="add-bridge-from-the-gallery"></a>Adicionar o Bridge da galeria
Para configurar a integração do Bridge ao Azure AD, é necessário adicionar o Bridge por meio da galeria à lista de aplicativos SaaS gerenciados.

**Para adicionar o Bridge por meio da galeria, realize as seguintes etapas:**

1. No **Portal clássico do Azure**, no painel de navegação à esquerda, clique em **Active Directory**. 

    ![Active Directory][1]

2. Na lista **Diretório** , selecione o diretório para o qual você deseja habilitar a integração de diretórios.

3. Para abrir a visualização dos aplicativos, na exibição do diretório, clique em **Aplicativos** no menu principal.

    ![Aplicativos][2]

4. Clique em **Adicionar** na parte inferior da página.

    ![Aplicativos][3]

5. Na caixa de diálogo **O que você deseja fazer**, clique em **Adicionar um aplicativo da galeria**.

    ![Aplicativos][4]

6. Na caixa de pesquisa, digite **Bridge**.

    ![Criação de um usuário de teste do AD do Azure](./media/active-directory-saas-bridge-tutorial/tutorial_bridge_001.png)

7. No painel de resultados, selecione **Bridge** e clique em **Concluir** para adicionar o aplicativo.

    ![Criação de um usuário de teste do AD do Azure](./media/active-directory-saas-bridge-tutorial/tutorial_bridge_0001.png)


##  <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar logon único do Azure AD
Nesta seção, você configurará e testará o logon único (SSO) do Azure AD com o Bridge, com base em um usuário de teste chamado “Brenda Fernandes”.

Para que o SSO funcione, o Azure AD precisa saber qual usuário do Bridge é equivalente a um usuário do Azure AD. Em outras palavras, é necessário estabelecer uma relação de vínculo entre um usuário do Azure AD e o usuário relacionado no Bridge.

Essa relação de vínculo é estabelecida atribuindo o valor do **nome de usuário** no Azure AD como o valor do **Nome de usuário** no Bridge.

Para configurar e testar o logon único do Azure AD com o Bridge, é necessário concluir os seguintes blocos de construção:

1. **[Configurar logon único do Azure AD](#configuring-azure-ad-single-sign-on)** – para habilitar seus usuários a usar esse recurso.
2. **[Criação de um usuário de teste do AD do Azure](#creating-an-azure-ad-test-user)** : para testar o logon único do AD do Azure com Brenda Fernandes.
3. **[Criando um usuário de teste do Bridge](#creating-a-bridge-test-user)** – para ter um equivalente de Brenda Fernandes no Bridge que esteja vinculado à representação dela no Azure AD.
4. **[Atribuição do usuário de teste do AD do Azure](#assigning-the-azure-ad-test-user)** - para habilitar Britta Simon a usar o logon único do AD do Azure.
5. **[Teste do logon único](#testing-single-sign-on)** – para verificar se a configuração funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurar o logon único do Azure AD

Nesta seção, você habilitará o SSO do Azure AD no Portal Clássico e configurará o logon único em seu aplicativo Bridge.


**Para configurar o logon único do Azure AD com o Bridge, realize as seguintes etapas:**

1. No portal clássico, na página de integração do aplicativo **Bridge**, clique em **Configurar logon único** para abrir a caixa de diálogo **Configurar Logon Único**.

    ![Configurar Logon Único][6]

2. Na página **Como você deseja que os usuários façam logon no Bridge**, selecione **Logon Único do Azure AD** e clique em **Avançar**.
 
    ![Configurar Logon Único](./media/active-directory-saas-bridge-tutorial/tutorial_bridge_02.png)

3. Na página de diálogo **Definir Configurações de Aplicativo** , execute as seguintes etapas:

    ![Configurar Logon Único](./media/active-directory-saas-bridge-tutorial/tutorial_bridge_03.png)
  1. Na caixa de texto **URL de Logon**, digite uma URL usando o seguinte padrão: `https://<company name>.bridgeapp.com`
  2. Na caixa de texto **Identificador**, digite uma URL usando o seguinte padrão: `https://<company name>.bridgeapp.com`
  3. Clique em **Próximo**.

    >[!NOTE] 
    >Observe que esses não são os valores reais. É necessário atualizar esses valores com a URL de Entrada e o Identificador reais. É possível gerar o tíquete de suporte com o Bridge <a href="https://bridgeapp.zendesk.com/hc/en-us/requests/new">aqui</a> para obter esses valores.
    >

4. Na página **Configurar logon único no Bridge**, clique em **Baixar certificado** e salve o arquivo no computador:

    ![Configurar Logon Único](./media/active-directory-saas-bridge-tutorial/tutorial_bridge_04.png) 

5. Para obter o SSO configurado para seu aplicativo, é possível gerar o tíquete de suporte com a equipe de suporte do Bridge <a href="https://bridgeapp.zendesk.com/hc/en-us/requests/new">aqui</a> e fornecer o seguinte: 

 *  O **arquivo de certificado** baixado
 *  A **ID de Entidade**
 *  A **URL do Serviço de Logon Único**
 *  A **URL do Serviço de Logoff Único**

6. No portal clássico, selecione a confirmação da configuração de logon único e clique em **Avançar**.

    ![Logon Único do AD do Azure][10]

7. Na página **Confirmação de logon único**, clique em **Concluir**.  
  
    ![Logon Único do AD do Azure][11]


### <a name="create-an-azure-ad-test-user"></a>Criar um usuário de teste do Azure AD
O objetivo desta seção é criar um usuário de teste no Portal Clássico do Azure chamado Brenda Fernandes.

![Criar um usuário do AD do Azure][20]

**Para criar um usuário de teste no AD do Azure, execute as seguintes etapas:**

1. No **portal clássico do Azure**, no painel de navegação à esquerda, clique em **Active Directory**.

    ![Criação de um usuário de teste do AD do Azure](./media/active-directory-saas-bridge-tutorial/create_aaduser_09.png) 

2. Na lista **Diretório** , selecione o diretório para o qual você deseja habilitar a integração de diretórios.

3. Para exibir a lista de usuários, no menu na parte superior, clique em **Usuários**.

    ![Criação de um usuário de teste do AD do Azure](./media/active-directory-saas-bridge-tutorial/create_aaduser_03.png) 

4. Para abrir a caixa de diálogo **Adicionar Usuário**, na barra de ferramentas na parte inferior, clique em **Adicionar Usuário**.
 
    ![Criação de um usuário de teste do AD do Azure](./media/active-directory-saas-bridge-tutorial/create_aaduser_04.png) 

5. Na página do diálogo **Conte-nos sobre este usuário** , realize as seguintes etapas:
 
    ![Criação de um usuário de teste do AD do Azure](./media/active-directory-saas-bridge-tutorial/create_aaduser_05.png) 
 1. Em Tipo de Usuário, selecione Novo usuário na organização.
 2. Na **caixa de texto** Nome do Usuário, digite **BrendaFernandes**.
 3. Clique em **Próximo**.

6.  Na página do diálogo **Perfil do Usuário** , realize as seguintes etapas:

    ![Criação de um usuário de teste do AD do Azure](./media/active-directory-saas-bridge-tutorial/create_aaduser_06.png) 
 1. Na caixa de texto **Nome**, digite **Brenda**.  
 2. Na caixa de texto **Sobrenome**, digite **Fernandes**.
 3. Na caixa de texto **Nome de Exibição**, digite **Brenda Fernandes**.
 4. Na lista **Função**, selecione **Usuário**.
 5. Clique em **Próximo**.

7. Na página de diálogo **Obter senha temporária**, clique em **criar**.

    ![Criação de um usuário de teste do AD do Azure](./media/active-directory-saas-bridge-tutorial/create_aaduser_07.png) 

8. Na página de caixa de diálogo **Obter senha temporária** , execute as seguintes etapas:

    ![Criação de um usuário de teste do AD do Azure](./media/active-directory-saas-bridge-tutorial/create_aaduser_08.png) 
 1. Anote o valor da **Nova Senha**.
 2. Clique em **Concluído**.   

### <a name="create-a-bridge-test-user"></a>Criar um usuário de teste do Bridge

Nesta seção, você cria um usuário chamado Brenda Fernandes no Bridge. Trabalhe com a equipe de suporte do Bridge para criar um usuário na plataforma. É possível gerar o tíquete de suporte com o Bridge <a href="https://bridgeapp.zendesk.com/hc/en-us/requests/new">aqui</a> para adicionar os usuários à plataforma do Bridge.

### <a name="assign-the-azure-ad-test-user"></a>Atribuir o usuário de teste do Azure AD

Nesta seção, você permitirá que Brenda Fernandes use o logon único (SSO) do Azure concedendo-lhe acesso ao Bridge.

![Atribuir usuário][200] 

**Para atribuir Brenda Fernandes ao Bridge, realize as seguintes etapas:**

1. No portal clássico, para abrir o modo de exibição de aplicativos, no modo de exibição de diretório, clique em **Aplicativos** no menu superior.

    ![Atribuir usuário][201] 

2. Na lista de aplicativos, selecione **Bridge**.

    ![Configurar Logon Único](./media/active-directory-saas-bridge-tutorial/tutorial_bridge_50.png) 

3. No menu na parte superior, clique em **Usuários**.

    ![Atribuir usuário][203] 

4. Na lista de usuários, selecione **Brenda Fernandes**.

5. Na barra de ferramentas na parte inferior, clique em **Atribuir**.
    
    ![Atribuir usuário][205]

### <a name="test-single-sign-on"></a>Testar logon único

Nesta seção, você testará sua configuração de SSO do Azure AD usando o Painel de Acesso.

Ao clicar no bloco Bridge no Painel de Acesso, você deverá ser conectado automaticamente ao aplicativo Bridge.

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicativos SaaS com o Active Directory do Azure](active-directory-saas-tutorial-list.md)
* [O que é o acesso a aplicativos e logon único com o Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-bridge-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-bridge-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-bridge-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-bridge-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-bridge-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-bridge-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-bridge-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-bridge-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-bridge-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-bridge-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-bridge-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-bridge-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-bridge-tutorial/tutorial_general_205.png

