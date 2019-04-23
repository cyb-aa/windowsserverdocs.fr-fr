---
title: RSC (Receive Segment Coalescing) dans le commutateur virtuel
description: Réception Segment fusion (RSC) dans le commutateur virtuel est une fonctionnalité de Windows Server 2019 et mettre à jour du 10 octobre 2018 Windows est que vous aide à réduire l’utilisation du processeur d’ordinateur hôte et augmente le débit pour les charges de travail virtuelles par fusion de plusieurs segments TCP en moins, mais plus grand segments. Traitement des segments de moins de grande taille (fusionnés) est plus efficace que le traitement nombreux, petits segments.
manager: dougkim
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: ''
ms.author: dacuo
author: shortpatti
ms.date: 09/07/2018
ms.openlocfilehash: 667e795e398443cadd4c966cc31e65eeee4962f7
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59827780"
---
# <a name="rsc-in-the-vswitch"></a>RSC dans le commutateur virtuel
>S’applique à : Windows Server 2019

Réception Segment fusion (RSC) dans le commutateur virtuel est une fonctionnalité de Windows Server 2019 et mettre à jour du 10 octobre 2018 Windows est que vous aide à réduire l’utilisation du processeur d’ordinateur hôte et augmente le débit pour les charges de travail virtuelles par fusion de plusieurs segments TCP en moins, mais plus grand segments. Traitement des segments de moins de grande taille (fusionnés) est plus efficace que le traitement nombreux, petits segments.

Windows Server 2012 et versions ultérieur comprenait une version de déchargement de matériel uniquement (implémentée dans la carte réseau physique) de la technologie, également appelée de réception de la fusion du Segment. Cette version déchargée de RSC est toujours disponible dans les versions ultérieures de Windows. Toutefois, il n’est pas compatible avec les charges de travail virtuelles et a été désactivée une fois qu’une carte réseau physique est attachée à un commutateur virtuel. Pour plus d’informations sur la version du matériel uniquement de RSC, consultez [fusion de Segment de réception (RSC)](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh997024(v=ws.11)).

## <a name="scenarios-that-benefit-from-rsc-in-the-vswitch"></a>Scénarios qui bénéficient de RSC dans le commutateur virtuel

Charges de travail dont datapath traverse un commutateur virtuel tire parti de cette fonctionnalité.

Exemple :

-   Hôte virtuel cartes d’interface réseau, notamment :

    -   Mise en réseau SDN (Software Defined Networking)

    -   Ordinateur hôte Hyper-V

    -   Espaces de stockage directs

-   Cartes réseau virtuelles invité Hyper-V

-   Passerelles GRE de mise en réseau à définition logicielle

-   Conteneur

Charges de travail qui ne sont pas compatibles avec cette fonctionnalité sont les suivantes :

-   Passerelles IPSEC de mise en réseau à définition logicielle

-   SR-IOV activé des cartes réseau virtuelles

-   SMB Direct

## <a name="configure-rsc-in-the-vswitch"></a>Configurer RSC dans le commutateur virtuel


Par défaut, sur les commutateurs virtuels externes, RSC est activé.

**Afficher les paramètres actuels :**

```PowerShell
Get-VMSwitch -Name vSwitchName | Select-Object *RSC*
```

**Activer ou désactiver les RSC dans le commutateur virtuel**


>[!IMPORTANT]
>Important : RSC dans le commutateur virtuel peut être activé et désactivé à la volée sans impact sur les connexions existantes.


**Désactiver RSC dans le commutateur virtuel**

```PowerShell
Set-VMSwitch -Name vSwitchName -EnableSoftwareRsc $false
```

**Réactiver RSC dans le commutateur virtuel**

```PowerShell
Set-VMSwitch -Name vSwitchName -EnableSoftwareRsc $True
```
Pour plus d’informations, consultez [Set-VMSwitch](https://docs.microsoft.com/powershell/module/hyper-v/set-vmswitch?view=win10-ps).
