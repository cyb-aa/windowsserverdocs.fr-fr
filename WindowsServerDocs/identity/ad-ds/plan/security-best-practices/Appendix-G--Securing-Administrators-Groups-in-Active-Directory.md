---
ms.assetid: 4baefbd3-038f-44c0-85ba-f24e9722b757
title: 'Annexe G : sécurisation des groupes d’administrateurs dans Active Directory'
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: f113dc7fc5b131a2c0ef10433125ef12a775707c
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80821522"
---
# <a name="appendix-g-securing-administrators-groups-in-active-directory"></a>AnnexeG: Sécurisation des groupes d’administrateurs dans Active Directory

>S’applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012


## <a name="appendix-g-securing-administrators-groups-in-active-directory"></a>AnnexeG: Sécurisation des groupes d’administrateurs dans Active Directory  
Comme c’est le cas avec les groupes Administrateurs de l’entreprise (EA) et Admins du domaine (DA), l’appartenance au groupe Administrateurs intégré (BA) doit être requise uniquement dans les scénarios de génération ou de récupération d’urgence. Il ne doit y avoir aucun compte d’utilisateur quotidien dans le groupe administrateurs, à l’exception du compte administrateur intégré pour le domaine, s’il a été sécurisé, comme décrit dans l' [annexe D : sécurisation des comptes administrateur intégrés dans Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory.md).  

Les administrateurs sont, par défaut, les propriétaires de la plupart des objets AD DS dans leurs domaines respectifs. L’appartenance à ce groupe peut être nécessaire dans les scénarios de génération ou de récupération d’urgence dans lesquels la propriété ou la possibilité de prendre possession d’objets est requise. En outre, les DAs et EAs héritent d’un certain nombre de leurs droits et autorisations en raison de leur appartenance par défaut au groupe administrateurs. L’imbrication de groupe par défaut pour les groupes privilégiés dans Active Directory ne doit pas être modifiée, et chaque groupe administrateurs de domaine doit être sécurisé comme décrit dans les instructions pas à pas qui suivent.  

Pour le groupe administrateurs dans chaque domaine de la forêt :  

1.  Supprimez tous les membres du groupe administrateurs, à l’exception possible du compte administrateur intégré pour le domaine, à condition qu’il ait été sécurisé, comme décrit dans l' [annexe D : sécurisation des comptes administrateur intégrés dans Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory.md).  

2.  Dans les objets de stratégie de groupe liés à des unités d’organisation contenant des serveurs membres et des stations de travail dans chaque domaine, le groupe BA doit être ajouté aux droits d’utilisateur suivants dans la stratégie d’autorisation **\ stratégies \ stratégies \ autorisations \ utilisateurs \ attribution des droits utilisateur**:  

    -   Refuser l'accès à un ordinateur à partir du réseau  

    -   Refuser l'ouverture de session en tant que tâche  

    -   Refuser l'ouverture de session en tant que service  

3.  Au niveau de l’unité d’organisation contrôleurs de domaine dans chaque domaine de la forêt, le groupe administrateurs doit disposer des droits d’utilisateur suivants :  

    -   Accéder à cet ordinateur à partir du réseau  

    -   Permettre l'ouverture d'une session locale  

    -   Autoriser l’ouverture de session par les services Bureau à distance  

4.  L’audit doit être configuré pour envoyer des alertes si des modifications sont apportées aux propriétés ou à l’appartenance au groupe administrateurs.  

#### <a name="step-by-step-instructions-for-removing-all-members-from-the-administrators-group"></a>Instructions pas à pas pour la suppression de tous les membres du groupe administrateurs  

1.  Dans **Gestionnaire de serveur**, cliquez sur **Outils**, puis sur **Active Directory les utilisateurs et les ordinateurs**.  

2.  Pour supprimer tous les membres du groupe administrateurs, procédez comme suit :  

    1.  Double-cliquez sur le groupe **administrateurs** et cliquez sur l’onglet **membres** .  

        ![groupes d’administration sécurisés](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_79.gif)  

    2.  Sélectionnez un membre du groupe, cliquez sur **supprimer**, sur **Oui**, puis sur **OK**.  

3.  Répétez l’étape 2 jusqu’à ce que tous les membres du groupe administrateurs aient été supprimés.  

#### <a name="step-by-step-instructions-to-secure-administrators-groups-in-active-directory"></a>Instructions pas à pas pour sécuriser les groupes d’administrateurs dans Active Directory  

1.  Dans **Gestionnaire de serveur**, cliquez sur **Outils**, puis sur **gestion des stratégie de groupe**.  

2.  Dans l’arborescence de la console, développez &lt;forêt&gt;\Domains\\&lt;domaine&gt;, puis **stratégie de groupe objets** (où &lt;forêt&gt; est le nom de la forêt et &lt;domaine&gt; est le nom du domaine dans lequel vous souhaitez définir la stratégie de groupe).  

3.  Dans l’arborescence de la console, cliquez avec le bouton droit sur **stratégie de groupe objets**, puis cliquez sur **nouveau**.  

    ![groupes d’administration sécurisés](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_80.gif)  

4.  Dans la boîte de dialogue **nouvel objet GPO** , tapez <GPO Name>, puis cliquez sur **OK** (où nom de l' *objet de stratégie* de groupe est le nom de cet objet de stratégie de groupe).  

    ![groupes d’administration sécurisés](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_81.gif)  

5.  Dans le volet d’informations, cliquez avec le bouton droit sur **<GPO Name>** , puis cliquez sur **modifier**.  

6.  Accédez à **ordinateur \ stratégies \ stratégies \ stratégies \ stratégies \ stratégies locales**, puis cliquez sur **attribution des droits utilisateur**.  

    ![groupes d’administration sécurisés](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_82.gif)  

7.  Configurez les droits d’utilisateur pour empêcher les membres du groupe administrateurs d’accéder aux serveurs membres et aux stations de travail sur le réseau en procédant comme suit :  

    1.  Double-cliquez sur **interdire l’accès à cet ordinateur à partir du réseau** , puis sélectionnez **définir ces paramètres de stratégie**.  

    2.  Cliquez sur **Ajouter un utilisateur ou un groupe** , puis sur **Parcourir**.  

    3.  Tapez **administrateurs**, cliquez sur **vérifier les noms**, puis cliquez sur **OK**.  

        ![groupes d’administration sécurisés](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_83.gif)  

    4.  Cliquez sur **OK**, puis de nouveau sur **OK** .  

8.  Configurez les droits d’utilisateur pour empêcher les membres du groupe administrateurs de se connecter en tant que traitement par lots en procédant comme suit :  

    1.  Double-cliquez sur **interdire l’ouverture de session en tant que tâche** , puis sélectionnez **définir ces paramètres de stratégie**.  

    2.  Cliquez sur **Ajouter un utilisateur ou un groupe** , puis sur **Parcourir**.  

    3.  Tapez **administrateurs**, cliquez sur **vérifier les noms**, puis cliquez sur **OK**.  

        ![groupes d’administration sécurisés](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_84.gif)  

    4.  Cliquez sur **OK**, puis de nouveau sur **OK** .  

9. Configurez les droits d’utilisateur pour empêcher les membres du groupe administrateurs de se connecter en tant que service en procédant comme suit :  

    1.  Double-cliquez sur **interdire l’ouverture de session en tant que service** , puis sélectionnez **définir ces paramètres de stratégie**.  

    2.  Cliquez sur **Ajouter un utilisateur ou un groupe** , puis sur **Parcourir**.  

    3.  Tapez **administrateurs**, cliquez sur **vérifier les noms**, puis cliquez sur **OK**.  

        ![groupes d’administration sécurisés](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_85.gif)  

    4.  Cliquez sur **OK**, puis de nouveau sur **OK** .  

10. Pour quitter **éditeur de gestion des stratégies de groupe**, cliquez sur **fichier**, puis sur **quitter**.  

11. Dans **stratégie de groupe gestion**, liez l’objet de stratégie de groupe aux unités d’organisation serveur et station de travail membres en procédant comme suit :  

    1.  Accédez à la forêt &lt;&gt;> \Domains\\&lt;domaine&gt; (où &lt;forêt&gt; est le nom de la forêt et &lt;domaine&gt; est le nom du domaine dans lequel vous souhaitez définir la stratégie de groupe).  

    2.  Cliquez avec le bouton droit sur l’unité d’organisation à laquelle l’objet de stratégie de groupe sera appliqué, puis cliquez sur **lier un objet de stratégie de groupe existant**.  

        ![groupes d’administration sécurisés](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_86.gif)  

    3.  Sélectionnez l’objet de stratégie de groupe que vous venez de créer, puis cliquez sur **OK**.  

        ![groupes d’administration sécurisés](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_87.gif)  

    4.  Créez des liens vers toutes les autres unités d’organisation qui contiennent des stations de travail.  

    5.  Créez des liens vers toutes les autres unités d’organisation qui contiennent des serveurs membres.  

        > [!IMPORTANT]  
        > Si les serveurs de saut sont utilisés pour administrer des contrôleurs de domaine et des Active Directory, assurez-vous que les serveurs de saut se trouvent dans une UO à laquelle ces objets de stratégie de groupe ne sont pas liés.  

        > [!NOTE]  
        > Lorsque vous implémentez des restrictions sur le groupe administrateurs dans les objets de stratégie de groupe, Windows applique les paramètres aux membres du groupe Administrateurs local d’un ordinateur en plus du groupe administrateurs du domaine. Par conséquent, vous devez être prudent lors de l’implémentation de restrictions dans le groupe administrateurs. Bien que l’interdiction des ouvertures de session de réseau, de traitement et de service pour les membres du groupe administrateurs soit recommandée partout où il est possible d’implémenter, ne limitez pas les ouvertures de session locales ou les ouvertures de session via Services Bureau à distance. Le blocage de ces types d’ouverture de session peut bloquer l’administration légitime d’un ordinateur par les membres du groupe Administrateurs local.  
        >   
        > La capture d’écran suivante montre les paramètres de configuration qui bloquent la mauvaise utilisation des comptes d’administrateur de domaine et locaux intégrés, en plus de la mauvaise utilisation des groupes d’administrateurs de domaine ou locaux intégrés. Notez que le droit d’utilisateur **interdire l’ouverture de session via services Bureau à distance** n’inclut pas le groupe administrateurs, car son inclusion dans ce paramètre bloque également ces connexions pour les comptes qui sont membres du groupe administrateurs de l’ordinateur local. Si les services sur les ordinateurs sont configurés pour s’exécuter dans le contexte de l’un des groupes privilégiés décrits dans cette section, l’implémentation de ces paramètres peut entraîner l’échec des services et des applications. Par conséquent, comme pour toutes les recommandations de cette section, vous devez tester minutieusement les paramètres de mise en application dans votre environnement.  

        ![groupes d’administration sécurisés](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_88.gif)  

#### <a name="step-by-step-instructions-to-grant-user-rights-to-the-administrators-group"></a>Instructions pas à pas pour accorder des droits d’utilisateur au groupe administrateurs  

1.  Dans **Gestionnaire de serveur**, cliquez sur **Outils**, puis sur **gestion des stratégie de groupe**.  

2.  Dans l’arborescence de la console, développez <Forest>\Domains\\<Domain>, puis **stratégie de groupe objets** (où <Forest> est le nom de la forêt et <Domain> le nom du domaine dans lequel vous souhaitez définir la stratégie de groupe).  

3.  Dans l’arborescence de la console, cliquez avec le bouton droit sur **stratégie de groupe objets**, puis cliquez sur **nouveau**.  

    ![groupes d’administration sécurisés](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_89.gif)  

4.  Dans la boîte de dialogue **nouvel objet GPO** , tapez <GPO Name>, puis cliquez sur **OK** (où <GPO Name> est le nom de cet objet de stratégie de groupe).  

    ![groupes d’administration sécurisés](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_90.gif)  

5.  Dans le volet d’informations, cliquez avec le bouton droit sur **<GPO Name>** , puis cliquez sur **modifier**.  

6.  Accédez à **ordinateur \ stratégies \ stratégies \ stratégies \ stratégies \ stratégies locales**, puis cliquez sur **attribution des droits utilisateur**.  

    ![groupes d’administration sécurisés](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_91.gif)  

7.  Configurez les droits d’utilisateur pour permettre aux membres du groupe administrateurs d’accéder aux contrôleurs de domaine sur le réseau en procédant comme suit :  

    1.  Double-cliquez sur **accès à cet ordinateur à partir du réseau** , puis sélectionnez **définir ces paramètres de stratégie**.  

    2.  Cliquez sur **Ajouter un utilisateur ou un groupe** , puis sur **Parcourir**.  

    3.  Cliquez sur **Ajouter un utilisateur ou un groupe** , puis sur **Parcourir**.  

        ![groupes d’administration sécurisés](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_92.gif)  

    4.  Cliquez sur **OK**, puis de nouveau sur **OK** .  

8.  Configurez les droits d’utilisateur pour permettre aux membres du groupe administrateurs de se connecter localement en procédant comme suit :  

    1.  Double-cliquez sur **permettre l’ouverture d’une session locale** , puis sélectionnez **définir ces paramètres de stratégie**.  

    2.  Cliquez sur **Ajouter un utilisateur ou un groupe** , puis sur **Parcourir**.  

    3.  Tapez **administrateurs**, cliquez sur vérifier les **noms**, puis cliquez sur **OK**.  

        ![groupes d’administration sécurisés](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_93.gif)  

    4.  Cliquez sur **OK**, puis de nouveau sur **OK** .  

9. Configurez les droits d’utilisateur pour permettre aux membres du groupe administrateurs de se connecter via Services Bureau à distance en procédant comme suit :  

    1.  Double-cliquez sur **autoriser l’ouverture de session via services Bureau à distance** et sélectionnez **définir ces paramètres de stratégie**.  

    2.  Cliquez sur **Ajouter un utilisateur ou un groupe** , puis sur **Parcourir**.  

    3.  Tapez **administrateurs**, cliquez sur **vérifier les noms**, puis cliquez sur **OK**.  

        ![groupes d’administration sécurisés](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_94.gif)  

    4.  Cliquez sur **OK**, puis de nouveau sur **OK** .  

10. Pour quitter **éditeur de gestion des stratégies de groupe**, cliquez sur **fichier**, puis sur **quitter**.  

11. Dans **stratégie de groupe gestion**, liez l’objet de stratégie de groupe à l’unité d’organisation contrôleurs de domaine en procédant comme suit :  

    1.  Accédez au <Forest>\Domains\\<Domain> (où <Forest> est le nom de la forêt et <Domain> est le nom du domaine dans lequel vous souhaitez définir la stratégie de groupe).  

    2.  Cliquez avec le bouton droit sur l’unité d’organisation contrôleurs de domaine et cliquez sur **lier un objet de stratégie de groupe existant**.  

        ![groupes d’administration sécurisés](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_95.gif)  

    3.  Sélectionnez l’objet de stratégie de groupe que vous venez de créer, puis cliquez sur **OK**.  

        ![groupes d’administration sécurisés](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_96.gif)  

#### <a name="verification-steps"></a>Étapes de vérification  

##### <a name="verify-deny-access-to-this-computer-from-the-network-gpo-settings"></a>Vérifier les paramètres de l’objet de stratégie de groupe « refuser l’accès à cet ordinateur à partir du réseau »  
À partir de n’importe quel serveur ou station de travail membre qui n’est pas affecté par les modifications de l’objet de stratégie de groupe (par exemple, un « serveur de saut »), essayez d’accéder à un serveur membre ou à une station de travail sur le réseau affecté par les modifications de l’objet de stratégie de groupe Pour vérifier les paramètres de l’objet de stratégie de groupe, essayez de mapper le lecteur système à l’aide de la commande **net use** .  

1.  Connectez-vous localement à l’aide d’un compte membre du groupe administrateurs.  

2.  À l’aide de la souris, déplacez le pointeur dans le coin supérieur droit ou en bas à droite de l’écran. Lorsque la barre des **icônes** s’affiche, cliquez sur **Rechercher**.  

3.  Dans la zone de **recherche** , tapez **invite de commandes**, cliquez avec le bouton droit sur invite de **commandes**, puis cliquez sur **exécuter en tant qu’administrateur** pour ouvrir une invite de commandes avec élévation de privilèges.  

4.  Lorsque vous êtes invité à approuver l’élévation, cliquez sur **Oui**.  

    ![groupes d’administration sécurisés](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_97.gif)  

5.  Dans la fenêtre d' **invite de commandes** , tapez **net use \\\\\<nom du serveur\>\c $** , où \<nom du serveur\> est le nom du serveur membre ou de la station de travail à laquelle vous tentez d’accéder sur le réseau.  

6.  La capture d’écran suivante montre le message d’erreur qui doit s’afficher.  

    ![groupes d’administration sécurisés](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_98.gif)  

##### <a name="verify-deny-log-on-as-a-batch-job-gpo-settings"></a>Vérifier les paramètres de l’objet de stratégie de groupe « interdire l’ouverture d’une session en tant que tâche »  
À partir de n’importe quel serveur ou station de travail membre affecté par les modifications de l’objet de stratégie de groupe, connectez-vous localement.  

###### <a name="create-a-batch-file"></a>Créer un fichier de commandes  

1.  À l’aide de la souris, déplacez le pointeur dans le coin supérieur droit ou en bas à droite de l’écran. Lorsque la barre des **icônes** s’affiche, cliquez sur **Rechercher**.  

2.  Dans la zone de **recherche** , tapez **Notepad**, puis cliquez sur **bloc-notes**.  

3.  Dans **le bloc-notes**, tapez **dir c :** .  

4.  Cliquez sur **fichier**, puis sur **Enregistrer sous**.  

5.  Dans le champ **nom de fichier** , tapez **<Filename>. bat** (où <Filename> est le nom du nouveau fichier de commandes).  

###### <a name="schedule-a-task"></a>Planifier une tâche  

1.  À l’aide de la souris, déplacez le pointeur dans le coin supérieur droit ou en bas à droite de l’écran. Lorsque la barre des **icônes** s’affiche, cliquez sur **Rechercher**.  

2.  Dans la zone de **recherche** , tapez **Planificateur de tâches**, puis cliquez sur **Planificateur de tâches**.  

    > [!NOTE]  
    > Sur les ordinateurs exécutant Windows 8, dans la zone de recherche, tapez planifier des tâches, puis cliquez sur planifier des tâches.  

3.  Cliquez sur **action**, puis sur **créer une tâche**.  

4.  Dans la boîte de dialogue **créer une tâche** , tapez **<Task Name>** (où <Task Name> est le nom de la nouvelle tâche).  

5.  Cliquez sur l’onglet **actions** , puis sur **nouveau**.  

6.  Dans le champ **action** , sélectionnez **Démarrer un programme**.  

7.  Dans le champ **programme/script** , cliquez sur **Parcourir**, recherchez et sélectionnez le fichier de commandes créé dans la section **créer un fichier de commandes** , puis cliquez sur **ouvrir**.  

8.  Cliquez sur **OK**.  

9. Cliquez sur l’onglet **Général**.  

10. Dans le champ **options de sécurité** , cliquez sur modifier l' **utilisateur ou le groupe**.  

11. Tapez le nom d’un compte membre du groupe administrateurs, cliquez sur **vérifier les noms**, puis cliquez sur **OK**.  

12. Sélectionnez **exécuter si l’utilisateur a ouvert une session onor** et **ne pas stocker le mot de passe**. La tâche aura uniquement accès aux ressources de l’ordinateur local.  

13. Cliquez sur **OK**.  

14. Une boîte de dialogue doit s’afficher pour demander les informations d’identification du compte d’utilisateur pour exécuter la tâche.  

15. Après avoir entré le mot de passe, cliquez sur **OK**.  

16. Une boîte de dialogue semblable à la suivante doit s’afficher.  

    ![groupes d’administration sécurisés](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_99.gif)  

##### <a name="verify-deny-log-on-as-a-service-gpo-settings"></a>Vérifier les paramètres de l’objet de stratégie de groupe « interdire l’ouverture d’une session en tant que service »  

1.  À partir de n’importe quel serveur ou station de travail membre affecté par les modifications de l’objet de stratégie de groupe, connectez-vous localement.  

2.  À l’aide de la souris, déplacez le pointeur dans le coin supérieur droit ou en bas à droite de l’écran. Lorsque la barre des **icônes** s’affiche, cliquez sur **Rechercher**.  

3.  Dans la zone de **recherche** , tapez **services**, puis cliquez sur **services**.  

4.  Recherchez et double-cliquez sur **spouleur d’impression**.  

5.  Cliquez sur l'onglet **Se connecter**.  

6.  Dans le champ **ouvrir une session en tant que** , sélectionnez **ce compte**.  

7.  Cliquez sur **Parcourir**, tapez le nom d’un compte qui est membre du groupe administrateurs, cliquez sur **vérifier les noms**, puis cliquez sur **OK**.  

8.  Dans les champs **mot** de passe et **confirmer le mot de passe** , tapez le mot de passe du compte sélectionné, puis cliquez sur **OK**.  

9. Cliquez sur **OK** trois fois de plus.  

10. Cliquez avec le bouton droit sur **spouleur d’impression** , puis cliquez sur **redémarrer**.  

11. Une fois le service redémarré, une boîte de dialogue semblable à la suivante doit s’afficher.  

    ![groupes d’administration sécurisés](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_100.png)  

##### <a name="revert-changes-to-the-printer-spooler-service"></a>Annuler les modifications apportées au service spouleur d’impression  

1.  À partir de n’importe quel serveur ou station de travail membre affecté par les modifications de l’objet de stratégie de groupe, connectez-vous localement.  

2.  À l’aide de la souris, déplacez le pointeur dans le coin supérieur droit ou en bas à droite de l’écran. Lorsque la barre des **icônes** s’affiche, cliquez sur **Rechercher**.  

3.  Dans la zone de **recherche** , tapez **services**, puis cliquez sur **services**.  

4.  Recherchez et double-cliquez sur **spouleur d’impression**.  

5.  Cliquez sur l'onglet **Se connecter**.  

6.  Dans le champ **ouvrir une session en tant que** , cliquez sur compte **système local** , puis cliquez sur **OK**.  
