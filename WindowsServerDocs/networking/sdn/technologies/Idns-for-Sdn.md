---
title: Service DNS interne pour SDN
description: Cette rubrique explique comment fournir des services DNS à vos charges de travail de locataire hébergées à l’aide de DNS interne (iDNS), qui est intégré à la mise en réseau définie par logiciel dans Windows Server 2016.
manager: grcusanz
ms.prod: windows-server
ms.technology: networking-sdn
ms.topic: get-started-article
ms.assetid: ad848a5b-0811-4c67-afe5-6147489c0384
ms.author: anpaul
author: AnirbanPaul
ms.openlocfilehash: 717575693d40ff7d17c160772886f7ae3f648a9c
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80854322"
---
# <a name="internal-dns-service-idns-for-sdn"></a>Service DNS interne pour SDN

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Si vous travaillez pour un fournisseur de services Cloud \(CSP\) ou entreprise qui envisage de déployer des\) de mise en réseau à définition logicielle \(SDN dans Windows Server 2016, vous pouvez fournir des services DNS à vos charges de travail de locataire hébergées à l’aide de DNS interne \(iDNS\), qui est intégré à SDN.

Les machines virtuelles hébergées \(les machines virtuelles\) et les applications requièrent le DNS pour communiquer au sein de leurs propres réseaux et avec des ressources externes sur Internet. Avec iDNS, vous pouvez fournir aux locataires des services de résolution de noms DNS pour leur espace de noms local isolé et pour les ressources Internet.

Étant donné que le service iDNS n’est pas accessible à partir de réseaux virtuels locataires, à l’exception du proxy iDNS, le serveur n’est pas vulnérable aux activités malveillantes sur les réseaux locataires.

**Fonctionnalités clés**

Voici les principales fonctionnalités de iDNS.

- Fournit des services de résolution de noms DNS partagés pour les charges de travail des locataires
- Service DNS faisant autorité pour la résolution de noms et l’inscription DNS dans l’espace de noms du locataire
- Service DNS récursif pour la résolution de noms Internet à partir de machines virtuelles clientes.
- Si vous le souhaitez, vous pouvez configurer l’hébergement simultané des noms de l’infrastructure et des locataires
- Solution DNS rentable : les locataires n’ont pas besoin de déployer leur propre infrastructure DNS.
- Haute disponibilité avec intégration de Active Directory, ce qui est nécessaire.

En plus de ces fonctionnalités, si vous vous inquiétez de garder vos serveurs DNS intégrés à Active Directory ouverts sur Internet, vous pouvez déployer des serveurs iDNS derrière un autre programme de résolution récursif dans le réseau de périmètre.

Étant donné que iDNS est un serveur centralisé pour toutes les requêtes DNS, un CSP ou une entreprise peut également implémenter des pare-feu DNS de locataire, appliquer des filtres, détecter des activités malveillantes et auditer des transactions dans un emplacement central.

## <a name="idns-infrastructure"></a>Infrastructure iDNS
L’infrastructure iDNS comprend les serveurs iDNS et le proxy iDNS.

### <a name="idns-servers"></a>Serveurs iDNS
iDNS comprend un ensemble de serveurs DNS qui hébergent des données propres au locataire, telles que des enregistrements de ressources DNS de machine virtuelle.

les serveurs iDNS sont les serveurs faisant autorité pour leurs zones DNS internes et servent également de programme de résolution pour les noms publics lorsque des machines virtuelles clientes tentent de se connecter à des ressources externes.

Tous les noms d’hôte des machines virtuelles sur les réseaux virtuels sont stockés sous la forme d’enregistrements de ressources DNS sous la même zone. Par exemple, si vous déployez iDNS pour une zone nommée contoso. local, les enregistrements de ressources DNS pour les machines virtuelles sur ce réseau sont stockés dans la zone contoso. local.

Les noms de domaine complets des machines virtuelles du locataire \(noms de domaine complets\) se composent du nom d’ordinateur et de la chaîne de suffixe DNS pour le réseau virtuel, au format GUID. Par exemple, si vous avez une machine virtuelle cliente nommée LOCATAIRE1 qui se trouve sur le réseau virtuel Contoso, local, le nom de domaine complet de la machine virtuelle est LOCATAIRE1. *VN-GUID*. contoso. local, où *VN-GUID* est la chaîne de suffixe DNS pour le réseau virtuel.

>[!NOTE]
>Si vous êtes administrateur de structure, vous pouvez utiliser votre fournisseur de services de chiffrement ou votre infrastructure DNS d’entreprise en tant que serveurs iDNS au lieu de déployer de nouveaux serveurs DNS spécifiquement pour une utilisation en tant que serveurs iDNS. Que vous déployiez de nouveaux serveurs pour iDNS ou que vous utilisiez votre infrastructure existante, iDNS s’appuie sur Active Directory pour fournir une haute disponibilité. Vos serveurs iDNS doivent donc être intégrés à Active Directory.

### <a name="idns-proxy"></a>Proxy iDNS
le proxy iDNS est un service Windows qui s’exécute sur chaque ordinateur hôte et qui transmet le trafic DNS du réseau virtuel du locataire au serveur iDNS.

L’illustration suivante représente les chemins de trafic DNS des réseaux virtuels locataires via le proxy iDNS vers le serveur iDNS et Internet.

![Infrastructure iDNS](../../media/Internal-Dns/Internal-Dns.jpg)

## <a name="how-to-deploy-idns"></a>Comment déployer iDNS
Lorsque vous déployez SDN dans Windows Server 2016 à l’aide de scripts, iDNS est inclus automatiquement dans votre déploiement.

Pour plus d’informations, voir les rubriques suivantes :

- [Déployer une infrastructure réseau définie par logiciel à l’aide de scripts](https://docs.microsoft.com/windows-server/networking/sdn/deploy/deploy-a-software-defined-network-infrastructure-using-scripts)


## <a name="understanding-idns-deployment-steps"></a>Comprendre les étapes de déploiement de iDNS
Vous pouvez utiliser cette section pour comprendre comment iDNS est installé et configuré lorsque vous déployez des SDN à l’aide de scripts.

Voici un résumé des étapes nécessaires pour déployer iDNS.

>[!NOTE]
>Si vous avez déployé SDN à l’aide de scripts, vous n’avez pas besoin d’effectuer ces étapes. Les étapes sont fournies à titre d’information et de résolution des problèmes uniquement.

### <a name="step-1-deploy-dns"></a>Étape 1 : déployer DNS
Vous pouvez déployer un serveur DNS à l’aide de l’exemple de commande Windows PowerShell suivant.
    
    Install-WindowsFeature DNS -IncludeManagementTools
    
### <a name="step-2-configure-idns-information-in-network-controller"></a>Étape 2 : configurer les informations iDNS dans le contrôleur de réseau
Ce segment de script est un appel REST effectué par l’administrateur au contrôleur de réseau, en l’informant de la configuration de la zone iDNS, par exemple l’adresse IP du iDNSServer et la zone utilisée pour héberger les noms de iDNS. 

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
>Il s’agit d’un extrait de la section **configuration ConfigureIDns** dans SDNExpress. ps1. Pour plus d’informations, consultez [déployer une infrastructure réseau définie par logiciel à l’aide de scripts](https://technet.microsoft.com/windows-server-docs/networking/sdn/deploy/deploy-a-software-defined-network-infrastructure-using-scripts).

### <a name="step-3-configure-the-idns-proxy-service"></a>Étape 3 : configurer le service de proxy iDNS
Le service proxy iDNS s’exécute sur chacun des hôtes Hyper-V, en fournissant la passerelle entre les réseaux virtuels des locataires et le réseau physique où se trouvent les serveurs iDNS. Les clés de Registre suivantes doivent être créées sur chaque hôte Hyper-V.


**Port DNS :** Port fixe 53

- Clé de Registre = HKLM\SYSTEM\CurrentControlSet\Services\NcHostAgent\Parameters\Plugins\Vnet\InfraServices\DnsProxyService "
- ValueName = "port"
- ValueData = 53
- ValueType = "DWORD"
       

**Port du proxy DNS :** Port fixe 53

- Clé de Registre = HKLM\SYSTEM\CurrentControlSet\Services\NcHostAgent\Parameters\Plugins\Vnet\InfraServices\DnsProxyService "
- ValueName = "ProxyPort"
- ValueData = 53
- ValueType = "DWORD"
        
**adresse IP DNS :** Adresse IP fixe configurée sur l’interface réseau, au cas où le locataire choisit d’utiliser le service iDNS

- Clé de Registre = HKLM\SYSTEM\CurrentControlSet\Services\NcHostAgent\Parameters\Plugins\Vnet\InfraServices\DnsProxyService "
- ValueName = "IP"
- ValueData = "169.254.169.254"
- ValueType = "String"

        
**Adresse Mac :** Adresse Access Control du média du serveur DNS

- Clé de Registre = HKLM\SYSTEM\CurrentControlSet\Services\NcHostAgent\Parameters\Plugins\Vnet\InfraServices\DnsProxyService
- ValueName = "MAC"
- ValueData = "AA-BB-CC-AA-BB-CC"
- ValueType = "String"

**Adresse du serveur IDNs :** Liste séparée par des virgules de serveurs iDNS.

- Clé de Registre : HKLM\SYSTEM\CurrentControlSet\Services\DNSProxy\Parameters
- ValueName = "redirecteurs"
- ValueData = "10.0.0.9"
- ValueType = "String"



>[!NOTE]
>Il s’agit d’un extrait de la section **configuration ConfigureIDnsProxy** dans SDNExpress. ps1. Pour plus d’informations, consultez [déployer une infrastructure réseau définie par logiciel à l’aide de scripts](https://technet.microsoft.com/windows-server-docs/networking/sdn/deploy/deploy-a-software-defined-network-infrastructure-using-scripts).

### <a name="step-4-restart-the-network-controller-host-agent-service"></a>Étape 4 : redémarrer le service agent hôte du contrôleur de réseau
Vous pouvez utiliser la commande Windows PowerShell suivante pour redémarrer le service agent hôte du contrôleur de réseau.
    
    Restart-Service nchostagent -Force
    
Pour plus d’informations, consultez [Restart-Service](https://technet.microsoft.com/library/hh849823.aspx).

### <a name="enable-firewall-rules-for-the-dns-proxy-service"></a>Activer les règles de pare-feu pour le service proxy DNS
Vous pouvez utiliser la commande Windows PowerShell suivante pour créer une règle de pare-feu qui autorise les exceptions pour que le proxy communique avec la machine virtuelle et le serveur iDNS.
    
    Enable-NetFirewallRule -DisplayGroup 'DNS Proxy Firewall'

Pour plus d’informations, consultez [Enable-NetFirewallRule](https://technet.microsoft.com/library/jj554869.aspx).
    
### <a name="validate-the-idns-service"></a>Valider le service iDNS
Pour valider le service iDNS, vous devez déployer un exemple de charge de travail de locataire.

Pour plus d’informations, consultez [créer une machine virtuelle et se connecter à un réseau virtuel locataire ou à un réseau local virtuel](https://technet.microsoft.com/windows-server-docs/networking/sdn/manage/create-a-tenant-vm).

Si vous souhaitez que la machine virtuelle cliente utilise le service iDNS, vous devez laisser la configuration du serveur DNS des interfaces de réseau d’ordinateurs virtuels vide et autoriser les interfaces à utiliser le protocole DHCP. 

Une fois que la machine virtuelle avec une interface réseau de ce type a été initialisée, elle reçoit automatiquement une configuration qui permet à la machine virtuelle d’utiliser iDNS, et la machine virtuelle commence immédiatement à effectuer la résolution de noms à l’aide du service iDNS.

Si vous configurez la machine virtuelle cliente pour utiliser le service iDNS en laissant vierge le serveur DNS de l’interface réseau et les informations sur le serveur DNS auxiliaire, le contrôleur de réseau fournit une adresse IP à la machine virtuelle et effectue une inscription de nom DNS pour le compte de la machine virtuelle avec le serveur iDNS . 

Le contrôleur de réseau informe également le proxy iDNS sur la machine virtuelle et les informations requises pour effectuer la résolution de noms pour la machine virtuelle. 

Lorsque la machine virtuelle lance une requête DNS, le proxy agit comme un redirecteur de la requête du réseau virtuel vers le service iDNS. 

Le proxy DNS s’assure également que les requêtes de machine virtuelle du locataire sont isolées. Si le serveur iDNS fait autorité pour la requête, le serveur iDNS répond avec une réponse faisant autorité. Si le serveur iDNS ne fait pas autorité pour la requête, il effectue une récurrence DNS pour résoudre les noms Internet.

>[!NOTE]
>Ces informations sont incluses dans la section **configuration AttachToVirtualNetwork** dans SDNExpressTenant. ps1. Pour plus d’informations, consultez [déployer une infrastructure réseau définie par logiciel à l’aide de scripts](https://technet.microsoft.com/windows-server-docs/networking/sdn/deploy/deploy-a-software-defined-network-infrastructure-using-scripts).

