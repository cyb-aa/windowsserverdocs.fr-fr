---
title: Contrôle de sortie dans un réseau virtuel
description: Un aspect fondamental de la monétisation du réseau Cloud est la sortie de la bande passante réseau. Par exemple, les transferts de données sortantes dans Microsoft Azure modèle d’entreprise. Les données sortantes sont facturées en fonction de la quantité totale de données déplacées des centres de données Azure via Internet au cours d’un cycle de facturation donné.
manager: dougkim
ms.prod: windows-server
ms.technology: networking-hv-switch
ms.topic: get-started-article
ms.assetid: ''
ms.author: pashort
author: shortpatti
ms.date: 10/02/2018
ms.openlocfilehash: e68a3889867b75152ea941ac1d8eb113b9acd3cb
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71406004"
---
# <a name="egress-metering-in-a-virtual-network"></a>Sortie du contrôle dans un réseau virtuel

>S’applique à : Windows Server 2019


Un aspect fondamental de la monétisation du réseau Cloud est la capacité à facturer l’utilisation de la bande passante réseau. Les données sortantes sont facturées en fonction de la quantité totale de données déplacées en dehors du centre de données via Internet dans un cycle de facturation donné.

Le contrôle de sortie pour le trafic réseau SDN dans Windows Server 2019 permet d’offrir des compteurs d’utilisation pour les transferts de données sortantes. Le trafic réseau qui quitte chaque réseau virtuel, mais reste dans le centre de données, peut être suivi séparément afin de pouvoir être exclu des calculs de facturation. Les paquets liés aux adresses IP de destination qui ne sont pas incluses dans l’une des plages d’adresses non facturées sont suivis comme des transferts de données sortants facturés.

## <a name="virtual-network-unbilled-address-ranges-whitelist-of-ip-ranges"></a>Plages d’adresses non facturées du réseau virtuel (liste verte des plages d’adresses IP)

Vous pouvez trouver des plages d’adresses non facturées sous la propriété **UnbilledAddressRanges** d’un réseau virtuel existant. Par défaut, aucune plage d’adresses n’est ajoutée.

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


## <a name="example-manage-the-unbilled-address-ranges-of-a-virtual-network"></a>Exemple : Gérer les plages d’adresses non facturées d’un réseau virtuel

Vous pouvez gérer l’ensemble des préfixes de sous-réseau IP à exclure de la mesure de sortie facturée en définissant la propriété **UnbilledAddressRange** d’un réseau virtuel.  Tout trafic envoyé par les interfaces réseau sur le réseau virtuel avec une adresse IP de destination correspondant à l’un des préfixes ne sera pas inclus dans la propriété BilledEgressBytes.

1.  Mettez à jour la propriété **UnbilledAddressRanges** pour qu’elle contienne les sous-réseaux qui ne seront pas facturés pour l’accès.

    ```PowerShell
    $vnet = Get-NetworkControllerVirtualNetwork -ConnectionUri $uri -ResourceID "VNet1"
    $vnet.Properties.UnbilledAddressRanges = "10.10.2.0/24,10.10.3.0/24"
    ```

    >[!TIP]
    >Si vous ajoutez plusieurs sous-réseaux IP, utilisez une virgule entre chacun des sous-réseaux IP.  N’incluez pas d’espace avant ou après la virgule.

2.  Mettez à jour la ressource de réseau virtuel avec la propriété **UnbilledAddressRanges** modifiée.

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


3. Vérifiez le réseau virtuel pour voir les **UnbilledAddressRanges**configurés.

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

## <a name="check-the-billed-the-unbilled-egress-usage-of-a-virtual-network"></a>Vérifier la facturation de l’utilisation de sortie non facturée d’un réseau virtuel

Après avoir configuré la propriété **UnbilledAddressRanges** , vous pouvez vérifier l’utilisation facturée et non facturée de chaque sous-réseau au sein d’un réseau virtuel. Le trafic sortant est mis à jour toutes les quatre minutes avec le nombre total d’octets des plages facturées et non facturées.

Les propriétés suivantes sont disponibles pour chaque sous-réseau virtuel :

-   **UnbilledEgressBytes** affiche le nombre d’octets non facturés envoyés par les interfaces réseau connectées à ce sous-réseau virtuel. Les octets non facturés sont des octets envoyés à des plages d’adresses qui font partie de la propriété **UnbilledAddressRanges** du réseau virtuel parent.

-   **BilledEgressBytes** affiche le nombre d’octets facturés envoyés par les interfaces réseau connectées à ce sous-réseau virtuel. Les octets facturés sont des octets envoyés à des plages d’adresses qui ne font pas partie de la propriété **UnbilledAddressRanges** du réseau virtuel parent.

Utilisez l’exemple suivant pour interroger l’utilisation de sortie :

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
