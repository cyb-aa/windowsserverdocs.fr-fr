---
ms.assetid: 017b88a6-f29b-4787-99b6-b5c8eaf8c3df
title: Annexe F-sécurisation des groupes Admins du domaine dans Active Directory
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: d1df7403b7e50fa50894bb4dbaa0cac9c6f42727
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80821502"
---
# <a name="appendix-f-securing-domain-admins-groups-in-active-directory"></a>Annexe F : Sécurisation des groupes d’administrateurs de domaine dans Active Directory

>S’applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012


## <a name="appendix-f-securing-domain-admins-groups-in-active-directory"></a>Annexe F : Sécurisation des groupes d’administrateurs de domaine dans Active Directory  
Comme c’est le cas avec le groupe administrateurs de l’entreprise (EA), l’appartenance au groupe Admins du domaine (DA) doit être obligatoire uniquement dans les scénarios de génération ou de récupération d’urgence. Il ne doit y avoir aucun compte d’utilisateur quotidien dans le groupe DA, à l’exception du compte administrateur intégré pour le domaine, s’il a été sécurisé, comme décrit dans l' [annexe D : sécurisation des comptes administrateur intégrés dans Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory.md).  

Les administrateurs de domaine sont, par défaut, membres des groupes Administrateurs locaux sur tous les serveurs membres et stations de travail dans leurs domaines respectifs. Cette imbrication par défaut ne doit pas être modifiée pour des raisons de prise en charge et de récupération d’urgence. Si les administrateurs de domaine ont été supprimés des groupes Administrateurs locaux sur les serveurs membres, le groupe doit être ajouté au groupe Administrateurs sur chaque serveur membre et station de travail dans le domaine. Le groupe Admins du domaine de chaque domaine doit être sécurisé comme décrit dans les instructions pas à pas qui suivent.  

Pour le groupe Admins du domaine dans chaque domaine de la forêt :  

1.  Supprimez tous les membres du groupe, à l’exception du compte administrateur intégré pour le domaine, à condition qu’il ait été sécurisé, comme décrit dans l' [annexe D : sécurisation des comptes administrateur intégrés dans Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory.md).  

2.  Dans les objets de stratégie de groupe liés à des unités d’organisation contenant des serveurs membres et des stations de travail dans chaque domaine, le groupe DA doit être ajouté aux droits d’utilisateur suivants dans l' **ordinateur \ stratégies \ paramètres d’autorisation \ Stratégies locales\Attribution des droits**:  

    -   Refuser l'accès à un ordinateur à partir du réseau  

    -   Refuser l'ouverture de session en tant que tâche  

    -   Refuser l'ouverture de session en tant que service  

    -   Interdire l'ouverture d'une session locale  

    -   Interdire l’ouverture de session via Services Bureau à distance des droits d’utilisateur  

3.  L’audit doit être configuré pour envoyer des alertes si des modifications sont apportées aux propriétés ou à l’appartenance au groupe Admins du domaine.  

#### <a name="step-by-step-instructions-for-removing-all-members-from-the-domain-admins-group"></a>Instructions pas à pas pour la suppression de tous les membres du groupe Admins du domaine  

1.  Dans **Gestionnaire de serveur**, cliquez sur **Outils**, puis sur **Active Directory les utilisateurs et les ordinateurs**.  

2.  Pour supprimer tous les membres du groupe DA, procédez comme suit :  

    1.  Double-cliquez sur le groupe **Admins du domaine** , puis cliquez sur l’onglet **membres** .  

        ![groupes d’administrateurs de domaine sécurisés](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_62.gif)  

    2.  Sélectionnez un membre du groupe, cliquez sur **supprimer**, sur **Oui**, puis sur **OK**.  

3.  Répétez l’étape 2 jusqu’à ce que tous les membres du groupe DA aient été supprimés.  

#### <a name="step-by-step-instructions-to-secure-domain-admins-in-active-directory"></a>Instructions pas à pas pour sécuriser les administrateurs de domaine dans Active Directory  

1.  Dans **Gestionnaire de serveur**, cliquez sur **Outils**, puis sur **gestion des stratégie de groupe**.  

2.  Dans l’arborescence de la console, développez \<forêt\>\\domaines\\\<domaine\>, puis **stratégie de groupe objets** (où \<forêt\> est le nom de la forêt et \<domaine\> est le nom du domaine dans lequel vous souhaitez définir la stratégie de groupe).  

3.  Dans l’arborescence de la console, cliquez avec le bouton droit sur **stratégie de groupe objets**, puis cliquez sur **nouveau**.  

    ![groupes d’administrateurs de domaine sécurisés](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_63.gif)  

4.  Dans la boîte de dialogue **nouvel objet GPO** , tapez \<nom de l’objet de stratégie de groupe\>, puis cliquez sur **OK** (où \<nom de l’objet de stratégie de groupe\> est le nom de cet objet de stratégie de groupe).  

    ![groupes d’administrateurs de domaine sécurisés](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_64.gif)  

5.  Dans le volet d’informations, cliquez avec le bouton droit sur \<nom de l’objet de stratégie de groupe\>, puis cliquez sur **modifier**.  

6.  Accédez à **ordinateur \ stratégies \ stratégies \ stratégies \ stratégies \ stratégies locales**, puis cliquez sur **attribution des droits utilisateur**.  

    ![groupes d’administrateurs de domaine sécurisés](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_65.gif)  

7.  Configurez les droits d’utilisateur pour empêcher les membres du groupe Admins du domaine d’accéder aux serveurs membres et aux stations de travail sur le réseau en procédant comme suit :  

    1.  Double-cliquez sur **interdire l’accès à cet ordinateur à partir du réseau** , puis sélectionnez **définir ces paramètres de stratégie**.  

    2.  Cliquez sur **Ajouter un utilisateur ou un groupe** , puis sur **Parcourir**.  

    3.  Tapez **Admins du domaine**, cliquez sur **vérifier les noms**, puis cliquez sur **OK**.  

        ![groupes d’administrateurs de domaine sécurisés](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_66.gif)  

    4.  Cliquez sur **OK**, puis de nouveau sur **OK** .  

8.  Configurez les droits d’utilisateur pour empêcher les membres du groupe DA de se connecter en tant que traitement par lots en procédant comme suit :  

    1.  Double-cliquez sur **interdire l’ouverture de session en tant que tâche** , puis sélectionnez **définir ces paramètres de stratégie**.  

    2.  Cliquez sur **Ajouter un utilisateur ou un groupe** , puis sur **Parcourir**.  

    3.  Tapez **Admins du domaine**, cliquez sur **vérifier les noms**, puis cliquez sur **OK**.  

        ![groupes d’administrateurs de domaine sécurisés](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_67.gif)  

    4.  Cliquez sur **OK**, puis de nouveau sur **OK** .  

9. Configurez les droits d’utilisateur pour empêcher les membres du groupe DA de se connecter en tant que service en procédant comme suit :  

    1.  Double-cliquez sur **interdire l’ouverture de session en tant que service** , puis sélectionnez **définir ces paramètres de stratégie**.  

    2.  Cliquez sur **Ajouter un utilisateur ou un groupe** , puis sur **Parcourir**.  

    3.  Tapez **Admins du domaine**, cliquez sur **vérifier les noms**, puis cliquez sur **OK**.  

        ![groupes d’administrateurs de domaine sécurisés](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_68.gif)  

    4.  Cliquez sur **OK**, puis de nouveau sur **OK** .  

10. Configurez les droits d’utilisateur pour empêcher les membres du groupe Admins du domaine de se connecter localement aux serveurs et stations de travail membres en procédant comme suit :  

    1.  Double-cliquez sur **interdire l’ouverture d’une session locale** , puis sélectionnez **définir ces paramètres de stratégie**.  

    2.  Cliquez sur **Ajouter un utilisateur ou un groupe** , puis sur **Parcourir**.  

    3.  Tapez **Admins du domaine**, cliquez sur **vérifier les noms**, puis cliquez sur **OK**.  

        ![groupes d’administrateurs de domaine sécurisés](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_69.gif)  

    4.  Cliquez sur **OK**, puis de nouveau sur **OK** .  

11. Configurez les droits d’utilisateur pour empêcher les membres du groupe Admins du domaine d’accéder aux serveurs membres et aux stations de travail via Services Bureau à distance en procédant comme suit :  

    1.  Double-cliquez sur **interdire l’ouverture de session via services Bureau à distance** et sélectionnez **définir ces paramètres de stratégie**.  

    2.  Cliquez sur **Ajouter un utilisateur ou un groupe** , puis sur **Parcourir**.  

    3.  Tapez **Admins du domaine**, cliquez sur **vérifier les noms**, puis cliquez sur **OK**.  

        ![groupes d’administrateurs de domaine sécurisés](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_70.gif)  

    4.  Cliquez sur **OK**, puis de nouveau sur **OK** .  

12. Pour quitter **éditeur de gestion des stratégies de groupe**, cliquez sur **fichier**, puis sur **quitter**.  

13. Dans stratégie de groupe gestion, liez l’objet de stratégie de groupe aux unités d’organisation serveur et station de travail membres en procédant comme suit :  

    1.  Accédez à la forêt \<\>\Domains\\\<Domain\> (où \<forêt\> est le nom de la forêt et \<Domain\> est le nom du domaine dans lequel vous souhaitez définir la stratégie de groupe).  

    2.  Cliquez avec le bouton droit sur l’unité d’organisation à laquelle l’objet de stratégie de groupe sera appliqué, puis cliquez sur **lier un objet de stratégie de groupe existant**.  

        ![groupes d’administrateurs de domaine sécurisés](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_71.gif)  

    3.  Sélectionnez l’objet de stratégie de groupe que vous venez de créer, puis cliquez sur **OK**.  

        ![groupes d’administrateurs de domaine sécurisés](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_72.gif)  

    4.  Créez des liens vers toutes les autres unités d’organisation qui contiennent des stations de travail.  

    5.  Créez des liens vers toutes les autres unités d’organisation qui contiennent des serveurs membres.  

        > [!IMPORTANT]  
        > Si les serveurs de saut sont utilisés pour administrer des contrôleurs de domaine et des Active Directory, assurez-vous que les serveurs de saut se trouvent dans une UO à laquelle ces objets de stratégie de groupe ne sont pas liés.  

#### <a name="verification-steps"></a>Étapes de vérification  

##### <a name="verify-deny-access-to-this-computer-from-the-network-gpo-settings"></a>Vérifier les paramètres de l’objet de stratégie de groupe « refuser l’accès à cet ordinateur à partir du réseau »  
À partir de n’importe quel serveur ou station de travail membre qui n’est pas affecté par les modifications de l’objet de stratégie de groupe (par exemple, un « serveur de saut »), essayez d’accéder à un serveur membre ou à une station de travail sur le réseau affecté par les modifications de l’objet de stratégie de groupe Pour vérifier les paramètres de l’objet de stratégie de groupe, essayez de mapper le lecteur système à l’aide de la commande **net use** .  

1.  Connectez-vous localement à l’aide d’un compte membre du groupe Admins du domaine.  

2.  À l’aide de la souris, déplacez le pointeur dans le coin supérieur droit ou en bas à droite de l’écran. Lorsque la barre des **icônes** s’affiche, cliquez sur **Rechercher**.  

3.  Dans la zone de **recherche** , tapez **invite de commandes**, cliquez avec le bouton droit sur invite de **commandes**, puis cliquez sur **exécuter en tant qu’administrateur** pour ouvrir une invite de commandes avec élévation de privilèges.  

4.  Lorsque vous êtes invité à approuver l’élévation, cliquez sur **Oui**.  

    ![groupes d’administrateurs de domaine sécurisés](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_73.gif)  

5.  Dans la fenêtre d' **invite de commandes** , tapez **net use \\\\\<nom du serveur\>\c $** , où \<nom du serveur\> est le nom du serveur membre ou de la station de travail à laquelle vous tentez d’accéder sur le réseau.  

6.  La capture d’écran suivante montre le message d’erreur qui doit s’afficher.  

    ![groupes d’administrateurs de domaine sécurisés](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_74.gif)  

##### <a name="verify-deny-log-on-as-a-batch-job-gpo-settings"></a>Vérifier les paramètres de l’objet de stratégie de groupe « interdire l’ouverture d’une session en tant que tâche »  

À partir de n’importe quel serveur ou station de travail membre affecté par les modifications de l’objet de stratégie de groupe, connectez-vous localement.  

###### <a name="create-a-batch-file"></a>Créer un fichier de commandes  

1.  À l’aide de la souris, déplacez le pointeur dans le coin supérieur droit ou en bas à droite de l’écran. Lorsque la barre des **icônes** s’affiche, cliquez sur **Rechercher**.  

2.  Dans la zone de **recherche** , tapez **Notepad**, puis cliquez sur **bloc-notes**.  

3.  Dans **le bloc-notes**, tapez **dir c :** .  

4.  Cliquez sur **fichier**, puis sur **Enregistrer sous**.  

5.  Dans le champ nom de **fichier** , tapez\<nom de fichier **\>. bat** (où \<nom de fichier\> est le nom du nouveau fichier de commandes).  

###### <a name="schedule-a-task"></a>Planifier une tâche  

1.  À l’aide de la souris, déplacez le pointeur dans le coin supérieur droit ou en bas à droite de l’écran. Lorsque la barre des **icônes** s’affiche, cliquez sur **Rechercher**.  

2.  Dans la zone de **recherche** , tapez **Planificateur de tâches**, puis cliquez sur **Planificateur de tâches**.  

    > [!NOTE]  
    > Sur les ordinateurs exécutant Windows 8, dans la zone de **recherche** , tapez **planifier des tâches**, puis cliquez sur **planifier des tâches**.  

3.  Dans la barre de menus **Planificateur de tâches** , cliquez sur **action**, puis sur **créer une tâche**.  

4.  Dans la boîte de dialogue **créer une tâche** , tapez\<nom de la **tâche\>** (où \<nom de la tâche\> est le nom de la nouvelle tâche).  

5.  Cliquez sur l’onglet **actions** , puis sur **nouveau**.  

6.  Dans le champ **action** , sélectionnez **Démarrer un programme**.  

7.  Sous **programme/script**, cliquez sur **Parcourir**, recherchez et sélectionnez le fichier de commandes créé dans la section **créer un fichier de commandes** , puis cliquez sur **ouvrir**.  

8.  Cliquez sur **OK**.  

9. Cliquez sur l’onglet **Général**.  

10. Sous options de **sécurité** , cliquez sur **modifier l’utilisateur ou le groupe**.  

11. Tapez le nom d’un compte membre du groupe Admins du domaine, cliquez sur **vérifier les noms**, puis cliquez sur **OK**.  

12. Sélectionnez **exécuter si l’utilisateur a ouvert une session ou non** et sélectionnez **ne pas stocker le mot de passe**. La tâche aura uniquement accès aux ressources de l’ordinateur local.  

13. Cliquez sur **OK**.  

14. Une boîte de dialogue doit s’afficher pour demander les informations d’identification du compte d’utilisateur pour exécuter la tâche.  

15. Après avoir entré les informations d’identification, cliquez sur **OK**.  

16. Une boîte de dialogue semblable à la suivante doit s’afficher.  

    ![groupes d’administrateurs de domaine sécurisés](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_75.gif)  

##### <a name="verify-deny-log-on-as-a-service-gpo-settings"></a>Vérifier les paramètres de l’objet de stratégie de groupe « interdire l’ouverture d’une session en tant que service »  

1.  À partir de n’importe quel serveur ou station de travail membre affecté par les modifications de l’objet de stratégie de groupe, connectez-vous localement.  

2.  À l’aide de la souris, déplacez le pointeur dans le coin supérieur droit ou en bas à droite de l’écran. Lorsque la barre des **icônes** s’affiche, cliquez sur **Rechercher**.  

3.  Dans la zone de **recherche** , tapez **services**, puis cliquez sur **services**.  

4.  Recherchez et double-cliquez sur **spouleur d’impression**.  

5.  Cliquez sur l'onglet **Se connecter**.  

6.  Sous **ouvrir une session en tant que**, sélectionnez l’option **ce compte** .  

7.  Cliquez sur **Parcourir**, tapez le nom d’un compte qui est membre du groupe Admins du domaine, cliquez sur **vérifier les noms**, puis cliquez sur **OK**.  

8.  Sous **mot de** passe et **confirmer le mot de passe**, tapez le mot de passe du compte sélectionné, puis cliquez sur **OK**.  

9. Cliquez sur **OK** trois fois de plus.  

10. Cliquez avec le bouton droit sur **spouleur d’impression** , puis cliquez sur **redémarrer**.  

11. Une fois le service redémarré, une boîte de dialogue semblable à la suivante doit s’afficher.  

    ![groupes d’administrateurs de domaine sécurisés](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_76.gif)  

##### <a name="revert-changes-to-the-printer-spooler-service"></a>Annuler les modifications apportées au service spouleur d’impression  

1.  À partir de n’importe quel serveur ou station de travail membre affecté par les modifications de l’objet de stratégie de groupe, connectez-vous localement.  

2.  À l’aide de la souris, déplacez le pointeur dans le coin supérieur droit ou en bas à droite de l’écran. Lorsque la barre des **icônes** s’affiche, cliquez sur **Rechercher**.  

3.  Dans la zone de **recherche** , tapez **services**, puis cliquez sur **services**.  

4.  Recherchez et double-cliquez sur **spouleur d’impression**.  

5.  Cliquez sur l'onglet **Se connecter**.  

6.  Sous **ouvrir une session en tant que**, sélectionnez le compte **système local** , puis cliquez sur **OK**.  

##### <a name="verify-deny-log-on-locally-gpo-settings"></a>Vérifier les paramètres de l’objet de stratégie de groupe « interdire l’ouverture d’une session locale »  

1.  À partir de n’importe quel serveur ou station de travail membre affecté par les modifications de l’objet de stratégie de groupe, essayez de vous connecter localement à l’aide d’un compte membre du groupe Admins du domaine. Une boîte de dialogue semblable à la suivante doit s’afficher.  

    ![groupes d’administrateurs de domaine sécurisés](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_77.gif)  

##### <a name="verify-deny-log-on-through-remote-desktop-services-gpo-settings"></a>Vérifier les paramètres de l’objet de stratégie de groupe « interdire l’ouverture d’une session via Services Bureau à distance »    
1.  À l’aide de la souris, déplacez le pointeur dans le coin supérieur droit ou en bas à droite de l’écran. Lorsque la barre des **icônes** s’affiche, cliquez sur **Rechercher**.  

2.  Dans la zone de **recherche** , tapez **Connexion Bureau à distance**, puis cliquez sur **Connexion Bureau à distance**.  

3.  Dans le champ **ordinateur** , tapez le nom de l’ordinateur auquel vous souhaitez vous connecter, puis cliquez sur **se connecter**. (Vous pouvez également taper l’adresse IP à la place du nom de l’ordinateur.)  

4.  Lorsque vous y êtes invité, fournissez les informations d’identification d’un compte membre du groupe Admins du domaine.  

5.  Une boîte de dialogue semblable à la suivante doit s’afficher.  

    ![groupes d’administrateurs de domaine sécurisés](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_78.gif)  
