---
title: "Nouveautés pour les comptes de Service administrés"
description: "Sécurité de Windows Server"
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: security-gmsa
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2f2a8b6b-c152-4c40-b712-bfabff0e408b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: cac55d04a40c84ce160eb3883d6095a7db0ef3be
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/17/2017
---
# <a name="what39s-new-for-managed-service-accounts"></a>Quelle & #39; s nouveaux comptes de Service gérés

>S’applique à: Windows Server (canal annuel un point-virgule), Windows Server2016

Cette rubrique destinée aux professionnels de l’informatique décrit les modifications apportées aux fonctionnalités des comptes de Service administrés avec l’introduction du groupe (gMSA) de compte de Service administré dans Windows Server 2012 et Windows 8.

Le compte de service administré est conçu pour fournir des services et des tâches telles que les services Windows et les pools d’applications IIS pour partager leurs propres comptes de domaine, tout en supprimant la nécessité d’un administrateur de gérer manuellement les mots de passe pour ces comptes. Il est un compte de domaine géré qui fournit la gestion des mots de passe automatique.

## <a name="versions"></a>Nouveautés pour les comptes de Service administrés dans Windows Server 2012 et Windows 8
Le tableau suivant décrit les modifications de fonctionnalités ont été apportées au compte de service administré dans Windows Server 2012 et Windows 8.

### <a name="group-managed-service-accounts"></a>Comptes de Service administrés de groupe
Lorsqu’un compte de domaine est configuré pour un serveur dans un domaine, l’ordinateur client peut s’authentifier et se connecter à ce service. Auparavant, seuls deux types de compte pouvaient fournir une identité sans nécessiter de gestion de mot de passe. Mais ces types de compte ont des limitations:

-   Compte d’ordinateur est limité à un serveur de domaine et les mots de passe sont gérés par l’ordinateur

-   Compte de Service administré est limité à un serveur de domaine et les mots de passe sont gérés par l’ordinateur.

Ces comptes ne peuvent pas être partagés sur plusieurs systèmes. Par conséquent, vous devez mettre à jour régulièrement le compte pour chaque service sur chaque système pour empêcher l’expiration du mot de passe indésirables.

**La valeur ajouter par ce changement?**

Le compte de Service administré de groupe résout ce problème, car le mot de passe du compte est géré par les contrôleurs de domaine Windows Server 2012 et peut être récupéré à plusieurs systèmes Windows Server 2012. Cela permet de réduire la charge administrative d’un compte de service en permettant à Windows de gérer le mot de passe pour ces comptes.

**Ce qui fonctionne différemment?**

Sur les ordinateurs exécutant Windows Server 2012 ou Windows 8, un groupe MSA peut être créé et géré par le biais du Gestionnaire de contrôle de Service afin que plusieurs instances du service, déployées dans une batterie de serveurs, peut être géré à partir d’un serveur. Outils et utilitaires que vous avez utilisé pour administrer les comptes de Service administrés, par exemple, Application Pool Gestionnaire des services Internet, peuvent être utilisés avec les comptes de Service administrés groupe. Les administrateurs de domaine peuvent déléguer la gestion des services aux administrateurs de service, qui peuvent gérer le cycle de vie complet d’un compte de Service administré ou du compte de Service administré de groupe. Les ordinateurs clients existants seront en mesure de s’authentifier auprès de ce type de service sans savoir à quelle instance de service ils ont affaire.

### <a name="interoperability"></a>Fonctionnalité supprimée ou déconseillée
Pour Windows Server 2012, la valeur par défaut des applets de commande Windows PowerShell pour gérer les comptes de Service administrés groupe au lieu des comptes de Service du serveur géré.

## <a name="see-also"></a>Voir aussi

-   [Vue d’ensemble des comptes de Service administrés de groupe](group-managed-service-accounts-overview.md)

-   [Vue d’ensemble des Services de domaine ActiveDirectory](active-directory-domain-services-overview.md)

-   [Comptes de Service administrés: Présentation, implémentation, recommandations et dépannage](http://blogs.technet.com/b/askds/archive/20../managed-service-accounts-understanding-implementing-best-practices-and-troubleshooting.aspx)


