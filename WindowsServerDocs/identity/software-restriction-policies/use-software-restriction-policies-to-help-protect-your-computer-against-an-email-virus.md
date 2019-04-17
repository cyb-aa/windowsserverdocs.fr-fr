---
title: "Utiliser des stratégies de Restriction logicielle pour protéger votre ordinateur contre un Virus de courrier électronique"
description: "Sécurité de Windows Server"
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: security-software-restriction-policies
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 02f23979-f832-4e46-bdea-21fd77db35b2
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: 41b4c2399a86ef96d34b62295eda4a1ce9300609
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="use-software-restriction-policies-to-help-protect-your-computer-against-an-email-virus"></a>Utiliser des stratégies de Restriction logicielle pour protéger votre ordinateur contre un Virus de courrier électronique

>S’applique à: Windows Server2016, Windows Server2012R2, Windows Server2012

Cette rubrique fournit des informations de définition de contrôle de l’application des stratégies à l’aide de stratégies de Restriction logicielle (SRP) pour aider à protéger votre ordinateur contre un courrier électronique virus à compter de Windows Server 2008 et Windows Vista.

## <a name="introduction"></a>Introduction
Stratégies de Restriction logicielle (SRP) est la fonctionnalité de stratégie de groupe qui identifie les programmes logiciels s’exécutant sur des ordinateurs dans un domaine et contrôle la capacité de ces programmes à exécuter. Vous utilisez des stratégies de restriction logicielle pour créer une configuration fortement restreinte pour vos ordinateurs, dans laquelle vous uniquement spécifiquement les applications identifiées est autorisée à s’exécuter. Elles sont intégrées aux Services de domaine Microsoft Active Directory et la stratégie de groupe, mais peuvent également être configurés sur des ordinateurs autonomes. Pour un point de départ pour les stratégies de restriction logicielle, consultez le [stratégies de Restriction logicielle](software-restriction-policies.md).

À partir de Windows Server 2008 R2 et Windows 7, Windows AppLocker peut servir à la place ou conjointement avec les stratégies de restriction logicielle pour une partie de votre stratégie de contrôle d’application. 

#### <a name="configure-srp-to-help-protect-against-an-e-mail-virus"></a>Configurer des stratégies de restriction logicielle pour vous protéger contre un virus de courrier électronique

1.  Passez en revue les meilleures pratiques pour les stratégies de restriction logicielle comprendre le fonctionnement des stratégies de restriction logicielle.

    -   [Meilleures pratiques](software-restriction-policies-technical-overview.md#BKMK_Best_Practices)

    -   [Fonctionnement des stratégies de Restriction logicielle](https://technet.microsoft.com/library/cc786941(v=WS.10).aspx)

2.  Ouvrez stratégies de Restriction logicielle.

    -   [Pour votre ordinateur local](administer-software-restriction-policies.md#BKMK_1)

    -   [Pour un domaine, site, ou unité d’organisation et lorsque vous êtes sur un serveur membre ou sur une station de travail qui est joint à un domaine](administer-software-restriction-policies.md#BKMK_2)

3.  Si vous n’avez pas précédemment défini des stratégies de restriction logicielle, créer des stratégies de restriction logicielle.

    -   [Pour créer des stratégies de restriction logicielle](administer-software-restriction-policies.md#BKMK_Create_SRP)

4.  Créer une règle de chemin d’accès pour le dossier de votre programme de messagerie utilise pour exécuter les pièces jointes de courrier électronique, puis définir la sécurité au niveau de **non autorisé**.

    -   [Utilisation des règles de chemin d’accès](work-with-software-restriction-policies-rules.md#BKMK_Path_Rules)

5.  Spécifier les types de fichier à laquelle la règle s’applique.

    -   [Pour ajouter ou supprimer un type de fichier désigné](administer-software-restriction-policies.md#BKMK_Add_Del)

6.  Modifier les paramètres de stratégie afin qu’elles s’appliquent aux utilisateurs et groupes que vous souhaitez:

    -   Spécifier les utilisateurs ou groupes pour lesquels vous ne souhaitez pas que l’objet de stratégie de groupe (GPO) pour appliquer des paramètres de stratégie.

    -   Exclure les stratégies de restriction logicielle d’un paramètre de stratégie spécifique dans la stratégie de groupe des administrateurs locaux et toujours le reste de la stratégie de groupe s’appliquent aux administrateurs.

        -   [Pour empêcher l’application aux administrateurs locaux des stratégies de restriction logicielle](administer-software-restriction-policies.md#BKMK_Prevent_Admin)

7.  Tester la stratégie.


