---
ms.assetid: f052dfcd-dace-4485-8d0a-cc7df5cf3751
title: Vue d’ensemble des services de domaine Active Directory
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 069cdb493cd0ad442e8922ec67c2b9cc6b2ec5fc
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59858170"
---
# <a name="active-directory-domain-services-overview"></a>Vue d’ensemble des services de domaine Active Directory

>S'applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012


Un répertoire est une structure hiérarchique qui stocke des informations sur les objets sur le réseau. Un service d’annuaire, telles que les Services de domaine Active Directory (AD DS), fournit les méthodes pour le stockage des données d’annuaire et rendre ces données disponibles pour les administrateurs et les utilisateurs du réseau. Par exemple, les services AD DS stocke des informations sur les comptes d’utilisateur, telles que les noms, les mots de passe, les numéros de téléphone et ainsi de suite et permet aux autres utilisateurs autorisés sur le même réseau accéder à ces informations.

Active Directory stocke des informations sur les objets sur le réseau et facilite ces informations pour les administrateurs et les utilisateurs à trouver et à utiliser. Active Directory utilise un magasin de données structurées comme base pour une organisation logique et hiérarchique des informations d’annuaire.

Ce magasin de données, également connu sous le répertoire, contient des informations sur les objets Active Directory. Ces objets incluent généralement des ressources partagées telles que des serveurs, des volumes, des imprimantes et des comptes d’utilisateur et d’ordinateur réseau. Pour plus d’informations sur le magasin de données Active Directory, consultez [magasin de données Directory](https://technet.microsoft.com/library/cc736627(v=ws.10).aspx).

La sécurité est intégrée à Active Directory via l’authentification de connexion et de contrôle d’accès aux objets dans l’annuaire. Avec une ouverture de session réseau unique, les administrateurs peuvent gérer les données d’annuaire et de l’organisation tout au long de leur réseau, et les utilisateurs autorisés peuvent accéder aux ressources n’importe où sur le réseau. L’administration basée sur des stratégies facilite même la gestion des réseaux les plus complexes. Pour plus d’informations sur la sécurité d’Active Directory, consultez [vue d’ensemble de la sécurité](../../plan/security-best-practices/best-practices-for-securing-active-directory.md).

Active Directory comprend également :
* Un ensemble de règles, **le schéma**, qui définit les classes d’objets et attributs contenus dans le répertoire, les contraintes et les limites sur les instances de ces objets et le format de leurs noms. Pour plus d’informations sur le schéma, consultez le schéma.


* Un **catalogue global** qui contient des informations sur chaque objet dans l’annuaire. Cela permet aux utilisateurs et administrateurs pour rechercher des informations de répertoire, quel que soit le domaine dans le répertoire qui contient les données. Pour plus d’informations sur le catalogue global, consultez le rôle du catalogue global.


* Un **mécanisme de requête et des index**, de sorte que les objets et leurs propriétés peuvent être publiées et trouvées par les utilisateurs du réseau ou des applications. Pour plus d’informations sur l’interrogation de l’annuaire, consultez Recherche d’informations d’annuaire.


* Un **service de réplication** qui distribue les données d’annuaire sur un réseau. Tous les contrôleurs de domaine dans un domaine participent à la réplication et contiennent une copie complète de toutes les informations d’annuaire de leur domaine. Toute modification des données d’annuaire est répliquée sur tous les contrôleurs de domaine inclus dans le domaine. Pour plus d’informations sur la réplication Active Directory, consultez Vue d’ensemble de la réplication.

## <a name="understanding-active-directory"></a>Présentation d’Active Directory
 Cette section fournit des liens vers les principaux concepts de Active Directory :
 
* [Structure Active Directory et les Technologies de stockage](https://technet.microsoft.com/library/cc759186(v=ws.10).aspx)
* [Rôles de contrôleur de domaines](https://technet.microsoft.com/library/cc786438(v=ws.10).aspx) 
* Schéma Active Directory 
* [Présentation des approbations](https://technet.microsoft.com/library/cc771294(v=ws.10).aspx) 
* [Technologies de réplication Active Directory](https://technet.microsoft.com/library/cc786438(v=ws.10).aspx) 
* [Recherche dans Active Directory et les Technologies de Publication](https://technet.microsoft.com/library/cc775686(v=ws.10).aspx) 
* Interopérabilité avec DNS et stratégie de groupe 
* [Présentation du schéma](https://technet.microsoft.com/library/cc759402(v=ws.10).aspx) 

Pour obtenir une liste détaillée des concepts Active Directory, consultez [comprendre Active Directory](https://technet.microsoft.com/library/cc781408(v=ws.10).aspx). 


