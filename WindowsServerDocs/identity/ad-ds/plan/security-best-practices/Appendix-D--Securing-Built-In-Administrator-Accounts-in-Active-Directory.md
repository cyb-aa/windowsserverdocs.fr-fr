---
ms.assetid: 11f36f2b-9981-4da0-9e7c-4eca78035f37
title: "Annexe D - sécurisation des comptes d’administrateur intégrés dans Active Directory"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 2878e661e1bb93fcdc3161c46b73d8e4baec76d2
ms.sourcegitcommit: 2782a80a916f8432c030af76e860a72f08481899
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/13/2018
---
# <a name="appendix-d-securing-built-in-administrator-accounts-in-active-directory"></a>Annexe d: sécurisation des comptes d’administrateur intégrés dans ActiveDirectory

>S’applique à: Windows Server2016, Windows Server2012R2, Windows Server2012


## <a name="appendix-d-securing-built-in-administrator-accounts-in-active-directory"></a>Annexe d: sécurisation des comptes d’administrateur intégrés dans ActiveDirectory  
Dans chaque domaine dans Active Directory, un compte d’administrateur est créé dans le cadre de la création du domaine. Ce compte est par défaut un membre des groupes Admins du domaine et administrateurs du domaine, et si le domaine est le domaine racine de forêt, le compte est également un membre du groupe Administrateurs de l’entreprise.

Utilisation du compte d’administrateur d’un domaine doit être réservée uniquement pour les activités de génération initiale et, éventuellement, des scénarios de reprise après sinistre. Pour garantir qu’un compte d’administrateur peut être utilisé pour effectuer des réparations dans le cas où aucun autre compte ne peut être utilisé, vous ne devez pas modifier l’appartenance par défaut du compte d’administrateur dans n’importe quel domaine de la forêt. Au lieu de cela, vous devez sécuriser le compte d’administrateur dans chaque domaine dans la forêt comme décrit dans la section suivante et détaillées dans les instructions pas à pas qui suivent. 

> [!NOTE]
> Ce guide sert à recommander la désactivation du compte. Cela a été supprimé en tant que le livre blanc de récupération de forêt utilise le compte d’administrateur par défaut. La raison est, c’est le seul compte qui permet d’ouverture de session sans serveur de catalogue Global.


#### <a name="controls-for-built-in-administrator-accounts"></a>Contrôles pour les comptes d’administrateur intégré  
Pour le compte administrateur intégré dans chaque domaine dans votre forêt, vous devez configurer les paramètres suivants:  

-   Activer la **compte est sensible et ne peut pas être délégué** indicateur sur le compte.  

-   Activer la **carte à puce est requise pour l’ouverture de session interactive** indicateur sur le compte.  

-   Configurer la stratégie de groupe pour restreindre l’utilisation du compte administrateur sur les systèmes joints au domaine:  

    -   Dans un ou plusieurs objets stratégie de groupe que vous créez et liez à une station de travail et le serveur membre unités d’organisation dans chaque domaine, ajouter le compte d’administrateur de chaque domaine pour les droits d’utilisateur suivants dans **ordinateur Configuration ordinateur\Stratégies\Paramètres Windows\Paramètres sécurité\Stratégies Settings\nom droits**:  

        -   Refuser l’accès à cet ordinateur à partir du réseau  

        -   Refuse la connexion en tant que tâche  

        -   Refuse la connexion en tant que service  

        -   Refuse la connexion par le biais des Services Bureau à distance  

> [!NOTE]  
> Lorsque vous ajoutez des comptes à ce paramètre, vous devez spécifier si vous configurez les comptes administrateur locaux ou les comptes d’administrateur de domaine. Par exemple, pour ajouter le compte d’administrateur du domaine NWTRADERS à ces refuser des droits, vous devez taper le compte en tant que NWTRADERS\Administrateur ou recherchez le compte d’administrateur du domaine NWTRADERS. Si vous tapez «Administrateur» dans ces paramètres de droits d’utilisateur dans l’éditeur d’objets de stratégie de groupe, vous allez restreindre le compte administrateur local sur chaque ordinateur sur lequel l’objet de stratégie de groupe est appliquée.
>   
> Nous vous recommandons de restreindre les comptes d’administrateur locales sur les serveurs membres et stations de travail dans la même manière que les comptes administrateur de domaine. Par conséquent, vous devez ajouter généralement le compte d’administrateur pour chaque domaine dans la forêt et le compte d’administrateur pour les ordinateurs locaux à ces paramètres de droits d’utilisateur. La capture d’écran suivante montre un exemple de configuration de ces droits d’utilisateur à bloquer les comptes d’administrateur locales et le compte d’administrateur d’un domaine d’effectuer des ouvertures de session qui ne doit pas être nécessaire pour ces comptes.  


![sécurisation des comptes d’administrateur intégré](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_23.gif)  

-   Configurer la stratégie de groupe pour restreindre les comptes d’administrateur sur les contrôleurs de domaine  
    -   Dans chaque domaine dans la forêt, l’objet de stratégie de groupe contrôleurs de domaine par défaut ou une stratégie liés aux contrôleurs de domaine, unité d’organisation doit être modifiée pour ajouter le compte d’administrateur de chaque domaine pour les droits d’utilisateur suivants dans **ordinateur Configuration ordinateur\Stratégies\Paramètres Windows\Paramètres sécurité\Stratégies Settings\nom droits**:   
        -   Refuser l’accès à cet ordinateur à partir du réseau  

        -   Refuse la connexion en tant que tâche  

        -   Refuse la connexion en tant que service  

        -   Refuse la connexion par le biais des Services Bureau à distance  

> [!NOTE]  
> Ces paramètres garantit que le compte administrateur du domaine ne peut pas être utilisé pour se connecter à un contrôleur de domaine, bien que le compte, s’il est activé, peut ouvrir une session localement aux contrôleurs de domaine. Étant donné que ce compte seulement doit être activé et utilisé dans les scénarios de récupération d’urgence, il est prévu que l’accès physique au moins un contrôleur de domaine sera disponible, ou que les autres comptes avec des autorisations d’accès à distance des contrôleurs de domaine peuvent être utilisés.  

-   Configurer l’audit des comptes d’administrateur  

    Lorsque vous avez sécurisé du compte d’administrateur de chaque domaine et désactivée, vous devez configurer l’audit pour surveiller les modifications apportées au compte. Si le compte est activé, son mot de passe est réinitialisé ou toutes les autres modifications sont apportées au compte, les alertes doivent être envoyées aux utilisateurs ou équipes responsables de l’administration d’Active Directory, en plus des équipes de réponse aux incidents de votre organisation.  

#### <a name="step-by-step-instructions-to-secure-built-in-administrator-accounts-in-active-directory"></a>Obtenir des Instructions pas à pas pour sécuriser les comptes d’administrateur intégrés dans Active Directory  

1.  Dans **le Gestionnaire de serveur**, cliquez sur **outils**, puis cliquez sur **Active Directory Users and Computers**.  

2.  Pour empêcher les attaques qui exploitent de délégation pour utiliser les informations d’identification du compte sur d’autres systèmes, procédez comme suit:  

    1.  Avec le bouton droit le **administrateur** de compte, puis cliquez sur **propriétés**.  

    2.  Cliquez sur le **compte** onglet.  

    3.  Sous **options de compte**, sélectionnez **compte est sensible et ne peut pas être délégué** indicateur comme indiqué dans la capture d’écran suivante, puis cliquez sur **OK**.  

        ![sécurisation des comptes d’administrateur intégré](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_24.gif)  

3.  Pour activer la **carte à puce est requise pour l’ouverture de session interactive** indicateur sur le compte, effectuez les opérations suivantes:  

    1.  Avec le bouton droit le **administrateur** de compte et sélectionnez **propriétés**.  

    2.  Cliquez sur le **compte** onglet.  

    3.  Sous **compte** options, sélectionnez-le le **carte à puce est requise pour l’ouverture de session interactive** indicateur comme indiqué dans la capture d’écran suivante, puis cliquez sur **OK**.  

        ![sécurisation des comptes d’administrateur intégré](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_25.gif) 

##### <a name="configuring-gpos-to-restrict-administrator-accounts-at-the-domain-level"></a>La configuration de stratégie de groupe pour limiter les comptes d’administrateur au niveau du domaine  

> [!WARNING]  
> Cet objet de stratégie de groupe ne doit jamais être liée au niveau du domaine, car il peut rendre le compte administrateur intégré inutilisable, même dans les scénarios de récupération d’urgence.  

1.  Dans **le Gestionnaire de serveur**, cliquez sur **outils**, puis cliquez sur **gestion des stratégies de groupe**.  

2.  Dans l’arborescence de la console, développez <Forest>\Domains\\<Domain>, puis **objets de stratégie de groupe** (où <Forest> est le nom de la forêt et <Domain> est le nom du domaine dans lequel vous voulez créer la stratégie de groupe).  

3.  Dans l’arborescence de la console, cliquez sur **objets de stratégie de groupe**, puis cliquez sur **New**.  

    ![sécurisation des comptes d’administrateur intégré](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_27.gif)  

4.  Dans le **nouvel objet GPO** boîte de dialogue, tapez <GPO Name>, puis cliquez sur **OK** (où <GPO Name> est le nom de cet objet de stratégie de groupe) comme indiqué dans la capture d’écran suivante.  

    ![sécurisation des comptes d’administrateur intégré](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_28.gif)  

5.  Dans le volet d’informations, cliquez sur <GPO Name>, puis cliquez sur **modifier**.  

6.  Accédez à **Configuration ordinateur\Stratégies\Paramètres Windows\Paramètres de Sécurité\stratégies**, puis cliquez sur **attribution des droits utilisateur**.  

    ![sécurisation des comptes d’administrateur intégré](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_29.gif)  

7.  Configurez les droits d’utilisateur pour empêcher le compte d’administrateur d’accéder à des serveurs membres et stations de travail sur le réseau en procédant comme suit:  

    1.  Double-cliquez sur **refuser l’accès à cet ordinateur à partir du réseau** et sélectionnez **définir ces paramètres de stratégie**.  

    2.  Cliquez sur **ajouter un utilisateur ou groupe** et cliquez sur **Parcourir**.  

    3.  Type **administrateur**, cliquez sur **vérifier les noms**, puis cliquez sur **OK**. Vérifiez que le compte est affiché dans <DomainName>\Username format comme indiqué dans la capture d’écran suivante.  

        ![sécurisation des comptes d’administrateur intégré](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_30.gif)  

    4.  Cliquez sur **OK**, et **OK** à nouveau.  

8.  Configurez les droits d’utilisateur pour empêcher le compte d’administrateur d’ouvrir une session en tant que traitement par lots en procédant comme suit:  

    1.  Double-cliquez sur **refuse la connexion en tant que traitement par lots** et sélectionnez **définir ces paramètres de stratégie**.  

    2.  Cliquez sur **ajouter un utilisateur ou groupe** et cliquez sur **Parcourir**.  

    3.  Type **administrateur**, cliquez sur **vérifier les noms**, puis cliquez sur **OK**. Vérifiez que le compte est affiché dans <DomainName>\Username format comme indiqué dans la capture d’écran suivante.  

        ![sécurisation des comptes d’administrateur intégré](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_31.gif)  

    4.  Cliquez sur **OK**, et **OK** à nouveau.  

9. Configurez les droits d’utilisateur pour empêcher le compte d’administrateur d’ouvrir une session en tant que service en effectuant les opérations suivantes:  

    1.  Double-cliquez sur **refuse la connexion en tant que service** et sélectionnez **définir ces paramètres de stratégie**.  

    2.  Cliquez sur **ajouter un utilisateur ou groupe** et cliquez sur **Parcourir**.  

    3.  Type **administrateur**, cliquez sur **vérifier les noms**, puis cliquez sur **OK**. Vérifiez que le compte est affiché dans <DomainName>\Username format comme indiqué dans la capture d’écran suivante.  

        ![sécurisation des comptes d’administrateur intégré](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_32.gif)  

    4.  Cliquez sur **OK**, et **OK** à nouveau.  

10. Configurez les droits d’utilisateur pour empêcher le compte BA d’accéder à des serveurs membres et les stations de travail via les Services Bureau à distance en effectuant les opérations suivantes:  

    1.  Double-cliquez sur **refuse la connexion par le biais des Services Bureau à distance** et sélectionnez **définir ces paramètres de stratégie**.  

    2.  Cliquez sur **ajouter un utilisateur ou groupe** et cliquez sur **Parcourir**.  

    3.  Type **administrateur**, cliquez sur **vérifier les noms**, puis cliquez sur **OK**. Vérifiez que le compte est affiché dans <DomainName>\Username format comme indiqué dans la capture d’écran suivante.  

        ![sécurisation des comptes d’administrateur intégré](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_33.gif)  

    4.  Cliquez sur **OK**, et **OK** à nouveau.  

11. Pour quitter **éditeur de gestion de stratégie de groupe**, cliquez sur **fichier**, puis cliquez sur **quitter**.  

12. Dans **gestion des stratégies de groupe**, liez l’objet de stratégie de groupe pour les serveurs membres et d’une station de travail unités d’organisation en procédant comme suit:  

    1.  Accédez à la <Forest>\Domains\\<Domain> (où <Forest>est le nom de la forêt et <Domain>est le nom du domaine dans lequel vous souhaitez définir la stratégie de groupe).  

    2.  Avec le bouton droit de l’unité d’organisation que l’objet de stratégie de groupe seront appliqués au et cliquez sur **lier un objet de stratégie de groupe existant**.  

        ![sécurisation des comptes d’administrateur intégré](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_34.gif)  

    3.  Sélectionnez l’objet de stratégie de groupe que vous avez créé et cliquez sur **OK**.  

        ![sécurisation des comptes d’administrateur intégré](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_35.gif)  

    4.  Créer des liens vers tous les autres unités d’organisation qui contiennent des stations de travail.  

    5.  Créer des liens vers tous les autres unités d’organisation qui contiennent des serveurs membres.  

> [!IMPORTANT]  
> Lorsque vous ajoutez le compte d’administrateur pour ces paramètres, vous spécifiez si vous configurez un compte d’administrateur local ou un compte d’administrateur de domaine par la façon dont vous étiquetez les comptes. Par exemple, pour ajouter le compte d’administrateur du domaine TAILSPINTOYS à ces refuser des droits, vous devez sélectionner le compte d’administrateur pour le domaine TAILSPINTOYS, qui apparaît en tant que TAILSPINTOYS\Administrator. Si vous tapez «Administrateur» dans ces paramètres de droits d’utilisateur dans l’éditeur d’objets de stratégie de groupe, vous allez restreindre le compte administrateur local sur chaque ordinateur sur lequel l’objet de stratégie de groupe est appliquée, comme décrit précédemment.  

#### <a name="verification-steps"></a>Étapes de vérification  
Les étapes de vérification présentées ici sont spécifiques à Windows 8 et Windows Server 2012.  

##### <a name="verify-smart-card-is-required-for-interactive-logon-account-option"></a>Vérifier «carte à puce est requise pour l’ouverture de session interactive» Option de compte  

1.  À partir de n’importe quel serveur membre ou une station de travail affectées par les modifications de stratégie de groupe, essayez d’ouvrir une session interactive au domaine à l’aide du compte administrateur du domaine. Après avoir essayé d’ouvrir une session, une boîte de dialogue semblable à la suivante doit apparaître.  

![sécurisation des comptes d’administrateur intégré](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_36.gif)  

##### <a name="verify-account-is-disabled-account-option"></a>Vérifiez que «Le compte est désactivé» Option de compte  

1.  À partir de n’importe quel serveur membre ou une station de travail affectées par les modifications de stratégie de groupe, essayez d’ouvrir une session interactive au domaine à l’aide du compte administrateur du domaine. Après avoir essayé d’ouvrir une session, une boîte de dialogue semblable à la suivante doit apparaître.  

![sécurisation des comptes d’administrateur intégré](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_37.gif)  

##### <a name="verify-deny-access-to-this-computer-from-the-network-gpo-settings"></a>Vérifier les paramètres de stratégie de groupe «Refuser l’accès à cet ordinateur à partir du réseau»  
À partir de n’importe quel serveur membre ou une station de travail qui n’est pas affectée par les modifications de stratégie de groupe (par exemple, un serveur de renvoi), essayez d’accéder à un serveur membre ou une station de travail sur le réseau qui est affecté par les modifications de stratégie de groupe. Pour vérifier les paramètres de stratégie de groupe, tenter de mapper le lecteur du système à l’aide de la **NET USE** commande en effectuant les étapes suivantes:  

1.  Ouvrez une session sur le domaine à l’aide du compte administrateur du domaine.  

2.  Avec la souris, déplacez le pointeur dans le coin supérieur droit ou inférieur droit de l’écran. Lorsque le **icônes** barre s’affiche, cliquez sur **recherche**.  

3.  Dans le **recherche**, tapez **invite de commandes**, avec le bouton droit **invite de commandes**, puis cliquez sur **exécuter en tant qu’administrateur** pour ouvrir une invite de commandes avec élévation de privilèges.  

4.  Lorsque vous êtes invité à approuver l’élévation, cliquez sur **Oui**.  

    ![sécurisation des comptes d’administrateur intégré](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_38.gif)  

5.  Dans le **invite de commandes** fenêtre, tapez **commande net use \\\<Server Name>\c$**, où <Server Name> est le nom du serveur membre ou de station de travail vous essayez d’accéder via le réseau.  

6.  La capture d’écran suivante affiche le message d’erreur qui doit apparaître.  

    ![sécurisation des comptes d’administrateur intégré](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_39.gif)  

##### <a name="verify-deny-log-on-as-a-batch-job-gpo-settings"></a>Vérifier les paramètres de stratégie de groupe «Refuse la connexion en tant que tâche»  

À partir de n’importe quel serveur membre ou une station de travail affectées par les modifications de stratégie de groupe, ouvrez une session localement.  

###### <a name="create-a-batch-file"></a>Créer un fichier de commandes  

1.  Avec la souris, déplacez le pointeur dans le coin supérieur droit ou inférieur droit de l’écran. Lorsque le **icônes** barre s’affiche, cliquez sur **recherche**.  

2.  Dans le **recherche**, tapez **le bloc-notes**, puis cliquez sur **le bloc-notes**.  

3.  Dans **le bloc-notes**, type **dir c:**.  

4.  Cliquez sur **fichier** et cliquez sur **Enregistrer sous**.  

5.  Dans le **Filename** , tapez ** <Filename>.bat** (où <Filename> est le nom du nouveau fichier de commandes).  

###### <a name="schedule-a-task"></a>Planifier une tâche  

1.  Avec la souris, déplacez le pointeur dans le coin supérieur droit ou inférieur droit de l’écran. Lorsque le **icônes** barre s’affiche, cliquez sur **recherche**.  

2.  Dans le **recherche** , tapez **le Planificateur de tâches**, puis cliquez sur **le Planificateur de tâches**.  

    > [!NOTE]  
    > Sur les ordinateurs exécutant Windows 8, dans la zone de recherche, tapez **planifier des tâches**, puis cliquez sur **planifier des tâches**.  

3.  Sur **le Planificateur de tâches**, cliquez sur **Action**, puis cliquez sur **créer une tâche**.  

4.  Dans le **créer une tâche** boîte de dialogue, tapez ** <Task Name> ** (où ** <Task Name> ** est le nom de la nouvelle tâche).  

5.  Cliquez sur le **Actions** onglet, puis cliquez sur **New**.  

6.  Sous **Action:**, sélectionnez **démarrer un programme**.  

7.  Sous **programme ou le script:**, cliquez sur **Parcourir**, recherchez et sélectionnez le fichier de commandes créé dans la section «Créer un fichier de commandes», puis cliquez sur **ouvrir**.  

8.  Cliquez sur **OK**.  

9. Cliquez sur le **général** onglet.  

10. Sous **sécurité** options, cliquez sur **modifier un utilisateur ou groupe**.  

11. Tapez le nom du compte BA au niveau du domaine, cliquez sur **vérifier les noms**, puis cliquez sur **OK**.  

12. Sélectionnez **exécuter même si l’utilisateur est connecté ou non** et **ne stockent pas de mot de passe**. La tâche n’aura accès aux ressources de l’ordinateur local.  

13. Cliquez sur **OK**.  

14. Une boîte de dialogue doit apparaître, compte d’utilisateur demande des informations d’identification pour exécuter la tâche.  

15. Après avoir entré les informations d’identification, cliquez sur **OK**.  

16. Une boîte de dialogue semblable à la suivante doit apparaître.  

    ![sécurisation des comptes d’administrateur intégré](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_40.gif)  

##### <a name="verify-deny-log-on-as-a-service-gpo-settings"></a>Vérifier les paramètres de stratégie de groupe «Refuse la connexion en tant que service»  

1.  À partir de n’importe quel serveur membre ou une station de travail affectées par les modifications de stratégie de groupe, ouvrez une session localement.  

2.  Avec la souris, déplacez le pointeur dans le coin supérieur droit ou inférieur droit de l’écran. Lorsque le **icônes** barre s’affiche, cliquez sur **recherche**.  

3.  Dans le **recherche** , tapez **services**, puis cliquez sur **Services**.  

4.  Recherchez et double-cliquez sur **spouleur d’impression**.  

5.  Cliquez sur le **ouverture de session** onglet.  

6.  Sous **ouvrir une session en tant que:**, sélectionnez **ce compte**.  

7.  Cliquez sur **Parcourir**, tapez le nom du compte BA au niveau du domaine, cliquez sur **vérifier les noms**, puis cliquez sur **OK**.  

8.  Sous **mot de passe:** et **confirmer le mot de passe:**, tapez le mot de passe du compte administrateur, puis cliquez sur **OK**.  

9. Cliquez sur **OK** trois fois de plus.  

10. Cliquez sur le **service Spouleur d’impression** et sélectionnez **redémarrer**.  

11. Lorsque le service est redémarré, une boîte de dialogue semblable au suivant doit s’afficher.  

    ![sécurisation des comptes d’administrateur intégré](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_41.gif)  

##### <a name="revert-changes-to-the-printer-spooler-service"></a>Annuler les modifications pour le Service Spouleur d’imprimante  

1.  À partir de n’importe quel serveur membre ou une station de travail affectées par les modifications de stratégie de groupe, ouvrez une session localement.  

2.  Avec la souris, déplacez le pointeur dans le coin supérieur droit ou inférieur droit de l’écran. Lorsque le **icônes** barre s’affiche, cliquez sur **recherche**.  

3.  Dans le **recherche** , tapez **services**, puis cliquez sur **Services**.  

4.  Recherchez et double-cliquez sur **spouleur d’impression**.  

5.  Cliquez sur le **ouverture de session** onglet.  

6.  Sous **ouvrir une session en tant que:**, sélectionnez le **système Local** de compte, puis cliquez sur **OK**.  

##### <a name="verify-deny-log-on-through-remote-desktop-services-gpo-settings"></a>Vérifier les paramètres de stratégie de groupe «Refuse la connexion par le biais des Services Bureau à distance»

1.  Avec la souris, déplacez le pointeur dans le coin supérieur droit ou inférieur droit de l’écran. Lorsque le **icônes** barre s’affiche, cliquez sur **recherche**.  

2.  Dans le **recherche** , tapez **connexion Bureau à distance**, puis cliquez sur **connexion Bureau à distance**.  

3.  Dans le **ordinateur** , tapez le nom de l’ordinateur que vous souhaitez vous connecter à, puis cliquez sur **Connect**. (Vous pouvez également taper l’adresse IP au lieu du nom d’ordinateur).  

4.  Lorsque vous y êtes invité, fournissez les informations d’identification pour le nom du compte BA au niveau du domaine.  

5.  Une boîte de dialogue semblable à la suivante doit apparaître.  

    ![sécurisation des comptes d’administrateur intégré](media/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory/SAD_42.gif)  
