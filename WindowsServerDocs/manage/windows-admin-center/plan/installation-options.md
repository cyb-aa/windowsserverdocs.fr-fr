---
title: Quel est le type d’installation qui vous convient
description: Cette rubrique décrit les différentes options d’installation du centre d’administration Windows, y compris l’installation de sur un PC Windows 10 ou un serveur Windows Server pour une utilisation par plusieurs administrateurs.
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.localizationpriority: medium
ms.prod: windows-server
ms.date: 12/02/2019
ms.openlocfilehash: 503cd64cac0673829fe21bc15e8ad9d6a83bbb15
ms.sourcegitcommit: 083ff9bed4867604dfe1cb42914550da05093d25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/14/2020
ms.locfileid: "75950514"
---
# <a name="what-type-of-installation-is-right-for-you"></a>Quel type d’installation vous convient ?

Cette rubrique décrit les différentes options d’installation du centre d’administration Windows, y compris l’installation de sur un PC Windows 10 ou un serveur Windows Server pour une utilisation par plusieurs administrateurs. Pour installer le centre d’administration Windows sur une machine virtuelle dans Azure, consultez [déployer le centre d’administration Windows dans Azure](../azure/deploy-wac-in-azure.md).

## <a name="installation-types"></a>Installation : types

![img](../media/deployment-options/install-options.PNG)

| Client local                                | Serveur de passerelle                                  | Serveur géré                               | Cluster de basculement                           |
|---------------------------------------------|-------------------------------------------------|----------------------------------------------|--------------------------------------------|
| [Installez](../deploy/install.md) sur un client Windows 10 local qui dispose d’une connectivité aux serveurs gérés.  Idéal pour les scénarios de démarrage rapide, de test, ad hoc ou à petite échelle. |[Installez](../deploy/install.md) sur un serveur de passerelle désigné et accédez à partir de n’importe quel navigateur client avec une connectivité au serveur de passerelle.  Idéal pour les scénarios à grande échelle. | [Installez](../deploy/install.md) directement sur un serveur géré dans le but de gérer lui-même ou un cluster dans lequel il est un nœud membre.  Idéal pour les scénarios distribués. | [Déployez](#high-availability) dans un cluster de basculement pour activer la haute disponibilité du service de passerelle. Idéal pour les environnements de production afin de garantir la résilience de votre service de gestion. |

## <a name="installation-supported-operating-systems"></a>Installation : systèmes d’exploitation pris en charge

Vous pouvez **installer** le centre d’administration Windows sur les systèmes d’exploitation Windows suivants :

| **Plateforme**                       | **Mode d’installation** |
| -----------------------------------| --------------------- |
| Windows 10                         | Client local |
| Canal semi-annuel Windows Server | Serveur de passerelle, serveur géré, cluster de basculement |
| Windows Server 2016                | Serveur de passerelle, serveur géré, cluster de basculement |
| Windows Server 2019                | Serveur de passerelle, serveur géré, cluster de basculement |

Pour faire fonctionner le centre d’administration Windows :

- **Dans le scénario client local :** Lancez la passerelle du centre d’administration Windows à partir du menu Démarrer et connectez-vous à partir d’un navigateur Web client en accédant à `https://localhost:6516`.
- **Dans d’autres scénarios :** Connectez-vous à la passerelle du centre d’administration Windows sur un autre ordinateur à partir d’un navigateur client via son URL, par exemple, `https://servername.contoso.com`

> [!WARNING]
> L’installation du centre d’administration Windows sur un contrôleur de domaine n’est pas prise en charge. [En savoir plus sur les meilleures pratiques en matière de sécurité du contrôleur de domaine](https://docs.microsoft.com/windows-server/identity/ad-ds/plan/security-best-practices/securing-domain-controllers-against-attack).

## <a name="installation-supported-web-browsers"></a>Installation : navigateurs Web pris en charge

Microsoft Edge (y compris [Microsoft Edge Insider](https://microsoftedgeinsider.com)) et Google Chrome sont testés et pris en charge sur Windows 10. D’autres navigateurs Web, y compris Internet Explorer et Firefox, ne font pas partie de notre matrice de test et ne sont donc pas *officiellement* pris en charge. Ces navigateurs peuvent rencontrer des problèmes lors de l’exécution du centre d’administration Windows. Par exemple, Firefox possède son propre magasin de certificats. vous devez donc importer le certificat de `Windows Admin Center Client` dans Firefox pour utiliser le centre d’administration Windows sur Windows 10. Pour plus d’informations, consultez [problèmes connus spécifiques au navigateur](../support/known-issues.md#browser-specific-issues).

## <a name="management-target-supported-operating-systems"></a>Cible de gestion : systèmes d’exploitation pris en charge

Vous pouvez **gérer** les systèmes d’exploitation Windows suivants à l’aide du centre d’administration Windows :

| Version | Gérer les *nœuds* via *Gestionnaire de serveur* | Gérer via le *Gestionnaire de cluster* |
| ------------------------- |--------------- | ----- |
| Windows 10 | Oui (via gestion de l’ordinateur) | NON APPLICABLE |
| Canal semi-annuel Windows Server | Oui | Oui |
| Windows Server 2019 | Oui | Oui |
| Windows Server 2016 | Oui | Oui, avec la [dernière mise à jour cumulative](../use/manage-hyper-converged.md#prepare-your-windows-server-2016-cluster-for-windows-admin-center) |
| Microsoft Hyper-V Server 2016 | Oui | Oui |
| R2 Windows Server 2012 | Oui | Oui |
| Microsoft Hyper-V Server 2012 R2 | Oui | Oui |
| Windows Server 2012 | Oui | Oui |
| Windows Server 2008 R2 | Oui, fonctionnalités limitées | NON APPLICABLE |

> [!NOTE]
> Le centre d’administration Windows requiert des fonctionnalités PowerShell qui ne sont pas incluses dans Windows Server 2008 R2, 2012 et 2012 R2. Si vous souhaitez les gérer à l’aide du centre d’administration Windows, vous devez installer Windows Management Framework (WMF) version 5,1 ou ultérieure sur ces serveurs.
> 
> Saisissez `$PSVersiontable` dans PowerShell pour vérifier que WMF est installé, et que sa version est 5.1 ou ultérieure. 
> 
> Si WMF n’est pas installé, vous pouvez [Télécharger wmf 5,1](https://www.microsoft.com/download/details.aspx?id=54616).

## <a name="high-availability"></a>Haute disponibilité

Vous pouvez activer la haute disponibilité du service de passerelle en déployant le centre d’administration Windows dans un modèle actif/passif sur un cluster de basculement. Si l’un des nœuds du cluster échoue, le centre d’administration Windows bascule normalement vers un autre nœud, ce qui vous permet de continuer à gérer les serveurs de votre environnement en toute transparence.

[Découvrez comment déployer le centre d’administration Windows avec une haute disponibilité.](../deploy/high-availability.md)

> [!Tip]
> Prêt à installer Windows Admin Center ? [Télécharger maintenant](https://aka.ms/windowsadmincenter)
