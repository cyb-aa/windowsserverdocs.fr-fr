---
ms.assetid: f052dfcd-dace-4485-8d0a-cc7df5cf3751
title: Vue d’ensemble des services de domaine Active Directory
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 84ce4986d27884f817eb5e632ac8dc1c5a22b922
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71390480"
---
# <a name="active-directory-domain-services-overview"></a>Vue d’ensemble des services de domaine Active Directory

>S'applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012


Un répertoire est une structure hiérarchique qui stocke des informations sur les objets sur le réseau. Un service d’annuaire, tel que Active Directory Domain Services (AD DS), fournit les méthodes permettant de stocker les données d’annuaire et de mettre ces données à la disposition des utilisateurs et administrateurs du réseau. Par exemple, AD DS stocke des informations sur les comptes d’utilisateur, telles que les noms, les mots de passe, les numéros de téléphone, etc., et permet à d’autres utilisateurs autorisés du même réseau d’accéder à ces informations.

Active Directory stocke des informations sur les objets sur le réseau et facilite la recherche et l’utilisation de ces informations pour les administrateurs et les utilisateurs. Active Directory utilise un magasin de données structuré comme base pour une organisation hiérarchique et logique des informations d’annuaire.

Ce magasin de données, également connu sous le nom d’annuaire, contient des informations sur les objets Active Directory. Ces objets incluent généralement des ressources partagées, telles que des serveurs, des volumes, des imprimantes et les comptes d’utilisateur et d’ordinateur réseau. Pour plus d’informations sur le magasin de données Active Directory, consultez la rubrique [magasin de données d’annuaire](https://technet.microsoft.com/library/cc736627(v=ws.10).aspx).

La sécurité est intégrée à Active Directory par le biais de l’authentification d’ouverture de session et du contrôle d’accès aux objets de l’annuaire. Avec une ouverture de session réseau unique, les administrateurs peuvent gérer les données et l’organisation de l’annuaire au sein de leur réseau, et les utilisateurs réseau autorisés peuvent accéder aux ressources n’importe où sur le réseau. L’administration basée sur des stratégies facilite même la gestion des réseaux les plus complexes. Pour plus d’informations sur la sécurité de Active Directory, consultez [vue d’ensemble](../../plan/security-best-practices/best-practices-for-securing-active-directory.md)de la sécurité.

Active Directory comprend également :
* Ensemble de règles, **le schéma**, qui définit les classes d’objets et d’attributs contenus dans l’annuaire, les contraintes et les limites sur les instances de ces objets, ainsi que le format de leurs noms. Pour plus d’informations sur le schéma, consultez schéma.


* Un **catalogue global** qui contient des informations sur chaque objet de l’annuaire. Cela permet aux utilisateurs et aux administrateurs de trouver des informations sur les annuaires, quel que soit le domaine de l’annuaire qui contient réellement les données. Pour plus d’informations sur le catalogue global, consultez le rôle du catalogue global.


* Un **mécanisme de requête et d’index**, afin que les objets et leurs propriétés puissent être publiés et recherchés par les utilisateurs ou les applications réseau. Pour plus d’informations sur l’interrogation de l’annuaire, consultez recherche d’informations d’annuaire.


* **Service de réplication** qui distribue les données d’annuaire sur un réseau. Tous les contrôleurs de domaine d’un domaine participent à la réplication et contiennent une copie complète de toutes les informations d’annuaire de leur domaine. Toute modification des données d’annuaire est répliquée sur tous les contrôleurs de domaine inclus dans le domaine. Pour plus d’informations sur la réplication de Active Directory, consultez vue d’ensemble de la réplication.

## <a name="understanding-active-directory"></a>Compréhension des Active Directory
 Cette section fournit des liens vers les principaux concepts de Active Directory :
 
* [Active Directory des technologies de stockage et de structure](https://technet.microsoft.com/library/cc759186(v=ws.10).aspx)
* [Rôles de contrôleur de domaine](https://technet.microsoft.com/library/cc786438(v=ws.10).aspx) 
* [Schéma Active Directory](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc771796(v=ws.10))
* [Comprendre les approbations](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc771568(v=ws.10)) 
* [Technologies de réplication Active Directory](https://technet.microsoft.com/library/cc786438(v=ws.10).aspx) 
* [Technologies de recherche et de publication Active Directory](https://technet.microsoft.com/library/cc775686(v=ws.10).aspx) 
* [Interopérabilité avec DNS et stratégie de groupe](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd197486(v=ws.10))
* [Fonctionnement du schéma](https://technet.microsoft.com/library/cc759402(v=ws.10).aspx) 

Pour obtenir une liste détaillée des concepts de Active Directory, voir [understanding Active Directory](https://technet.microsoft.com/library/cc781408(v=ws.10).aspx). 


