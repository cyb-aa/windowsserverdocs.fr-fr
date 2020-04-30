---
ms.assetid: 692bd2af-deee-44cf-9af9-f364677e267f
title: Planification de l'emplacement des contrôleurs de domaine
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: bb68a3551b362f0beb5d42a92a7c18d0913e47bc
ms.sourcegitcommit: 11421f4005f9f3a3f6c0db95b1836d0f765a9fa3
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2020
ms.locfileid: "81624087"
---
# <a name="planning-domain-controller-placement"></a>Planification de l'emplacement des contrôleurs de domaine

> S'applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Une fois que vous avez rassemblé toutes les informations réseau qui seront utilisées pour concevoir la topologie de votre site, planifiez l’emplacement où vous souhaitez placer les contrôleurs de domaine, y compris les contrôleurs de domaine racine de la forêt, les contrôleurs de domaine régionaux, les titulaires du rôle de maître d’opérations et les serveurs de catalogue global.

Dans Windows Server 2008, vous pouvez également tirer parti des contrôleurs de domaine en lecture seule (RODC). Un RODC est un nouveau type de contrôleur de domaine qui héberge des partitions en lecture seule de la base de données Active Directory. À l’exception des mots de passe de compte, un RODC contient tous les objets et attributs Active Directory qu’un contrôleur de domaine accessible en écriture détient. Toutefois, les modifications ne peuvent pas être apportées à la base de données stockée sur le contrôleur de domaine en lecture seule. Les modifications doivent être apportées sur un contrôleur de domaine accessible en écriture, puis répliquées sur le RODC.

Un contrôleur de domaine en lecture seule est conçu principalement pour être déployé dans des environnements distants ou des succursales, qui ont généralement relativement peu d’utilisateurs, une sécurité physique médiocre, une bande passante réseau relativement médiocre à un site Hub et le personnel ayant une connaissance limitée des technologies informatiques. Le déploiement de RODC produit une sécurité améliorée et un accès plus efficace aux ressources réseau. Pour plus d’informations sur les fonctionnalités RODC, consultez [AD DS : contrôleurs de domaine en lecture seule](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc732801(v=ws.10)). Pour plus d’informations sur le déploiement d’un contrôleur de [domaine en lecture seule, consultez le guide pas à pas des contrôleurs de domaine en lecture seule](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc772234(v=ws.10))

> [!NOTE]
> Ce guide n’explique pas comment déterminer le nombre approprié de contrôleurs de domaine et la configuration matérielle requise du contrôleur de domaine pour chaque domaine représenté dans chaque site.

## <a name="in-this-section"></a>Contenu de cette section

- [Planification de l’emplacement des contrôleurs de domaine racines de forêt](../../ad-ds/plan/Planning-Forest-Root-Domain-Controller-Placement.md)

- [Planification de l’emplacement des contrôleurs de domaine régionaux](../../ad-ds/plan/Planning-Regional-Domain-Controller-Placement.md)

- [Planification de l’emplacement des serveurs de catalogues globaux](../../ad-ds/plan/Planning-Global-Catalog-Server-Placement.md)

- [Planification de l’emplacement des rôles de maître d’opérations](../../ad-ds/plan/Planning-Operations-Master-Role-Placement.md)

