---
title: Group Managed Service Accounts Overview
description: Sécurité de Windows Server
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: security-gmsa
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: cef0693c-f861-48a7-a1c0-8d1bc06143ce
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: 24e3e3c15544de2f3bed4a7ef177b659e8095385
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59836970"
---
# <a name="group-managed-service-accounts-overview"></a>Group Managed Service Accounts Overview

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016

Cette rubrique destinées aux professionnels de l’informatique présente le compte de Service administré de groupe en décrivant des applications pratiques, les modifications apportées dans l’implémentation de Microsoft et exigences matérielles et logicielles.


## <a name="BKMK_OVER"></a>Description de la fonctionnalité
Un compte de Service administré (sMSA) autonome est un compte de domaine géré qui fournit la gestion des mots de passe automatique, gestion de nom principal (SPN) simplifiée de service et la possibilité de déléguer la gestion à d’autres administrateurs. Ce type de compte de service administré (MSA) a été introduit dans Windows Server 2008 R2 et Windows 7.

Le groupe compte de Service administré (gMSA) fournit les mêmes fonctionnalités au sein du domaine, mais également les étend sur plusieurs serveurs. Lorsque vous vous connectez à un service hébergé sur une batterie de serveurs, tels que de la solution d’équilibrage de charge de réseau, les protocoles d’authentification prenant en charge l’authentification mutuelle nécessitent que toutes les instances des services utilisent le même principal. Lorsqu’un compte gMSA est utilisé comme principaux de service, le système d’exploitation Windows gère le mot de passe pour le compte au lieu d’utiliser l’administrateur de gérer le mot de passe.

Le Service de Distribution de clés Microsoft \(kdssvc.dll\) fournit le mécanisme permettant d’obtenir en toute sécurité la dernière clé ou une clé spécifique avec un identificateur de clé pour un compte Active Directory. Le service de distribution de clés partage un secret qui sert à créer des clés pour le compte. Ces clés sont modifiées périodiquement. Pour un compte gMSA le contrôleur de domaine calcule le mot de passe sur la clé fournie par les Services de Distribution de clés, en plus des autres attributs de compte gMSA.  Hôtes membres peuvent obtenir les valeurs de mot de passe actuelles et précédentes en contactant un contrôleur de domaine.

## <a name="BKMK_APP"></a>Applications pratiques
comptes Gmsa fournissent une solution d’identité unique pour les services en cours d’exécution sur une batterie de serveurs ou sur des systèmes derrière l’équilibreur de charge réseau. En fournissant une solution de service administré de groupe, les services peuvent être configurés pour le nouveau service administré de groupe principal et la gestion de mot de passe est gérée par Windows.

À l’aide d’un compte gMSA, services ou les administrateurs de service n’avez pas besoin gérer la synchronisation de mot de passe entre instances de service. Le compte gMSA prend en charge les hôtes qui sont conservées hors connexion pendant une période de temps prolongée et la gestion des hôtes membres pour toutes les instances d’un service. Cela signifie que vous pouvez déployer une batterie de serveurs qui prend en charge une identité unique auprès de laquelle les ordinateurs clients existants peuvent s’authentifier sans savoir à quelle instance de service ils se connectent.

Les clusters de basculement ne prennent pas en charge les comptes de service administrés de groupe (gMSA, group Managed Service Account). Toutefois, les services qui s'exécutent sur le service de cluster peuvent utiliser un compte gMSA ou un compte de service administré autonome (sMSA, standalone Managed Service Account) s'il s'agit d'un service Windows, d'un pool d'applications, d'une tâche planifiée ou s'ils prennent nativement en charge les comptes gMSA ou sMSA.

## <a name="BKMK_SOFT"></a>Configuration logicielle requise

Un 64\-architecture bits est requis pour exécuter les commandes Windows PowerShell qui sont utilisés pour administrer les comptes Gmsa.

Un compte de service administré dépend de types de chiffrement pris en charge par Kerberos. Lorsqu’un ordinateur client s’authentifie auprès d’un serveur à l’aide du protocole Kerberos, le contrôleur de domaine crée un ticket de service Kerberos protégé avec le chiffrement pris en charge par le serveur et par le contrôleur de domaine. Le contrôleur de domaine utilise msDS du compte\-attribut SupportedEncryptionTypes pour déterminer ce qu’il prend en charge le chiffrement du serveur et, s’il n’existe aucun attribut, il part du principe que l’ordinateur client ne prend pas en charge les types de chiffrement renforcés. Si l’hôte est configuré pour ne prend ne pas en charge RC4, puis l’authentification échouera toujours. Pour cette raison, AES doit toujours être configuré de manière explicite pour les comptes de service administrés.

> [!NOTE]
> À compter de Windows Server 2008 R2, DES est désactivé par défaut. Pour plus d’informations sur les types de chiffrement pris en charge, voir [Modifications apportées à l’authentification Kerberos](https://technet.microsoft.com/library/dd560670(WS.10).aspx).

comptes Gmsa n’est pas applicables aux systèmes d’exploitation de Windows antérieures à Windows Server 2012.

## <a name="server-manager-information"></a>Informations sur le Gestionnaire de serveur
Aucune étape de configuration ne sont nécessaires pour implémenter les MSA et à l’aide du Gestionnaire de serveur ou de l’installation de service administré de groupe\-WindowsFeature applet de commande.

## <a name="BKMK_LINKS"></a>Voir aussi
Le tableau ci-dessous fournit des liens vers des ressources supplémentaires relatives aux comptes de service administrés et aux comptes de service administrés de groupe.

|Type de contenu|Références|
|--------|-------|
|**Évaluation du produit**|[Quelles sont les nouveautés pour les comptes de Service administrés](what-s-new-for-managed-service-accounts.md)<br /><br />[Documentation de Windows 7 et Windows Server 2008 R2 de comptes de Service administrés](https://technet.microsoft.com/library/ff641731(v=ws.10).aspx)<br /><br />[Étape de comptes de service\-par\-Guide pas](https://technet.microsoft.com/library/dd548356(v=ws.10).aspx)|
|**Planification**|Pas encore disponible|
|**Déploiement**|Pas encore disponible|
|**Opérations**|[Dans Active Directory, les comptes de Service administrés](https://technet.microsoft.com/library/dd378925(v=ws.10).aspx)|
|**Résolution des problèmes**|Pas encore disponible|
|**Evaluation**|[Mise en route avec le groupe de comptes de Service administrés](getting-started-with-group-managed-service-accounts.md)|
|**Outils et paramètres**|[Dans les Services de domaine Active Directory, les comptes de Service administrés](https://technet.microsoft.com/library/dd378925(v=WS.10).aspx)|
|**Ressources de la communauté**|[Comptes de Service administrés : Présentation, implémentation, recommandations et dépannage](http://blogs.technet.com/b/askds/archive/2009/09/10/managed-service-accounts-understanding-implementing-best-practices-and-troubleshooting.aspx)|
|**Technologies connexes**|[Vue d’ensemble des Services de domaine Active Directory](active-directory-domain-services-overview.md)|


