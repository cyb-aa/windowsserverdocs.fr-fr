---
title: Déterminer la liste d’autorisation/exclusion et l’inventaire des applications pour les stratégies de restriction logicielle
description: Sécurité de Windows Server
ms.prod: windows-server
ms.technology: security-software-restriction-policies
ms.topic: article
ms.assetid: 0abb73b6-b5d8-4505-8ab1-2f29e4bf0411
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: c0fdb5c1d7c4b03610a173c6cd0575d39646a7d0
ms.sourcegitcommit: af1cf89632d62a94943d3ad9f6b5234b88499278
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81524904"
---
# <a name="determine-allow-deny-list-and-application-inventory-for-software-restriction-policies"></a>Déterminer la liste d’autorisation/exclusion et l’inventaire des applications pour les stratégies de restriction logicielle

>S’applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Cette rubrique destinée aux professionnels de l’informatique fournit des instructions sur la création d’une liste verte et d’exclusion pour les applications qui doivent être gérées par les stratégies de restriction logicielle (SRP) à partir de Windows Server 2008 et Windows Vista.

## <a name="introduction"></a>Introduction
La fonctionnalité Stratégies de restriction logicielle est une fonctionnalité fondée sur les stratégies de groupe qui identifie les programmes logiciels s’exécutant sur les ordinateurs d’un domaine et qui contrôle la capacité de ces programmes à s’exécuter. Les stratégies de restriction logicielle peuvent aussi contribuer à créer une configuration fortement restreinte pour vos ordinateurs, dans laquelle seule l’exécution d’applications clairement identifiées est autorisée. Celles-ci sont intégrées à Microsoft Active Directory Domain Services et stratégie de groupe mais peuvent également être configurées sur des ordinateurs autonomes. Pour un point de départ pour les SRP, consultez [stratégies de restriction logicielle](software-restriction-policies.md).

À compter de Windows Server 2008 R2 et Windows 7, il est possible d’utiliser Windows AppLocker à la place ou en collaboration avec les stratégies de restriction logicielle pour une partie de votre stratégie de contrôle des applications.

Pour plus d’informations sur la façon d’accomplir des tâches spécifiques à l’aide de SRP, consultez les rubriques suivantes :

-   [Utiliser des règles de stratégies de restriction logicielle](work-with-software-restriction-policies-rules.md)

-   [Utiliser des stratégies de restriction logicielle pour aider à protéger votre ordinateur contre un virus de courrier électronique](use-software-restriction-policies-to-help-protect-your-computer-against-an-email-virus.md)

### <a name="what-default-rule-to-choose-allow-or-deny"></a>Quelle règle par défaut choisir : autoriser ou refuser
Les stratégies de restriction logicielle peuvent être déployées dans l’un des deux modes qui constituent la base de votre règle par défaut : liste verte ou liste d’exclusion. Vous pouvez créer une stratégie qui identifie toutes les applications qui sont autorisées à s’exécuter dans votre environnement. la règle par défaut dans votre stratégie est restreinte et bloquera toutes les applications que vous n’autorisez pas à exécuter de manière explicite. Ou vous pouvez créer une stratégie qui identifie toutes les applications qui ne peuvent pas s’exécuter ; la règle par défaut est sans restriction et restreint uniquement les applications que vous avez explicitement listées.

> [!IMPORTANT]
> Le mode liste de refus peut être une stratégie de maintenance élevée pour votre organisation en ce qui concerne le contrôle d’application. La création et la maintenance d’une liste en constante évolution qui empêche tous les logiciels malveillants et autres applications problématiques seraient fastidieuses et sujettes à des erreurs.

### <a name="create-an-inventory-of-your-applications-for-the-allow-list"></a>Créer un inventaire de vos applications pour la liste verte
Pour utiliser efficacement la règle autoriser les valeurs par défaut, vous devez déterminer exactement les applications requises dans votre organisation. Des outils sont conçus pour produire un inventaire des applications, tel que le collecteur d’inventaire dans Microsoft Application Compatibility Toolkit. Mais SRP dispose d’une fonctionnalité de journalisation avancée pour vous aider à comprendre exactement quelles applications s’exécutent dans votre environnement.

##### <a name="to-discover-which-applications-to-allow"></a>Pour découvrir les applications à autoriser

1.  Dans un environnement de test, déployez la stratégie de restriction logicielle avec la règle par défaut définie sur non restreint et supprimez toutes les règles supplémentaires. Si vous activez le SRP sans le forcer à limiter les applications, SPR pourra surveiller les applications en cours d’exécution.

2.  Créez la valeur de Registre suivante pour activer la fonctionnalité de journalisation avancée et définir le chemin d’accès à l’emplacement où le fichier journal doit être écrit.

    **« HKEY_LOCAL_MACHINE \SOFTWARE\Policies\Microsoft\Windows\Safer\CodeIdentifiers »**

    Valeur de chaîne : *chemin LogFileName de LogFileName*

    Étant donné que le SRP évalue toutes les applications lors de son exécution, une entrée est écrite dans le fichier journal *NameLogFile* chaque fois que l’application est exécutée.

3.  Évaluer le fichier journal

    Chaque entrée de journal indique :

    -   l’appelant de la stratégie de restriction logicielle et l’ID de processus (PID) du processus appelant

    -   cible évaluée

    -   la règle SRP qui a été rencontrée lors de l’exécution de cette application

    -   identificateur de la règle de SRP.

    Exemple de sortie écrite dans un fichier journal :

**Explorer. exe (PID = 4728) identifiedC : \ Windows\system32\onenote.exe en tant que règle usingpath non restreinte, Guid = {320bd852-aa7c-4674-82c5-9a80321670a3}**    Toutes les applications et le code associé que les stratégies de restriction logicielle (SRP) vérifient et configurent pour bloquer sont notés dans le fichier journal, que vous pouvez ensuite utiliser pour déterminer les exécutables à prendre en compte pour votre liste autorisée.

