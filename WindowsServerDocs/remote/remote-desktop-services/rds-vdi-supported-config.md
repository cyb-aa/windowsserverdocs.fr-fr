---
title: Configurations de sécurité Windows 10 prises en charge pour l’infrastructure VDI des services Bureau à distance
description: Fournit des informations sur les configurations prises en charge pour l’infrastructure VDI de Windows 10, avec les services Bureau à distance dans Windows Server 2016.
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.author: elizapo
ms.date: 10/27/2016
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 8f164f5d-a498-4f91-a12f-3e01d554f810
author: lizap
manager: dongill
ms.openlocfilehash: 08941c49469dcf9b9e3e42c7ab799186380bab35
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71387029"
---
# <a name="supported-windows-10-security-configurations-for-remote-desktop-services-vdi"></a>Configurations de sécurité Windows 10 prises en charge pour l’infrastructure VDI des services Bureau à distance

> S'applique à : Windows Server 2016

Windows 10 et Windows Server 2016 fournissent de nouvelles couches de protection intégrées au système d’exploitation pour davantage renforcer la protection vis-à-vis des violations de la sécurité, bloquer plus efficacement les attaques malveillantes et améliorer la sécurité des machines virtuelles, des applications et des données.

> [!NOTE]
> Veillez à consulter l’article [Informations sur les configurations prises en charge pour les services Bureau à distance](rds-supported-config.md).

Le tableau suivant met en évidence les nouvelles fonctionnalités qui sont prises en charge dans un déploiement VDI avec les services Bureau à distance.

|  Type de collection VDI               |  Managée mise en pool |  Managée personnelle |  Non managée mise en pool                                     |  Non managée personnelle                                    |
|-------------------------------------|------------------|--------------------|--------------------------------------------------------|--------------------------------------------------------|
| [Credential Guard](https://technet.microsoft.com/itpro/windows/keep-secure/credential-guard)                    | Oui              | Oui                | Oui                                                    | Oui                                                    |
| [Device Guard](https://technet.microsoft.com/itpro/windows/keep-secure/device-guard-deployment-guide)                        | Oui              | Oui                | Oui                                                    | Oui                                                    |
| [Credential Guard à distance](https://technet.microsoft.com/itpro/windows/keep-secure/remote-credential-guard)             | Non               | Non                 | Non                                                     | Non                                                     |
| [Machines virtuelles dotées d’une protection maximale et d’un chiffrement](../../security/guarded-fabric-shielded-vm/guarded-fabric-and-shielded-vms.md) | Non               | Non                 | Machines virtuelles dotées d’un chiffrement avec configuration supplémentaire | Machines virtuelles dotées d’un chiffrement avec configuration supplémentaire |

## <a name="remote-credential-guard"></a>Credential Guard à distance :

La fonctionnalité Credential Guard à distance est uniquement prise en charge pour les connexions directes aux machines cibles, et non pour celles qui transitent par Service Broker pour les connexions Bureau à distance ou via la passerelle Bureau à distance.
> [!NOTE]
> Si vous disposez d’un service Broker pour les connexions dans un environnement mono-instance, et que le nom DNS correspond au nom de l’ordinateur, vous pouvez peut-être utiliser la fonctionnalité Credential Guard à distance, même si elle n’est pas prise en charge.

## <a name="shielded-vms-and-encryption-supported-vms"></a>Machines virtuelles dotées d’une protection maximale et machines virtuelles pourvues d’un chiffrement : 

- Les machines virtuelles dotées d’une protection maximale ne sont pas prises en charge dans l’infrastructure VDI des services Bureau à distance 

Pour tirer parti des machines virtuelles dotées d’un chiffrement :
- Utilisez une collection non managée et une technologie de provisionnement en dehors du processus de création de la collection Services Bureau à distance pour provisionner les machines virtuelles. 
- Les disques de profil utilisateur ne sont pas pris en charge, car ils s’appuient sur des disques différentiels. 

