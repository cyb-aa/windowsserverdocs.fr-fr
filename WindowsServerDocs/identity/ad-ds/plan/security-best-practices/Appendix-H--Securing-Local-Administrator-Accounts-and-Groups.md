---
ms.assetid: ea015cbc-dea9-4c72-a9d8-d6c826d07608
title: "Annexe H - sécurisation des comptes d’administrateur Local et les groupes"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 222e6725456bb76267240467469e97c5b64fc888
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2017
---
# <a name="appendix-h-securing-local-administrator-accounts-and-groups"></a>Annexeh: Sécurisation des comptes administrateur locaux et groupes

>S’applique à: Windows Server2016, Windows Server2012R2, Windows Server2012


## <a name="appendix-h-securing-local-administrator-accounts-and-groups"></a>Annexeh: Sécurisation des comptes administrateur locaux et groupes  
Toutes les versions de Windows actuellement dans le support standard, le compte administrateur local est désactivé par défaut, ce qui rend le compte inutilisables pour pass-the-hash et autres attaques de vol d’informations d’identification. Toutefois, dans les environnements qui contiennent des systèmes d’exploitation ou dans lequel les comptes d’administrateur locales ont été activées, ces comptes peuvent être utilisés comme décrit précédemment pour propager les compromis entre les stations de travail et les serveurs membres. Chaque compte d’administrateur et le groupe local doivent être sécurisé, comme décrit dans les instructions pas à pas qui suivent.  

Pour plus d’informations sur les considérations en matière de sécurisation des groupes d’administrateur intégré (BA), voir [implémentation de modèles d’administration de moindre privilège](../../../ad-ds/plan/security-best-practices/Implementing-Least-Privilege-Administrative-Models.md).  

#### <a name="controls-for-local-administrator-accounts"></a>Contrôles pour les comptes d’administrateur Local  
Pour le compte administrateur local dans chaque domaine dans votre forêt, vous devez configurer les paramètres suivants:  

-   Configurer la stratégie de groupe pour restreindre l’utilisation du compte d’administrateur du domaine sur des systèmes joints au domaine  
    -   Dans un ou plusieurs objets stratégie de groupe que vous créez et liez à une station de travail et le serveur membre unités d’organisation dans chaque domaine, ajoutez le compte d’administrateur pour les droits d’utilisateur suivants dans **ordinateur Configuration ordinateur\Stratégies\Paramètres Windows\Paramètres sécurité\Stratégies Settings\nom droits**:  

        -   Refuser l’accès à cet ordinateur à partir du réseau  

        -   Refuse la connexion en tant que tâche  

        -   Refuse la connexion en tant que service  

        -   Refuse la connexion par le biais des Services Bureau à distance  

#### <a name="step-by-step-instructions-to-secure-local-administrators-groups"></a>Step-by-Step Instructions pour sécuriser des groupes d’administrateurs locaux  

###### <a name="configuring-gpos-to-restrict-administrator-account-on-domain-joined-systems"></a>La configuration de stratégie de groupe pour restreindre le compte d’administrateur sur les systèmes joints au domaine  

1.  Dans **le Gestionnaire de serveur**, cliquez sur **outils**, puis cliquez sur **gestion des stratégies de groupe**.  

2.  Dans l’arborescence de la console, développez <Forest>\Domains\\<Domain>, puis **objets de stratégie de groupe** (où <Forest>est le nom de la forêt et <Domain>est le nom du domaine dans lequel vous souhaitez définir la stratégie de groupe).  

3.  Dans l’arborescence de la console, cliquez sur **objets de stratégie de groupe**, puis cliquez sur **New**.  

    ![Groupes et comptes d’administrateur local sécurisé](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_101.png)  

4.  Dans le **nouvel objet GPO** boîte de dialogue, tapez **<GPO Name>**, puis cliquez sur **OK** (où <GPO Name>est le nom de cet objet de stratégie de groupe).  

    ![Groupes et comptes d’administrateur local sécurisé](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_102.png)  

5.  Dans le volet d’informations, cliquez sur **<GPO Name>**, puis cliquez sur **modifier**.  

6.  Accédez à **Configuration ordinateur\Stratégies\Paramètres Windows\Paramètres de Sécurité\stratégies**, puis cliquez sur **attribution des droits utilisateur**.  

    ![Groupes et comptes d’administrateur local sécurisé](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_103.png)  

7.  Configurez les droits d’utilisateur pour empêcher le compte administrateur local d’accéder à des serveurs membres et stations de travail sur le réseau en procédant comme suit:  

    1.  Double-cliquez sur **refuser l’accès à cet ordinateur à partir du réseau** et sélectionnez **définir ces paramètres de stratégie**.  

    2.  Cliquez sur **ajouter un utilisateur ou groupe**, tapez le nom d’utilisateur du compte d’administrateur local, puis cliquez sur **OK**. Ce nom d’utilisateur sera **administrateur**, la valeur par défaut lorsque Windows est installé.  

        ![Groupes et comptes d’administrateur local sécurisé](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_104.png)  

    3.  Cliquez sur OK.  

        > [!IMPORTANT]  
        > Lorsque vous ajoutez le compte d’administrateur pour ces paramètres, vous spécifiez si vous configurez un compte d’administrateur local ou un compte d’administrateur de domaine par la façon dont vous étiquetez les comptes. Par exemple, pour ajouter le compte d’administrateur du domaine TAILSPINTOYS à ces refuser des droits, vous devez sélectionner le compte d’administrateur pour le domaine TAILSPINTOYS, qui apparaît en tant que TAILSPINTOYS\Administrator. Si vous tapez **administrateur** dans ces paramètres de droits d’utilisateur dans l’éditeur d’objets de stratégie de groupe, vous allez restreindre le compte administrateur local sur chaque ordinateur sur lequel l’objet de stratégie de groupe est appliquée, comme décrit précédemment.  

8.  Configurez les droits d’utilisateur pour empêcher le compte d’administrateur local à partir de l’ouverture de session en tant que traitement par lots en procédant comme suit:  

    1.  Double-cliquez sur **refuse la connexion en tant que traitement par lots** et sélectionnez **définir ces paramètres de stratégie**.  

    2.  Cliquez sur **ajouter un utilisateur ou groupe**, tapez le nom d’utilisateur du compte d’administrateur local, puis cliquez sur **OK**. Ce nom d’utilisateur sera **administrateur**, la valeur par défaut lorsque Windows est installé.  

        ![Groupes et comptes d’administrateur local sécurisé](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_105.png)  

    3.  Cliquez sur **OK**.  

        > [!IMPORTANT]  
        > Lorsque vous ajoutez le compte d’administrateur pour ces paramètres, vous spécifiez si vous configurez compte administrateur local ou le compte d’administrateur de domaine par la façon dont vous étiquetez les comptes. Par exemple, pour ajouter le compte d’administrateur du domaine TAILSPINTOYS à ces refuser des droits, vous devez sélectionner le compte d’administrateur pour le domaine TAILSPINTOYS, qui apparaît en tant que TAILSPINTOYS\Administrator. Si vous tapez **administrateur** dans ces paramètres de droits d’utilisateur dans l’éditeur d’objets de stratégie de groupe, vous allez restreindre le compte administrateur local sur chaque ordinateur sur lequel l’objet de stratégie de groupe est appliquée, comme décrit précédemment.  

9. Configurez les droits d’utilisateur pour empêcher le compte d’administrateur local à partir de l’ouverture de session en tant que service en effectuant les opérations suivantes:  

    1.  Double-cliquez sur **refuse la connexion en tant que service** et sélectionnez **définir ces paramètres de stratégie**.  

    2.  Cliquez sur **ajouter un utilisateur ou groupe**, tapez le nom d’utilisateur du compte d’administrateur local, puis cliquez sur **OK**. Ce nom d’utilisateur sera **administrateur**, la valeur par défaut lorsque Windows est installé.  

        ![Groupes et comptes d’administrateur local sécurisé](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_106.png)  

    3.  Cliquez sur **OK**.  

        > [!IMPORTANT]  
        > Lorsque vous ajoutez le compte d’administrateur pour ces paramètres, vous spécifiez si vous configurez compte administrateur local ou le compte d’administrateur de domaine par la façon dont vous étiquetez les comptes. Par exemple, pour ajouter le compte d’administrateur du domaine TAILSPINTOYS à ces refuser des droits, vous devez sélectionner le compte d’administrateur pour le domaine TAILSPINTOYS, qui apparaît en tant que TAILSPINTOYS\Administrator. Si vous tapez **administrateur** dans ces paramètres de droits d’utilisateur dans l’éditeur d’objets de stratégie de groupe, vous allez restreindre le compte administrateur local sur chaque ordinateur sur lequel l’objet de stratégie de groupe est appliquée, comme décrit précédemment.  

10. Configurez les droits d’utilisateur pour empêcher le compte administrateur local d’accéder à des serveurs membres et les stations de travail via les Services Bureau à distance en effectuant les opérations suivantes:  

    1.  Double-cliquez sur **refuse la connexion par le biais des Services Bureau à distance** et sélectionnez **définir ces paramètres de stratégie**.  

    2.  Cliquez sur **ajouter un utilisateur ou groupe**, tapez le nom d’utilisateur du compte d’administrateur local, puis cliquez sur **OK**. Ce nom d’utilisateur sera **administrateur**, la valeur par défaut lorsque Windows est installé.  

        ![Groupes et comptes d’administrateur local sécurisé](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_107.png)  

    3.  Cliquez sur **OK**.  

        > [!IMPORTANT]  
        > Lorsque vous ajoutez le compte d’administrateur pour ces paramètres, vous spécifiez si vous configurez compte administrateur local ou le compte d’administrateur de domaine par la façon dont vous étiquetez les comptes. Par exemple, pour ajouter le compte d’administrateur du domaine TAILSPINTOYS à ces refuser des droits, vous devez sélectionner le compte d’administrateur pour le domaine TAILSPINTOYS, qui apparaît en tant que TAILSPINTOYS\Administrator. Si vous tapez **administrateur** dans ces paramètres de droits d’utilisateur dans l’éditeur d’objets de stratégie de groupe, vous allez restreindre le compte administrateur local sur chaque ordinateur sur lequel l’objet de stratégie de groupe est appliquée, comme décrit précédemment.  

11. Pour quitter **éditeur de gestion de stratégie de groupe**, cliquez sur **fichier**, puis cliquez sur **quitter**.  

12. Dans **gestion des stratégies de groupe**, liez l’objet de stratégie de groupe pour les serveurs membres et d’une station de travail unités d’organisation en procédant comme suit:  

    1.  Accédez à la <Forest>\Domains\\<Domain> (où <Forest>est le nom de la forêt et <Domain>est le nom du domaine dans lequel vous souhaitez définir la stratégie de groupe).  

    2.  Avec le bouton droit de l’unité d’organisation que l’objet de stratégie de groupe seront appliqués au et cliquez sur **lier un objet de stratégie de groupe existant**.  

        ![Groupes et comptes d’administrateur local sécurisé](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_108.png)  

    3.  Sélectionnez l’objet de stratégie de groupe que vous avez créé et cliquez sur **OK**.  

        ![Groupes et comptes d’administrateur local sécurisé](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_109.png)  

    4.  Créer des liens vers tous les autres unités d’organisation qui contiennent des stations de travail.  

    5.  Créer des liens vers tous les autres unités d’organisation qui contiennent des serveurs membres.  

#### <a name="verification-steps"></a>Étapes de vérification  

##### <a name="verify-deny-access-to-this-computer-from-the-network-gpo-settings"></a>Vérifier les paramètres de stratégie de groupe «Refuser l’accès à cet ordinateur à partir du réseau»  

À partir de n’importe quel serveur membre ou une station de travail qui n’est pas affectée par les modifications de stratégie de groupe (par exemple, un serveur de renvoi), essayez d’accéder à un serveur membre ou une station de travail sur le réseau qui est affecté par les modifications de stratégie de groupe. Pour vérifier les paramètres de stratégie de groupe, tenter de mapper le lecteur du système à l’aide de la **NET USE** commande.  

1.  Ouvrez une session localement sur tout serveur membre ou d’une station de travail qui n’est pas affectée par les modifications de stratégie de groupe.  

2.  Avec la souris, déplacez le pointeur dans le coin supérieur droit ou inférieur droit de l’écran. Lorsque le **icônes** barre s’affiche, cliquez sur **recherche**.  

3.  Dans le **recherche**, tapez **invite de commandes**, avec le bouton droit **invite de commandes**, puis cliquez sur **exécuter en tant qu’administrateur** pour ouvrir une invite de commandes avec élévation de privilèges.  

4.  Lorsque vous êtes invité à approuver l’élévation, cliquez sur **Oui**.  

    ![Groupes et comptes d’administrateur local sécurisé](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_110.png)  

5.  Dans le **invite de commandes** fenêtre, tapez **commande net use \\\<Server Name>\c$ /user:<Server Name>\Administrator**, où <Server Name>est le nom du serveur membre ou de station de travail que vous tentez d’accéder via le réseau.  

    > [!NOTE]  
    > Les informations d’identification d’administrateur locales doivent être dans le même système que vous tentez d’accéder via le réseau.  

6.  La capture d’écran suivante affiche le message d’erreur qui doit apparaître.  

    ![Groupes et comptes d’administrateur local sécurisé](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_111.png)  

##### <a name="verify-deny-log-on-as-a-batch-job-gpo-settings"></a>Vérifier les paramètres de stratégie de groupe «Refuse la connexion en tant que tâche»  
À partir de n’importe quel serveur membre ou une station de travail affectées par les modifications de stratégie de groupe, ouvrez une session localement.  

###### <a name="create-a-batch-file"></a>Créer un fichier de commandes  

1.  Avec la souris, déplacez le pointeur dans le coin supérieur droit ou inférieur droit de l’écran. Lorsque le **icônes** barre s’affiche, cliquez sur **recherche**.  

2.  Dans le **recherche**, tapez **le bloc-notes**, puis cliquez sur **le bloc-notes**.  

3.  Dans **le bloc-notes**, type **dir c:**.  

4.  Cliquez sur **fichier**, puis cliquez sur **Enregistrer sous**.  

5.  Dans le **nom de fichier**, tapez **<Filename>.bat** (où <Filename>est le nom du nouveau fichier de commandes).  

###### <a name="schedule-a-task"></a>Planifier une tâche  

1.  Avec la souris, déplacez le pointeur dans le coin supérieur droit ou inférieur droit de l’écran. Lorsque le **icônes** barre s’affiche, cliquez sur **recherche**.  

2.  Dans le **recherche**, tapez le Planificateur de tâches et cliquez sur **le Planificateur de tâches**.  

    > [!NOTE]  
    > Sur les ordinateurs exécutant Windows 8, dans le **recherche** , tapez **planifier des tâches**, puis cliquez sur **planifier des tâches**.  

3.  Cliquez sur **Action**, puis cliquez sur **créer une tâche**.  

4.  Dans le **créer une tâche** boîte de dialogue, tapez ** <Task Name> ** (où <Task Name> est le nom de la nouvelle tâche).  

5.  Cliquez sur le **Actions** onglet, puis cliquez sur **New**.  

6.  Dans le **Action** , cliquez sur **démarrer un programme**.  

7.  Dans le **programme ou le script** , cliquez sur **Parcourir**, recherchez et sélectionnez le fichier de commandes créé dans le **créer un fichier de commandes** section, puis cliquez sur **ouvrir**.  

8.  Cliquez sur **OK**.  

9. Cliquez sur le **général** onglet.  

10. Dans le **options de sécurité** , cliquez sur **modifier un utilisateur ou groupe**.  

11. Tapez le nom du compte d’administrateur local du système, cliquez sur **vérifier les noms**, puis cliquez sur **OK**.  

12. Sélectionnez **exécuter même si l’utilisateur est connecté ou non** et **ne stockent pas de mot de passe**. La tâche n’aura accès aux ressources de l’ordinateur local.  

13. Cliquez sur **OK**.  

14. Une boîte de dialogue doit apparaître, compte d’utilisateur demande des informations d’identification pour exécuter la tâche.  

15. Après avoir entré les informations d’identification, cliquez sur **OK**.  

16. Une boîte de dialogue semblable à la suivante doit apparaître.  

    ![Groupes et comptes d’administrateur local sécurisé](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_112.png)  

###### <a name="verify-deny-log-on-as-a-service-gpo-settings"></a>Vérifier les paramètres de stratégie de groupe «Refuse la connexion en tant que service»  

1.  À partir de n’importe quel serveur membre ou une station de travail affectées par les modifications de stratégie de groupe, ouvrez une session localement.  

2.  Avec la souris, déplacez le pointeur dans le coin supérieur droit ou inférieur droit de l’écran. Lorsque le **icônes** barre s’affiche, cliquez sur **recherche**.  

3.  Dans le **recherche** , tapez **services**, puis cliquez sur **Services**.  

4.  Recherchez et double-cliquez sur **spouleur d’impression**.  

5.  Cliquez sur le **ouverture de session** onglet.  

6.  Dans **ouvrir une session en tant que** , cliquez sur **ce compte**.  

7.  Cliquez sur **Parcourir**, tapez le compte d’administrateur local du système, cliquez sur **vérifier les noms**, puis cliquez sur **OK**.  

8.  Dans le **mot de passe** et **confirmer le mot de passe** champs, tapez le mot de passe du compte sélectionné, puis cliquez sur **OK**.  

9. Cliquez sur **OK** trois fois de plus.  

10. Avec le bouton droit **spouleur d’impression** et cliquez sur **redémarrer**.  

11. Lorsque le service est redémarré, une boîte de dialogue semblable au suivant doit s’afficher.  

    ![Groupes et comptes d’administrateur local sécurisé](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_113.png)  

###### <a name="revert-changes-to-the-printer-spooler-service"></a>Annuler les modifications pour le Service Spouleur d’imprimante  

1.  À partir de n’importe quel serveur membre ou une station de travail affectées par les modifications de stratégie de groupe, ouvrez une session localement.  

2.  Avec la souris, déplacez le pointeur dans le coin supérieur droit ou inférieur droit de l’écran. Lorsque le **icônes** barre s’affiche, cliquez sur **recherche**.  

3.  Dans le **recherche** , tapez **services**, puis cliquez sur **Services**.  

4.  Recherchez et double-cliquez sur **spouleur d’impression**.  

5.  Cliquez sur le **ouverture de session** onglet.  

6.  Dans le **ouvrir une session en tant que**: champ, sélectionnez **Systemaccount Local**, puis cliquez sur **OK**.  

###### <a name="verify-deny-log-on-through-remote-desktop-services-gpo-settings"></a>Vérifier les paramètres de stratégie de groupe «Refuse la connexion par le biais des Services Bureau à distance»  

1.  Avec la souris, déplacez le pointeur dans le coin supérieur droit ou inférieur droit de l’écran. Lorsque le **icônes** barre s’affiche, cliquez sur **recherche**.  

2.  Dans le **recherche** , tapez **connexion Bureau à distance**, puis cliquez sur **connexion Bureau à distance**.  

3.  Dans le **ordinateur** , tapez le nom de l’ordinateur que vous souhaitez vous connecter à, puis cliquez sur **Connect**. (Vous pouvez également taper l’adresse IP au lieu du nom d’ordinateur).  

4.  Lorsque vous y êtes invité, fournissez les informations d’identification pour le système local de **administrateur** compte.  

5.  Une boîte de dialogue semblable à la suivante doit apparaître.  

    ![Groupes et comptes d’administrateur local sécurisé](media/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups/SAD_114.png)  
