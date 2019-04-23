---
ms.assetid: f643099e-f9c6-476f-9378-5a9228c39b33
title: Annexe E - sécurisation des groupes administrateurs d’entreprise dans Active Directory
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 976eb8c7159c8349b72bee05a5248b5cc116d96b
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59856720"
---
# <a name="appendix-e-securing-enterprise-admins-groups-in-active-directory"></a>Annexe E : Sécurisation des groupes administrateurs d’entreprise dans Active Directory

>S'applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012


## <a name="appendix-e-securing-enterprise-admins-groups-in-active-directory"></a>Annexe E : Sécurisation des groupes administrateurs d’entreprise dans Active Directory  
Le groupe Administrateurs de l’entreprise (EA), qui est hébergé dans le domaine racine de forêt, ne doit contenir aucun utilisateur sur une base quotidienne, à l’exception du compte d’administrateur du domaine racine, sous réserve qu’il est sécurisé comme décrit dans [annexe d : Sécurisation des comptes d’administrateur intégré dans Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory.md).  

Administrateurs de l’entreprise sont, par défaut, les membres du groupe Administrateurs dans chaque domaine dans la forêt. Vous ne devez pas supprimer le groupe EA des groupes d’administrateurs dans chaque domaine, car en cas d’un scénario de récupération d’urgence de forêt, les droits EA sera probablement nécessaire. Groupe d’administrateurs de l’entreprise de la forêt doit être sécurisée comme détaillé dans les instructions détaillées qui suivent.  

Pour le groupe Administrateurs de l’entreprise dans la forêt :  

1.  Dans la stratégie de groupe liés aux unités d’organisation contenant les serveurs membres et les stations de travail dans chaque domaine, le groupe Administrateurs de l’entreprise doit être ajouté pour les droits d’utilisateur suivants dans **ordinateur Configuration\Policies\Windows Settings\Security Settings\Local Policies\ Attribution des droits utilisateur**:  

    -   Interdire l’accès à cet ordinateur à partir du réseau  

    -   Interdire l’ouverture de session en tant que tâche  

    -   Interdire l’ouverture de session en tant que service  

    -   Interdire l’ouverture d’une session locale  

    -   Interdire l’ouverture de session par les services Bureau à distance  

2.  Configurez l’audit pour envoyer des alertes si des modifications sont apportées aux propriétés ou aux membres du groupe Administrateurs de l’entreprise.  

### <a name="step-by-step-instructions-for-removing-all-members-from-the-enterprise-admins-group"></a>Obtenir des Instructions détaillées pour la suppression de tous les membres du groupe d’administrateurs entreprise  

1.  Dans **le Gestionnaire de serveur**, cliquez sur **outils**, puis cliquez sur **Active Directory Users and Computers**.  

2.  Si vous ne gérez pas le domaine racine de la forêt, dans l’arborescence de la console, cliquez sur <Domain>, puis cliquez sur **changer de domaine** (où <Domain> est le nom du domaine que vous administrez).  

    ![sécuriser des groupes d’administrateurs entreprise](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_43.gif)  

3.  Dans le **changer de domaine** boîte de dialogue, cliquez sur **Parcourir**, sélectionnez le domaine racine de la forêt, puis cliquez sur **OK**.  

    ![sécuriser des groupes d’administrateurs entreprise](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_44.gif)  

4.  Pour supprimer tous les membres du groupe de contrat entreprise :  

    1.  Double-cliquez sur le **administrateurs de l’entreprise** de groupe, puis cliquez sur le **membres** onglet.  

        ![sécuriser des groupes d’administrateurs entreprise](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_45.gif)  

    2.  Sélectionnez un membre du groupe, cliquez sur **supprimer**, cliquez sur **Oui**, puis cliquez sur **OK**.  

5.  Répétez l’étape 2 jusqu'à ce que tous les membres du groupe de contrat entreprise ont été supprimés.  

### <a name="step-by-step-instructions-to-secure-enterprise-admins-in-active-directory"></a>Instructions pas à pas pour sécuriser les administrateurs de l’entreprise dans Active Directory  

1.  Dans **le Gestionnaire de serveur**, cliquez sur **outils**, puis cliquez sur **Group Policy Management**.  

2.  Dans l’arborescence de la console, développez <Forest>\Domains\\<Domain>, puis **les objets de stratégie de groupe** (où <Forest> est le nom de la forêt et <Domain> est le nom du domaine où vous souhaitez Définissez la stratégie de groupe).  

    > [!NOTE]  
    > Dans une forêt qui contient plusieurs domaines, un objet de stratégie de groupe similaire doit être créée dans chaque domaine que le groupe Administrateurs de l’entreprise soit sécurisé.  

3.  Dans l’arborescence de la console, cliquez sur **les objets de stratégie de groupe**, puis cliquez sur **New**.  

    ![sécuriser des groupes d’administrateurs entreprise](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_46.gif)  

4.  Dans le **nouvel objet GPO** boîte de dialogue, tapez <GPO Name>, puis cliquez sur **OK** (où <GPO Name> est le nom de cet objet de stratégie de groupe).  

    ![sécuriser des groupes d’administrateurs entreprise](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_47.gif)  

5.  Dans le volet détails, cliquez sur <GPO Name>, puis cliquez sur **modifier**.  

6.  Accédez à **Configuration ordinateur\Stratégies\Paramètres Windows\Paramètres de Sécurité\stratégies**, puis cliquez sur **attribution des droits utilisateur**.  

    ![sécuriser des groupes d’administrateurs entreprise](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_48.gif)  

7.  Configurer les droits d’utilisateur pour empêcher les membres du groupe Administrateurs de l’entreprise d’accéder à des serveurs membres et les stations de travail sur le réseau en procédant comme suit :  

    1.  Double-cliquez sur **refuser l’accès à cet ordinateur à partir du réseau** et sélectionnez **définir ces paramètres de stratégie**.  

    2.  Cliquez sur **ajouter un utilisateur ou groupe** et cliquez sur **Parcourir**.  

    3.  Type **administrateurs de l’entreprise**, cliquez sur **vérifier les noms**, puis cliquez sur **OK**.  

        ![sécuriser des groupes d’administrateurs entreprise](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_49.gif)  

    4.  Cliquez sur **OK**, et **OK** à nouveau.  

8.  Configurer les droits d’utilisateur pour empêcher les membres du groupe Administrateurs de l’entreprise à partir de la journalisation comme un traitement par lots en procédant comme suit :  

    1.  Double-cliquez sur **refuse la connexion en tant que traitement par lots** et sélectionnez **définir ces paramètres de stratégie**.  

    2.  Cliquez sur **ajouter un utilisateur ou groupe** et cliquez sur **Parcourir**.  

        > [!NOTE]  
        > Dans une forêt qui contient plusieurs domaines, cliquez sur **emplacements** et sélectionnez le domaine racine de la forêt.  

    3.  Type **administrateurs de l’entreprise**, cliquez sur **vérifier les noms**, puis cliquez sur **OK**.  

        ![sécuriser des groupes d’administrateurs entreprise](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_50.gif)  

    4.  Cliquez sur **OK**, et **OK** à nouveau.  

9. Configurer les droits d’utilisateur pour empêcher les membres du groupe EA de journalisation en tant que service en procédant comme suit :  

    1.  Double-cliquez sur **journal refuser en tant que service** et sélectionnez **définir ces paramètres de stratégie**.  

    2.  Cliquez sur **ajouter un utilisateur ou groupe** puis cliquez sur **Parcourir**.  

        > [!NOTE]  
        > Dans une forêt qui contient plusieurs domaines, cliquez sur **emplacements** et sélectionnez le domaine racine de la forêt.  

    3.  Type **administrateurs de l’entreprise**, cliquez sur **vérifier les noms**, puis cliquez sur **OK**.  

        ![sécuriser des groupes d’administrateurs entreprise](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_51.gif)  

    4.  Cliquez sur **OK**, et **OK** à nouveau.  

10. Configurer les droits d’utilisateur pour empêcher les membres du groupe Administrateurs de l’entreprise à partir de la session locale sur les serveurs membres et les stations de travail en procédant comme suit :  

    1.  Double-cliquez sur **refuse la connexion locale** et sélectionnez **définir ces paramètres de stratégie**.  

    2.  Cliquez sur **ajouter un utilisateur ou groupe** puis cliquez sur **Parcourir**.  

        > [!NOTE]  
        > Dans une forêt qui contient plusieurs domaines, cliquez sur **emplacements** et sélectionnez le domaine racine de la forêt.  

    3.  Type **administrateurs de l’entreprise**, cliquez sur **vérifier les noms**, puis cliquez sur **OK**.  

        ![sécuriser des groupes d’administrateurs entreprise](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_52.gif)  

    4.  Cliquez sur **OK**, et **OK** à nouveau.  

11. Configurer les droits d’utilisateur pour empêcher les membres du groupe Administrateurs de l’entreprise d’accéder à des serveurs membres et les stations de travail via les Services Bureau à distance en procédant comme suit :  

    1.  Double-cliquez sur **refuse la connexion via les Services Bureau à distance** et sélectionnez **définir ces paramètres de stratégie**.  

    2.  Cliquez sur **ajouter un utilisateur ou groupe** puis cliquez sur **Parcourir**.  

        > [!NOTE]  
        > Dans une forêt qui contient plusieurs domaines, cliquez sur **emplacements** et sélectionnez le domaine racine de la forêt.  

    3.  Type **administrateurs de l’entreprise**, cliquez sur **vérifier les noms**, puis cliquez sur **OK**.  

        ![sécuriser des groupes d’administrateurs entreprise](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_53.gif)  

    4.  Cliquez sur **OK**, et **OK** à nouveau.  

12. Pour quitter **éditeur de gestion de stratégie de groupe**, cliquez sur **fichier**, puis cliquez sur **quitter**.  

13. Dans **Group Policy Management**, lier l’objet de stratégie de groupe pour le serveur membre et de la station de travail unités d’organisation en procédant comme suit :  

    1.  Accédez à la <Forest>\Domains\\ <Domain> (où <Forest> est le nom de la forêt et <Domain> est le nom du domaine dans lequel vous souhaitez définir la stratégie de groupe).  

    2.  Avec le bouton droit de l’unité d’organisation qui seront appliqués à l’objet de stratégie de groupe et cliquez sur **lier un objet de stratégie de groupe existant**.  

        ![sécuriser des groupes d’administrateurs entreprise](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_54.gif)  

    3.  Sélectionnez l’objet de stratégie de groupe que vous venez de créer et cliquez sur **OK**.  

        ![sécuriser des groupes d’administrateurs entreprise](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_55.gif)  

    4.  Créer des liens vers toutes les autres unités d’organisation qui contiennent des stations de travail.  

    5.  Créer des liens vers toutes les autres unités d’organisation qui contiennent des serveurs membres.  

    6.  Dans une forêt qui contient plusieurs domaines, un objet de stratégie de groupe similaire doit être créée dans chaque domaine que le groupe Administrateurs de l’entreprise soit sécurisé.  

> [!IMPORTANT]  
> Si les serveurs jump sont utilisés pour administrer les contrôleurs de domaine et Active Directory, vérifiez que les serveurs jump sont trouvent dans une unité d’organisation sur lesquels cette stratégie de groupe n’est pas lié.  

### <a name="verification-steps"></a>Étapes de vérification  

#### <a name="verify-deny-access-to-this-computer-from-the-network-gpo-settings"></a>Vérifiez les paramètres de stratégie de groupe « Refuser l’accès à cet ordinateur à partir du réseau »  
À partir de n’importe quel serveur membre ou une station de travail qui n’est pas affectée par les modifications de GPO (par exemple, un « serveur de renvoi) », essayez d’accéder à un serveur membre ou station de travail sur le réseau qui est affecté par les modifications de GPO. Pour vérifier les paramètres de stratégie de groupe, essayez de mapper le lecteur du système à l’aide de la **NET USE** commande en effectuant les étapes suivantes :  

1.  Connectez-vous localement à l’aide d’un compte qui est membre du groupe EA.  

2.  Avec la souris, déplacez le pointeur dans le coin supérieur droit ou inférieur droit de l’écran. Lorsque le **icônes** barre s’affiche, cliquez sur **recherche**.  

3.  Dans le **recherche** , tapez **invite de commandes**, avec le bouton droit **invite de commandes**, puis cliquez sur **exécuter en tant qu’administrateur** pour ouvrir une avec élévation de privilèges invite de commandes.  

4.  Lorsque vous êtes invité à approuver l’élévation, cliquez sur **Oui**.  

    ![sécuriser des groupes d’administrateurs entreprise](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_56.gif)  

5.  Dans le **invite de commandes** fenêtre, tapez **net utilisation \\ \\ \<nom du serveur\>\c$**, où \<nom du serveur\> est le nom du serveur membre ou station de travail que vous tentez d’accéder via le réseau.  

6.  La capture d’écran suivante montre le message d’erreur qui doit apparaître.  

    ![sécuriser des groupes d’administrateurs entreprise](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_57.gif)  

#### <a name="verify-deny-log-on-as-a-batch-job-gpo-settings"></a>Vérifiez les paramètres de stratégie de groupe « Refuse la connexion en tant que tâche »  

À partir de n’importe quel serveur membre ou une station de travail affecté par les modifications de GPO, ouvrir une session localement.  

##### <a name="create-a-batch-file"></a>Créer un fichier de commandes  

1.  Avec la souris, déplacez le pointeur dans le coin supérieur droit ou inférieur droit de l’écran. Lorsque le **icônes** barre s’affiche, cliquez sur **recherche**.  

2.  Dans le **recherche** , tapez **le bloc-notes**, puis cliquez sur **le bloc-notes**.  

3.  Dans **le bloc-notes**, type **dir c:**.  

4.  Cliquez sur **fichier**, puis cliquez sur **Enregistrer sous**.  

5.  Dans le **fichier** zone Nom, tapez  **<Filename>.bat** (où <Filename> est le nom du nouveau fichier batch).  

##### <a name="schedule-a-task"></a>Planifier une tâche  

1.  Avec la souris, déplacez le pointeur dans le coin supérieur droit ou inférieur droit de l’écran. Lorsque le **icônes** barre s’affiche, cliquez sur **recherche**.  

2.  Dans le **recherche** , tapez **Planificateur de tâches**, puis cliquez sur **Planificateur de tâches**.  

    > [!NOTE]  
    > Sur les ordinateurs exécutant Windows 8, dans le **recherche** , tapez **planifier des tâches**, puis cliquez sur **planifier des tâches**.  

3.  Cliquez sur **Action**, puis cliquez sur **créer une tâche**.  

4.  Dans le **créer une tâche** boîte de dialogue, tapez **<Task Name>** (où <Task Name> est le nom de la nouvelle tâche).  

5.  Cliquez sur le **Actions** onglet, puis cliquez sur **New**.  

6.  Dans le **Action** champ, sélectionnez **démarrer un programme**.  

7.  Sous **programme/script**, cliquez sur **Parcourir**, recherchez et sélectionnez le fichier de commandes créé dans le **créer un fichier de commandes** section, puis cliquez sur **ouvrir**.  

8.  Cliquez sur **OK**.  

9. Cliquez sur l’onglet **Général**.  

10. Dans le **options de sécurité** , cliquez sur **changer d’utilisateur ou groupe**.  

11. Tapez le nom d’un compte qui est membre du groupe EAs, cliquez sur **vérifier les noms**, puis cliquez sur **OK**.  

12. Sélectionnez **exécuter même si l’utilisateur est connecté ou non** et sélectionnez **ne stockez pas de mot de passe**. La tâche a uniquement accès aux ressources de l’ordinateur local.  

13. Cliquez sur **OK**.  

14. Compte d’utilisateur demandeur une boîte de dialogue doit apparaître, informations d’identification pour exécuter la tâche.  

15. Après avoir entré les informations d’identification, cliquez sur **OK**.  

16. Une boîte de dialogue similaire à ce qui suit doit apparaître.  

    ![sécuriser des groupes d’administrateurs entreprise](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_58.gif)  

#### <a name="verify-deny-log-on-as-a-service-gpo-settings"></a>Vérifiez les paramètres de stratégie de groupe « Refuse la connexion en tant que service »  

1.  À partir de n’importe quel serveur membre ou une station de travail affecté par les modifications de GPO, ouvrir une session localement.  

2.  Avec la souris, déplacez le pointeur dans le coin supérieur droit ou inférieur droit de l’écran. Lorsque le **icônes** barre s’affiche, cliquez sur **recherche**.  

3.  Dans le **recherche** , tapez **services**, puis cliquez sur **Services**.  

4.  Recherchez et double-cliquez sur **spouleur d’impression**.  

5.  Cliquez sur l'onglet **Se connecter**.  

6.  Sous **connecter en tant que**, sélectionnez **ce compte**.  

7.  Cliquez sur **Parcourir**, tapez le nom d’un compte qui est membre du groupe EAs, cliquez sur **vérifier les noms**, puis cliquez sur **OK**.  

8.  Sous **mot de passe :** et **confirmer le mot de passe**, tapez le mot de passe du compte sélectionné, puis cliquez sur **OK**.  

9. Cliquez sur **OK** trois fois plus.  

10. Cliquez sur le **spouleur d’impression** de service et sélectionnez **redémarrer**.  

11. Lorsque le service est redémarré, une boîte de dialogue similaire à ce qui suit doit apparaître.  

    ![sécuriser des groupes d’administrateurs entreprise](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_59.gif)  

#### <a name="revert-changes-to-the-printer-spooler-service"></a>Annuler les modifications pour le Service de spouleur d’impression  

1.  À partir de n’importe quel serveur membre ou une station de travail affecté par les modifications de GPO, ouvrir une session localement.  

2.  Avec la souris, déplacez le pointeur dans le coin supérieur droit ou inférieur droit de l’écran. Lorsque le **icônes** barre s’affiche, cliquez sur **recherche**.  

3.  Dans le **recherche** , tapez **services**, puis cliquez sur **Services**.  

4.  Recherchez et double-cliquez sur **spouleur d’impression**.  

5.  Cliquez sur l'onglet **Se connecter**.  

6.  Sous **connecter en tant que**, sélectionnez le **système Local** compte, puis cliquez sur **OK**.  

#### <a name="verify-deny-log-on-locally-gpo-settings"></a>Vérifiez les paramètres de stratégie de groupe « Refuser l’ouverture d’une session locale »  

1.  À partir de n’importe quel serveur membre ou une station de travail affecté par les modifications de GPO, tentent de se connecter localement à l’aide d’un compte qui est membre du groupe EA. Une boîte de dialogue similaire à ce qui suit doit apparaître.  

    ![sécuriser des groupes d’administrateurs entreprise](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_60.gif)  

#### <a name="verify-deny-log-on-through-remote-desktop-services-gpo-settings"></a>Vérifiez les paramètres de stratégie de groupe « Refuse la connexion par le biais des Services Bureau à distance »  

1.  Avec la souris, déplacez le pointeur dans le coin supérieur droit ou inférieur droit de l’écran. Lorsque le **icônes** barre s’affiche, cliquez sur **recherche**.  

2.  Dans le **recherche** , tapez **connexion Bureau à distance**, puis cliquez sur **connexion Bureau à distance**.  

3.  Dans le **ordinateur** , tapez le nom de l’ordinateur que vous souhaitez vous connecter à, puis cliquez sur **Connect**. (Vous pouvez également taper l’adresse IP au lieu du nom d’ordinateur).  

4.  Lorsque vous y êtes invité, fournissez les informations d’identification d’un compte qui est membre du groupe EA.  

5.  Une boîte de dialogue similaire à ce qui suit doit apparaître.  

    ![sécuriser des groupes d’administrateurs entreprise](media/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory/SAD_61.gif)  
