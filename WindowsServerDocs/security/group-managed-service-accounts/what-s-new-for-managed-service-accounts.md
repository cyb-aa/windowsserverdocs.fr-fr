---
title: What's New for Managed Service Accounts
description: Sécurité de Windows Server
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: de4d64e3dbe4bc7c7cba32f066a696636632224d
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71403722"
---
# <a name="what39s-new-for-managed-service-accounts"></a>Nouveautés&#39;pour les comptes de service administrés

>S'applique à : Windows Server (Canal semi-annuel), Windows Server 2016

Cette rubrique destinée aux professionnels de l’informatique décrit les modifications apportées aux fonctionnalités pour les comptes de service administrés avec l’introduction du compte de service administré de groupe (gMSA) dans Windows Server 2012 et Windows 8.

Le compte de service administré est conçu pour fournir des services et effectuer des tâches, à l’image des services Windows et des pools d’applications IIS qui partagent leurs propres comptes de domaine, tout en supprimant la nécessité, pour un administrateur, de gérer manuellement les mots de passe de ces comptes. Il s’agit d’un compte de domaine administré qui fournit la gestion automatique des mots de passe.

## <a name="versions"></a>Nouveautés des comptes de service administrés dans Windows Server 2012 et Windows 8
Les éléments suivants décrivent les modifications apportées aux fonctionnalités de MSA dans Windows Server 2012 et Windows 8.

### <a name="group-managed-service-accounts"></a>Comptes de service administrés de groupe
Lorsqu’un compte de domaine est configuré pour un serveur dans un domaine, l’ordinateur client peut s’authentifier auprès de ce service et s’y connecter. Auparavant, seuls deux types de compte pouvaient fournir une identité sans une gestion obligatoire du mot de passe. Toutefois, ces types de compte ont des limites :

-   Le compte d’ordinateur est limité à un serveur de domaine et les mots de passe sont gérés par l’ordinateur.

-   Le compte de service administré est limité à un serveur de domaine et les mots de passe sont gérés par l’ordinateur.

Ces comptes ne peuvent pas être partagés entre plusieurs systèmes. Par conséquent, vous devez régulièrement effectuer la maintenance du compte de chaque service, sur chaque système, afin d’éviter une expiration non souhaitée du mot de passe.

**Quels avantages cette modification procure-t-elle ?**

Le compte de service administré de groupe résout ce problème, car le mot de passe du compte est géré par les contrôleurs de domaine Windows Server 2012 et peut être extrait par plusieurs systèmes Windows Server 2012. Cela permet de réduire la charge de travail d’administration d’un compte de service en permettant à Windows de gérer les mots de passe de ces comptes.

**En quoi le fonctionnement est-il différent ?**

Sur les ordinateurs exécutant Windows Server 2012 ou Windows 8, un MSA de groupe peut être créé et géré par le biais du gestionnaire de contrôle des services afin que de nombreuses instances du service, telles que déployées sur une batterie de serveurs, puissent être gérées à partir d’un serveur. Les outils et utilitaires (par exemple le Gestionnaire de pool d’applications IIS) dont vous vous servez pour gérer les comptes de service administrés, peuvent être utilisés avec les comptes de service administrés de groupe. Les administrateurs de domaine peuvent déléguer la gestion des services aux administrateurs de services, qui peuvent gérer le cycle de vie complet d’un compte de service administré ou du compte de service administré de groupe. Les ordinateurs clients existants sont en mesure de s’authentifier auprès de ce type de service sans savoir à quelle instance de service ils ont affaire.

### <a name="interoperability"></a>Fonctionnalités supprimées ou déconseillées
Pour Windows Server 2012, les applets de commande Windows PowerShell gèrent par défaut les comptes de service administrés de groupe au lieu des comptes de service administrés de serveur.

## <a name="see-also"></a>Voir aussi

-   [Présentation des comptes de service administrés de groupe](group-managed-service-accounts-overview.md)

-   [Vue d’ensemble d’Active Directory Domain Services](active-directory-domain-services-overview.md)

-   Comptes de service @no__t 0Managed : Comprendre, implémenter, meilleures pratiques et résolution des problèmes @ no__t-0


