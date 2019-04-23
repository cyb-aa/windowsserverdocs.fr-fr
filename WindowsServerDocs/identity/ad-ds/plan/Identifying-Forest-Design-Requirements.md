---
ms.assetid: 7d957ebb-3476-49d8-b00b-6e93b4a94778
title: Identifier les conditions requises pour la conception d'une forêt
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/07/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: bf8d9d164bf07151572785cda906be911f97b53e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59835330"
---
# <a name="identifying-forest-design-requirements"></a>Identifier les conditions requises pour la conception d'une forêt

>S'applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Pour créer une conception de forêt pour votre organisation, vous devez identifier les besoins de l’entreprise qui a besoin de votre structure de répertoires pour prendre en charge. Cela implique de déterminer la quantité autonomie les groupes dans les besoins de votre organisation pour gérer leurs ressources réseau et s’il faut ou non chaque groupe doit isoler leurs ressources sur le réseau à partir d’autres groupes.  
  
Services de domaine Active Directory (AD DS) vous permet de concevoir une infrastructure d’annuaire prenant en charge plusieurs groupes au sein d’une organisation qui ont des exigences de gestion unique et à l’indépendance opérationnelle et structurelles entre les groupes en fonction des besoins.  
  
Groupes de votre organisation peuvent avoir certains des types suivants de la configuration requise :  
  
-   **Exigences de la structure organisationnelle**. Parties d’une organisation peuvent participer à une infrastructure partagée pour réduire les coûts, mais doit pouvoir fonctionner indépendamment du reste de l’organisation. Par exemple, un groupe de recherche au sein d’une grande organisation devrez garder le contrôle sur l’ensemble de ses données de recherche.  
  
-   **Les exigences opérationnelles**. Une partie d’une organisation peut placer des contraintes uniques sur la configuration de service d’annuaire, la disponibilité ou la sécurité, ou utiliser des applications qui créent des contraintes uniques sur le répertoire. Par exemple, des divisions individuelles au sein d’une organisation peuvent déployer des applications d’annuaire qui modifient le schéma d’annuaire qui ne sont pas déployées par les autres divisions. Étant donné que le schéma d’annuaire est partagé entre tous les domaines de la forêt, la création de plusieurs forêts est une solution pour un tel scénario. Autres exemples sont trouvent dans les scénarios et les entreprises suivantes :  
  
    -   Organisations militaires  
  
    -   Scénarios d’hébergement  
  
    -   Organisations qui gèrent un répertoire qui n’est disponible à la fois en interne et en externe (par exemple, celles qui sont publiquement accessibles aux utilisateurs sur Internet)  
  
-   **Exigences légales**. Certaines organisations ont des exigences légales de fonctionner de manière spécifique, par exemple, restreindre l’accès à certaines informations comme spécifié dans un contrat entreprise. Certaines organisations ont des exigences de sécurité pour fonctionner sur les réseaux internes isolés. Échec de répondre à ces exigences peut entraîner une perte du contrat et de l’action en justice éventuellement.  
  
Partie de vos exigences de conception de forêt implique identifiant le degré auquel les groupes dans votre organisation peuvent faire confiance les propriétaires de forêt potentiels et leurs administrateurs de service et d’identifier les exigences d’autonomie et d’isolation pour chaque groupe de votre organisation.  
  
L’équipe de conception doit documenter les exigences d’isolation et d’autonomie pour l’administration de service et de données pour chaque groupe dans l’organisation qui a l’intention d’utiliser les services AD DS. L’équipe doit noter également toutes les zones d’une connectivité limitée susceptibles d’affecter le déploiement des services AD DS.  
  
L’équipe de conception doit documenter les exigences d’isolation et d’autonomie pour l’administration de service et de données pour chaque groupe dans l’organisation qui a l’intention d’utiliser les services AD DS. L’équipe doit noter également toutes les zones d’une connectivité limitée susceptibles d’affecter le déploiement des services AD DS. Pour une feuille de calcul pour vous aider à documenter les régions que vous avez identifié, téléchargez Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip à partir de [travail aides pour Windows Server 2003 Deployment Kit](https://go.microsoft.com/fwlink/?LinkID=102558) et ouvrez « forêt Configuration requise » (DSSLOGI_2.doc).  
  
## <a name="in-this-section"></a>Dans cette section  
  
-   [Étendue d’administrateur de service de l’autorité](../../ad-ds/plan/Service-Administrator-Scope-of-Authority.md)  
  
-   [Autonomie et. Isolation](../../ad-ds/plan/Autonomy-vs.-Isolation.md)  
