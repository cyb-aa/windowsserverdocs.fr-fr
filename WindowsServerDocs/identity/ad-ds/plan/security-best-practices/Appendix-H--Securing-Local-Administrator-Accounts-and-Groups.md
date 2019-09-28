---
ms.assetid: ea015cbc-dea9-4c72-a9d8-d6c826d07608
title: Annexe H-sécurisation des comptes et des groupes d’administrateurs locaux
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 7e0cff62851250009d8af6ec7d87ec8191dcaec0
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71408630"
---
# <a name="appendix-h-securing-local-administrator-accounts-and-groups"></a>Annexe H : Sécurisation des comptes et des groupes d’administrateurs locaux

>S'applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012


## <a name="appendix-h-securing-local-administrator-accounts-and-groups"></a>Annexe H : Sécurisation des comptes et des groupes d’administrateurs locaux  
Sur toutes les versions de Windows actuellement en support standard, le compte d’administrateur local est désactivé par défaut, ce qui rend le compte inutilisable pour les attaques Pass-The-hash et autres vols d’informations d’identification. Toutefois, dans les environnements qui contiennent des systèmes d’exploitation hérités ou dans lesquels les comptes d’administrateurs locaux ont été activés, ces comptes peuvent être utilisés comme décrit précédemment pour propager la compromission entre les serveurs membres et les stations de travail. Chaque compte et groupe d’administrateur local doit être sécurisé, comme décrit dans les instructions pas à pas qui suivent.  

Pour plus d’informations sur les considérations relatives à la sécurisation des groupes d’administration intégrés, consultez [implémentation de modèles d’administration à privilèges faibles](../../../ad-ds/plan/security-best-practices/Implementing-Least-Privilege-Administrative-Models.md).  

#### <a name="controls-for-local-administrator-accounts"></a>Contrôles pour les comptes d’administrateur local  
Pour le compte d’administrateur local dans chaque domaine de votre forêt, vous devez configurer les paramètres suivants :  

-   Configurer des objets de stratégie de groupe pour limiter l’utilisation du compte administrateur du domaine sur les systèmes joints à un domaine  
    -   Dans un ou plusieurs objets de stratégie de groupe que vous créez et liez à des unités d’organisation de station de travail et de serveur membre dans chaque domaine, ajoutez le compte d’administrateur aux droits d’utilisateur suivants dans la stratégie d’autorisation \ paramètres d’utilisateur \ **stratégies d’autorisation Attributions**:  

        -   Interdire l’accès à cet ordinateur à partir du réseau  

        -   Interdire l’ouverture de session en tant que tâche  

        -   Interdire l’ouverture de session en tant que service  

        -   Interdire l’ouverture de session par les services Bureau à distance  

#### <a name="step-by-step-instructions-to-secure-local-administrators-groups"></a>Instructions pas à pas pour sécuriser les groupes d’administrateurs locaux  

###### <a name="configuring-gpos-to-restrict-administrator-account-on-domain-joined-systems"></a>Configuration des objets de stratégie de groupe pour restreindre le compte administrateur sur les systèmes joints à un domaine  

1.  Dans **Gestionnaire de serveur**, cliquez sur **Outils**, puis sur **gestion des stratégie de groupe**.  

2.  Dans l’arborescence de la console, développez <Forest> \ Domains @ no__t-1 @ no__t-2, puis **stratégie de groupe objets** (où <Forest> est le nom de la forêt et <Domain> est le nom du domaine dans lequel vous souhaitez définir la stratégie de groupe).  

3.  Dans l’arborescence de la console, cliquez avec le bouton droit sur **stratégie de groupe objets**, puis cliquez sur **nouveau**.  

    ![sécuriser les comptes et les groupes d’administrateurs locaux](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_101.png)  

4.  Dans la boîte de dialogue **nouvel objet GPO** , tapez **<GPO Name>** , puis cliquez sur **OK** (où <GPO Name> est le nom de cet objet de stratégie de groupe).  

    ![sécuriser les comptes et les groupes d’administrateurs locaux](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_102.png)  

5.  Dans le volet d’informations, cliquez avec le bouton droit sur **<GPO Name>** , puis cliquez sur **modifier**.  

6.  Accédez à **ordinateur \ stratégies \ stratégies \ stratégies \ stratégies \ stratégies locales**, puis cliquez sur **attribution des droits utilisateur**.  

    ![sécuriser les comptes et les groupes d’administrateurs locaux](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_103.png)  

7.  Configurez les droits d’utilisateur pour empêcher le compte d’administrateur local d’accéder aux serveurs et stations de travail membres sur le réseau en procédant comme suit :  

    1.  Double-cliquez sur **interdire l’accès à cet ordinateur à partir du réseau** , puis sélectionnez **définir ces paramètres de stratégie**.  

    2.  Cliquez sur **Ajouter un utilisateur ou un groupe**, tapez le nom d’utilisateur du compte d’administrateur local, puis cliquez sur **OK**. Ce nom d’utilisateur sera **administrateur**, la valeur par défaut lors de l’installation de Windows.  

        ![sécuriser les comptes et les groupes d’administrateurs locaux](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_104.png)  

    3.  Cliquez sur OK.  

        > [!IMPORTANT]  
        > Lorsque vous ajoutez le compte administrateur à ces paramètres, vous spécifiez si vous configurez un compte d’administrateur local ou un compte d’administrateur de domaine selon la manière dont vous étiquetez les comptes. Par exemple, pour ajouter le compte administrateur du domaine TAILSPINTOYS à ces droits Deny, vous devez accéder au compte administrateur du domaine TAILSPINTOYS, qui apparaîtrait sous la forme TAILSPINTOYS\Administrator. Si vous tapez **administrateur** dans ces paramètres de droits d’utilisateur dans l’éditeur d’objets de stratégie de groupe, vous limitez le compte d’administrateur local sur chaque ordinateur auquel l’objet de stratégie de groupe est appliqué, comme décrit précédemment.  

8.  Configurez les droits d’utilisateur pour empêcher le compte d’administrateur local de se connecter en tant que traitement par lots en procédant comme suit :  

    1.  Double-cliquez sur **interdire l’ouverture de session en tant que tâche** , puis sélectionnez **définir ces paramètres de stratégie**.  

    2.  Cliquez sur **Ajouter un utilisateur ou un groupe**, tapez le nom d’utilisateur du compte d’administrateur local, puis cliquez sur **OK**. Ce nom d’utilisateur sera **administrateur**, la valeur par défaut lors de l’installation de Windows.  

        ![sécuriser les comptes et les groupes d’administrateurs locaux](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_105.png)  

    3.  Cliquez sur **OK**.  

        > [!IMPORTANT]  
        > Lorsque vous ajoutez le compte administrateur à ces paramètres, vous spécifiez si vous configurez un compte administrateur local ou un compte d’administrateur de domaine en vous sur la manière dont vous étiquetez les comptes. Par exemple, pour ajouter le compte administrateur du domaine TAILSPINTOYS à ces droits Deny, vous devez accéder au compte administrateur du domaine TAILSPINTOYS, qui apparaîtrait sous la forme TAILSPINTOYS\Administrator. Si vous tapez **administrateur** dans ces paramètres de droits d’utilisateur dans l’éditeur d’objets de stratégie de groupe, vous limitez le compte d’administrateur local sur chaque ordinateur auquel l’objet de stratégie de groupe est appliqué, comme décrit précédemment.  

9. Configurez les droits d’utilisateur pour empêcher le compte d’administrateur local de se connecter en tant que service en procédant comme suit :  

    1.  Double-cliquez sur **interdire l’ouverture de session en tant que service** , puis sélectionnez **définir ces paramètres de stratégie**.  

    2.  Cliquez sur **Ajouter un utilisateur ou un groupe**, tapez le nom d’utilisateur du compte d’administrateur local, puis cliquez sur **OK**. Ce nom d’utilisateur sera **administrateur**, la valeur par défaut lors de l’installation de Windows.  

        ![sécuriser les comptes et les groupes d’administrateurs locaux](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_106.png)  

    3.  Cliquez sur **OK**.  

        > [!IMPORTANT]  
        > Lorsque vous ajoutez le compte administrateur à ces paramètres, vous spécifiez si vous configurez un compte administrateur local ou un compte d’administrateur de domaine en vous sur la manière dont vous étiquetez les comptes. Par exemple, pour ajouter le compte administrateur du domaine TAILSPINTOYS à ces droits Deny, vous devez accéder au compte administrateur du domaine TAILSPINTOYS, qui apparaîtrait sous la forme TAILSPINTOYS\Administrator. Si vous tapez **administrateur** dans ces paramètres de droits d’utilisateur dans l’éditeur d’objets de stratégie de groupe, vous limitez le compte d’administrateur local sur chaque ordinateur auquel l’objet de stratégie de groupe est appliqué, comme décrit précédemment.  

10. Configurez les droits d’utilisateur pour empêcher le compte d’administrateur local d’accéder aux serveurs membres et aux stations de travail via Services Bureau à distance en procédant comme suit :  

    1.  Double-cliquez sur **interdire l’ouverture de session via services Bureau à distance** et sélectionnez **définir ces paramètres de stratégie**.  

    2.  Cliquez sur **Ajouter un utilisateur ou un groupe**, tapez le nom d’utilisateur du compte d’administrateur local, puis cliquez sur **OK**. Ce nom d’utilisateur sera **administrateur**, la valeur par défaut lors de l’installation de Windows.  

        ![sécuriser les comptes et les groupes d’administrateurs locaux](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_107.png)  

    3.  Cliquez sur **OK**.  

        > [!IMPORTANT]  
        > Lorsque vous ajoutez le compte administrateur à ces paramètres, vous spécifiez si vous configurez un compte administrateur local ou un compte d’administrateur de domaine en vous sur la manière dont vous étiquetez les comptes. Par exemple, pour ajouter le compte administrateur du domaine TAILSPINTOYS à ces droits Deny, vous devez accéder au compte administrateur du domaine TAILSPINTOYS, qui apparaîtrait sous la forme TAILSPINTOYS\Administrator. Si vous tapez **administrateur** dans ces paramètres de droits d’utilisateur dans l’éditeur d’objets de stratégie de groupe, vous limitez le compte d’administrateur local sur chaque ordinateur auquel l’objet de stratégie de groupe est appliqué, comme décrit précédemment.  

11. Pour quitter **éditeur de gestion des stratégies de groupe**, cliquez sur **fichier**, puis sur **quitter**.  

12. Dans **stratégie de groupe gestion**, liez l’objet de stratégie de groupe aux unités d’organisation serveur et station de travail membres en procédant comme suit :  

    1.  Accédez au <Forest> \ Domains @ no__t-1 @ no__t-2 (où <Forest> est le nom de la forêt et <Domain> est le nom du domaine dans lequel vous souhaitez définir le stratégie de groupe).  

    2.  Cliquez avec le bouton droit sur l’unité d’organisation à laquelle l’objet de stratégie de groupe sera appliqué, puis cliquez sur **lier un objet de stratégie de groupe existant**.  

        ![sécuriser les comptes et les groupes d’administrateurs locaux](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_108.png)  

    3.  Sélectionnez l’objet de stratégie de groupe que vous avez créé, puis cliquez sur **OK**.  

        ![sécuriser les comptes et les groupes d’administrateurs locaux](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_109.png)  

    4.  Créez des liens vers toutes les autres unités d’organisation qui contiennent des stations de travail.  

    5.  Créez des liens vers toutes les autres unités d’organisation qui contiennent des serveurs membres.  

#### <a name="verification-steps"></a>Étapes de vérification  

##### <a name="verify-deny-access-to-this-computer-from-the-network-gpo-settings"></a>Vérifier les paramètres de l’objet de stratégie de groupe « refuser l’accès à cet ordinateur à partir du réseau »  

À partir de n’importe quel serveur ou station de travail membre qui n’est pas affecté par les modifications de l’objet de stratégie de groupe (par exemple, un serveur de renvoi), essayez d’accéder à un serveur membre ou à une station de travail sur le réseau affecté par les modifications de l’objet Pour vérifier les paramètres de l’objet de stratégie de groupe, essayez de mapper le lecteur système à l’aide de la commande **net use** .  

1.  Connectez-vous localement à n’importe quel serveur ou station de travail membre qui n’est pas affecté par les modifications de l’objet de stratégie de groupe.  

2.  À l’aide de la souris, déplacez le pointeur dans le coin supérieur droit ou en bas à droite de l’écran. Lorsque la barre des **icônes** s’affiche, cliquez sur **Rechercher**.  

3.  Dans la zone de **recherche** , tapez **invite de commandes**, cliquez avec le bouton droit sur invite de **commandes**, puis cliquez sur **exécuter en tant qu’administrateur** pour ouvrir une invite de commandes avec élévation de privilèges.  

4.  Lorsque vous êtes invité à approuver l’élévation, cliquez sur **Oui**.  

    ![sécuriser les comptes et les groupes d’administrateurs locaux](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_110.png)  

5.  Dans la fenêtre d' **invite de commandes** , tapez **net use \\ @ no__t-3 @ no__t-4\c $/user : <Server Name> \ Administrator**, où <Server Name> est le nom du serveur membre ou de la station de travail auquel vous essayez d’accéder sur le réseau.  

    > [!NOTE]  
    > Les informations d’identification de l’administrateur local doivent provenir du même système que celui auquel vous essayez d’accéder sur le réseau.  

6.  La capture d’écran suivante montre le message d’erreur qui doit s’afficher.  

    ![sécuriser les comptes et les groupes d’administrateurs locaux](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_111.png)  

##### <a name="verify-deny-log-on-as-a-batch-job-gpo-settings"></a>Vérifier les paramètres de l’objet de stratégie de groupe « interdire l’ouverture d’une session en tant que tâche »  
À partir de n’importe quel serveur ou station de travail membre affecté par les modifications de l’objet de stratégie de groupe, connectez-vous localement.  

###### <a name="create-a-batch-file"></a>Créer un fichier de commandes  

1.  À l’aide de la souris, déplacez le pointeur dans le coin supérieur droit ou en bas à droite de l’écran. Lorsque la barre des **icônes** s’affiche, cliquez sur **Rechercher**.  

2.  Dans la zone de **recherche** , tapez **Notepad**, puis cliquez sur **bloc-notes**.  

3.  Dans **le bloc-notes**, tapez **dir c :** .  

4.  Cliquez sur **fichier**, puis sur **Enregistrer sous**.  

5.  Dans la zone **nom de fichier** , tapez @no__t- **2. bat** (où <Filename> est le nom du nouveau fichier de commandes).  

###### <a name="schedule-a-task"></a>Planifier une tâche  

1.  À l’aide de la souris, déplacez le pointeur dans le coin supérieur droit ou en bas à droite de l’écran. Lorsque la barre des **icônes** s’affiche, cliquez sur **Rechercher**.  

2.  Dans la zone de **recherche** , tapez planificateur de tâches, puis cliquez sur **Planificateur de tâches**.  

    > [!NOTE]  
    > Sur les ordinateurs exécutant Windows 8, dans la zone de **recherche** , tapez **planifier des tâches**, puis cliquez sur **planifier des tâches**.  

3.  Cliquez sur **action**, puis sur **créer une tâche**.  

4.  Dans la boîte de dialogue **créer une tâche** , tapez **<Task Name>** (où <Task Name> est le nom de la nouvelle tâche).  

5.  Cliquez sur l’onglet **actions** , puis sur **nouveau**.  

6.  Dans le champ **action** , cliquez sur **Démarrer un programme**.  

7.  Dans le champ **programme/script** , cliquez sur **Parcourir**, recherchez et sélectionnez le fichier de commandes créé dans la section **créer un fichier de commandes** , puis cliquez sur **ouvrir**.  

8.  Cliquez sur **OK**.  

9. Cliquez sur l’onglet **Général**.  

10. Dans le champ **options de sécurité** , cliquez sur modifier l' **utilisateur ou le groupe**.  

11. Tapez le nom du compte d’administrateur local du système, cliquez sur **vérifier les noms**, puis cliquez sur **OK**.  

12. Sélectionnez **exécuter si l’utilisateur a ouvert une session ou non** et **ne pas stocker le mot de passe**. La tâche aura uniquement accès aux ressources de l’ordinateur local.  

13. Cliquez sur **OK**.  

14. Une boîte de dialogue doit s’afficher pour demander les informations d’identification du compte d’utilisateur pour exécuter la tâche.  

15. Après avoir entré les informations d’identification, cliquez sur **OK**.  

16. Une boîte de dialogue semblable à la suivante doit s’afficher.  

    ![sécuriser les comptes et les groupes d’administrateurs locaux](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_112.png)  

###### <a name="verify-deny-log-on-as-a-service-gpo-settings"></a>Vérifier les paramètres de l’objet de stratégie de groupe « interdire l’ouverture d’une session en tant que service »  

1.  À partir de n’importe quel serveur ou station de travail membre affecté par les modifications de l’objet de stratégie de groupe, connectez-vous localement.  

2.  À l’aide de la souris, déplacez le pointeur dans le coin supérieur droit ou en bas à droite de l’écran. Lorsque la barre des **icônes** s’affiche, cliquez sur **Rechercher**.  

3.  Dans la zone de **recherche** , tapez **services**, puis cliquez sur **services**.  

4.  Recherchez et double-cliquez sur **spouleur d’impression**.  

5.  Cliquez sur l'onglet **Se connecter**.  

6.  Dans le champ **ouvrir une session en tant que** , cliquez sur **ce compte**.  

7.  Cliquez sur **Parcourir**, tapez le compte d’administrateur local du système, cliquez sur **vérifier les noms**, puis cliquez sur **OK**.  

8.  Dans les champs **mot** de passe et **confirmer le mot de passe** , tapez le mot de passe du compte sélectionné, puis cliquez sur **OK**.  

9. Cliquez sur **OK** trois fois de plus.  

10. Cliquez avec le bouton droit sur **spouleur d’impression** , puis cliquez sur **redémarrer**.  

11. Une fois le service redémarré, une boîte de dialogue semblable à la suivante doit s’afficher.  

    ![sécuriser les comptes et les groupes d’administrateurs locaux](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_113.png)  

###### <a name="revert-changes-to-the-printer-spooler-service"></a>Annuler les modifications apportées au service spouleur d’impression  

1.  À partir de n’importe quel serveur ou station de travail membre affecté par les modifications de l’objet de stratégie de groupe, connectez-vous localement.  

2.  À l’aide de la souris, déplacez le pointeur dans le coin supérieur droit ou en bas à droite de l’écran. Lorsque la barre des **icônes** s’affiche, cliquez sur **Rechercher**.  

3.  Dans la zone de **recherche** , tapez **services**, puis cliquez sur **services**.  

4.  Recherchez et double-cliquez sur **spouleur d’impression**.  

5.  Cliquez sur l'onglet **Se connecter**.  

6.  Dans le champ **ouvrir une session en tant que**, sélectionnez **local SystemAccount**, puis cliquez sur **OK**.  

###### <a name="verify-deny-log-on-through-remote-desktop-services-gpo-settings"></a>Vérifier les paramètres de l’objet de stratégie de groupe « interdire l’ouverture d’une session via Services Bureau à distance »  

1.  À l’aide de la souris, déplacez le pointeur dans le coin supérieur droit ou en bas à droite de l’écran. Lorsque la barre des **icônes** s’affiche, cliquez sur **Rechercher**.  

2.  Dans la zone de **recherche** , tapez **Connexion Bureau à distance**, puis cliquez sur **Connexion Bureau à distance**.  

3.  Dans le champ **ordinateur** , tapez le nom de l’ordinateur auquel vous souhaitez vous connecter, puis cliquez sur **se connecter**. (Vous pouvez également taper l’adresse IP à la place du nom de l’ordinateur.)  

4.  Lorsque vous y êtes invité, fournissez les informations d’identification du compte d' **administrateur** local du système.  

5.  Une boîte de dialogue semblable à la suivante doit s’afficher.  

    ![sécuriser les comptes et les groupes d’administrateurs locaux](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_114.png)  
