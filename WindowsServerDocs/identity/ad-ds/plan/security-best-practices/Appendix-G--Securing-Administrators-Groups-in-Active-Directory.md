---
ms.assetid: 4baefbd3-038f-44c0-85ba-f24e9722b757
title: 'Annexe G : sécurisation des groupes d’administrateurs dans Active Directory'
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 2912dfc534d751d4aa121d238dffc36c07562d76
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59882710"
---
# <a name="appendix-g-securing-administrators-groups-in-active-directory"></a>Annexe g : Sécurisation des groupes d’administrateurs dans Active Directory

>S'applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012


## <a name="appendix-g-securing-administrators-groups-in-active-directory"></a>Annexe g : Sécurisation des groupes d’administrateurs dans Active Directory  
Comme c’est le cas avec les groupes Administrateurs de l’entreprise (EA) et les administrateurs de domaine (DA), l’appartenance au groupe intégré Administrateurs (BA) doit être nécessaires uniquement dans les scénarios de récupération d’urgence ou de build. Il ne doit être aucun compte d’utilisateur ordinaires dans le groupe administrateurs à l’exception du compte administrateur intégré pour le domaine, si elle a été sécurisée comme décrit dans [annexe d : Sécurisation des comptes d’administrateur intégré dans Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory.md).  

Les administrateurs sont, par défaut, les propriétaires de la plupart des objets AD DS dans leurs domaines respectifs. L’appartenance à ce groupe peut être nécessaire dans les scénarios de récupération d’urgence ou de la build dans laquelle la propriété ou la possibilité de prendre possession d’objets est requise. En outre, DAs et EAs héritent d’un nombre de leurs droits et autorisations en vertu de leur appartenance par défaut dans le groupe Administrateurs. Groupe par défaut d’imbrication des groupes privilégiés dans Active Directory ne doit pas être modifié, et groupe d’administrateurs de chaque domaine doit être sécurisée comme décrit dans les instructions détaillées qui suivent.  

Pour le groupe Administrateurs dans chaque domaine dans la forêt :  

1.  Supprimez tous les membres du groupe Administrateurs, à l’exception du compte administrateur intégré pour le domaine, sous réserve qu’il a été sécurisé comme décrit dans [annexe d : Sécurisation des comptes d’administrateur intégré dans Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory.md).  

2.  Dans la stratégie de groupe liés aux unités d’organisation contenant les serveurs membres et les stations de travail dans chaque domaine, le groupe DA doit être ajouté pour les droits d’utilisateur suivants dans **ordinateur Configuration ordinateur\Stratégies\Paramètres Windows\Paramètres Settings\Local Policies\ droits d’utilisateur Affectation**:  

    -   Interdire l’accès à cet ordinateur à partir du réseau  

    -   Interdire l’ouverture de session en tant que tâche  

    -   Interdire l’ouverture de session en tant que service  

3.  À l’unité d’organisation dans chaque domaine dans la forêt de contrôleurs de domaine, du groupe Administrateurs doit avoir des droits d’utilisateur suivants :  

    -   Accéder à cet ordinateur à partir du réseau  

    -   Permettre l’ouverture d’une session locale  

    -   Autoriser l’ouverture de session par les services Bureau à distance  

4.  L’audit doit être configuré pour envoyer des alertes si des modifications sont apportées aux propriétés ou aux membres du groupe Administrateurs.  

#### <a name="step-by-step-instructions-for-removing-all-members-from-the-administrators-group"></a>Obtenir des Instructions détaillées pour la suppression de tous les membres du groupe Administrateurs  

1.  Dans **le Gestionnaire de serveur**, cliquez sur **outils**, puis cliquez sur **Active Directory Users and Computers**.  

2.  Pour supprimer tous les membres du groupe Administrateurs, procédez comme suit :  

    1.  Double-cliquez sur le **administrateurs** de groupe et cliquez sur le **membres** onglet.  

        ![groupes d’administration sécurisée](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_79.gif)  

    2.  Sélectionnez un membre du groupe, cliquez sur **supprimer**, cliquez sur **Oui**, puis cliquez sur **OK**.  

3.  Répétez l’étape 2 jusqu'à ce que tous les membres du groupe Administrateurs ont été supprimés.  

#### <a name="step-by-step-instructions-to-secure-administrators-groups-in-active-directory"></a>Instructions pas à pas sur la sécurisation des groupes d’administrateurs dans Active Directory  

1.  Dans **le Gestionnaire de serveur**, cliquez sur **outils**, puis cliquez sur **Group Policy Management**.  

2.  Dans l’arborescence de la console, développez &lt;forêt&gt;\Domains\\&lt;domaine&gt;, puis **les objets de stratégie de groupe** (où &lt;forêt&gt; est le nom de la forêt et &lt;domaine&gt; est le nom du domaine dans lequel vous souhaitez définir la stratégie de groupe).  

3.  Dans l’arborescence de la console, cliquez sur **les objets de stratégie de groupe**, puis cliquez sur **New**.  

    ![groupes d’administration sécurisée](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_80.gif)  

4.  Dans le **nouvel objet GPO** boîte de dialogue, tapez <GPO Name>, puis cliquez sur **OK** (où *nom de l’objet stratégie de groupe* est le nom de cet objet de stratégie de groupe).  

    ![groupes d’administration sécurisée](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_81.gif)  

5.  Dans le volet détails, cliquez sur **<GPO Name>**, puis cliquez sur **modifier**.  

6.  Accédez à **Configuration ordinateur\Stratégies\Paramètres Windows\Paramètres de Sécurité\stratégies**, puis cliquez sur **attribution des droits utilisateur**.  

    ![groupes d’administration sécurisée](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_82.gif)  

7.  Configurer les droits d’utilisateur pour empêcher les membres du groupe Administrateurs de l’accès aux serveurs membres et les stations de travail sur le réseau en procédant comme suit :  

    1.  Double-cliquez sur **refuser l’accès à cet ordinateur à partir du réseau** et sélectionnez **définir ces paramètres de stratégie**.  

    2.  Cliquez sur **ajouter un utilisateur ou groupe** et cliquez sur **Parcourir**.  

    3.  Type **administrateurs**, cliquez sur **vérifier les noms**, puis cliquez sur **OK**.  

        ![groupes d’administration sécurisée](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_83.gif)  

    4.  Cliquez sur **OK**, et **OK** à nouveau.  

8.  Configurer les droits d’utilisateur pour empêcher les membres du groupe Administrateurs de se connecter en tant que traitement par lots en procédant comme suit :  

    1.  Double-cliquez sur **refuse la connexion en tant que traitement par lots** et sélectionnez **définir ces paramètres de stratégie**.  

    2.  Cliquez sur **ajouter un utilisateur ou groupe** et cliquez sur **Parcourir**.  

    3.  Type **administrateurs**, cliquez sur **vérifier les noms**, puis cliquez sur **OK**.  

        ![groupes d’administration sécurisée](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_84.gif)  

    4.  Cliquez sur **OK**, et **OK** à nouveau.  

9. Configurer les droits d’utilisateur pour empêcher les membres du groupe Administrateurs de se connecter en tant que service en procédant comme suit :  

    1.  Double-cliquez sur **refuse la connexion en tant que service** et sélectionnez **définir ces paramètres de stratégie**.  

    2.  Cliquez sur **ajouter un utilisateur ou groupe** et cliquez sur **Parcourir**.  

    3.  Type **administrateurs**, cliquez sur **vérifier les noms**, puis cliquez sur **OK**.  

        ![groupes d’administration sécurisée](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_85.gif)  

    4.  Cliquez sur **OK**, et **OK** à nouveau.  

10. Pour quitter **éditeur de gestion de stratégie de groupe**, cliquez sur **fichier**, puis cliquez sur **quitter**.  

11. Dans **Group Policy Management**, lier l’objet de stratégie de groupe pour le serveur membre et de la station de travail unités d’organisation en procédant comme suit :  

    1.  Accédez à la &lt;forêt&gt;> \Domains\\&lt;domaine&gt; (où &lt;forêt&gt; est le nom de la forêt et &lt;domaine&gt; est le nom du domaine où vous souhaitez définir la stratégie de groupe).  

    2.  Avec le bouton droit de l’unité d’organisation qui seront appliqués à l’objet de stratégie de groupe et cliquez sur **lier un objet de stratégie de groupe existant**.  

        ![groupes d’administration sécurisée](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_86.gif)  

    3.  Sélectionnez l’objet de stratégie de groupe que vous venez de créer et cliquez sur **OK**.  

        ![groupes d’administration sécurisée](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_87.gif)  

    4.  Créer des liens vers toutes les autres unités d’organisation qui contiennent des stations de travail.  

    5.  Créer des liens vers toutes les autres unités d’organisation qui contiennent des serveurs membres.  

        > [!IMPORTANT]  
        > Si les serveurs jump sont utilisés pour administrer les contrôleurs de domaine et Active Directory, vérifiez que les serveurs jump sont trouvent dans une unité d’organisation sur lesquels cette stratégie de groupe n’est pas lié.  

        > [!NOTE]  
        > Lorsque vous implémentez des restrictions sur le groupe Administrateurs de stratégie de groupe, Windows applique les paramètres aux membres du groupe Administrateurs local de l’ordinateur en plus du groupe d’administrateurs du domaine. Par conséquent, vous soyez prudent lors de l’implémentation des restrictions dans le groupe Administrateurs. Bien que l’interdiction de réseau, batch et les connexions de service pour les membres du groupe Administrateurs est recommandé, là où il est possible d’implémenter, ne pas restreindre les ouvertures de session locales ou des ouvertures de session via les Services Bureau à distance. Ces types d’ouverture de session de blocage peut bloquer l’administration légitime d’un ordinateur par les membres du groupe Administrateurs local.  
        >   
        > La capture d’écran suivante montre les paramètres de configuration de bloquer l’utilisation incorrecte d’intégré local et comptes d’administrateur domaine, en plus de l’utilisation incorrecte du groupe Administrateurs intégré au domaine ou local. Notez que le **refuse la connexion via les Services Bureau à distance** droit d’utilisateur n’inclut pas le groupe Administrateurs, car son inclusion dans ce paramètre bloque également ces ouvertures de session pour les comptes qui sont membres de l’ordinateur local Groupe d’administrateurs. Si les services sur les ordinateurs sont configurés pour s’exécuter dans le contexte d’un des groupes privilégiés décrites dans cette section, mise en œuvre de ces paramètres peut entraîner services et applications en échec. Par conséquent, avec toutes les recommandations contenues dans cette section, vous devez tester rigoureusement les paramètres de mise en application dans votre environnement.  

        ![groupes d’administration sécurisée](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_88.gif)  

#### <a name="step-by-step-instructions-to-grant-user-rights-to-the-administrators-group"></a>Obtenir des Instructions détaillées pour les droits accordez à l’utilisateur au groupe Administrateurs  

1.  Dans **le Gestionnaire de serveur**, cliquez sur **outils**, puis cliquez sur **Group Policy Management**.  

2.  Dans l’arborescence de la console, développez <Forest>\Domains\\<Domain>, puis **les objets de stratégie de groupe** (où <Forest> est le nom de la forêt et <Domain> est le nom du domaine où vous souhaitez Définissez la stratégie de groupe).  

3.  Dans l’arborescence de la console, cliquez sur **les objets de stratégie de groupe**, puis cliquez sur **New**.  

    ![groupes d’administration sécurisée](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_89.gif)  

4.  Dans le **nouvel objet GPO** boîte de dialogue, tapez <GPO Name>, puis cliquez sur **OK** (où <GPO Name> est le nom de cet objet de stratégie de groupe).  

    ![groupes d’administration sécurisée](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_90.gif)  

5.  Dans le volet détails, cliquez sur **<GPO Name>**, puis cliquez sur **modifier**.  

6.  Accédez à **Configuration ordinateur\Stratégies\Paramètres Windows\Paramètres de Sécurité\stratégies**, puis cliquez sur **attribution des droits utilisateur**.  

    ![groupes d’administration sécurisée](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_91.gif)  

7.  Configurer les droits d’utilisateur pour permettre aux membres du groupe Administrateurs sur les contrôleurs de domaine de l’accès sur le réseau en procédant comme suit :  

    1.  Double-cliquez sur **accès à cet ordinateur à partir du réseau** et sélectionnez **définir ces paramètres de stratégie**.  

    2.  Cliquez sur **ajouter un utilisateur ou groupe** et cliquez sur **Parcourir**.  

    3.  Cliquez sur **ajouter un utilisateur ou groupe** et cliquez sur **Parcourir**.  

        ![groupes d’administration sécurisée](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_92.gif)  

    4.  Cliquez sur **OK**, et **OK** à nouveau.  

8.  Configurer les droits d’utilisateur pour permettre aux membres du groupe Administrateurs de se connecter localement en procédant comme suit :  

    1.  Double-cliquez sur **autoriser la connexion localement** et sélectionnez **définir ces paramètres de stratégie**.  

    2.  Cliquez sur **ajouter un utilisateur ou groupe** et cliquez sur **Parcourir**.  

    3.  Type **administrateurs**, cliquez sur vérification **noms**, puis cliquez sur **OK**.  

        ![groupes d’administration sécurisée](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_93.gif)  

    4.  Cliquez sur **OK**, et **OK** à nouveau.  

9. Configurer les droits d’utilisateur pour permettre aux membres du groupe Administrateurs de se connecter par le biais des Services Bureau à distance en procédant comme suit :  

    1.  Double-cliquez sur **autoriser la connexion par le biais des Services Bureau à distance** et sélectionnez **définir ces paramètres de stratégie**.  

    2.  Cliquez sur **ajouter un utilisateur ou groupe** et cliquez sur **Parcourir**.  

    3.  Type **administrateurs**, cliquez sur **vérifier les noms**, puis cliquez sur **OK**.  

        ![groupes d’administration sécurisée](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_94.gif)  

    4.  Cliquez sur **OK**, et **OK** à nouveau.  

10. Pour quitter **éditeur de gestion de stratégie de groupe**, cliquez sur **fichier**, puis cliquez sur **quitter**.  

11. Dans **Group Policy Management**, liez-le à l’UO des contrôleurs de domaine en procédant comme suit :  

    1.  Accédez à la <Forest>\Domains\\ <Domain> (où <Forest> est le nom de la forêt et <Domain> est le nom du domaine dans lequel vous souhaitez définir la stratégie de groupe).  

    2.  Cliquez sur les contrôleurs de domaine, unité d’organisation et cliquez sur **lier un objet de stratégie de groupe existant**.  

        ![groupes d’administration sécurisée](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_95.gif)  

    3.  Sélectionnez l’objet de stratégie de groupe que vous venez de créer et cliquez sur **OK**.  

        ![groupes d’administration sécurisée](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_96.gif)  

#### <a name="verification-steps"></a>Étapes de vérification  

##### <a name="verify-deny-access-to-this-computer-from-the-network-gpo-settings"></a>Vérifiez les paramètres de stratégie de groupe « Refuser l’accès à cet ordinateur à partir du réseau »  
À partir de n’importe quel serveur membre ou une station de travail qui n’est pas affectée par les modifications de GPO (par exemple, un « serveur de renvoi) », essayez d’accéder à un serveur membre ou station de travail sur le réseau qui est affecté par les modifications de GPO. Pour vérifier les paramètres de stratégie de groupe, essayez de mapper le lecteur du système à l’aide de la **NET USE** commande.  

1.  Connectez-vous localement à l’aide d’un compte qui est membre du groupe Administrateurs.  

2.  Avec la souris, déplacez le pointeur dans le coin supérieur droit ou inférieur droit de l’écran. Lorsque le **icônes** barre s’affiche, cliquez sur **recherche**.  

3.  Dans le **recherche** , tapez **invite de commandes**, avec le bouton droit **invite de commandes**, puis cliquez sur **exécuter en tant qu’administrateur** pour ouvrir une avec élévation de privilèges invite de commandes.  

4.  Lorsque vous êtes invité à approuver l’élévation, cliquez sur **Oui**.  

    ![groupes d’administration sécurisée](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_97.gif)  

5.  Dans le **invite de commandes** fenêtre, tapez **net utilisation \\ \\ \<nom du serveur\>\c$**, où \<nom du serveur\> est le nom du serveur membre ou station de travail que vous tentez d’accéder via le réseau.  

6.  La capture d’écran suivante montre le message d’erreur qui doit apparaître.  

    ![groupes d’administration sécurisée](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_98.gif)  

##### <a name="verify-deny-log-on-as-a-batch-job-gpo-settings"></a>Vérifiez les paramètres de stratégie de groupe « Refuse la connexion en tant que tâche »  
À partir de n’importe quel serveur membre ou une station de travail affecté par les modifications de GPO, ouvrir une session localement.  

###### <a name="create-a-batch-file"></a>Créer un fichier de commandes  

1.  Avec la souris, déplacez le pointeur dans le coin supérieur droit ou inférieur droit de l’écran. Lorsque le **icônes** barre s’affiche, cliquez sur **recherche**.  

2.  Dans le **recherche** , tapez **le bloc-notes**, puis cliquez sur **le bloc-notes**.  

3.  Dans **le bloc-notes**, type **dir c:**.  

4.  Cliquez sur **fichier**, puis cliquez sur **Enregistrer sous**.  

5.  Dans le **nom de fichier** , tapez  **<Filename>.bat** (où <Filename> est le nom du nouveau fichier batch).  

###### <a name="schedule-a-task"></a>Planifier une tâche  

1.  Avec la souris, déplacez le pointeur dans le coin supérieur droit ou inférieur droit de l’écran. Lorsque le **icônes** barre s’affiche, cliquez sur **recherche**.  

2.  Dans le **recherche** , tapez **Planificateur de tâches**, puis cliquez sur **Planificateur de tâches**.  

    > [!NOTE]  
    > Sur les ordinateurs exécutant Windows 8, dans la zone de recherche, tapez planifier des tâches, puis cliquez sur Planifier des tâches.  

3.  Cliquez sur **Action**, puis cliquez sur **créer une tâche**.  

4.  Dans le **créer une tâche** boîte de dialogue, tapez **<Task Name>** (où <Task Name> est le nom de la nouvelle tâche).  

5.  Cliquez sur le **Actions** onglet, puis cliquez sur **New**.  

6.  Dans le **Action** champ, sélectionnez **démarrer un programme**.  

7.  Dans le **programme/script** , cliquez sur **Parcourir**, recherchez et sélectionnez le fichier de commandes créé dans le **créer un fichier de commandes** section, puis cliquez sur **ouvrir**.  

8.  Cliquez sur **OK**.  

9. Cliquez sur l’onglet **Général**.  

10. Dans le **options de sécurité** , cliquez sur **changer d’utilisateur ou groupe**.  

11. Tapez le nom d’un compte qui est membre du groupe Administrateurs, cliquez sur **vérifier les noms**, puis cliquez sur **OK**.  

12. Sélectionnez **exécuter même si l’utilisateur n’est pas connecté onor** et **ne stockez pas de mot de passe**. La tâche a uniquement accès aux ressources de l’ordinateur local.  

13. Cliquez sur **OK**.  

14. Compte d’utilisateur demandeur une boîte de dialogue doit apparaître, informations d’identification pour exécuter la tâche.  

15. Après avoir entré le mot de passe, cliquez sur **OK**.  

16. Une boîte de dialogue similaire à ce qui suit doit apparaître.  

    ![groupes d’administration sécurisée](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_99.gif)  

##### <a name="verify-deny-log-on-as-a-service-gpo-settings"></a>Vérifiez les paramètres de stratégie de groupe « Refuse la connexion en tant que service »  

1.  À partir de n’importe quel serveur membre ou une station de travail affecté par les modifications de GPO, ouvrir une session localement.  

2.  Avec la souris, déplacez le pointeur dans le coin supérieur droit ou inférieur droit de l’écran. Lorsque le **icônes** barre s’affiche, cliquez sur **recherche**.  

3.  Dans le **recherche** , tapez **services**, puis cliquez sur **Services**.  

4.  Recherchez et double-cliquez sur **spouleur d’impression**.  

5.  Cliquez sur l'onglet **Se connecter**.  

6.  Dans le **connecter en tant que** champ, sélectionnez **ce compte**.  

7.  Cliquez sur **Parcourir**, tapez le nom d’un compte qui est membre du groupe Administrateurs, cliquez sur **vérifier les noms**, puis cliquez sur **OK**.  

8.  Dans le **mot de passe** et **confirmer le mot de passe** champs, tapez le mot de passe du compte sélectionné, puis cliquez sur **OK**.  

9. Cliquez sur **OK** trois fois plus.  

10. Avec le bouton droit **spouleur d’impression** et cliquez sur **redémarrer**.  

11. Lorsque le service est redémarré, une boîte de dialogue similaire à ce qui suit doit apparaître.  

    ![groupes d’administration sécurisée](media/Appendix-G--Securing-Administrators-Groups-in-Active-Directory/SAD_100.png)  

##### <a name="revert-changes-to-the-printer-spooler-service"></a>Annuler les modifications pour le Service de spouleur d’impression  

1.  À partir de n’importe quel serveur membre ou une station de travail affecté par les modifications de GPO, ouvrir une session localement.  

2.  Avec la souris, déplacez le pointeur dans le coin supérieur droit ou inférieur droit de l’écran. Lorsque le **icônes** barre s’affiche, cliquez sur **recherche**.  

3.  Dans le **recherche** , tapez **services**, puis cliquez sur **Services**.  

4.  Recherchez et double-cliquez sur **spouleur d’impression**.  

5.  Cliquez sur l'onglet **Se connecter**.  

6.  Dans le **connecter en tant que** , cliquez sur **système Local** compte, puis cliquez sur **OK**.  
