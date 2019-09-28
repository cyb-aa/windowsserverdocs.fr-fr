---
title: Configuration système requise pour Hyper-V sur Windows Server
description: Répertorie la configuration matérielle et logicielle requise pour Hyper-V dans Windows Server
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: bc4a4971-f727-40cd-91f5-2ee6d24b54cb
author: KBDAzure
ms.author: kathydav
ms.date: 9/30/2016
ms.openlocfilehash: fabaa1933fef836bb6ce3fc01badf337b832d072
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71365438"
---
# <a name="system-requirements-for-hyper-v-on-windows-server"></a>Configuration système requise pour Hyper-V sur Windows Server

>S'applique à : Windows Server 2016, Microsoft Hyper-V Server 2016, Windows Server 2019, Microsoft Hyper-V Server 2019

Hyper-V a une configuration matérielle requise spécifique et certaines fonctionnalités Hyper-V ont des exigences supplémentaires. Utilisez les détails de cet article pour déterminer les exigences auxquelles votre système doit répondre pour que vous puissiez utiliser Hyper-V comme vous le souhaitez. Ensuite, examinez le [Catalogue Windows Server](https://www.windowsservercatalog.com/). Gardez à l’esprit que la configuration requise pour Hyper-V dépasse la configuration minimale générale requise pour Windows Server 2016, car un environnement de virtualisation nécessite davantage de ressources informatiques.

Si vous utilisez déjà Hyper-V, il est probable que vous puissiez utiliser votre matériel existant. La configuration matérielle requise générale n’a pas changé de manière significative depuis Windows Server 2012 R2.  Toutefois, vous aurez besoin d’un matériel plus récent pour utiliser des machines virtuelles protégées ou une attribution discrète des appareils. Ces fonctionnalités s’appuient sur une prise en charge matérielle spécifique, comme décrit ci-dessous. À part cela, la principale différence de matériel réside dans le fait que la traduction d’adresses de second niveau (SLAT) est désormais requise à la place de la recommandation.

Pour plus d’informations sur les configurations maximales prises en charge pour Hyper-V, telles que le nombre d’ordinateurs virtuels en cours d’exécution, consultez [planifier l’extensibilité d’Hyper-v dans Windows Server 2016](plan/Plan-for-Hyper-V-scalability-in-Windows-Server-2016.md). La liste des systèmes d’exploitation que vous pouvez exécuter sur vos machines virtuelles est couverte par les [systèmes d’exploitation invités Windows pris en charge pour Hyper-V sur Windows Server](Supported-Windows-guest-operating-systems-for-Hyper-V-on-Windows.md).

## <a name="general-requirements"></a>Configuration générale requise

Quelles que soient les fonctionnalités Hyper-V que vous souhaitez utiliser, vous avez besoin des éléments suivants :

- Un processeur 64 bits avec la traduction d’adresses de second niveau (SLAT). Pour installer les composants de virtualisation Hyper-V tels que l’hyperviseur Windows, le processeur doit avoir une SLAT. Toutefois, il n’est pas nécessaire d’installer les outils de gestion Hyper-V tels que la connexion à un ordinateur virtuel (VMConnect), le Gestionnaire Hyper-V et les applets de commande Hyper-v pour Windows PowerShell. Consultez la section « Vérification des conditions requises pour Hyper-V » ci-dessous pour savoir si votre processeur a une SLAT.

- Extensions du mode d’analyse de machine virtuelle

- Un plan de mémoire suffisant pour *au moins* 4 Go de RAM. La mémoire est meilleure. Vous devez disposer de suffisamment de mémoire pour l’ordinateur hôte et toutes les machines virtuelles que vous souhaitez exécuter en même temps.

- Prise en charge de la virtualisation activée dans le BIOS ou UEFI :

  - Virtualisation à assistance matérielle. Elle est disponible dans les processeurs qui incluent une option de virtualisation, spécifiquement des processeurs avec la technologie Intel VT (Intel Virtualization Technology) ou AMD-V (AMD Virtualization).

  - La prévention de l’exécution des données par le matériel doit être disponible et activée. Pour les systèmes Intel, il s’agit du bit XD (Execute Disable Bit). Pour les systèmes AMD, il s’agit du bit NX (aucun bit d’exécution).

## <a name="how-to-check-for-hyper-v-requirements"></a>Guide pratique pour vérifier la configuration requise pour Hyper-V

Ouvrez Windows PowerShell ou une invite de commandes et tapez :

```cmd
Systeminfo.exe
```

Faites défiler jusqu’à la section Configuration requise pour Hyper-V pour examiner le rapport.

## <a name="requirements-for-specific-features"></a>Conditions requises pour des fonctionnalités spécifiques

Voici la configuration requise pour l’affectation discrète des appareils et les machines virtuelles protégées. Pour obtenir une description de ces fonctionnalités, consultez [Nouveautés d’Hyper-V sur Windows Server](What-s-new-in-Hyper-V-on-Windows.md).

### <a name="discrete-device-assignment"></a>Attribution d’appareil discrète

La configuration requise pour l' **ordinateur hôte** est semblable à celle des exigences existantes pour la fonctionnalité SR-IOV dans Hyper-V.

- Le processeur doit avoir la table EPT (Extended Page Table) d’Intel ou la table de pages imbriquée d’AMD (NPT).

- Le chipset doit avoir :

  - Interruption du remappage : VT-d Intel avec la fonctionnalité de remappage d’interruption (VT-D2) ou n’importe quelle version de l’unité de gestion de mémoire d’e/s AMD (MMU d’e/s).

  - Remappage DMA-VT-d Intel avec invalidations en file d’attente ou un MMU d’e/s AMD.

  - Access Control services (ACS) sur les ports racine PCI Express.

- Les tables de microprogrammes doivent exposer les MMU d’e/s à l’hyperviseur Windows. Notez que cette fonctionnalité peut être désactivée dans UEFI ou BIOS. Pour obtenir des instructions, consultez la documentation matérielle ou contactez le fabricant de votre matériel.

Les **appareils** ont besoin d’un GPU ou d’une mémoire non volatile Express (NVMe). Pour GPU, seuls certains appareils prennent en charge l’affectation discrète des appareils. Pour vous en assurer, consultez la documentation matérielle ou contactez le fabricant de votre matériel. Pour plus d’informations sur cette fonctionnalité, y compris son utilisation et les considérations à prendre en compte, consultez la publication «[attribution de périphérique discrète-Description et arrière-plan](https://blogs.technet.com/b/virtualization/archive/2015/11/19/discrete-device-assignment.aspx)» dans le blog sur la virtualisation.

### <a name="shielded-virtual-machines"></a>Machines virtuelles dotées d’une protection maximale

Ces machines virtuelles s’appuient sur la sécurité basée sur la virtualisation et sont disponibles à partir de Windows Server 2016.

La configuration requise pour l' **ordinateur hôte** est :

- UEFI 2.3.1 c-prend en charge le démarrage en toute sécurité et mesuré

  Les deux éléments suivants sont facultatifs pour la sécurité basée sur la virtualisation en général, mais requis pour l’hôte si vous souhaitez bénéficier de la protection fournie par ces fonctionnalités :

- TPM v 2.0-protège les ressources de sécurité de la plateforme
- IOMMU (Intel VT-D) : l’hyperviseur peut fournir une protection d’accès direct à la mémoire (DMA)

La configuration requise pour les **ordinateurs virtuels est la** suivante :

- 2e génération
- Windows Server 2012 ou une version plus récente que le système d’exploitation invité

