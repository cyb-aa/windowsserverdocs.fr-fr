---
ms.assetid: c0d64566-5530-482e-a332-af029a5fb575
title: Mappage des exigences de conception aux modèles de conception de forêt
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/07/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 4577e65fe5dd2193fe7256cc555e859a78824b4b
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59867940"
---
# <a name="mapping-design-requirements-to-forest-design-models"></a>Mappage des exigences de conception aux modèles de conception de forêt

>S'applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

La plupart des groupes de votre organisation peuvent partager une seule forêt d’organisation qui est géré par un seul groupe informatique (IT) et qui contient les comptes d’utilisateur et les ressources pour tous les groupes qui partagent la forêt. Cette forêt partagée, appelée la forêt d’organisation initiale, constitue la base du modèle de conception de forêt pour l’organisation.  

Étant donné que la forêt d’organisation initiale peut héberger plusieurs groupes de l’organisation, le propriétaire de la forêt doit établir des contrats de niveau de service avec chaque groupe afin que toutes les parties comprennent ce que l'on attend d’eux. Cela protège les groupes et le propriétaire de la forêt en établissant des attentes des services convenues au préalable.  

Si ce n’est pas le cas, tous les groupes dans votre organisation peuvent partager une seule forêt d’organisation, vous devez développer votre conception de forêt pour répondre aux besoins des différents groupes. Cela implique l’identification des exigences de conception qui s’appliquent aux groupes en fonction des besoins d’autonomie et d’isolation et ou non, ils ont un réseau limité à la connectivité et identifier le modèle de forêt que vous pouvez utiliser pour prendre en compte celles configuration requise. Le tableau suivant répertorie les scénarios de modèle de conception de forêt basés sur l’autonomie, isolation et les facteurs de connectivité. Après avoir identifié le scénario de conception de forêt qui correspond le mieux à vos besoins, déterminez si vous devez prendre des décisions tout supplémentaires pour répondre à vos spécifications de conception.  

> [!NOTE]  
> Si un facteur est répertorié comme n/a, il n’est pas un facteur important, car les autres exigences prendre également en charge ce facteur.  

|Scénario|Connectivité limitée|Isolation des données|Autonomie des données|Isolation de service|Autonomie des services|  
|------------|------------------------|------------------|-----------------|---------------------|--------------------|  
|[Scénario 1 : Joindre une forêt existante pour l’autonomie des données](#BKMK_1)|Non|Non|Oui|Non|Non|  
|[Scénario 2 : Utilisez une forêt d’organisation ou un domaine d’autonomie du service](#BKMK_2)|Non|Non|N/A|Non|Oui|  
|[Scénario 3 : Utilisez une forêt d’organisation ou la forêt de ressources pour l’isolation de service](#BKMK_3)|Non|Non|N/A|Oui|N/A|  
|[Scénario 4 : Utilisez une forêt d’organisation ou la forêt à accès restreint pour l’isolation des données](#BKMK_4)|N/A|Oui|N/A|N/A|N/A|  
|[Scénario 5 : Utilisez une forêt d’organisation ou reconfigurer le pare-feu pour une connectivité limitée](#BKMK_5)|Oui|Non|N/A|Non|Non|  
|[Scénario 6 : Utilisez une forêt d’organisation ou un domaine et reconfigurer le pare-feu pour l’autonomie des services avec une connectivité limitée](#BKMK_6)|Oui|Non|N/A|Non|Oui|  
|[Scénario 7 : Utilisez une forêt de ressources et de reconfigurer le pare-feu pour l’isolation de service avec une connectivité limitée](#BKMK_7)|Oui|Non|N/A|Oui|N/A|  

## <a name="BKMK_1"></a>Scénario 1 : Joindre une forêt existante pour l’autonomie des données  

Vous pouvez satisfaire une condition requise pour l’autonomie des données simplement en hébergeant le groupe dans les unités d’organisation (UO) dans une forêt d’organisation existante. Déléguer le contrôle sur les unités d’organisation pour les administrateurs de données à partir de ce groupe pour obtenir l’autonomie des données. Pour plus d’informations sur la délégation du contrôle à l’aide d’unités d’organisation, consultez [création d’une conception d’unité d’organisation](../../ad-ds/plan/Creating-an-Organizational-Unit-Design.md).  
  
## <a name="BKMK_2"></a>Scénario 2 : Utilisez une forêt d’organisation ou un domaine d’autonomie du service  

Si un groupe dans votre organisation identifie l’autonomie des services comme une exigence, nous recommandons que vous jugez tout d’abord nécessaire cette exigence. Atteindre l’autonomie des services crée davantage de surcharge de gestion et des coûts supplémentaires pour l’organisation. Assurez-vous que la configuration requise pour l’autonomie des services n’est pas simplement pour des raisons pratiques et que vous pouvez justifier les coûts impliqués dans cette exigence de réunion.  
  
Vous pouvez remplir une condition requise pour l’autonomie des services en effectuant l’une des opérations suivantes :  

- Création d’une forêt d’organisation. Placez les utilisateurs, les groupes et les ordinateurs du groupe qui requiert l’autonomie des services dans une forêt d’organisation distincte. Affecter un individu à partir de ce groupe soit le propriétaire de la forêt. Si le groupe a besoin pour l’accès ou partage des ressources avec d’autres forêts de l’organisation, ils peuvent établir une approbation entre la forêt de leur organisation et les autres forêts.  

- À l’aide de domaines d’organisation. Placez les utilisateurs, groupes et des ordinateurs dans un domaine distinct dans une forêt d’organisation existante. Ce modèle fournit d’autonomie de service au niveau domaine uniquement et pas pour l’autonomie des services complète, service d’isolation ou l’isolation des données.  

Pour plus d’informations sur l’utilisation des domaines d’organisation, consultez [à l’aide du modèle de forêt de domaine d’organisation](../../ad-ds/plan/../../ad-ds/plan/Using-the-Organizational-Domain-Forest-Model.md).  

## <a name="BKMK_3"></a>Scénario 3 : Utilisez une forêt d’organisation ou la forêt de ressources pour l’isolation de service  

Vous pouvez remplir une exigence pour l’isolation de service en effectuant l’une des opérations suivantes :  

- À l’aide d’une forêt d’organisation. Placez les utilisateurs, les groupes et les ordinateurs du groupe qui requiert l’isolation de service dans une forêt d’organisation distincte. Affecter un individu à partir de ce groupe soit le propriétaire de la forêt. Si le groupe a besoin pour l’accès ou partage des ressources avec d’autres forêts de l’organisation, ils peuvent établir une approbation entre la forêt de leur organisation et les autres forêts. Toutefois, nous déconseillons cette approche, car l’accès aux ressources via les groupes universels est fortement limité dans les scénarios de confiance de forêt.  

- À l’aide d’une forêt de ressources. Placer les ressources et les comptes de service dans une forêt de ressources distinct, en conservant des comptes d’utilisateur dans une forêt d’organisation existante. Si nécessaire, autres comptes peuvent être créés dans la forêt de ressources pour accéder aux ressources dans la forêt de ressources si la forêt d’organisation n’est plus disponible. Les comptes de remplacement doivent avoir l’autorité nécessaire pour vous connecter à la forêt de ressources et de garder le contrôle des ressources jusqu'à ce que la forêt d’organisation est en ligne.  

   Établir une relation d’approbation entre les ressources et les forêts de l’organisation, afin que les utilisateurs peuvent accéder aux ressources de la forêt lors de l’utilisation de leurs comptes d’utilisateur standard. Cette configuration permet une gestion centralisée des comptes d’utilisateur tout en permettant aux utilisateurs de revenir à d’autres comptes dans la forêt de ressources si la forêt d’organisation n’est plus disponible.  

Considérations pour l’isolation de service sont les suivantes :

- Forêts créés pour l’isolation de service peuvent approuver des domaines à partir d’autres forêts mais ne doivent pas inclure les utilisateurs d’autres forêts dans leurs groupes d’administrateurs de service. Si les utilisateurs d’autres forêts sont incluses dans les groupes d’administration dans la forêt isolée, la sécurité de la forêt isolée peut être compromise, car les administrateurs de service dans la forêt n’ont pas de contrôle exclusif.  

- Tant que contrôleurs de domaine sont accessibles sur un réseau, ils sont soumis à des attaques (par exemple, les attaques par déni de service) contre les logiciels malveillants sur ce réseau. Vous pouvez effectuer les opérations suivantes pour protéger contre le risque d’attaque :  

   - Contrôleurs de domaine hôte uniquement sur les réseaux qui sont considérés comme sûrs.  

   - Limiter l’accès au réseau ou réseaux hébergeant les contrôleurs de domaine.  

- Isolation de service requiert la création d’une forêt supplémentaire. Déterminez si le coût de maintenance de l’infrastructure pour prendre en charge de la forêt supplémentaire est supérieur au coût associé à une perte d’accès aux ressources en raison d’une forêt d’organisation ne soit pas disponible.  

## <a name="BKMK_4"></a>Scénario 4 : Utilisez une forêt d’organisation ou la forêt à accès restreint pour l’isolation des données  

Vous pouvez obtenir l’isolation des données en effectuant l’une des opérations suivantes :  

- À l’aide d’une forêt d’organisation. Placez les utilisateurs, les groupes et les ordinateurs du groupe qui requiert l’isolation des données dans une forêt d’organisation distincte. Affecter un individu à partir de ce groupe soit le propriétaire de la forêt. Si le groupe a besoin pour l’accès ou partage des ressources avec d’autres forêts de l’organisation, établissez une approbation entre la forêt d’organisation et les autres forêts. Seuls les utilisateurs qui ont besoin d’accéder aux informations classifiées existent dans la forêt d’organisation. Les utilisateurs ont un seul compte qu’ils utilisent pour les données d’accès classés dans leur propre forêt et les données non classifiées dans d’autres forêts au moyen de relations d’approbation.  

- À l’aide d’une forêt à accès restreint. Il s’agit d’une forêt distincte qui contient les données restreintes et les comptes d’utilisateur qui sont utilisés pour accéder à ces données. Comptes d’utilisateurs distincts sont conservées dans les forêts d’organisation existantes qui sont utilisés pour accéder aux ressources sur le réseau sans restriction. Aucune des approbations ne sont créées entre la forêt à accès restreint et les autres forêts de l’entreprise. Vous pouvez restreindre davantage la forêt en déployant la forêt sur un réseau physique distinct, afin qu’il ne peut pas se connecter à d’autres forêts. Si vous déployez la forêt sur un réseau séparé, les utilisateurs doivent disposer de deux stations de travail : une pour l’accès à la forêt à accès restreint et l’autre pour accéder à des zones nonrestricted du réseau.  

Considérations pour la création de forêts d’isolation des données sont les suivantes :  

- Forêts d’organisation créées pour l’isolation des données peuvent approuver les domaines d’autres forêts, mais les utilisateurs d’autres forêts ne doivent pas être inclus dans les éléments suivants :  

   - Groupes responsables de la gestion des services ou des groupes qui peuvent gérer l’appartenance aux groupes d’administrateur de service  

   - Groupes qui ont un contrôle administratif sur les ordinateurs qui stockent des données protégées  

   - Les groupes qui ont accès aux données protégées ou les groupes qui sont responsables de la gestion des objets utilisateur ou des objets de groupe qui ont accès à des données protégées  

   Si les utilisateurs à partir d’une autre forêt sont inclus dans un de ces groupes, une compromission de l’autre forêt peut entraîner une compromission de la forêt isolée et à la divulgation des données protégées.  

- Autres forêts peuvent être configurés pour approuver la forêt d’organisation créée pour l’isolation des données afin que les utilisateurs dans la forêt isolée peuvent accéder aux ressources dans d’autres forêts. Toutefois, les utilisateurs à partir de la forêt isolée doivent jamais connecter de façon interactive les stations de travail dans la forêt d’approbation. L’ordinateur dans la forêt d’approbation peut être compromis par des logiciels malveillants et peut être utilisé pour capturer les informations d’identification d’ouverture de session de l’utilisateur.  

   > [!NOTE]
   > Pour empêcher les serveurs dans une forêt d’approbation d’emprunter l’identité des utilisateurs à partir de la forêt isolée et puis accèdent aux ressources de la forêt isolée, le propriétaire de la forêt peut désactiver l’authentification déléguée ou utiliser la fonctionnalité de délégation contrainte. Pour plus d’informations sur l’authentification déléguée et la délégation contrainte, consultez [Delegating authentication](https://go.microsoft.com/fwlink/?LinkId=106614).  

- Vous devrez peut-être établir un pare-feu entre la forêt d’organisation et les autres forêts de l’organisation pour limiter l’accès utilisateur aux informations en dehors de leur forêt.  

- Bien que la création d’une forêt distincte permet d’isolation des données, tant que les contrôleurs de domaine dans la forêt isolée et ordinateurs ces informations hôte protégé sont accessibles sur un réseau, ils sont soumis à des attaques lancées à partir d’ordinateurs sur ce réseau. Les organisations qui décide que le risque d’attaque est trop élevé ou que la conséquence d’une violation de sécurité ou d’attaque est trop grande ont besoin limiter l’accès au réseau ou à des réseaux qui hébergent les contrôleurs de domaine et les ordinateurs qui hébergent les données protégées. . Limiter l’accès peut être effectuée à l’aide de technologies telles que les pare-feux et Internet Protocol security (IPsec). Dans les cas extrêmes, les organisations peuvent choisir de conserver des données protégées sur un réseau indépendant qui ne possède aucune connexion physique à un autre réseau de l’organisation.  

   > [!NOTE]  
   > Si aucune connectivité réseau existe entre une forêt à accès restreint et un autre réseau, il est possible pour les données dans la zone restreinte doit être transmis à l’autre réseau.  

## <a name="BKMK_5"></a>Scénario 5 : Utilisez une forêt d’organisation ou reconfigurer le pare-feu pour une connectivité limitée  

Pour répondre à une exigence d’une connectivité limitée, vous pouvez effectuer l’une des opérations suivantes :  

- Placez les utilisateurs dans une forêt d’organisation existante, puis ouvrez suffisamment le pare-feu pour autoriser le trafic Active Directory à traverser.  

- Utilisez une forêt d’organisation. Placez les utilisateurs, groupes et des ordinateurs pour le groupe pour lequel la connectivité est limitée dans une forêt d’organisation distincte. Affecter un individu à partir de ce groupe soit le propriétaire de la forêt. La forêt d’organisation fournit un environnement distinct de l’autre côté du pare-feu. La forêt inclut des comptes d’utilisateur et les ressources qui sont gérés au sein de la forêt, afin que les utilisateurs n’êtes pas obligé de traverser le pare-feu pour accomplir leurs tâches quotidiennes. Des utilisateurs spécifiques ou des applications peuvent avoir des besoins particuliers qui requièrent la fonctionnalité à franchir le pare-feu pour contacter les autres forêts. Vous pouvez répondre à ces besoins individuellement en ouvrant les interfaces appropriées dans le pare-feu, y compris celles nécessaires pour les approbations de fonctionner.  

Pour plus d’informations sur la configuration de pare-feu pour une utilisation avec les Services de domaine Active Directory (AD DS), consultez [Active Directory sur les réseaux segmentés par des pare-feu](https://go.microsoft.com/fwlink/?LinkId=37928).  

## <a name="BKMK_6"></a>Scénario 6 : Utilisez une forêt d’organisation ou un domaine et reconfigurer le pare-feu pour l’autonomie des services avec une connectivité limitée  

Si un groupe dans votre organisation identifie l’autonomie des services comme une exigence, nous recommandons que vous jugez tout d’abord nécessaire cette exigence. Atteindre l’autonomie des services crée davantage de surcharge de gestion et des coûts supplémentaires pour l’organisation. Assurez-vous que la configuration requise pour l’autonomie des services n’est pas simplement pour des raisons pratiques et que vous pouvez justifier les coûts impliqués dans cette exigence de réunion.  

Si une connectivité limitée est un problème et que vous avez besoin pour l’autonomie des services, vous pouvez effectuer l’une des opérations suivantes :  

- Utilisez une forêt d’organisation. Placez les utilisateurs, les groupes et les ordinateurs du groupe qui requiert l’autonomie des services dans une forêt d’organisation distincte. Affecter un individu à partir de ce groupe soit le propriétaire de la forêt. La forêt d’organisation fournit un environnement distinct de l’autre côté du pare-feu. La forêt inclut des comptes d’utilisateur et les ressources qui sont gérés au sein de la forêt, afin que les utilisateurs n’êtes pas obligé de traverser le pare-feu pour accomplir leurs tâches quotidiennes. Des utilisateurs spécifiques ou des applications peuvent avoir des besoins particuliers qui requièrent la fonctionnalité à franchir le pare-feu pour contacter les autres forêts. Vous pouvez répondre à ces besoins individuellement en ouvrant les interfaces appropriées dans le pare-feu, y compris celles nécessaires pour les approbations de fonctionner.  

- Placez les utilisateurs, groupes et des ordinateurs dans un domaine distinct dans une forêt d’organisation existante. Ce modèle fournit d’autonomie de service au niveau domaine uniquement et pas pour l’autonomie des services complète, service d’isolation ou l’isolation des données. Autres groupes dans la forêt doivent approuver les administrateurs de service du nouveau domaine dans la même mesure qu’ils approuvent le propriétaire de la forêt. Pour cette raison, nous déconseillons cette approche. Pour plus d’informations sur l’utilisation des domaines d’organisation, consultez [à l’aide du modèle de forêt de domaine d’organisation](../../ad-ds/plan/../../ad-ds/plan/Using-the-Organizational-Domain-Forest-Model.md).  

Vous devez également ouvrir le pare-feu suffisamment pour autoriser le trafic Active Directory à traverser. Pour plus d’informations sur la configuration de pare-feu pour une utilisation avec les services AD DS, consultez [Active Directory sur les réseaux segmentés par des pare-feu](https://go.microsoft.com/fwlink/?LinkId=37928).  

## <a name="BKMK_7"></a>Scénario 7 : Utilisez une forêt de ressources et de reconfigurer le pare-feu pour l’isolation de service avec une connectivité limitée  

Si une connectivité limitée est un problème et que vous avez besoin pour l’isolation de service, vous pouvez effectuer l’une des opérations suivantes :  

- Utilisez une forêt d’organisation. Placez les utilisateurs, les groupes et les ordinateurs du groupe qui requiert l’isolation de service dans une forêt d’organisation distincte. Affecter un individu à partir de ce groupe soit le propriétaire de la forêt. La forêt d’organisation fournit un environnement distinct de l’autre côté du pare-feu. La forêt inclut des comptes d’utilisateur et les ressources qui sont gérés au sein de la forêt, afin que les utilisateurs n’êtes pas obligé de traverser le pare-feu pour accomplir leurs tâches quotidiennes. Des utilisateurs spécifiques ou des applications peuvent avoir des besoins particuliers qui requièrent la fonctionnalité à franchir le pare-feu pour contacter les autres forêts. Vous pouvez répondre à ces besoins individuellement en ouvrant les interfaces appropriées dans le pare-feu, y compris celles nécessaires pour les approbations de fonctionner.  

- Utilisez une forêt de ressources. Placer les ressources et les comptes de service dans une forêt de ressources distinct, en conservant des comptes d’utilisateur dans une forêt d’organisation existante. Il peut être nécessaire de créer des comptes d’utilisateur autre dans la forêt de ressources pour maintenir l’accès à la forêt de ressources si la forêt d’organisation n’est plus disponible. Les comptes de remplacement doivent avoir l’autorité nécessaire pour vous connecter à la forêt de ressources et de garder le contrôle des ressources jusqu'à ce que la forêt d’organisation est en ligne.  

   Établir une relation d’approbation entre les ressources et les forêts de l’organisation, afin que les utilisateurs peuvent accéder aux ressources de la forêt lors de l’utilisation de leurs comptes d’utilisateur standard. Cette configuration permet une gestion centralisée des comptes d’utilisateur tout en permettant aux utilisateurs de revenir à d’autres comptes dans la forêt de ressources si la forêt d’organisation n’est plus disponible.  

Considérations pour l’isolation de service sont les suivantes :  

- Forêts créés pour l’isolation de service peuvent approuver des domaines à partir d’autres forêts mais ne doivent pas inclure les utilisateurs d’autres forêts dans leurs groupes d’administrateurs de service. Si les utilisateurs d’autres forêts sont incluses dans les groupes d’administration dans la forêt isolée, la sécurité de la forêt isolée peut être compromise, car les administrateurs de service dans la forêt n’ont pas de contrôle exclusif.  

- Tant que contrôleurs de domaine sont accessibles sur un réseau, ils sont soumis à des attaques (par exemple, les attaques par déni de service) à partir d’ordinateurs sur ce réseau. Vous pouvez effectuer les opérations suivantes pour protéger contre le risque d’attaque :  

   - Contrôleurs de domaine hôte uniquement sur les réseaux qui sont considérés comme sûrs.  

   - Limiter l’accès au réseau ou réseaux hébergeant les contrôleurs de domaine.  

- Isolation de service requiert la création d’une forêt supplémentaire. Déterminez si le coût de maintenance de l’infrastructure pour prendre en charge de la forêt supplémentaire est supérieur au coût associé à une perte d’accès aux ressources en raison d’une forêt d’organisation ne soit pas disponible.  

   Des utilisateurs spécifiques ou des applications peuvent avoir des besoins particuliers qui requièrent la fonctionnalité à franchir le pare-feu pour contacter les autres forêts. Vous pouvez répondre à ces besoins individuellement en ouvrant les interfaces appropriées dans le pare-feu, y compris celles nécessaires pour les approbations de fonctionner.  

Pour plus d’informations sur la configuration de pare-feu pour une utilisation avec les services AD DS, consultez [Active Directory sur les réseaux segmentés par des pare-feu](https://go.microsoft.com/fwlink/?LinkId=37928).  
