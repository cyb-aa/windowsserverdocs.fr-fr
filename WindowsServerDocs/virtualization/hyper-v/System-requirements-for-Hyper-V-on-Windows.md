---
title: Configuration système requise pour Hyper-V sur Windows Server
description: Répertorie les exigences de matériel et microprogramme pour Hyper-V dans Windows Server
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: bc4a4971-f727-40cd-91f5-2ee6d24b54cb
author: KBDAzure
ms.author: kathydav
ms.date: 9/30/2016
ms.openlocfilehash: 55114821b5ac2f1cc028c662217f4bee6980c923
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59845200"
---
# <a name="system-requirements-for-hyper-v-on-windows-server"></a>Configuration système requise pour Hyper-V sur Windows Server

>S'applique à : Windows Server 2016, Microsoft Hyper-V Server 2016, Windows Server 2019, Microsoft Hyper-V Server 2019

Hyper-V a une configuration matérielle spécifique, et certaines fonctionnalités Hyper-V ont des exigences supplémentaires. Utilisez les détails dans cet article pour déterminer quelles spécifications de votre système doit satisfaire pour pouvoir utiliser Hyper-V la façon dont vous prévoyez. Ensuite, passez en revue la [catalogue Windows Server](https://www.windowsservercatalog.com/). N’oubliez pas que configuration requise pour Hyper-V dépasse la configuration générale requise pour Windows Server 2016, car un environnement de virtualisation nécessite davantage de ressources informatiques.

Si vous utilisez déjà Hyper-V, il est probable que vous pouvez utiliser votre matériel existant. La configuration matérielle requise générale n’ont pas changé considérablement depuis Windows Server 2012 R2.  Toutefois, vous devez matériel plus récent à utilisez protégé les machines virtuelles ou attribution discrète d’appareils. Ces fonctionnalités reposent sur la prise en charge du matériel spécifique, comme décrit ci-dessous. Autrement, la principale différence dans le matériel est cette adresse de second niveau traduction (SLAT) est désormais requis au lieu de recommandé.

Pour plus d’informations sur les configurations prises en charge maximales pour Hyper-V, comme le nombre de machines virtuelles en cours d’exécution, consultez [planifier extensibilité Hyper-V dans Windows Server 2016](plan/Plan-for-Hyper-V-scalability-in-Windows-Server-2016.md). La liste des systèmes d’exploitation que vous pouvez exécuter sur vos machines virtuelles est couverte dans [Windows pris en charge les systèmes d’exploitation invités pour Hyper-V sur Windows Server](Supported-Windows-guest-operating-systems-for-Hyper-V-on-Windows.md).

## <a name="general-requirements"></a>Configuration générale requise

Quel que soit les fonctionnalités de Hyper-V que vous souhaitez utiliser, vous devez :

- Un processeur 64 bits avec traduction d’adresses de second niveau (SLAT). Pour installer les composants de virtualisation Hyper-V comme hyperviseur de Windows, le processeur doit avoir SLAT. Toutefois, il n’est pas nécessaire pour installer les outils de gestion Hyper-V, comme la connexion à une Machine virtuelle (VMConnect), le Gestionnaire Hyper-V et les applets de commande Hyper-V pour Windows PowerShell. Voir « Comment vérifier les conditions requises d’Hyper-V, » ci-dessous, pour déterminer si votre processeur a SLAT.

- Extensions de Mode de surveillance de machine virtuelle

- Mémoire insuffisante - plan pour *au moins* 4 Go de RAM. Plus de mémoire est préférable. Vous aurez besoin de suffisamment de mémoire pour l’hôte et toutes les machines virtuelles que vous souhaitez exécuter en même temps.

- Prise en charge de la virtualisation activée dans le BIOS ou UEFI :

  - Virtualisation à assistance matérielle. Il est disponible dans les processeurs qui incluent une option de virtualisation - plus particulièrement les processeurs à Intel Virtualization Technology (Intel VT) ou une technologie de virtualisation AMD-V (AMD).

  - La prévention de l’exécution des données par le matériel doit être disponible et activée. Pour les systèmes Intel, il s’agit du bit XD (execute disable bit). Pour les systèmes AMD, ceci est le bit NX (aucun bit de non-exécution).

## <a name="bkmk_CheckReq"></a>Comment vérifier la configuration requise pour Hyper-V

Ouvrez Windows PowerShell ou d’une invite de commandes et tapez :

```cmd
Systeminfo.exe
```

Faites défiler jusqu'à la section Configuration requise pour Hyper-V pour passer en revue le rapport.

## <a name="requirements-for-specific-features"></a>Configuration requise pour les fonctionnalités spécifiques

Voici la configuration requise pour l’attribution discrète d’appareils et des machines virtuelles protégées. Pour obtenir une description de ces fonctionnalités, consultez [quelles sont les nouveautés dans Hyper-V sur Windows Server](What-s-new-in-Hyper-V-on-Windows.md).

### <a name="discrete-device-assignment"></a>Attribution discrète d’appareils

**Hôte** exigences sont similaires aux exigences de la fonctionnalité de SR-IOV dans Hyper-V existants.

- Le processeur doit avoir soit Intel Extended Page Table (EPT) ou Nested Page Table (NPT d’AMD).

- Le chipset doit avoir :

  - Remappage - d’interruption VT-d d’Intel avec la fonctionnalité d’interruption remappage (VT-d2) ou n’importe quelle version de l’unité de gestion AMD d’e/s de mémoire (MMU d’e/s).

  - DMA remappage - VT-d d’Intel avec Invalidations en file d’attente ou de n’importe quel MMU d’e/s AMD.

  - Services de contrôle d’accès (ACS) sur la norme PCI Express racine des ports.

- Les tables de microprogramme doivent exposer la MMU d’e/s à l’hyperviseur de Windows. Notez que cette fonctionnalité peut être désactivée dans le BIOS ou UEFI. Pour obtenir des instructions, consultez la documentation ou contactez votre fabricant de matériel.

**Appareils** devez GPU ou la mémoire non volatile (NVMe) express. Pour le GPU, uniquement sur certains appareils prennent en charge l’attribution discrète d’appareils. Pour vérifier, consultez la documentation ou contactez votre fabricant de matériel. Pour plus d’informations sur cette fonctionnalité, y compris comment utiliser et considérations, consultez le billet «[discrètes affectation d’appareils--Description et arrière-plan](https://blogs.technet.com/b/virtualization/archive/2015/11/19/discrete-device-assignment.aspx)» dans le blog de virtualisation.

### <a name="shielded-virtual-machines"></a>Machines virtuelles dotées d’une protection maximale

Ces machines virtuelles s’appuient sur la sécurité basée sur la virtualisation et sont disponibles à compter de Windows Server 2016.

**Hôte** exigences sont :

- UEFI 2.3.1c - prend en charge le démarrage sécurisé, mesuré

  Ce qui suit deux sont facultatifs pour la sécurité basée sur la virtualisation en général, mais requis pour l’hôte si vous souhaitez que la protection de que ces fonctionnalités fournissent :

- TPM v2.0 - protège les ressources de sécurité de plateforme
- IOMMU (Intel VT-D) - pour l’hyperviseur peut fournir la protection d’accès (DMA) direct à la mémoire

**Machine virtuelle** exigences sont :

- 2e génération
- Windows Server 2012 ou version ultérieure comme système d’exploitation invité

