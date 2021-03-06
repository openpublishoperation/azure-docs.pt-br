---
title: "Migrar as redes virtuais associadas ao ExpressRoute do clássico para o Resource Manager: Azure: PowerShell | Microsoft Docs"
description: "Esta página descreve como migrar redes virtuais associadas ao Resource Manager depois de migrar o circuito."
documentationcenter: na
services: expressroute
author: ganesr
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/11/2017
ms.author: ganesr;cherylmc
translationtype: Human Translation
ms.sourcegitcommit: 785d3a8920d48e11e80048665e9866f16c514cf7
ms.openlocfilehash: 5cebc3c5f2bcfb89f939b98391ffd072263c3e08
ms.lasthandoff: 04/12/2017


---
# <a name="migrate-expressroute-associated-virtual-networks-from-classic-to-resource-manager"></a>Migrar redes virtuais associadas ao ExpressRoute do clássico para o Resource Manager

Este artigo explica como migrar redes virtuais associadas ao Azure ExpressRoute do modelo de implantação clássico para o modelo de implantação do Azure Resource Manager depois de migrar o circuito do ExpressRoute. 


## <a name="before-you-begin"></a>Antes de começar
* Verifique se você tem a versão mais recente dos módulos do Azure PowerShell. Para saber mais, consulte [Como instalar e configurar o Azure PowerShell](/powershell/azureps-cmdlets-docs):
* Certifique-se de que você leu os [pré-requisitos](expressroute-prerequisites.md), os [requisitos de roteamento](expressroute-routing.md) e os [fluxos de trabalho](expressroute-workflows.md) antes de começar a configuração.
* Examine as informações fornecidas em [Como mover um circuito de ExpressRoute do clássico para o Resource Manager](expressroute-move.md). Certifique-se de entender completamente os limites e limitações.
* Verifique se o circuito está totalmente operacional no modelo de implantação clássico.
* Verifique se você tem um grupo de recursos que foi criado no modelo de implantação do Gerenciador de Recursos.
* Examine a documentação de migração de recurso a seguir:

    * [Migração de recursos de IaaS com suporte da plataforma do clássico para o Azure Resource Manager](../virtual-machines/virtual-machines-windows-migration-classic-resource-manager.md)
    * [Análise técnica aprofundada sobre a migração com suporte da plataforma do clássico para o Azure Resource Manager](../virtual-machines/virtual-machines-windows-migration-classic-resource-manager-deep-dive.md)
    * [Perguntas frequentes: migração de recursos de IaaS com suporte da plataforma do clássico para o Azure Resource Manager](../virtual-machines/virtual-machines-windows-migration-classic-resource-manager.md)
    * [Confira os erros mais comuns de migração e as mitigações](../virtual-machines/windows/migration-classic-resource-manager-errors.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

## <a name="supported-and-unsupported-scenarios"></a>Cenários com e sem suporte

* Um circuito do ExpressRoute pode ser migrado do ambiente clássico para o Resource Manager sem nenhum tempo de inatividade. É possível mover um circuito de ExpressRoute do ambiente clássico para o do Resource Manager sem nenhum tempo de inatividade. Siga as instruções em [Movendo circuitos do ExpressRoute do clássico para o modelo de implantação do Resource Manager usando o PowerShell](expressroute-howto-move-arm.md). Este é um pré-requisito para mover recursos conectados à rede virtual.
* Redes virtuais, gateways e implantações associadas dentro da rede virtual que estão anexadas a um circuito de ExpressRoute na mesma assinatura podem ser migradas para o ambiente do Resource Manager sem nenhum tempo de inatividade. Você pode seguir as etapas descritas posteriormente para migrar recursos como redes virtuais, gateways e máquinas virtuais implantadas na rede virtual. Certifique-se de que as redes virtuais estão configuradas corretamente antes de elas serem migradas. 
* Redes virtuais, gateways e implantações associadas dentro da rede virtual que não estão na mesma assinatura que o circuito de ExpressRoute requerem algum tempo de inatividade para que a migração seja concluída. A última seção do documento descreve as etapas a serem seguidas para migrar os recursos.

## <a name="move-an-expressroute-circuit-from-classic-to-resource-manager"></a>Mover um circuito de ExpressRoute de clássico para Resource Manager
Você deve mover um circuito de ExpressRoute do ambiente clássico para o do Resource Manager antes de tentar migrar os recursos que estão anexados ao circuito de ExpressRoute. Para realizar essa tarefa, consulte os artigos a seguir:

* Examine as informações fornecidas em [Como mover um circuito de ExpressRoute do clássico para o Resource Manager](expressroute-move.md).
* [Migrar um circuito de clássico para o Resource Manager usando o Azure PowerShell](expressroute-howto-move-arm.md).
* Use o Portal de Gerenciamento de Serviços do Azure. Você pode seguir o fluxo de trabalho para [criar um novo circuito de ExpressRoute](expressroute-howto-circuit-portal-resource-manager.md) e selecionar a opção de importação. 

Esta operação não envolve tempo de inatividade. Você pode continuar a transferir dados entre seu local e a Microsoft enquanto a migração está em andamento.

## <a name="prepare-your-virtual-network-for-migration"></a>Prepare a sua rede virtual para migração
Certifique-se de que a rede da sua rede virtual a ser migrada não tem artefatos desnecessários. Para baixar sua configuração de rede virtual e atualizá-la conforme necessário, execute o seguinte cmdlet do PowerShell:

```powershell
Add-AzureAccount
Select-AzureSubscription -SubscriptionName <VNET Subscription>
Get-AzureVNetConfig -ExportToFile C:\virtualnetworkconfig.xml
```
      
Você deve assegurar que todas as referências aos <ConnectionsToLocalNetwork> sejam removidas das redes virtuais a serem migradas. Uma configuração de rede de exemplo é mostrada no trecho a seguir:

```
    <VirtualNetworkSite name="MyVNet" Location="East US">
        <AddressSpace>
            <AddressPrefix>10.0.0.0/8</AddressPrefix>
        </AddressSpace>
        <Subnets>
            <Subnet name="Subnet-1">
                <AddressPrefix>10.0.0.0/11</AddressPrefix>
            </Subnet>
            <Subnet name="GatewaySubnet">
                <AddressPrefix>10.32.0.0/28</AddressPrefix>
            </Subnet>
        </Subnets>
        <Gateway>
            <ConnectionsToLocalNetwork>
            </ConnectionsToLocalNetwork>
        </Gateway>
    </VirtualNetworkSite>
```
 
Se <ConnectionsToLocalNetwork> não for vazio, exclua as referências abaixo dele e reenvie a sua configuração de rede. Podemos fazer isso executando o cmdlet do PowerShell a seguir:

```powershell
Set-AzureVNetConfig -ConfigurationPath c:\virtualnetworkconfig.xml
```

## <a name="migrate-virtual-networks-gateways-and-associated-deployments"></a>Migrar redes virtuais, gateways e implantações associadas

As etapas a serem seguidas para a migração dependem se os recursos estão na mesma assinatura, em assinaturas diferentes ou ambos.

### <a name="migrate-virtual-networks-gateways-and-associated-deployments-in-the-same-subscription-as-the-expressroute-circuit"></a>Migrar redes virtuais, gateways e implantações associadas na mesma assinatura que o circuito de ExpressRoute
Esta seção descreve as etapas a serem seguidas para migrar de uma rede virtual, gateway e implantações associadas na mesma assinatura que o circuito de ExpressRoute. Nenhum tempo de inatividade é associado a essa migração. Você pode continuar a usar todos os recursos durante o processo de migração. O plano de gerenciamento é bloqueado enquanto a migração está em andamento. 

1. Certifique-se de que o circuito de ExpressRoute tenha sido migrado do ambiente clássico para o do Resource Manager.
2. Certifique-se de que a rede virtual foi preparada adequadamente para a migração.
3. Registre sua assinatura para a migração de recursos. Para registrar sua assinatura para a migração de recursos, use o seguinte trecho do PowerShell:

  ```powershell 
  Select-AzureRmSubscription -SubscriptionName <Your Subscription Name>
  Register-AzureRmResourceProvider -ProviderNamespace Microsoft.ClassicInfrastructureMigrate
  Get-AzureRmResourceProvider -ProviderNamespace Microsoft.ClassicInfrastructureMigrate
  ```
4. Valide, prepare e migre. Para mover a rede virtual, use o seguinte trecho do PowerShell:

  ```powershell
  Move-AzureVirtualNetwork -Prepare $vnetName  
  Move-AzureVirtualNetwork -Commit $vnetName
  ```

  Você também pode anular a migração executando o seguinte cmdlet do PowerShell:

  ```powershell
  Move-AzureVirtualNetwork -Abort $vnetName
  ```

### <a name="migrate-virtual-networks-gateways-and-associated-deployments-in-a-different-subscription-from-that-of-the-expressroute-circuit"></a>Migrar redes virtuais, gateways e implantações associadas em uma assinatura diferente daquela do circuito de ExpressRoute

1. Certifique-se de que o circuito de ExpressRoute tenha sido migrado do ambiente clássico para o do Resource Manager.
2. Certifique-se de que a rede virtual foi preparada adequadamente para a migração.
3. Certifique-se de que o circuito de ExpressRoute possa operar tanto no ambiente clássico quanto no do Resource Manager. Para permitir que o circuito seja usado tanto no ambiente clássico quanto no do Resource Manager, use o seguinte script do PowerShell:

  ```powershell
  Login-AzureRmAccount
  Select-AzureRmSubscription -SubscriptionName <My subscription>
  $circuit = Get-AzureRmExpressRouteCircuit -Name <CircuitName> -ResourceGroupName <ResourceGroup Name> 
  $circuit.AllowClassicOperations = $true
  Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $circuit
  ```
4. Crie autorizações no ambiente do Resource Manager. Para saber como criar autorizações, consulte [como vincular redes virtuais a circuitos de ExpressRoute](expressroute-howto-linkvnet-arm.md). Para criar uma autorização, use o seguinte trecho do PowerShell:

  ```powershell
  circuit = Get-AzureRmExpressRouteCircuit -Name <CircuitName> -ResourceGroupName <ResourceGroup Name> 
  Add-AzureRmExpressRouteCircuitAuthorization -ExpressRouteCircuit $circuit -Name "AuthorizationForMigration"
  Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $circuit
  $circuit = Get-AzureRmExpressRouteCircuit -Name MigrateCircuit -ResourceGroupName MigrateRGWest

  $id = $circuit.id 
  $auth1 = Get-AzureRmExpressRouteCircuitAuthorization -ExpressRouteCircuit $circuit -Name "AuthorizationForMigration"

  $key=$auth1.AuthorizationKey 
 ```

    Anote a chave de ID e a chave de autorização. Esses elementos serão usados para conectar o circuito à rede virtual depois que a migração estiver concluída.
  
5. Exclua o link de circuito dedicado que está associado à rede virtual. Para remover o link de circuito no ambiente clássico, use o seguinte cmdlet:

  ```powershell
  $skey = Get-AzureDedicatedCircuit | select ServiceKey
  Remove-AzureDedicatedCircuitLink -ServiceKey $skey -VNetName $vnetName
  ```  

6. Registre sua assinatura para a migração de recursos. Para registrar sua assinatura para a migração de recursos, use o seguinte trecho do PowerShell:

  ```powershell
  Select-AzureRmSubscription -SubscriptionName <Your Subscription Name>
  Register-AzureRmResourceProvider -ProviderNamespace Microsoft.ClassicInfrastructureMigrate
  Get-AzureRmResourceProvider -ProviderNamespace Microsoft.ClassicInfrastructureMigrate
  ```
7. Valide, prepare e migre. Para mover a rede virtual, use o seguinte trecho do PowerShell:

  ```powershell
  Move-AzureVirtualNetwork -Prepare $vnetName  
  Move-AzureVirtualNetwork -Commit $vnetName
  ```

    Você também pode anular a migração executando o seguinte cmdlet do PowerShell:

  ```powershell
  Move-AzureVirtualNetwork -Abort $vnetName
  ```
8. Conecte a Rede Virtual de volta ao circuito de ExpressRoute. O seguinte trecho do PowerShell é executado no contexto da assinatura na qual a rede virtual é criada. Você não deve executar este trecho na assinatura em que o circuito é criado. Use a ID de circuito como PeerID e a chave de autorização anotada na etapa 4.

  ```powershell
  Select-AzureRMSubscription –SubscriptionName <customer subscription>  
  $gw = Get-AzureRmVirtualNetworkGateway -Name $vnetName-Default-Gateway -ResourceGroupName ($vnetName + "-Migrated")
  $vnet = Get-AzureRmVirtualNetwork -Name $vnetName -ResourceGroup  ($vnetName + "-Migrated")  

  New-AzureRmVirtualNetworkGatewayConnection -Name  ($vnetName + "-GwConn") -ResourceGroupName ($vnetName + "-Migrated")  -Location $vnet.Location -VirtualNetworkGateway1 $gw -PeerId $id -ConnectionType ExpressRoute -AuthorizationKey $key
  ```

## <a name="next-steps"></a>Próximas etapas
* [Migração de recursos de IaaS com suporte da plataforma do clássico para o Azure Resource Manager](../virtual-machines/virtual-machines-windows-migration-classic-resource-manager.md)
* [Análise técnica aprofundada sobre a migração com suporte da plataforma do clássico para o Azure Resource Manager](../virtual-machines/virtual-machines-windows-migration-classic-resource-manager-deep-dive.md)
* [Perguntas frequentes: migração de recursos de IaaS com suporte da plataforma do clássico para o Azure Resource Manager](../virtual-machines/virtual-machines-windows-migration-classic-resource-manager.md)
* [Confira os erros mais comuns de migração e as mitigações](../virtual-machines/windows/migration-classic-resource-manager-errors.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

