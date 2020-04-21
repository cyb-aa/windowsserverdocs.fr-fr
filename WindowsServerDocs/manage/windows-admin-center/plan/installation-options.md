---
title: Quel type d’installation vous convient ?
description: Cette rubrique décrit les différentes options d’installation de Windows Admin Center, notamment l’installation sur un PC Windows 10 ou un serveur Windows Server pour une utilisation par plusieurs administrateurs.
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.localizationpriority: medium
ms.prod: windows-server
ms.date: 12/02/2019
ms.openlocfilehash: bd7ec8a5a072cbda99b036718d24ec1908fb8b53
ms.sourcegitcommit: 20d07170c7f3094c2fb4455f54b13ec4b102f2d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/13/2020
ms.locfileid: "81269246"
---
# <a name="what-type-of-installation-is-right-for-you"></a>Quel type d’installation vous convient ?

Cette rubrique décrit les différentes options d’installation de Windows Admin Center, notamment l’installation sur un PC Windows 10 ou un serveur Windows Server pour une utilisation par plusieurs administrateurs. Pour installer Windows Admin Center sur une machine virtuelle dans Azure, consultez [Déployer Windows Admin Center dans Azure](../azure/deploy-wac-in-azure.md).

## <a name="installation-types"></a>Installation : Types

![img](../media/deployment-options/install-options.PNG)

| Client local                                | Serveur de passerelle                                  | Serveur managé                               | Cluster de basculement                           |
|---------------------------------------------|-------------------------------------------------|----------------------------------------------|--------------------------------------------|
| [Installer](../deploy/install.md) sur un client Windows 10 local qui dispose d’une connectivité aux serveurs managés.  Idéal pour les scénarios de démarrage rapide, de test, ad hoc ou à petite échelle. |[Installer](../deploy/install.md) sur un serveur de passerelle désigné, avec accès à partir de n’importe quel navigateur client disposant d’une connectivité au serveur de passerelle.  Idéal pour les scénarios à grande échelle. | [Installer](../deploy/install.md) directement sur un serveur managé dans le but de gérer ce même serveur ou un cluster dans lequel il est un nœud membre.  Idéal pour les scénarios distribués. | [Déployer](#high-availability) dans un cluster de basculement pour activer la haute disponibilité du service de passerelle. Idéal pour les environnements de production afin de garantir la résilience de votre service d’administration. |

## <a name="installation-supported-operating-systems"></a>Installation : Systèmes d'exploitation pris en charge

Vous pouvez **installer** Windows Admin Center sur les systèmes d’exploitation Windows suivants :

| **Plateforme**                       | **Mode d’installation** |
| -----------------------------------| --------------------- |
| Windows 10                         | Client local |
| Canal semi-annuel Windows Server | Serveur de passerelle, serveur managé, cluster de basculement |
| Windows Server 2016                | Serveur de passerelle, serveur managé, cluster de basculement |
| Windows Server 2019                | Serveur de passerelle, serveur managé, cluster de basculement |

Pour utiliser Windows Admin Center :

- **Dans un scénario de client local :** lancez la passerelle Windows Admin Center à partir du menu Démarrer et connectez-vous à partir d’un navigateur web client en accédant à `https://localhost:6516`.
- **Dans d’autres scénarios :** connectez-vous à la passerelle Windows Admin Center sur un autre ordinateur à partir d’un navigateur client par le biais de son URL, par exemple `https://servername.contoso.com`.

> [!WARNING]
> L’installation de Windows Admin Center sur un contrôleur de domaine n’est pas prise en charge. [Apprenez-en davantage sur les bonnes pratiques en matière de sécurité du contrôleur de domaine](https://docs.microsoft.com/windows-server/identity/ad-ds/plan/security-best-practices/securing-domain-controllers-against-attack).

## <a name="installation-supported-web-browsers"></a>Installation : navigateurs web pris en charge

Microsoft Edge (y compris [Microsoft Edge Insider](https://microsoftedgeinsider.com)) et Google Chrome sont testés et pris en charge sur Windows 10. D’autres navigateurs web, y compris Internet Explorer et Firefox, ne font pas actuellement partie de notre matrice de test et ne sont donc pas *officiellement* pris en charge. Ces navigateurs peuvent rencontrer des problèmes lors de l’exécution de Windows Admin Center. Par exemple, Firefox ayant son propre magasin de certificats, vous devez importer le certificat `Windows Admin Center Client` dans Firefox pour utiliser Windows Admin Center sur Windows 10. Pour plus d’informations, consultez [Problèmes connus propres au navigateur](../support/known-issues.md#browser-specific-issues).

## <a name="management-target-supported-operating-systems"></a>Cible d’administration : Systèmes d'exploitation pris en charge

Vous pouvez **administrer** les systèmes d’exploitation Windows suivants à l’aide de Windows Admin Center :

| Version | Gérer le *nœud* par le biais du *Gestionnaire de serveur* | Gérer par le biais du *Gestionnaire de cluster* |
| ------------------------- |--------------- | ----- |
| Windows 10 | Oui (par le biais de Gestion de l’ordinateur) | NON APPLICABLE |
| Canal semi-annuel Windows Server | Oui | Oui |
| Windows Server 2019 | Oui | Oui |
| Windows Server 2016 | Oui | Oui, avec [dernière mise à jour cumulative](../use/manage-hyper-converged.md#prepare-your-windows-server-2016-cluster-for-windows-admin-center) |
| Microsoft Hyper-V Server 2016 | Oui | Oui |
| Windows Server 2012 R2 | Oui | Oui |
| Microsoft Hyper-V Server 2012 R2 | Oui | Oui |
| Windows Server 2012 | Oui | Oui |

> [!NOTE]
> Windows Admin Center nécessite des fonctionnalités PowerShell qui ne sont pas incluses dans Windows Server 2012 et 2012 R2. Si vous comptez gérer ces systèmes avec Windows Admin Center, vous devrez installer WMF (Windows Management Framework) version 5.1 ou ultérieure sur les serveurs concernés.
> 
> Tapez `$PSVersiontable` dans PowerShell pour vérifier que WMF est installé, et que sa version est 5.1 ou ultérieure. 
> 
> S’il n’est pas installé, vous pouvez [télécharger WMF 5.1](https://www.microsoft.com/download/details.aspx?id=54616).

## <a name="high-availability"></a>Haute disponibilité

Vous pouvez activer la haute disponibilité du service de passerelle en déployant Windows Admin Center dans un modèle actif/passif sur un cluster de basculement. Si l’un des nœuds du cluster échoue, Windows Admin Center bascule normalement vers un autre nœud, ce qui vous permet de continuer à gérer les serveurs de votre environnement sans aucune interruption.

[Découvrez comment déployer Windows Admin Center avec une haute disponibilité.](../deploy/high-availability.md)

> [!Tip]
> Prêt à installer Windows Admin Center ? [Télécharger maintenant](https://aka.ms/windowsadmincenter)
