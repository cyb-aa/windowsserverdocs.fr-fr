---
title: Administrer les stratégies de restriction logicielle
description: Sécurité de Windows Server
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: security-software-restriction-policies
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 8cc22093-67d1-47b6-9ddd-4569b6761ce9
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: c75c7813041870f79ed95250857a5c7d1576c7dc
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71407182"
---
# <a name="administer-software-restriction-policies"></a>Administrer les stratégies de restriction logicielle

>S'applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Cette rubrique destinée aux professionnels de l’informatique contient des procédures permettant d’administrer des stratégies de contrôle d’application à l’aide de stratégies de restriction logicielle (SRP) à partir de Windows Server 2008 et Windows Vista.

## <a name="introduction"></a>Présentation
La fonctionnalité Stratégies de restriction logicielle est une fonctionnalité fondée sur les stratégies de groupe qui identifie les programmes logiciels s’exécutant sur les ordinateurs d’un domaine et qui contrôle la capacité de ces programmes à s’exécuter. Les stratégies de restriction logicielle peuvent aussi contribuer à créer une configuration fortement restreinte pour vos ordinateurs, dans laquelle seule l’exécution d’applications clairement identifiées est autorisée. Celles-ci sont intégrées à Microsoft Active Directory Domain Services et stratégie de groupe mais peuvent également être configurées sur des ordinateurs autonomes. Pour plus d’informations sur les SRP, consultez [stratégies de restriction logicielle](software-restriction-policies.md).

À compter de Windows Server 2008 R2 et Windows 7, il est possible d’utiliser Windows AppLocker à la place ou en collaboration avec les stratégies de restriction logicielle pour une partie de votre stratégie de contrôle des applications.

Cette rubrique contient les éléments suivants :

-   [Pour ouvrir des stratégies de restriction logicielle](#BKMK_Open_SRP)

-   [Pour créer des stratégies de restriction logicielle](#BKMK_Create_SRP)

-   [Pour ajouter ou supprimer un type de fichier désigné](#BKMK_Add_Del)

-   [Pour empêcher l’application de stratégies de restriction logicielle aux administrateurs locaux](#BKMK_Prevent_Admin)

-   [Pour modifier le niveau de sécurité par défaut des stratégies de restriction logicielle](#BKMK_Sec_Lvl)

-   [Pour appliquer des stratégies de restriction logicielle aux dll](#BKMK_Apply_SRP_DLLs)

Pour plus d’informations sur la façon d’accomplir des tâches spécifiques à l’aide de SRP, consultez les rubriques suivantes :

-   [Déterminer l’inventaire des applications et de la liste verte-refuser pour les stratégies de restriction logicielle](determine-allow-deny-list-and-application-inventory-for-software-restriction-policies.md)

-   [Utiliser des règles de stratégies de restriction logicielle](work-with-software-restriction-policies-rules.md)

-   [Utiliser des stratégies de restriction logicielle pour aider à protéger votre ordinateur contre un virus de courrier électronique](use-software-restriction-policies-to-help-protect-your-computer-against-an-email-virus.md)

## <a name="BKMK_Open_SRP"></a>Pour ouvrir des stratégies de restriction logicielle

-   [Pour votre ordinateur local](#BKMK_1)

-   [Pour un domaine, un site ou une unité d’organisation, et vous êtes sur un serveur membre ou sur une station de travail jointe à un domaine](#BKMK_2)

-   [Pour un domaine ou une unité d’organisation, et vous êtes sur un contrôleur de domaine ou sur une station de travail sur laquelle le Outils d’administration de serveur distant installé](#BKMK_3)

-   [Pour un site, et vous êtes sur un contrôleur de domaine ou sur une station de travail sur laquelle le Outils d’administration de serveur distant installé](#BKMK_4)

### <a name="BKMK_1"></a>Pour votre ordinateur local

1.  Ouvrez Paramètres de sécurité locaux.

2.  Dans l’arborescence de la console, cliquez sur **stratégies de restriction logicielle**.

    **Cela?**

    -   Paramètres de sécurité/stratégies de restriction logicielle

> [!NOTE]
> Pour effectuer cette procédure, vous devez être membre du groupe Administrateurs sur l’ordinateur local, ou l’autorité appropriée doit vous avoir été déléguée.

### <a name="BKMK_2"></a>Pour un domaine, un site ou une unité d’organisation, et vous êtes sur un serveur membre ou sur une station de travail jointe à un domaine

1.  Ouvrez la console MMC.

2.  Dans le menu **Fichier**, cliquez sur **Ajouter/Supprimer un composant logiciel enfichable**, puis sur **Ajouter**.

3.  Cliquez sur **Éditeur d’objet de stratégie de groupe locale.** , puis sur **Ajouter**.

4.  Dans **Sélectionner un objet de stratégie de groupe**, cliquez sur **Parcourir**.

5.  Dans **Rechercher un objet stratégie de groupe**, sélectionnez un objet stratégie de groupe (GPO) dans le domaine, le site ou l’unité d’organisation appropriés, ou créez-en un nouveau, puis cliquez sur **Terminer**.

6.  Cliquez sur **Fermer**, puis sur **OK**.

7.  Dans l’arborescence de la console, cliquez sur **stratégies de restriction logicielle**.

    **Cela?**

    -   *Stratégie de groupe objet* [*ComputerName*] stratégie/Configuration de l’ordinateur ou

        Configuration utilisateur/Paramètres Windows/paramètres de sécurité/stratégies de restriction logicielle

> [!NOTE]
> Pour effectuer cette procédure, vous devez être membre du groupe Admins du domaine.

### <a name="BKMK_3"></a>Pour un domaine ou une unité d’organisation, et vous êtes sur un contrôleur de domaine ou sur une station de travail sur laquelle le Outils d’administration de serveur distant installé

1.  Ouvrez Console de gestion des stratégies de groupe.

2.  Dans l’arborescence de la console, cliquez avec le bouton droit sur l’objet de stratégie de groupe (objet de stratégie de groupe) pour lequel vous souhaitez ouvrir des stratégies de restriction logicielle.

3.  Cliquez sur **Modifier** pour ouvrir l’objet de stratégie de groupe à modifier. Vous pouvez également cliquer sur **Nouveau** pour créer un objet de stratégie de groupe, puis cliquez sur **Modifier**.

4.  Dans l’arborescence de la console, cliquez sur **stratégies de restriction logicielle**.

    **Cela?**

    -   *Stratégie de groupe objet* [*ComputerName*] stratégie/Configuration de l’ordinateur ou

        Configuration utilisateur/Paramètres Windows/paramètres de sécurité/stratégies de restriction logicielle

> [!NOTE]
> Pour effectuer cette procédure, vous devez être membre du groupe Admins du domaine.

### <a name="BKMK_4"></a>Pour un site, et vous êtes sur un contrôleur de domaine ou sur une station de travail sur laquelle le Outils d’administration de serveur distant installé

1.  Ouvrez Console de gestion des stratégies de groupe.

2.  Dans l’arborescence de la console, cliquez avec le bouton droit sur le site pour lequel vous souhaitez définir des stratégie de groupe.

    **Cela?**

    -   Sites et services Active Directory [*Domain_Controller_Name. nom_domaine*]/sites/site

3.  Cliquez sur une entrée dans **stratégie de groupe des liens vers des objets** pour sélectionner un objet stratégie de groupe existant (GPO), puis cliquez sur **modifier**. Vous pouvez également cliquer sur **Nouveau** pour créer un objet de stratégie de groupe, puis cliquez sur **Modifier**.

4.  Dans l’arborescence de la console, cliquez sur **stratégies de restriction logicielle**.

    **Cela**

    -   *Stratégie de groupe objet* [*ComputerName*] stratégie/Configuration de l’ordinateur ou

        Configuration utilisateur/Paramètres Windows/paramètres de sécurité/stratégies de restriction logicielle

> [!NOTE]
> -   Pour effectuer cette procédure, vous devez être membre du groupe Administrateurs sur l’ordinateur local, ou l’autorité appropriée doit vous avoir été déléguée. Si l'ordinateur est joint à un domaine, les membres du groupe Admins du domaine peuvent être en mesure d'effectuer cette procédure.
> -   Pour définir les paramètres de stratégie qui seront appliqués aux ordinateurs, quels que soient les utilisateurs qui se connectent à ceux-ci, cliquez sur Configuration de l' **ordinateur**.
> -   Pour définir les paramètres de stratégie qui seront appliqués aux utilisateurs, quel que soit l’ordinateur sur lequel ils se connectent, cliquez sur Configuration de l' **utilisateur**.

## <a name="BKMK_Create_SRP"></a>Pour créer des stratégies de restriction logicielle

1.  Ouvrez Stratégies de restriction logicielle.

2.  Dans le menu **Action**, cliquez sur **Nouvelles stratégies de restriction logicielle**.

> [!WARNING]
> -   Différentes informations d’identification d’administration sont requises pour exécuter cette procédure, en fonction de votre environnement :
> 
>     -   Si vous créez des stratégies de restriction logicielle pour votre ordinateur local : L'appartenance au groupe local **Administrateurs**, ou équivalent, est la condition minimale requise pour effectuer cette procédure.
>     -   Si vous créez des stratégies de restriction logicielle pour un ordinateur joint à un domaine, les membres du groupe Administrateurs du domaine peuvent exécuter cette procédure.
> -   Si des stratégies de restriction logicielle ont déjà été créés pour un objet de stratégie de groupe, la commande **Nouvelles stratégies de restriction logicielle** ne s’affiche pas dans le menu **Action**. Pour supprimer les stratégies de restriction logicielle appliquées à un objet de stratégie de groupe, dans l’arborescence de la console, cliquez avec le bouton droit sur **Stratégies de restriction logicielle**, puis cliquez sur **Supprimer les stratégies de restriction logicielle**. Lorsque vous supprimez des stratégies de restriction logicielle pour un objet de stratégie de groupe, vous supprimez également toutes les règles de stratégies de restriction logicielle pour cet objet de stratégie de groupe. Une fois les stratégies de restriction logicielle supprimées, vous pouvez en créer de nouvelles pour cet objet de stratégie de groupe.

## <a name="BKMK_Add_Del"></a>Pour ajouter ou supprimer un type de fichier désigné

1.  Ouvrez Stratégies de restriction logicielle.

2.  Dans le volet d’informations, double-cliquez sur **Types de fichiers désignés**.

3.  Faites une des actions suivantes :

    -   Pour ajouter un type de fichier, dans **Extension de nom de fichier**, tapez l’extension de nom de fichier, puis cliquez sur **Ajouter**.

    -   Pour supprimer un type de fichier, dans **Types de fichiers désignés**, cliquez sur le type de fichier, puis cliquez sur **Supprimer**.

> [!NOTE]
> -   Différentes informations d’identification d’administration sont requises pour exécuter cette procédure, en fonction de l’environnement dans lequel vous ajoutez ou supprimez un type de fichier désigné :
> 
>     -   Si vous ajoutez ou supprimez un type de fichier désigné pour votre ordinateur local : L'appartenance au groupe local **Administrateurs**, ou équivalent, est la condition minimale requise pour effectuer cette procédure.
>     -   Si vous créez des stratégies de restriction logicielle pour un ordinateur joint à un domaine, les membres du groupe Administrateurs du domaine peuvent exécuter cette procédure.
> -   Il peut être nécessaire de créer un paramètre de stratégie de restriction logicielle pour l’objet de stratégie de groupe si cela n’est pas encore fait.
> -   La liste des types de fichiers désignés est partagée par toutes les règles pour la configuration de l’ordinateur et la configuration de l’utilisateur pour un objet de stratégie de groupe.

## <a name="BKMK_Prevent_Admin"></a>Pour empêcher l’application de stratégies de restriction logicielle aux administrateurs locaux

1.  Ouvrez Stratégies de restriction logicielle.

2.  Dans le volet d’informations, double-cliquez sur **Mise en œuvre**.

3.  Sous **Appliquer les stratégies de restriction logicielle aux utilisateurs suivants**, cliquez sur **Tous les utilisateurs exceptés les administrateurs locaux**.

> [!WARNING]
> -   L'appartenance au groupe local **Administrateurs**, ou équivalent, est la condition minimale requise pour effectuer cette procédure.
> -   Il peut être nécessaire de créer un paramètre de stratégie de restriction logicielle pour l’objet de stratégie de groupe si cela n’est pas encore fait.
> -   Si la plupart des utilisateurs de votre organisation sont membres du groupe local Administrateurs sur leur ordinateur, il est préférable de ne pas activer cette option.
> -   Si vous définissez une stratégie de restriction logicielle pour votre ordinateur local, utilisez cette procédure pour empêcher l’application de ces stratégies aux administrateurs locaux. Si vous définissez un paramètre de stratégie de restriction logicielle pour votre réseau, filtrez les paramètres de stratégie d’utilisateur en fonction de l’appartenance aux groupes de sécurité via stratégie de groupe.

## <a name="BKMK_Sec_Lvl"></a>Pour modifier le niveau de sécurité par défaut des stratégies de restriction logicielle

1.  Ouvrez Stratégies de restriction logicielle.

2.  Dans le volet d’informations, double-cliquez sur **Niveaux de sécurité**.

3.  Cliquez avec le bouton droit sur le niveau de sécurité à définir par défaut, puis cliquez sur **Par défaut**.

> [!CAUTION]
> Dans certains répertoires, la définition du niveau de sécurité par défaut à **Non autorisé** peut avoir un effet néfaste sur votre système d’exploitation.

> [!NOTE]
> -   Différentes informations d’identification d’administration sont requises pour exécuter cette procédure, en fonction de l’environnement pour lequel vous modifiez le niveau de sécurité par défaut des stratégies de restriction logicielle :
> -   Il peut être nécessaire de créer un paramètre de stratégie de restriction logicielle pour cet objet de stratégie de groupe si cela n’est pas encore fait.
> -   Dans le volet d’informations, le niveau de sécurité par défaut actuel est indiqué par un cercle noir avec une coche. Si vous cliquez avec le bouton droit sur le niveau de sécurité par défaut actuel, la commande **Par défaut** ne s’affiche pas dans le menu.
> -   Des règles de stratégies de restriction logicielle sont créées pour spécifier des exceptions au niveau de sécurité par défaut. Quand le niveau de sécurité par défaut est **Non restreint**, les règles peuvent indiquer les logiciels dont l’exécution n’est pas autorisée. Quand le niveau de sécurité par défaut est **Non autorisé**, les règles peuvent indiquer les logiciels dont l’exécution est autorisée.
> -   Au moment de l’installation, le niveau de sécurité par défaut des stratégies de restriction logicielle sur tous les fichiers du système est défini à **Non restreint**.

## <a name="BKMK_Apply_SRP_DLLs"></a>Pour appliquer des stratégies de restriction logicielle aux dll

1.  Ouvrez Stratégies de restriction logicielle.

2.  Dans le volet d’informations, double-cliquez sur **Mise en œuvre**.

3.  Sous **Appliquer les stratégies de restriction logicielle aux fichiers suivants**, cliquez sur **Tous les fichiers de logiciel**.

> [!NOTE]
> -   Pour effectuer cette procédure, vous devez être membre du groupe Administrateurs sur l’ordinateur local, ou l’autorité appropriée doit vous avoir été déléguée. Si l'ordinateur est joint à un domaine, les membres du groupe Admins du domaine peuvent être en mesure d'effectuer cette procédure.
> -   Par défaut, les stratégies de restriction logicielle ne vérifient pas les DLL. La vérification des DLL peut impacter les performances du système, car les stratégies de restriction logicielle doivent être évaluées chaque fois qu’une DLL est chargée. Cependant, vous pouvez choisir de vérifier les DLL si vous avez peur de recevoir un virus qui cible les DLL. Si le niveau de sécurité par défaut est défini sur non **autorisé**, et que vous activez la vérification de la dll, vous devez créer des règles de stratégies de restriction logicielle qui autorisent l’exécution de chaque dll.


