---
title: Compatibilité avec la fonctionnalité Hyper-V génération et invité
description: Répertorie les générations et les systèmes d’exploitation qui sont compatibles avec les principales fonctionnalités d’Hyper-V
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 81c1f32d-7814-4992-8a66-dd4b77c939b4
author: KBDAzure
ms.author: kathydav
ms.date: 12/05/2016
ms.openlocfilehash: 1863c1736d3c8573b3d11c6bef492c6645d28a77
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59859760"
---
# <a name="hyper-v-feature-compatibility-by-generation-and-guest"></a>Compatibilité avec la fonctionnalité Hyper-V génération et invité

>S'applique à : Windows Server 2016
  
Les tables dans cet article montrent les générations et les systèmes d’exploitation qui sont compatibles avec certaines fonctionnalités Hyper-V, regroupées par catégories. En règle générale, vous obtenez la disponibilité des fonctionnalités avec une machine virtuelle de génération 2 qui exécute le système d’exploitation plus récent.  
  
N’oubliez pas que certaines fonctionnalités reposent sur le matériel ou toute autre infrastructure. Pour plus d’informations de matériel, consultez [configuration système requise pour Hyper-V sur Windows Server 2016](System-requirements-for-Hyper-V-on-Windows.md). Dans certains cas, une fonctionnalité peut être utilisée avec n’importe quel système d’exploitation invités pris en charge. Pour plus d’informations sur lequel les systèmes d’exploitation sont pris en charge, consultez :  
  
* [Prise en charge des machines virtuelles Linux et FreeBSD](Supported-Linux-and-FreeBSD-virtual-machines-for-Hyper-V-on-Windows.md)  
* [Systèmes d’exploitation invités Windows pris en charge](Supported-Windows-guest-operating-systems-for-Hyper-V-on-Windows.md)  
  
## <a name="availability-and-backup"></a>Disponibilité et sauvegarde  
  
Fonctionnalité  | génération | Système d’exploitation invité  
------------- | ------------- | -----------  
Points de contrôle | 1 et 2 | Les invités pris en charge  
Clustering invité | 1 et 2 | Invités qui exécutent des applications adaptée aux clusters et être équipés des logiciels de cible iSCSI  
Réplication | 1 et 2 | Les invités pris en charge  
Contrôleur de domaine | 1 et 2 | Tout pris en charge les invités Windows Server à l’aide de points de contrôle de production uniquement. Consultez [systèmes de d’exploitation invités pris en charge de Windows Server](https://docs.microsoft.com/windows-server/virtualization/hyper-v/supported-windows-guest-operating-systems-for-hyper-v-on-windows#supported-windows-server-guest-operating-systems)   
  
## <a name="compute"></a>Calcul :  
  
Fonctionnalité  | génération | Système d’exploitation invité  
------------- | ------------- | -----------  
Mémoire dynamique | 1 et 2 | Versions spécifiques des invités pris en charge. Consultez [Hyper-V Dynamic Memory Overview](https://technet.microsoft.com/library/hh831766.aspx) pour les versions antérieures à Windows Server 2016 et Windows 10.  
Ajout/suppression à chaud de mémoire | 1 et 2 | Windows Server 2016, Windows 10  
NUMA virtuel | 1 et 2 | Les invités pris en charge  
  
## <a name="development-and-test"></a>Développement et test  
Fonctionnalité  | génération | Système d’exploitation invité  
------------- | ------------- | -----------  
Ports COM/série | 1 et 2 <br>**Remarque :** Pour la génération 2, utilisez Windows PowerShell pour configurer. Pour plus d’informations, consultez [ajouter un port COM pour le débogage du noyau](./plan/should-i-create-a-generation-1-or-2-virtual-machine-in-hyper-v.md#BKMK_Debug). | Les invités pris en charge  
  
## <a name="mobility"></a>Mobilité  
  
Fonctionnalité  | génération | Système d’exploitation invité  
------------- | ------------- | -----------  
Migration en direct  | 1 et 2 |  Les invités pris en charge  
Importation/exportation | 1 et 2 |  Les invités pris en charge  
  
## <a name="networking"></a>Mise en réseau  
  
Fonctionnalité  | génération | Système d’exploitation invité  
------------- | ------------- | -----------  
Ajout/suppression à chaud de la carte réseau virtuelle | 2 | Les invités pris en charge  
Carte réseau virtuelle héritée | 1 | Les invités pris en charge  
Virtualisation d’entrée/sortie de racine unique (SR-IOV) | 1 et 2 | invités de Windows 64 bits, en commençant par Windows Server 2012 et Windows 8.  
File d’attente de plusieurs machines virtuelles (VMMQ) | 1 et 2  | Les invités pris en charge  
  
## <a name="remote-connection-experience"></a>Expérience de connexion à distance  
  
Fonctionnalité  | génération | Système d’exploitation invité  
------------- | ------------- | -----------  
Attribution discrète d’appareils (DDA) | 1 et 2 | Windows Server 2016, Windows Server 2012 R2 uniquement avec 3133690 de mise à jour installée, Windows 10 <br> **Remarque :** Pour plus d’informations sur la mise à jour 3133690, consultez [cela](https://support.microsoft.com/kb/3133690) article du support technique.  
Mode de session amélioré | 1 et 2 | Windows Server 2016, Windows Server 2012 R2, Windows 10 et Windows 8.1, avec les Services Bureau à distance activés <br>**Remarque**: Vous devrez peut-être également configurer l’hôte. Pour plus d’informations, consultez [utiliser les ressources locales sur l’ordinateur virtuel Hyper-V avec VMConnect](./learn-more/Use-local-resources-on-Hyper-V-virtual-machine-with-VMConnect.md).  
RemoteFx | 1 et 2 | Génération 1 sur les versions de Windows 32 bits et 64 bits en commençant par Windows 8. <br> Génération 2 sur les versions de Windows 10 64 bits  
  
## <a name="security"></a>Sécurité  
  
Fonctionnalité  | génération | Système d’exploitation invité  
------------- | ------------- | -----------  
Démarrage sécurisé | 2 | **Linux**: Ubuntu 14.04 et versions ultérieur, SUSE Linux Enterprise Server 12 et ultérieur, Red Hat Enterprise Linux 7.0 et versions ultérieur et CentOS 7.0 et versions ultérieures<br>**Windows**: Toutes les versions qui peuvent s’exécuter sur une machine virtuelle de génération 2 prises en charge  
Machines virtuelles dotées d’une protection maximale | 2 | **Windows**: Toutes les versions qui peuvent s’exécuter sur une machine virtuelle de génération 2 prises en charge  
  
## <a name="storage"></a>Stockage  
  
Fonctionnalité  | génération | Système d’exploitation invité  
------------- | ------------- | -----------  
Disques durs virtuels partagés (VHDX uniquement) | 1 et 2  | Windows Server 2016, Windows Server 2012 R2, Windows Server 2012  
SMB3 | 1 et 2 | Tous les qui prennent en charge SMB3  
Espaces de stockage direct | 2 | Windows Server 2016  
Fibre Channel virtuel | 1 et 2 | Windows Server 2016, Windows Server 2012 R2, Windows Server 2012  
Format VHDX | 1 et 2 | Les invités pris en charge   
  
  
  
  
    


