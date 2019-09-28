---
title: Utiliser les stratégies de restriction logicielle pour protéger votre ordinateur contre un virus de courrier électronique
description: Sécurité de Windows Server
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 4c691255683eb37eecdbeaa55c094b7ce5c4e26d
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71357650"
---
# <a name="use-software-restriction-policies-to-help-protect-your-computer-against-an-email-virus"></a>Utiliser les stratégies de restriction logicielle pour protéger votre ordinateur contre un virus de courrier électronique

>S'applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Cette rubrique fournit des informations sur la façon de définir des stratégies de contrôle d’application à l’aide de stratégies de restriction logicielle (SRP) pour protéger votre ordinateur contre les virus du courrier électronique à partir de Windows Server 2008 et Windows Vista.

## <a name="introduction"></a>Présentation
La fonctionnalité Stratégies de restriction logicielle est une fonctionnalité fondée sur les stratégies de groupe qui identifie les programmes logiciels s’exécutant sur les ordinateurs d’un domaine et qui contrôle la capacité de ces programmes à s’exécuter. Les stratégies de restriction logicielle peuvent aussi contribuer à créer une configuration fortement restreinte pour vos ordinateurs, dans laquelle seule l’exécution d’applications clairement identifiées est autorisée. Celles-ci sont intégrées à Microsoft Active Directory Domain Services et stratégie de groupe mais peuvent également être configurées sur des ordinateurs autonomes. Pour un point de départ pour les SRP, consultez [stratégies de restriction logicielle](software-restriction-policies.md).

À compter de Windows Server 2008 R2 et Windows 7, il est possible d’utiliser Windows AppLocker à la place ou en collaboration avec les stratégies de restriction logicielle pour une partie de votre stratégie de contrôle des applications. 

#### <a name="configure-srp-to-help-protect-against-an-e-mail-virus"></a>Configurer des stratégies de restriction logicielle pour vous protéger contre un virus de courrier électronique

1.  Passez en revue les meilleures pratiques pour les stratégies de restriction logicielle pour comprendre le fonctionnement du SRP.

    -   [Bonnes pratiques](software-restriction-policies-technical-overview.md#BKMK_Best_Practices)

    -   [Fonctionnement des stratégies de restriction logicielle](https://technet.microsoft.com/library/cc786941(v=WS.10).aspx)

2.  Ouvrez Stratégies de restriction logicielle.

    -   [Pour votre ordinateur local](administer-software-restriction-policies.md#BKMK_1)

    -   [Pour un domaine, un site ou une unité d’organisation, et vous êtes sur un serveur membre ou sur une station de travail jointe à un domaine](administer-software-restriction-policies.md#BKMK_2)

3.  Si vous n’avez pas encore défini de stratégies de restriction logicielle, créez de nouvelles stratégies de restriction logicielle.

    -   [Pour créer des stratégies de restriction logicielle](administer-software-restriction-policies.md#BKMK_Create_SRP)

4.  Créez une règle de chemin d’accès pour le dossier que votre programme de messagerie utilise pour exécuter les pièces jointes, puis définissez le niveau de sécurité sur non **autorisé**.

    -   [Utilisation des règles de chemin d’accès](work-with-software-restriction-policies-rules.md#BKMK_Path_Rules)

5.  Spécifiez les types de fichiers auxquels la règle s’applique.

    -   [Pour ajouter ou supprimer un type de fichier désigné](administer-software-restriction-policies.md#BKMK_Add_Del)

6.  Modifiez les paramètres de stratégie afin qu’ils s’appliquent aux utilisateurs et aux groupes que vous souhaitez :

    -   Spécifiez les utilisateurs ou les groupes auxquels vous ne voulez pas appliquer les paramètres de stratégie de l’objet stratégie de groupe.

    -   Excluez les administrateurs locaux des stratégies de restriction logicielle d’un paramètre de stratégie spécifique dans stratégie de groupe tout en gardant le reste de stratégie de groupe s’appliquer aux administrateurs.

        -   [Pour empêcher l’application de stratégies de restriction logicielle aux administrateurs locaux](administer-software-restriction-policies.md#BKMK_Prevent_Admin)

7.  Testez la stratégie.


