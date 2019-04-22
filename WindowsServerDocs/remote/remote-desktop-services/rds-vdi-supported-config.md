---
title: Configurations de sécurité Windows 10 prises en charge pour l’infrastructure VDI des Services Bureau à distance
description: Fournit des informations sur les configurations prises en charge pour Windows 10 VDI avec les services Bureau à distance dans Windows Server 2016.
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: ff890150dcea30c425267dcaae9b1bdbc6d78b8c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59820080"
---
# <a name="supported-windows-10-security-configurations-for-remote-desktop-services-vdi"></a>Configurations de sécurité Windows 10 prises en charge pour l’infrastructure VDI des Services Bureau à distance

> S'applique à : Windows Server 2016

Windows 10 et Windows Server 2016 ont de nouvelles couches de protection intégrées au système d’exploitation pour garantir une protection contre les failles de sécurité davantage pour bloquer les attaques malveillantes et améliorer la sécurité des machines virtuelles, applications et données.

> [!NOTE]
> Passez en revue la [Services Bureau à distance pris en charge les informations de configuration](rds-supported-config.md).

Le tableau suivant indique laquelle de ces nouvelles fonctionnalités est prises en charge dans un déploiement VDI à l’aide de RDS.

|  Type de collection de VDI               |  Gérés mis en pool |  Gérés personnel |  Non gérés mis en pool                                     |  Personnel non managé                                    |
|-------------------------------------|------------------|--------------------|--------------------------------------------------------|--------------------------------------------------------|
| [Credential Guard](https://technet.microsoft.com/itpro/windows/keep-secure/credential-guard)                    | Oui              | Oui                | Oui                                                    | Oui                                                    |
| [Device Guard](https://technet.microsoft.com/itpro/windows/keep-secure/device-guard-deployment-guide)                        | Oui              | Oui                | Oui                                                    | Oui                                                    |
| [Credential Guard à distance](https://technet.microsoft.com/itpro/windows/keep-secure/remote-credential-guard)             | Non               | Non                 | Non                                                     | Non                                                     |
| [Protégées & chiffrement pris en charge les machines virtuelles](../../security/guarded-fabric-shielded-vm/guarded-fabric-and-shielded-vms.md) | Non               | Non                 | Chiffrement pris en charge les machines virtuelles avec une configuration supplémentaire | Chiffrement pris en charge les machines virtuelles avec une configuration supplémentaire |

## <a name="remote-credential-guard"></a>Credential Guard à distance :

À distance de Credential Guard est uniquement compatible avec les connexions directes vers les ordinateurs cibles et pas pour celles via Service Broker pour les connexions Bureau à distance et passerelle Bureau à distance.
> [!NOTE]
> Si vous avez un agent de connexion dans un environnement à instance unique, et le nom DNS correspond au nom de l’ordinateur, vous pourrez peut-être utiliser de Credential Guard à distance, même si cela n’est pas pris en charge.

## <a name="shielded-vms-and-encryption-supported-vms"></a>Chiffrement et dotées d’une prise en charge des machines virtuelles : 

- Machines virtuelles protégées ne sont pas pris en charge dans l’infrastructure VDI des Services Bureau à distance 

Pour tirer parti de chiffrement pris en charge de machines virtuelles :
- Utiliser une collection non gérés et une technologie d’approvisionnement en dehors du processus de création de collection de Services Bureau à distance pour approvisionner les machines virtuelles. 
- Disques de profil utilisateur ne sont pas pris en charge car ils s’appuient sur les disques différentiels 

