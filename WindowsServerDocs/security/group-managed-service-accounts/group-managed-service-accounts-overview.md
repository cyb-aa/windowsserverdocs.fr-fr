---
title: Group Managed Service Accounts Overview
description: Sécurité de Windows Server
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 924fcd8e0c981c9164c3026a58cbb41ef8c0085a
ms.sourcegitcommit: 083ff9bed4867604dfe1cb42914550da05093d25
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/14/2020
ms.locfileid: "75950351"
---
# <a name="group-managed-service-accounts-overview"></a>Group Managed Service Accounts Overview

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Cette rubrique destinée aux professionnels de l’informatique présente le compte de service administré de groupe en décrivant des applications pratiques, les modifications apportées à l’implémentation de Microsoft et la configuration matérielle et logicielle requise.


## <a name="BKMK_OVER"></a>Description de la fonctionnalité
Un compte de service administré autonome (sMSA) est un compte de domaine géré qui fournit une gestion automatique des mots de passe, une gestion simplifiée des noms de principal du service (SPN) et la possibilité de déléguer la gestion à d’autres administrateurs. Ce type de compte de service administré est apparu pour la première fois dans Windows Server 2008 R2 et Windows 7.

Le compte de service administré de groupe (gMSA) fournit les mêmes fonctionnalités dans le domaine, mais étend également cette fonctionnalité sur plusieurs serveurs. Lorsque vous vous connectez à un service hébergé sur une batterie de serveurs, telle que la solution d’équilibrage de la charge réseau, les protocoles d’authentification prenant en charge l’authentification mutuelle requièrent que toutes les instances des services utilisent le même principal. Quand un gMSA est utilisé en tant que principal du service, le système d’exploitation Windows gère le mot de passe du compte au lieu de s’appuyer sur l’administrateur pour gérer le mot de passe.

Le service de distribution de clés Microsoft \(kdssvc. dll\) fournit le mécanisme permettant d’obtenir en toute sécurité la dernière clé ou une clé spécifique avec un identificateur de clé pour un compte Active Directory. Le service de distribution de clés partage un secret qui sert à créer des clés pour le compte. Ces clés sont modifiées périodiquement. Pour un gMSA, le contrôleur de domaine calcule le mot de passe sur la clé fournie par les services de distribution de clés, en plus d’autres attributs du gMSA.  Les hôtes membres peuvent obtenir les valeurs de mot de passe actuelles et précédentes en contactant un contrôleur de domaine.

## <a name="BKMK_APP"></a>Applications pratiques
Service administrés fournissent une solution d’identité unique pour les services exécutés sur une batterie de serveurs, ou sur les systèmes se trouvant derrière le réseau Load Balancer. En fournissant une solution gMSA, les services peuvent être configurés pour le nouveau principal gMSA et la gestion des mots de passe est gérée par Windows.

Grâce aux gMSA, les services ou les administrateurs de services n’ont plus besoin de gérer la synchronisation des mots de passe entre les instances de service. Le gMSA prend en charge les hôtes qui sont conservés hors connexion pendant une période prolongée et la gestion des hôtes membres pour toutes les instances d’un service. Cela signifie que vous pouvez déployer une batterie de serveurs qui prend en charge une identité unique auprès de laquelle les ordinateurs clients existants peuvent s’authentifier sans savoir à quelle instance de service ils se connectent.

Les clusters de basculement ne prennent pas en charge les comptes de service administrés de groupe (gMSA, group Managed Service Account). Toutefois, les services qui s'exécutent sur le service de cluster peuvent utiliser un compte gMSA ou un compte de service administré autonome (sMSA, standalone Managed Service Account) s'il s'agit d'un service Windows, d'un pool d'applications, d'une tâche planifiée ou s'ils prennent nativement en charge les comptes gMSA ou sMSA.

## <a name="BKMK_SOFT"></a>Configuration logicielle requise

Une architecture de bits 64\-est requise pour exécuter les commandes Windows PowerShell utilisées pour administrer service administrés.

Un compte de service administré dépend de types de chiffrement pris en charge par Kerberos. Lorsqu’un ordinateur client s’authentifie auprès d’un serveur à l’aide du protocole Kerberos, le contrôleur de domaine crée un ticket de service Kerberos protégé avec le chiffrement pris en charge par le serveur et par le contrôleur de domaine. Le contrôleur de service utilise l’attribut msDS\-SupportedEncryptionTypes du compte pour déterminer le chiffrement pris en charge par le serveur et, s’il n’y a pas d’attribut, il suppose que l’ordinateur client ne prend pas en charge les types de chiffrement plus forts. Si l’hôte est configuré pour ne pas prendre en charge RC4, l’authentification échouera toujours. Pour cette raison, AES doit toujours être configuré de manière explicite pour les comptes de service administrés.

> [!NOTE]
> À compter de Windows Server 2008 R2, DES est désactivé par défaut. Pour plus d’informations sur les types de chiffrement pris en charge, voir [Modifications apportées à l’authentification Kerberos](https://technet.microsoft.com/library/dd560670(WS.10).aspx).

les service administrés ne s’appliquent pas aux systèmes d’exploitation Windows antérieurs à Windows Server 2012.

## <a name="server-manager-information"></a>Informations sur le Gestionnaire de serveur
Aucune étape de configuration n’est nécessaire pour implémenter MSA et gMSA à l’aide de Gestionnaire de serveur ou de l’applet de commande Install\-WindowsFeature.

## <a name="BKMK_LINKS"></a>Voir aussi
Le tableau ci-dessous fournit des liens vers des ressources supplémentaires relatives aux comptes de service administrés et aux comptes de service administrés de groupe.

|Type de contenu|Références|
|--------|-------|
|**Évaluation du produit**|[Nouveautés des comptes de service administrés](what-s-new-for-managed-service-accounts.md)<br /><br />[Documentation sur les comptes de service administrés pour Windows 7 et Windows Server 2008 R2](https://technet.microsoft.com/library/ff641731(v=ws.10).aspx)<br /><br />[Guide de l’étape\-des comptes de service par\-](https://technet.microsoft.com/library/dd548356(v=ws.10).aspx)|
|**Planification**|Pas encore disponible|
|**Déploiement**|Pas encore disponible|
|**Opérations**|[Comptes de service administrés dans Active Directory](https://technet.microsoft.com/library/dd378925(v=ws.10).aspx)|
|**Résolution des problèmes**|Pas encore disponible|
|**Évaluation**|[Prise en main des comptes de service administrés de groupe](getting-started-with-group-managed-service-accounts.md)|
|**Outils et paramètres**|[Comptes de service administrés dans Active Directory Domain Services](https://technet.microsoft.com/library/dd378925(v=WS.10).aspx)|
|**Ressources de la communauté**|[Comptes de service administrés : comprendre, implémenter, meilleures pratiques et résolution des problèmes](https://blogs.technet.com/b/askds/archive/2009/09/10/managed-service-accounts-understanding-implementing-best-practices-and-troubleshooting.aspx)|
|**Technologies connexes**|[Vue d’ensemble d’Active Directory Domain Services](active-directory-domain-services-overview.md)|


