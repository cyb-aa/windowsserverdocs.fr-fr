---
title: RSC (Receive Segment Coalescing) dans le commutateur virtuel
description: La fusion de segment de réception (RSC) dans le vSwitch est une fonctionnalité de Windows Server 2019 et de la mise à jour 2018 d’octobre de Windows 10, qui permet de réduire l’utilisation du processeur de l’ordinateur hôte et augmente le débit des charges de travail virtuelles en fusionnant plusieurs segments TCP en un nombre inférieur, mais plus grand segments. Le traitement d’un nombre réduit de segments de grande taille (fusionné) est plus efficace que le traitement de nombreux petits segments.
manager: dougkim
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.author: dacuo
author: dcuomo
ms.date: 09/07/2018
ms.openlocfilehash: 0ffb417728bbdb73d8fb462ff7783b17b511bcd3
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80814772"
---
# <a name="rsc-in-the-vswitch"></a>RSC dans le vSwitch
>S’applique à : Windows Server 2019

La fusion de segment de réception (RSC) dans le vSwitch est une fonctionnalité de Windows Server 2019 et de la mise à jour 2018 d’octobre de Windows 10, qui permet de réduire l’utilisation du processeur de l’ordinateur hôte et augmente le débit des charges de travail virtuelles en fusionnant plusieurs segments TCP en un nombre inférieur, mais plus grand segments. Le traitement d’un nombre réduit de segments de grande taille (fusionné) est plus efficace que le traitement de nombreux petits segments.

Windows Server 2012 et versions ultérieures comprenaient une version de déchargement matérielle uniquement (implémentée dans la carte réseau physique) de la technologie également connue sous le nom de fusion de segment de réception. Cette version déchargée de RSC est toujours disponible dans les versions ultérieures de Windows. Toutefois, il est incompatible avec les charges de travail virtuelles et a été désactivé une fois qu’une carte réseau physique est attachée à un vSwitch. Pour plus d’informations sur la version matérielle uniquement de RSC, consultez [réception de la fusion de segments (RSC)](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh997024(v=ws.11)).

## <a name="scenarios-that-benefit-from-rsc-in-the-vswitch"></a>Scénarios qui tirent parti de RSC dans le vSwitch

Les charges de travail dont chemin traverse un commutateur virtuel tirent parti de cette fonctionnalité.

Par exemple :

-   Héberger des cartes réseau virtuelles, notamment :

    -   Mise en réseau SDN (Software Defined Networking)

    -   Ordinateur hôte Hyper-V

    -   Espaces de stockage directs

-   Cartes réseau virtuelles invitées Hyper-V

-   Passerelles GRE de mise en réseau à définition logicielle

-   Conteneur

Les charges de travail qui ne sont pas compatibles avec cette fonctionnalité sont les suivantes :

-   Passerelles IPSEC de mise en réseau définie par logiciel

-   Cartes réseau virtuelles SR-IOV activées

-   SMB Direct

## <a name="configure-rsc-in-the-vswitch"></a>Configurer RSC dans le vSwitch


Par défaut, sur les commutateurs virtuels externes, RSC est activé.

**Affichez les paramètres actuels :**

```PowerShell
Get-VMSwitch -Name vSwitchName | Select-Object *RSC*
```

**Activer ou désactiver RSC dans le vSwitch**


>[!IMPORTANT]
>Important : la RSC dans le vSwitch peut être activée et désactivée à la volée sans impact sur les connexions existantes.


**Désactiver RSC dans le vSwitch**

```PowerShell
Set-VMSwitch -Name vSwitchName -EnableSoftwareRsc $false
```

**Réactivez RSC dans le vSwitch**

```PowerShell
Set-VMSwitch -Name vSwitchName -EnableSoftwareRsc $True
```
Pour plus d’informations, consultez [Set-VMSwitch](https://docs.microsoft.com/powershell/module/hyper-v/set-vmswitch?view=win10-ps).
