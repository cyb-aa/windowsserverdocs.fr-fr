---
title: Activation de machine virtuelle automatique
TOCTitle: Automatic VM Activation
description: Comment faire pour activer les ordinateurs virtuels dans Windows Server 2012 R2, Windows Server 2016 et Windows Server 2019
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: server-general
ms.tgt_pltfrm: na
ms.topic: article
author: lizap
ms.author: elizapo
ms.localizationpriority: medium
ms.openlocfilehash: 62873140c8e114ba537dc4fd3ff7c44868c33243
ms.sourcegitcommit: ca5c80bdb903b282e292488172a7cc92546574c0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/04/2018
ms.locfileid: "4375486"
---
# Activation de machine virtuelle automatique

> S’applique à: Windows Server 2019, canal semi-annuel de Windows Server, Windows Server 2016, Windows Server 2012 R2

L’Activation de Machine virtuelle automatique (AVMA) agit comme un mécanisme de preuve d’achat, vous aidant à garantir que les produits Windows sont utilisées conformément aux termes du contrat de licence de logiciel Microsoft les droits d’utilisation du produit.

AVMA vous permet d’installer des ordinateurs virtuels sur un serveur Windows activé correctement sans avoir à gérer les clés de produit pour chaque machine virtuelle individuel, même dans les environnements déconnectés. AVMA lie l’activation de la machine virtuelle sur le serveur de la virtualisation sous licence et Active la machine virtuelle lors de son démarrage. AVMA fournit également des rapports en temps réel sur l’utilisation et les données historiques sur l’état de licence de la machine virtuelle. Création de rapports et les données de suivi sont disponible sur le serveur de virtualisation.

## Applications pratiques

Sur les serveurs de virtualisation qui sont activés à l’aide du Gestionnaire de licences des licences en Volume ou OEM, AVMA offre plusieurs avantages.

Responsables de centre de données de serveur peuvent utiliser AVMA pour effectuer les opérations suivantes:

  - Activer les ordinateurs virtuels dans des emplacements distants

  - Activer les ordinateurs virtuels avec ou sans connexion internet

  - Effectuer le suivi des licences et utilisation de la machine virtuelle à partir du serveur de la virtualisation, sans nécessiter de droits d’accès sur les systèmes virtualisés

Il existe sans clé de produit pour gérer et aucune vignettes sur les serveurs à lire. La machine virtuelle est activée et continue de fonctionner même lors de la migration sur un groupe de serveurs de la virtualisation.

Les partenaires du contrat de licence des fournisseur de service (SPLA) et autres fournisseurs d’hébergement n’ont pas de partager des clés de produit avec des locataires ou accéder la machine virtuelle d’un client pour l’activer. Activation de machine virtuelle est transparente pour le client lorsque AVMA est utilisé. Fournisseurs d’hébergement peuvent utiliser les journaux de serveur pour vérifier la conformité des licences et pour effectuer le suivi de l’historique de l’utilisation de client.

## Configuration requise

AVMA requiert un serveur de la virtualisation de Microsoft exécutant Windows Server 2012 R2, Windows Server 2016 Datacenter ou Windows Server 2019 Datacenter. 

Voici les invités qui les hôtes de version différent peuvent activer:

|Version du serveur hôte|Windows Server 2019|WindowsServer2016|WindowsServer2012R2|
|-|-|-|-|
|Windows Server 2019|X|X|X|
|WindowsServer2016| |X|X|
|WindowsServer2012R2| ||X|

Notez que ces Active toutes les éditions (Datacenter, Standard ou Essentials).

Cet outil ne fonctionne pas avec d’autres technologies de virtualisation serveur.

## Comment implémenter AVMA

1.  Sur un serveur de la virtualisation Windows Server Datacenter, installer et configurer le rôle de serveur Hyper-V de Microsoft. Pour plus d’informations, voir [Installer Hyper-V Server](../virtualization/hyper-v/get-started/install-the-hyper-v-role-on-windows-server.md).

2.  [Créer une machine virtuelle](../virtualization/hyper-v/get-started/create-a-virtual-machine-in-hyper-v.md) et installer un système d’exploitation pris en charge sur ce dernier.

3.  Installez la clé AVMA sur l’ordinateur virtuel. À partir d’une invite de commandes avec élévation de privilèges, exécutez la commande suivante:
    
    ``` 
    slmgr /ipk <AVMA_key>  
    ```

La machine virtuelle sera automatiquement activé la licence par rapport au serveur de la virtualisation.


> [!TIP]
> Vous pouvez également utiliser les clés AVMA dans n’importe quel fichier d’installation Unattend.exe.


## Clés AVMA

Les touches AVMA suivantes peuvent être utilisées pour Windows Server 2019.

|Édition|   Clé AVMA|
|-|-|
|Datacenter|    H3RNG-8C32Q-Q8FRX-6TDXV-WMBMW|
|Standard|  TNK62-RXVTB-4P47B - 2D 623-4GF74|
|Éléments essentiels|    2CTP7-NHT64-BP62M-FV6GG-HFV28|
 
Les touches AVMA suivantes peuvent être utilisées pour Windows Server, version 1809.

|Édition|   Clé AVMA|
|-|-|
|Datacenter|    H3RNG-8C32Q-Q8FRX-6TDXV-WMBMW|
|Standard|  TNK62-RXVTB-4P47B - 2D 623-4GF74|

Les touches AVMA suivantes peuvent être utilisées pour Windows Server, version 1803 et 1709.

|Édition|Clé AVMA|
|-|-|
|Datacenter|TMJ3Y-NTRTM-FJYXT-T22BY-CWG3J|
|Standard|C3RCX-M6NRP-6CXC9-TW2F2-4RHYD|


Les touches AVMA suivantes peuvent être utilisées pour Windows Server 2016.

|Édition|Clé AVMA|
|-|-|
|Datacenter|TMJ3Y-NTRTM-FJYXT-T22BY-CWG3J|
|Standard|C3RCX-M6NRP-6CXC9-TW2F2-4RHYD|
|Éléments essentiels|B4YNW-62DX9-W8V6M-82649-MHBKQ|


Les touches AVMA suivantes peuvent être utilisées pour Windows Server 2012 R2.

|Édition|Clé AVMA|
|-|-|
|Datacenter|Y4TGP-NPTV9-HTC2H-7MGQ3-DV4TW|
|Standard|DBGBW-NPF86-BJVTX-K3WKJ-MTB6V|
|Éléments essentiels|K2XGM-NMBT3-2R6Q8-WF2FK-P36R2|

## Création de rapports et le suivi

Le Registre (KVP) sur le serveur de virtualisation fournit des données de suivi en temps réel pour les systèmes d’exploitation invités. Dans la mesure où la clé de Registre se déplace avec la machine virtuelle, vous pouvez obtenir également les informations de licence. Par défaut le KVP renvoie des informations sur la machine virtuelle, y compris les éléments suivants:

  - Nom de domaine complet

  - Système d’exploitation et les service packs installés

  - Architecture de processeur

  - Adresses réseau IPv4 et IPv6

  - Adresses RDP

Pour plus d’informations sur la façon d’obtenir des informations, voir [Script Hyper-V: regardant KVP GuestIntrinsicExchangeItems](http://blogs.msdn.com/b/virtual_pc_guy/archive/2008/11/18/hyper-v-script-looking-at-kvp-guestintrinsicexchangeitems.aspx).


> [!NOTE]
> Données KVP ne sont pas sécurisées. Il peut être modifié et n’est pas surveillé pour les modifications.



> [!IMPORTANT]
> Données KVP doivent être supprimées si la clé AVMA est remplacée par une autre clé de produit (version commerciale, d’OEM ou volume clé de licence).


Les données historiques relatives aux demandes AVMA sont disponibles dans un fichier journal sur le serveur de la virtualisation (EventID 12310).

Dans la mesure où le processus d’activation AVMA est transparent, les messages d’erreur ne sont pas affichées. Toutefois, les événements suivants sont capturées dans un fichier journal sur les ordinateurs virtuels (EventID 12309).

|Notification|Description|
|-|-|
|Réussite AVMA|La machine virtuelle a été activée.|
|Hôte non valide|Le serveur de virtualisation ne répond pas. Cela peut se produire lorsque le serveur n’exécute pas une version prise en charge de Windows.|
|Données non valides|Cela entraîne généralement un échec de communication entre le serveur de la virtualisation et la machine virtuelle, souvent causé par incompatibilité de corruption, de chiffrement ou de données.|
|Activation refusée|Le serveur de virtualisation Impossible d’activer le système d’exploitation invité, car le code AVMA ne correspond pas.|

