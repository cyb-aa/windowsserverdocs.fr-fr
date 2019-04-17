---
ms.assetid: c0d64566-5530-482e-a332-af029a5fb575
title: "Mappage des exigences de conception aux modèles de conception de forêt"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 48a2b03c6e29afcca565e861383a831050ef4233
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="mapping-design-requirements-to-forest-design-models"></a>Mappage des exigences de conception aux modèles de conception de forêt

>S’applique à: Windows Server2016, Windows Server2012R2, Windows Server2012

La plupart des groupes de votre organisation peuvent partager une forêt d’organisation unique qui est géré par un seul groupe informatique (IT) et qui contient les comptes d’utilisateur et les ressources pour tous les groupes qui partagent la forêt. Cette forêt partagée, appelée la forêt d’organisation initiale, constitue le fondement du modèle de conception de forêt pour l’organisation.  
  
Étant donné que la forêt d’organisation initiale peut héberger plusieurs groupes de l’organisation, le propriétaire de la forêt doit établir des contrats de niveau de service avec chaque groupe afin que toutes les parties comprennent ce qui est attendu d'entre eux. Cela protège les groupes et le propriétaire de la forêt en établissant des attentes de service accepté.  
  
Si ce n’est pas tous les groupes de votre organisation peuvent partager une seule forêt d’organisation, vous devez développer votre conception de forêt pour répondre aux besoins des différents groupes. Cela implique l’identification des exigences de conception qui s’appliquent aux groupes en fonction de leurs besoins d’autonomie et isolation et ils aient ou non d’un réseau connectivité limitée et puis identifier le modèle de forêt que vous pouvez utiliser pour prendre en compte ces exigences. Le tableau suivant répertorie les scénarios de modèle de conception de forêt basées sur l’autonomie, isolation et les facteurs de connectivité. Après avoir identifié le scénario de conception de forêt qui correspond le mieux à vos besoins, déterminez si vous souhaitez apporter des décisions supplémentaires pour répondre aux spécifications de la conception.  
  
> [!NOTE]  
> Si un facteur est répertorié comme non applicable, il n’est pas un facteur important, car les autres conditions requises également prendre en charge de ce facteur.  
  
|Scénario|Connectivité limitée|Isolation des données|Autonomie des données|Isolation des services|Autonomie du service|  
|------------|------------------------|------------------|-----------------|---------------------|--------------------|  
|[Scénario 1: Joindre une forêt existante autonomie des données](#BKMK_1)|N°|N°|Oui|N°|N°|  
|[Scénario 2: Utiliser une forêt d’organisation ou un domaine d’autonomie du service](#BKMK_2)|N°|N°|NON APPLICABLE|N°|Oui|  
|[Scénario 3: Utiliser une forêt d’organisation ou la forêt de ressources pour l’isolation du service](#BKMK_3)|N°|N°|NON APPLICABLE|Oui|NON APPLICABLE|  
|[Scénario 4: Utiliser une forêt d’organisation ou la forêt à accès restreint d’isolation des données](#BKMK_4)|NON APPLICABLE|Oui|NON APPLICABLE|NON APPLICABLE|NON APPLICABLE|  
|[Scénario 5: Utiliser une forêt d’organisation ou reconfigurer le pare-feu pour une connectivité limitée](#BKMK_5)|Oui|N°|NON APPLICABLE|N°|N°|  
|[Scénario 6: Utiliser une forêt d’organisation ou un domaine et de reconfigurer le pare-feu d’autonomie du service avec une connectivité limitée](#BKMK_6)|Oui|N°|NON APPLICABLE|N°|Oui|  
|[Scénario 7: Utiliser une forêt de ressources et reconfigurer le pare-feu pour l’isolation de service avec une connectivité limitée](#BKMK_7)|Oui|N°|NON APPLICABLE|Oui|NON APPLICABLE|  
  
## <a name="BKMK_1"></a>Scénario 1: Joindre une forêt existante autonomie des données  
Vous pouvez répondre à une exigence d’autonomie des données simplement en héberge le groupe dans les unités d’organisation (UO) dans une forêt d’organisation existante. Déléguer le contrôle sur les unités d’organisation aux administrateurs de données à partir de ce groupe pour assurer l’autonomie des données. Pour plus d’informations sur la délégation de contrôle à l’aide des unités d’organisation, voir [création d’une conception d’unité d’organisation](../../ad-ds/plan/Creating-an-Organizational-Unit-Design.md).  
  
## <a name="BKMK_2"></a>Scénario 2: Utiliser une forêt d’organisation ou un domaine d’autonomie du service  
Si un groupe dans votre organisation identifie l’autonomie du service comme une exigence, nous recommandons que tout d’abord reconsidérez cette exigence. Réalisation d’autonomie du service crée des frais de gestion et des coûts supplémentaires pour l’organisation. Vérifiez que l’exigence d’autonomie du service n’est pas simplement pour des raisons pratiques et que vous pouvez justifier les coûts liés à cette exigence de réunion.  
  
Vous pouvez répondre à une exigence d’autonomie du service en effectuant l’une des opérations suivantes:  
  
-   Création d’une forêt d’organisation. Placez les utilisateurs, les groupes et les ordinateurs du groupe qui requiert l’autonomie du service dans une forêt d’organisation distincte. Affecter un individu à partir de ce groupe pour être le propriétaire de la forêt. Si le groupe doit accès ou partage des ressources avec d’autres forêts de l’organisation, ils peuvent établir une relation d’approbation entre la forêt de leur organisation et les autres forêts.  
  
-   À l’aide de domaines d’organisation. Placez les utilisateurs, les groupes et les ordinateurs dans un domaine distinct dans une forêt d’organisation existante. Ce modèle fournit d’autonomie du service au niveau du domaine uniquement et pas pour autonomie du service complet, le service d’isolation ou l’isolation des données.  
  
Pour plus d’informations sur l’utilisation des domaines d’organisation, voir [à l’aide du modèle de forêt de domaine d’organisation](../../ad-ds/plan/../../ad-ds/plan/Using-the-Organizational-Domain-Forest-Model.md).  
  
## <a name="BKMK_3"></a>Scénario 3: Utiliser une forêt d’organisation ou la forêt de ressources pour l’isolation du service  
Vous pouvez répondre à une condition requise pour l’isolation du service en effectuant l’une des opérations suivantes:  
  
-   À l’aide d’une forêt d’organisation. Placez les utilisateurs, les groupes et les ordinateurs du groupe qui nécessite l’isolation du service dans une forêt d’organisation distincte. Affecter un individu à partir de ce groupe pour être le propriétaire de la forêt. Si le groupe doit accès ou partage des ressources avec d’autres forêts de l’organisation, ils peuvent établir une relation d’approbation entre la forêt de leur organisation et les autres forêts. Toutefois, nous déconseillons cette approche, car l’accès aux ressources via les groupes universels est très limité dans les scénarios d’approbation de forêt.  
  
-   À l’aide d’une forêt de ressources. Placez les ressources et les comptes de service dans une forêt de ressources distinct, en conservant des comptes d’utilisateurs dans une forêt d’organisation existante. Si nécessaire, autres comptes peuvent être créés dans la forêt de ressources pour accéder aux ressources dans la forêt de ressources si la forêt d’organisation devient indisponible. Les autres comptes doivent disposer de l’autorité nécessaire pour ouvrir une session sur la forêt de ressources et garder le contrôle des ressources jusqu'à ce que la forêt d’organisation est remis en ligne.  
  
    Établir une relation d’approbation entre les ressources et les forêts d’organisation, afin que les utilisateurs peuvent accéder aux ressources de la forêt lors de l’utilisation de leurs comptes d’utilisateur standard. Cette configuration permet une gestion centralisée des comptes d’utilisateur tout en autorisant les utilisateurs à revenir à d’autres comptes dans la forêt de ressources si la forêt d’organisation devient indisponible.  
  
Considérations pour l’isolation de service sont les suivantes:  
  
-   Forêts créés pour l’isolation de service peuvent approuver les domaines d’autres forêts, mais ne doivent pas inclure les utilisateurs d’autres forêts dans leurs groupes Administrateurs de service. Si les utilisateurs d’autres forêts sont inclus dans les groupes d’administration de la forêt isolée, la sécurité de la forêt isolée permettre être compromise, car les administrateurs de service dans la forêt ne sont pas exclusive.  
  
-   Tant que contrôleurs de domaine sont accessibles sur un réseau, ils sont soumis à des attaques (tels que les attaques par déni de service) à partir d’un logiciel malveillant sur ce réseau. Vous pouvez effectuer les opérations suivantes pour vous protéger contre les risques d’une attaque:  
  
    -   Contrôleurs de domaine hôte uniquement sur les réseaux qui sont considérées comme sécurisées.  
  
    -   Limiter l’accès au réseau ou à des réseaux qui héberge les contrôleurs de domaine.  
  
-   L’isolation du service nécessite la création d’une forêt supplémentaire. Déterminez si le coût de gestion de l’infrastructure pour prendre en charge de la forêt supplémentaire emportent sur les coûts liés à la perte de l’accès aux ressources en raison d’une forêt d’organisation n’est pas disponible.  
  
## <a name="BKMK_4"></a>Scénario 4: Utiliser une forêt d’organisation ou la forêt à accès restreint d’isolation des données  
Vous pouvez obtenir l’isolation des données en effectuant l’une des opérations suivantes:  
  
-   À l’aide d’une forêt d’organisation. Placez les utilisateurs, les groupes et les ordinateurs du groupe qui nécessite l’isolation des données dans une forêt d’organisation distincte. Affecter un individu à partir de ce groupe pour être le propriétaire de la forêt. Si le groupe doit accès ou partage des ressources avec d’autres forêts de l’organisation, établissez une relation d’approbation entre la forêt d’organisation et les autres forêts. Seuls les utilisateurs qui ont besoin d’accéder aux informations classées existent dans la nouvelle forêt d’organisation. Les utilisateurs ont un compte qu’ils utilisent pour les données des accès classées dans leur propre forêt et les données non classées dans d’autres forêts au moyen de relations d’approbation.  
  
-   À l’aide d’une forêt à accès restreint. Il s’agit d’une forêt distincte qui contient les données restreintes et les comptes d’utilisateur qui sont utilisés pour accéder à ces données. Comptes d’utilisateurs distincts sont conservés dans les forêts existantes d’organisation qui sont utilisés pour accéder aux ressources sur le réseau non restreint. Aucune des approbations ne sont créées entre la forêt à accès restreint et d’autres forêts de l’entreprise. Vous pouvez restreindre davantage la forêt par le déploiement de la forêt sur un autre réseau physique, afin qu’il ne peut pas se connecter à d’autres forêts. Si vous déployez la forêt sur un réseau séparé, les utilisateurs doivent disposer de deux stations de travail: un pour l’accès à la forêt à accès restreint et l’autre pour accéder aux nonrestricted zones du réseau.  
  
Considérations pour la création des forêts d’isolation des données sont les suivantes:  
  
-   Les forêts d’organisation créées pour l’isolation des données peuvent approuver les domaines d’autres forêts, mais les utilisateurs d’autres forêts ne doivent pas être inclus dans les éléments suivants:  
  
    -   Groupes responsables de la gestion des services ou groupes qui peuvent gérer les membres des groupes Administrateurs de services  
  
    -   Les groupes qui ont un contrôle administratif sur les ordinateurs qui stockent des données protégées  
  
    -   Les groupes qui ont accès à des données protégées ou groupes qui sont chargés de la gestion des objets utilisateur ou des objets de groupe qui ont accès à des données protégées  
  
    Si les utilisateurs à partir d’une autre forêt sont inclus dans un de ces groupes, une compromission de l’autre forêt peut entraîner une compromission de la forêt isolée et à la divulgation des données protégées.  
  
-   Autres forêts peuvent être configurés pour approuver la forêt d’organisation créée pour l’isolation des données afin que les utilisateurs dans la forêt isolée peuvent accéder aux ressources dans d’autres forêts. Toutefois, les utilisateurs à partir de la forêt isolée doivent jamais connecter de façon interactive aux stations de travail dans la forêt d’approbation. L’ordinateur dans la forêt d’approbation peut être compromis par des logiciels malveillants et peut être utilisé pour capturer les informations d’identification d’ouverture de session de l’utilisateur.  
  
    > [!NOTE]  
    > Pour empêcher des serveurs dans une forêt d’approbation de l’emprunt d’identité des utilisateurs à partir de la forêt isolée et puis accèdent aux ressources de la forêt isolée, le propriétaire de la forêt permettre désactiver l’authentification déléguée ou utiliser la fonctionnalité de délégation contrainte. Pour plus d’informations sur l’authentification déléguée et la délégation contrainte, voir Délégation de l’authentification ([https://go.microsoft.com/fwlink/?LinkId=106614](https://go.microsoft.com/fwlink/?LinkId=106614)).  
  
-   Vous devrez peut-être établir un pare-feu entre la forêt d’organisation et les autres forêts de l’organisation pour limiter l’accès utilisateur aux informations en dehors de leur forêt.  
  
-   Bien que la création d’une forêt distincte permet d’isolation des données, dans la mesure où les contrôleurs de domaine dans la forêt isolé et les ordinateurs ces informations hôte protégé sont accessibles sur un réseau, ils sont soumis à des attaques lancées à partir d’ordinateurs de ce réseau. Les organisations qui décide que le risque d’attaque est trop élevé ou que les conséquences d’une violation de sécurité ou d’attaque sont trop importante doivent limiter l’accès au réseau ou à des réseaux qui hébergent les contrôleurs de domaine et les ordinateurs qui hébergent des données protégées. Limitation de l’accès peut être effectuée à l’aide de technologies telles que les pare-feu et de sécurité du protocole Internet (IPsec). Dans les cas extrêmes, les organisations peuvent choisir de conserver les données protégées sur un réseau indépendant qui ne dispose d’aucune connexion à un autre réseau physique de l’organisation.  
  
    > [!NOTE]  
    > Si aucune connectivité réseau existe entre une forêt à accès restreint et un autre réseau, il est possible pour les données dans la zone restreinte pour être transmises à l’autre réseau.  
  
## <a name="BKMK_5"></a>Scénario 5: Utiliser une forêt d’organisation ou reconfigurer le pare-feu pour une connectivité limitée  
Pour respecter une connectivité limitée, vous pouvez effectuer l’une des opérations suivantes:  
  
-   Placer des utilisateurs dans une forêt d’organisation existante et ouvrez suffisamment le pare-feu pour autoriser le trafic de traverser Active Directory.  
  
-   Utilisez une forêt d’organisation. Placez les utilisateurs, les groupes et les ordinateurs du groupe pour lequel la connectivité est limitée dans une forêt d’organisation distincte. Affecter un individu à partir de ce groupe pour être le propriétaire de la forêt. La forêt d’organisation fournit un environnement distinct de l’autre côté du pare-feu. La forêt inclut des comptes d’utilisateur et les ressources qui sont gérés dans la forêt, afin que les utilisateurs n’ont pas besoin de traverser le pare-feu pour effectuer leurs tâches quotidiennes. Des utilisateurs spécifiques ou des applications peuvent avoir des besoins spéciaux qui requièrent la possibilité de traverser le pare-feu de contacter d’autres forêts. Vous pouvez répondre à ces besoins individuellement en ouvrant les interfaces appropriées dans le pare-feu, y compris celles nécessaires pour les approbations de fonctionner.  
  
Pour plus d’informations sur la configuration des pare-feu pour une utilisation avec les Services de domaine ActiveDirectory (ADDS), voir ActiveDirectory sur les réseaux segmentés par des pare-feu ([https://go.microsoft.com/fwlink/?LinkId=37928](https://go.microsoft.com/fwlink/?LinkId=37928)).  
  
## <a name="BKMK_6"></a>Scénario 6: Utiliser une forêt d’organisation ou un domaine et de reconfigurer le pare-feu d’autonomie du service avec une connectivité limitée  
Si un groupe dans votre organisation identifie l’autonomie du service comme une exigence, nous recommandons que tout d’abord reconsidérez cette exigence. Réalisation d’autonomie du service crée des frais de gestion et des coûts supplémentaires pour l’organisation. Vérifiez que l’exigence d’autonomie du service n’est pas simplement pour des raisons pratiques et que vous pouvez justifier les coûts liés à cette exigence de réunion.  
  
Si une connectivité limitée est un problème et que vous avez besoin d’autonomie du service, vous pouvez effectuer l’une des opérations suivantes:  
  
-   Utilisez une forêt d’organisation. Placez les utilisateurs, les groupes et les ordinateurs du groupe qui requiert l’autonomie du service dans une forêt d’organisation distincte. Affecter un individu à partir de ce groupe pour être le propriétaire de la forêt. La forêt d’organisation fournit un environnement distinct de l’autre côté du pare-feu. La forêt inclut des comptes d’utilisateur et les ressources qui sont gérés dans la forêt, afin que les utilisateurs n’ont pas besoin de traverser le pare-feu pour effectuer leurs tâches quotidiennes. Des utilisateurs spécifiques ou des applications peuvent avoir des besoins spéciaux qui requièrent la possibilité de traverser le pare-feu de contacter d’autres forêts. Vous pouvez répondre à ces besoins individuellement en ouvrant les interfaces appropriées dans le pare-feu, y compris celles nécessaires pour les approbations de fonctionner.  
  
-   Placez les utilisateurs, les groupes et les ordinateurs dans un domaine distinct dans une forêt d’organisation existante. Ce modèle fournit d’autonomie du service au niveau du domaine uniquement et pas pour autonomie du service complet, le service d’isolation ou l’isolation des données. Autres groupes dans la forêt doivent approuver les administrateurs de service du nouveau domaine au même degré qu’ils approuvent le propriétaire de la forêt. Pour cette raison, nous déconseillons cette approche. Pour plus d’informations sur l’utilisation des domaines d’organisation, voir [à l’aide du modèle de forêt de domaine d’organisation](../../ad-ds/plan/../../ad-ds/plan/Using-the-Organizational-Domain-Forest-Model.md).  
  
Vous devez également ouvrir le pare-feu suffisamment pour autoriser le trafic de traverser Active Directory. Pour plus d’informations sur la configuration des pare-feu pour une utilisation avec les services ADDS, voir ActiveDirectory sur les réseaux segmentés par des pare-feu ([https://go.microsoft.com/fwlink/?LinkId=37928](https://go.microsoft.com/fwlink/?LinkId=37928)).  
  
## <a name="BKMK_7"></a>Scénario 7: Utiliser une forêt de ressources et reconfigurer le pare-feu pour l’isolation de service avec une connectivité limitée  
Si une connectivité limitée est un problème et que vous avez besoin pour l’isolation du service, vous pouvez effectuer l’une des opérations suivantes:  
  
-   Utilisez une forêt d’organisation. Placez les utilisateurs, les groupes et les ordinateurs du groupe qui nécessite l’isolation du service dans une forêt d’organisation distincte. Affecter un individu à partir de ce groupe pour être le propriétaire de la forêt. La forêt d’organisation fournit un environnement distinct de l’autre côté du pare-feu. La forêt inclut des comptes d’utilisateur et les ressources qui sont gérés dans la forêt, afin que les utilisateurs n’ont pas besoin de traverser le pare-feu pour effectuer leurs tâches quotidiennes. Des utilisateurs spécifiques ou des applications peuvent avoir des besoins spéciaux qui requièrent la possibilité de traverser le pare-feu de contacter d’autres forêts. Vous pouvez répondre à ces besoins individuellement en ouvrant les interfaces appropriées dans le pare-feu, y compris celles nécessaires pour les approbations de fonctionner.  
  
-   Utilisez une forêt de ressources. Placez les ressources et les comptes de service dans une forêt de ressources distinct, en conservant des comptes d’utilisateurs dans une forêt d’organisation existante. Il peut être nécessaire de créer certains comptes d’utilisateur dans la forêt de ressources pour conserver l’accès à la forêt de ressources si la forêt d’organisation devient indisponible. Les autres comptes doivent disposer de l’autorité nécessaire pour ouvrir une session sur la forêt de ressources et garder le contrôle des ressources jusqu'à ce que la forêt d’organisation est remis en ligne.  
  
    Établir une relation d’approbation entre les ressources et les forêts d’organisation, afin que les utilisateurs peuvent accéder aux ressources de la forêt lors de l’utilisation de leurs comptes d’utilisateur standard. Cette configuration permet une gestion centralisée des comptes d’utilisateur tout en autorisant les utilisateurs à revenir à d’autres comptes dans la forêt de ressources si la forêt d’organisation devient indisponible.  
  
Considérations pour l’isolation de service sont les suivantes:  
  
-   Forêts créés pour l’isolation de service peuvent approuver les domaines d’autres forêts, mais ne doivent pas inclure les utilisateurs d’autres forêts dans leurs groupes Administrateurs de service. Si les utilisateurs d’autres forêts sont inclus dans les groupes d’administration de la forêt isolée, la sécurité de la forêt isolée permettre être compromise, car les administrateurs de service dans la forêt ne sont pas exclusive.  
  
-   Dans la mesure où les contrôleurs de domaine sont accessibles sur un réseau, ils sont soumis à des attaques (tels que les attaques par déni de service) à partir d’ordinateurs sur ce réseau. Vous pouvez effectuer les opérations suivantes pour vous protéger contre les risques d’une attaque:  
  
    -   Contrôleurs de domaine hôte uniquement sur les réseaux qui sont considérées comme sécurisées.  
  
    -   Limiter l’accès au réseau ou à des réseaux qui héberge les contrôleurs de domaine.  
  
-   L’isolation du service nécessite la création d’une forêt supplémentaire. Déterminez si le coût de gestion de l’infrastructure pour prendre en charge de la forêt supplémentaire emportent sur les coûts liés à la perte de l’accès aux ressources en raison d’une forêt d’organisation n’est pas disponible.  
  
    Des utilisateurs spécifiques ou des applications peuvent avoir des besoins spéciaux qui requièrent la possibilité de traverser le pare-feu de contacter d’autres forêts. Vous pouvez répondre à ces besoins individuellement en ouvrant les interfaces appropriées dans le pare-feu, y compris celles nécessaires pour les approbations de fonctionner.  
  
Pour plus d’informations sur la configuration des pare-feu pour une utilisation avec les services ADDS, voir ActiveDirectory sur les réseaux segmentés par des pare-feu ([https://go.microsoft.com/fwlink/?LinkId=37928](https://go.microsoft.com/fwlink/?LinkId=37928)).  
  


