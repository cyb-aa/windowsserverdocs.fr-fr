---
title: Vue d’ensemble des comptes de Service administrés de groupe
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
ms.openlocfilehash: 4912ae273e603b4a3362c4984da710f780c3e8b3
ms.sourcegitcommit: f26d2668f57624a3865ca4ffd36a698eea7b503e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/12/2018
---
# <a name="group-managed-service-accounts-overview"></a>Vue d’ensemble des comptes de Service administrés de groupe

>S’applique à: Windows Server (canal annuel un point-virgule), Windows Server2016

Cette rubrique pour les professionnels de l’informatique présente le compte de Service administré de groupe en décrivant des applications pratiques, des modifications dans l’implémentation de Microsoft et la configuration matérielle et logicielle.


## <a name="BKMK_OVER"></a>Description de la fonctionnalité
Autonome comptes de Service administrés, qui ont été introduits dans Windows Server 2008 R2 et Windows 7, sont des comptes de domaine administrés qui fournissent une gestion automatique de mot de passe et gestion simplifiée des SPN, y compris la délégation de la gestion à d’autres administrateurs.

Le groupe compte de Service administré offre les mêmes fonctionnalités au sein du domaine, mais également les étend sur plusieurs serveurs. Lors de la connexion à un service hébergé sur une batterie de serveurs, par exemple, l’équilibrage de la charge réseau, les protocoles d’authentification prenant en charge l’authentification mutuelle nécessitent que toutes les instances des services utilisent le même principal. Lorsque le compte de Service administré de groupe sont utilisés en tant qu’entités de service, le système d’exploitation Windows gère le mot de passe du compte au lieu d’appuyer sur l’administrateur de gérer le mot de passe.

Le Service de Distribution de clés Microsoft \(kdssvc.dll\) fournit le mécanisme nécessaire pour obtenir en toute sécurité la dernière clé ou une clé spécifique avec un identificateur de clé pour un compte Active Directory. Le Service de Distribution de clés partage un secret qui sert à créer des clés pour le compte. Ces clés sont modifiées périodiquement. Pour un compte de Service administré de groupe du contrôleur de domaine calcule le mot de passe sur la clé fournie par les Services de Distribution de clés, en outre, vers d’autres attributs du compte de Service administré de groupe.  Hôtes membres peuvent obtenir les valeurs de mot de passe actuelles et précédentes en contactant un contrôleur de domaine.

## <a name="BKMK_APP"></a>Applications pratiques
Comptes de Service administrés groupe fournissent une solution d’identité unique pour les services en cours d’exécution sur une batterie de serveurs ou sur des systèmes derrière l’équilibrage de la charge réseau. En fournissant une solution de compte de service administré de groupe, services peuvent être configurés pour le nouveau compte principal de service administré de groupe et la gestion des mots de passe est gérée par Windows.

À l’aide d’un groupe compte de Service administré, services ou les administrateurs de service n’avez pas besoin de gérer la synchronisation de mot de passe entre les instances de service. Le groupe compte de Service administré prend en charge les ordinateurs hôtes qui sont maintenus hors connexion pour une période prolongée et la gestion des hôtes membres pour toutes les instances d’un service. Cela signifie que vous pouvez déployer une batterie de serveurs qui prend en charge une identité unique à laquelle les ordinateurs clients existants peuvent s’authentifier sans savoir à l’instance du service à laquelle ils se connectent.

Clusters de basculement ne prennent pas en charge les comptes Gmsa. Toutefois, les services qui s’exécutent sur le service de Cluster peuvent utiliser un compte gMSA ou un sMSA s’ils sont un service Windows, un pool d’application, une tâche planifiée ou en mode natif prend en charge des comptes gMSA ou sMSA.

## <a name="BKMK_SOFT"></a>Configuration logicielle requise

Une architecture 64 bits est requise pour exécuter les commandes Windows PowerShell qui sont utilisées pour administrer les comptes de Service administrés groupe.

Un compte de service administré dépend des types de chiffrement Kerberos pris en charge. Lorsqu’un ordinateur client s’authentifie auprès d’un serveur à l’aide de Kerberos, le contrôleur de domaine crée un ticket de service Kerberos protégé avec le chiffrement à la fois le contrôleur de domaine et le serveur prend en charge. Le contrôleur de domaine utilise l’attribut de msDS\-SupportedEncryptionTypes du compte pour déterminer ce que le serveur de chiffrement prend en charge et, si aucun attribut n’est activée, il suppose que l’ordinateur client ne prend pas en charge les types de chiffrement renforcés. Si l’ordinateur hôte est configuré pour ne prend ne pas en charge RC4, puis l’authentification échouera toujours. Pour cette raison, AES doit toujours être configurées de façon explicite pour les comptes de service administrés.

> [!NOTE]
> À compter de Windows Server2008R2, DES est désactivé par défaut. Pour plus d’informations sur les types de chiffrement pris en charge, voir [les modifications apportées à l’authentification Kerberos](https://technet.microsoft.com/library/dd560670(WS.10).aspx).

Comptes de Service administrés groupe ne sont pas applicables aux systèmes d’exploitation de Windows avant Windows Server2012.

## <a name="server-manager-information"></a>Informations du Gestionnaire de serveur
Aucune étape de configuration ne sont nécessaires pour implémenter le compte de service administré et le groupe compte de service administré à l’aide du Gestionnaire de serveur ou l’applet de commande Install\-WindowsFeature.

## <a name="BKMK_LINKS"></a>Voir aussi
Le tableau suivant fournit des liens vers des ressources supplémentaires relatives aux comptes de Service administrés et les comptes de Service administrés groupe.

|Type de contenu|Références|
|--------|-------|
|**Évaluation du produit**|[Nouveautés pour les comptes de Service administrés](what-s-new-for-managed-service-accounts.md)<br /><br />[Comptes de Service administrés Documentation pour Windows 7 et Windows Server 2008 R2](https://technet.microsoft.com/library/ff641731(v=ws.10).aspx)<br /><br />[Guide de Step\-by\-étape de comptes de service](https://technet.microsoft.com/library/dd548356(v=ws.10).aspx)|
|**Planification**|Pas encore disponible|
|**Déploiement**|Pas encore disponible|
|**Opérations**|[Comptes de Service dans Active Directory gérés](https://technet.microsoft.com/library/dd378925(v=ws.10).aspx)|
|**Résolution des problèmes**|Pas encore disponible|
|**Évaluation**|[Prise en main de groupe des comptes de Service administrés](getting-started-with-group-managed-service-accounts.md)|
|**Outils et paramètres**|[Dans les Services de domaine Active Directory, les comptes de Service administrés](https://technet.microsoft.com/library/dd378925(v=WS.10).aspx)|
|**Ressources de la Communauté**|[Comptes de Service administrés: Présentation, implémentation, recommandations et dépannage](http://blogs.technet.com/b/askds/archive/2009/09/10/managed-service-accounts-understanding-implementing-best-practices-and-troubleshooting.aspx)|
|**Technologies connexes**|[Vue d’ensemble des Services de domaine ActiveDirectory](active-directory-domain-services-overview.md)|


