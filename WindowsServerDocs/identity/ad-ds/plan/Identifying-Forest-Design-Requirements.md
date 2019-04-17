---
ms.assetid: 7d957ebb-3476-49d8-b00b-6e93b4a94778
title: "Identification des exigences de conception de forêt"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 91d551e5d7b6daa6b1476570dad57e85d3bc163f
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="identifying-forest-design-requirements"></a>Identification des exigences de conception de forêt

>S’applique à: Windows Server2016, Windows Server2012R2, Windows Server2012

Pour créer une conception de forêt pour votre organisation, vous devez identifier les besoins de l’entreprise qui doit prendre en compte votre structure de répertoires. Cela implique de déterminer la quantité autonomie les groupes dans votre organisation doivent gérer leurs ressources réseau et ou non chaque groupe a besoin d’isoler leurs ressources sur le réseau à partir d’autres groupes.  
  
Services de domaine Active Directory (AD DS) vous permet de concevoir une infrastructure d’annuaire qui prend en compte plusieurs groupes au sein d’une organisation qui ont des exigences de gestion unique et de l’indépendance structurelle et opérationnelle entre les groupes en fonction des besoins.  
  
Groupes dans votre organisation peuvent avoir certains des types suivants de la configuration requise:  
  
-   **Exigences de la structure organisationnelle**. Parties d’une organisation peuvent participer à une infrastructure partagée pour réduire les coûts, mais doit pouvoir fonctionner indépendamment du reste de l’organisation. Par exemple, un groupe de recherche au sein d’une grande organisation devrez garder le contrôle sur tous leurs propres données de recherche.  
  
-   **Exigences opérationnelles**. Une partie d’une organisation peut placer des contraintes uniques sur la configuration du service annuaire, la disponibilité ou la sécurité, ou utiliser les applications qui créent des contraintes uniques sur le répertoire. Par exemple, des divisions individuelles au sein d’une organisation peuvent déployer des applications d’annuaire qui modifient le schéma d’annuaire qui ne sont pas déployées par d’autres unités commerciales. Étant donné que le schéma d’annuaire est partagé entre tous les domaines dans la forêt, la création de plusieurs forêts est une solution pour un tel scénario. Autres exemples sont trouvent dans les scénarios et des organisations suivantes:  
  
    -   Organisations militaires  
  
    -   Scénarios d’hébergement  
  
    -   Organisations qui gèrent un répertoire qui est disponible à la fois en interne et externe (telles que celles qui sont accessibles par les utilisateurs sur Internet)  
  
-   **Exigences légales**. Certaines organisations ont des exigences légales de fonctionner de manière spécifique, par exemple, limitant l’accès à certaines informations comme spécifié dans un contrat. Certaines organisations ont des exigences de sécurité de fonctionner sur des réseaux isolés internes. Échec pour répondre à ces exigences peut entraîner une perte de l’action éventuellement juridique et le contrat.  
  
Une partie d’identification de vos besoins en matière de conception de forêt consiste à identifier le degré auquel des groupes de votre organisation peuvent approuver les propriétaires de forêt potentiels et leurs administrateurs de service et identification des exigences autonomie et isolation pour chaque groupe de votre organisation.  
  
L’équipe de conception doit documenter les exigences d’isolation et d’autonomie pour l’administration du service et des données pour chaque groupe de l’organisation qui a l’intention d’utiliser les services AD DS. L’équipe doit Notez également toutes les zones d’une connectivité limitée susceptibles d’affecter le déploiement des services AD DS.  
  
L’équipe de conception doit documenter les exigences d’isolation et d’autonomie pour l’administration du service et des données pour chaque groupe de l’organisation qui a l’intention d’utiliser les services AD DS. L’équipe doit Notez également toutes les zones d’une connectivité limitée susceptibles d’affecter le déploiement des services AD DS. Pour une feuille de calcul pour vous aider aux régions que vous avez identifié la documentation, téléchargez Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip).  
  
## <a name="in-this-section"></a>Dans cette section  
  
-   [Administrateur de service étendue de l’autorité](../../ad-ds/plan/Service-Administrator-Scope-of-Authority.md)  
  
-   [Autonomie et Isolation](../../ad-ds/plan/Autonomy-vs.-Isolation.md)  
  


