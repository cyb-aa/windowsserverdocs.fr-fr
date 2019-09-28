---
ms.assetid: 11f36f2b-9981-4da0-9e7c-4eca78035f37
title: Annexe D-sécurisation des comptes administrateur intégrés dans Active Directory
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: cc91ccd00c951863f4f9802f3d669e36ff3f3d9a
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71367836"
---
# <a name="appendix-d-securing-built-in-administrator-accounts-in-active-directory"></a>Annexe D : Sécurisation des comptes administrateur intégrés dans Active Directory

>S'applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012


## <a name="appendix-d-securing-built-in-administrator-accounts-in-active-directory"></a>Annexe D : Sécurisation des comptes administrateur intégrés dans Active Directory  
Dans chaque domaine de Active Directory, un compte d’administrateur est créé dans le cadre de la création du domaine. Ce compte est par défaut membre du groupe administrateurs du domaine et administrateurs du domaine, et si le domaine est le domaine racine de la forêt, le compte est également membre du groupe administrateurs de l’entreprise.

L’utilisation du compte administrateur d’un domaine doit être réservée pour les activités de génération initiales, et éventuellement les scénarios de récupération d’urgence. Pour vous assurer qu’un compte d’administrateur peut être utilisé pour effectuer des réparations dans le cas où aucun autre compte ne peut être utilisé, vous ne devez pas modifier l’appartenance par défaut du compte administrateur dans n’importe quel domaine de la forêt. Au lieu de cela, vous devez sécuriser le compte administrateur dans chaque domaine de la forêt, comme décrit dans la section suivante et détaillé dans les instructions pas à pas qui suivent. 

> [!NOTE]
> Ce guide est utilisé pour recommander la désactivation du compte. Ce document a été supprimé, car le livre blanc sur la récupération de la forêt utilise le compte d’administrateur par défaut. La raison en est que c’est le seul compte qui autorise l’ouverture de session sans serveur de catalogue global.


#### <a name="controls-for-built-in-administrator-accounts"></a>Contrôles pour les comptes d’administrateur intégrés  
Pour le compte administrateur intégré dans chaque domaine de votre forêt, vous devez configurer les paramètres suivants :  

-   Activer le **compte est sensible et ne peut pas être** un indicateur délégué sur le compte.  

-   Activez la **carte à puce est nécessaire pour l’indicateur d’ouverture de session interactive** sur le compte.  

-   Configurez les objets de stratégie de groupe pour limiter l’utilisation du compte administrateur sur les systèmes joints à un domaine :  

    -   Dans un ou plusieurs objets de stratégie de groupe que vous créez et liez à des unités d’organisation de station de travail et de serveur membre dans chaque domaine, ajoutez le compte d’administrateur de chaque domaine aux droits d’utilisateur suivants dans la **stratégie \ stratégies \ stratégies \ stratégies \ Attributions des droits utilisateur**:  

        -   Interdire l’accès à cet ordinateur à partir du réseau  

        -   Interdire l’ouverture de session en tant que tâche  

        -   Interdire l’ouverture de session en tant que service  

        -   Interdire l’ouverture de session par les services Bureau à distance  

> [!NOTE]  
> Lorsque vous ajoutez des comptes à ce paramètre, vous devez spécifier si vous configurez des comptes d’administrateur local ou des comptes d’administrateur de domaine. Par exemple, pour ajouter le compte administrateur du domaine NWTRADERS à ces droits Deny, vous devez taper le compte en tant que NWTRADERS\Administrateur ou accéder au compte administrateur du domaine NWTRADERS. Si vous tapez « administrateur » dans ces paramètres de droits d’utilisateur dans l’éditeur d’objets stratégie de groupe, vous limitez le compte d’administrateur local sur chaque ordinateur auquel l’objet de stratégie de groupe est appliqué.
>   
> Nous vous recommandons de restreindre les comptes d’administrateur local sur les serveurs et stations de travail membres de la même manière que les comptes d’administrateur basés sur un domaine. Par conséquent, vous devez généralement ajouter le compte administrateur pour chaque domaine de la forêt et le compte administrateur pour les ordinateurs locaux à ces paramètres de droits d’utilisateur. La capture d’écran suivante montre un exemple de configuration de ces droits d’utilisateur pour bloquer les comptes d’administrateur local et le compte administrateur d’un domaine pour effectuer des ouvertures de session qui ne doivent pas être nécessaires pour ces comptes.  


![sécurisation des comptes administrateur intégrés](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_23.gif)  

-   Configurer des objets de stratégie de groupe pour restreindre les comptes administrateur sur les contrôleurs de domaine  
    -   Dans chaque domaine de la forêt, l’objet de stratégie de groupe des contrôleurs de domaine par défaut ou une stratégie liée à l’UO des contrôleurs de domaine doit être modifié de façon à ajouter le compte d’administrateur de chaque domaine aux droits d’utilisateur suivants dans la **configuration \ paramètres ordinateur \ Sécurité \ stratégies d’autorisation Locales\attribution des droits**:   
        -   Interdire l’accès à cet ordinateur à partir du réseau  

        -   Interdire l’ouverture de session en tant que tâche  

        -   Interdire l’ouverture de session en tant que service  

        -   Interdire l’ouverture de session par les services Bureau à distance  

> [!NOTE]  
> Ces paramètres garantissent que le compte administrateur intégré du domaine ne peut pas être utilisé pour se connecter à un contrôleur de domaine, même si le compte, s’il est activé, peut ouvrir une session localement sur les contrôleurs de domaine. Étant donné que ce compte ne doit être activé et utilisé que dans les scénarios de récupération d’urgence, il est prévu que l’accès physique à au moins un contrôleur de domaine soit disponible, ou que d’autres comptes disposant d’autorisations d’accès à distance aux contrôleurs de domaine puissent être servir.  

-   Configurer l’audit des comptes d’administrateur  

    Une fois que vous avez sécurisé le compte administrateur d’un domaine et que vous l’avez désactivé, vous devez configurer l’audit pour surveiller les modifications apportées au compte. Si le compte est activé, son mot de passe est réinitialisé, ou toute autre modification apportée au compte, des alertes doivent être envoyées aux utilisateurs ou équipes responsables de l’administration de Active Directory, en plus des équipes de réponse aux incidents de votre organisation.  

#### <a name="step-by-step-instructions-to-secure-built-in-administrator-accounts-in-active-directory"></a>Instructions pas à pas pour sécuriser les comptes administrateur intégrés dans Active Directory  

1.  Dans **Gestionnaire de serveur**, cliquez sur **Outils**, puis sur **Active Directory les utilisateurs et les ordinateurs**.  

2.  Pour empêcher les attaques qui tirent parti de la délégation afin d’utiliser les informations d’identification du compte sur d’autres systèmes, procédez comme suit :  

    1.  Cliquez avec le bouton droit sur le compte **administrateur** , puis cliquez sur **Propriétés**.  

    2.  Cliquez sur l’onglet **compte** .  

    3.  Sous **options de compte**, sélectionnez le **compte est sensible et ne peut pas être** un indicateur délégué comme indiqué dans la capture d’écran suivante, puis cliquez sur **OK**.  

        ![sécurisation des comptes administrateur intégrés](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_24.gif)  

3.  Pour activer la **carte à puce requise pour l’indicateur d’ouverture de session interactive** sur le compte, procédez comme suit :  

    1.  Cliquez avec le bouton droit sur le compte **administrateur** et sélectionnez **Propriétés**.  

    2.  Cliquez sur l’onglet **compte** .  

    3.  Sous options de **compte** , sélectionnez la **carte à puce est nécessaire pour l’indicateur de connexion interactive** , comme indiqué dans la capture d’écran suivante, puis cliquez sur **OK**.  

        ![sécurisation des comptes administrateur intégrés](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_25.gif) 

##### <a name="configuring-gpos-to-restrict-administrator-accounts-at-the-domain-level"></a>Configuration des objets de stratégie de groupe pour restreindre les comptes administrateur au niveau du domaine  

> [!WARNING]  
> Cet objet de stratégie de groupe ne doit jamais être lié au niveau du domaine, car il peut rendre le compte administrateur intégré inutilisable, même en cas de récupération d’urgence.  

1.  Dans **Gestionnaire de serveur**, cliquez sur **Outils**, puis sur **gestion des stratégie de groupe**.  

2.  Dans l’arborescence de la console, développez <Forest> \ Domains @ no__t-1 @ no__t-2, puis **stratégie de groupe objets** (où <Forest> est le nom de la forêt et <Domain> est le nom du domaine dans lequel vous souhaitez créer le stratégie de groupe).  

3.  Dans l’arborescence de la console, cliquez avec le bouton droit sur **stratégie de groupe objets**, puis cliquez sur **nouveau**.  

    ![sécurisation des comptes administrateur intégrés](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_27.gif)  

4.  Dans la boîte de dialogue **nouvel objet GPO** , tapez <GPO Name>, puis cliquez sur **OK** (où <GPO Name> est le nom de cet objet de stratégie de groupe) comme indiqué dans la capture d’écran suivante.  

    ![sécurisation des comptes administrateur intégrés](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_28.gif)  

5.  Dans le volet d’informations, cliquez avec le bouton droit sur <GPO Name>, puis cliquez sur **modifier**.  

6.  Accédez à **ordinateur \ stratégies \ stratégies \ stratégies \ stratégies \ stratégies locales**, puis cliquez sur **attribution des droits utilisateur**.  

    ![sécurisation des comptes administrateur intégrés](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_29.gif)  

7.  Configurez les droits d’utilisateur pour empêcher le compte administrateur d’accéder aux serveurs et stations de travail membres sur le réseau en procédant comme suit :  

    1.  Double-cliquez sur **interdire l’accès à cet ordinateur à partir du réseau** , puis sélectionnez **définir ces paramètres de stratégie**.  

    2.  Cliquez sur **Ajouter un utilisateur ou un groupe** , puis sur **Parcourir**.  

    3.  Tapez **administrateur**, cliquez sur **vérifier les noms**, puis cliquez sur **OK**. Vérifiez que le compte est affiché au format <DomainName> \ nom d’utilisateur, comme indiqué dans la capture d’écran suivante.  

        ![sécurisation des comptes administrateur intégrés](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_30.gif)  

    4.  Cliquez sur **OK**, puis de nouveau sur **OK** .  

8.  Configurez les droits d’utilisateur pour empêcher le compte d’administrateur de se connecter en tant que traitement par lots en procédant comme suit :  

    1.  Double-cliquez sur **interdire l’ouverture de session en tant que tâche** , puis sélectionnez **définir ces paramètres de stratégie**.  

    2.  Cliquez sur **Ajouter un utilisateur ou un groupe** , puis sur **Parcourir**.  

    3.  Tapez **administrateur**, cliquez sur **vérifier les noms**, puis cliquez sur **OK**. Vérifiez que le compte est affiché au format <DomainName> \ nom d’utilisateur, comme indiqué dans la capture d’écran suivante.  

        ![sécurisation des comptes administrateur intégrés](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_31.gif)  

    4.  Cliquez sur **OK**, puis de nouveau sur **OK** .  

9. Configurez les droits d’utilisateur pour empêcher le compte d’administrateur de se connecter en tant que service en procédant comme suit :  

    1.  Double-cliquez sur **interdire l’ouverture de session en tant que service** , puis sélectionnez **définir ces paramètres de stratégie**.  

    2.  Cliquez sur **Ajouter un utilisateur ou un groupe** , puis sur **Parcourir**.  

    3.  Tapez **administrateur**, cliquez sur **vérifier les noms**, puis cliquez sur **OK**. Vérifiez que le compte est affiché au format <DomainName> \ nom d’utilisateur, comme indiqué dans la capture d’écran suivante.  

        ![sécurisation des comptes administrateur intégrés](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_32.gif)  

    4.  Cliquez sur **OK**, puis de nouveau sur **OK** .  

10. Configurez les droits d’utilisateur pour empêcher le compte BA d’accéder aux serveurs membres et aux stations de travail via Services Bureau à distance en procédant comme suit :  

    1.  Double-cliquez sur **interdire l’ouverture de session via services Bureau à distance** et sélectionnez **définir ces paramètres de stratégie**.  

    2.  Cliquez sur **Ajouter un utilisateur ou un groupe** , puis sur **Parcourir**.  

    3.  Tapez **administrateur**, cliquez sur **vérifier les noms**, puis cliquez sur **OK**. Vérifiez que le compte est affiché au format <DomainName> \ nom d’utilisateur, comme indiqué dans la capture d’écran suivante.  

        ![sécurisation des comptes administrateur intégrés](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_33.gif)  

    4.  Cliquez sur **OK**, puis de nouveau sur **OK** .  

11. Pour quitter **éditeur de gestion des stratégies de groupe**, cliquez sur **fichier**, puis sur **quitter**.  

12. Dans **stratégie de groupe gestion**, liez l’objet de stratégie de groupe aux unités d’organisation serveur et station de travail membres en procédant comme suit :  

    1.  Accédez au <Forest> \ Domains @ no__t-1 @ no__t-2 (où <Forest> est le nom de la forêt et <Domain> est le nom du domaine dans lequel vous souhaitez définir le stratégie de groupe).  

    2.  Cliquez avec le bouton droit sur l’unité d’organisation à laquelle l’objet de stratégie de groupe sera appliqué, puis cliquez sur **lier un objet de stratégie de groupe existant**.  

        ![sécurisation des comptes administrateur intégrés](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_34.gif)  

    3.  Sélectionnez l’objet de stratégie de groupe que vous avez créé, puis cliquez sur **OK**.  

        ![sécurisation des comptes administrateur intégrés](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_35.gif)  

    4.  Créez des liens vers toutes les autres unités d’organisation qui contiennent des stations de travail.  

    5.  Créez des liens vers toutes les autres unités d’organisation qui contiennent des serveurs membres.  

> [!IMPORTANT]  
> Lorsque vous ajoutez le compte administrateur à ces paramètres, vous spécifiez si vous configurez un compte d’administrateur local ou un compte d’administrateur de domaine selon la manière dont vous étiquetez les comptes. Par exemple, pour ajouter le compte administrateur du domaine TAILSPINTOYS à ces droits Deny, vous devez accéder au compte administrateur du domaine TAILSPINTOYS, qui apparaîtrait sous la forme TAILSPINTOYS\Administrator. Si vous tapez « administrateur » dans ces paramètres de droits d’utilisateur dans l’éditeur d’objets stratégie de groupe, vous limitez le compte d’administrateur local sur chaque ordinateur auquel l’objet de stratégie de groupe est appliqué, comme décrit précédemment.  

#### <a name="verification-steps"></a>Étapes de vérification  
Les étapes de vérification décrites ici sont spécifiques à Windows 8 et Windows Server 2012.  

##### <a name="verify-smart-card-is-required-for-interactive-logon-account-option"></a>Vérifier l’option de compte « la carte à puce est nécessaire pour l’ouverture de session interactive »  

1.  À partir de tout serveur membre ou station de travail affecté par les modifications de l’objet de stratégie de groupe, essayez de vous connecter de manière interactive au domaine à l’aide du compte administrateur intégré du domaine. Une fois que vous avez essayé d’ouvrir une session, une boîte de dialogue semblable à la suivante doit s’afficher.  

![sécurisation des comptes administrateur intégrés](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_36.gif)  

##### <a name="verify-account-is-disabled-account-option"></a>Vérifiez que l’option de compte « le compte est désactivé »  

1.  À partir de tout serveur membre ou station de travail affecté par les modifications de l’objet de stratégie de groupe, essayez de vous connecter de manière interactive au domaine à l’aide du compte administrateur intégré du domaine. Une fois que vous avez essayé d’ouvrir une session, une boîte de dialogue semblable à la suivante doit s’afficher.  

![sécurisation des comptes administrateur intégrés](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_37.gif)  

##### <a name="verify-deny-access-to-this-computer-from-the-network-gpo-settings"></a>Vérifier les paramètres de l’objet de stratégie de groupe « refuser l’accès à cet ordinateur à partir du réseau »  
À partir de n’importe quel serveur ou station de travail membre qui n’est pas affecté par les modifications de l’objet de stratégie de groupe (par exemple, un serveur de renvoi), essayez d’accéder à un serveur membre ou à une station de travail sur le réseau affecté par les modifications de l’objet Pour vérifier les paramètres de l’objet de stratégie de groupe, essayez de mapper le lecteur système à l’aide de la commande **net use** en effectuant les étapes suivantes :  

1.  Connectez-vous au domaine à l’aide du compte administrateur intégré du domaine.  

2.  À l’aide de la souris, déplacez le pointeur dans le coin supérieur droit ou en bas à droite de l’écran. Lorsque la barre des **icônes** s’affiche, cliquez sur **Rechercher**.  

3.  Dans la zone de **recherche** , tapez **invite de commandes**, cliquez avec le bouton droit sur invite de **commandes**, puis cliquez sur **exécuter en tant qu’administrateur** pour ouvrir une invite de commandes avec élévation de privilèges.  

4.  Lorsque vous êtes invité à approuver l’élévation, cliquez sur **Oui**.  

    ![sécurisation des comptes administrateur intégrés](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_38.gif)  

5.  Dans la fenêtre d' **invite de commandes** , tapez **net use \\ @ no__t-3 @ No__t-4Server Name @ no__t-5\c $** , où \<Server Name @ no__t-7 est le nom du serveur membre ou de la station de travail auquel vous essayez d’accéder sur le réseau.  

6.  La capture d’écran suivante montre le message d’erreur qui doit s’afficher.  

    ![sécurisation des comptes administrateur intégrés](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_39.gif)  

##### <a name="verify-deny-log-on-as-a-batch-job-gpo-settings"></a>Vérifier les paramètres de l’objet de stratégie de groupe « interdire l’ouverture d’une session en tant que tâche »  

À partir de n’importe quel serveur ou station de travail membre affecté par les modifications de l’objet de stratégie de groupe, connectez-vous localement.  

###### <a name="create-a-batch-file"></a>Créer un fichier de commandes  

1.  À l’aide de la souris, déplacez le pointeur dans le coin supérieur droit ou en bas à droite de l’écran. Lorsque la barre des **icônes** s’affiche, cliquez sur **Rechercher**.  

2.  Dans la zone de **recherche** , tapez **Notepad**, puis cliquez sur **bloc-notes**.  

3.  Dans **le bloc-notes**, tapez **dir c :** .  

4.  Cliquez sur **fichier** , puis sur **Enregistrer sous**.  

5.  Dans le champ **nom** de fichier, tapez **<Filename>. bat** (où <Filename> est le nom du nouveau fichier de commandes).  

###### <a name="schedule-a-task"></a>Planifier une tâche  

1.  À l’aide de la souris, déplacez le pointeur dans le coin supérieur droit ou en bas à droite de l’écran. Lorsque la barre des **icônes** s’affiche, cliquez sur **Rechercher**.  

2.  Dans la zone de **recherche** , tapez **Planificateur de tâches**, puis cliquez sur **Planificateur de tâches**.  

    > [!NOTE]  
    > Sur les ordinateurs exécutant Windows 8, dans la zone de recherche, tapez **planifier des tâches**, puis cliquez sur **planifier des tâches**.  

3.  Sur **Planificateur de tâches**, cliquez sur **action**, puis sur **créer une tâche**.  

4.  Dans la boîte de dialogue **créer une tâche** , tapez **<Task Name>** (où **<Task Name>** est le nom de la nouvelle tâche).  

5.  Cliquez sur l’onglet **actions** , puis sur **nouveau**.  

6.  Sous **action :** , sélectionnez **Démarrer un programme**.  

7.  Sous **programme/script :** , cliquez sur **Parcourir**, recherchez et sélectionnez le fichier de commandes créé dans la section « créer un fichier de commandes », puis cliquez sur **ouvrir**.  

8.  Cliquez sur **OK**.  

9. Cliquez sur l’onglet **Général**.  

10. Sous options de **sécurité** , cliquez sur **modifier l’utilisateur ou le groupe**.  

11. Tapez le nom du compte BA au niveau du domaine, cliquez sur **vérifier les noms**, puis cliquez sur **OK**.  

12. Sélectionnez **exécuter si l’utilisateur a ouvert une session ou non** et **ne pas stocker le mot de passe**. La tâche aura uniquement accès aux ressources de l’ordinateur local.  

13. Cliquez sur **OK**.  

14. Une boîte de dialogue doit s’afficher pour demander les informations d’identification du compte d’utilisateur pour exécuter la tâche.  

15. Après avoir entré les informations d’identification, cliquez sur **OK**.  

16. Une boîte de dialogue semblable à la suivante doit s’afficher.  

    ![sécurisation des comptes administrateur intégrés](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_40.gif)  

##### <a name="verify-deny-log-on-as-a-service-gpo-settings"></a>Vérifier les paramètres de l’objet de stratégie de groupe « interdire l’ouverture d’une session en tant que service »  

1.  À partir de n’importe quel serveur ou station de travail membre affecté par les modifications de l’objet de stratégie de groupe, connectez-vous localement.  

2.  À l’aide de la souris, déplacez le pointeur dans le coin supérieur droit ou en bas à droite de l’écran. Lorsque la barre des **icônes** s’affiche, cliquez sur **Rechercher**.  

3.  Dans la zone de **recherche** , tapez **services**, puis cliquez sur **services**.  

4.  Recherchez et double-cliquez sur **spouleur d’impression**.  

5.  Cliquez sur l'onglet **Se connecter**.  

6.  Sous **ouvrir une session en tant que :** , sélectionnez **ce compte**.  

7.  Cliquez sur **Parcourir**, tapez le nom du compte BA au niveau du domaine, cliquez sur **vérifier les noms**, puis cliquez sur **OK**.  

8.  Sous **mot de passe :** et **confirmer le mot de passe :** , tapez le mot de passe du compte d’administrateur, puis cliquez sur **OK**.  

9. Cliquez sur **OK** trois fois de plus.  

10. Cliquez avec le bouton droit sur le **service spouleur d’impression** , puis sélectionnez **redémarrer**.  

11. Une fois le service redémarré, une boîte de dialogue semblable à la suivante doit s’afficher.  

    ![sécurisation des comptes administrateur intégrés](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_41.gif)  

##### <a name="revert-changes-to-the-printer-spooler-service"></a>Annuler les modifications apportées au service spouleur d’impression  

1.  À partir de n’importe quel serveur ou station de travail membre affecté par les modifications de l’objet de stratégie de groupe, connectez-vous localement.  

2.  À l’aide de la souris, déplacez le pointeur dans le coin supérieur droit ou en bas à droite de l’écran. Lorsque la barre des **icônes** s’affiche, cliquez sur **Rechercher**.  

3.  Dans la zone de **recherche** , tapez **services**, puis cliquez sur **services**.  

4.  Recherchez et double-cliquez sur **spouleur d’impression**.  

5.  Cliquez sur l'onglet **Se connecter**.  

6.  Sous **ouvrir une session en tant que :** , sélectionnez le compte **système local** , puis cliquez sur **OK**.  

##### <a name="verify-deny-log-on-through-remote-desktop-services-gpo-settings"></a>Vérifier les paramètres de l’objet de stratégie de groupe « interdire l’ouverture d’une session via Services Bureau à distance »

1.  À l’aide de la souris, déplacez le pointeur dans le coin supérieur droit ou en bas à droite de l’écran. Lorsque la barre des **icônes** s’affiche, cliquez sur **Rechercher**.  

2.  Dans la zone de **recherche** , tapez **Connexion Bureau à distance**, puis cliquez sur **Connexion Bureau à distance**.  

3.  Dans le champ **ordinateur** , tapez le nom de l’ordinateur auquel vous souhaitez vous connecter, puis cliquez sur **se connecter**. (Vous pouvez également taper l’adresse IP à la place du nom de l’ordinateur.)  

4.  Lorsque vous y êtes invité, fournissez les informations d’identification pour le nom du compte BA au niveau du domaine.  

5.  Une boîte de dialogue semblable à la suivante doit s’afficher.  

    ![sécurisation des comptes administrateur intégrés](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_42.gif)  
