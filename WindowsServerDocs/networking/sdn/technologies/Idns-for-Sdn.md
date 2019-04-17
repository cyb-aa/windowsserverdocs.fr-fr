---
title: Service DNS interne (noms de domaines internationaux) pour SDN
description: Cette rubrique explique comment vous pouvez fournir des services DNS pour vos charges de travail clientes hébergé à l’aide de DNS interne (noms de domaines internationaux), qui est intégré avec logiciel défini de mise en réseau dans Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-sdn
ms.tgt_pltfrm: na
ms.topic: get-started-article
ms.assetid: ad848a5b-0811-4c67-afe5-6147489c0384
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 4bdb495b585cf60315dee60f9365e282877fdc1d
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/28/2018
---
# <a name="internal-dns-service-idns-for-sdn"></a>\(IDNS\) du Service DNS internes pour SDN

>S’applique à: Windows Server (canal annuel un point-virgule), Windows Server2016

Si vous travaillez pour un fournisseur de services Cloud \(CSP\) ou d’une entreprise qui est planification du déploiement de logiciel défini de mise en réseau de \(SDN\) dans Windows Server 2016, vous pouvez fournir des services DNS pour vos charges de travail clientes hébergé à l’aide de DNS interne \(iDNS\), qui est intégré avec SDN.

Ordinateurs virtuels hébergés \(VMs\) et applications requièrent DNS de communiquer au sein de leurs propres réseaux et avec les ressources externes sur Internet. Avec les noms de domaines internationaux, vous pouvez fournir des clients avec les services de résolution de noms DNS pour leur espace de noms isolé, local et pour les ressources Internet.

Étant donné que le service de noms de domaines internationaux n’est pas accessible à partir de clients des réseaux virtuels, autre que via le proxy de noms de domaines internationaux, le serveur n’est pas vulnérable aux activités malveillantes sur les réseaux des clients.

**Principales fonctionnalités**

Voici les principales fonctionnalités de domaine internationaux.

- Fournit des que services de résolution pour les clients de charges de travail nom DNS partagés
- Service DNS faisant autorité pour la résolution de noms et l’enregistrement DNS au sein de l’espace de noms client
- Service DNS récursives pour la résolution de noms Internet à partir d’ordinateurs virtuels clients.
- Si vous le souhaitez, vous pouvez configurer simultanées hébergeant des noms de structure et le client
- Une solution rentable de DNS - locataires n’avez pas besoin de déployer leur propres infrastructure DNS
- Haute disponibilité avec l’intégration Active Directory, qui est requise.

Outre ces fonctionnalités, si vous avez peur de conserver votre publicité intégrée ouvrir des serveurs DNS à Internet, vous pouvez déployer des serveurs de noms de domaines internationaux derrière un autre résolution récursive dans le réseau de périmètre.

Étant donné que les noms de domaines internationaux est un serveur centralisé pour toutes les requêtes DNS, un fournisseur de services cryptographiques ou une entreprise peut également implémenter les pare-feux de client DNS, appliquer des filtres, détecter des activités malveillantes et d’audit des transactions à un emplacement central

## <a name="idns-infrastructure"></a>Infrastructure des noms de domaines internationaux
L’infrastructure de noms de domaines internationaux comprend des serveurs de noms de domaines internationaux et les noms de domaines internationaux proxy.

### <a name="idns-servers"></a>Serveurs des noms de domaines internationaux
noms de domaines internationaux comprend un ensemble de serveurs DNS qui hébergent des données client spécifiques, tels que les enregistrements de ressource DNS de machine virtuelle.

serveurs de noms de domaines internationaux sont les serveurs faisant autorités pour les zones DNS internes et également agir en tant qu’un programme de résolution de noms publics pour les clients lorsque tentative d’ordinateurs virtuels pour se connecter à des ressources externes.

Tous les noms d’hôte pour les ordinateurs virtuels sur des réseaux virtuels sont stockés sous la forme d’enregistrements de ressource DNS sous la même zone. Par exemple, si vous déployez des noms de domaines internationaux pour une zone nommée contoso.local, les enregistrements de ressource DNS pour les ordinateurs virtuels sur ce réseau sont stockés dans la zone contoso.local.

Client VM noms de domaine complets \(FQDNs\) comprennent le nom d’ordinateur et la chaîne de suffixe DNS pour le réseau virtuel, au format GUID. Par exemple, si vous disposez d’un ordinateur virtuel nommé TENANT1 qui se trouve sur le réseau virtuel contoso, local, client nom de domaine complet de l’ordinateur virtuel est TENANT1. *vn-guid*. contoso.local, où *vn-guid* est la chaîne de suffixe DNS pour le réseau virtuel.

>[!NOTE]
>Si vous êtes un administrateur de structure, vous pouvez utiliser votre infrastructure de fournisseur de services cryptographiques ou DNS d’entreprise comme serveurs de noms de domaines internationaux au lieu de déployer de nouveaux serveurs DNS spécifiquement pour les utilisent en tant que noms de domaines internationaux des serveurs. Déployer de nouveaux serveurs de noms de domaines internationaux si ou si vous utilisez votre infrastructure existante, les noms de domaines internationaux s’appuie sur Active Directory pour fournir une haute disponibilité. Vos serveurs de noms de domaines internationaux doivent donc être intégrés à Active Directory.

### <a name="idns-proxy"></a>noms de domaines internationaux Proxy
noms de domaines internationaux proxy est un service Windows qui s’exécute sur chaque ordinateur hôte, et qui transfère locataire le trafic DNS du réseau virtuel pour les noms de domaines internationaux serveur.

L’illustration suivante décrit les chemins de trafic DNS du client des réseaux virtuels via le proxy de noms de domaines internationaux pour les noms de domaines internationaux serveur et d’Internet.

![Infrastructure des noms de domaines internationaux](../../media/Internal-Dns/Internal-Dns.jpg)

## <a name="how-to-deploy-idns"></a>Comment déployer noms de domaines internationaux
Lorsque vous déployez SDN dans Windows Server 2016 à l’aide de scripts, les noms de domaines internationaux est automatiquement inclus dans votre déploiement.

Pour plus d’informations, consultez les rubriques suivantes.

- [Déployer une infrastructure réseau à définition logicielle à l’aide de scripts](https://technet.microsoft.com/windows-server-docs/networking/sdn/deploy/deploy-a-software-defined-network-infrastructure-using-scripts)


## <a name="understanding-idns-deployment-steps"></a>Présentation des étapes de déploiement des noms de domaines internationaux
Vous pouvez utiliser cette section pour mieux comprendre comment les noms de domaines internationaux est installé et configuré lorsque vous déployez SDN à l’aide de scripts.

Voici un résumé des étapes nécessaires pour déployer des noms de domaines internationaux.

>[!NOTE]
>Si vous avez déployé SDN à l’aide de scripts, il est inutile d’effectuer une de ces étapes. Les étapes sont fournies pour des informations et à des fins de dépannage uniquement.

### <a name="step-1-deploy-dns"></a>Étape 1: Déployer le système DNS
Vous pouvez déployer un serveur DNS à l’aide de l’exemple suivant de commande Windows PowerShell.
    
    Install-WindowsFeature DNS -IncludeManagementTools
    
### <a name="step-2-configure-idns-information-in-network-controller"></a>Étape 2: Configurer les informations de noms de domaines internationaux dans le contrôleur de réseau
Ce segment de script est un appel reste effectuée par l’administrateur pour le contrôleur de réseau, il informer de la configuration de zone de noms de domaines internationaux - telles que l’adresse IP de l’iDNSServer et la zone qui est utilisée pour héberger les noms des noms de domaines internationaux. 

```
    Url: https://<url>/networking/v1/iDnsServer/configuration
Method: PUT
{
      "properties": {
        "connections": [
          {
            "managementAddresses": [
              "10.0.0.9"
            ],
            "credential": {
              "resourceRef": "/credentials/iDnsServer-Credentials"
            },
            "credentialType": "usernamePassword"
          }
        ],
        "zone": "contoso.local"
      }
    }
```

>[!NOTE]
>Il s’agit d’un extrait de la section **Configuration ConfigureIDns** dans SDNExpress.ps1. Pour plus d’informations, voir [déployer une infrastructure réseau à définition logicielle à l’aide de scripts](https://technet.microsoft.com/windows-server-docs/networking/sdn/deploy/deploy-a-software-defined-network-infrastructure-using-scripts).

### <a name="step-3-configure-the-idns-proxy-service"></a>Étape 3: Configurer le Service de Proxy des noms de domaines internationaux
Le le Service de Proxy des noms de domaines internationaux s’exécute sur chacun des ordinateurs hôtes Hyper-V, en fournissant le pont entre les réseaux virtuels de clients et le réseau physique dans lequel se trouvent les serveurs de noms de domaines internationaux. Les clés de Registre suivante doivent être créés sur chaque ordinateur hôte Hyper-V.


**Port DNS:** port 53 fixe

- Clé de Registre = HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\NcHostAgent\Parameters\Plugins\Vnet\InfraServices\DnsProxyService»
- Nom de valeur = «Port»
- ValueData = 53
- Type de valeur = «Dword»
       

**Port du Proxy DNS:** port 53 fixe

- Clé de Registre = HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\NcHostAgent\Parameters\Plugins\Vnet\InfraServices\DnsProxyService»
- Nom de valeur = «ProxyPort»
- ValueData = 53
- Type de valeur = «Dword»
        
**DNS IP:** il s’agit de l’adresse IP statique configurée sur l’interface réseau, au cas où le client choisit d’utiliser le service de noms de domaines internationaux.

- Clé de Registre = HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\NcHostAgent\Parameters\Plugins\Vnet\InfraServices\DnsProxyService»
- Nom de valeur = «IP»
- ValueData = «169.254.169.254»
- Type de valeur = «Chaîne»

        
**Adresse MAC:** adresse Media Access Control du serveur DNS

- Clé de Registre = HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\NcHostAgent\Parameters\Plugins\Vnet\InfraServices\DnsProxyService
- Nom de valeur = «MAC»
- ValueData = «aa-bb-cc-aa-bb-cc»
- Type de valeur = «Chaîne»

**Adresse du serveur de noms de domaines internationaux:** la liste des noms de domaines internationaux serveurs séparés par des virgules.

- Clé de Registre: HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\DNSProxy\Parameters
- Nom de valeur = «Redirecteurs»
- ValueData = «10.0.0.9»
- Type de valeur = «Chaîne»



>[!NOTE]
>Il s’agit d’un extrait de la section **Configuration ConfigureIDnsProxy** dans SDNExpress.ps1. Pour plus d’informations, voir [déployer une infrastructure réseau à définition logicielle à l’aide de scripts](https://technet.microsoft.com/windows-server-docs/networking/sdn/deploy/deploy-a-software-defined-network-infrastructure-using-scripts).

### <a name="step-4-restart-the-network-controller-host-agent-service"></a>Étape 4: Redémarrer le Service d’Agent hôte contrôleur réseau
Vous pouvez utiliser la commande Windows PowerShell suivante pour redémarrer le Service d’Agent hôte contrôleur de réseau.
    
    Restart-Service nchostagent -Force
    
Pour plus d’informations, voir [Restart-Service](https://technet.microsoft.com/library/hh849823.aspx).

### <a name="enable-firewall-rules-for-the-dns-proxy-service"></a>Activer les règles de pare-feu pour le service de proxy DNS
Vous pouvez utiliser la commande Windows PowerShell suivante pour créer une règle de pare-feu qui autorise les exceptions du proxy communiquer avec l’ordinateur virtuel et le serveur de noms de domaines internationaux.
    
    Enable-NetFirewallRule -DisplayGroup 'DNS Proxy Firewall'

Pour plus d’informations, voir [Enable-NetFirewallRule](https://technet.microsoft.com/library/jj554869.aspx).
    
### <a name="validate-the-idns-service"></a>Valider les noms de domaines internationaux Service
Pour valider les noms de domaines internationaux Service, vous devez déployer une charge de travail client exemple.

Pour plus d’informations, voir [créer un ordinateur virtuel et se connecter à un réseau virtuel locataire ou un réseau local virtuel](https://technet.microsoft.com/windows-server-docs/networking/sdn/manage/create-a-tenant-vm).

Si vous souhaitez que l’ordinateur virtuel à utiliser le service de noms de domaines internationaux de locataire, vous devez laisser la configuration de serveur DNS d’interfaces réseau VM vide et autoriser les interfaces utiliser le protocole DHCP. 

Une fois que l’ordinateur virtuel avec une interface réseau est lancée, il reçoit automatiquement une configuration qui permet d’utiliser des noms de domaines internationaux de l’ordinateur virtuel et l’ordinateur virtuel commence immédiatement à la résolution des noms en utilisant le service de noms de domaines internationaux.

Si vous configurez le client de machine virtuelle à utiliser le service de noms de domaines internationaux en laissant les informations de serveur DNS et serveur DNS auxiliaire interface réseau vide, le contrôleur de réseau fournit l’ordinateur virtuel avec une adresse IP et effectue une inscription de nom DNS pour le compte de l’ordinateur virtuel avec les noms de domaines internationaux serveur. 

Contrôleur de réseau indique également le noms de domaines internationaux proxy sur l’ordinateur virtuel et les informations requises pour effectuer la résolution de noms pour l’ordinateur virtuel. 

Lorsque l’ordinateur virtuel lance une requête DNS, le serveur proxy agit comme un redirecteur de la requête à partir du réseau virtuel pour le service de noms de domaines internationaux. 

Le proxy DNS garantit également que les requêtes de machine virtuelle locataire isolés. Si le serveur de noms de domaines internationaux est faisant autorité pour la requête, le serveur de noms de domaines internationaux répond avec une réponse faisant autorité. Si le serveur de noms de domaines internationaux ne fait pas autorité pour la requête, il effectue une récursivité DNS pour résoudre des noms Internet.

>[!NOTE]
>Ces informations sont incluses dans la section **Configuration AttachToVirtualNetwork** dans SDNExpressTenant.ps1. Pour plus d’informations, voir [déployer une infrastructure réseau à définition logicielle à l’aide de scripts](https://technet.microsoft.com/windows-server-docs/networking/sdn/deploy/deploy-a-software-defined-network-infrastructure-using-scripts).

