## <a name="understanding-azure-resource-templates-and-resource-groups"></a>Noções básicas sobre grupos de recursos e modelos de recursos do Azure
A maioria dos aplicativos implantados e executados no Microsoft Azure é criada com base em uma combinação de diferentes tipos de recurso de nuvem (por exemplo, uma ou mais VMs e contas de Armazenamento, um banco de dados SQL, uma rede virtual, uma CDN etc.).  [Com os modelos do Azure Resource Manager](../articles/resource-group-authoring-templates.md) , é possível implantar e gerenciar esses recursos diferentes em conjunto usando uma descrição JSON dos recursos e parâmetros associados de configuração e implantação.

Depois de definir um modelo de recurso baseado em JSON, você poderá executá-lo e fazer com que os recursos definidos nele sejam implantados no Azure usando um comando do PowerShell.  Você pode executar esse comando do PowerShell de forma autônoma no shell de comando do PowerShell ou integrá-lo a um script do PowerShell que contenha lógica de automação adicional.

Os recursos que você criar usando modelos do Gerenciador de Recursos do Azure serão implantados em um grupo de recursos novo ou existente do Azure.  Um grupo de recursos do Azure permite que você gerencie vários recursos implantados juntos, como um grupo lógico. Normalmente, um grupo conterá recursos relacionados a um aplicativo específico.  Os grupos de recursos do Azure fornecem uma maneira de gerenciar o ciclo de vida total do grupo/aplicativo, bem como fornecem APIs que permitem iniciar/parar/excluir todos os recursos dentro do grupo de uma só vez, aplicar regras RBAC (Controle de Acesso Baseado em Função) para bloquear as permissões de segurança neles, auditar operações, bem como marcar recursos com metadados adicionais para melhor rastreamento. Para saber mais sobre Grupos de Recursos do Azure, consulte [Visão geral do Gerenciador de Recursos do Azure](https://azure.microsoft.com/documentation/articles/resource-group-overview/). 

Os exemplos de automação a seguir demonstram como usar os Modelos do Gerenciador de Recursos do Azure e implantar Grupos de Recursos usando o PowerShell ou a CLI.



<!--HONumber=Jan17_HO3-->


