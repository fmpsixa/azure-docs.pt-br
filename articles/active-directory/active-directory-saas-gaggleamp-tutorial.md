---
title: "Tutorial: integração do Azure Active Directory com o GaggleAMP | Microsoft Docs"
description: "Saiba como configurar o logon único entre o Active Directory do Azure e o GaggleAMP."
services: active-directory
documentationcenter: 
author: jeevansd
manager: femila
editor: 
ms.assetid: 9cc1a4b7-964b-406b-9e0c-05cb1a7c9856
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/17/2017
ms.author: jeedes
translationtype: Human Translation
ms.sourcegitcommit: 3c83745c09514e63cfe686fffd319328b19ee460
ms.openlocfilehash: b70cb12131d97fcf2569f5efd97b87037baa1837
ms.lasthandoff: 02/28/2017


---
# <a name="tutorial-azure-active-directory-integration-with-gaggleamp"></a>Tutorial: Integração do Active Directory do Azure com o GaggleAMP
O objetivo desse tutorial é mostrar como integrar o GaggleAMP ao Azure AD (Azure Active Directory).

A integração do GaggleAMP ao Azure AD oferece os seguintes benefícios:

* No Azure AD, é possível controlar quem tem acesso ao GaggleAMP
* Você pode permitir seus usuários façam logon automaticamente no GaggleAMP usando SSO (logon único) com suas contas do Azure AD
* Gerenciar suas contas em um único local: o Portal clássico do Azure 

Para conhecer mais detalhadamente a integração de aplicativos de SaaS ao AD do Azure, consulte [O que é o acesso a aplicativos e logon único com o Active Directory do Azure](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Pré-requisitos
Para configurar a integração do Azure AD ao GaggleAMP, você precisa dos seguintes itens:

* Uma assinatura do AD do Azure
* Uma assinatura do GaggleAMP com SSO habilitado

> [!NOTE]
> Para testar as etapas deste tutorial, nós não recomendamos o uso de um ambiente de produção.
> 
> 

Para testar as etapas deste tutorial, você deve seguir estas recomendações:

* Não use o ambiente de produção, a menos que seja necessário.
* Se não tiver um ambiente de avaliação do Azure AD, você pode obter uma [versão de avaliação de um mês](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
O objetivo deste tutorial é permitir que você teste o SSO do Azure AD em um ambiente de teste. 

O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adição do GaggleAMP da galeria
2. Configurar e testar o SSO do Azure AD

## <a name="add-gaggleamp-from-the-gallery"></a>Adicionar o GaggleAMP da galeria
Para configurar a integração do GaggleAMP com o Azure AD, você precisa adicionar o GaggleAMP à sua lista de aplicativos SaaS gerenciados a partir da galeria.

**Para adicionar o GaggleAMP a partir da galeria, execute as seguintes etapas:**

1. No **portal clássico do Azure**, no painel de navegação à esquerda, clique em **Active Directory**. 
   
    ![Active Directory][1]
2. Na lista **Diretório** , selecione o diretório para o qual você deseja habilitar a integração de diretórios.
3. Para abrir a visualização dos aplicativos, na exibição do diretório, clique em **Aplicativos** no menu principal.
   
    ![Aplicativos][2]
4. Clique em **Adicionar** na parte inferior da página.
   
    ![Aplicativos][3]
5. Na caixa de diálogo **O que você deseja fazer**, clique em **Adicionar um aplicativo da galeria**.
   
    ![Aplicativos][4]
6. Na caixa de pesquisa, digite **GaggleAMP**.
    ![Criação de um usuário de teste do AD do Azure](./media/active-directory-saas-gaggleamp-tutorial/tutorial_gaggleamp_01.png)
7. No painel de resultados, selecione **GaggleAMP** e clique em **Concluir** para adicionar o aplicativo.
   
    ![Criação de um usuário de teste do AD do Azure](./media/active-directory-saas-gaggleamp-tutorial/tutorial_gaggleamp_02.png)>

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar logon único do Azure AD
O objetivo desta seção é mostrar como configurar e testar o SSO do Azure AD com o GaggleAMP, com base em um usuário de teste chamado "Brenda Fernandes".

Para que o SSO funcione, o Azure AD precisa saber qual usuário do GaggleAMP é equivalente a um usuário do Azure AD. Em outras palavras, é necessário estabelecer uma relação de vínculo entre um usuário do Azure AD e o usuário relacionado no GaggleAMP.

Essa relação de vínculo é estabelecida atribuindo o valor do **nome de usuário** no Azure AD ao valor do **Nome de Usuário** no GaggleAMP.

Para configurar e testar o SSO do Azure AD com o GaggleAMP, você precisa concluir os seguintes blocos de construção:

1. **[Configurar logon único do Azure AD](#configuring-azure-ad-single-single-sign-on)** – para habilitar seus usuários a usar esse recurso.
2. **[Criação de um usuário de teste do AD do Azure](#creating-an-azure-ad-test-user)** - para testar o logon único do AD do Azure com Brenda Fernandes.
3. **[Criação de um usuário de teste do GaggleAMP](#creating-a-GaggleAMP-test-user)** : para ter um equivalente de Brenda Fernandes no GaggleAMP que esteja vinculado à representação dela no Azure AD.
4. **[Atribuição do usuário de teste do AD do Azure](#assigning-the-azure-ad-test-user)** - para habilitar Britta Simon a usar o logon único do Azure AD.
5. **[Teste do logon único](#testing-single-sign-on)** – para verificar se a configuração funciona.

### <a name="configure-azure-ad-sso"></a>Configurar o SSO do Azure AD
O objetivo desta seção é habilitar o SSO do Azure AD no Portal Clássico do Azure e configurar o SSO em seu aplicativo GaggleAMP.

**Para configurar o SSO do Azure AD com o GaggleAMP, execute as seguintes etapas:**

1. No portal clássico do Azure, na página de integração do aplicativo **GaggleAMP**, clique em **Configurar logon único** para abrir a caixa de diálogo **Configurar Logon Único**.
   
    ![Configurar o logon único][6] 
2. Na página **Como você deseja que os usuários façam logon no GaggleAMP**, selecione **Logon Único do Azure AD** e clique em **Avançar**.
   
    ![Configurar Logon Único](./media/active-directory-saas-gaggleamp-tutorial/tutorial_gaggleamp_03.png) 
3. Na página de diálogo **Definir Configurações de Aplicativo** , execute as seguintes etapas:
   
    ![Configurar Logon Único](./media/active-directory-saas-gaggleamp-tutorial/tutorial_gaggleamp_04.png) 
   1. Na caixa de texto URL de Logon, digite a URL usada pelos usuários para fazer logon em seu aplicativo do GaggleAMP usando o seguinte padrão: **“https://secure4.gaggleamp.com”**.

    >[!NOTE]
    >Entre em contato com a [equipe de vendas do GaggleAMP](mailto:sales@gaggleamp.com) se precisar da **URL de Logon** para seu aplicativo.”
    >
   2. Clique em **Próximo**.

4. Na página **Configurar logon único no GaggleAMP** , execute as seguintes etapas:
   
    ![Configurar o logon único](./media/active-directory-saas-gaggleamp-tutorial/tutorial_gaggleamp_05.png)   
  1. Clique em **Baixar Certificado**e salve o arquivo em seu computador. Precisaremos desse certificado e das URLs de metadados (ID da Entidade, URL de Entrada e URL de saída de SSO) para configurar o SSO no lado do GaggleAMP.
  2. Clique em **Próximo**.
5. Em outra instância do navegador, navegue até a página de SSO do SAML criada para você pela equipe de suporte do Gaggle (por exemplo: *https://accounts.gaggleamp.com/saml_configurations/oXH8sQcP79dOzgFPqrMTyw/edit*).
6. Na página **SSO de SAML** , execute as seguintes etapas:  
   
    ![Logon único do GaggleAMP](./media/active-directory-saas-gaggleamp-tutorial/tutorial_gaggleamp_06.png)  
  1. No portal clássico do Azure, copie a **URL do Emissor** e cole-a na caixa de texto **URL do Provedor de Identidade**.  
  2. No portal clássico do Azure, copie a **URL do Serviço de Logon Único** e cole-a na caixa de texto **URL de Logon Único do Provedor de Identidade**. 
  3. Clique em **Salvar**      
  4. Envie o certificado baixado à [equipe de vendas do GaggleAMP](mailto:sales@gaggleamp.com).
5. No portal clássico do Azure, selecione a confirmação da configuração de logon único e clique em **Avançar**.
   
    ![Logon Único do AD do Azure][10]
6. Na página **Confirmação de logon único**, clique em **Concluir**.  
   
    ![Logon Único do AD do Azure][11]

### <a name="create-an-azure-ad-test-user"></a>Criar um usuário de teste do Azure AD
O objetivo desta seção é criar um usuário de teste no Portal Clássico do Azure chamado Brenda Fernandes.

![Criar um usuário do AD do Azure][20]

**Para criar um usuário de teste no AD do Azure, execute as seguintes etapas:**

1. No **portal clássico do Azure**, no painel de navegação à esquerda, clique em **Active Directory**.
   
    ![Criação de um usuário de teste do AD do Azure](./media/active-directory-saas-gaggleamp-tutorial/create_aaduser_09.png) 
2. Na lista **Diretório** , selecione o diretório para o qual você deseja habilitar a integração de diretórios.
3. Para exibir a lista de usuários, no menu na parte superior, clique em **Usuários**.
   
    ![Criação de um usuário de teste do AD do Azure](./media/active-directory-saas-gaggleamp-tutorial/create_aaduser_03.png) 
4. Para abrir a caixa de diálogo **Adicionar Usuário**, na barra de ferramentas na parte inferior, clique em **Adicionar Usuário**.
   
    ![Criação de um usuário de teste do AD do Azure](./media/active-directory-saas-gaggleamp-tutorial/create_aaduser_04.png) 
5. Na página do diálogo **Conte-nos sobre este usuário** , realize as seguintes etapas:
   
    ![Criação de um usuário de teste do AD do Azure](./media/active-directory-saas-gaggleamp-tutorial/create_aaduser_05.png) 
  1. Em Tipo de Usuário, selecione Novo usuário na organização.  
  2. Na **caixa de texto** Nome do Usuário, digite **BrendaFernandes**.
  3. Clique em **Próximo**.
6. Na página do diálogo **Perfil do Usuário** , realize as seguintes etapas:
   
   ![Criação de um usuário de teste do AD do Azure](./media/active-directory-saas-gaggleamp-tutorial/create_aaduser_06.png) 
  1. Na caixa de texto **Nome**, digite **Brenda**.  
  2. Na caixa de texto **Sobrenome**, digite **Fernandes**.
  3. Na caixa de texto **Nome de Exibição**, digite **Brenda Fernandes**.
  4. Na lista **Função**, selecione **Usuário**.
  5. Clique em **Próximo**.
7. Na página de diálogo **Obter senha temporária**, clique em **criar**.
   
    ![Criação de um usuário de teste do AD do Azure](./media/active-directory-saas-gaggleamp-tutorial/create_aaduser_07.png) 
8. Na página de caixa de diálogo **Obter senha temporária** , execute as seguintes etapas:
   
    ![Criação de um usuário de teste do AD do Azure](./media/active-directory-saas-gaggleamp-tutorial/create_aaduser_08.png) 
  1. Anote o valor da **Nova Senha**.
  2. Clique em **Concluído**.   

### <a name="create-a-gaggleamp-test-user"></a>Criar um usuário de teste do GaggleAMP
O objetivo desta seção é criar um usuário chamado Brenda Fernandes no GaggleAMP. O GaggleAMP dá suporte ao provisionamento just-in-time, que está habilitado por padrão.

Não há itens de ação para você nesta seção. Um novo usuário será criado durante uma tentativa de acessar o GaggleAMP, caso ainda não exista. 

### <a name="assign-the-azure-ad-test-user"></a>Atribuir o usuário de teste do Azure AD
O objetivo desta seção é permitir que Brenda Fernandes use o SSO do Azure, concedendo a ela acesso ao GaggleAMP.

![Atribuir usuário][200] 

**Para atribuir Brenda Fernandes ao GaggleAMP, execute as seguintes etapas:**

1. No portal do Azure, para abrir a exibição de aplicativos, na exibição de diretório, clique em **Aplicativos** no menu principal.
   
    ![Atribuir usuário][201] 
2. Na lista de aplicativos, selecione **GaggleAMP**.
    ![Lista do Azure](./media/active-directory-saas-gaggleamp-tutorial/tutorial_gaggleamp_50.png)
3. No menu na parte superior, clique em **Usuários**.
   
    ![Atribuir usuário][203] 
4. Na lista de usuários, selecione **Brenda Fernandes**.
5. Na barra de ferramentas na parte inferior, clique em **Atribuir**.
   
    ![Atribuir usuário][205]

### <a name="test-single-sign-on"></a>Testar logon único
O objetivo desta seção é testar sua configuração de SSO do Azure AD usando o Painel de Acesso.

Ao clicar no bloco GaggleAMP no Painel de Acesso, você deve ser automaticamente conectado a seu aplicativo GaggleAMP.

## <a name="additional-resources"></a>Recursos adicionais
* [Lista de tutoriais sobre como integrar aplicativos SaaS com o Active Directory do Azure](active-directory-saas-tutorial-list.md)
* [O que é o acesso a aplicativos e logon único com o Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-gaggleamp-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-gaggleamp-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-gaggleamp-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-gaggleamp-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-gaggleamp-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-gaggleamp-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-gaggleamp-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-gaggleamp-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-gaggleamp-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-gaggleamp-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-gaggleamp-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-gaggleamp-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-gaggleamp-tutorial/tutorial_general_205.png

