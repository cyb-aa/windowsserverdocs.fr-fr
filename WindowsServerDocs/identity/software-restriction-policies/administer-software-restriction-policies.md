---
title: "Administrer les stratégies de Restriction logicielle"
description: "Sécurité de Windows Server"
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
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2017
---
# <a name="administer-software-restriction-policies"></a>Administrer les stratégies de Restriction logicielle

>S’applique à: Windows Server2016, Windows Server2012R2, Windows Server2012

Cette rubrique destinée aux professionnels de l’informatique contient des procédures administrer des stratégies de contrôle d’application à l’aide de début des stratégies de Restriction logicielle (SRP) avec Windows Server 2008 et Windows Vista.

## <a name="introduction"></a>Introduction
Stratégies de Restriction logicielle (SRP) est la fonctionnalité de stratégie de groupe qui identifie les programmes logiciels s’exécutant sur des ordinateurs dans un domaine et contrôle la capacité de ces programmes à exécuter. Vous utilisez des stratégies de restriction logicielle pour créer une configuration fortement restreinte pour vos ordinateurs, dans laquelle vous uniquement spécifiquement les applications identifiées est autorisée à s’exécuter. Elles sont intégrées aux Services de domaine Microsoft Active Directory et la stratégie de groupe, mais peuvent également être configurés sur des ordinateurs autonomes. Pour plus d’informations sur les stratégies de restriction logicielle, consultez le [stratégies de Restriction logicielle](software-restriction-policies.md).

À partir de Windows Server 2008 R2 et Windows 7, Windows AppLocker peut servir à la place ou conjointement avec les stratégies de restriction logicielle pour une partie de votre stratégie de contrôle d’application.

Cette rubrique contient:

-   [Pour ouvrir des stratégies de Restriction logicielle](#BKMK_Open_SRP)

-   [Pour créer des stratégies de restriction logicielle](#BKMK_Create_SRP)

-   [Pour ajouter ou supprimer un type de fichier désigné](#BKMK_Add_Del)

-   [Pour empêcher l’application aux administrateurs locaux des stratégies de restriction logicielle](#BKMK_Prevent_Admin)

-   [Pour modifier le niveau de sécurité par défaut des stratégies de restriction logicielle](#BKMK_Sec_Lvl)

-   [Pour appliquer des stratégies de restriction logicielle aux DLL](#BKMK_Apply_SRP_DLLs)

Pour plus d’informations sur la façon d’accomplir des tâches spécifiques à l’aide de stratégies de restriction logicielle, consultez les rubriques suivantes:

-   [Déterminer la liste d’autorisation / exclusion et l’inventaire des applications pour les stratégies de Restriction logicielle](determine-allow-deny-list-and-application-inventory-for-software-restriction-policies.md)

-   [Travailler avec des règles de stratégies de Restriction logicielle](work-with-software-restriction-policies-rules.md)

-   [Utiliser des stratégies de Restriction logicielle pour protéger votre ordinateur contre un Virus de courrier électronique](use-software-restriction-policies-to-help-protect-your-computer-against-an-email-virus.md)

## <a name="BKMK_Open_SRP"></a>Pour ouvrir des stratégies de Restriction logicielle

-   [Pour votre ordinateur local](#BKMK_1)

-   [Pour un domaine, site, ou unité d’organisation et lorsque vous êtes sur un serveur membre ou sur une station de travail qui est joint à un domaine](#BKMK_2)

-   [Pour un domaine ou l’unité d’organisation et vous êtes sur un contrôleur de domaine ou sur une station de travail qui a les outils d’Administration de serveur distant installé](#BKMK_3)

-   [Pour un site, vous vous trouvez sur un contrôleur de domaine ou sur une station de travail qui a les outils d’Administration de serveur distant installé](#BKMK_4)

### <a name="BKMK_1"></a>Pour votre ordinateur local

1.  Ouvrir les paramètres de sécurité locale.

2.  Dans l’arborescence de la console, cliquez sur **stratégies de Restriction logicielle**.

    **Où?**

    -   Stratégies de Restriction logicielle/paramètres de sécurité

> [!NOTE]
> Pour effectuer cette procédure, vous devez être membre du groupe Administrateurs sur l’ordinateur local, ou doit vous avoir été déléguée l’autorité appropriée.

### <a name="BKMK_2"></a>Pour un domaine, site, ou unité d’organisation et lorsque vous êtes sur un serveur membre ou sur une station de travail qui est joint à un domaine

1.  Ouvrez Microsoft Management Console (MMC).

2.  Sur le **fichier** menu, cliquez sur **ajouter/supprimer un composant logiciel enfichable**, puis cliquez sur **ajouter**.

3.  Cliquez sur **Éditeur d’objets de stratégie de groupe Local**, puis cliquez sur **ajouter**.

4.  Dans **sélectionner un objet de stratégie de groupe**, cliquez sur **Parcourir**.

5.  Dans **rechercher un objet de stratégie de groupe**, sélectionnez un objet de stratégie de groupe (GPO) dans le domaine approprié, le site ou l’unité d’organisation- ou créer un nouveau, puis cliquez sur **Terminer**.

6.  Cliquez sur **fermer**, puis cliquez sur **OK**.

7.  Dans l’arborescence de la console, cliquez sur **stratégies de Restriction logicielle**.

    **Où?**

    -   *Objet de stratégie de groupe* [*Nom_Ordinateur*] Configuration de la stratégie ordinateur ou

        Stratégies de Restriction utilisateur Configuration/Windows/sécurité paramètres paramètres/logiciel

> [!NOTE]
> Pour effectuer cette procédure, vous devez être membre du groupe Admins du domaine.

### <a name="BKMK_3"></a>Pour un domaine ou l’unité d’organisation et vous êtes sur un contrôleur de domaine ou sur une station de travail qui a les outils d’Administration de serveur distant installé

1.  Ouvrez la Console de gestion de stratégie de groupe.

2.  Dans l’arborescence de la console, cliquez sur l’objet de stratégie de groupe (GPO) que vous souhaitez ouvrir des stratégies de restriction logicielle pour.

3.  Cliquez sur **modifier** pour ouvrir l’objet de stratégie de groupe que vous souhaitez modifier. Vous pouvez également cliquer sur **New** pour créer un nouvel objet GPO, puis cliquez sur **modifier**.

4.  Dans l’arborescence de la console, cliquez sur **stratégies de Restriction logicielle**.

    **Où?**

    -   *Objet de stratégie de groupe* [*Nom_Ordinateur*] Configuration de la stratégie ordinateur ou

        Stratégies de Restriction utilisateur Configuration/Windows/sécurité paramètres paramètres/logiciel

> [!NOTE]
> Pour effectuer cette procédure, vous devez être membre du groupe Admins du domaine.

### <a name="BKMK_4"></a>Pour un site, vous vous trouvez sur un contrôleur de domaine ou sur une station de travail qui a les outils d’Administration de serveur distant installé

1.  Ouvrez la Console de gestion de stratégie de groupe.

2.  Dans l’arborescence de la console, cliquez sur le site que vous souhaitez définir la stratégie de groupe.

    **Où?**

    -   Sites et Services Active Directory [*nom_contrôleur_domaine.nom_domaine*] / Sites/Site

3.  Cliquez sur une entrée dans **liaisons d’objet de stratégie de groupe** pour sélectionner un objet de stratégie de groupe (GPO) existant, puis cliquez sur **modifier**. Vous pouvez également cliquer sur **New** pour créer un nouvel objet GPO, puis cliquez sur **modifier**.

4.  Dans l’arborescence de la console, cliquez sur **stratégies de Restriction logicielle**.

    **Où**

    -   *Objet de stratégie de groupe* [*Nom_Ordinateur*] Configuration de la stratégie ordinateur ou

        Stratégies de Restriction utilisateur Configuration/Windows/sécurité paramètres paramètres/logiciel

> [!NOTE]
> -   Pour effectuer cette procédure, vous devez être membre du groupe Administrateurs sur l’ordinateur local, ou doit vous avoir été déléguée l’autorité appropriée. Si l’ordinateur est joint à un domaine, les membres du groupe Admins du domaine peuvent être en mesure d’effectuer cette procédure.
> -   Pour définir les paramètres de stratégie qui seront appliqués aux ordinateurs, quel que soit l’utilisateurs se connectent, cliquez sur **Configuration ordinateur**.
> -   Pour définir les paramètres de stratégie qui seront appliqués aux utilisateurs, quel que soit l’ordinateur sur lequel ils ouvrent une session sur, cliquez sur **Configuration utilisateur**.

## <a name="BKMK_Create_SRP"></a>Pour créer des stratégies de restriction logicielle

1.  Ouvrez stratégies de Restriction logicielle.

2.  Sur le **Action** menu, cliquez sur **des stratégies de Restriction logicielle**.

> [!WARNING]
> -   Différentes informations d’identification d’administration sont requis pour effectuer cette procédure, en fonction de votre environnement:
> 
>     -   Si vous créez des stratégies de restriction logicielle pour votre ordinateur local: l’appartenance au groupe **administrateurs** , ou équivalent, est la condition minimale requise pour effectuer cette procédure.
>     -   Si vous créez des stratégies de restriction logicielle pour un ordinateur qui est joint à un domaine, les membres du groupe Admins du domaine peuvent effectuer cette procédure.
> -   Si les stratégies de restriction logicielle ont déjà été créées pour un objet de stratégie de groupe (GPO), la **des stratégies de Restriction logicielle** commande n’apparaît pas sur le **Action** menu. Pour supprimer les stratégies de restriction logicielle sont appliquées à un objet stratégie de groupe, dans l’arborescence de la console, faites un clic droit **stratégies de Restriction logicielle**, puis cliquez sur **supprimer des stratégies de Restriction logicielle**. Lorsque vous supprimez des stratégies de restriction logicielle pour un objet de stratégie de groupe, vous supprimez également toutes les règles de stratégies de restriction logicielle pour cet objet de stratégie de groupe. Après avoir supprimé les stratégies de restriction logicielle, vous pouvez créer des stratégies de restriction logicielle pour cet objet de stratégie de groupe.

## <a name="BKMK_Add_Del"></a>Pour ajouter ou supprimer un type de fichier désigné

1.  Ouvrez stratégies de Restriction logicielle.

2.  Dans le volet d’informations, double-cliquez sur **Types de fichiers désignés**.

3.  Effectuez l’une des opérations suivantes:

    -   Pour ajouter un type de fichier dans **extension de nom de fichier**, tapez l’extension de nom de fichier, puis cliquez sur **ajouter**.

    -   Pour supprimer un type de fichier dans **types de fichiers désignés**et cliquez sur le type de fichier, puis cliquez sur **supprimer**.

> [!NOTE]
> -   Différentes informations d’identification d’administration sont requis pour effectuer cette procédure, en fonction de l’environnement dans lequel vous ajoutez ou supprimez un type de fichier désigné:
> 
>     -   Si vous ajoutez ou supprimez un type de fichier désigné pour votre ordinateur local: l’appartenance au groupe **administrateurs** , ou équivalent, est la condition minimale requise pour effectuer cette procédure.
>     -   Si vous créez des stratégies de restriction logicielle pour un ordinateur qui est joint à un domaine, les membres du groupe Admins du domaine peuvent effectuer cette procédure.
> -   Il peut être nécessaire de créer un nouveau paramètre de stratégie de restriction logicielle pour l’objet de stratégie de groupe (GPO) si vous ne le n'avez pas déjà fait.
> -   La liste des types de fichiers désignés est partagée par toutes les règles de Configuration de l’ordinateur et Configuration de l’utilisateur pour un objet de stratégie de groupe.

## <a name="BKMK_Prevent_Admin"></a>Pour empêcher l’application aux administrateurs locaux des stratégies de restriction logicielle

1.  Ouvrez stratégies de Restriction logicielle.

2.  Dans le volet d’informations, double-cliquez sur **mise en œuvre**.

3.  Sous **appliquer des stratégies de restriction logicielle aux utilisateurs suivants**, cliquez sur **tous les utilisateurs exceptés les administrateurs locaux**.

> [!WARNING]
> -   L’appartenance au groupe **administrateurs** , ou équivalent, est la condition minimale requise pour effectuer cette procédure.
> -   Il peut être nécessaire de créer un nouveau paramètre de stratégie de restriction logicielle pour l’objet de stratégie de groupe (GPO) si vous ne le n'avez pas déjà fait.
> -   S’il est courant pour les utilisateurs membres du groupe Administrateurs local sur leurs ordinateurs de votre organisation, il pouvez que vous ne souhaitez pas Activez cette option.
> -   Si vous définissez un paramètre de stratégie de restriction logicielle pour votre ordinateur local, utilisez cette procédure pour empêcher les administrateurs locaux d’avoir des stratégies de restriction logicielle appliquées à ces derniers. Si vous définissez un paramètre de stratégie de restriction logicielle pour votre réseau, filtrez les paramètres de stratégie utilisateur selon l’appartenance aux groupes de sécurité via la stratégie de groupe.

## <a name="BKMK_Sec_Lvl"></a>Pour modifier le niveau de sécurité par défaut des stratégies de restriction logicielle

1.  Ouvrez stratégies de Restriction logicielle.

2.  Dans le volet d’informations, double-cliquez sur **niveaux de sécurité**.

3.  Cliquez sur le niveau de sécurité que vous souhaitez définir comme la valeur par défaut, puis cliquez sur **par défaut**.

> [!CAUTION]
> Dans certains répertoires, la définition de la sécurité par défaut au niveau de **non autorisé** peut nuire à votre système d’exploitation.

> [!NOTE]
> -   Différentes informations d’identification d’administration sont requis pour effectuer cette procédure, en fonction de l’environnement pour lequel vous modifiez le niveau de sécurité par défaut des stratégies de restriction logicielle.
> -   Il peut être nécessaire de créer un nouveau paramètre de stratégie de restriction logicielle pour cet objet de stratégie de groupe (GPO) si vous ne le n'avez pas déjà fait.
> -   Dans le volet d’informations, le niveau de sécurité par défaut actuel est indiqué par un cercle noir avec une coche dans celui-ci. Si vous cliquez sur le niveau de sécurité par défaut actuel, le **par défaut** commande n’apparaît pas dans le menu.
> -   Règles de stratégies de restriction logicielle sont créées pour indiquer des exceptions au niveau de sécurité par défaut. Lorsque le niveau de sécurité par défaut a la valeur **non restreint**, les règles peuvent indiquer les logiciels qui n’est pas autorisé à exécuter. Lorsque le niveau de sécurité par défaut a la valeur **non autorisé**, les règles peuvent indiquer les logiciels qui est autorisé à exécuter.
> -   Lors de l’installation, le niveau de sécurité par défaut des stratégies de restriction logicielle sur tous les fichiers sur votre système est défini **non restreint**.

## <a name="BKMK_Apply_SRP_DLLs"></a>Pour appliquer des stratégies de restriction logicielle aux DLL

1.  Ouvrez stratégies de Restriction logicielle.

2.  Dans le volet d’informations, double-cliquez sur **mise en œuvre**.

3.  Sous **appliquer des stratégies de restriction logicielle à la suivante**, cliquez sur **tous les fichiers logiciels**.

> [!NOTE]
> -   Pour effectuer cette procédure, vous devez être membre du groupe Administrateurs sur l’ordinateur local, ou doit vous avoir été déléguée l’autorité appropriée. Si l’ordinateur est joint à un domaine, les membres du groupe Admins du domaine peuvent être en mesure d’effectuer cette procédure.
> -   Par défaut, les stratégies de restriction logicielle ne vérifient pas les bibliothèques de liens dynamiques (DLL). La vérification des DLL peut réduire les performances du système, car les stratégies de restriction logicielle doivent être évaluées chaque fois qu’une DLL est chargée. Toutefois, vous pouvez décider de vérifier les DLL si vous avez peur de recevoir un virus qui cible les DLL. Si le niveau de sécurité par défaut est défini sur **non autorisé**, et que vous activez la vérification, vous devez créer de restriction logicielle des règles de stratégies qui permettent de chaque DLL exécuter des DLL.


