---
ms.assetid: 017b88a6-f29b-4787-99b6-b5c8eaf8c3df
title: Annexe F - sécurisation des groupes d’administrateurs de domaine dans Active Directory
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 1f35503d1f02d616255c067fbc1750a0cab974cc
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59880140"
---
# <a name="appendix-f-securing-domain-admins-groups-in-active-directory"></a>Annexe F : Sécurisation des groupes d’administrateurs de domaine dans Active Directory

>S'applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012


## <a name="appendix-f-securing-domain-admins-groups-in-active-directory"></a>Annexe F : Sécurisation des groupes d’administrateurs de domaine dans Active Directory  
Comme c’est le cas avec le groupe Administrateurs de l’entreprise (EA), appartenance au groupe Admins du domaine (DA) doit être nécessaires uniquement dans les scénarios de récupération d’urgence ou de build. Il ne doit être aucun compte d’utilisateur ordinaires dans le groupe DA à l’exception du compte administrateur intégré pour le domaine, si elle a été sécurisée comme décrit dans [annexe d : Sécurisation des comptes d’administrateur intégré dans Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory.md).  

Les administrateurs de domaine sont, par défaut, les membres du groupe Administrateurs local sur tous les serveurs membres et les stations de travail de leurs domaines respectifs. Cette imbrication par défaut ne doit pas être modifiée pour la prise en charge et à des fins de récupération d’urgence. Si Admins du domaine ont été supprimé à partir du groupe Administrateurs locaux sur les serveurs membres, le groupe doit être ajouté au groupe Administrateurs sur chaque serveur membre et de la station de travail dans le domaine. Groupe d’administrateurs du domaine de chaque domaine doit être sécurisé, comme décrit dans les instructions détaillées qui suivent.  

Pour le groupe Admins du domaine dans chaque domaine dans la forêt :  

1.  Supprimer tous les membres du groupe, à l’exception du compte administrateur intégré pour le domaine, sous réserve qu’il a été sécurisé comme décrit dans [annexe d : Sécurisation des comptes d’administrateur intégré dans Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory.md).  

2.  Dans la stratégie de groupe liés aux unités d’organisation contenant les serveurs membres et les stations de travail dans chaque domaine, le groupe DA doit être ajouté pour les droits d’utilisateur suivants dans **ordinateur Configuration ordinateur\Stratégies\Paramètres Windows\Paramètres sécurité\Stratégies locales\Attribution des droits Affectations**:  

    -   Interdire l’accès à cet ordinateur à partir du réseau  

    -   Interdire l’ouverture de session en tant que tâche  

    -   Interdire l’ouverture de session en tant que service  

    -   Interdire l’ouverture d’une session locale  

    -   Refuse la connexion via les droits d’utilisateur des Services Bureau à distance  

3.  L’audit doit être configuré pour envoyer des alertes si des modifications sont apportées aux propriétés ou aux membres du groupe Admins du domaine.  

#### <a name="step-by-step-instructions-for-removing-all-members-from-the-domain-admins-group"></a>Obtenir des Instructions détaillées pour la suppression de tous les membres du groupe Administrateurs de domaine  

1.  Dans **le Gestionnaire de serveur**, cliquez sur **outils**, puis cliquez sur **Active Directory Users and Computers**.  

2.  Pour supprimer tous les membres du groupe DA, procédez comme suit :  

    1.  Double-cliquez sur le **Admins du domaine** de groupe et cliquez sur le **membres** onglet.  

        ![sécuriser des groupes d’administrateurs de domaine](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_62.gif)  

    2.  Sélectionnez un membre du groupe, cliquez sur **supprimer**, cliquez sur **Oui**, puis cliquez sur **OK**.  

3.  Répétez l’étape 2 jusqu'à ce que tous les membres du groupe DA ont été supprimés.  

#### <a name="step-by-step-instructions-to-secure-domain-admins-in-active-directory"></a>Instructions pas à pas pour sécuriser les Admins du domaine dans Active Directory  

1.  Dans **le Gestionnaire de serveur**, cliquez sur **outils**, puis cliquez sur **Group Policy Management**.  

2.  Dans l’arborescence de la console, développez \<forêt\>\\domaines\\\<domaine\>, puis **les objets de stratégie de groupe** (où \<forêt\> est le nom de la forêt et \<domaine\> est le nom du domaine dans lequel vous souhaitez définir la stratégie de groupe).  

3.  Dans l’arborescence de la console, cliquez sur **les objets de stratégie de groupe**, puis cliquez sur **New**.  

    ![sécuriser des groupes d’administrateurs de domaine](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_63.gif)  

4.  Dans le **nouvel objet GPO** boîte de dialogue, tapez \<nom de l’objet stratégie de groupe\>, puis cliquez sur **OK** (où \<nom de l’objet stratégie de groupe\> est le nom de cet objet de stratégie de groupe).  

    ![sécuriser des groupes d’administrateurs de domaine](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_64.gif)  

5.  Dans le volet détails, cliquez sur \<nom de l’objet stratégie de groupe\>, puis cliquez sur **modifier**.  

6.  Accédez à **Configuration ordinateur\Stratégies\Paramètres Windows\Paramètres de Sécurité\stratégies**, puis cliquez sur **attribution des droits utilisateur**.  

    ![sécuriser des groupes d’administrateurs de domaine](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_65.gif)  

7.  Configurer les droits d’utilisateur pour empêcher les membres du groupe Admins du domaine de l’accès aux serveurs de membres et les stations de travail sur le réseau en procédant comme suit :  

    1.  Double-cliquez sur **refuser l’accès à cet ordinateur à partir du réseau** et sélectionnez **définir ces paramètres de stratégie**.  

    2.  Cliquez sur **ajouter un utilisateur ou groupe** et cliquez sur **Parcourir**.  

    3.  Type **Admins du domaine**, cliquez sur **vérifier les noms**, puis cliquez sur **OK**.  

        ![sécuriser des groupes d’administrateurs de domaine](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_66.gif)  

    4.  Cliquez sur **OK**, et **OK** à nouveau.  

8.  Configurer les droits d’utilisateur pour empêcher les membres du groupe DA de journalisation comme un traitement par lots en procédant comme suit :  

    1.  Double-cliquez sur **refuse la connexion en tant que traitement par lots** et sélectionnez **définir ces paramètres de stratégie**.  

    2.  Cliquez sur **ajouter un utilisateur ou groupe** et cliquez sur **Parcourir**.  

    3.  Type **Admins du domaine**, cliquez sur **vérifier les noms**, puis cliquez sur **OK**.  

        ![sécuriser des groupes d’administrateurs de domaine](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_67.gif)  

    4.  Cliquez sur **OK**, et **OK** à nouveau.  

9. Configurer les droits d’utilisateur pour empêcher les membres du groupe DA de journalisation en tant que service en procédant comme suit :  

    1.  Double-cliquez sur **refuse la connexion en tant que service** et sélectionnez **définir ces paramètres de stratégie**.  

    2.  Cliquez sur **ajouter un utilisateur ou groupe** et cliquez sur **Parcourir**.  

    3.  Type **Admins du domaine**, cliquez sur **vérifier les noms**, puis cliquez sur **OK**.  

        ![sécuriser des groupes d’administrateurs de domaine](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_68.gif)  

    4.  Cliquez sur **OK**, et **OK** à nouveau.  

10. Configurer les droits d’utilisateur pour empêcher les membres du groupe Admins du domaine à partir de la session locale sur les serveurs membres et les stations de travail en procédant comme suit :  

    1.  Double-cliquez sur **refuse la connexion locale** et sélectionnez **définir ces paramètres de stratégie**.  

    2.  Cliquez sur **ajouter un utilisateur ou groupe** et cliquez sur **Parcourir**.  

    3.  Type **Admins du domaine**, cliquez sur **vérifier les noms**, puis cliquez sur **OK**.  

        ![sécuriser des groupes d’administrateurs de domaine](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_69.gif)  

    4.  Cliquez sur **OK**, et **OK** à nouveau.  

11. Configurer les droits d’utilisateur pour empêcher les membres du groupe Admins du domaine d’accéder à des serveurs membres et les stations de travail via les Services Bureau à distance en procédant comme suit :  

    1.  Double-cliquez sur **refuse la connexion via les Services Bureau à distance** et sélectionnez **définir ces paramètres de stratégie**.  

    2.  Cliquez sur **ajouter un utilisateur ou groupe** et cliquez sur **Parcourir**.  

    3.  Type **Admins du domaine**, cliquez sur **vérifier les noms**, puis cliquez sur **OK**.  

        ![sécuriser des groupes d’administrateurs de domaine](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_70.gif)  

    4.  Cliquez sur **OK**, et **OK** à nouveau.  

12. Pour quitter **éditeur de gestion de stratégie de groupe**, cliquez sur **fichier**, puis cliquez sur **quitter**.  

13. Dans la gestion de stratégie de groupe, liez-le au serveur membre et de la station de travail unités d’organisation en procédant comme suit :  

    1.  Accédez à la \<forêt\>\Domains\\\<domaine\> (où \<forêt\> est le nom de la forêt et \<domaine\> est le nom de le domaine où vous souhaitez définir la stratégie de groupe).  

    2.  Avec le bouton droit de l’unité d’organisation qui seront appliqués à l’objet de stratégie de groupe et cliquez sur **lier un objet de stratégie de groupe existant**.  

        ![sécuriser des groupes d’administrateurs de domaine](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_71.gif)  

    3.  Sélectionnez l’objet de stratégie de groupe que vous venez de créer et cliquez sur **OK**.  

        ![sécuriser des groupes d’administrateurs de domaine](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_72.gif)  

    4.  Créer des liens vers toutes les autres unités d’organisation qui contiennent des stations de travail.  

    5.  Créer des liens vers toutes les autres unités d’organisation qui contiennent des serveurs membres.  

        > [!IMPORTANT]  
        > Si les serveurs jump sont utilisés pour administrer les contrôleurs de domaine et Active Directory, vérifiez que les serveurs jump sont trouvent dans une unité d’organisation sur lesquels cette stratégie de groupe n’est pas lié.  

#### <a name="verification-steps"></a>Étapes de vérification  

##### <a name="verify-deny-access-to-this-computer-from-the-network-gpo-settings"></a>Vérifiez les paramètres de stratégie de groupe « Refuser l’accès à cet ordinateur à partir du réseau »  
À partir de n’importe quel serveur membre ou une station de travail qui n’est pas affectée par les modifications de GPO (par exemple, un « serveur de renvoi) », essayez d’accéder à un serveur membre ou station de travail sur le réseau qui est affecté par les modifications de GPO. Pour vérifier les paramètres de stratégie de groupe, essayez de mapper le lecteur du système à l’aide de la **NET USE** commande.  

1.  Connectez-vous localement à l’aide d’un compte qui est membre du groupe Admins du domaine.  

2.  Avec la souris, déplacez le pointeur dans le coin supérieur droit ou inférieur droit de l’écran. Lorsque le **icônes** barre s’affiche, cliquez sur **recherche**.  

3.  Dans le **recherche** , tapez **invite de commandes**, avec le bouton droit **invite de commandes**, puis cliquez sur **exécuter en tant qu’administrateur** pour ouvrir une avec élévation de privilèges invite de commandes.  

4.  Lorsque vous êtes invité à approuver l’élévation, cliquez sur **Oui**.  

    ![sécuriser des groupes d’administrateurs de domaine](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_73.gif)  

5.  Dans le **invite de commandes** fenêtre, tapez **net utilisation \\ \\ \<nom du serveur\>\c$**, où \<nom du serveur\> est le nom du serveur membre ou station de travail que vous tentez d’accéder via le réseau.  

6.  La capture d’écran suivante montre le message d’erreur qui doit apparaître.  

    ![sécuriser des groupes d’administrateurs de domaine](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_74.gif)  

##### <a name="verify-deny-log-on-as-a-batch-job-gpo-settings"></a>Vérifiez les paramètres de stratégie de groupe « Refuse la connexion en tant que tâche »  

À partir de n’importe quel serveur membre ou une station de travail affecté par les modifications de GPO, ouvrir une session localement.  

###### <a name="create-a-batch-file"></a>Créer un fichier de commandes  

1.  Avec la souris, déplacez le pointeur dans le coin supérieur droit ou inférieur droit de l’écran. Lorsque le **icônes** barre s’affiche, cliquez sur **recherche**.  

2.  Dans le **recherche** , tapez **le bloc-notes**, puis cliquez sur **le bloc-notes**.  

3.  Dans **le bloc-notes**, type **dir c:**.  

4.  Cliquez sur **fichier**, puis cliquez sur **Enregistrer sous**.  

5.  Dans le **fichier** champ nom, tapez  **\<Filename\>.bat** (où \<Filename\> est le nom du nouveau fichier batch).  

###### <a name="schedule-a-task"></a>Planifier une tâche  

1.  Avec la souris, déplacez le pointeur dans le coin supérieur droit ou inférieur droit de l’écran. Lorsque le **icônes** barre s’affiche, cliquez sur **recherche**.  

2.  Dans le **recherche** , tapez **Planificateur de tâches**, puis cliquez sur **Planificateur de tâches**.  

    > [!NOTE]  
    > Sur les ordinateurs exécutant Windows 8, dans le **recherche** , tapez **planifier des tâches**, puis cliquez sur **planifier des tâches**.  

3.  Dans le **Planificateur de tâches** barre de menus, cliquez sur **Action**, puis cliquez sur **créer une tâche**.  

4.  Dans le **créer une tâche** boîte de dialogue, tapez **\<Nom_tâche\>** (où \<nom de la tâche\> est le nom de la nouvelle tâche).  

5.  Cliquez sur le **Actions** onglet, puis cliquez sur **New**.  

6.  Dans le **Action** champ, sélectionnez **démarrer un programme**.  

7.  Sous **programme/script**, cliquez sur **Parcourir**, recherchez et sélectionnez le fichier de commandes créé dans le **créer un fichier de commandes** section, puis cliquez sur **ouvrir**.  

8.  Cliquez sur **OK**.  

9. Cliquez sur l’onglet **Général**.  

10. Sous **sécurité** options, cliquez sur **changer d’utilisateur ou groupe**.  

11. Tapez le nom d’un compte qui est membre du groupe Admins du domaine, cliquez sur **vérifier les noms**, puis cliquez sur **OK**.  

12. Sélectionnez **exécuter même si l’utilisateur est connecté ou non** et sélectionnez **ne stockez pas de mot de passe**. La tâche a uniquement accès aux ressources de l’ordinateur local.  

13. Cliquez sur **OK**.  

14. Compte d’utilisateur demandeur une boîte de dialogue doit apparaître, informations d’identification pour exécuter la tâche.  

15. Après avoir entré les informations d’identification, cliquez sur **OK**.  

16. Une boîte de dialogue similaire à ce qui suit doit apparaître.  

    ![sécuriser des groupes d’administrateurs de domaine](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_75.gif)  

##### <a name="verify-deny-log-on-as-a-service-gpo-settings"></a>Vérifiez les paramètres de stratégie de groupe « Refuse la connexion en tant que service »  

1.  À partir de n’importe quel serveur membre ou une station de travail affecté par les modifications de GPO, ouvrir une session localement.  

2.  Avec la souris, déplacez le pointeur dans le coin supérieur droit ou inférieur droit de l’écran. Lorsque le **icônes** barre s’affiche, cliquez sur **recherche**.  

3.  Dans le **recherche** , tapez **services**, puis cliquez sur **Services**.  

4.  Recherchez et double-cliquez sur **spouleur d’impression**.  

5.  Cliquez sur l'onglet **Se connecter**.  

6.  Sous **connecter en tant que**, sélectionnez le **ce compte** option.  

7.  Cliquez sur **Parcourir**, tapez le nom d’un compte qui est membre du groupe Admins du domaine, cliquez sur **vérifier les noms**, puis cliquez sur **OK**.  

8.  Sous **mot de passe** et **confirmer le mot de passe**, tapez le mot de passe du compte sélectionné, puis cliquez sur **OK**.  

9. Cliquez sur **OK** trois fois plus.  

10. Avec le bouton droit **spouleur d’impression** et cliquez sur **redémarrer**.  

11. Lorsque le service est redémarré, une boîte de dialogue similaire à ce qui suit doit apparaître.  

    ![sécuriser des groupes d’administrateurs de domaine](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_76.gif)  

##### <a name="revert-changes-to-the-printer-spooler-service"></a>Annuler les modifications pour le Service de spouleur d’impression  

1.  À partir de n’importe quel serveur membre ou une station de travail affecté par les modifications de GPO, ouvrir une session localement.  

2.  Avec la souris, déplacez le pointeur dans le coin supérieur droit ou inférieur droit de l’écran. Lorsque le **icônes** barre s’affiche, cliquez sur **recherche**.  

3.  Dans le **recherche** , tapez **services**, puis cliquez sur **Services**.  

4.  Recherchez et double-cliquez sur **spouleur d’impression**.  

5.  Cliquez sur l'onglet **Se connecter**.  

6.  Sous **connecter en tant que**, sélectionnez le **système Local** compte, puis cliquez sur **OK**.  

##### <a name="verify-deny-log-on-locally-gpo-settings"></a>Vérifiez les paramètres de stratégie de groupe « Refuser l’ouverture d’une session locale »  

1.  À partir de n’importe quel serveur membre ou une station de travail affecté par les modifications de GPO, tentent de se connecter localement à l’aide d’un compte qui est membre du groupe Admins du domaine. Une boîte de dialogue similaire à ce qui suit doit apparaître.  

    ![sécuriser des groupes d’administrateurs de domaine](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_77.gif)  

##### <a name="verify-deny-log-on-through-remote-desktop-services-gpo-settings"></a>Vérifiez les paramètres de stratégie de groupe « Refuse la connexion par le biais des Services Bureau à distance »    
1.  Avec la souris, déplacez le pointeur dans le coin supérieur droit ou inférieur droit de l’écran. Lorsque le **icônes** barre s’affiche, cliquez sur **recherche**.  

2.  Dans le **recherche** , tapez **connexion Bureau à distance**, puis cliquez sur **connexion Bureau à distance**.  

3.  Dans le **ordinateur** , tapez le nom de l’ordinateur que vous souhaitez vous connecter à, puis cliquez sur **Connect**. (Vous pouvez également taper l’adresse IP au lieu du nom d’ordinateur).  

4.  Lorsque vous y êtes invité, fournissez les informations d’identification d’un compte qui est membre du groupe Admins du domaine.  

5.  Une boîte de dialogue similaire à ce qui suit doit apparaître.  

    ![sécuriser des groupes d’administrateurs de domaine](media/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory/SAD_78.gif)  
