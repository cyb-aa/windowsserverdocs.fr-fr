---
title: Compatibilité des fonctionnalités Hyper-V par génération et invité
description: Répertorie les générations et les systèmes d’exploitation compatibles avec les principales fonctionnalités Hyper-V
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 81c1f32d-7814-4992-8a66-dd4b77c939b4
author: KBDAzure
ms.author: kathydav
ms.date: 12/05/2016
ms.openlocfilehash: cdca6c31ff14fe63e99ec4afa2581885677bb61d
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71365540"
---
# <a name="hyper-v-feature-compatibility-by-generation-and-guest"></a>Compatibilité des fonctionnalités Hyper-V par génération et invité

>S'applique à : Windows Server 2016
  
Les tableaux de cet article décrivent les générations et les systèmes d’exploitation compatibles avec certaines fonctionnalités Hyper-V, regroupées par catégories. En général, vous obtiendrez la meilleure disponibilité des fonctionnalités avec un ordinateur virtuel de génération 2 qui exécute le système d’exploitation le plus récent.  
  
N’oubliez pas que certaines fonctionnalités dépendent du matériel ou d’une autre infrastructure. Pour plus d’informations sur le matériel, consultez [Configuration système requise pour Hyper-V sur Windows Server 2016](System-requirements-for-Hyper-V-on-Windows.md). Dans certains cas, une fonctionnalité peut être utilisée avec n’importe quel système d’exploitation invité pris en charge. Pour plus d’informations sur les systèmes d’exploitation pris en charge, consultez :  
  
* [Machines virtuelles Linux et FreeBSD prises en charge](Supported-Linux-and-FreeBSD-virtual-machines-for-Hyper-V-on-Windows.md)  
* [Systèmes d’exploitation invités Windows pris en charge](Supported-Windows-guest-operating-systems-for-Hyper-V-on-Windows.md)  
  
## <a name="availability-and-backup"></a>Disponibilité et sauvegarde  
  
Fonctionnalité  | Toute | Système d’exploitation invité  
------------- | ------------- | -----------  
Points de contrôle | 1 et 2 | Tout invité pris en charge  
Clustering invité | 1 et 2 | Invités qui exécutent des applications prenant en charge les clusters et sur lesquels sont installés des logiciels cibles iSCSI  
Réplication | 1 et 2 | Tout invité pris en charge  
Contrôleur de domaine | 1 et 2 | Tout invité Windows Server pris en charge utilisant uniquement des points de contrôle de production. Voir [systèmes d’exploitation invités Windows Server pris en charge](https://docs.microsoft.com/windows-server/virtualization/hyper-v/supported-windows-guest-operating-systems-for-hyper-v-on-windows#supported-windows-server-guest-operating-systems)   
  
## <a name="compute"></a>Calcul :  
  
Fonctionnalité  | Toute | Système d’exploitation invité  
------------- | ------------- | -----------  
Mémoire dynamique | 1 et 2 | Versions spécifiques des invités pris en charge. Consultez la page [vue d’ensemble d’Hyper-V mémoire dynamique](https://technet.microsoft.com/library/hh831766.aspx) pour les versions antérieures à windows server 2016 et Windows 10.  
Ajout/suppression de mémoire à chaud | 1 et 2 | Windows Server 2016, Windows 10  
NUMA virtuel | 1 et 2 | Tout invité pris en charge  
  
## <a name="development-and-test"></a>Développement et test  
Fonctionnalité  | Toute | Système d’exploitation invité  
------------- | ------------- | -----------  
Ports COM/série | 1 et 2 <br>**Remarque :** Pour la génération 2, utilisez Windows PowerShell pour configurer. Pour plus d’informations, consultez [Ajouter un port com pour le débogage du noyau](./plan/should-i-create-a-generation-1-or-2-virtual-machine-in-hyper-v.md#add-a-com-port-for-kernel-debugging). | Tout invité pris en charge  
  
## <a name="mobility"></a>Mobilité  
  
Fonctionnalité  | Toute | Système d’exploitation invité  
------------- | ------------- | -----------  
Migration en direct  | 1 et 2 |  Tout invité pris en charge  
Importation/exportation | 1 et 2 |  Tout invité pris en charge  
  
## <a name="networking"></a>Mise en réseau  
  
Fonctionnalité  | Toute | Système d’exploitation invité  
------------- | ------------- | -----------  
Ajout/suppression d’une carte réseau virtuelle à chaud | 2 | Tout invité pris en charge  
Carte réseau virtuelle héritée | 1 | Tout invité pris en charge  
Virtualisation d’entrée/sortie à la racine unique (SR-IOV) | 1 et 2 | invités Windows 64 bits, à partir de Windows Server 2012 et Windows 8.  
Plusieurs files d’attente d’ordinateurs virtuels (VMMQ) | 1 et 2  | Tout invité pris en charge  
  
## <a name="remote-connection-experience"></a>Expérience de connexion à distance  
  
Fonctionnalité  | Toute | Système d’exploitation invité  
------------- | ------------- | -----------  
Attribution d’appareil discrète (DDA) | 1 et 2 | Windows Server 2016, Windows Server 2012 R2 uniquement avec la mise à jour 3133690 installée, Windows 10 <br> **Remarque :** Pour plus d’informations sur la mise à jour 3133690, consultez [cet](https://support.microsoft.com/kb/3133690) article de support.  
Mode de session amélioré | 1 et 2 | Windows Server 2016, Windows Server 2012 R2, Windows 10 et Windows 8.1, avec Services Bureau à distance activé <br>**Remarque**: Vous devrez peut-être également configurer l’hôte. Pour plus d’informations, consultez [utiliser des ressources locales sur un ordinateur virtuel Hyper-V avec vmconnect](./learn-more/Use-local-resources-on-Hyper-V-virtual-machine-with-VMConnect.md).  
RemoteFx | 1 et 2 | Génération 1 sur les versions de Windows 32 bits et 64 bits à partir de Windows 8. <br> Génération 2 sur les versions 64 bits de Windows 10  
  
## <a name="security"></a>Sécurité  
  
Fonctionnalité  | Toute | Système d’exploitation invité  
------------- | ------------- | -----------  
Démarrage sécurisé | 2 | **Linux**: Ubuntu 14,04 et versions ultérieures, SUSE Linux Enterprise Server 12 et versions ultérieures, Red Hat Enterprise Linux 7,0 et versions ultérieures, et CentOS 7,0 et versions ultérieures<br>**Windows**: Toutes les versions prises en charge qui peuvent s’exécuter sur un ordinateur virtuel de génération 2  
Machines virtuelles dotées d’une protection maximale | 2 | **Windows**: Toutes les versions prises en charge qui peuvent s’exécuter sur un ordinateur virtuel de génération 2  
  
## <a name="storage"></a>Stockage  
  
Fonctionnalité  | Toute | Système d’exploitation invité  
------------- | ------------- | -----------  
Disques durs virtuels partagés (VHDX uniquement) | 1 et 2  | Windows Server 2016, Windows Server 2012 R2, Windows Server 2012  
SMB3 | 1 et 2 | Tout ce qui prend en charge SMB3  
Espaces de stockage direct | 2 | Windows Server 2016  
Fibre Channel virtuel | 1 et 2 | Windows Server 2016, Windows Server 2012 R2, Windows Server 2012  
Format VHDX | 1 et 2 | Tout invité pris en charge   
  
  
  
  
    


