---
title: Administrer les stratégies de restriction logicielle
description: Sécurité de Windows Server
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 67ef99c95f10585ab72dee4bdf6512e715d13f4a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59863590"
---
# <a name="administer-software-restriction-policies"></a>Administrer les stratégies de restriction logicielle

>S'applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Cette rubrique destinée aux professionnels de l’informatique contient des procédures comment administrer les stratégies de contrôle d’application à l’aide de début de stratégies de Restriction logicielle (SRP) avec Windows Server 2008 et Windows Vista.

## <a name="introduction"></a>Introduction
La fonctionnalité Stratégies de restriction logicielle est une fonctionnalité fondée sur les stratégies de groupe qui identifie les programmes logiciels s’exécutant sur les ordinateurs d’un domaine et qui contrôle la capacité de ces programmes à s’exécuter. Les stratégies de restriction logicielle peuvent aussi contribuer à créer une configuration fortement restreinte pour vos ordinateurs, dans laquelle seule l’exécution d’applications clairement identifiées est autorisée. Elles sont intégrées aux Services de domaine Microsoft Active Directory et stratégie de groupe, mais peuvent également être configurés sur des ordinateurs autonomes. Pour plus d’informations sur les stratégies de restriction logicielle, consultez le [stratégies de Restriction logicielle](software-restriction-policies.md).

Depuis Windows Server 2008 R2 et Windows 7, Windows AppLocker peut servir à la place d’ou de concert avec les stratégies de restriction logicielle pour une partie de votre stratégie de contrôle d’application.

Cette rubrique contient :

-   [Pour ouvrir des stratégies de Restriction logicielle](#BKMK_Open_SRP)

-   [Pour créer des stratégies de restriction logicielle](#BKMK_Create_SRP)

-   [Pour ajouter ou supprimer un type de fichier désigné](#BKMK_Add_Del)

-   [Pour empêcher les stratégies de restriction logicielle de s’appliquer aux administrateurs locaux](#BKMK_Prevent_Admin)

-   [Pour modifier le niveau de sécurité par défaut des stratégies de restriction logicielle](#BKMK_Sec_Lvl)

-   [Pour appliquer des stratégies de restriction logicielle aux DLL](#BKMK_Apply_SRP_DLLs)

Pour plus d’informations sur la façon d’accomplir des tâches spécifiques à l’aide de stratégies de restriction logicielle, consultez les rubriques suivantes :

-   [Déterminer la liste d’autorisation / exclusion et l’inventaire des applications pour les stratégies de Restriction logicielle](determine-allow-deny-list-and-application-inventory-for-software-restriction-policies.md)

-   [Travailler avec des règles de stratégies de Restriction logicielle](work-with-software-restriction-policies-rules.md)

-   [Utiliser des stratégies de Restriction logicielle pour vous aider à protéger votre ordinateur contre un Virus de courrier électronique](use-software-restriction-policies-to-help-protect-your-computer-against-an-email-virus.md)

## <a name="BKMK_Open_SRP"></a>Pour ouvrir des stratégies de Restriction logicielle

-   [Pour votre ordinateur local](#BKMK_1)

-   [Pour un domaine, site, ou unité d’organisation, vous êtes sur un serveur membre ou une station de travail qui est joint à un domaine](#BKMK_2)

-   [Pour un domaine ou unité d’organisation et vous sont sur un contrôleur de domaine ou sur une station de travail qui a les outils d’Administration de serveur distant installé](#BKMK_3)

-   [Pour un site, vous vous trouvez sur un contrôleur de domaine ou sur une station de travail qui a les outils d’Administration de serveur distant installé](#BKMK_4)

### <a name="BKMK_1"></a>Pour votre ordinateur local

1.  Ouvrez Paramètres de sécurité locaux.

2.  Dans l’arborescence de la console, cliquez sur **stratégies de Restriction logicielle**.

    **Où ?**

    -   Stratégies de Restriction de paramètres/logiciels de sécurité

> [!NOTE]
> Pour effectuer cette procédure, vous devez être membre du groupe Administrateurs sur l’ordinateur local, ou l’autorité appropriée doit vous avoir été déléguée.

### <a name="BKMK_2"></a>Pour un domaine, site, ou unité d’organisation, vous êtes sur un serveur membre ou une station de travail qui est joint à un domaine

1.  Ouvrez la console MMC.

2.  Dans le menu **Fichier**, cliquez sur **Ajouter/Supprimer un composant logiciel enfichable**, puis sur **Ajouter**.

3.  Cliquez sur **Éditeur d’objet de stratégie de groupe locale.**, puis sur **Ajouter**.

4.  Dans **Sélectionner un objet de stratégie de groupe**, cliquez sur **Parcourir**.

5.  Dans **rechercher un objet de stratégie de groupe**, sélectionnez un objet de stratégie de groupe (GPO) dans le domaine approprié, un site ou une unité d’organisation- ou créez-en un, puis cliquez sur **Terminer**.

6.  Cliquez sur **Fermer**, puis sur **OK**.

7.  Dans l’arborescence de la console, cliquez sur **stratégies de Restriction logicielle**.

    **Où ?**

    -   *Objet de stratégie de groupe* [*Nom_Ordinateur*] Configuration de stratégie/ordinateur ou

        Stratégies de Restriction des paramètres/logiciel utilisateur Configuration/Windows/Paramètres de sécurité

> [!NOTE]
> Pour effectuer cette procédure, vous devez être membre du groupe Admins du domaine.

### <a name="BKMK_3"></a>Pour un domaine ou unité d’organisation et vous sont sur un contrôleur de domaine ou sur une station de travail qui a les outils d’Administration de serveur distant installé

1.  Ouvrez la Console de gestion de stratégie de groupe.

2.  Dans l’arborescence de la console, cliquez sur l’objet de stratégie de groupe (GPO) que vous souhaitez ouvrir des stratégies de restriction logicielle pour.

3.  Cliquez sur **Modifier** pour ouvrir l’objet de stratégie de groupe à modifier. Vous pouvez également cliquer sur **Nouveau** pour créer un objet de stratégie de groupe, puis cliquez sur **Modifier**.

4.  Dans l’arborescence de la console, cliquez sur **stratégies de Restriction logicielle**.

    **Où ?**

    -   *Objet de stratégie de groupe* [*Nom_Ordinateur*] Configuration de stratégie/ordinateur ou

        Stratégies de Restriction des paramètres/logiciel utilisateur Configuration/Windows/Paramètres de sécurité

> [!NOTE]
> Pour effectuer cette procédure, vous devez être membre du groupe Admins du domaine.

### <a name="BKMK_4"></a>Pour un site, vous vous trouvez sur un contrôleur de domaine ou sur une station de travail qui a les outils d’Administration de serveur distant installé

1.  Ouvrez la Console de gestion de stratégie de groupe.

2.  Dans l’arborescence de la console, cliquez sur le site que vous souhaitez définir la stratégie de groupe pour.

    **Où ?**

    -   Sites et Services Active Directory [*nom_contrôleur_domaine.nom_domaine*] / Sites/Site

3.  Cliquez sur une entrée dans **liaisons d’objet de stratégie de groupe** pour sélectionner un objet de stratégie de groupe (GPO) existant, puis cliquez sur **modifier**. Vous pouvez également cliquer sur **Nouveau** pour créer un objet de stratégie de groupe, puis cliquez sur **Modifier**.

4.  Dans l’arborescence de la console, cliquez sur **stratégies de Restriction logicielle**.

    **Où**

    -   *Objet de stratégie de groupe* [*Nom_Ordinateur*] Configuration de stratégie/ordinateur ou

        Stratégies de Restriction des paramètres/logiciel utilisateur Configuration/Windows/Paramètres de sécurité

> [!NOTE]
> -   Pour effectuer cette procédure, vous devez être membre du groupe Administrateurs sur l’ordinateur local, ou l’autorité appropriée doit vous avoir été déléguée. Si l'ordinateur est joint à un domaine, les membres du groupe Admins du domaine peuvent être en mesure d'effectuer cette procédure.
> -   Pour définir les paramètres de stratégie qui seront appliqués aux ordinateurs, quel que soit l’utilisateurs y ouvrent une session, cliquez sur **Configuration ordinateur**.
> -   Pour définir les paramètres de stratégie qui seront appliqués aux utilisateurs, quel que soit l’ordinateur sur lequel ils se connectent, cliquez sur **Configuration utilisateur**.

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
> -   La liste des types de fichiers désignés est partagée par toutes les règles de Configuration de l’ordinateur et Configuration de l’utilisateur pour un objet de stratégie de groupe.

## <a name="BKMK_Prevent_Admin"></a>Pour empêcher les stratégies de restriction logicielle de s’appliquer aux administrateurs locaux

1.  Ouvrez Stratégies de restriction logicielle.

2.  Dans le volet d’informations, double-cliquez sur **Mise en œuvre**.

3.  Sous **Appliquer les stratégies de restriction logicielle aux utilisateurs suivants**, cliquez sur **Tous les utilisateurs exceptés les administrateurs locaux**.

> [!WARNING]
> -   L'appartenance au groupe local **Administrateurs**, ou équivalent, est la condition minimale requise pour effectuer cette procédure.
> -   Il peut être nécessaire de créer un paramètre de stratégie de restriction logicielle pour l’objet de stratégie de groupe si cela n’est pas encore fait.
> -   Si la plupart des utilisateurs de votre organisation sont membres du groupe local Administrateurs sur leur ordinateur, il est préférable de ne pas activer cette option.
> -   Si vous définissez une stratégie de restriction logicielle pour votre ordinateur local, utilisez cette procédure pour empêcher l’application de ces stratégies aux administrateurs locaux. Si vous définissez un paramètre de stratégie de restriction logicielle pour votre réseau, filtrez les paramètres de stratégie utilisateur selon l’appartenance aux groupes de sécurité via la stratégie de groupe.

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
> -   Règles de stratégies de restriction logicielle sont créées pour spécifier les exceptions au niveau de sécurité par défaut. Quand le niveau de sécurité par défaut est **Non restreint**, les règles peuvent indiquer les logiciels dont l’exécution n’est pas autorisée. Quand le niveau de sécurité par défaut est **Non autorisé**, les règles peuvent indiquer les logiciels dont l’exécution est autorisée.
> -   Au moment de l’installation, le niveau de sécurité par défaut des stratégies de restriction logicielle sur tous les fichiers du système est défini à **Non restreint**.

## <a name="BKMK_Apply_SRP_DLLs"></a>Pour appliquer des stratégies de restriction logicielle aux DLL

1.  Ouvrez Stratégies de restriction logicielle.

2.  Dans le volet d’informations, double-cliquez sur **Mise en œuvre**.

3.  Sous **Appliquer les stratégies de restriction logicielle aux fichiers suivants**, cliquez sur **Tous les fichiers de logiciel**.

> [!NOTE]
> -   Pour effectuer cette procédure, vous devez être membre du groupe Administrateurs sur l’ordinateur local, ou l’autorité appropriée doit vous avoir été déléguée. Si l'ordinateur est joint à un domaine, les membres du groupe Admins du domaine peuvent être en mesure d'effectuer cette procédure.
> -   Par défaut, les stratégies de restriction logicielle ne vérifient pas les DLL. La vérification des DLL peut impacter les performances du système, car les stratégies de restriction logicielle doivent être évaluées chaque fois qu’une DLL est chargée. Cependant, vous pouvez choisir de vérifier les DLL si vous avez peur de recevoir un virus qui cible les DLL. Si le niveau de sécurité par défaut est défini sur **rejeté**, et que vous activez la vérification, vous devez créer de restriction logicielle des règles de stratégies qui permettent à chaque DLL exécuter des DLL.


