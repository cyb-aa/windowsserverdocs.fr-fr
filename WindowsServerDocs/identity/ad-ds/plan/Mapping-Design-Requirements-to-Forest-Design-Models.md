---
ms.assetid: c0d64566-5530-482e-a332-af029a5fb575
title: Mappage des exigences de conception aux modèles de conception de forêt
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/07/2018
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 9e5a1d17cbbc5a17b98dff2abf72359ce22142f1
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80822232"
---
# <a name="mapping-design-requirements-to-forest-design-models"></a>Mappage des exigences de conception aux modèles de conception de forêt

>S’applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

La plupart des groupes de votre organisation peuvent partager une forêt d’organisation unique qui est gérée par un seul groupe informatique et qui contient les comptes d’utilisateurs et les ressources pour tous les groupes qui partagent la forêt. Cette forêt partagée, appelée forêt d’organisation initiale, est la base du modèle de conception de forêt pour l’organisation.  

Étant donné que la forêt d’organisation initiale peut héberger plusieurs groupes dans l’organisation, le propriétaire de la forêt doit établir des contrats de niveau de service avec chaque groupe afin que toutes les parties sachent ce qui est attendu. Cela protège à la fois les groupes individuels et le propriétaire de la forêt en établissant des attentes de service accordées.  

Si tous les groupes de votre organisation ne peuvent pas partager une seule forêt d’organisation, vous devez développer votre conception de forêt pour répondre aux besoins des différents groupes. Cela implique d’identifier les exigences de conception qui s’appliquent aux groupes en fonction de leurs besoins en matière d’autonomie et d’isolation et s’ils disposent d’un réseau de connectivité limitée, puis d’identifier le modèle de forêt que vous pouvez utiliser pour répondre à ces exigences. Le tableau suivant répertorie les scénarios de modèle de conception de forêt basés sur les facteurs d’autonomie, d’isolation et de connectivité. Après avoir identifié le scénario de conception de forêt qui correspond le mieux à vos besoins, déterminez si vous devez prendre des décisions supplémentaires pour répondre à vos spécifications de conception.  

> [!NOTE]  
> Si un facteur est listé comme N/A, il n’est pas important, car d’autres exigences prennent également en compte ce facteur.  

|Scénario|Connectivité limitée|Isolation des données|Autonomie des données|Isolation du service|Autonomie des services|  
|------------|------------------------|------------------|-----------------|---------------------|--------------------|  
|[Scénario 1 : rejoindre une forêt existante pour l’autonomie des données](#BKMK_1)|Non|Non|Oui|Non|Non|  
|[Scénario 2 : utiliser une forêt ou un domaine d’organisation pour l’autonomie des services](#BKMK_2)|Non|Non|N/A|Non|Oui|  
|[Scénario 3 : utiliser une forêt d’organisation ou une forêt de ressources pour l’isolation de service](#BKMK_3)|Non|Non|N/A|Oui|N/A|  
|[Scénario 4 : utiliser une forêt d’organisation ou une forêt à accès restreint pour l’isolation des données](#BKMK_4)|N/A|Oui|N/A|N/A|N/A|  
|[Scénario 5 : utiliser une forêt d’organisation ou reconfigurer le pare-feu pour une connectivité limitée](#BKMK_5)|Oui|Non|N/A|Non|Non|  
|[Scénario 6 : utiliser une forêt ou un domaine d’organisation et reconfigurer le pare-feu pour l’autonomie des services avec une connectivité limitée](#BKMK_6)|Oui|Non|N/A|Non|Oui|  
|[Scénario 7 : utiliser une forêt de ressources et reconfigurer le pare-feu pour l’isolation des services avec une connectivité limitée](#BKMK_7)|Oui|Non|N/A|Oui|N/A|  

## <a name="scenario-1-join-an-existing-forest-for-data-autonomy"></a><a name="BKMK_1"></a>Scénario 1 : rejoindre une forêt existante pour l’autonomie des données  

Vous pouvez répondre à une exigence d’autonomie des données simplement en hébergeant le groupe dans des unités d’organisation (UO) dans une forêt d’organisation existante. Déléguez le contrôle des UO aux administrateurs de données à partir de ce groupe pour bénéficier de l’autonomie des données. Pour plus d’informations sur la délégation du contrôle à l’aide d’unités d’organisation, consultez [création d’une conception d’unité d’organisation](../../ad-ds/plan/Creating-an-Organizational-Unit-Design.md).  
  
## <a name="scenario-2-use-an-organizational-forest-or-domain-for-service-autonomy"></a><a name="BKMK_2"></a>Scénario 2 : utiliser une forêt ou un domaine d’organisation pour l’autonomie des services  

Si un groupe de votre organisation identifie l’autonomie du service en tant qu’exigence, nous vous recommandons de reconsidérer cette exigence. L’autonomie du service crée davantage de charge de gestion et des coûts supplémentaires pour l’organisation. Assurez-vous que la configuration requise pour l’autonomie du service n’est pas simple pour des raisons pratiques et que vous pouvez justifier les coûts liés à la satisfaction de cette exigence.  
  
Vous pouvez répondre à une exigence d’autonomie de service en procédant de l’une des manières suivantes :  

- Création d’une forêt d’organisation. Placez les utilisateurs, les groupes et les ordinateurs du groupe qui requièrent l’autonomie du service dans une forêt d’organisation distincte. Attribuez un individu de ce groupe en tant que propriétaire de la forêt. Si le groupe a besoin d’accéder à des ressources ou de les partager avec d’autres forêts de l’organisation, il peut établir une relation d’approbation entre la forêt de l’organisation et les autres forêts.  

- Utilisation des domaines organisationnels. Placez les utilisateurs, les groupes et les ordinateurs dans un domaine distinct dans une forêt d’organisation existante. Ce modèle fournit uniquement l’autonomie de service au niveau du domaine, et non l’autonomie complète du service, l’isolation du service ou l’isolation des données.  

Pour plus d’informations sur l’utilisation des domaines d’organisation, consultez [utilisation du modèle de forêt de domaines organisationnels](../../ad-ds/plan/../../ad-ds/plan/Using-the-Organizational-Domain-Forest-Model.md).  

## <a name="scenario-3-use-an-organizational-forest-or-resource-forest-for-service-isolation"></a><a name="BKMK_3"></a>Scénario 3 : utiliser une forêt d’organisation ou une forêt de ressources pour l’isolation de service  

Vous pouvez répondre à une exigence d’isolation des services en procédant de l’une des façons suivantes :  

- À l’aide d’une forêt d’organisation. Placez les utilisateurs, les groupes et les ordinateurs du groupe qui requièrent l’isolation de service dans une forêt d’organisation distincte. Attribuez un individu de ce groupe en tant que propriétaire de la forêt. Si le groupe a besoin d’accéder à des ressources ou de les partager avec d’autres forêts de l’organisation, il peut établir une relation d’approbation entre la forêt de l’organisation et les autres forêts. Toutefois, cette approche n’est pas recommandée, car l’accès aux ressources par le biais de groupes universels est fortement restreint dans les scénarios d’approbation de forêt.  

- Utilisation d’une forêt de ressources. Placez les ressources et les comptes de service dans une forêt de ressources distincte, en conservant les comptes d’utilisateur dans une forêt d’organisation existante. Si nécessaire, des comptes alternatifs peuvent être créés dans la forêt de ressources pour accéder aux ressources de la forêt de ressources si la forêt d’organisation n’est plus disponible. Les comptes secondaires doivent avoir l’autorité requise pour se connecter à la forêt de ressources et maintenir le contrôle des ressources jusqu’à ce que la forêt soit de nouveau en ligne.  

   Établissez une relation d’approbation entre la ressource et les forêts de l’organisation, afin que les utilisateurs puissent accéder aux ressources de la forêt tout en utilisant leurs comptes d’utilisateur standard. Cette configuration permet une gestion centralisée des comptes d’utilisateur tout en permettant aux utilisateurs de revenir à d’autres comptes dans la forêt de ressources si la forêt d’organisation n’est plus disponible.  

Les éléments à prendre en compte pour l’isolation des services sont les suivants :

- Les forêts créées pour l’isolation de service peuvent approuver les domaines d’autres forêts, mais ne doivent pas inclure les utilisateurs d’autres forêts dans leurs groupes d’administrateurs de service. Si les utilisateurs d’autres forêts sont inclus dans des groupes d’administration de la forêt isolée, la sécurité de la forêt isolée peut potentiellement être compromise, car les administrateurs de service dans la forêt n’ont pas de contrôle exclusif.  

- Tant que les contrôleurs de domaine sont accessibles sur un réseau, ils sont soumis à des attaques (telles que des attaques par déni de service) à partir de logiciels malveillants sur ce réseau. Vous pouvez effectuer les opérations suivantes pour vous protéger contre le risque d’une attaque :  

   - N’hébergez les contrôleurs de domaine que sur les réseaux considérés comme sécurisés.  

   - Limitez l’accès au réseau ou aux réseaux qui hébergent les contrôleurs de domaine.  

- L’isolation de service requiert la création d’une forêt supplémentaire. Déterminez si le coût de maintenance de l’infrastructure pour prendre en charge la forêt supplémentaire compense les coûts associés à la perte d’accès aux ressources en raison de l’indisponibilité d’une forêt d’organisation.  

## <a name="scenario-4-use-an-organizational-forest-or-restricted-access-forest-for-data-isolation"></a><a name="BKMK_4"></a>Scénario 4 : utiliser une forêt d’organisation ou une forêt à accès restreint pour l’isolation des données  

Vous pouvez optimiser l’isolation des données en procédant de l’une des manières suivantes :  

- À l’aide d’une forêt d’organisation. Placez les utilisateurs, les groupes et les ordinateurs du groupe qui requièrent l’isolation des données dans une forêt d’organisation distincte. Attribuez un individu de ce groupe en tant que propriétaire de la forêt. Si le groupe a besoin d’accéder à des ressources ou de les partager avec d’autres forêts de l’organisation, établissez une relation d’approbation entre la forêt de l’organisation et les autres forêts. Seuls les utilisateurs qui ont besoin d’accéder aux informations classifiées existent dans la nouvelle forêt d’organisation. Les utilisateurs disposent d’un compte qu’ils utilisent pour accéder à des données classifiées dans leur propre forêt et à des données non classifiées dans d’autres forêts par le biais de relations d’approbation.  

- Utilisation d’une forêt avec accès restreint. Il s’agit d’une forêt distincte qui contient les données restreintes et les comptes d’utilisateur utilisés pour accéder à ces données. Les comptes d’utilisateurs distincts sont conservés dans les forêts organisationnelles existantes qui sont utilisées pour accéder aux ressources non restreintes sur le réseau. Aucune approbation n’est créée entre la forêt à accès restreint et les autres forêts de l’entreprise. Vous pouvez restreindre davantage la forêt en déployant la forêt sur un réseau physique distinct, afin qu’elle ne puisse pas se connecter à d’autres forêts. Si vous déployez la forêt sur un réseau distinct, les utilisateurs doivent disposer de deux stations de travail : l’une pour accéder à la forêt restreinte et l’autre pour accéder aux zones non restreintes du réseau.  

Les éléments à prendre en compte pour la création de forêts pour l’isolation des données sont les suivants :  

- Les forêts organisationnelles créées pour l’isolation des données peuvent approuver les domaines d’autres forêts, mais les utilisateurs d’autres forêts ne doivent pas être inclus dans l’un des éléments suivants :  

  - Groupes responsables de la gestion des services ou des groupes pouvant gérer l’appartenance des groupes d’administrateurs de service  

  - Groupes qui ont un contrôle administratif sur les ordinateurs qui stockent des données protégées  

  - Groupes ayant accès aux données ou groupes protégés qui sont responsables de la gestion des objets utilisateur ou des objets de groupe qui ont accès aux données protégées  

    Si les utilisateurs d’une autre forêt sont inclus dans l’un de ces groupes, une compromission de l’autre forêt peut entraîner une compromission de la forêt isolée et la divulgation des données protégées.  

- D’autres forêts peuvent être configurées pour approuver la forêt d’organisation créée pour l’isolation des données afin que les utilisateurs de la forêt isolée puissent accéder aux ressources d’autres forêts. Toutefois, les utilisateurs de la forêt isolée ne doivent jamais se connecter de manière interactive aux stations de travail de la forêt d’approbation. L’ordinateur de la forêt d’approbation peut potentiellement être compromis par des logiciels malveillants et peut être utilisé pour capturer les informations d’identification d’ouverture de session de l’utilisateur.  

   > [!NOTE]
   > Pour empêcher les serveurs d’une forêt approuvée d’emprunter l’identité des utilisateurs de la forêt isolée, puis d’accéder aux ressources de la forêt isolée, le propriétaire de la forêt peut désactiver l’authentification déléguée ou utiliser la fonctionnalité de délégation limitée. Pour plus d’informations sur l’authentification déléguée et la délégation avec restriction, consultez délégation de [l’authentification](https://go.microsoft.com/fwlink/?LinkId=106614).  

- Vous devrez peut-être établir un pare-feu entre la forêt de l’organisation et les autres forêts de l’Organisation pour limiter l’accès des utilisateurs aux informations situées en dehors de leur forêt.  

- Bien que la création d’une forêt distincte permette l’isolation des données, tant que les contrôleurs de domaine dans la forêt isolée et les ordinateurs qui hébergent les informations protégées sont accessibles sur un réseau, ils sont soumis à des attaques lancées à partir d’ordinateurs sur ce réseau. Les organisations qui décident que le risque d’attaque est trop élevé ou que la conséquence d’une attaque ou d’une violation de la sécurité est trop importante pour limiter l’accès au réseau ou aux réseaux qui hébergent les contrôleurs de domaine et les ordinateurs qui hébergent des données protégées. La limitation de l’accès peut être effectuée à l’aide de technologies telles que les pare-feu et IPsec (Internet Protocol Security). Dans les cas extrêmes, les organisations peuvent choisir de conserver les données protégées sur un réseau indépendant qui n’a pas de connexion physique à un autre réseau de l’organisation.  

   > [!NOTE]  
   > S’il existe une connectivité réseau entre une forêt à accès restreint et un autre réseau, il est possible que les données de la zone restreinte soient transmises à l’autre réseau.  

## <a name="scenario-5-use-an-organizational-forest-or-reconfigure-the-firewall-for-limited-connectivity"></a><a name="BKMK_5"></a>Scénario 5 : utiliser une forêt d’organisation ou reconfigurer le pare-feu pour une connectivité limitée  

Pour répondre à une exigence de connectivité limitée, vous pouvez effectuer l’une des opérations suivantes :  

- Placez les utilisateurs dans une forêt d’organisation existante, puis ouvrez le pare-feu suffisamment pour autoriser le trafic de Active Directory.  

- Utilisez une forêt d’organisation. Placez les utilisateurs, les groupes et les ordinateurs du groupe pour lesquels la connectivité est limitée dans une forêt d’organisation distincte. Attribuez un individu de ce groupe en tant que propriétaire de la forêt. La forêt d’organisation fournit un environnement distinct de l’autre côté du pare-feu. La forêt comprend des comptes d’utilisateurs et des ressources qui sont gérés au sein de la forêt, de sorte que les utilisateurs n’ont pas besoin de traverser le pare-feu pour accomplir leurs tâches quotidiennes. Des utilisateurs ou des applications spécifiques peuvent avoir des besoins spéciaux qui nécessitent la possibilité de traverser le pare-feu pour contacter d’autres forêts. Vous pouvez répondre à ces besoins individuellement en ouvrant les interfaces appropriées dans le pare-feu, y compris celles nécessaires pour que les approbations fonctionnent.  

Pour plus d’informations sur la configuration de pare-feu pour une utilisation avec Active Directory Domain Services (AD DS), consultez [Active Directory dans les réseaux segmentés par des pare-feu](https://go.microsoft.com/fwlink/?LinkId=37928).  

## <a name="scenario-6-use-an-organizational-forest-or-domain-and-reconfigure-the-firewall-for-service-autonomy-with-limited-connectivity"></a><a name="BKMK_6"></a>Scénario 6 : utiliser une forêt ou un domaine d’organisation et reconfigurer le pare-feu pour l’autonomie des services avec une connectivité limitée  

Si un groupe de votre organisation identifie l’autonomie du service en tant qu’exigence, nous vous recommandons de reconsidérer cette exigence. L’autonomie du service crée davantage de charge de gestion et des coûts supplémentaires pour l’organisation. Assurez-vous que la configuration requise pour l’autonomie du service n’est pas simple pour des raisons pratiques et que vous pouvez justifier les coûts liés à la satisfaction de cette exigence.  

Si la connectivité limitée est un problème et que vous avez besoin d’une autonomie de service, vous pouvez effectuer l’une des opérations suivantes :  

- Utilisez une forêt d’organisation. Placez les utilisateurs, les groupes et les ordinateurs du groupe qui requièrent l’autonomie du service dans une forêt d’organisation distincte. Attribuez un individu de ce groupe en tant que propriétaire de la forêt. La forêt d’organisation fournit un environnement distinct de l’autre côté du pare-feu. La forêt comprend des comptes d’utilisateurs et des ressources qui sont gérés au sein de la forêt, de sorte que les utilisateurs n’ont pas besoin de traverser le pare-feu pour accomplir leurs tâches quotidiennes. Des utilisateurs ou des applications spécifiques peuvent avoir des besoins spéciaux qui nécessitent la possibilité de traverser le pare-feu pour contacter d’autres forêts. Vous pouvez répondre à ces besoins individuellement en ouvrant les interfaces appropriées dans le pare-feu, y compris celles nécessaires pour que les approbations fonctionnent.  

- Placez les utilisateurs, les groupes et les ordinateurs dans un domaine distinct dans une forêt d’organisation existante. Ce modèle fournit uniquement l’autonomie de service au niveau du domaine, et non l’autonomie complète du service, l’isolation du service ou l’isolation des données. Les autres groupes de la forêt doivent faire confiance aux administrateurs de service du nouveau domaine pour qu’ils approuvent le propriétaire de la forêt. Pour cette raison, nous ne recommandons pas cette approche. Pour plus d’informations sur l’utilisation des domaines d’organisation, consultez [utilisation du modèle de forêt de domaines organisationnels](../../ad-ds/plan/../../ad-ds/plan/Using-the-Organizational-Domain-Forest-Model.md).  

Vous devez également ouvrir le pare-feu suffisamment pour permettre le transfert de Active Directory. Pour plus d’informations sur la configuration de pare-feu pour une utilisation avec AD DS, consultez [Active Directory dans les réseaux segmentés par des pare-feu](https://go.microsoft.com/fwlink/?LinkId=37928).  

## <a name="scenario-7-use-a-resource-forest-and-reconfigure-the-firewall-for-service-isolation-with-limited-connectivity"></a><a name="BKMK_7"></a>Scénario 7 : utiliser une forêt de ressources et reconfigurer le pare-feu pour l’isolation des services avec une connectivité limitée  

Si la connectivité limitée est un problème et que vous avez besoin d’un isolement de service, vous pouvez effectuer l’une des opérations suivantes :  

- Utilisez une forêt d’organisation. Placez les utilisateurs, les groupes et les ordinateurs du groupe qui requièrent l’isolation de service dans une forêt d’organisation distincte. Attribuez un individu de ce groupe en tant que propriétaire de la forêt. La forêt d’organisation fournit un environnement distinct de l’autre côté du pare-feu. La forêt comprend des comptes d’utilisateurs et des ressources qui sont gérés au sein de la forêt, de sorte que les utilisateurs n’ont pas besoin de traverser le pare-feu pour accomplir leurs tâches quotidiennes. Des utilisateurs ou des applications spécifiques peuvent avoir des besoins spéciaux qui nécessitent la possibilité de traverser le pare-feu pour contacter d’autres forêts. Vous pouvez répondre à ces besoins individuellement en ouvrant les interfaces appropriées dans le pare-feu, y compris celles nécessaires pour que les approbations fonctionnent.  

- Utilisez une forêt de ressources. Placez les ressources et les comptes de service dans une forêt de ressources distincte, en conservant les comptes d’utilisateur dans une forêt d’organisation existante. Il peut être nécessaire de créer des comptes d’utilisateur de substitution dans la forêt de ressources pour conserver l’accès à la forêt de ressources si celle-ci n’est plus disponible. Les comptes secondaires doivent avoir l’autorité requise pour se connecter à la forêt de ressources et maintenir le contrôle des ressources jusqu’à ce que la forêt soit de nouveau en ligne.  

   Établissez une relation d’approbation entre la ressource et les forêts de l’organisation, afin que les utilisateurs puissent accéder aux ressources de la forêt tout en utilisant leurs comptes d’utilisateur standard. Cette configuration permet une gestion centralisée des comptes d’utilisateur tout en permettant aux utilisateurs de revenir à d’autres comptes dans la forêt de ressources si la forêt d’organisation n’est plus disponible.  

Les éléments à prendre en compte pour l’isolation des services sont les suivants :  

- Les forêts créées pour l’isolation de service peuvent approuver les domaines d’autres forêts, mais ne doivent pas inclure les utilisateurs d’autres forêts dans leurs groupes d’administrateurs de service. Si les utilisateurs d’autres forêts sont inclus dans des groupes d’administration de la forêt isolée, la sécurité de la forêt isolée peut potentiellement être compromise, car les administrateurs de service dans la forêt n’ont pas de contrôle exclusif.  

- Tant que les contrôleurs de domaine sont accessibles sur un réseau, ils sont soumis à des attaques (telles que des attaques par déni de service) à partir d’ordinateurs sur ce réseau. Vous pouvez effectuer les opérations suivantes pour vous protéger contre le risque d’une attaque :  

   - N’hébergez les contrôleurs de domaine que sur les réseaux considérés comme sécurisés.  

   - Limitez l’accès au réseau ou aux réseaux qui hébergent les contrôleurs de domaine.  

- L’isolation de service requiert la création d’une forêt supplémentaire. Déterminez si le coût de maintenance de l’infrastructure pour prendre en charge la forêt supplémentaire compense les coûts associés à la perte d’accès aux ressources en raison de l’indisponibilité d’une forêt d’organisation.  

   Des utilisateurs ou des applications spécifiques peuvent avoir des besoins spéciaux qui nécessitent la possibilité de traverser le pare-feu pour contacter d’autres forêts. Vous pouvez répondre à ces besoins individuellement en ouvrant les interfaces appropriées dans le pare-feu, y compris celles nécessaires pour que les approbations fonctionnent.  

Pour plus d’informations sur la configuration de pare-feu pour une utilisation avec AD DS, consultez [Active Directory dans les réseaux segmentés par des pare-feu](https://go.microsoft.com/fwlink/?LinkId=37928).  
