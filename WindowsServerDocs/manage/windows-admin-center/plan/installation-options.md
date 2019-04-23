---
title: Quel type d’installation vous convient
description: Quel type d’installation vous convient Windows Admin Center (projet Honolulu). Installer sur un cluster de basculement pour une haute disponibilité et la résilience.
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: fae0305e454cdd10109219c6182ff612f539e9c9
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59868010"
---
# <a name="what-type-of-installation-is-right-for-you"></a>Quel type d’installation vous convient ?

>S'applique à : Windows Admin Center, version préliminaire de Windows Admin Center

## <a name="supported-operating-systems-installation"></a>Systèmes d’exploitation pris en charge : Installation

Vous pouvez **installer** Windows Admin Center sur les systèmes d’exploitation Windows suivants :

| **Version** | **Mode d’installation** |
|-------------|-----------------------|
|Windows 10, version 1709 ou ultérieure | Mode bureau |
|Canal semi-annuel Windows Server | Mode passerelle |
|Windows Server 2016 | Mode passerelle |
|Windows Server 2019 | Mode passerelle |

**Mode bureau :** Lancez à partir du Menu Démarrer et se connecter à la passerelle Windows Admin Center depuis le même ordinateur que celui sur lequel il est installé (par exemple, `https://localhost:6516`)

**Mode de passerelle :** Se connecter à la passerelle Windows Admin Center à partir d’un navigateur client sur un autre ordinateur (par exemple, `https://servername.contoso.com`) 

> [!WARNING]
> Installation de Windows Admin Center sur un contrôleur de domaine n’est pas pris en charge. [Méthodes conseillées pour en savoir plus sur la sécurité de contrôleur de domaine](https://docs.microsoft.com/windows-server/identity/ad-ds/plan/security-best-practices/securing-domain-controllers-against-attack). 

> [!IMPORTANT]
> Vous ne pouvez pas utiliser Internet Explorer pour gérer Windows Admin Center : au lieu de cela, vous devez utiliser un [navigateur pris en charge](../understand/faq.md#which-web-browsers-are-supported-by-windows-admin-center
).  Si vous installez Windows Admin Center sur Windows Server, nous vous recommandons de gérer en vous connectant à distance avec Windows 10 et Edge.  Vous pouvez également gérer localement sur Windows Server si vous avez installé un navigateur pris en charge.

## <a name="supported-operating-systems-management"></a>Systèmes d’exploitation pris en charge : Gestion

Vous pouvez **gérer** systèmes d’exploitation Windows suivants à l’aide de Windows Admin Center :

| Version | Gérer *nœud* via *Gestionnaire de serveur* | Gérer *cluster* via *Gestionnaire du Cluster de basculement* | Gérer *HCL* via *Gestionnaire du Cluster de HCL*|
|-------------------------|---------------|-----|------------------------|
| Windows 10, version 1709 ou ultérieure | Oui (via la gestion de l’ordinateur) | N/A | N/A |
| Canal semi-annuel Windows Server | Oui | Oui | N/A |
| Windows Server 2019 | Oui | Oui | Oui |
| Windows Server 2016 | Oui | Oui | Oui, avec [dernière mise à jour cumulative](../use/manage-hyper-converged.md#prepare-your-windows-server-2016-cluster-for-windows-admin-center) |
| Microsoft Hyper-V Server 2016 | Oui | Oui | N/A |
| Windows Server 2012 R2 | Oui | Oui | N/A |
| Microsoft Hyper-V Server 2012 R2 | Oui | Oui | N/A |
| Windows Server 2012 | Oui | Oui | N/A |
| Windows Server 2008 R2 | Oui, des fonctionnalités limitées | N/A | N/A |

> [!NOTE]
> Windows Admin Center requiert des fonctionnalités de PowerShell qui ne sont pas incluses dans Windows Server 2008 R2, 2012 et 2012 R2. Si vous allez gérer avec Windows Admin Center, vous devrez installer Windows Management Framework (WMF) version 5.1 ou ultérieur sur ces serveurs.

>Saisissez `$PSVersiontable` dans PowerShell pour vérifier que WMF est installé, et que sa version est 5.1 ou ultérieure. 

>Si WMF n’est pas installé, vous pouvez [télécharger WMF 5.1](https://www.microsoft.com/en-us/download/details.aspx?id=54616).

## <a name="deployment-options"></a>Options de déploiement

| ![img](../media/deployment-options/W10.png) | ![img](../media/deployment-options/gateway.png) | ![img](../media/deployment-options/node.png) | ![img](../media/deployment-options/HA.png) |
|---|---|---|---|

| Client local | Serveur de passerelle | Serveur géré | Cluster de basculement |
| --- | --- | --- | --- |
| Installer sur un client Windows 10 local qui dispose d’une connectivité sur les serveurs gérés.  Idéal pour le Guide de démarrage rapide, de test, ad hoc ou les scénarios à petite échelle. |Installer sur un serveur de passerelle et accéder à partir de n’importe quel navigateur du client avec une connectivité au serveur de passerelle.  Idéal pour les scénarios à grande échelle. | Installer directement sur un serveur géré à des fins de gestion lui-même ou un cluster dans lequel il est un nœud membre.  Idéal pour les scénarios distribués. | Déployer dans un cluster de basculement pour activer la haute disponibilité du service de passerelle. Idéal pour les environnements de production garantir la résilience de votre service de gestion. |

## <a name="high-availability"></a>Haute disponibilité

Vous pouvez activer la haute disponibilité du service de passerelle en déployant Windows Admin Center dans un modèle actif / passif sur un cluster de basculement. Si un des nœuds du cluster échoue, Windows Admin Center normalement bascule vers un autre nœud, ce qui vous permet de continuer à gérer les serveurs dans votre environnement en toute transparence.

[Découvrez comment déployer Windows Admin Center avec une haute disponibilité.](../deploy/high-availability.md)

> [!Tip]
> Prêt à installer Windows Admin Center ? [Télécharger maintenant](https://aka.ms/windowsadmincenter)
