---
title: Créer des stratégies de sécurité avec les listes de contrôle d’accès de ports étendues
description: Cette rubrique fournit des informations sur les listes de Access Control de ports (ACL) étendues dans Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-hv-switch
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a92e61c3-f7d4-4e42-8575-79d75d05a218
ms.author: pashort
author: shortpatti
ms.openlocfilehash: f76a3146c1cb38dab26019be655fadbd15d924c5
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71365603"
---
# <a name="create-security-policies-with-extended-port-access-control-lists"></a>Créer des stratégies de sécurité avec les listes de contrôle d’accès de ports étendues

>S'applique à : Windows Server (Canal semi-annuel), Windows Server 2016

Cette rubrique fournit des informations sur les listes de Access Control de ports (ACL) étendues dans Windows Server 2016. Vous pouvez configurer les listes de contrôle d’accès étendues sur le commutateur virtuel Hyper-V pour autoriser et bloquer le trafic réseau depuis et vers les ordinateurs virtuels (VM) qui sont connectés au commutateur via les cartes réseau virtuelles.  
  
Cette rubrique contient les sections suivantes.  
  
-   [Règles ACL détaillées](#bkmk_detailed)  
  
-   [Règles ACL avec état](#bkmk_stateful)  
  
## <a name="bkmk_detailed"></a>Règles ACL détaillées  
Les listes de contrôle d’accès étendues du commutateur virtuel Hyper-V vous permettent de créer des règles détaillées que vous pouvez appliquer à des cartes réseau de machines virtuelles individuelles connectées au commutateur virtuel Hyper-V. La possibilité de créer des règles détaillées permet aux entreprises et aux fournisseurs de services Cloud (CSP) de répondre aux menaces de sécurité réseau dans un environnement de serveur partagé mutualisée.  
  
Grâce aux listes de contrôle d’accès étendues, au lieu de créer des règles qui bloquent ou autorisent tout le trafic de tous les protocoles depuis et vers un ordinateur virtuel, vous pouvez désormais bloquer ou autoriser individuellement le trafic réseau de chaque protocole qui s’exécute sur les ordinateurs virtuels. Vous pouvez créer des règles ACL étendues dans Windows Server 2016 qui incluent l’ensemble de paramètres 5 tuples suivant : adresse IP source, adresse IP de destination, protocole, port source et port de destination. De plus, chaque règle peut indiquer le sens du trafic réseau (entrant ou sortant) et l’action prise en charge par la règle (autoriser ou bloquer le trafic).  
  
Par exemple, vous pouvez configurer des listes de contrôle d’accès de port pour un ordinateur virtuel qui autorisent tout le trafic HTTP et HTTPS entrant et sortant sur le port 80, et bloquent le trafic réseau de tous les autres protocoles sur tous les ports.  
  
La possibilité de pouvoir identifier le trafic du protocole qui peut ou ne peut pas être reçu par les ordinateurs virtuels clients vous offre davantage de flexibilité lors de la configuration des stratégies de sécurité.  
  
### <a name="configuring-acl-rules-with-windows-powershell"></a>Configuration de règles ACL avec Windows PowerShell  
Pour configurer une ACL étendue, vous devez utiliser la commande Windows PowerShell **Add-VMNetworkAdapterExtendedAcl**. Cette commande utilise quatre syntaxes distinctes, avec une utilisation spécifique pour chaque syntaxe :  
  
1.  Ajoutez une liste de contrôle d’accès (ACL) étendue à toutes les cartes réseau d’une machine virtuelle nommée, qui est spécifiée par le premier paramètre,-VMName. Syntaxe :  
  
    > [!NOTE]  
    > Si vous souhaitez ajouter une liste de contrôle d’accès étendue à une carte réseau plutôt qu’à la totalité, vous pouvez spécifier la carte réseau avec le paramètre-VMNetworkAdapterName.  
  
    ```  
    Add-VMNetworkAdapterExtendedAcl [-VMName] <string[]> [-Action] <VMNetworkAdapterExtendedAclAction> {Allow | Deny}  
        [-Direction] <VMNetworkAdapterExtendedAclDirection> {Inbound | Outbound} [[-LocalIPAddress] <string>]  
        [[-RemoteIPAddress] <string>] [[-LocalPort] <string>] [[-RemotePort] <string>] [[-Protocol] <string>] [-Weight]  
        <int> [-Stateful <bool>] [-IdleSessionTimeout <int>] [-IsolationID <int>] [-Passthru] [-VMNetworkAdapterName  
        <string>] [-ComputerName <string[]>] [-WhatIf] [-Confirm]  [<CommonParameters>]  
    ```  
  
2.  Ajouter une ACL étendue à une carte réseau virtuelle spécifique sur un ordinateur virtuel spécifique. Syntaxe :  
  
    ```  
    Add-VMNetworkAdapterExtendedAcl [-VMNetworkAdapter] <VMNetworkAdapterBase[]> [-Action]  
        <VMNetworkAdapterExtendedAclAction> {Allow | Deny} [-Direction] <VMNetworkAdapterExtendedAclDirection> {Inbound |  
        Outbound} [[-LocalIPAddress] <string>] [[-RemoteIPAddress] <string>] [[-LocalPort] <string>] [[-RemotePort]  
        <string>] [[-Protocol] <string>] [-Weight] <int> [-Stateful <bool>] [-IdleSessionTimeout <int>] [-IsolationID  
        <int>] [-Passthru] [-WhatIf] [-Confirm]  [<CommonParameters>]  
    ```  
  
3.  Ajoutez une ACL étendue à toutes les cartes réseau virtuelles qui sont réservées à une utilisation par le système d’exploitation de gestion hôte Hyper-V.  
  
    > [!NOTE]  
    > Si vous souhaitez ajouter une liste de contrôle d’accès étendue à une carte réseau plutôt qu’à la totalité, vous pouvez spécifier la carte réseau avec le paramètre-VMNetworkAdapterName.  
  
    ```  
    Add-VMNetworkAdapterExtendedAcl [-Action] <VMNetworkAdapterExtendedAclAction> {Allow | Deny} [-Direction]  
        <VMNetworkAdapterExtendedAclDirection> {Inbound | Outbound} [[-LocalIPAddress] <string>] [[-RemoteIPAddress]  
        <string>] [[-LocalPort] <string>] [[-RemotePort] <string>] [[-Protocol] <string>] [-Weight] <int> -ManagementOS  
        [-Stateful <bool>] [-IdleSessionTimeout <int>] [-IsolationID <int>] [-Passthru] [-VMNetworkAdapterName <string>]  
        [-ComputerName <string[]>] [-WhatIf] [-Confirm]  [<CommonParameters>]  
    ```  
  
4.  Ajoutez une liste de contrôle d’accès étendue à un objet de machine virtuelle que vous avez créé dans Windows PowerShell, par exemple **$VM = obtenir-VM « my_vm »** . Dans la prochaine ligne de code, vous pouvez exécuter cette commande pour créer une ACL étendue avec la syntaxe suivante :  
  
    ```  
    Add-VMNetworkAdapterExtendedAcl [-VM] <VirtualMachine[]> [-Action] <VMNetworkAdapterExtendedAclAction> {Allow |  
        Deny} [-Direction] <VMNetworkAdapterExtendedAclDirection> {Inbound | Outbound} [[-LocalIPAddress] <string>]  
        [[-RemoteIPAddress] <string>] [[-LocalPort] <string>] [[-RemotePort] <string>] [[-Protocol] <string>] [-Weight]  
        <int> [-Stateful <bool>] [-IdleSessionTimeout <int>] [-IsolationID <int>] [-Passthru] [-VMNetworkAdapterName  
        <string>] [-WhatIf] [-Confirm]  [<CommonParameters>]  
    ```  
  
### <a name="detailed-acl-rule-examples"></a>Exemples de règle ACL détaillée  
Vous trouverez ci-dessous plusieurs exemples d’utilisation de la commande **Add-VMNetworkAdapterExtendedAcl** pour configurer les ACL de port étendues et pour créer des stratégies de sécurité pour les ordinateurs virtuels.  
  
-   [Appliquer la sécurité au niveau de l’application](#bkmk_enforce)  
  
-   [Appliquer la sécurité au niveau utilisateur et au niveau de l’application](#bkmk_both)  
  
-   [Fournir la prise en charge de la sécurité à une application non-TCP/UDP](#bkmk_tcp)  
  
> [!NOTE]  
> Les valeurs pour le paramètre de règle **Direction** dans les tableaux ci-dessous sont basées sur le flux de trafic depuis et vers l’ordinateur virtuel pour lequel vous créez la règle. Si l’ordinateur virtuel reçoit du trafic, le trafic est entrant ; si l’ordinateur virtuel envoie du trafic, le trafic est sortant. Par exemple, si vous appliquez une règle à un ordinateur virtuel qui bloque le trafic entrant, la direction du trafic entrant s’entend des ressources externes vers l’ordinateur virtuel. Si vous appliquez une règle qui bloque le trafic sortant, la direction du trafic sortant s’entend de l’ordinateur virtuel local vers les ressources externes.  
  
### <a name="bkmk_enforce"></a>Appliquer la sécurité au niveau de l’application  
Dans la mesure où de nombreux serveurs d’applications utilisent les ports TCP/UDP standard avec des ordinateurs clients, il est facile de créer des règles qui bloquent ou autorisent l’accès à un serveur d’applications en filtrant le trafic qui arrive et qui part du port indiqué à l’application.  
  
Par exemple, vous voulez autoriser un utilisateur à se connecter à un serveur d’applications dans votre centre de données en utilisant une connexion Bureau à distance (RDP, Remote Desktop Connection). Dans la mesure où RDP utilise le port TCP 3389, vous pouvez rapidement créer la règle suivante :  
  
|Adresse IP source|Adresse IP de destination|Protocol|Port source|Port de destination|Direction|Action|  
|-------------|------------------|------------|---------------|--------------------|-------------|----------|  
|*|*|TCP|*|3389|Vers l’intérieur|Autoriser|  
  
Vous trouverez ci-dessous deux exemples de création de règles avec des commandes Windows PowerShell. Le premier exemple de règle bloque tout le trafic vers la machine virtuelle nommée « ApplicationServer ». Le deuxième exemple de règle, qui est appliqué à la carte réseau de la machine virtuelle nommée « ApplicationServer », autorise uniquement le trafic RDP entrant vers la machine virtuelle.  
  
> [!NOTE]  
> Lorsque vous créez des règles, vous pouvez utiliser le paramètre **-Weight** pour déterminer l’ordre dans lequel le commutateur virtuel Hyper-V traite les règles. Les valeurs de **-Weight** sont exprimées comme des entiers ; les règles avec un entier supérieur sont traitées avant les règles avec des entiers inférieurs. Par exemple, si vous avez appliqué deux règles à une carte réseau d’ordinateur virtuel, une avec une pondération de 1 et l’autre avec une pondération de 10, la règle avec la pondération de 10 est appliquée en premier.  
  
```  
Add-VMNetworkAdapterExtendedAcl -VMName "ApplicationServer" -Action "Deny" -Direction "Inbound" -Weight 1  
Add-VMNetworkAdapterExtendedAcl -VMName "ApplicationServer" -Action "Allow" -Direction "Inbound" -LocalPort 3389 -Protocol "TCP" -Weight 10  
```  
  
### <a name="bkmk_both"></a>Appliquer la sécurité au niveau utilisateur et au niveau de l’application  
Dans la mesure où une règle peut correspondre à un paquet IP 5-tuple (IP source, IP de destination, protocole, port source et port de destination), la règle peut mettre en œuvre une stratégie de sécurité plus détaillée qu’une ACL de port.  
  
Par exemple, si vous souhaitez fournir un service DHCP à un nombre limité d’ordinateurs clients à l’aide d’un ensemble spécifique de serveurs DHCP, vous pouvez configurer les règles suivantes sur l’ordinateur Windows Server 2016 qui exécute Hyper-V, où les machines virtuelles utilisateur sont hébergées :  
  
|Adresse IP source|Adresse IP de destination|Protocol|Port source|Port de destination|Direction|Action|  
|-------------|------------------|------------|---------------|--------------------|-------------|----------|  
|*|255.255.255.255|UDP|*|67|Vers l’extérieur|Autoriser|  
|*|10.175.124.0/25|UDP|*|67|Vers l’extérieur|Autoriser|  
|10.175.124.0/25|*|UDP|*|68|Vers l’intérieur|Autoriser|  
  
Vous trouverez ci-dessous des exemples de création de ces règles avec des commandes Windows PowerShell.  
  
```  
Add-VMNetworkAdapterExtendedAcl -VMName "ServerName" -Action "Deny" -Direction "Outbound" -Weight 1  
Add-VMNetworkAdapterExtendedAcl -VMName "ServerName" -Action "Allow" -Direction "Outbound" -RemoteIPAddress 255.255.255.255 -RemotePort 67 -Protocol "UDP"-Weight 10  
Add-VMNetworkAdapterExtendedAcl -VMName "ServerName" -Action "Allow" -Direction "Outbound" -RemoteIPAddress 10.175.124.0/25 -RemotePort 67 -Protocol "UDP"-Weight 20  
Add-VMNetworkAdapterExtendedAcl -VMName "ServerName" -Action "Allow" -Direction "Inbound" -RemoteIPAddress 10.175.124.0/25 -RemotePort 68 -Protocol "UDP"-Weight 20  
```  
  
### <a name="bkmk_tcp"></a>Fournir la prise en charge de la sécurité à une application non-TCP/UDP  
Bien que la grande majorité du trafic réseau dans un centre données utilise TCP et UDP, d’autres protocoles peuvent être utilisés. Par exemple, si vous voulez autoriser un groupe de serveurs à exécuter une application de multidiffusion IP qui utilise le protocole IGMP (Internet Group Management Protocol), vous pouvez créer la règle suivante.  
  
> [!NOTE]  
> Le numéro de protocole IP de IGMP est 0x02.  
  
|Adresse IP source|Adresse IP de destination|Protocol|Port source|Port de destination|Direction|Action|  
|-------------|------------------|------------|---------------|--------------------|-------------|----------|  
|*|*|0x02|*|*|Vers l’intérieur|Autoriser|  
|*|*|0x02|*|*|Vers l’extérieur|Autoriser|  
  
Vous trouverez ci-dessous un exemple de création de ces règles avec des commandes Windows PowerShell.  
  
```  
Add-VMNetworkAdapterExtendedAcl -VMName "ServerName" -Action "Allow" -Direction "Inbound" -Protocol 2 -Weight 20  
Add-VMNetworkAdapterExtendedAcl -VMName "ServerName" -Action "Allow" -Direction "Outbound" -Protocol 2 -Weight 20  
```  
  
## <a name="bkmk_stateful"></a>Règles ACL avec état  
Les ACL étendues permettent désormais de configurer des règles avec état. Une règle avec état filtre les paquets en fonction de cinq attributs dans une adresse IP de source de paquets, une adresse IP de destination, un protocole, un port source et un port de destination.  
  
Les règles avec état ont les particularités suivantes :  
  
-   Elles autorisent toujours le trafic et ne sont pas utilisées pour bloquer le trafic.  
  
-   Si vous indiquez que la valeur du paramètre **Direction** est entrant et que le trafic correspond à la règle, le commutateur virtuel Hyper-V crée dynamiquement une règle correspondante qui autorise l’ordinateur virtuel à envoyer le trafic sortant en réponse à la ressource externe.  
  
-   Si vous indiquez que la valeur du paramètre **Direction** est sortant et que le trafic correspond à la règle, le commutateur virtuel Hyper-V crée dynamiquement une règle correspondante qui autorise la réception du trafic entrant de la ressource externe par l’ordinateur virtuel.  
  
-   Elles comprennent un attribut de délai d’attente en secondes. Quand un paquet réseau arrive sur le commutateur et que le paquet correspond à une règle avec état, le commutateur virtuel Hyper-V crée un état afin que tous les paquets suivants dans les deux directions du flux soient autorisés. L’état expire si aucun trafic ne se produit dans l’une ou l’autre direction pendant la période de temps spécifiée par la valeur de délai d’attente.  
  
Voici un exemple de l’utilisation des règles avec état.  
  
### <a name="allow-inbound-remote-server-traffic-only-after-it-is-contacted-by-the-local-server"></a>Autoriser le trafic serveur distant entrant uniquement après avoir été contacté par le serveur local  
Dans certains cas, une règle avec état doit être utilisée, car elle seule peut suivre une connexion établie connue et distinguer cette connexion des autres connexions.  
  
Par exemple, si vous voulez autoriser un serveur d’applications VM à initier des connexions sur le port 80 vers les services Web sur Internet, et que vous voulez que les serveurs Web distants puissent répondre au trafic VM, vous pouvez configurer une règle avec état qui autorise le trafic sortant initial de l’ordinateur virtuel vers les services Web ; la règle étant avec état, le trafic retourné de l’ordinateur virtuel à partir des serveurs Web est également autorisé. Pour des raisons de sécurité, vous pouvez bloquer tout le reste du trafic entrant vers l’ordinateur virtuel.  
  
Pour obtenir cette configuration de règle, vous pouvez utiliser les paramètres du tableau ci-dessous.  
  
> [!NOTE]  
> En raison de limitations de mise en forme et de la quantité d’informations présentée dans le tableau ci-dessous, les informations ne sont pas affichées comme dans les précédents tableaux de ce document.  
  
|Paramètre|Règle 1|Règle 2|Règle 3|  
|-------------|----------|----------|----------|  
|Adresse IP source|*|*|*|  
|Adresse IP de destination|*|*|*|  
|Protocol|*|*|TCP|  
|Port source|*|*|*|  
|Port de destination|*|*|80|  
|Direction|Vers l’intérieur|Vers l’extérieur|Vers l’extérieur|  
|Action|Refuser|Refuser|Autoriser|  
|Avec état|Non|Non|Oui|  
|Délai d’attente (en secondes)|N/A|N/A|3600|  
  
La règle avec état autorise la connexion du serveur d’applications VM au serveur Web distant. Quand le premier paquet est envoyé, le commutateur virtuel Hyper-V crée dynamiquement deux états de flux pour autoriser tous les paquets envoyés au serveur Web distant et retournés par lui. Quand le flux de paquets entre les serveurs s’arrête, les états de flux dépassent le délai d’attente de 3 600 secondes, ou une heure.  
  
Vous trouverez ci-dessous un exemple de création de ces règles avec des commandes Windows PowerShell.  
  
```  
Add-VMNetworkAdapterExtendedAcl -VMName "ApplicationServer" -Action "Deny" -Direction "Inbound" -Weight 1   
Add-VMNetworkAdapterExtendedAcl -VMName "ApplicationServer" -Action "Deny" -Direction "Outbound" -Weight 1  
Add-VMNetworkAdapterExtendedAcl -VMName "ApplicationServer" -Action "Allow" -Direction "Outbound" 80 "TCP" -Weight 100 -Stateful -Timeout 3600  
```  
  


