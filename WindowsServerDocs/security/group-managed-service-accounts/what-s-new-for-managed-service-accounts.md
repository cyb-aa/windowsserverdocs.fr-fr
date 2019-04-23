---
title: What's New for Managed Service Accounts
description: Sécurité de Windows Server
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
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59872180"
---
# <a name="what39s-new-for-managed-service-accounts"></a>Ce que&#39;s nouveauté pour les comptes de Service administrés

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016

Cette rubrique destinée aux professionnels de l’informatique décrit les modifications de fonctionnalités pour les comptes de Service administrés avec l’introduction du groupe compte de Service administré (gMSA) dans Windows Server 2012 et Windows 8.

Le compte de service administré est conçu pour fournir des services et effectuer des tâches, à l’image des services Windows et des pools d’applications IIS qui partagent leurs propres comptes de domaine, tout en supprimant la nécessité, pour un administrateur, de gérer manuellement les mots de passe de ces comptes. Il s’agit d’un compte de domaine administré qui fournit la gestion automatique des mots de passe.

## <a name="versions"></a>Quelles sont les nouveautés des comptes de Service administrés dans Windows Server 2012 et Windows 8
La section suivante décrit les modifications de fonctionnalités ont été apportées au compte de service administré dans Windows Server 2012 et Windows 8.

### <a name="group-managed-service-accounts"></a>Comptes de service administrés de groupe
Lorsqu’un compte de domaine est configuré pour un serveur dans un domaine, l’ordinateur client peut s’authentifier auprès de ce service et s’y connecter. Auparavant, seuls deux types de compte pouvaient fournir une identité sans une gestion obligatoire du mot de passe. Toutefois, ces types de compte ont des limites :

-   Le compte d’ordinateur est limité à un serveur de domaine et les mots de passe sont gérés par l’ordinateur.

-   Le compte de service administré est limité à un serveur de domaine et les mots de passe sont gérés par l’ordinateur.

Ces comptes ne peuvent pas être partagés entre plusieurs systèmes. Par conséquent, vous devez régulièrement effectuer la maintenance du compte de chaque service, sur chaque système, afin d’éviter une expiration non souhaitée du mot de passe.

**Quels avantages cette modification procure-t-elle ?**

Le compte de Service administré de groupe résout ce problème, car le mot de passe du compte est géré par les contrôleurs de domaine Windows Server 2012 et peut être récupérée par plusieurs systèmes de Windows Server 2012. Cela permet de réduire la charge de travail d’administration d’un compte de service en permettant à Windows de gérer les mots de passe de ces comptes.

**En quoi le fonctionnement est-il différent ?**

Sur les ordinateurs exécutant Windows Server 2012 ou Windows 8, un groupe de compte de service administré peut être créé et géré par le biais du Gestionnaire de contrôle de Service afin que plusieurs instances du service, déployées dans une batterie de serveurs peut être géré à partir d’un seul serveur. Les outils et utilitaires (par exemple le Gestionnaire de pool d’applications IIS) dont vous vous servez pour gérer les comptes de service administrés, peuvent être utilisés avec les comptes de service administrés de groupe. Les administrateurs de domaine peuvent déléguer la gestion des services aux administrateurs de services, qui peuvent gérer le cycle de vie complet d’un compte de service administré ou du compte de service administré de groupe. Les ordinateurs clients existants sont en mesure de s’authentifier auprès de ce type de service sans savoir à quelle instance de service ils ont affaire.

### <a name="interoperability"></a>Fonctionnalité supprimée ou déconseillée
Pour Windows Server 2012, la valeur par défaut des applets de commande Windows PowerShell pour gérer les comptes de Service administrés groupe au lieu des comptes de Service du serveur géré.

## <a name="see-also"></a>Voir aussi

-   [Présentation des comptes de Service administrés de groupe](group-managed-service-accounts-overview.md)

-   [Vue d’ensemble des Services de domaine Active Directory](active-directory-domain-services-overview.md)

-   [Comptes de Service administrés : Présentation, implémentation, recommandations et dépannage](http://blogs.technet.com/b/askds/archive/20../managed-service-accounts-understanding-implementing-best-practices-and-troubleshooting.aspx)


