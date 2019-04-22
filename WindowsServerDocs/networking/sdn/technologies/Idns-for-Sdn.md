---
title: Service DNS interne pour SDN
description: Cette rubrique explique comment vous pouvez fournir des services DNS pour vos charges de travail client hébergé à l’aide de DNS interne (IDN), qui est intégré avec Sdn dans Windows Server 2016.
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
ms.openlocfilehash: 4d4ae5ee5f5600d86349ca26b7acbdb284b45bac
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59824080"
---
# <a name="internal-dns-service-idns-for-sdn"></a>Service DNS interne pour SDN

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Si vous travaillez pour un fournisseur de services Cloud \(CSP\) ou de l’entreprise qui prévoit de déployer Sdn \(SDN\) dans Windows Server 2016, vous pouvez fournir des services DNS pour vos charges de travail client hébergé à l’aide de DNS interne \(iDNS\), qui est intégré à SDN.

Machines virtuelles hébergées \(machines virtuelles\) et applications nécessitent DNS communiquer au sein de leurs propres réseaux et avec des ressources externes sur Internet. Avec les noms de domaines internationaux, vous pouvez fournir des clients avec les services de résolution de noms DNS pour leur espace de noms isolé, local et de ressources Internet.

Étant donné que le service de noms de domaines internationaux n’est pas accessible à partir de réseaux virtuels de locataire, autre que via le proxy de noms de domaines internationaux, le serveur n'est pas vulnérable à des activités malveillantes sur réseaux clients.

**Fonctionnalités clés**

Voici les principales fonctionnalités de domaine internationaux.

- Fournit des que services de résolution pour le locataire de noms DNS partagés charges de travail
- Service DNS faisant autorité pour la résolution de noms et l’enregistrement DNS dans l’espace de noms de client
- Service DNS récursives pour la résolution de noms Internet à partir de machines virtuelles du locataire.
- Si vous le souhaitez, vous pouvez configurer l’hébergement simultanées des noms de fabric et locataire
- Une solution rentable de DNS - locataires n’avez pas besoin de déployer leur propre infrastructure DNS
- Haute disponibilité avec l’intégration Active Directory, ce qui est nécessaire.

Outre ces fonctionnalités, si vous avez besoin de conserver votre AD intégré ouverts par les serveurs DNS sur Internet, vous pouvez déployer des serveurs de noms de domaines internationaux derrière un autre programme de résolution récursive dans le réseau de périmètre.

Étant donné que les noms de domaines internationaux est un serveur centralisé pour toutes les requêtes DNS, un CSP ou une entreprise peut également implémenter les pare-feux de client DNS, appliquer des filtres, détecter les activités malveillantes et d’audit des transactions dans un emplacement central

## <a name="idns-infrastructure"></a>Infrastructure des noms de domaines internationaux
L’infrastructure de noms de domaines internationaux comprend des serveurs de noms de domaines internationaux et des noms de domaines internationaux proxy.

### <a name="idns-servers"></a>noms de domaines internationaux serveurs
noms de domaines internationaux inclut un ensemble de serveurs DNS qui hébergent des données spécifiques du client, telles que les enregistrements de ressources DNS de machine virtuelle.

serveurs de noms de domaines internationaux sont les serveurs faisant autorités pour leurs zones DNS internes et également agir comme un programme de résolution de noms publics locataire lorsque des machines virtuelles tente de se connecter à des ressources externes.

Tous les noms d’hôte pour les machines virtuelles sur les réseaux virtuels sont stockés sous forme d’enregistrements de ressource DNS sous la même zone. Par exemple, si vous déployez des noms de domaines internationaux pour une zone nommée contoso.local, les enregistrements de ressource DNS pour les machines virtuelles sur ce réseau sont stockés dans la zone de contoso.local.

Machine virtuelle noms de domaine complet du locataire \(noms de domaine complets\) comprennent le nom d’ordinateur et de la chaîne de suffixe DNS pour le réseau virtuel, au format GUID. Par exemple, si vous avez un locataire de machine virtuelle nommée locataire1 qui se trouve sur le réseau virtuel contoso, local, nom de domaine complet de la machine virtuelle est un locataire 1. *vn-guid*. contoso.local, où *vn-guid* est la chaîne de suffixe DNS pour le réseau virtuel.

>[!NOTE]
>Si vous êtes un administrateur d’infrastructure, vous pouvez utiliser votre infrastructure DNS de l’entreprise ou de CSP comme serveurs de noms de domaines internationaux au lieu de déployer de nouveaux serveurs DNS spécifiquement pour les utilisent en tant que noms de domaines internationaux serveurs. Si vous déployez de nouveaux serveurs de domaine internationaux ou vous utilisez votre infrastructure existante, noms de domaines internationaux s’appuie sur Active Directory pour fournir une haute disponibilité. Vos serveurs de noms de domaines internationaux doivent donc être intégrés à Active Directory.

### <a name="idns-proxy"></a>noms de domaines internationaux Proxy
noms de domaines internationaux proxy est un service Windows qui s’exécute sur chaque hôte, puis qui transfère locataire le trafic DNS de réseau virtuel pour les serveur de noms IDN.

L’illustration suivante représente les chemins d’accès du trafic DNS à partir de réseaux virtuels du locataire via le proxy de noms de domaines internationaux pour les noms de domaines internationaux Server et Internet.

![Infrastructure des noms de domaines internationaux](../../media/Internal-Dns/Internal-Dns.jpg)

## <a name="how-to-deploy-idns"></a>Comment déployer noms de domaines internationaux
Lorsque vous déployez SDN dans Windows Server 2016 à l’aide de scripts, les noms de domaines internationaux est automatiquement inclus dans votre déploiement.

Pour plus d’informations, voir les rubriques suivantes :

- [Déployer une infrastructure de réseau à définition logicielle à l’aide de scripts](https://docs.microsoft.com/windows-server/networking/sdn/deploy/deploy-a-software-defined-network-infrastructure-using-scripts)


## <a name="understanding-idns-deployment-steps"></a>Présentation des étapes de déploiement des noms de domaines internationaux
Cette section vous permettent de mieux comprendre comment les noms de domaines internationaux est installé et configuré lorsque vous déployez SDN à l’aide de scripts.

Voici un résumé des étapes nécessaires pour déployer les noms de domaines internationaux.

>[!NOTE]
>Si vous avez déployé SDN à l’aide de scripts, il est inutile d’effectuer ces étapes. Les étapes sont fournies pour plus d’informations et le dépannage uniquement.

### <a name="step-1-deploy-dns"></a>Étape 1 : Déployer le système DNS
Vous pouvez déployer un serveur DNS à l’aide de l’exemple suivant de commande Windows PowerShell.
    
    Install-WindowsFeature DNS -IncludeManagementTools
    
### <a name="step-2-configure-idns-information-in-network-controller"></a>Étape 2 : Configurer les informations de noms de domaines internationaux dans le contrôleur de réseau
Ce segment de script est un appel REST qui est effectué par l’administrateur au contrôleur de réseau, qui informe sur la configuration de zone de noms de domaines internationaux - telles que l’adresse IP de l’iDNSServer et de la zone qui est utilisée pour héberger les noms de domaine internationaux. 

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
>Il s’agit d’un extrait à partir de la section **Configuration ConfigureIDns** dans SDNExpress.ps1. Pour plus d’informations, consultez [déployer une infrastructure de réseau à définition logicielle à l’aide de scripts](https://technet.microsoft.com/windows-server-docs/networking/sdn/deploy/deploy-a-software-defined-network-infrastructure-using-scripts).

### <a name="step-3-configure-the-idns-proxy-service"></a>Étape 3 : Configurer le Service de Proxy des noms de domaines internationaux
Le Service de Proxy des noms de domaines internationaux s’exécute sur chacun des hôtes Hyper-V, en fournissant le pont entre les réseaux virtuels de locataires et le réseau physique dans lequel se trouvent les serveurs de noms de domaines internationaux. Les clés de Registre suivante doivent être créés sur chaque hôte Hyper-V.


**Port DNS :** Port fixe 53

- Registry Key = HKLM\SYSTEM\CurrentControlSet\Services\NcHostAgent\Parameters\Plugins\Vnet\InfraServices\DnsProxyService"
- Nom de valeur = « Port »
- ValueData = 53
- ValueType = "Dword"
       

**Port du Proxy DNS :** Port fixe 53

- Registry Key = HKLM\SYSTEM\CurrentControlSet\Services\NcHostAgent\Parameters\Plugins\Vnet\InfraServices\DnsProxyService"
- ValueName = "ProxyPort"
- ValueData = 53
- ValueType = "Dword"
        
**ADRESSE IP DU DNS :** Adresse IP fixe configuré sur l’interface réseau, au cas où le locataire choisit d’utiliser le service de noms de domaines internationaux

- Registry Key = HKLM\SYSTEM\CurrentControlSet\Services\NcHostAgent\Parameters\Plugins\Vnet\InfraServices\DnsProxyService"
- Nom de valeur = « IP »
- ValueData = "169.254.169.254"
- ValueType = « String »

        
**Adresse MAC :** Adresse de contrôle d’accès de support du serveur DNS

- Registry Key = HKLM\SYSTEM\CurrentControlSet\Services\NcHostAgent\Parameters\Plugins\Vnet\InfraServices\DnsProxyService
- Nom de valeur = « MAC »
- ValueData = « aa-bb-cc-aa-bb-cc »
- ValueType = « String »

**Adresse du serveur de noms de domaines internationaux :** Liste des noms de domaines internationaux serveurs séparés par des virgules.

- Clé de Registre : HKLM\SYSTEM\CurrentControlSet\Services\DNSProxy\Parameters
- Nom de valeur = « Redirecteurs »
- ValueData = « 10.0.0.9 »
- ValueType = « String »



>[!NOTE]
>Il s’agit d’un extrait à partir de la section **Configuration ConfigureIDnsProxy** dans SDNExpress.ps1. Pour plus d’informations, consultez [déployer une infrastructure de réseau à définition logicielle à l’aide de scripts](https://technet.microsoft.com/windows-server-docs/networking/sdn/deploy/deploy-a-software-defined-network-infrastructure-using-scripts).

### <a name="step-4-restart-the-network-controller-host-agent-service"></a>Étape 4 : Redémarrez le Service de l’Agent hôte réseau contrôleur
Vous pouvez utiliser la commande Windows PowerShell suivante pour redémarrer le Service d’Agent hôte contrôleur de réseau.
    
    Restart-Service nchostagent -Force
    
Pour plus d’informations, consultez [Restart-Service](https://technet.microsoft.com/library/hh849823.aspx).

### <a name="enable-firewall-rules-for-the-dns-proxy-service"></a>Activer les règles de pare-feu pour le service de proxy DNS
Vous pouvez utiliser la commande Windows PowerShell suivante pour créer une règle de pare-feu qui autorise les exceptions du proxy communiquer avec la machine virtuelle et le serveur de noms de domaines internationaux.
    
    Enable-NetFirewallRule -DisplayGroup 'DNS Proxy Firewall'

Pour plus d’informations, consultez [Enable-NetFirewallRule](https://technet.microsoft.com/library/jj554869.aspx).
    
### <a name="validate-the-idns-service"></a>Valider le Service des noms de domaines internationaux
Pour valider les noms de domaines internationaux Service, vous devez déployer une charge de travail de locataire exemple.

Pour plus d’informations, consultez [créer une machine virtuelle et la connexion à un réseau virtuel locataire ou un réseau local virtuel](https://technet.microsoft.com/windows-server-docs/networking/sdn/manage/create-a-tenant-vm).

Si vous souhaitez que la machine virtuelle pour utiliser le service de noms de domaines internationaux locataire, vous devez laisser la configuration de serveur DNS de machine virtuelle réseau interfaces vide et autoriser les interfaces à utiliser le protocole DHCP. 

Une fois que la machine virtuelle avec une interface réseau de ce type est initiée, il reçoit automatiquement une configuration qui permet à la machine virtuelle à utiliser des noms de domaines internationaux, et la machine virtuelle commence immédiatement à la résolution des noms en utilisant le service de noms de domaines internationaux.

Si vous configurez la machine virtuelle à utiliser le service de noms de domaines internationaux en laissant la case vide les informations de serveur DNS et serveur DNS auxiliaire interface réseau locataire, contrôleur de réseau fournit la machine virtuelle avec une adresse IP et effectue une inscription de nom DNS pour le compte de la machine virtuelle avec le serveur des noms de domaines internationaux . 

Contrôleur de réseau informe également le proxy de noms de domaines internationaux sur la machine virtuelle et les informations requises pour effectuer la résolution de nom pour la machine virtuelle. 

Lorsque la machine virtuelle lance une requête DNS, le proxy agit comme un redirecteur de la requête à partir du réseau virtuel pour le service de noms de domaines internationaux. 

Le proxy DNS garantit également que les requêtes de la machine virtuelle locataire sont isolés. Si le serveur de noms de domaines internationaux fait autorité pour la requête, le serveur de noms de domaines internationaux répond avec une réponse faisant autorité. Si le serveur de noms de domaines internationaux ne fait pas autorité pour la requête, il effectue une récurrence DNS pour résoudre des noms Internet.

>[!NOTE]
>Ces informations sont incluses dans la section **Configuration AttachToVirtualNetwork** dans SDNExpressTenant.ps1. Pour plus d’informations, consultez [déployer une infrastructure de réseau à définition logicielle à l’aide de scripts](https://technet.microsoft.com/windows-server-docs/networking/sdn/deploy/deploy-a-software-defined-network-infrastructure-using-scripts).

