---
title: Contrôle de sortie dans un réseau virtuel
description: Un aspect fondamental de monétisation de mise en réseau de cloud est la sortie de la bande passante réseau. Par exemple, les transferts de données sortantes modèle d’entreprise dans Microsoft Azure. Données sortantes sont facturées en fonction de la quantité totale de données déplacées des centres de données Azure via Internet dans un cycle de facturation donné.
manager: dougkim
ms.prod: windows-server-threshold
ms.technology: networking-hv-switch
ms.topic: get-started-article
ms.assetid: ''
ms.author: pashort
author: shortpatti
ms.date: 10/02/2018
ms.openlocfilehash: ad1bed11308420e271b8e06410d5a4548181314a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59876420"
---
# <a name="egress-metering-in-a-virtual-network"></a>Sortie de contrôle dans un réseau virtuel

>S’applique à : Windows Server 2019


Un aspect fondamental de monétisation de mise en réseau de cloud est la possibilité de sont facturées à l’utilisation de la bande passante réseau. Données sortantes sont facturées en fonction de la quantité totale de données déplacées hors du centre de données via Internet dans un cycle de facturation donné.

Sortie de contrôle pour le trafic de réseau SDN dans Windows Server 2019 vous permettent d’offrir des compteurs d’utilisation pour les transferts de données sortantes. Le trafic réseau qui quitte chaque réseau virtuel, mais reste dans le centre de données peut par suivies séparément afin qu’il peut être exclu de calculs de facturation. Les paquets destinés aux adresses IP de destination qui ne sont pas inclus dans un des plages d’adresses non facturée en sont suivies comme facturés les transferts de données sortantes.

## <a name="virtual-network-unbilled-address-ranges-whitelist-of-ip-ranges"></a>(Liste d’autorisation de plages d’adresses IP) de plages d’adresses de réseau virtuel non facturé

Vous pouvez trouver des plages d’adresses non facturés sous le **UnbilledAddressRanges** propriété d’un réseau virtuel existant. Par défaut, il n’existe aucune des plages d’adresses ajoutés.

   ```PowerShell
   import-module NetworkController
   $uri = "https://sdn.contoso.com"

   (Get-NetworkControllerVirtualNetwork -ConnectionURI $URI -ResourceId "VNet1").properties
   ```

Votre sortie doit ressembler à ceci :
   ```
    AddressSpace           : Microsoft.Windows.NetworkController.AddressSpace
    DhcpOptions            :
    UnbilledAddressRanges  :
    ConfigurationState     :
    ProvisioningState      : Succeeded
    Subnets                : {21e71701-9f59-4ee5-b798-2a9d8c2762f0, 5f4758ef-9f96-40ca-a389-35c414e996cc,
                         29fe67b8-6f7b-486c-973b-8b9b987ec8b3}
    VirtualNetworkPeerings :
    EncryptionCredential   :
    LogicalNetwork         : Microsoft.Windows.NetworkController.LogicalNetwork
   ```


## <a name="example-manage-the-unbilled-address-ranges-of-a-virtual-network"></a>Exemple : Gérer les plages d’adresses non facturée d’un réseau virtuel

Vous pouvez gérer le jeu de préfixes de sous-réseau IP à exclure de la facturation de sortie de contrôle en définissant le **UnbilledAddressRange** propriété d’un réseau virtuel.  Tout le trafic envoyé par les interfaces réseau sur le réseau virtuel avec une adresse IP de destination qui correspond à l’un préfixes de ne pas être inclus dans la propriété BilledEgressBytes.

1.  Mise à jour le **UnbilledAddressRanges** propriété pour contenir les sous-réseaux qui ne seront pas facturés pour l’accès.

    ```PowerShell
    $vnet = Get-NetworkControllerVirtualNetwork -ConnectionUri $uri -ResourceID "VNet1"
    $vnet.Properties.UnbilledAddressRanges = "10.10.2.0/24,10.10.3.0/24"
    ```
    
    >[!TIP]
    >Si vous ajoutez plusieurs sous-réseaux IP, utilisez une virgule entre chacun des sous-réseaux IP.  N’incluez pas d’espaces avant ou après la virgule.

2.  Mettre à jour de la ressource de réseau virtuel avec le texte modifié **UnbilledAddressRanges** propriété.

    ```PowerShell
    New-NetworkControllerVirtualNetwork -ConnectionUri $uri -ResourceId "VNet1" -Properties $unbilled.Properties -PassInnerException
    ```

    Votre sortie doit ressembler à ceci :
    ```
    Confirm
    Performing the operation 'New-NetworkControllerVirtualNetwork' on entities of type
    'Microsoft.Windows.NetworkController.VirtualNetwork' via
    'https://sdn.contoso.com/networking/v3/virtualNetworks/VNet1'. Are you sure you want to continue?
    [Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"): y
    
    
    Tags             :
    ResourceRef      : /virtualNetworks/VNet1
    InstanceId       : 29654b0b-9091-4bed-ab01-e172225dc02d
    Etag             : W/"6970d0a3-3444-41d7-bbe4-36327968d853"
    ResourceMetadata :
    ResourceId       : VNet1
    Properties       : Microsoft.Windows.NetworkController.VirtualNetworkProperties
    ```


3.  Vérifiez le réseau virtuel a posteriori configuré **UnbilledAddressRanges**.

    ```PowerShell
    (Get-NetworkControllerVirtualNetwork -ConnectionUri $uri -ResourceID "VNet1").properties
    ```

    Votre sortie doit maintenant ressembler à ceci :
    ```
    AddressSpace           : Microsoft.Windows.NetworkController.AddressSpace
    DhcpOptions            :
    UnbilledAddressRanges  : 10.10.2.0/24,192.168.2.0/24
    ConfigurationState     :
    ProvisioningState      : Succeeded
    Subnets                : {21e71701-9f59-4ee5-b798-2a9d8c2762f0, 5f4758ef-9f96-40ca-a389-35c414e996cc,
                         29fe67b8-6f7b-486c-973b-8b9b987ec8b3}
    VirtualNetworkPeerings :
    EncryptionCredential   :
    LogicalNetwork         : Microsoft.Windows.NetworkController.LogicalNetwork
    ```

## <a name="check-the-billed-the-unbilled-egress-usage-of-a-virtual-network"></a>Vérifier le facturée l’utilisation non facturée en sortie d’un réseau virtuel

Après avoir configuré le **UnbilledAddressRanges** propriété, vous pouvez vérifier l’utilisation de la facturation et non facturée en sortie de chaque sous-réseau au sein d’un réseau virtuel. Le trafic de sortie met à jour toutes les quatre minutes avec le nombre total d’octets des plages facturées et non facturés.

Les propriétés suivantes sont disponibles pour chaque sous-réseau virtuel :

-   **UnbilledEgressBytes** indique le nombre d’octets non envoyés par les interfaces réseau connectées à ce sous-réseau virtuel. Octets non facturée en sont des octets envoyés aux plages d’adresses qui font partie de la **UnbilledAddressRanges** propriété du réseau virtuel parent.

-   **BilledEgressBytes** affiche le nombre d’octets facturées envoyés par les interfaces réseau connectées à ce sous-réseau virtuel. Facturé octets sont des octets envoyés aux plages d’adresses qui ne sont pas dans le cadre de la **UnbilledAddressRanges** propriété du réseau virtuel parent.

Utilisez l’exemple suivant pour l’utilisation de sortie de requête :

```PowerShell
(Get-NetworkControllerVirtualNetwork -ConnectionURI $URI -ResourceId "VNet1").properties.subnets.properties | ft AddressPrefix,BilledEgressBytes,UnbilledEgressBytes
```

Votre sortie doit ressembler à ceci :
```
AddressPrefix BilledEgressBytes UnbilledEgressBytes
------------- ----------------- -------------------
10.0.255.8/29          16827067                   0
10.0.2.0/24           781733019                   0
10.0.4.0/24                   0                   0
```
    

---