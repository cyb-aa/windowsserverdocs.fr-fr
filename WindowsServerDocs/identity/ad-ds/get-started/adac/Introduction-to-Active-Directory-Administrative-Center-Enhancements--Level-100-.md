---
ms.assetid: 074e63e9-976c-49da-8cba-9ae0b3325e34
title: "Introduction aux améliorations du centre d’administration ActiveDirectory (niveau 100)"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 7f52b22ec74ba12c383952e68b412f871a56474c
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="introduction-to-active-directory-administrative-center-enhancements-level-100"></a>Introduction aux améliorations du centre d’administration ActiveDirectory (niveau 100)

>S’applique à: Windows Server2016, Windows Server2012R2, Windows Server2012

ADAC dans Windows Server2012 inclut les fonctionnalités de gestion pour les éléments suivants:

-   [Corbeille ActiveDirectory](../../../ad-ds/get-started/adac/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-.md#ad_recycle_bin_mgmt)

-   [Stratégie de mot de passe affinée](../../../ad-ds/get-started/adac/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-.md#fine_grained_pswd_policy_mgmt)

-   [Visionneuse de l’historique de PowerShell de Windows](../../../ad-ds/get-started/adac/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-.md#windows_powershell_history_viewer)

## <a name="ad_recycle_bin_mgmt"></a>Corbeille ActiveDirectory
La suppression accidentelle des objets ActiveDirectory est une occurrence courante pour les utilisateurs des Services de domaine ActiveDirectory (ADDS) et ActiveDirectory Lightweight Directory Services (ADLDS). Dans les versions antérieures de Windows Server, avant Windows Server2008R2, une possible de récupérer des objets supprimés accidentellement dans ActiveDirectory, mais les solutions avaient leurs inconvénients.

Dans Windows Server2008, vous pouvez utiliser la fonctionnalité sauvegarde Windows Server et **ntdsutil** commande de restauration faisant autorité pour marquer des objets comme faisant autorité pour vous assurer que les données restaurées soient répliquées dans tout le domaine. L’inconvénient de la solution de restauration faisant autorité était qu’elle devait être effectuée en Mode restauration des Services d’annuaire (DSRM). Dans ce mode, le contrôleur de domaine en cours de restauration devait rester hors connexion. Par conséquent, il n’a pas pu traiter les demandes client.

Dans Windows Server2003 ActiveDirectory et ADDS Windows Server2008, vous pourriez récupérer des objets ActiveDirectory supprimés par le biais de la réanimation des désactivations. Toutefois, réanimés lien attributs à valeur des objets (par exemple, les appartenances de groupe des comptes d’utilisateur) qui étaient supprimés physiquement et attributs non-liée qui étaient supprimés n’étaient pas récupérés. Par conséquent, les administrateurs peuvent s’appuie pas sur la réanimation des désactivations comme solution suprême à la suppression accidentelle d’objets. Pour plus d’informations sur la réanimation des désactivations, voir [réanimation des objets Tombstone d’ActiveDirectory](https://go.microsoft.com/fwlink/?LinkID=125452).

La Corbeille ActiveDirectory, à compter de Windows Server2008R2, s’appuie sur l’infrastructure existante de réanimation des objets tombstone et améliore votre capacité à préserver et récupérer des objets ActiveDirectory supprimés accidentellement.

Lorsque vous activez la Corbeille ActiveDirectory, toutes les valeurs de lien et les attributs de non-liée des objets ActiveDirectory supprimés sont préservés et les objets sont restaurés dans leur intégralité dans le même état logique et cohérent qu’ils avaient juste avant la suppression. Par exemple, les comptes d’utilisateurs restaurés retrouvent automatiquement tous les droits d’accès correspondants dont ils disposaient juste avant la suppression, dans et entre les domaines et les appartenances aux groupes. La Corbeille ActiveDirectory fonctionne pour les environnements ADDS et ADLDS. Pour obtenir une description détaillée de la Corbeille ActiveDirectory, voir [Nouveautés dans ADDS: Corbeille ActiveDirectory](https://technet.microsoft.com/library/dd391916(WS.10).aspx).

**Quelles sont les nouveautés?** Dans Windows Server2012, la fonctionnalité de la Corbeille ActiveDirectory a été améliorée avec une nouvelle interface graphique utilisateur pour les utilisateurs de gérer et restaurer des objets supprimés. Les utilisateurs peuvent désormais visuellement localiser une liste d’objets supprimés et les restaurer à leur emplacement d’origine ou souhaitée.

Si vous prévoyez d’activer la Corbeille ActiveDirectory dans Windows Server2012, procédez comme suit:

-   Par défaut, la Corbeille ActiveDirectory est désactivée. Pour l’activer, vous devez tout d’abord augmenter le niveau fonctionnel de forêt de votre environnement ADDS ou ADLDS vers Windows Server2008R2 ou une version ultérieure. Cela nécessite que tous les contrôleurs de domaine dans la forêt ou tous les serveurs qui hébergent des instances de jeux de configuration ADLDS être exécutant Windows Server2008R2 ou version ultérieure.

-   Le processus d’activation de la Corbeille ActiveDirectory est irréversible. Après avoir activé la Corbeille ActiveDirectory dans votre environnement, vous ne pouvez pas le désactiver.

-   Pour gérer la fonctionnalité de la Corbeille par le biais d’une interface utilisateur, vous devez installer la version du centre d’administration ActiveDirectory dans Windows Server2012.

    > [!NOTE]
    > Vous pouvez utiliser **le Gestionnaire de serveur** pour installer les outils d’Administration de serveur distant (RSAT) sur les ordinateurs Windows Server2012 à utiliser la version correcte du centre d’administration ActiveDirectory pour gérer la Corbeille par le biais d’une interface utilisateur.
    > 
    > Vous pouvez utiliser [RSAT](https://go.microsoft.com/fwlink/?LinkID=238560) sur Windows&reg; 8ordinateurs d’utiliser la version correcte du centre d’administration ActiveDirectory pour gérer la Corbeille par le biais d’une interface utilisateur.

### <a name="active-directory-recycle-bin-step-by-step"></a>La Corbeille ActiveDirectory pas à pas
Dans les étapes suivantes, vous allez utiliser ADAC pour effectuer les tâches suivantes de la Corbeille ActiveDirectory dans Windows Server2012:

-   [Étape1: Augmenter le niveau fonctionnel de forêt](../../../ad-ds/get-started/adac/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-.md#bkmk_raise_ffl)

-   [Étape2: Activer la Corbeille](../../../ad-ds/get-started/adac/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-.md#bkmk_enable_recycle_bin)

-   [Étape3: Créer des utilisateurs de test, un groupe et unité d’organisation](../../../ad-ds/get-started/adac/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-.md#bkmk_create_test_env)

-   [Étape4: Restaurer les objets supprimés](../../../ad-ds/get-started/adac/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-.md#bkmk_restore_del_obj)

> [!NOTE]
> L’appartenance au groupe Administrateurs de l’entreprise ou des autorisations équivalentes est requis pour effectuer les étapes suivantes.

### <a name="bkmk_raise_ffl"></a>Étape1: Augmenter le niveau fonctionnel de forêt
Dans cette étape, vous allez augmenter le niveau fonctionnel de forêt. Vous devez tout d’abord augmenter le niveau fonctionnel de la forêt cible à Windows Server2008R2 au minimum avant d’activer la Corbeille ActiveDirectory.

##### <a name="to-raise-the-functional-level-on-the-target-forest"></a>Pour augmenter le niveau fonctionnel de la forêt cible

1.  Cliquez avec le bouton droit sur l’icône Windows PowerShell, cliquez sur **exécuter en tant qu’administrateur** et type **dsac.exe** pour ouvrir ADAC.

2.  Cliquez sur **gérer**, cliquez sur **ajouter des nœuds de Navigation** et sélectionnez le domaine cible approprié dans le **ajouter des nœuds de Navigation** boîte de dialogue, puis cliquez sur **OK**.

3.  Cliquez sur le domaine cible dans le volet de navigation gauche et, dans le **tâches** volet, cliquez sur **augmenter le niveau fonctionnel de forêt**. Sélectionnez un niveau fonctionnel de forêt qui est au moins Windows Server2008R2 ou version ultérieure, puis **OK**.

![Présentation du centre d’administration ActiveDirectory](media/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-/PowerShellLogoSmall.gif)Windows PowerShell équivalentes commandes ***

L’applet de commande Windows PowerShell ou les applets de commande suivantes remplissent la même fonction que la procédure précédente. Entrez chaque applet de commande sur une seule ligne, même si elles peuvent apparaître comme renvoyées sur plusieurs lignes ici en raison de contraintes de mise en forme.

```
Set-ADForestMode -Identity contoso.com -ForestMode Windows2008R2Forest -Confirm:$false
```

Pour le **-identité** argument, spécifiez le nom DNS complet.

### <a name="bkmk_enable_recycle_bin"></a>Étape2: Activer la Corbeille
Dans cette étape, vous allez activer la Corbeille pour restaurer des objets supprimés dans ADDS.

##### <a name="to-enable-active-directory-recycle-bin-in-adac-on-the-target-domain"></a>Pour activer la Corbeille ActiveDirectory dans ADAC sur le domaine cible

1.  Cliquez avec le bouton droit sur l’icône Windows PowerShell, cliquez sur **exécuter en tant qu’administrateur** et type **dsac.exe** pour ouvrir ADAC.

2.  Cliquez sur **gérer**, cliquez sur **ajouter des nœuds de Navigation** et sélectionnez le domaine cible approprié dans le **ajouter des nœuds de Navigation** boîte de dialogue, puis cliquez sur **OK**.

3.  Dans le **tâches** volet, cliquez sur **activer la Corbeille... **dans les **tâches** volet, cliquez sur **OK** dans la boîte de message d’avertissement, puis cliquez sur **OK** pour le message d’actualisation ADAC.

4.  Appuyez sur F5 pour actualiser ADAC.

![Présentation du centre d’administration ActiveDirectory](media/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-/PowerShellLogoSmall.gif)Windows PowerShell équivalentes commandes ***

L’applet de commande Windows PowerShell ou les applets de commande suivantes remplissent la même fonction que la procédure précédente. Entrez chaque applet de commande sur une seule ligne, même si elles peuvent apparaître comme renvoyées sur plusieurs lignes ici en raison de contraintes de mise en forme.

```
Enable-ADOptionalFeature -Identity 'CN=Recycle Bin Feature,CN=Optional Features,CN=Directory Service,CN=Windows NT,CN=Services,CN=Configuration,DC=contoso,DC=com' -Scope ForestOrConfigurationSet -Target 'contoso.com'
```

### <a name="bkmk_create_test_env"></a>Étape3: Créer des utilisateurs de test, un groupe et unité d’organisation
Dans les procédures suivantes, vous allez créer deux utilisateurs test. Puis vous créez un groupe test et ajouter les utilisateurs de test au groupe. En outre, vous allez créer une unité d’organisation.

##### <a name="to-create-test-users"></a>Pour créer des utilisateurs de test

1.  Cliquez avec le bouton droit sur l’icône Windows PowerShell, cliquez sur **exécuter en tant qu’administrateur** et type **dsac.exe** pour ouvrir ADAC.

2.  Cliquez sur **gérer**, cliquez sur **ajouter des nœuds de Navigation** et sélectionnez le domaine cible approprié dans le **ajouter des nœuds de Navigation** boîte de dialogue, puis cliquez sur **OK**.

3.  Dans le **tâches** volet, cliquez sur **New** puis cliquez sur **utilisateur**.

    ![Présentation du centre d’administration ActiveDirectory](media/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-/ADDS_ADACNewUser.gif)

4.  Entrez les informations suivantes sous **compte** puis cliquez sur OK:

    -   Nom complet: test1

    -   Ouverture de session SamAccountName de l’utilisateur: test1

    -   Mot de passe:p@ssword1

    -   Confirmer le mot de passe:p@ssword1

5.  Répétez les étapes précédentes pour créer un second utilisateur, test2.

##### <a name="to-create-a-test-group-and-add-users-to-the-group"></a>Pour créer un groupe de test et ajouter des utilisateurs au groupe

1.  Cliquez avec le bouton droit sur l’icône Windows PowerShell, cliquez sur **exécuter en tant qu’administrateur** et type **dsac.exe** pour ouvrir ADAC.

2.  Cliquez sur **gérer**, cliquez sur **ajouter des nœuds de Navigation** et sélectionnez le domaine cible approprié dans le **ajouter des nœuds de Navigation** boîte de dialogue, puis cliquez sur **OK**.

3.  Dans le **tâches** volet, cliquez sur **New** puis cliquez sur **groupe**.

4.  Entrez les informations suivantes sous **groupe** puis cliquez sur **OK**:

    -   **Nom de groupe: group1**

5.  Cliquez sur **group1**, puis, sous le **tâches** volet, cliquez sur **propriétés**.

6.  Cliquez sur **membres**, cliquez sur **ajouter**, type **test1; test2**, puis cliquez sur **OK**.

![Présentation du centre d’administration ActiveDirectory](media/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-/PowerShellLogoSmall.gif)Windows PowerShell équivalentes commandes ***

L’applet de commande Windows PowerShell ou les applets de commande suivantes remplissent la même fonction que la procédure précédente. Entrez chaque applet de commande sur une seule ligne, même si elles peuvent apparaître comme renvoyées sur plusieurs lignes ici en raison de contraintes de mise en forme.

```
Add-ADGroupMember -Identity group1 -Member test1
```

##### <a name="to-create-an-organizational-unit"></a>Pour créer une unité d’organisation

1.  Cliquez avec le bouton droit sur l’icône Windows PowerShell, cliquez sur **exécuter en tant qu’administrateur** et type **dsac.exe** pour ouvrir ADAC.

2.  Cliquez sur **gérer**, cliquez sur **ajouter des nœuds de Navigation** et sélectionnez le domaine cible approprié dans le **ajouter des nœuds de Navigation** boîte de dialogue, puis cliquez sur **OK**.

3.  Dans le **tâches** volet, cliquez sur **New** puis cliquez sur **unité d’organisation**.

4.  Entrez les informations suivantes sous **unité d’organisation** puis cliquez sur **OK**:

    -   **NameOU1**

![Présentation du centre d’administration ActiveDirectory](media/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-/PowerShellLogoSmall.gif)Windows PowerShell équivalentes commandes ***

L’applet de commande Windows PowerShell ou les applets de commande suivantes remplissent la même fonction que la procédure précédente. Entrez chaque applet de commande sur une seule ligne, même si elles peuvent apparaître comme renvoyées sur plusieurs lignes ici en raison de contraintes de mise en forme.

```
1..2 | ForEach-Object {New-ADUser -SamAccountName test$_ -Name "test$_" -Path "DC=fabrikam,DC=com" -AccountPassword (ConvertTo-SecureString -AsPlainText "p@ssword1" -Force) -Enabled $true}
New-ADGroup -Name "group1" -SamAccountName group1 -GroupCategory Security -GroupScope Global -DisplayName "group1"
New-ADOrganizationalUnit -Name OU1 -Path "DC=fabrikam,DC=com"

```

### <a name="bkmk_restore_del_obj"></a>Étape4: Restaurer les objets supprimés
Dans les procédures suivantes, vous allez restaurer des objets supprimés à partir de la **objets supprimés** conteneur vers leur emplacement d’origine et à un autre emplacement.

##### <a name="to-restore-deleted-objects-to-their-original-location"></a>Pour restaurer des objets supprimés à leur emplacement d’origine

1.  Cliquez avec le bouton droit sur l’icône Windows PowerShell, cliquez sur **exécuter en tant qu’administrateur** et type **dsac.exe** pour ouvrir ADAC.

2.  Cliquez sur **gérer**, cliquez sur **ajouter des nœuds de Navigation** et sélectionnez le domaine cible approprié dans le **ajouter des nœuds de Navigation** boîte de dialogue, puis cliquez sur **OK**.

3.  Sélectionnez utilisateurs **test1** et **test2**, cliquez sur **supprimer** dans les **tâches** volet, puis cliquez sur **Oui** pour confirmer la suppression.

    ![Présentation du centre d’administration ActiveDirectory](media/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-/PowerShellLogoSmall.gif)Windows PowerShell équivalentes commandes ***

    L’applet de commande Windows PowerShell ou les applets de commande suivantes remplissent la même fonction que la procédure précédente. Entrez chaque applet de commande sur une seule ligne, même si elles peuvent apparaître comme renvoyées sur plusieurs lignes ici en raison de contraintes de mise en forme.

    ```
    Get-ADUser -Filter 'Name -Like "*test*"'|Remove-ADUser -Confirm:$false
    ```

4.  Accédez à la **objets supprimés** conteneur, sélectionnez **test2** et **test1** puis cliquez sur **restaurer** dans les **tâches** volet.

5.  Pour vérifier que les objets ont été restaurés à leur emplacement d’origine, accédez au domaine cible et vérifiez que les comptes d’utilisateur sont répertoriés.

    > [!NOTE]
    > Si vous accédez à la **propriétés** des comptes d’utilisateur **test1** et **test2** puis cliquez sur **membre de**, vous verrez que leur appartenance aux groupes a été restaurée également.

L’applet de commande Windows PowerShell ou les applets de commande suivantes remplissent la même fonction que la procédure précédente. Entrez chaque applet de commande sur une seule ligne, même si elles peuvent apparaître comme renvoyées sur plusieurs lignes ici en raison de contraintes de mise en forme.

![Présentation du centre d’administration ActiveDirectory](media/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-/PowerShellLogoSmall.gif)Windows PowerShell équivalentes commandes ***

```
Get-ADObject -Filter 'Name -Like "*test*"' -IncludeDeletedObjects | Restore-ADObject
```

##### <a name="to-restore-deleted-objects-to-a-different-location"></a>Pour restaurer des objets supprimés à un autre emplacement

1.  Cliquez avec le bouton droit sur l’icône Windows PowerShell, cliquez sur **exécuter en tant qu’administrateur** et type **dsac.exe** pour ouvrir ADAC.

2.  Cliquez sur **gérer**, cliquez sur **ajouter des nœuds de Navigation** et sélectionnez le domaine cible approprié dans le **ajouter des nœuds de Navigation** boîte de dialogue, puis cliquez sur **OK**.

3.  Sélectionnez utilisateurs **test1** et **test2**, cliquez sur **supprimer** dans les **tâches** volet, puis cliquez sur **Oui** pour confirmer la suppression.

4.  Accédez à la **objets supprimés** conteneur, sélectionnez **test2** et **test1** puis cliquez sur **restaurer sur** dans les **tâches** volet.

5.  Sélectionnez **OU1** puis cliquez sur **OK**.

6.  Pour confirmer que les objets ont été restaurés sur **OU1**, accédez au domaine cible, double-cliquez sur **OU1** et vérifiez que les comptes d’utilisateurs sont répertoriés.

![Présentation du centre d’administration ActiveDirectory](media/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-/PowerShellLogoSmall.gif)Windows PowerShell équivalentes commandes ***

L’applet de commande Windows PowerShell ou les applets de commande suivantes remplissent la même fonction que la procédure précédente. Entrez chaque applet de commande sur une seule ligne, même si elles peuvent apparaître comme renvoyées sur plusieurs lignes ici en raison de contraintes de mise en forme.

```
Get-ADObject -Filter 'Name -Like "*test*"' -IncludeDeletedObjects | Restore-ADObject -TargetPath "OU=OU1,DC=contoso,DC=com"
```

## <a name="fine_grained_pswd_policy_mgmt"></a>Stratégie de mot de passe affinée
Le système d’exploitation Windows Server2008 fournit aux organisations un moyen de définir le mot de passe et les stratégies de verrouillage de compte pour des ensembles d’utilisateurs dans un domaine. Dans les domaines ActiveDirectory antérieurs à Windows Server2008, stratégie de mot de passe qu’une seule et la stratégie de verrouillage de compte peuvent être appliquées à tous les utilisateurs dans le domaine. Ces stratégies étaient spécifiées dans la stratégie de domaine par défaut pour le domaine. Par conséquent, les organisations qui voulaient différents paramètres de verrouillage du compte et de mot de passe pour des ensembles d’utilisateurs devaient créer un filtre de mot de passe ou déployer plusieurs domaines. Les deux options sont onéreuses.

Vous pouvez utiliser des stratégies de mot de passe affinées pour spécifier plusieurs stratégies de mot de passe dans un seul domaine et appliquer des restrictions différentes stratégies de verrouillage de compte et de mot de passe à différents groupes d’utilisateurs dans un domaine. Par exemple, vous pouvez appliquer des paramètres plus stricts à des comptes privilégiés et paramètres moins stricts aux comptes des autres utilisateurs. Dans d’autres cas, vous souhaiterez peut-être appliquer une stratégie de mot de passe spéciale pour les comptes dont les mots de passe sont synchronisés avec d’autres sources de données. Pour obtenir une description détaillée de la stratégie de mot de passe affinée, voir [ADDS: stratégies de mot de passe affinées](https://technet.microsoft.com/library/cc770394(WS.10).aspx)

**Quelles sont les nouveautés?** Dans Windows Server2012, gestion des stratégies de mot de passe affinées soit plus facile et plus visuelle en fournissant une interface utilisateur pour les administrateurs de domaine ActiveDirectory pour les gérer dans ADAC. Les administrateurs peuvent désormais afficher la stratégie résultante d’un utilisateur donné, afficher et trier toutes les stratégies de mot de passe dans un domaine donné et gérer visuellement les stratégies de mot de passe individuelles.

Si vous prévoyez d’utiliser des stratégies de mot de passe affinée dans Windows Server2012, procédez comme suit:

-   Les stratégies de mot de passe affinées s’appliquent uniquement des groupes de sécurité globaux et les objets utilisateur (ou des objets inetOrgPerson s’ils sont utilisés au lieu d’objets utilisateur). Par défaut, seuls les membres du groupe Admins du domaine peuvent définir des stratégies de mot de passe affinées. Toutefois, vous pouvez également déléguer la capacité de définir ces stratégies à d’autres utilisateurs. Le niveau fonctionnel du domaine doit être Windows Server2008 ou version supérieure.

-   Vous devez utiliser la version de Windows Server2012 du centre d’administration ActiveDirectory pour administrer les stratégies de mot de passe affinées par le biais d’une interface graphique utilisateur.

    > [!NOTE]
    > Vous pouvez utiliser **le Gestionnaire de serveur** pour installer les outils d’Administration de serveur distant (RSAT) sur les ordinateurs Windows Server2012 à utiliser la version correcte du centre d’administration ActiveDirectory pour gérer la Corbeille par le biais d’une interface utilisateur.
    > 
    > Vous pouvez utiliser [RSAT](https://go.microsoft.com/fwlink/?LinkID=238560) sur Windows&reg; 8ordinateurs d’utiliser la version correcte du centre d’administration ActiveDirectory pour gérer la Corbeille par le biais d’une interface utilisateur.

### <a name="fine-grained-password-policy-step-by-step"></a>Pas à pas de stratégie de mot de passe affinées
Dans les étapes suivantes, vous allez utiliser ADAC pour effectuer les tâches de stratégie de mot de passe affinées suivantes:

-   [Étape1: Augmenter le niveau fonctionnel du domaine](../../../ad-ds/get-started/adac/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-.md#bkmk_raise_dfl)

-   [Étape2: Créer des utilisateurs de test, un groupe et unité d’organisation](../../../ad-ds/get-started/adac/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-.md#bkmk2_test_fgpp)

-   [Étape3: Créer une nouvelle stratégie de mot de passe affinée](../../../ad-ds/get-started/adac/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-.md#bkmk_create_fgpp)

-   [Étape4: Afficher un jeu de stratégies pour un utilisateur résultant](../../../ad-ds/get-started/adac/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-.md#bkmk_view_resultant_fgpp)

-   [Étape5: Modifier une stratégie de mot de passe affinée](../../../ad-ds/get-started/adac/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-.md#bkmk_edit_fgpp)

-   [Étape6: Supprimer une stratégie de mot de passe affinée](../../../ad-ds/get-started/adac/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-.md#bkmk_delete_fgpp)

> [!NOTE]
> L’appartenance au groupe Admins du domaine ou des autorisations équivalentes est requis pour effectuer les étapes suivantes.

#### <a name="bkmk_raise_dfl"></a>Étape1: Augmenter le niveau fonctionnel du domaine
Dans la procédure suivante, vous allez augmenter le niveau fonctionnel du domaine du domaine cible vers Windows Server2008 ou une version ultérieure. Un niveau fonctionnel du domaine de Windows Server2008 ou version ultérieure est requis pour activer des stratégies de mot de passe affinées.

###### <a name="to-raise-the-domain-functional-level"></a>Pour augmenter le niveau fonctionnel du domaine

1.  Cliquez avec le bouton droit sur l’icône Windows PowerShell, cliquez sur **exécuter en tant qu’administrateur** et type **dsac.exe** pour ouvrir ADAC.

2.  Cliquez sur **gérer**, cliquez sur **ajouter des nœuds de Navigation** et sélectionnez le domaine cible approprié dans le **ajouter des nœuds de Navigation** boîte de dialogue, puis cliquez sur **OK**.

3.  Cliquez sur le domaine cible dans le volet de navigation gauche et, dans le **tâches** volet, cliquez sur **augmenter le niveau fonctionnel du domaine**. Sélectionnez un niveau fonctionnel de forêt qui est au moins Windows Server2008 ou version supérieure, puis cliquez sur **OK**.

![Présentation du centre d’administration ActiveDirectory](media/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-/PowerShellLogoSmall.gif)Windows PowerShell équivalentes commandes ***

L’applet de commande Windows PowerShell ou les applets de commande suivantes remplissent la même fonction que la procédure précédente. Entrez chaque applet de commande sur une seule ligne, même si elles peuvent apparaître comme renvoyées sur plusieurs lignes ici en raison de contraintes de mise en forme.

```
Set-ADDomainMode -Identity contoso.com -DomainMode 3
```

#### <a name="bkmk2_test_fgpp"></a>Étape2: Créer des utilisateurs de test, un groupe et unité d’organisation
Pour créer les utilisateurs de test et le groupe requis pour cette étape, suivez les procédures situés ici: [étape3: créer une unité d’organisation, un groupe et les utilisateurs test](../../../ad-ds/get-started/adac/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-.md#bkmk_create_test_env) (vous n’avez pas besoin créer l’unité d’organisation pour illustrer une stratégie de mot de passe affinée).

#### <a name="bkmk_create_fgpp"></a>Étape3: Créer une nouvelle stratégie de mot de passe affinée
Dans la procédure suivante, vous allez créer une nouvelle stratégie de mot de passe affinée à l’aide de l’interface utilisateur dans ADAC.

###### <a name="to-create-a-new-fine-grained-password-policy"></a>Pour créer une nouvelle stratégie de mot de passe affinée

1.  Cliquez avec le bouton droit sur l’icône Windows PowerShell, cliquez sur **exécuter en tant qu’administrateur** et type **dsac.exe** pour ouvrir ADAC.

2.  Cliquez sur **gérer**, cliquez sur **ajouter des nœuds de Navigation** et sélectionnez le domaine cible approprié dans le **ajouter des nœuds de Navigation** boîte de dialogue, puis cliquez sur **OK**.

3.  Dans le volet de navigation ADAC, ouvrez le **système** conteneur, puis cliquez sur **Password Settings Container**.

4.  Dans le **tâches** volet, cliquez sur **New**, puis cliquez sur **les paramètres de mot de passe**.

    Remplissez ou modifiez les champs à l’intérieur de la page de propriétés pour créer un nouveau **les paramètres de mot de passe** objet. Le **nom** et **priorité** champs sont obligatoires.

    ![Présentation du centre d’administration ActiveDirectory](media/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-/ADDS_ADACNewFGPP.gif)

5.  Sous **directement s’applique à**, cliquez sur **ajouter**, type **group1**, puis cliquez sur **OK**.

    Cela associe l’objet de stratégie de mot de passe avec les membres du groupe global que vous avez créé pour l’environnement de test.

6.  Cliquez sur **OK** pour soumettre la création.

![Présentation du centre d’administration ActiveDirectory](media/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-/PowerShellLogoSmall.gif)Windows PowerShell équivalentes commandes ***

L’applet de commande Windows PowerShell ou les applets de commande suivantes remplissent la même fonction que la procédure précédente. Entrez chaque applet de commande sur une seule ligne, même si elles peuvent apparaître comme renvoyées sur plusieurs lignes ici en raison de contraintes de mise en forme.

```
New-ADFineGrainedPasswordPolicy TestPswd -ComplexityEnabled:$true -LockoutDuration:"00:30:00" -LockoutObservationWindow:"00:30:00" -LockoutThreshold:"0" -MaxPasswordAge:"42.00:00:00" -MinPasswordAge:"1.00:00:00" -MinPasswordLength:"7" -PasswordHistoryCount:"24" -Precedence:"1" -ReversibleEncryptionEnabled:$false -ProtectedFromAccidentalDeletion:$true
Add-ADFineGrainedPasswordPolicySubject TestPswd -Subjects group1
```

#### <a name="bkmk_view_resultant_fgpp"></a>Étape4: Afficher un jeu de stratégies pour un utilisateur résultant
Dans la procédure suivante, vous allez consulter les paramètres de mot de passe résultants pour un utilisateur qui est membre du groupe auquel vous avez attribué une stratégie de mot de passe affinée dans [étape3: créer une nouvelle stratégie de mot de passe affinées](../../../ad-ds/get-started/adac/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-.md#bkmk_create_fgpp).

###### <a name="to-view-a-resultant-set-of-policies-for-a-user"></a>Pour afficher un jeu de stratégies pour un utilisateur résultant

1.  Cliquez avec le bouton droit sur l’icône Windows PowerShell, cliquez sur **exécuter en tant qu’administrateur** et type **dsac.exe** pour ouvrir ADAC.

2.  Cliquez sur **gérer**, cliquez sur **ajouter des nœuds de Navigation** et sélectionnez le domaine cible approprié dans le **ajouter des nœuds de Navigation** boîte de dialogue, puis cliquez sur **OK**.

3.  Sélectionnez un utilisateur, **test1** qui appartient au groupe, **group1** que vous avez associé une stratégie de mot de passe affinée dans [étape3: créer une nouvelle stratégie de mot de passe affinées](../../../ad-ds/get-started/adac/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-.md#bkmk_create_fgpp).

4.  Cliquez sur **afficher les paramètres de mot de passe résultant** dans les **tâches** volet.

5.  Examiner la stratégie des paramètres de mot de passe, puis cliquez sur **Annuler**.

![Présentation du centre d’administration ActiveDirectory](media/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-/PowerShellLogoSmall.gif)Windows PowerShell équivalentes commandes ***

L’applet de commande Windows PowerShell ou les applets de commande suivantes remplissent la même fonction que la procédure précédente. Entrez chaque applet de commande sur une seule ligne, même si elles peuvent apparaître comme renvoyées sur plusieurs lignes ici en raison de contraintes de mise en forme.

```
Get-ADUserResultantPasswordPolicy test1
```

#### <a name="bkmk_edit_fgpp"></a>Étape5: Modifier une stratégie de mot de passe affinée
Dans la procédure suivante, vous allez modifier la stratégie de mot de passe affinée que vous avez créé dans [étape3: créer une nouvelle stratégie de mot de passe affinée](../../../ad-ds/get-started/adac/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-.md#bkmk_create_fgpp)

###### <a name="to-edit-a-fine-grained-password-policy"></a>Pour modifier une stratégie de mot de passe affinée

1.  Cliquez avec le bouton droit sur l’icône Windows PowerShell, cliquez sur **exécuter en tant qu’administrateur** et type **dsac.exe** pour ouvrir ADAC.

2.  Cliquez sur **gérer**, cliquez sur **ajouter des nœuds de Navigation** et sélectionnez le domaine cible approprié dans le **ajouter des nœuds de Navigation** boîte de dialogue, puis cliquez sur **OK**.

3.  Dans le ADAC **volet de Navigation**, développez **système** puis cliquez sur **Password Settings Container**.

4.  Sélectionnez la stratégie de mot de passe affinée que vous avez créé dans [étape3: créer une nouvelle stratégie de mot de passe affinées](../../../ad-ds/get-started/adac/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-.md#bkmk_create_fgpp) et cliquez sur **propriétés** dans les **tâches** volet.

5.  Sous **conserver l’historique du mot de passe**, modifiez la valeur de **nombre de mots de passe mémorisés** à **30**.

6.  Cliquez sur **OK**.

![Présentation du centre d’administration ActiveDirectory](media/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-/PowerShellLogoSmall.gif)Windows PowerShell équivalentes commandes ***

L’applet de commande Windows PowerShell ou les applets de commande suivantes remplissent la même fonction que la procédure précédente. Entrez chaque applet de commande sur une seule ligne, même si elles peuvent apparaître comme renvoyées sur plusieurs lignes ici en raison de contraintes de mise en forme.

```
Set-ADFineGrainedPasswordPolicy TestPswd -PasswordHistoryCount:"30"
```

#### <a name="bkmk_delete_fgpp"></a>Étape6: Supprimer une stratégie de mot de passe affinée

###### <a name="to-delete-a-fine-grained-password-policy"></a>Pour supprimer une stratégie de mot de passe affinée

1.  Cliquez avec le bouton droit sur l’icône Windows PowerShell, cliquez sur **exécuter en tant qu’administrateur** et type **dsac.exe** pour ouvrir ADAC.

2.  Cliquez sur **gérer**, cliquez sur **ajouter des nœuds de Navigation** et sélectionnez le domaine cible approprié dans le **ajouter des nœuds de Navigation** boîte de dialogue, puis cliquez sur **OK**.

3.  Dans le volet de Navigation ADAC, développez **système** puis cliquez sur **Password Settings Container**.

4.  Sélectionnez la stratégie de mot de passe affinée que vous avez créé dans [étape3: créer une nouvelle stratégie de mot de passe affinées](../../../ad-ds/get-started/adac/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-.md#bkmk_create_fgpp) et dans le **tâches** volet, cliquez sur **propriétés**.

5.  Désactivez le **protéger contre la suppression accidentelle** case à cocher et cliquez sur **OK**.

6.  Sélectionnez la stratégie de mot de passe affinée, puis, dans le **tâches** volet, cliquez sur **supprimer**.

7.  Cliquez sur **OK** dans la boîte de dialogue de confirmation.

![Présentation du centre d’administration ActiveDirectory](media/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-/PowerShellLogoSmall.gif)Windows PowerShell équivalentes commandes ***

L’applet de commande Windows PowerShell ou les applets de commande suivantes remplissent la même fonction que la procédure précédente. Entrez chaque applet de commande sur une seule ligne, même si elles peuvent apparaître comme renvoyées sur plusieurs lignes ici en raison de contraintes de mise en forme.

```
Set-ADFineGrainedPasswordPolicy -Identity TestPswd -ProtectedFromAccidentalDeletion $False
Remove-ADFineGrainedPasswordPolicy TestPswd -Confirm
```

## <a name="windows_powershell_history_viewer"></a>Visionneuse de l’historique de PowerShell de Windows
ADAC est un outil d’interface utilisateur construit par-dessus Windows PowerShell.  Dans Windows Server2012, les administrateurs informatiques peuvent tirer parti des ADAC pour découvrir Windows PowerShell pour les applets de commande ActiveDirectory à l’aide de la visionneuse de l’historique de Windows PowerShell. Comme les actions sont exécutées dans l’interface utilisateur, la commande Windows PowerShell équivalente est affichée à l’utilisateur dans la visionneuse de l’historique de Windows PowerShell. Cela permet aux administrateurs de créer des scripts automatisés et de réduire les tâches répétitives, ce qui augmente la productivité informatique.  En outre, cette fonctionnalité réduit le temps d’apprentissage de Windows PowerShell pour ActiveDirectory et augmente la confiance des utilisateurs dans l’exactitude de leurs scripts d’automatisation.

Lors de l’utilisation de la visionneuse de l’historique de Windows PowerShell dans Windows Server2012, procédez comme suit:

-   Pour utiliser la visionneuse de Script Windows PowerShell, vous devez utiliser la version de Windows Server2012 de ADAC

    > [!NOTE]
    > Vous pouvez utiliser **le Gestionnaire de serveur** pour installer les outils d’Administration de serveur distant (RSAT) sur les ordinateurs Windows Server2012 à utiliser la version correcte du centre d’administration ActiveDirectory pour gérer la Corbeille par le biais d’une interface utilisateur.
    > 
    > Vous pouvez utiliser [RSAT](https://go.microsoft.com/fwlink/?LinkID=238560) sur Windows&reg; 8ordinateurs d’utiliser la version correcte du centre d’administration ActiveDirectory pour gérer la Corbeille par le biais d’une interface utilisateur.

-   Avoir des connaissances de base Windows PowerShell. Par exemple, vous devez connaître le fonctionnement des pipelines dans Windows PowerShell. Pour plus d’informations sur les pipelines dans Windows PowerShell, voir [et utilisation des pipelines dans Windows PowerShell](https://technet.microsoft.com/library/ee176927.aspx).

### <a name="windows-powershell-history-viewer-step-by-step"></a>Visionneuse de l’historique Windows PowerShell pas à pas
Dans la procédure suivante, vous allez utiliser la visionneuse de l’historique de Windows PowerShell dans ADAC pour élaborer un script Windows PowerShell.  Avant de commencer cette procédure, supprimez l’utilisateur **test1** à partir du groupe, **group1**.

##### <a name="to-construct-a-script-using-powershell-history-viewer"></a>Pour élaborer un script à l’aide de la visionneuse de l’historique de PowerShell

1.  Cliquez avec le bouton droit sur l’icône Windows PowerShell, cliquez sur **exécuter en tant qu’administrateur** et type **dsac.exe** pour ouvrir ADAC.

2.  Cliquez sur **gérer**, cliquez sur **ajouter des nœuds de Navigation** et sélectionnez le domaine cible approprié dans le **ajouter des nœuds de Navigation** boîte de dialogue, puis cliquez sur **OK**.

3.  Développez le **historique de Windows PowerShell** volet au bas de l’écran ADAC.

4.  Sélectionnez l’utilisateur **test1**.

5.  Cliquez sur **ajouter au groupe... **dans les **tâches** volet.

6.  Accédez à **group1** et cliquez sur **OK** dans la boîte de dialogue.

7.  Accédez à la **historique de Windows PowerShell** volet et localisez la commande qui vient d’être générée.

8.  Copiez la commande et collez-la dans l’éditeur de votre choix pour élaborer votre script.

    Par exemple, vous pouvez modifier la commande pour ajouter un autre utilisateur à **group1**, ou ajoutez **test1** vers un autre groupe.

## <a name="see-also"></a>Voir aussi
[Gestion services avancée ADDS à l’aide du centre d’administration ActiveDirectory et #40; niveau 200 et #41;](Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-.md)


