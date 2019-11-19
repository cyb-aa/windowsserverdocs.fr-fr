---
title: Activation automatique de machine virtuelle
TOCTitle: Automatic VM Activation
description: Découvrez comment activer des machines virtuelles dans Windows Server 2019, Windows Server 2016 et Windows Server 2012 R2.
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: server-general
ms.tgt_pltfrm: na
ms.topic: article
author: lizap
ms.author: elizapo
ms.localizationpriority: medium
ms.openlocfilehash: 43c0ce500058bd4115d58b68dc79068a52c0bb3e
ms.sourcegitcommit: b9ec35416a06854c1bc875a2b731d42a436fe313
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/12/2019
ms.locfileid: "73956091"
---
# <a name="automatic-virtual-machine-activation"></a>Activation automatique de machine virtuelle

> S’applique à : Windows Server 2019, Windows Server (Canal semi-annuel), Windows Server 2016, Windows Server 2012 R2

L’activation automatique d’ordinateur virtuel (AVMA) joue le rôle de mécanisme de preuve d’achat qui permet de garantir que les produits Windows sont utilisés conformément aux droits d’utilisation de logiciels et termes du contrat de licence logiciel Microsoft.

AVMA vous permet d’installer des ordinateurs virtuels sur un serveur Windows correctement activé, sans avoir à gérer de clés de produit pour chaque ordinateur virtuel, même dans des environnements déconnectés. AVMA lie l'activation de l'ordinateur virtuel au serveur de virtualisation sous licence et active l'ordinateur virtuel lorsqu'il démarre. AVMA fournit également des rapports en temps réel sur l’utilisation et des données d’historique sur l’état de licence de l’ordinateur virtuel. Les rapports et données de suivi sont disponibles sur le serveur de virtualisation.

## <a name="practical-applications"></a>Cas pratiques

AVMA présente plusieurs avantages pour les serveurs de virtualisation qui sont activés avec des licences en volume ou OEM.

Les responsables de centre de données de serveur peuvent utiliser AVMA pour :

  - activer les ordinateurs virtuels dans des emplacements distants ;

  - activer des ordinateurs virtuels avec ou sans connexion Internet ;

  - suivre l’utilisation et les licences d’ordinateur virtuel à partir du serveur de virtualisation, sans exiger de droits d’accès sur les systèmes virtualisés.

Aucune gestion de clé de produit ni lecture d’autocollant sur les serveurs n’est requise. L’ordinateur virtuel est activé et continue de fonctionner même lors de sa migration entre des serveurs de virtualisation.

Les partenaires SPLA (Service Provider License Agreement) et d’autres fournisseurs d’hébergement n’ont pas besoin de partager des clés de produit entre clients ou d’accéder à l’ordinateur virtuel du client pour l’activer. L’activation de l’ordinateur virtuel est transparente pour le client quand AVMA est utilisé. Les fournisseurs d’hébergement peuvent utiliser les journaux serveur pour vérifier la conformité des licences et suivre l’historique de l’utilisation des clients.

## <a name="system-requirements"></a>Configuration requise

AVMA nécessite un serveur de virtualisation Microsoft exécutant Windows Server Datacenter 2019, Windows Server 2016 Datacenter ou Windows Server 2012 R2. 

Voici les invités qui peuvent être activés par les hôtes de différentes versions :

|Version du serveur hôte|Windows Server 2019|Windows Server 2016|Windows Server 2012 R2|
|-|-|-|-|
|Windows Server 2019|X|X|X|
|Windows Server 2016| |X|X|
|Windows Server 2012 R2| ||X|

Notez que ceux-ci activent toutes les éditions (Datacenter, Standard ou Essentials).

Cet outil ne fonctionne pas avec d’autres technologies de serveur de virtualisation.

## <a name="how-to-implement-avma"></a>Comment implémenter AVMA

1.  Sur un serveur de virtualisation Windows Server Datacenter, installez et configurez le rôle serveur Microsoft Hyper-V. Pour plus d’informations, consultez [Installer Hyper-V Server](../virtualization/hyper-v/get-started/install-the-hyper-v-role-on-windows-server.md).

2.  [Créez une machine virtuelle](../virtualization/hyper-v/get-started/create-a-virtual-machine-in-hyper-v.md) et installez-y un système d’exploitation serveur pris en charge.

3.  Installez la clé AVMA dans l’ordinateur virtuel. À partir d’une invite de commandes avec élévation de privilèges, exécutez la commande suivante :
    
    ``` 
    slmgr /ipk <AVMA_key>  
    ```

L’ordinateur virtuel active automatiquement la licence sur le serveur de virtualisation.


> [!TIP]
> Vous pouvez également employer les clés AVMA dans tout fichier d’installation Unattend.exe.


## <a name="avma-keys"></a>Clés AVMA

Les clés AVMA suivantes peuvent être utilisées pour Windows Server 2019.

|Édition|   Clé AVMA|
|-|-|
|Datacenter|    H3RNG-8C32Q-Q8FRX-6TDXV-WMBMW|
|Standard|  TNK62-RXVTB-4P47B-2D623-4GF74|
|Éléments essentiels|    2CTP7-NHT64-BP62M-FV6GG-HFV28|
 
Les clés AVMA suivantes peuvent être utilisées pour Windows Server version 1909, 1903 et 1809.

|Édition|   Clé AVMA|
|-|-|
|Datacenter|    H3RNG-8C32Q-Q8FRX-6TDXV-WMBMW|
|Standard|  TNK62-RXVTB-4P47B-2D623-4GF74|

Les clés AVMA suivantes peuvent être utilisées pour Windows Server versions 1803 et 1709.

|Édition|Clé AVMA|
|-|-|
|Datacenter|TMJ3Y-NTRTM-FJYXT-T22BY-CWG3J|
|Standard|C3RCX-M6NRP-6CXC9-TW2F2-4RHYD|


Les clés AVMA suivantes peuvent être utilisées pour Windows Server 2016.

|Édition|Clé AVMA|
|-|-|
|Datacenter|TMJ3Y-NTRTM-FJYXT-T22BY-CWG3J|
|Standard|C3RCX-M6NRP-6CXC9-TW2F2-4RHYD|
|Éléments essentiels|B4YNW-62DX9-W8V6M-82649-MHBKQ|


Les clés AVMA suivantes peuvent être utilisées pour Windows Server 2012 R2.

|Édition|Clé AVMA|
|-|-|
|Datacenter|Y4TGP-NPTV9-HTC2H-7MGQ3-DV4TW|
|Standard|DBGBW-NPF86-BJVTX-K3WKJ-MTB6V|
|Éléments essentiels|K2XGM-NMBT3-2R6Q8-WF2FK-P36R2|

## <a name="reporting-and-tracking"></a>Rapports et suivi

Le Registre (paire de valeurs de clé ou KVP) sur le serveur de virtualisation fournit des données de suivi en temps réel pour les systèmes d’exploitation invités. Étant donné que la clé de Registre se déplace avec l’ordinateur virtuel, vous pouvez également obtenir les informations de licence. Par défaut, la paire KVP retourne des informations sur l’ordinateur virtuel, notamment les suivantes :

  - Nom de domaine complet

  - Système d’exploitation et Service Packs installés

  - Architecture du processeur

  - Adresses réseau IPv4 et IPv6

  - Adresses RDP

Pour plus d’informations sur la façon d’obtenir ces informations, consultez [Hyper-V Script : Looking at KVP GuestIntrinsicExchangeItems](https://blogs.msdn.com/b/virtual_pc_guy/archive/2008/11/18/hyper-v-script-looking-at-kvp-guestintrinsicexchangeitems.aspx).


> [!NOTE]
> Les données de la paire KVP ne sont pas sécurisées. Elles peuvent être modifiées et ces modifications ne sont pas surveillées.



> [!IMPORTANT]
> Les données de la paire KVP doivent être supprimées si la clé AVMA est remplacée par une autre clé de produit (clé commercialisée, OEM ou de licence en volume).


Les données d’historique sur les demandes AVMA sont disponibles dans un fichier journal sur le serveur de virtualisation (ID d’événement 12310).

Puisque le processus d’activation AVMA est transparent, les messages d’erreur ne sont pas affichés. Toutefois, les événements suivants sont capturés dans un fichier journal sur les ordinateurs virtuels (ID d’événement 12309).

|Notification|Description|
|-|-|
|Réussite d’AVMA|L’ordinateur virtuel a été activé.|
|Hôte non valide|Le serveur de virtualisation ne répond pas. Cette situation peut se produire lorsque le serveur n’exécute pas une version prise en charge de Windows.|
|Données non valides|Cette notification résulte généralement d’un échec de communication entre le serveur de virtualisation et l’ordinateur virtuel, souvent provoqué par un endommagement, un chiffrement ou une incompatibilité des données.|
|Activation refusée|Le serveur de virtualisation n’a pas pu activer le système d’exploitation invité car l’ID AVMA ne correspondait pas.|

