---
title: Déterminer la liste d’autorisation/exclusion et l’inventaire des applications pour les stratégies de restriction logicielle
description: Sécurité de Windows Server
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: security-software-restriction-policies
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0abb73b6-b5d8-4505-8ab1-2f29e4bf0411
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: 60e78912284715649938567d66ffb90b9890b1b9
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59847290"
---
# <a name="determine-allow-deny-list-and-application-inventory-for-software-restriction-policies"></a>Déterminer la liste d’autorisation/exclusion et l’inventaire des applications pour les stratégies de restriction logicielle

>S'applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Cette rubrique destinée aux professionnels de l’informatique fournit conseils créer un autoriser et refuser de liste pour les applications qui seront gérés par les stratégies de Restriction logicielle (SRP) à compter de Windows Server 2008 et Windows Vista.

## <a name="introduction"></a>Introduction
La fonctionnalité Stratégies de restriction logicielle est une fonctionnalité fondée sur les stratégies de groupe qui identifie les programmes logiciels s’exécutant sur les ordinateurs d’un domaine et qui contrôle la capacité de ces programmes à s’exécuter. Les stratégies de restriction logicielle peuvent aussi contribuer à créer une configuration fortement restreinte pour vos ordinateurs, dans laquelle seule l’exécution d’applications clairement identifiées est autorisée. Elles sont intégrées aux Services de domaine Microsoft Active Directory et stratégie de groupe, mais peuvent également être configurés sur des ordinateurs autonomes. Pour un point de départ pour les stratégies de restriction logicielle, consultez le [stratégies de Restriction logicielle](software-restriction-policies.md).

Depuis Windows Server 2008 R2 et Windows 7, Windows AppLocker peut servir à la place d’ou de concert avec les stratégies de restriction logicielle pour une partie de votre stratégie de contrôle d’application.

Pour plus d’informations sur la façon d’accomplir des tâches spécifiques à l’aide de stratégies de restriction logicielle, consultez les rubriques suivantes :

-   [Travailler avec des règles de stratégies de Restriction logicielle](work-with-software-restriction-policies-rules.md)

-   [Utiliser des stratégies de Restriction logicielle pour vous aider à protéger votre ordinateur contre un Virus de courrier électronique](use-software-restriction-policies-to-help-protect-your-computer-against-an-email-virus.md)

### <a name="what-default-rule-to-choose-allow-or-deny"></a>Quelle règle par défaut pour choisir : Autoriser ou refuser
Stratégies de restriction logicielle peuvent être déployées dans deux modes qui constituent la base de votre règle par défaut : Liste verte ou liste d’exclusion. Vous pouvez créer une stratégie qui identifie chaque application est autorisée à s’exécuter dans votre environnement ; la règle par défaut au sein de votre stratégie est restreint et bloquera toutes les applications que vous n’autorisez pas explicitement à exécuter. Ou vous pouvez créer une stratégie qui identifie toutes les applications qui ne peut pas s’exécuter ; la règle par défaut est non restreint et restreint seulement les applications que vous avez répertoriées explicitement.

> [!IMPORTANT]
> Le mode liste de refus peut être une stratégie de maintenance élevée pour votre organisation en matière de contrôle de l’application. Création et gestion d’une liste en constante évolution qui interdit tous les programmes malveillants et autres applications problématiques serait beaucoup de temps et sujette aux erreurs.

### <a name="create-an-inventory-of-your-applications-for-the-allow-list"></a>Créer un inventaire de vos applications pour la liste verte
Pour utiliser efficacement la règle d’autorisation par défaut, vous devez déterminer exactement quelles applications sont nécessaires dans votre organisation. Il existe des outils conçus pour produire un inventaire des applications, telles que le collecteur d’inventaire dans Microsoft Application Compatibility Toolkit. Mais les stratégies de restriction logicielle possède une fonctionnalité de journalisation avancée pour vous aider à comprendre exactement ce que les applications sont en cours d’exécution dans votre environnement.

##### <a name="to-discover-which-applications-to-allow"></a>Pour découvrir les applications à autoriser

1.  Dans un environnement de test, déployez la stratégie de Restriction logicielle avec l’ensemble de règles par défaut non restreint et supprimez les règles supplémentaires. Si vous activez des stratégies de restriction logicielle sans forcer à restreindre toutes les applications, SPR sera en mesure de surveiller ce que les applications sont en cours d’exécution.

2.  Créez la valeur de Registre suivante afin d’activer la fonctionnalité de journalisation avancée et de la valeur est le chemin d’accès dans lequel le fichier journal doit être écrit.

    **"HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows\Safer\ CodeIdentifiers"**

    Valeur de chaîne : *Chemin d’accès de NameLogFile à NameLogFile*

    Étant donné que les stratégies de restriction logicielle évalue toutes les applications lorsqu’elles s’exécutent, une entrée est écrite dans le fichier journal *NameLogFile* chaque fois que l’application est exécutée.

3.  Évaluer le fichier journal

    Chaque entrée de journal, les États :

    -   l’appelant de la stratégie de restriction logicielle et l’ID de processus (PID) du processus appelant

    -   la cible en cours d’évaluation

    -   la règle de stratégie de restriction logicielle qui s’est produite lors de l’exécution de cette application

    -   un identificateur pour la règle de stratégie de restriction logicielle.

    Un exemple de la sortie écrite dans un fichier journal :

**Explorer.exe (PID = 4728) identifiedC:\Windows\system32\onenote.exe en tant que règle usingpath sans restriction, Guid = {320bd852-aa7c-4674-82c5-9a80321670a3}** toutes les applications et le code associé qui recherche de stratégies de restriction logicielle et configuré pour bloquer seront indiqués dans le journal fichier, vous pouvez ensuite utiliser pour déterminer les exécutables doit être considérée pour votre liste autorisées.


