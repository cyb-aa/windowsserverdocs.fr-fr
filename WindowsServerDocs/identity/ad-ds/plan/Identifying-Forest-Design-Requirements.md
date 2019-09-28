---
ms.assetid: 7d957ebb-3476-49d8-b00b-6e93b4a94778
title: Identifier les conditions requises pour la conception d'une forêt
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/07/2018
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 41caeca82819eaea3d86d5f1eb4883ab8bbf53cc
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71408791"
---
# <a name="identifying-forest-design-requirements"></a>Identifier les conditions requises pour la conception d'une forêt

>S'applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Pour créer une conception de forêt pour votre organisation, vous devez identifier les besoins de l’entreprise qui doivent être pris en charge par votre structure de répertoires. Cela implique de déterminer l’autonomie dont les groupes de votre organisation ont besoin pour gérer leurs ressources réseau et si chaque groupe doit ou non isoler ses ressources sur le réseau à partir d’autres groupes.  
  
Active Directory Domain Services (AD DS) vous permet de concevoir une infrastructure d’annuaire qui prend en charge plusieurs groupes au sein d’une organisation qui ont des exigences de gestion uniques et pour obtenir une indépendance structurelle et opérationnelle entre les groupes. Si nécessaire.  
  
Les groupes de votre organisation peuvent présenter certains des types de configuration suivants :  
  
-   **Exigences relatives à la structure organisationnelle**. Les parties d’une organisation peuvent participer à une infrastructure partagée pour réduire les coûts tout en exigeant la possibilité de fonctionner indépendamment du reste de l’organisation. Par exemple, un groupe de recherche au sein d’une grande organisation peut avoir besoin de garder le contrôle sur toutes ses données de recherche.  
  
-   **Exigences opérationnelles**. Une partie d’une organisation peut placer des contraintes uniques sur la configuration, la disponibilité ou la sécurité du service d’annuaire, ou utiliser des applications qui placent des contraintes uniques sur l’annuaire. Par exemple, des unités commerciales individuelles au sein d’une organisation peuvent déployer des applications compatibles avec l’annuaire qui modifient le schéma d’annuaire qui n’est pas déployé par d’autres divisions. Étant donné que le schéma d’annuaire est partagé entre tous les domaines de la forêt, la création de plusieurs forêts est une solution pour ce type de scénario. D’autres exemples sont disponibles dans les organisations et les scénarios suivants :  
  
    -   Organisations militaires  
  
    -   Scénarios d’hébergement  
  
    -   Organisations qui maintiennent un répertoire disponible à la fois en interne et en externe (par exemple, ceux qui sont accessibles publiquement par les utilisateurs sur Internet)  
  
-   **Exigences légales**. Certaines organisations ont des exigences légales pour fonctionner d’une manière spécifique, par exemple, restreindre l’accès à certaines informations comme spécifié dans un contrat d’entreprise. Certaines organisations ont des exigences de sécurité pour fonctionner sur des réseaux internes isolés. Le non-respect de ces exigences peut entraîner la perte du contrat et éventuellement une action légale.  
  
Une partie de l’identification des exigences de conception de votre forêt consiste à identifier le degré auquel les groupes de votre organisation peuvent faire confiance aux propriétaires de forêts potentiels et à leurs administrateurs de service et à identifier les exigences d’autonomie et d’isolation pour chaque groupe au sein de votre organisation.  
  
L’équipe de conception doit documenter les exigences en matière d’isolation et d’autonomie pour l’administration des services et des données pour chaque groupe de l’organisation qui a l’intention d’utiliser AD DS. L’équipe doit également noter toutes les zones de connectivité limitée susceptibles d’affecter le déploiement de AD DS.  
  
L’équipe de conception doit documenter les exigences en matière d’isolation et d’autonomie pour l’administration des services et des données pour chaque groupe de l’organisation qui a l’intention d’utiliser AD DS. L’équipe doit également noter toutes les zones de connectivité limitée susceptibles d’affecter le déploiement de AD DS. Pour obtenir une feuille de calcul qui vous aide à documenter les régions que vous avez identifiées, téléchargez Job_Aids_Designing_and_Deploying_Directory_and_Security_Services. zip à partir des [Outils d’aide à la demande pour le kit de déploiement de Windows Server 2003](https://go.microsoft.com/fwlink/?LinkID=102558) et ouvrez « Configuration requise pour la conception de la forêt » ( DSSLOGI_2. doc).  
  
## <a name="in-this-section"></a>Dans cette section  
  
-   [Étendue de l’autorité de l’administrateur de service](../../ad-ds/plan/Service-Administrator-Scope-of-Authority.md)  
  
-   [Autonomie et isolation](../../ad-ds/plan/Autonomy-vs.-Isolation.md)  
