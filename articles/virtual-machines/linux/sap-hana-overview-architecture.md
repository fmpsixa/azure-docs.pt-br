---
title: "Visão geral e arquitetura do SAP HANA no Azure (Instâncias Grandes) | Microsoft Docs"
description: "Visão geral da arquitetura de como implantar SAP HANA no Azure (Instâncias Grandes)."
services: virtual-machines-linux
documentationcenter: 
author: RicksterCDN
manager: timlt
editor: 
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 12/01/2016
ms.author: rclaus
ms.custom: H1Hack27Feb2017
translationtype: Human Translation
ms.sourcegitcommit: fa842efd99718be7fa9eaf8aac8030c32cbceeec
ms.openlocfilehash: a412304c33d915be6e30c6b748c8431a9c7e9f34
ms.lasthandoff: 03/01/2017


---
# <a name="sap-hana-large-instances-overview-and-architecture-on-azure"></a>Visão geral e arquitetura do SAP HANA (instâncias grandes) no Azure 
Este é um guia com cinco partes de arquitetura e implantação técnica que fornece informações para ajudar você a implantar o SAP no novo SAP HANA no Azure (Instâncias Grandes). Ele não é abrangente e não aborda detalhes específicos que envolvem instalação de soluções SAP. Em vez disso, fornece informações valiosas para ajudar com sua implantação inicial e as operações em andamento. Não o utilize para substituir a documentação do SAP relacionada à instalação do SAP HANA (nem as várias Notas de Suporte do SAP que abrangem o tópico). Ele também fornece detalhes sobre a instalação do SAP HANA no Azure (Instâncias Grandes).


As cinco partes deste guia abrangem os seguintes tópicos:

- [Visão geral e arquitetura](sap-hana-overview-architecture.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
- [Infraestrutura e conectividade](sap-hana-overview-infrastructure-connectivity.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
- [Instalação do SAP HANA](sap-hana-overview-sap-hana-installation.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
- [Alta Disponibilidade e Recuperação de Desastre](sap-hana-overview-high-availability-disaster-recovery.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
- [Solução de problemas e monitoramento](sap-hana-overview-troubleshooting-monitoring.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

## <a name="definitions"></a>Definições

Várias definições comuns são amplamente usadas no guia de implantação técnica e arquitetura. Observe os seguintes termos e seus significados:

- **IaaS:** Infraestrutura como Serviço
- **PaaS:** Plataforma como Serviço
- **SaaS:** Software como Serviço
- **Componente SAP:** um aplicativo SAP individual como ECC, BW, Solution Manager ou EP. Os componentes SAP podem ser baseados em tecnologias ABAP ou Java tradicionais ou em um aplicativo não baseado em NetWeaver, como o Business Objects.
- **Ambiente SAP:** um ou mais componentes SAP agrupados logicamente para executar uma função de negócios, como Desenvolvimento, QAS, Treinamento, DR ou Produção.
- **Paisagem SAP:** refere-se à totalidade dos ativos SAP na estrutura de TI de um cliente. A estrutura da SAP inclui todos os ambientes de produção e de não produção.
- **Sistema SAP:** a combinação de camada DBMS e camada de aplicativo de um sistema de desenvolvimento SAP ERP, sistema de teste SAP BW, sistema de produção SAP CRM etc. Implantações do Azure não dão suporte a divisão essas duas camadas entre local e o Azure. Isso significa que um sistema SAP é implantado localmente ou no Azure. No entanto, você pode implantar os diferentes sistemas de uma estrutura da SAP no Azure ou localmente. Por exemplo, você poderia implantar os sistemas de desenvolvimento e teste SAP CRM no Azure, enquanto implanta o sistema de produção CRM SAP localmente. No caso do SAP HANA no Azure (Instâncias Grandes), há suporte para executar a camada do aplicativo SAP de sistemas SAP em VMs do Azure e a instância do HANA em uma unidade no carimbo de Instância Grande.
- **Grande carimbo da instância:** uma pilha de infraestrutura de hardware é TDI do SAP HANA certificados e dedicado para executar instâncias do SAP HANA no Azure.
- **SAP HANA no Azure (Instâncias Grandes):** nome oficial para a oferta no Azure para executar instâncias do HANA em hardware certificado por TDI do SAP HANA implantado em carimbos de Instância Grande em diferentes regiões do Azure. O termo relacionado **HANA em Instâncias Grandes** é a abreviação de SAP HANA no Azure (Instâncias Grandes) e é amplamente usado neste guia de implantação técnica.
- **Entre Locais:** descreve um cenário em que as VMs são implantadas em uma assinatura do Azure com conectividade site a site, multissite ou do ExpressRoute entre o(s) datacenter(s) local(is) e o Azure. Na documentação comum do Azure, esses tipos de implantações também são descritas como cenários entre instalações. O motivo para a conexão é estender domínios locais, Active Directory/OpenLDAP local e DNS local para o Azure. A estrutura local é estendida para os ativos do Azure da assinatura. Com esta extensão, as VMs podem ser parte do domínio local. Usuários de domínio do domínio local podem acessar os servidores e podem executar serviços nessas VMs (como serviços DBMS). A comunicação e resolução de nomes entre máquinas virtuais implantadas localmente e VMs implantadas no Azure são possíveis. Esse é o cenário típico no qual a maioria dos SAP ativos serão implantados. Consulte [de planejamento e design para o Gateway de VPN](../../vpn-gateway/vpn-gateway-plan-design.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) e [criar uma rede virtual com uma conexão Site a Site usando o portal do Azure](../../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) para obter mais informações.

Há uma variedade de recursos adicionais que foram publicadas no tópico de implantação SAP a carga de trabalho na nuvem pública do Microsoft Azure. É altamente recomendável que qualquer pessoa Planejando uma implantação do SAP HANA no Azure ser experiente e ciente das entidades de IaaS do Azure e a implantação de cargas de trabalho do SAP no Azure IaaS. Os recursos a seguir fornecem mais informações e devem ser consultados antes de continuar:

- [Uso do SAP em VMs (máquinas virtuais) do Windows](../virtual-machines-windows-sap-get-started.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
- [Uso das soluções SAP em máquinas virtuais do Microsoft Azure](../virtual-machines-linux-sap-get-started.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

## <a name="certification"></a>Certificação

Além de certificação NetWeaver, SAP exige uma certificação especial para SAP HANA dar suporte a SAP HANA em determinadas infraestruturas, como IaaS do Azure.

O SAP Note principal no NetWeaver e, até certo grau, na certificação do SAP HANA, é [SAP Note nº 1928533 – aplicativos SAP no Azure: produtos com suporte e tipos de VM do Azure](https://launchpad.support.sap.com/#/notes/1928533).

Este [SAP Note nº 2316233 - SAP HANA no Microsoft Azure (Instâncias Grandes)](https://launchpad.support.sap.com/#/notes/2316233/E) também é significativo. Ele aborda a solução descrita neste guia. Além disso, você têm suporte para executar SAP HANA no tipo GS5 VM do Azure. [Informações para esse caso são publicadas no site da SAP](http://global.sap.com/community/ebook/2014-09-02-hana-hardware/enEN/iaas.html).

O SAP HANA na solução do Azure (Instâncias Grandes) mencionada no SAP Note nº 2316233 fornece aos clientes Microsoft e SAP a capacidade de implantar grandes cargas de trabalho do SAP Business Suite, do SAP Business Warehouse (BW), do S/4 HANA, do BW/4HANA ou outras do SAP HANA no Azure. Isso se baseia no carimbo de hardware dedicado certificado do SAP HANA ([TDI – Tailored Datacenter Integration do SAP HANA](https://scn.sap.com/docs/DOC-63140)). A execução como uma solução configurada do SAP TDI oferece a confiança de saber que todos os aplicativos baseados no SAP HANA (incluindo o SAP Business Suite no SAP HANA, SAP Business Warehouse (BW) SAP HANA, S4/HANA e BW4/HANA) funcionarão.

Comparado à execução SAP HANA em máquinas virtuais Azure essa solução tem um benefício — fornece para volumes muito maiores de memória. Se você deseja habilitar esta solução, há alguns aspectos fundamentais para entender:

- Os aplicativos de camada e os aplicativos não SAP executados em VMs (Máquinas Virtuais) do Azure hospedados nos carimbos de hardware do Azure usuais.
- Implantações de infraestrutura, data centers e aplicativos de cliente locais são conectadas à plataforma de nuvem do Microsoft Azure por meio do ExpressRoute do Azure (recomendado) ou rede Virtual privada (VPN). Active Directory (AD) e DNS também se estendem para o Azure.
- A instância de banco de dados do SAP HANA para carga de trabalho do HANA é executada no SAP HANA no Azure (Instâncias Grandes). O carimbo de Instância Grande está conectado à rede do Azure para que o software em execução nas VMs do Azure possa interagir com a instância do HANA em execução no HANA em Instâncias Grandes.
- Hardware do SAP HANA no Azure (Instâncias Grandes) é fornecido em uma infraestrutura como serviço (IaaS) com o SUSE Linux Enterprise Server de hardware dedicado ou Red Hat Enterprise Linux, pré-instalado. Como com as máquinas virtuais do Azure, ainda mais as atualizações e manutenção para o sistema operacional é sua responsabilidade.
- Instalação do HANA ou todos os componentes adicionais necessários para executar SAP HANA em unidades de instâncias HANA grandes é sua responsabilidade, assim como todos os respectivos operações contínuas e administração do SAP HANA no Azure.
- Além das soluções descritas aqui, você pode instalar outros componentes na sua assinatura do Azure que se conecta ao SAP HANA no Azure (Instâncias Grandes).  Por exemplo, os componentes que permitem a comunicação com e/ou diretamente para o banco de dados do SAP HANA (servidores de salto, RDP servidores SAP HANA Studio, serviços de dados do SAP para cenários SAP BI, ou soluções de monitoramento de rede).
- Como no Azure, o HANA em Instâncias Grandes oferece suporte às funcionalidades de Alta Disponibilidade e de Recuperação de Desastre.

## <a name="architecture"></a>Arquitetura

Em um alto nível, a solução do SAP HANA no Azure (Instâncias Grandes) tem a camada do aplicativo SAP que reside nas VMs do Azure e a camada de banco de dados que reside em hardware TDI SAP configurado localizado em um carimbo de Instância Grande na mesma região do Azure que está conectado ao Azure IaaS.

> [!NOTE]
> A camada do aplicativo SAP precisa ser implantada na mesma região do Azure com a camada de DBMS SAP. Isso é bem documentado em informações publicadas sobre cargas de trabalho do SAP no Azure.

A arquitetura geral do SAP HANA no Azure (Instâncias Grandes) fornece que uma configuração de hardware TDI SAP certificada (não virtualizada, bare metal, servidor de alto desempenho para o banco de dados do SAP HANA) e a capacidade e a flexibilidade do Azure para dimensionar os recursos da camada de aplicativo SAP para atender às suas necessidades.

![Visão geral da arquitetura do SAP HANA no Azure (Instâncias Grandes)](./media/sap-hana-overview-architecture/image1-architecture.png)

A arquitetura acima é dividida em três seções:

- **Direita:** uma infraestrutura local executando aplicativos diferentes em data centers com usuários finais acesso a aplicativos de LOB (como SAP). Idealmente, esse local infraestrutura é conectada ao Azure com o Azure [ExpressRoute](https://azure.microsoft.com/services/expressroute/).

- **Centro:** mostra Azure IaaS e, nesse caso, o uso de máquinas virtuais do Azure para hospedar SAP ou outros aplicativos que aproveitam SAP HANA como um sistema DBMS. Instâncias HANA menores que fornecem função com a memória VMs do Azure são implantadas em VMs do Azure junto com sua camada de aplicativo. Saiba mais sobre [Máquinas virtuais](https://azure.microsoft.com/services/virtual-machines/).
<br />Rede do Azure é usada para agrupar os sistemas SAP junto com outros aplicativos em redes virtuais do Azure (VNets). Essas redes virtuais se conectar a sistemas locais, bem como SAP HANA no Azure (Instâncias Grandes).
<br />Para aplicativos SAP NetWeaver e bancos de dados que têm suporte para executar no Microsoft Azure, consulte [1928533 de n º de observação de suporte do SAP – aplicativos SAP no Azure: tipos de produtos com suporte e VM do Azure](https://launchpad.support.sap.com/#/notes/1928533). Para obter a documentação sobre como implantar soluções SAP no Azure, verifique:

  -  [Uso do SAP em VMs (máquinas virtuais) do Windows](../virtual-machines-windows-sap-get-started.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
  -  [Uso das soluções SAP em máquinas virtuais do Microsoft Azure](../virtual-machines-linux-sap-get-started.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

- **À esquerda:** mostra o hardware certificado por TDI do SAP HANA no carimbo de Instância Grande do Azure. As unidades do HANA em Instância Grande estão conectadas a redes virtuais do Azure de sua assinatura usando a mesma tecnologia como a conectividade do local para o Azure.

O carimbo de Instância Grande do Azure em si combina os seguintes componentes:

- **Computação:** servidores baseados em processadores Intel Xeon E7-8890v3 que fornecem a capacidade de computação necessária e são certificados pelo SAP HANA.
- **Rede:** A Unificação da malha de rede de alta velocidade que interconexões de computação, armazenamento e componentes de LAN.
- **Armazenamento:** uma infraestrutura de armazenamento que é acessada por meio de uma malha de rede unificada. Capacidade de armazenamento específica é fornecida dependendo do SAP HANA específico na configuração do Azure (Instâncias Grandes) que está sendo implantada (mais capacidade de armazenamento está disponível a um custo mensal adicional).

Na infraestrutura multilocatária do carimbo de Instância Grande, os clientes são implantados como locatários isolados. Esses locatários têm uma relação de 1:1 para a assinatura do Azure. Isso significa que você não pode acessar a mesma instância do SAP HANA no Azure (Instâncias Grandes) que é executado em um carimbo de data / instância grande do Azure fora de duas diferentes assinaturas do Azure.

Assim como acontece com VMs do Azure, SAP HANA no Azure (Instâncias Grandes) é oferecido em várias regiões do Azure. Para oferecer recursos de Recuperação de Desastre, você pode optar por aceitar. Os carimbos de Instância Grande diferentes em várias regiões do Azure são conectados uns aos outros.

Assim como você pode escolher entre diferentes tipos de VM com as máquinas virtuais do Azure, você pode escolher entre diferentes SKUs do HANA em Instâncias Grandes que são personalizados para tipos diferentes de carga de trabalho do SAP HANA. SAP se aplica a memória para taxas de soquete de processador para diferentes cargas de trabalho com base nas gerações de processadores Intel, há quatro tipos diferentes de SKU oferecidos:

A partir de dezembro de 2016, o SAP HANA no Azure (Instâncias Grandes) está disponível em seis configurações nas Regiões Oeste dos EUA e Leste dos EUA do Azure:

| Solução SAP | CPU | RAM | Armazenamento |
| --- | --- | --- | --- |
| Otimizado para OLAP: SAP BW, BW/4HANA<br /> ou SAP HANA para carga de trabalho OLAP genérica | SAP HANA no Azure S72<br /> – 2 x processadores Intel® Xeon® E7-8890 v3 |  768 GB |  3 TB |
| --- | SAP HANA no Azure S144<br /> – 4 x processadores Intel® Xeon® E7-8890 v3 |  1,5 TB |  6 TB |
| --- | SAP HANA no Azure S192<br /> – 4 x processadores Intel® Xeon® E7-8890 v4 |  2,0 TB |  8 TB |
| Otimizado para OLTP: SAP Business Suite<br /> no SAP HANA ou S/4HANA (OLTP),<br /> OLTP genérico | SAP HANA no Azure S72m<br /> – 2 x processadores Intel® Xeon® E7-8890 v3 |  1,5 TB |  6 TB |
|---| SAP HANA no Azure S144m<br /> – 4 x processadores Intel® Xeon® E7-8890 v3 |  3,0 TB |  12 TB |
|---| SAP HANA no Azure S192m<br /> – 4 x processadores Intel® Xeon® E7-8890 v4 |  4,0 TB |  16 TB |

As diferentes configurações acima são referenciadas no [SAP Note de Suporte nº 2316233 – SAP HANA no Microsoft Azure (Instâncias Grandes)](https://launchpad.support.sap.com/#/notes/2316233/E).

As configurações específicas de escolhido dependem da carga de trabalho, recursos de CPU e memória desejada. É possível que a carga de trabalho OLTP aproveite os SKUs otimizados para cargas de trabalho OLAP. Da mesma forma, você pode aproveitar os SKUs OLTP para carga de trabalho do OLAP HANA. Embora talvez seja necessário restringir a memória utilizada pelo HANA à memória certificada com a geração de processador Intel E7 como [listados no site da SAP](http://global12.sap.com/community/ebook/2014-09-02-hana-hardware/enEN/appliances.html) para o tipo de dispositivo _Escalar Verticalmente BW no HANA_

É importante observar que toda a infraestrutura do carimbo de Instância Grande não está alocada exclusivamente para uso de um único cliente. Isso se aplica ao SAP HANA no Azure (Instâncias Grandes), assim como acontece com racks de recursos de computação e armazenamento conectados por meio de uma malha de rede implantada no Azure. A infraestrutura do HANA em Instâncias Grandes, como o Azure, implanta &quot;locatários&quot; diferentes do cliente isolados uns dos outros por meio do isolamento de rede. Assim, essas implantações do HANA em Instâncias Grandes não interferem umas nas outras, nem estão visíveis umas para as outras. Um locatário implantado no carimbo de Instância Grande é atribuído a uma assinatura do Azure. Se você tiver uma segunda assinatura do Azure que também exija o HANA em Instâncias Grandes, ele deverá ser implantado em um locatário separado em um carimbo de Instância Grande com todo o isolamento de rede que impeça a comunicação direta entre essas instâncias.

No entanto, existem diferenças significativas entre executar o SAP HANA no HANA em Instâncias Grandes e o SAP HANA em execução em VMs do Azure implantadas no Azure:

- Não há nenhuma camada de virtualização para o SAP HANA no Azure (Instâncias Grandes). Você obtém o desempenho do hardware bare metal subjacente.
- Ao contrário do Azure, o servidor do servidor do SAP HANA no Azure (Instâncias Grandes) é dedicado a um cliente específico. Uma reinicialização ou desligamento do servidor não leva para o sistema operacional e SAP HANA que está sendo implantado em outro servidor. (A única exceção é quando um servidor pode encontrar problemas e a reimplantação precisa ser executada em outra folha).
- Ao contrário do Azure, em que tipos de host do processador são selecionados para melhor taxa de preço e desempenho, os tipos de processador escolhidos para o SAP HANA no Azure (Instâncias Grandes) têm o melhor desempenho da linha de processadores Intel E7v3.

Haverá vários clientes implantando no hardware do SAP HANA no Azure (Instâncias Grandes) e cada um está blindado em relação aos outros por meio da implantação em suas próprias VLANs. Para se conectar ao HANA em Instâncias Grandes em uma rede Virtual do Azure (VNet), os componentes de rede no local executam conversão de endereços de rede (NAT) entre o intervalo de endereços IP de rede virtual do Azure e o espaço de endereço IP VLAN dentro da infraestrutura de hardware.

## <a name="operations-model-and-responsibilities"></a>Responsabilidades e modelo de operações

O serviço fornecido com SAP HANA no Azure (Instâncias Grandes) é alinhado com os serviços do Azure IaaS. Você obtém uma instância do HANA em Instâncias Grandes com um sistema operacional instalado otimizado para o SAP HANA. Assim como acontece com as VMs do Azure IaaS, a maioria das tarefas de proteção de sistema operacional, instalar software adicional é necessário, instalando HANA, o sistema operacional e HANA, operacionais e atualizar o sistema operacional e HANA é sua responsabilidade. Microsoft não forçará atualizações do sistema operacional ou HANA para você.

![Responsabilidades do SAP HANA no Azure (Instâncias Grandes)](./media/sap-hana-overview-architecture/image2-responsibilities.png)

Como você pode ver no diagrama acima, SAP HANA no Azure (Instâncias Grandes) é um infraestrutura de multilocatário como uma oferta de serviço. E como resultado, a divisão de responsabilidade é o limite de infraestrutura de sistema operacional, na maior parte. A Microsoft é responsável por todos os aspectos do serviço abaixo da linha do sistema operacional e é responsável acima da linha, incluindo o sistema operacional. Então mais recentes métodos local que você pode aplicar para conformidade, segurança, gerenciamento de aplicativos, base e gerenciamento do sistema operacional podem continuar a ser usado. Os sistemas aparecem como se estivessem em sua rede em todos os aspectos.

No entanto, esse serviço é otimizado para SAP HANA, portanto, há áreas onde você e a Microsoft precisam trabalhar juntos para usar os recursos de infraestrutura subjacente para obter melhores resultados.

A lista a seguir fornece mais detalhes sobre cada uma das camadas e suas responsabilidades:

**Rede:** todas as redes internas para o carimbo de Instância Grande executando o SAP HANA, seu acesso a conectividade com o Azure, armazenamento e conectividade entre as instâncias (para expansão e outras funções), conectividade para o cenário em que a camada do aplicativo SAP é hospedada em máquinas virtuais do Azure. Ele também inclui conectividade WAN entre Data Centers do Azure para replicação de objetivos de Recuperação de Desastre. Todas as redes são particionadas pelo locatário e aplicou QOS.

**Armazenamento:** o virtualizado particionada armazenamento para todos os volumes necessários pelos servidores do SAP HANA, bem como para instantâneos.

**Servidores:** os servidores físicos dedicados para executar os bancos de dados SAP HANA atribuídos aos locatários. Eles são hardware virtualizado.

**SDDC:** centraliza o software de gerenciamento que é usado para gerenciar os dados como uma entidade de software definida. Ele permite que a Microsoft para recursos do pool por motivos de desempenho, disponibilidade e dimensionamento.

**Sistema operacional:** o sistema operacional que você escolher (SUSE Linux ou Red Hat Linux) que é executado em servidores. As imagens do SO que você recebe serão aquelas fornecidas pelo fornecedor individual do Linux à Microsoft para a finalidade de execução do SAP HANA. Você precisa ter uma assinatura com o fornecedor do Linux para receber a imagem específica otimizada para SAP HANA. Suas responsabilidades incluem o registro das imagens com o fornecedor do SO. Depois da entrega feita pela Microsoft, você também é responsável por qualquer aplicação de patch no sistema operacional Linux. Essa aplicação está relacionada aos pacotes adicionais que podem ser necessários para uma instalação bem-sucedida do SAP HANA (consulte a documentação de instalação do SAP HANA e as Notas da SAP) e que não foram incluídos pelo fornecedor específico do Linux nas respectivas imagens de SO otimizadas para SAP HANA. A responsabilidade do cliente também inclui a aplicação de patch do SO que está relacionado ao mau funcionamento/otimização do SO e seus drivers em relação ao hardware de servidor específico. Ou qualquer aplicação de patch de segurança ou funcional do SO. Também é responsabilidade do cliente monitorar e planejar a capacidade do(s):

- Consumo de recursos de CPU
- Consumo de memória
- Volumes de disco relacionados ao espaço livre, IOPS e latência
- Tráfego de volume de rede entre o Hana Instância Grande e a camada do aplicativo SAP

A infraestrutura subjacente do HANA em Instâncias Grandes fornece a funcionalidade de backup e restauração do volume do sistema operacional. Aproveitando essa funcionalidade também é sua responsabilidade.

**Middleware:** instância do SAP HANA, principalmente. Administração, operações e monitoramento são de sua responsabilidade. Não há funcionalidade fornecida que permite que você use instantâneos de armazenamento para fins de Recuperação de Desastre e backup/restauração. Essas funcionalidades são fornecidas pela infraestrutura. No entanto, suas responsabilidades incluem também a arquitetura de alta disponibilidade ou Recuperação de Desastre com essas funcionalidades, aproveitando-los e monitoramento que instantâneos de armazenamento tem sido executados com êxito.

**Dados:** seus dados gerenciados pelo SAP HANA e outros dados, como backups de arquivos localizados em volumes ou compartilhamentos de arquivos. Suas responsabilidades incluem monitoramento espaço livre em disco e gerenciamento de conteúdo em volumes e a execução bem-sucedida de backups de volumes de disco e instantâneos de armazenamento. No entanto, a execução bem-sucedida da replicação de dados para locais de DR é a responsabilidade da Microsoft.

**Aplicativos:** as instâncias do aplicativo SAP ou, no caso de aplicativos não-SAP, a camada de aplicativo desses aplicativos. Suas responsabilidades incluem a implantação, administração, operações e monitoramento dos aplicativos relacionados ao planejamento de capacidade de consumo de recursos de CPU, consumo de memória, consumo de armazenamento do Azure e consumo de largura de banda de rede nas redes virtuais do Azure e de redes virtuais do Azure para SAP HANA no Azure (Instâncias Grandes).

**WANs:** as conexões que você estabelece do local para implantações do Azure para cargas de trabalho. Se as cargas de trabalho são de missão crítica, use a ExpressRoute do Azure. Esta conexão não é parte do SAP HANA na solução do Azure (Instâncias Grandes), você é responsável pela instalação desta conexão.

**Arquivo:** talvez você prefira arquivar cópias dos dados usando seus próprios métodos em contas de armazenamento. Isso requer gerenciamento, conformidade, custos e operações. Você é responsável por gerar cópias de arquivos e backups no Azure e armazená-los de uma maneira em conformidade.

Configurações para o SAP HANA Veja o [SLA para o SAP HANA no Azure (Instâncias Grandes)](https://azure.microsoft.com/support/legal/sla/sap-hana-large/v1_0/).

## <a name="sizing"></a>Dimensionamento

O dimensionamento do HANA em Instâncias Grandes não é diferente de dimensionamento para o HANA em geral. Existentes e a implantação de sistemas, você deseja mover de outros RDBMS para HANA, SAP fornece vários relatórios que são executados em seus sistemas SAP existentes. Eles verificam os dados e calculam os requisitos de memória de tabela se o banco de dados é movido HANA. Leia as seguintes observações do SAP para obter mais informações sobre como executar esses relatórios e como obter seus patches/versões mais recentes:

- SAP Note nº 1793345 - dimensionamento do SAP Suite no HANA
- SAP Note nº 1872170 - relatório de dimensionamento do Suite no HANA e S/4 HANA
- SAP Note nº 2121330 - perguntas frequentes: relatório de dimensionamento do SAP BW no HANA
- SAP Note nº 1736976 - relatório de dimensionamento para BW no HANA
- SAP Note nº 2296290 - novo relatório de dimensionamento para BW no HANA

Para implementações de campo verde Sizer rápida SAP está disponível para calcular os requisitos de memória da implementação de software SAP sobre HANA.

Requisitos de memória para HANA aumentará à medida que o volume de dados cresce, portanto você deve estar atento agora o consumo de memória e ser capaz de prever o que será no futuro. Com base nos requisitos de memória, você pode mapear a demanda em uma das SKUs do Hana Instância Grande.

## <a name="requirements"></a>Requisitos

Estes são os requisitos para execução SAP HANA no Azure (instâncias maiores).

**Microsoft Azure:**

- Uma assinatura do Azure que pode ser vinculada ao SAP HANA no Azure (Instâncias Grandes).
- Suporte do Microsoft Premier missão crítica. Consulte [2015553 de n º de observação de suporte do SAP – SAP no Microsoft Azure: pré-requisitos de suporte](https://launchpad.support.sap.com/#/notes/2015553) para obter informações específicas relacionadas à execução de SAP no Azure.
- O reconhecimento dos SKUs de instâncias do HANA em Instâncias Grandes será necessário após a realização de um exercício de dimensionamento com o SAP.

**Conectividade de rede:**

- Azure ExpressRoute entre locais no Azure: solicite pelo menos uma conexão de 1 Gbps do seu ISP para conectar seu data center local ao Azure

**Sistema operacional:**

- Licenças para SUSE Linux Enterprise Server 12 para aplicativos SAP.

> [!NOTE] 
> O sistema operacional fornecido pela Microsoft não está registrado no SUSE, nem está conectado a uma instância SMT.

- O SMT da assinatura do SUSE Linux é implantado no Azure em uma VM do Azure. Isso fornece a capacidade de o SAP HANA no Azure (Instâncias Grandes) ser registrado e respectivamente atualizado pelo SUSE (pois não há nenhum acesso à internet no data center do HANA em Instâncias Grandes). 
- Licenças para Red Hat Enterprise Linux 6.7 ou 7.2 para SAP HANA.

> [!NOTE]
> O sistema operacional fornecido pela Microsoft não está registrado no Red Hat, nem está conectado a uma instância do gerenciador de assinaturas do Red Hat.

- Gerenciador do Red Hat assinatura implantado no Azure em uma VM do Azure. Isso fornece a capacidade para o SAP HANA no Azure (Instâncias Grandes) ser registrado e atualizado respectivamente pelo Red Hat (pois não há nenhum acesso direto à Internet de dentro do locatário implantado no carimbo de Instância Grande do Azure).
- Contrato de serviço e suporte com o provedor do Linux que está implicitamente incluído na assinatura da versão específica do Linux ou outro contrato de serviço e suporte que esteja cobrindo a versão específica do Linux usada e que atenda aos critérios da SAP.

**Banco de dados:**

- As licenças e os componentes de instalação de software para SAP HANA (plataforma ou enterprise edition).

**Aplicativos:**

- Contratos de suporte de licenças e os componentes de instalação de software para todos os aplicativos SAP conexão SAP HANA e relacionados ao SAP.
- Licenças e os componentes de instalação de software para todos os aplicativos SAP não usado em relação a SAP HANA no ambiente do Azure (Instâncias Grandes) e relacionadas a contratos de suporte.

**Habilidades:**

- Experiência e conhecimento sobre IaaS do Azure e seus componentes.
- Experiência e conhecimento sobre a implantação da carga de trabalho do SAP no Azure.
- Instalação do SAP HANA certificados pessoal.
- As habilidades de arquiteto do SAP para design de Alta Disponibilidade e Recuperação de Desastre no SAP HANA.

## <a name="networking"></a>Rede

A arquitetura de Rede do Azure é um componente-chave para a implantação bem-sucedida de aplicativos SAP. Normalmente, as implantações do SAP HANA no Azure (Instâncias Grandes) têm uma estrutura SAP maior com diversas soluções SAP com vários tamanhos de bancos de dados, consumo de recursos de CPU e a utilização de memória. É muito provável que apenas um ou dois desses sistemas SAP se baseiem no SAP HANA, de forma que seu cenário SAP provavelmente seja um híbrido que utiliza:

- Sistemas SAP implantados no local. Devido a seus tamanhos, no momento eles não podem ser hospedados no Azure; um exemplo clássico seria um sistema SAP ERP de produção executado no Microsoft SQL Server (como o banco de dados) com &gt;100 CPUs e 1 TB de memória.
- Implantado com base em sistemas SAP HANA locais (por exemplo, um sistema de ERP SAP que exige 6 TB ou mais de memória para seu banco de dados do SAP HANA).
- Sistemas SAP implantados em VMs do Azure. Esses sistemas podem estar instâncias de desenvolvimento, teste, área restrita ou produção para qualquer um dos aplicativos baseados no SAP NetWeaver que podem implantar com êxito no Azure (em VMs), com base na demanda de memória e consumo de recursos. Eles também pode ser baseados em bancos de dados como o SQL Server (consulte [1928533 de n º de observação de suporte do SAP – aplicativos SAP no Azure: tipos de produtos com suporte e VM do Azure](https://launchpad.support.sap.com/#/notes/1928533/E)) ou SAP HANA (consulte [SAP HANA Certified IaaS plataformas](http://global.sap.com/community/ebook/2014-09-02-hana-hardware/enEN/iaas.html)).
- Servidores de aplicativos SAP implantados no Azure (em VMs) que aproveitam o SAP HANA no Azure (Instância Grande) em carimbos de Instância Grande do Azure.

Enquanto uma paisagem SAP (com quatro ou mais diferentes cenários de implantação) híbrida é típica, existem instâncias de uma estrutura completa do SAP no Azure e como VMs do Microsoft Azure se tornam cada vez mais poderosos, o número de soluções SAP a implantação em VMs do Azure aumentará.

Rede no contexto de sistemas SAP implantados no Azure o Azure não é complicado. Ele se baseia em princípios a seguir:

- Redes virtuais do Azure (VNets) precisa estar conectado ao circuito do ExpressRoute do Azure que se conecta à rede local.
- Geralmente, um circuito do ExpressRoute deve ter uma largura de banda de 1 Gbps ou superior. Isso permite que a largura de banda adequada para transferir dados entre sistemas locais e em execução em máquinas virtuais do Azure (bem como conexão com sistemas do Azure de usuários finais locais).
- Todos os sistemas SAP no Azure precisam ser configurado em redes virtuais do Azure para se comunicar entre si.
- O Active Directory e DNS hospedados em locais são estendidos para o Azure por meio do ExpressRoute.

**Recomendação:** implante a paisagem completa do SAP no Azure em uma única assinatura do Azure. Muitos procedimentos dentro de uma estrutura SAP requerem conectividade de rede transparente e possivelmente em menor, entre o desenvolvimento, teste e instâncias de produção e a arquitetura tem muitos automatisms que contam com essa rede transparente entre esses diferentes instâncias do SAP NetWeaver SAP. Portanto, tendo a estrutura completa do SAP em uma assinatura do Azure, mesmo quando o ambiente é implantado em várias regiões do Azure, é altamente recomendável.

A arquitetura e os processos em torno do SAP HANA no Azure (Instâncias Grandes) baseia a recomendação acima.

> [!NOTE] 
> Uma única assinatura do Azure pode ser vinculada somente a um único locatário em um carimbo de Instância Grande em uma determinada região do Azure e, por outro lado, um único locatário de carimbo de Instância Grande pode ser vinculado somente a uma assinatura do Azure.

A implantação do SAP HANA no Azure (Instâncias Grandes) em duas regiões diferentes do Azure fará com que um locatário separado seja implantado no carimbo de Instância Grande. No entanto, você pode esperar que ambos acabará sob a mesma assinatura do Azure desde que essas instâncias são parte do mesmo cenário SAP.

> [!IMPORTANT] 
> Implantação de gerenciamento de recursos do Azure só é compatível com SAP HANA no Azure (Instâncias Grandes).

### <a name="additional-azure-vnet-information"></a>Informações adicionais de rede virtual do Azure

Para conectar uma rede virtual do Azure a ExpressRoute, um gateway do Azure deve ser criado (consulte [sobre gateways de rede virtual para ExpressRoute](../../expressroute/expressroute-about-virtual-network-gateways.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)). Um gateway do Azure pode ser usado com a ExpressRoute para uma infraestrutura fora do Azure (ou um carimbo da instância do Azure grande), ou para se conectar entre redes virtuais do Azure (consulte [configurar uma conexão de VNet para VNet para o Gerenciador de recursos usando o PowerShell](../../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)). Você pode conectar o gateway a um máximo de quatro diferentes conexões do ExpressRoute do Azure, desde aqueles são provenientes de diferentes MS Enterprise trocas (MSEE).

> [!NOTE] 
> A taxa de transferência fornece um gateway do Azure é diferente para os dois casos de uso (consulte [sobre o Gateway de VPN](../../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)). A taxa de transferência máxima que podemos conseguir com um gateway de rede virtual é de 10 Gbps, usando uma conexão do ExpressRoute. Tenha em mente que copiar arquivos entre uma VM do Azure que residem em uma rede virtual do Azure e um sistema local (como um fluxo de cópia única) não atingirá a taxa de transferência total de SKUs de gateway diferente. Para aproveitar a largura de banda completa do gateway de rede virtual, você deve usar vários fluxos ou copiar arquivos diferentes fluxos paralelos de um único arquivo.

Como ler os arquivos acima, cuidadosamente Observe as informações no gateway UltraPerformance disponibilidade SKU em diferentes regiões do Azure.

### <a name="networking-for-sap-hana-on-azure"></a>Sistema de rede para SAP HANA no Azure

Para conexão SAP HANA no Azure (Instâncias Grandes), uma rede virtual do Azure deve estar conectada por meio de seu gateway de rede virtual usando a ExpressRoute para a rede do cliente local. Além disso, ele precisa ser conectado por meio de um segundo circuito do ExpressRoute para o HANA em Instâncias Grandes localizado em um carimbo de Instância Grande do Azure. O gateway de rede virtual terá pelo menos duas conexões do ExpressRoute e conexões compartilharão a largura de banda máxima do gateway de rede virtual.

> [!IMPORTANT] 
> Dado o tráfego de rede geral entre as camadas de banco de dados e aplicativos SAP, somente o HighPerformance ou SKUs de gateway UltraPerformance para redes virtuais tem suporte para conectar-se ao SAP HANA no Azure (Instâncias Grandes).

![Rede virtual do Azure conectado ao SAP HANA no Azure (Instâncias Grandes) e local](./media/sap-hana-overview-architecture/image3-on-premises-infrastructure.png)

### <a name="single-sap-system"></a>Único sistema SAP

A infraestrutura no local mostrada acima é conectada por meio do ExpressRoute no Azure e o circuito do ExpressRoute se conecta em um roteador de borda do Microsoft Enterprise (MSEE) (consulte [visão geral técnica da ExpressRoute](../../expressroute/expressroute-introduction.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)). Uma vez estabelecida, rota, que se conecta o backbone do Microsoft Azure e todas as regiões do Azure são acessíveis.

> [!NOTE] 
> Para executar cenários SAP no Azure, conecte-se para o MSEE mais próximo para a região do Azure no cenário SAP. Os carimbos de Instância Grande do Azure são conectados por meio de dispositivos MSEE dedicados para minimizar a latência da rede entre VMs do Azure no Azure IaaS e os carimbos de Instância Grande.

O gateway de rede virtual para VMs do Azure hospedam instâncias do aplicativo SAP está conectado a esse circuito do ExpressRoute e a mesma rede virtual está conectada a um roteador MSEE separado dedicado à conexão com carimbos de Instância Grande.

Este é um exemplo simples de um sistema SAP, onde a camada de aplicativo SAP é hospedada no Azure e o banco de dados do SAP HANA é executado em SAP HANA no Azure (Instâncias Grandes). A suposição é que a largura de banda de gateway de rede virtual de 2 Gbps ou 10 Gbps não representam um afunilamento.

### <a name="multiple-sap-systems-or-large-sap-systems"></a>Vários sistemas SAP ou grandes sistemas SAP

Se vários sistemas SAP ou grandes sistemas SAP forem implantados e se conectem ao SAP HANA no Azure (Instâncias Grandes), é razoável presumir que a taxa de transferência do SKU de gateway de rede virtual HighPerformance poderá se tornar um gargalo. Nesse caso, escolha o SKU UltraPerformance, se estiver disponível. No entanto, se apenas o SKU HighPerformance (até 2 Gbps) está disponível ou é possível que o SKU UltraPerformance (até 10 Gbps) não seja suficiente, você precisará dividir as camadas do aplicativo em várias redes virtuais do Azure.

Uma arquitetura de rede mais escalonável:

- Aproveite diversas redes virtuais do Azure para uma camada de aplicativo SAP única e maior.
- Implante uma rede virtual do Azure separado para cada sistema SAP, em comparação com a combinação desses sistemas SAP em sub-redes separadas na mesma rede virtual.

 Uma arquitetura de rede mais escalonável para o SAP HANA no Azure (Instâncias Grandes):

![Implantação de camada do aplicativo SAP em várias redes virtuais do Azure](./media/sap-hana-overview-architecture/image4-networking-architecture.png)

Implantar a camada de aplicativos, ou componentes, SAP em várias redes virtuais do Azure, como mostrado acima, introduziu sobrecarga de latência inevitável que ocorreu durante a comunicação entre os aplicativos hospedados nessas redes virtuais do Azure. Por padrão, o tráfego de rede entre VMs do Azure localizadas em diferentes redes virtuais ocorrerá por meio de Roteadores MSEE nessa configuração. No entanto, desde setembro de 2016, isso pode ser evitado e otimizado. Para otimizar e reduzir a latência na comunicação entre duas redes virtuais basta emparelhar as redes virtuais do Azure na mesma região. Mesmo que elas estejam em assinaturas diferentes. Com o emparelhamento VNet do Azure na comunicação entre VMs em duas redes virtuais diferentes do Azure, estas podem usar o backbone da rede para se comunicarem diretamente entre si. Desse modo, a latência observada é semelhante a de VMs que estão na mesma rede virtual. Enquanto isso, o tráfego que inclui intervalos de endereço IP que são conectados por meio do gateway da rede virtual do Azure é roteado pelo gateway da rede virtual individual da rede virtual. Você pode obter detalhes sobre o emparelhamento de rede virtual do Azure no artigo [Emparelhamento VNet](https://docs.microsoft.com/en-us/azure/virtual-network/virtual-network-peering-overview).
### <a name="minimal-deployment"></a>Implantação mínima

Para um pequeno sistema SAP (implantação mínima), as VMs do Azure hospedam a camada do aplicativo SAP no Azure nativo (em uma única rede de virtual) e se conectam ao carimbo de Instância Grande por meio do ExpressRoute. Siga estas etapas para obter o SAP HANA em pronto do Azure (Instâncias Grandes) para usar:

- Colete informações específicas relacionadas a quatro diferentes intervalos de endereços IP:

  1. Um intervalo de endereços /29 para conexões P2P a ser usado para os circuitos do ExpressRoute de endereços.
  
  2. Um bloco CIDR /24 (recomendado) exclusivo para ser usado para atribuir os endereços IP específicos necessários ao SAP HANA no Azure (Instâncias Grandes).
  3. Um ou mais blocos CIDR /24 (recomendado) para suas sub-redes de locatário de rede virtual do Azure. Essas são sub-redes na assinatura do Azure do cliente onde as VMs do Azure relacionados ao SAP residirão; os endereços poderão acessar o SAP HANA no Azure (Instâncias Grandes). Deve haver um bloco de endereço de locatário por sub-rede e os blocos podem ser agregados se eles forem contíguos e se estiverem mesma rede virtual.
  4. Um /28 de suas sub-redes de gateway de rede virtual (um /27 deve ser usado se quisermos rede P2S).

  - Os dois primeiros intervalos são necessários (um por assinatura e região do Azure). Os intervalos de endereços IP definidos nos itens 3 e 4 são necessários como o mínimo por rede virtual do Azure, e se várias sub-redes/locatários em uma rede virtual forem desejados, vários intervalos devem ser especificados para o item 3.
![Intervalos de endereços IP necessários na implantação mínima do SAP HANA no Azure (Instâncias Grandes)](./media/sap-hana-overview-architecture/image5-ip-address-range-a.png)

  -  Se você estiver configurando várias sub-redes de locatário em uma rede virtual do Azure: ![intervalos de endereços IP com espaço de endereço adjacente para rede virtual do Azure](./media/sap-hana-overview-architecture/image6-ip-address-range-b.png)

> [!IMPORTANT] 
> Cada intervalo de endereços IP especificado acima não deve sobrepor outro intervalo; cada um deve ser discreto e não uma sub-rede de qualquer outro intervalo. Somente o endereço definido nos itens de 3 e 4 deve ser aplicado às redes virtuais do Azure, todos os outros são usados para conectividade e roteamento de Instância Grande. Além disso, como uma prática recomendada, os intervalos de endereços do espaço de endereço devem corresponder os intervalos de sub-rede e não têm espaço vazio ou não atribuído. Se ocorrerem sobreposições entre intervalos definidos nos itens 1 e 2 com intervalos definidos para 3 e 4, a rede virtual do Azure não se conectará ao circuito do ExpressRoute.

- Um circuito do ExpressRoute é criado pela Microsoft entre sua assinatura do Azure e o carimbo de Instância Grande.
- Crie um locatário de rede no carimbo de Instância Grande.
- Configure a rede na infraestrutura do SAP HANA no Azure (Instâncias Grandes) para aceitar os endereços IP do intervalo especificado em sua rede virtual do Azure que irá se comunicar com o HANA em Instâncias Grandes.
- Configure a NAT no locatário do cliente do carimbo de Instância Grande (para que o endereço IP interno do locatário seja mapeado para um endereço IP definido pelo locatário do Azure).
- Dependendo do SKU específico do SAP HANA no Azure (Instâncias Grandes) adquirido, atribua uma unidade de computação em uma rede de locatário, alocar e montar o armazenamento e instalar o sistema operacional (SUSE ou RedHat Linux).

Implantação mínima da arquitetura de rede do SAP HANA no Azure (Instâncias Grandes):

![Implantação mínima com intervalos de endereços IP](./media/sap-hana-overview-architecture/image7-minimal-deployment.png)

### <a name="routing-in-azure"></a>Roteamento no Azure

Há duas considerações de roteamento de rede importantes para SAP HANA no Azure (Instâncias Grandes):

1. O SAP HANA no Azure (Instâncias Grandes) só pode ser acessado pelas VMs do Azure na conexão do ExpressRoute dedicada; não diretamente do local. Dessa forma, os clientes de administração e quaisquer aplicativos que precisem de acesso direto, como o SAP Solution Manager em execução no local, não podem se conectar ao banco de dados do SAP HANA.

2. O SAP HANA no Azure (Instâncias Grandes) tem um endereço IP de um conjunto definido de NAT. Esse endereço IP é acessível por meio da assinatura do Azure e o ExpressRoute. Como o endereço IP faz parte de um pool de NAT, você precisará executar a configuração de rede adicional dentro do ambiente. Veja o artigo relacionado na instalação do SAP HANA para obter detalhes.

> [!NOTE] 
> Se você precisar se conectar ao SAP HANA no Azure (Instâncias Grandes) em um _do data warehouse_ cenário, onde aplicativos e/ou usuários finais precisam se conectar no banco de dados SAP HANA (executando diretamente), outro componente de rede deve ser usado: um proxy reverso para dados da rota, de e para. Por exemplo, F5 BIG-IP com o Gerenciador de tráfego implantado no Azure como uma solução de roteamento de tráfego/firewall virtual.

### <a name="leveraging-in-multiple-regions"></a>Utilização em várias regiões

Você pode ter outros motivos para implantar SAP HANA no Azure (Instâncias Grandes) em várias regiões do Azure, além de DR. Talvez você queira acessar o HANA em Instâncias Grandes de cada uma das máquinas virtuais implantadas nas redes virtuais diferentes nas regiões. Como os endereços IP com NAT de diferentes servidores HANA em Instâncias Grandes não são propagados além de redes virtuais do Azure (que estão diretamente conectadas por meio de seu gateway para as instâncias), há uma pequena alteração no design de rede virtual introduzido acima: um gateway de rede virtual do Azure pode lidar com quatro diferentes circuitos do ExpressRoute sem MSEEs diferentes, e cada rede virtual conectada a um dos carimbos de Instância Grande pode ser conectada ao carimbo de Instância Grande em outra região do Azure.

![Redes virtuais do Azure conectadas ao carimbos de Instância Grande do Azure em diferentes regiões do Azure](./media/sap-hana-overview-architecture/image8-multiple-regions.png)

A figura acima mostra como as diferentes redes virtuais do Azure em ambas as regiões estão conectadas a dois circuitos do ExpressRoute diferentes que são usados para se conectar ao SAP HANA no Azure (Instâncias Grandes) em ambas as regiões do Azure. As conexões recém-introduzidas são as linhas vermelhas retangulares. Com essas conexões fora de redes virtuais do Azure, as VMs em execução em uma dessas redes virtuais podem acessar cada unidade do HANA em Instâncias Grandes diferente implantada em duas regiões (supondo que você use a mesma assinatura). Como você pode ver nos gráficos acima, presume-se que você tenha duas conexões do ExpressRoute do local em duas regiões do Azure; recomendada por motivos de Recuperação de Desastre.

> [!IMPORTANT] 
> Se forem usados vários circuitos do ExpressRoute, as configurações de prefixação do AS Path e de BGP de Preferência Local deverão ser usadas para garantir o roteamento apropriado do tráfego.



