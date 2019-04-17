---
title: Quel type d’installation vous convient
description: Cette rubrique décrit les différentes options d’installation pour Windows Admin Center, notamment sur l’installation sur un PC Windows 10 ou sur un serveur Windows pour une utilisation par les administrateurs plusieurs.
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.date: 04/12/2019
ms.openlocfilehash: cc6f9e6c2f2572618c1c5fdae00047d24fbeb3cf
ms.sourcegitcommit: f1edfc6525e09dd116b106293f9260123a94de0c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/12/2019
ms.locfileid: "9296661"
---
# Quel type d’installation vous convient?

>S’applique à: Windows Admin Center, Windows Admin Center Preview

Cette rubrique décrit les différentes options d’installation pour Windows Admin Center, notamment sur l’installation sur un PC Windows 10 ou sur un serveur Windows pour une utilisation par les administrateurs plusieurs. Pour installer Windows Admin Center sur un ordinateur virtuel dans Azure, voir [Déployer Windows Admin Center dans Azure](../azure/deploy-wac-in-azure.md).

## Systèmes d’exploitation pris en charge: Installation

Vous pouvez **installer** Windows Admin Center sur les systèmes d’exploitation Windows suivants:

| **Version** | **Mode d’installation** |
|-------------|-----------------------|
|Windows 10, version 1709 ou ultérieure | Mode bureau |
|Canal semi-annuel WindowsServer | Mode passerelle |
|WindowsServer2016 | Mode passerelle |
|Windows Server2019 | Mode passerelle |

**Mode bureau:** Lancer à partir du Menu Démarrer et de se connecter à la passerelle Windows Admin Center à partir de l’ordinateur sur lequel il est installé (autrement dit, `https://localhost:6516`)

**Mode passerelle:** Se connecter à la passerelle Windows Admin Center à partir d’un navigateur client sur un autre ordinateur (autrement dit, `https://servername.contoso.com`) 

> [!WARNING]
> L’installation de Windows Admin Center sur un contrôleur de domaine n’est pas pris en charge. [En savoir plus sur les meilleures pratiques de la sécurité de contrôleur domaine](https://docs.microsoft.com/windows-server/identity/ad-ds/plan/security-best-practices/securing-domain-controllers-against-attack). 

> [!IMPORTANT]
> Vous ne pouvez pas utiliser Internet Explorer pour gérer Windows Admin Center: au lieu de cela, vous devez utiliser un [navigateur pris en charge](../understand/faq.md#which-web-browsers-are-supported-by-windows-admin-center
).  Si vous installez Windows Admin Center sur Windows Server, nous vous recommandons de la gestion en vous connectant à distance Windows 10 et Edge.  Par ailleurs, vous pouvez gérer localement sur Windows Server si vous avez installé un navigateur pris en charge.

## Systèmes d’exploitation pris en charge: gestion

Vous pouvez **Gérer** les systèmes d’exploitation Windows suivants à l’aide de Windows Admin Center:

| Version | Gérer le *nœud* via *Le Gestionnaire de serveur* | Gérer les *clusters* via le *Gestionnaire du Cluster de basculement* | Gérer les *HCI* via *Le Gestionnaire du Cluster HCI*|
|-------------------------|---------------|-----|------------------------|
| Windows 10, version 1709 ou ultérieure | Oui (via la gestion de l’ordinateur) | N/A | N/A |
| Canal semi-annuel WindowsServer | Oui | Oui | N/A |
| Windows Server2019 | Oui | Oui | Oui |
| WindowsServer2016 | Oui | Oui | Oui, avec la [mise à jour cumulative plus récentes](../use/manage-hyper-converged.md#prepare-your-windows-server-2016-cluster-for-windows-admin-center) |
| Microsoft Hyper-V Server2016 | Oui | Oui | N/A |
| WindowsServer2012R2 | Oui | Oui | N/A |
| Microsoft Hyper-V Server2012R2 | Oui | Oui | N/A |
| WindowsServer2012 | Oui | Oui | N/A |
| Windows Server2008R2 | Oui, des fonctionnalités limitées | N/A | N/A |

> [!NOTE]
> Windows Admin Center requiert des fonctionnalités de PowerShell qui ne sont pas incluses dans Windows Server 2008 R2, 2012 et 2012 R2. Si vous prévoyez de gérer ces avec Windows Admin Center, vous devez installer Windows Management Framework (WMF) version 5.1 ou ultérieure sur ces serveurs.

>Saisissez `$PSVersiontable` dans PowerShell pour vérifier que WMF est installé et que sa version est5.1 ou ultérieure. 

>Si WMF n’est pas installé, vous pouvez [Télécharger WMF 5.1](https://www.microsoft.com/en-us/download/details.aspx?id=54616).

## Options de déploiement

| ![IMG](../media/deployment-options/W10.png) | ![IMG](../media/deployment-options/gateway.png) | ![IMG](../media/deployment-options/node.png) | ![IMG](../media/deployment-options/HA.png) |
|---|---|---|---|

| Client local | Serveur de passerelle | Serveur géré | Cluster de basculement |
| --- | --- | --- | --- |
| Installer sur un client Windows 10 local qui dispose d’une connectivité sur les serveurs gérés.  Idéal pour le démarrage rapide, test, ad hoc ou des scénarios de petite échelle. |Installer sur un serveur de passerelle désigné et l’accès à partir de n’importe quel navigateur client disposant d’une connexion au serveur de passerelle.  Idéal pour les scénarios à grande échelle. | Installer directement sur un serveur géré dans le cadre de la gestion de lui-même ou un cluster dans lequel il s’agit d’un nœud membre.  Idéal pour les scénarios distribués. | Déployer dans un cluster de basculement pour activer la haute disponibilité du service de passerelle. Idéal pour les environnements de production garantir la résilience de votre service de gestion. |

## Haute disponibilité

Vous pouvez activer la haute disponibilité du service de passerelle en déployant Windows Admin Center dans un modèle actif / passif sur un cluster de basculement. Si un des nœuds du cluster échoue, Windows Admin Center normalement bascule vers un autre nœud, ce qui vous permet de continuer à gérer les serveurs dans votre environnement en toute transparence.

[Découvrez comment déployer Windows Admin Center à haute disponibilité.](../deploy/high-availability.md)

> [!Tip]
> Prêt à installer Windows Admin Center? [Télécharger maintenant](https://aka.ms/windowsadmincenter)
