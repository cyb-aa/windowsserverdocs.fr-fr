---
title: "Déterminer la liste d’autorisation / exclusion et l’inventaire des applications pour les stratégies de Restriction logicielle"
description: "Sécurité de Windows Server"
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
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="determine-allow-deny-list-and-application-inventory-for-software-restriction-policies"></a>Déterminer la liste d’autorisation / exclusion et l’inventaire des applications pour les stratégies de Restriction logicielle

>S’applique à: Windows Server2016, Windows Server2012R2, Windows Server2012

Cette rubrique destinée aux professionnels de l’informatique fournit des conseils comment créer un autoriser et refuser la liste des applications qui seront gérés par les stratégies de Restriction logicielle (SRP) à compter de Windows Server2008 et WindowsVista.

## <a name="introduction"></a>Introduction
Stratégies de Restriction logicielle (SRP) est la fonctionnalité de stratégie de groupe qui identifie les programmes logiciels s’exécutant sur des ordinateurs dans un domaine et contrôle la capacité de ces programmes à exécuter. Vous utilisez des stratégies de restriction logicielle pour créer une configuration fortement restreinte pour vos ordinateurs, dans laquelle vous uniquement spécifiquement les applications identifiées est autorisée à s’exécuter. Elles sont intégrées aux Services de domaine Microsoft Active Directory et la stratégie de groupe, mais peuvent également être configurés sur des ordinateurs autonomes. Pour un point de départ pour les stratégies de restriction logicielle, consultez le [stratégies de Restriction logicielle](software-restriction-policies.md).

À partir de Windows Server 2008 R2 et Windows 7, Windows AppLocker peut servir à la place ou conjointement avec les stratégies de restriction logicielle pour une partie de votre stratégie de contrôle d’application.

Pour plus d’informations sur la façon d’accomplir des tâches spécifiques à l’aide de stratégies de restriction logicielle, consultez les rubriques suivantes:

-   [Travailler avec des règles de stratégies de Restriction logicielle](work-with-software-restriction-policies-rules.md)

-   [Utiliser des stratégies de Restriction logicielle pour protéger votre ordinateur contre un Virus de courrier électronique](use-software-restriction-policies-to-help-protect-your-computer-against-an-email-virus.md)

### <a name="what-default-rule-to-choose-allow-or-deny"></a>Les règles par défaut pour choisir: autoriser ou refuser
Stratégies de restriction logicielle peuvent être déployés dans un des deux modes sont la base de votre règle par défaut: liste Autoriser ou refuser. Vous pouvez créer une stratégie qui identifie chaque application qui est autorisée à s’exécuter dans votre environnement; la règle par défaut au sein de votre stratégie est restreint et bloquer toutes les applications que vous n’autorisez pas explicitement à exécuter. Ou vous pouvez créer une stratégie qui identifie chaque application qui ne peut pas s’exécuter; la règle par défaut est non restreint et restreint seulement les applications que vous avez répertoriées explicitement.

> [!IMPORTANT]
> Le mode de liste d’exclusion peut être une stratégie de maintenance élevée pour votre organisation en matière de contrôle de l’application. Création et la maintenance d’une liste en constante évolution qui interdit à tous les programmes malveillants et autres applications problématiques serait beaucoup de temps et sensible aux erreurs.

### <a name="create-an-inventory-of-your-applications-for-the-allow-list"></a>Créer un inventaire de vos applications pour la liste verte
Pour utiliser efficacement la règle d’autorisation par défaut, vous devez déterminer exactement quelles applications sont nécessaires dans votre organisation. Il existe des outils conçus pour produire un inventaire des applications, telles que le collecteur d’inventaire de MicrosoftApplication Compatibility Toolkit. Mais les stratégies de restriction logicielle dispose d’une fonctionnalité de journalisation avancée pour vous aider à comprendre exactement ce que les applications sont en cours d’exécution dans votre environnement.

##### <a name="to-discover-which-applications-to-allow"></a>Pour découvrir les applications à autoriser

1.  Dans un environnement de test, déployer la stratégie de Restriction logicielle avec l’ensemble de règles par défaut non restreint et supprimer des règles supplémentaires. Si vous activez des stratégies de restriction logicielle sans forcer afin de limiter les applications, SPR sera en mesure d’analyser les applications sont en cours d’exécution.

2.  Créez la valeur de Registre suivante afin d’activer la fonctionnalité d’enregistrement avancées et de définir le chemin d’accès où le fichier journal doit être écrits.

    **«HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows\Safer\ CodeIdentifiers»**

    Valeur de chaîne: *NameLogFile NameLogFile chemin d’accès*

    Étant donné que les stratégies de restriction logicielle évalue toutes les applications lorsqu’ils exécutent, une entrée est écrite dans le fichier journal *NameLogFile* chaque fois que l’application est exécutée.

3.  Évaluer le fichier journal

    Chaque entrée de journal, les États:

    -   l’appelant de la stratégie de restriction logicielle et l’ID de processus (PID) du processus appelant

    -   La cible en cours d’évaluation

    -   La règle de restriction logicielle s’est produite lors de l’exécution de cette application

    -   Identificateur de la règle de restriction logicielle.

    Un exemple de sortie dans un fichier journal:

**Explorer.exe (PID = 4728) identifiedC:\Windows\system32\onenote.exe en tant que règle usingpath illimité, Guid = {320bd852-aa7c-4674-82c5-9a80321670a3}** toutes les applications et le code associé qui vérifie de stratégies de restriction logicielle et configuré pour bloquer seront indiqués dans le fichier journal, que vous pouvez ensuite utiliser pour déterminer quels fichiers exécutables doit être envisagée pour votre liste autorisée.


