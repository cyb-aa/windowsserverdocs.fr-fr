---
ms.assetid: 3ea48a72-20a2-4da4-84e4-26b5728513ce
title: "Planifier le fichier de l’audit d’accès"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 65e941f6741298da54186be10bd9c0add115ea14
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="plan-for-file-access-auditing"></a>Planifier le fichier de l’audit d’accès

>S’applique à: Windows Server2016, Windows Server2012R2, Windows Server2012

Les informations contenues dans cette rubrique expliquent la sécurité d’audit améliorations introduites dans Windows Server2012 et les nouvelles auditer les paramètres dont vous devez tenir compte lorsque vous déployez le contrôle d’accès dynamique dans votre entreprise. Les paramètres de stratégie d’audit actuels que vous déployez dépendront de vos objectifs, ce qui peuvent inclure la conformité aux normes, analyse, l’analyse d’investigation et résolution des problèmes.  
  
> [!NOTE]  
> Des informations détaillées sur la façon de planifier et déployer une stratégie d’audit pour votre entreprise dans de sécurité globale [planification et déploiement de stratégies d’Audit sécurité avancée](https://go.microsoft.com/fwlink/?LinkID=191139). Pour plus d’informations sur la configuration et déploiement d’une stratégie d’audit de sécurité, voir la [stratégie d’Audit de sécurité avancé Step-by-Step Guide](https://go.microsoft.com/fwlink/?LinkID=191141).  
  
Les fonctionnalités d’audit de sécurité suivantes dans Windows Server2012 peuvent être utilisées avec contrôle d’accès dynamique pour étendre votre stratégie d’audit de sécurité globale.  
  
-   **Stratégies d’audit basées sur des expressions**. Contrôle d’accès dynamique vous permet de créer des stratégies d’audit ciblées en utilisant des expressions basées sur des revendications utilisateur, ordinateur et des ressources. Par exemple, vous pouvez créer une stratégie d’audit pour effectuer le suivi des opérations de lecture et écriture sur les fichiers classés comme le fort impact commercial par les employés qui ne possèdent pas un jeu de haute sécurité. Stratégies d’audit basées sur des expressions peuvent être rédigées directement pour un fichier ou un dossier ou de manière centralisée par le biais de stratégie de groupe. Pour plus d’informations, voir [stratégie de groupe utilisant l’audit d’accès Global objet](https://go.microsoft.com/fwlink/?LinkId=241498).  
  
-   **Des informations supplémentaires à partir de l’accès aux objets**. Audit d’accès aux fichiers n’est pas introduit avec Windows Server2012. Avec la stratégie d’audit appropriée en place, les systèmes d’exploitation Windows et Windows Server génèrent un événement d’audit chaque fois un utilisateur accède à un fichier. Les événements d’accès aux fichiers existants (4656, 4663) contiennent des informations sur les attributs du fichier qui a accédé. Ces informations peuvent être utilisées par les outils de filtrage du journal des événements pour vous aider à identifier les événements d’audit plus pertinents. Pour plus d’informations, voir [d’Audit de Manipulation de Handle](https://technet.microsoft.com//library/dd772626(WS.10).aspx) et [Gestionnaire des comptes de sécurité d’Audit](https://go.microsoft.com/fwlink/?LinkId=241501).  
  
-   **Plus d’informations à partir des événements d’ouverture de session utilisateur**. Avec la stratégie d’audit appropriée en place, les systèmes d’exploitation Windows génèrent un événement d’audit chaque fois qu’un utilisateur se connecte à un ordinateur local ou à distance. Dans Windows Server2012 ou Windows8, vous pouvez également surveiller les revendications utilisateur et périphérique associées au jeton de sécurité d’un utilisateur. Exemples peuvent inclure des habilitations département, entreprise, projet et la sécurité. Événement4626 contient des informations sur ces revendications d’utilisateur et les revendications de périphérique, qui peuvent être exploitées par des outils de gestion de journal d’audit pour mettre en corrélation les événements d’ouverture de session utilisateur avec les événements d’accès aux objets pour activer le filtrage des événements basée sur les attributs de fichier et les attributs utilisateur. Pour plus d’informations sur l’audit d’ouverture de session utilisateur, voir [ouverture de session d’Audit](https://go.microsoft.com/fwlink/?LinkId=241502).  
  
-   **Suivi des modifications pour les nouveaux types d’objets sécurisables**. Suivi des modifications apportées aux objets sécurisables peut être important dans les scénarios suivants:  
  
    -   **Suivi des modifications de stratégies d’accès centralisées et des règles d’accès centralisées**. Stratégies d’accès centralisées et des règles d’accès centralisées définissent la stratégie centrale qui peut être utilisée pour contrôler l’accès aux ressources critiques. Toute modification apportée à ces peut influer directement sur les autorisations d’accès de fichier qui sont accordées aux utilisateurs sur plusieurs ordinateurs. Par conséquent, le suivi des modifications de stratégies d’accès centralisées et des règles d’accès centralisées peut être important pour votre organisation. Étant donné que les stratégies d’accès centralisées et des règles d’accès centralisées sont stockés dans les Services de domaine ActiveDirectory (ADDS), vous pouvez auditer les tentatives de les modifier, comme l’audit des modifications apportées à tout autre objet sécurisable dans ADDS. Pour plus d’informations, voir [auditer l’accès au Service d’annuaire](https://technet.microsoft.com/library/dd941618(WS.10).aspx).  
  
    -   **Suivi des modifications de définitions dans le dictionnaire des revendications**. Définitions des revendications comprennent le nom de la revendication, description et les valeurs possibles. Toute modification apportée à la définition de revendication permettre avoir un impact sur les autorisations d’accès sur les ressources critiques. Par conséquent, le suivi des modifications pour les définitions de revendication peuvent être importants pour votre organisation. Comme les stratégies d’accès centralisées et règles d’accès centralisées, les définitions des revendications sont stockées dans ADDS; par conséquent, ils peuvent être auditées comme tout autre objet sécurisable dans ADDS. Pour plus d’informations, voir [auditer l’accès au Service d’annuaire](https://technet.microsoft.com/library/dd941618(WS.10).aspx).  
  
    -   **Suivi des modifications des attributs de fichier**. Attributs de fichier déterminent centralisée qui s’applique de règle d’accès au fichier. Un changement dans les attributs de fichier peut avoir un impact sur les restrictions d’accès sur le fichier. Par conséquent, il est important de suivre les modifications apportées aux attributs de fichier. Vous pouvez suivre les modifications apportées aux attributs de fichier sur n’importe quel ordinateur en configurant la stratégie d’audit de modification de stratégie d’autorisation. Pour plus d’informations, voir [l’audit de modification de la stratégie d’autorisation](https://go.microsoft.com/fwlink/?LinkId=241504) et [l’audit de l’accès aux objets pour les systèmes de fichiers](https://go.microsoft.com/fwlink/?LinkId=241505). Dans Windows Server2012, événement 4911 fait et modifications de stratégie attribut de fichier à partir d’autres événements de modification de stratégie d’autorisation.  
  
    -   **Suivi des modifications pour la stratégie d’accès centralisée associée à un fichier.** L’événement 4913 affiche les identificateurs de sécurité (SID) des stratégies d’accès centralisées anciennes et nouvelles. Chaque stratégie d’accès centralisée a également un nom convivial qui peut être recherché à l’aide de cet identificateur de sécurité. Pour plus d’informations, voir [l’audit de modification de la stratégie d’autorisation](https://go.microsoft.com/fwlink/?LinkId=241504).  
  
    -   **Suivi des modifications des attributs d’utilisateur et ordinateur**. Comme les fichiers, les objets utilisateur et ordinateur peuvent avoir des attributs et modifications apportées à ces attributs peuvent avoir un impact sur la capacité d’utilisateur à accéder aux fichiers. Par conséquent, il peut être utile pour suivre les modifications apportées aux attributs utilisateur ou ordinateur. Les objets utilisateur et ordinateur sont stockés dans ADDS; par conséquent, les modifications apportées à leurs attributs peuvent être auditées. Pour plus d’informations, voir [accès DS](https://go.microsoft.com/fwlink/?LinkId=241508).  
  
-   **Modification de la stratégie intermédiaire**. Modifications apportées aux stratégies d’accès centralisées peuvent avoir une incidence sur les décisions de contrôle d’accès sur tous les ordinateurs où les stratégies sont appliquées. Une stratégie laxiste pourrait accorder plus d’accès que souhaité, et une stratégie exagérément restrictive pourrait générer un nombre excessif d’appels au support technique. Par conséquent, il peut être extrêmement bénéfique de vérifier les modifications apportées à une stratégie d’accès centralisée avant d’appliquer la modification. Pour cela, Windows Server2012 introduit le concept de «étape intermédiaire». L’étape intermédiaire permet aux utilisateurs de vérifier la stratégie de modifications envisagée avant de les appliquer. Pour utiliser la stratégie intermédiaire, les stratégies envisagées sont déployées avec les stratégies appliquées, mais les stratégies intermédiaires ne pas réellement accorder ou refuser des autorisations. Au lieu de cela, Windows Server2012 enregistre un événement d’audit (4818) chaque fois que le résultat de la vérification d’accès qui utilise la stratégie intermédiaire est différent du résultat d’une vérification d’accès qui utilise la stratégie appliquée.  
  


