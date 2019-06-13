---
ms.assetid: 82918181-525d-4e93-af96-957dac6aedb6
title: Configuration de l’annexe B de l’environnement de Test
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 3ebe125ce7850797d786e7b564c98889cfb19927
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66445858"
---
# <a name="appendix-b-setting-up-the-test-environment"></a>Annexe B : configuration de l'environnement de test

>S'applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Cette rubrique décrit les étapes nécessaires pour créer un laboratoire de test du contrôle d'accès dynamique. Vous devez suivre les instructions dans l'ordre indiqué car de nombreux composants ont des dépendances.  

## <a name="prerequisites"></a>Prérequis  
**Configuration matérielle et logicielle requise**  

Configuration requise pour la mise en place du laboratoire de test :  

-   un serveur hôte exécutant Windows Server 2008 R2 avec SP1 et Hyper-V ;  

-   Une copie de l’image ISO de Windows Server 2012  

-   Une copie de l’image ISO de Windows 8  

-   Microsoft Office 2010 ;  

-   un serveur exécutant Microsoft Exchange Server 2003 ou version ultérieure.  

Vous devez créer les ordinateurs virtuels suivants pour tester les scénarios de contrôle d'accès dynamique :  

-   DC1 (contrôleur de domaine) ;  

-   DC2 (contrôleur de domaine) ;  

-   FILE1 (serveur de fichiers et services AD RMS (Active Directory Rights Management Services)) ;  

-   SRV1 (serveur POP3 et SMTP) ;  

-   CLIENT1 (ordinateur client avec Microsoft Outlook).  

Les mots de passe des ordinateurs virtuels doivent être les suivants :  

-   BUILTIN\Administrator : pass@word1  

-   Contoso\administrator : pass@word1  

-   Tous les autres comptes : pass@word1  

## <a name="build-the-test-lab-virtual-machines"></a>Créer les ordinateurs virtuels du laboratoire de test  

### <a name="install-the-hyper-v-role"></a>Installer le rôle Hyper-V  
Vous devez installer le rôle Hyper-V sur un ordinateur Windows Server 2008 R2 avec SP1.  

##### <a name="to-install-the-hyper-v-role"></a>Pour installer le rôle Hyper-V  

1.  Cliquez sur **Démarrer**, puis sur Gestionnaire de serveur.  

2.  Dans la zone Résumé des rôles de la fenêtre principale du Gestionnaire de serveur, cliquez sur **Ajouter des rôles**.  

3.  Dans la page **Sélectionner des rôles de serveurs** , cliquez sur **Hyper-V**.  

4.  Dans la page **Créer des réseaux virtuels** , cliquez sur une ou plusieurs cartes réseau si vous souhaitez que leur connexion réseau soit accessible aux ordinateurs virtuels.  

5.  Dans la page **Confirmer les sélections pour l’installation** , cliquez sur **Installer**.  

6.  Vous devez redémarrer l'ordinateur pour terminer l'installation. Cliquez sur **Fermer** pour terminer l'Assistant, puis sur **Oui** pour redémarrer l'ordinateur.  

7.  Après avoir redémarré l'ordinateur, ouvrez une session avec le même compte que celui utilisé pour installer le rôle. Une fois que l'Assistant Reprise de la configuration a terminé l'installation, cliquez sur **Fermer** pour terminer l'Assistant.  

### <a name="create-an-internal-virtual-network"></a>Créer un réseau virtuel interne  
Vous allez maintenant créer un réseau virtuel interne nommé ID_AD_Network.  

##### <a name="to-create-a-virtual-network"></a>Pour créer un réseau virtuel  

1.  Ouvrez le Gestionnaire Hyper-V.  

2.  Dans le menu **Actions** , cliquez sur **Gestionnaire de réseau virtuel**.  

3.  Sous **Créer un réseau virtuel**, sélectionnez **Interne**.  

4.  Cliquez sur **Ajouter**. La page **Nouveau réseau virtuel** s'affiche à l'écran.  

5.  Tapez **ID_AD_Network** comme nom du nouveau réseau. Examinez les autres propriétés et modifiez-les si nécessaire.  

6.  Cliquez sur **OK** pour créer le réseau virtuel et fermer le Gestionnaire de réseau virtuel, ou cliquez sur **Appliquer** pour créer le réseau virtuel et continuer à utiliser le Gestionnaire de réseau virtuel.  

### <a name="BKMK_Build"></a>Créer le contrôleur de domaine  
Créez un ordinateur virtuel qui servira de contrôleur de domaine (DC1). Installez la machine virtuelle à l’aide de la norme ISO de Windows Server 2012 et nommez-le DC1.  

##### <a name="to-install-active-directory-domain-services"></a>Pour installer les services de domaine Active Directory  

1. Connectez l'ordinateur virtuel au réseau ID_AD_Network. Connectez-vous à DC1 en tant qu’administrateur avec le mot de passe <strong>pass@word1</strong>.  

2. Dans le Gestionnaire de serveur, cliquez sur **Gérer**, puis sur **Ajouter des rôles et des fonctionnalités**.  

3. Dans la page **Avant de commencer** , cliquez sur **Suivant**.  

4. Dans la page **Sélectionner le type d'installation**, cliquez sur **Installation basée sur un rôle ou une fonctionnalité**, puis sur **Suivant**.  

5. Dans la page **Sélectionner le serveur de destination**, cliquez sur **Suivant**.  

6. Dans la page **Sélectionner des rôles de serveurs** , cliquez sur **Services de domaine Active Directory**. Dans la boîte de dialogue **Assistant Ajout de rôles et de fonctionnalités**, cliquez sur **Ajouter des fonctionnalités**, puis sur **Suivant**.  

7. Dans la page **Sélectionner les fonctionnalités** , cliquez sur **Suivant**.  

8. Dans la page **Services de domaine Active Directory**, examinez les informations, puis cliquez sur **Suivant**.  

9. Dans la page **Confirmer les sélections d’installation** , cliquez sur **Installer**. La barre de progression Installation de fonctionnalité de la page Résultats indique que le rôle est en cours d'installation.  

10. Dans la page **Résultats** , vérifiez que l'installation a réussi, puis cliquez sur **Fermer**. Dans le Gestionnaire de serveur, cliquez sur l'icône d'avertissement avec un point d'exclamation qui se trouve en haut à droite de l'écran, à côté de **Gérer**. Dans la liste Tâches, cliquez sur le lien **Promouvoir ce serveur en contrôleur de domaine** .  

11. Dans la page **Configuration du déploiement**, cliquez sur **Ajouter une nouvelle forêt**, tapez le nom du domaine racine, **contoso.com**, puis cliquez sur **Suivant**.  

12. Sur le **Options du contrôleur de domaine** , sélectionnez les niveaux fonctionnels de domaine et de forêt en tant que Windows Server 2012, spécifiez le mot de passe DSRM <strong>pass@word1</strong>, puis cliquez sur **suivant**.  

13. Dans la page **Options DNS**, cliquez sur **Suivant**.  

14. Dans la page **Options supplémentaires**, cliquez sur **Suivant**.  

15. Dans la page **Chemins d'accès**, tapez les emplacements de la base de données Active Directory, des fichiers journaux et du dossier SYSVOL (ou acceptez les emplacements par défaut), puis cliquez sur **Suivant**.  

16. Dans la page **Examiner les options**, confirmez vos sélections, puis cliquez sur **Suivant**.  

17. Dans la page **Vérification de la configuration requise**, confirmez que la validation de la configuration requise est terminée, puis cliquez sur **Installer**.  

18. Dans la page **Résultats**, vérifiez que le serveur a été configuré correctement comme contrôleur de domaine, puis cliquez sur **Fermer**.  

19. Redémarrez le serveur pour terminer l'installation des services AD DS. (Par défaut, le redémarrage est automatique.)  

Créez les utilisateurs suivants à l'aide du Centre d'administration Active Directory.  

##### <a name="create-users-and-groups-on-dc1"></a>Créer des utilisateurs et des groupes sur DC1  

1. Connectez-vous à contoso.com en tant qu'Administrateur. Ouvrez le Centre d'administration Active Directory.  

2. Créez les groupes de sécurité suivants :  


   |    Nom du groupe    |        Adresse de messagerie         |
   |------------------|------------------------------|
   |   FinanceAdmin   |   financeadmin@contoso.com   |
   | FinanceException | financeexception@contoso.com |


3. Créez l'unité d'organisation suivante :  


   |   Nom de l'unité d'organisation    | Computers |
   |--------------|-----------|
   | FileServerOU |   FILE1   |


4. Créez les utilisateurs suivants avec les attributs indiqués :  


   |       Utilisateur       |  Nom d'utilisateur  |     Adresse électronique      | Service |      Regrouper       | Pays/région |
   |------------------|------------|------------------------|------------|------------------|----------------|
   | Myriam Delesalle | MDelesalle | MDelesalle@contoso.com |  Finance   |                  |       US       |
   |    Miles Reid    |   MReid    |   MReid@contoso.com    |  Finance   |   FinanceAdmin   |       US       |
   |   Esther Valle   |   EValle   |   EValle@contoso.com   | Opérations | FinanceException |       US       |
   |   Maira Wenzel   |  MWenzel   |  MWenzel@contoso.com   |     HR     |                  |       US       |
   |     Jeff Low     |    JLow    |    JLow@contoso.com    |     HR     |                  |       US       |
   |    Serveur RMS    |    rms     |    rms@contoso.com     |            |                  |                |

   Pour plus d’informations sur la création des groupes de sécurité, voir [Créer un groupe](https://technet.microsoft.com/library/dd861305.aspx) sur le site web Windows Server.  

##### <a name="to-create-a-group-policy-object"></a>Pour créer un objet de stratégie de groupe  

1.  Placez le curseur dans l'angle supérieur droit de l'écran et cliquez sur l'icône de recherche. Dans la zone Rechercher, tapez **gestion des stratégies de groupe**, puis cliquez sur **Gestion des stratégies de groupe**.  

2.  Développez **Forêt : contoso.com**, puis **Domaines**, accédez à **contoso.com**, développez **(contoso.com)** , puis sélectionnez **FileServerOU**. Avec le bouton droit **créer un objet GPO dans ce domaine et le lier ici**

3.  Tapez un nom descriptif pour l'objet de stratégie de groupe, tel que **GPOAccèsFlexible**, puis cliquez sur **OK**.  

##### <a name="to-enable-dynamic-access-control-for-contosocom"></a>Pour activer le contrôle d'accès dynamique pour contoso.com  

1.  Ouvrez la console de gestion des stratégies de groupe, cliquez sur **contoso.com**, puis double-cliquez sur **Contrôleurs de domaine**.  

2.  Cliquez avec le bouton droit sur **Stratégie des contrôleurs de domaine par défaut** et sélectionnez **Modifier**.  

3.  Dans la fenêtre Éditeur de gestion des stratégies de groupe, double-cliquez sur **Configuration ordinateur**, **Stratégies**, **Modèles d'administration**, **Système**, puis **Contrôleur de domaine Kerberos**.  

4.  Double-cliquez sur **Prise en charge des revendications, de l'authentification composée et du blindage Kerberos** et sélectionnez l'option à côté de **Activé**. Vous devez activer ce paramètre pour utiliser les stratégies d'accès centralisées.  

5.  Ouvrez une invite de commandes avec élévation de privilèges et exécutez la commande suivante :  

    ```  
    gpupdate /force  
    ```  

### <a name="BKMK_FS1"></a>Créer le serveur de fichiers et le serveur AD RMS (FILE1)  

1. Créer un ordinateur virtuel nommé FILE1 à partir de l’image ISO de Windows Server 2012.  

2. Connectez l'ordinateur virtuel au réseau ID_AD_Network.  

3. Joindre la machine virtuelle au domaine contoso.com, puis connectez-vous à FILE1 en tant que Contoso\Administrateur avec le mot de passe <strong>pass@word1</strong>.  

#### <a name="install-file-services-resource-manager"></a>Installer le Gestionnaire de ressources du serveur de fichiers  

###### <a name="to-install-the-file-services-role-and-the-file-server-resource-manager"></a>Pour installer le rôle Services de fichiers et le Gestionnaire de ressources du serveur de fichiers  

1.  Dans le Gestionnaire de serveur, cliquez sur **Ajouter des rôles et fonctionnalités**.  

2.  Dans la page **Avant de commencer** , cliquez sur **Suivant**.  

3.  Dans la page **Sélectionner le type d'installation**, cliquez sur **Suivant**.  

4.  Dans la page **Sélectionner le serveur de destination**, cliquez sur **Suivant**.  

5.  Dans la page **Sélectionner des rôles de serveurs** , développez **Services de fichiers et de stockage**, cochez la case en regard de **Services de fichiers et iSCSI**, développez, puis sélectionnez **Gestionnaire de ressources du serveur de fichiers**.  

    Dans l'Assistant Ajout de rôles et de fonctionnalités, cliquez sur **Ajouter des fonctionnalités**, puis sur **Suivant**.  

6.  Dans la page **Sélectionner les fonctionnalités** , cliquez sur **Suivant**.  

7.  Dans la page **Confirmer les sélections d’installation** , cliquez sur **Installer**.  

8.  Dans la page **Progression de l'installation** , cliquez sur **Fermer**.  

#### <a name="install-the-microsoft-office-filter-packs-on-the-file-server"></a>Installer les Filter Packs de Microsoft Office sur le serveur de fichiers  
Vous devez installer les Filter Packs de Microsoft Office sur Windows Server 2012 pour activer les IFilters pour une plus large gamme de fichiers Office que ceux fournis par défaut.  Windows Server 2012 n’a pas les IFilters pour les fichiers Microsoft Office installés par défaut, et l’infrastructure de classification de fichiers utilise les IFilters pour effectuer une analyse de contenu.  

Pour télécharger et installer les IFilters, voir [Filter Packs de Microsoft Office 2010](https://go.microsoft.com/fwlink/?LinkID=234122).  

#### <a name="configure-email-notifications-on-file1"></a>Configurer les notifications par courrier électronique sur FILE1  
Quand vous créez des quotas et des filtres de fichiers, vous pouvez envoyer des notifications par courrier électronique aux utilisateurs quand leur limite de quota approche ou quand ils ont essayé d'enregistrer des fichiers qui ont été bloqués. Si vous souhaitez informer certains administrateurs des événements liés aux quotas et aux filtres de fichiers, vous pouvez configurer un ou plusieurs destinataires par défaut. Pour envoyer ces notifications, vous devez spécifier le serveur SMTP à utiliser pour le transfert des messages électroniques.  

###### <a name="to-configure-email-options-in-file-server-resource-manager"></a>Pour configurer les options de messagerie dans le Gestionnaire de ressources du serveur de fichiers  

1. Ouvrez le Gestionnaire de ressources du serveur de fichiers. Pour ouvrir le Gestionnaire de ressources du serveur de fichiers, cliquez sur **Démarrer**, tapez **gestionnaire de ressources du serveur de fichiers**, puis cliquez sur **Gestionnaire de ressources du serveur de fichiers**.  

2. Dans l'interface du Gestionnaire de ressources du serveur de fichiers, cliquez avec le bouton droit sur **Gestionnaire de ressources du serveur de fichiers**, puis cliquez sur **Configurer les options**. La boîte de dialogue **Options du Gestionnaire de ressources du serveur de fichiers** s'affiche.  

3. Sous l'onglet **Notifications par courrier électronique** , sous Nom ou adresse IP du serveur SMTP, tapez le nom d'hôte ou l'adresse IP du serveur SMTP qui transfèrera les notifications par courrier électronique.  

4. Si vous souhaitez informer certains administrateurs de quota régulièrement ou d’événements de filtrage, de fichiers sous **Administrateurs destinataires par défaut**, tapez chaque adresse de messagerie comme fileadmin@contoso.com. Utilisez le format account@domainet utilisez des points-virgules pour séparer les comptes.  

#### <a name="create-groups-on-file1"></a>Créer des groupes sur FILE1  

###### <a name="to-create-security-groups-on-file1"></a>Pour créer des groupes de sécurité sur FILE1  

1. Connectez-vous à FILE1 en tant que Contoso\Administrateur avec le mot de passe : <strong>pass@word1</strong>.  

2. Ajoutez NT AUTHORITY\Utilisateurs authentifiés au groupe **WinRMRemoteWMIUsers__** .  

#### <a name="create-files-and-folders-on-file1"></a>Créer des fichiers et des dossiers sur FILE1  

1.  Créez un volume NTFS sur FILE1, puis créez le dossier suivant : D:\Documents financiers.  

2.  Créez les fichiers suivants avec les détails indiqués :  

    -   **Finance Memo.docx** : Ajoutez du texte de nature financière dans le document. Par exemple, « les règles d’entreprise sur qui peut accéder aux documents financiers ont changé. Ces documents sont désormais accessibles uniquement aux membres du groupe FinanceExpert. Aucun autre service ou groupes n’ont accès. » Vous devez évaluer l'impact de ce changement avant de l'implémenter dans l'environnement. Assurez-vous que ce document comporte la mention CONFIDENTIEL CONTOSO dans le pied de page de chaque page.  

    -   **Request for Approval to Hire.docx** : créez un formulaire dans ce document pour recueillir les informations relatives aux postulants à un emploi. Ce document doit comporter les champs suivants : **Nom du postulant, Numéro de sécurité sociale, Emploi, Salaire proposé, Date de début, Nom du responsable, Service**. Ajoutez une section supplémentaire au document qui comporte un formulaire pour **Signature du responsable, Salaire approuvé, Confirmation de l'offre**et **Statut de l'offre**.   
        Activez le document pour la gestion des droits.  

    -   **Word Document1.docx** : ajoutez du contenu test à ce document.  

    -   **Word Document2.docx** : ajoutez du contenu test à ce document.  

    -   **Workbook1.xlsx**  

    -   **Workbook2.xlsx**  

    -   Créez un dossier sur le Bureau nommé Expressions régulières. Créez un document texte sous le dossier nommé **RegEx-SSN**. Tapez le contenu suivant dans le fichier, puis enregistrez-le et fermez-le :   
        ^(?!000)([0-7]\d{2}|7([0-7]\d|7[012]))([ -]?)(?!00)\d\d\3(?!0000)\d{4}$  

3.  Partagez le dossier D:\Documents financiers comme Documents financiers et autorisez tout le monde à accéder à ce partage en lecture et en écriture.  

> [!NOTE]  
> Les stratégies d'accès centralisées ne sont pas activées par défaut sur le volume système ou de démarrage C:.  

#### <a name="BKMK_CS1"></a>Installer Active Directory Rights Management Services  
Ajoutez les services AD RMS et toutes les fonctionnalités nécessaires à l'aide du Gestionnaire de serveur. Choisissez tous les paramètres par défaut.  

###### <a name="to-install-active-directory-rights-management-services"></a>Pour installer les Services AD RMS  

1. Connectez-vous à FILE1 en tant que CONTOSO\Administrateur ou membre du groupe Admins du domaine.  

   > [!IMPORTANT]  
   > Pour installer le rôle serveur AD RMS, le compte d'installation (ici, CONTOSO\Administrateur) doit être membre du groupe Administrateurs local sur l'ordinateur serveur où les services AD RMS doivent être installés et du groupe Administrateurs de l'entreprise dans Active Directory.  

2. Dans le Gestionnaire de serveur, cliquez sur **Ajouter des rôles et fonctionnalités**. L’Assistant Ajout de rôles et de fonctionnalités apparaît.  

3. Dans l'écran **Avant de commencer** , cliquez sur **Suivant**.  

4. Dans l'écran **Sélectionner le type d'installation**, cliquez sur **Installation basée sur un rôle ou une fonctionnalité**, puis sur **Suivant**.  

5. Dans l'écran **Sélectionner les serveurs cibles**, cliquez sur **Suivant**.  

6. Dans l'écran **Sélectionner des rôles de serveurs** , cochez la case **Services AD RMS (Active Directory Rights Management Services)** , puis cliquez sur **Suivant**.  

7. Dans la boîte de dialogue **Ajouter les fonctionnalités requises pour Services AD RMS (Active Directory Rights Management Services) ?** , cliquez sur **Ajouter des fonctionnalités**.  

8. Dans l'écran **Sélectionner des rôles de serveurs**, cliquez sur **Suivant**.  

9. Dans l'écran **Sélectionner les fonctionnalités à installer**, cliquez sur **Suivant**.  

10. Dans l'écran **Services AD RMS (Active Directory Rights Management Services)** , cliquez sur Suivant.  

11. Dans l'écran **Sélectionner des services de rôle** , cliquez sur **Suivant**.  

12. Dans l’écran **Serveur Web (IIS)** , cliquez sur **Suivant**.  

13. Dans l'écran **Sélectionner des services de rôle** , cliquez sur **Suivant**.  

14. Dans l'écran **Confirmer les sélections d'installation** , cliquez sur **Installer**.  

15. Une fois l'installation terminée, dans l'écran **Progression de l'installation**, cliquez sur **Effectuer une configuration supplémentaire**. L'Assistant Configuration des services AD RMS s'affiche.  

16. Dans l'écran **AD RMS**, cliquez sur **Suivant**.  

17. Dans l'écran **Cluster AD RMS**, sélectionnez **Créer un nouveau cluster racine AD RMS**, puis cliquez sur **Suivant**.  

18. Dans l'écran **Base de données de configuration** , cliquez sur **Utiliser la base de données interne de Windows sur ce serveur**, puis cliquez sur **Suivant**.  

    > [!NOTE]  
    > L'utilisation de la base de données interne de Windows est recommandée pour les environnements de test uniquement, car elle ne prend pas en charge plus d'un serveur dans le cluster AD RMS. Les déploiements de production doivent utiliser un serveur de bases de données distinct.  

19. Sur le **compte de Service** écran dans **compte d’utilisateur de domaine**, cliquez sur **spécifier** , puis spécifiez le nom d’utilisateur (**contoso\rms**), et Mot de passe (<strong>pass@word1</strong>) et cliquez sur **OK**, puis cliquez sur **suivant**.  

20. Dans l'écran **Mode de chiffrement**, cliquez sur **Mode de chiffrement 2**.  

21. Dans l'écran **Stockage de clé de cluster** , cliquez sur **Suivant**.  

22. Sur le **mot de passe de clé Cluster** écran, dans le **mot de passe** et **confirmer le mot de passe** cases, tapez <strong>pass@word1</strong>, puis cliquez sur **Suivant**.  

23. Dans l'écran **Site Web de cluster** , vérifiez que **Site Web par défaut** est sélectionné, puis cliquez sur **Suivant**.  

24. Dans l'écran **Adresse du cluster**, sélectionnez l'option **Utiliser une connexion non chiffrée**, dans la zone **Nom de domaine complet**, tapez **FILE1.contoso.com**, puis cliquez sur **Suivant**.  

25. Dans l'écran **Nom du certificat de licence**, acceptez nom par défaut (**FILE1**) dans la zone de texte et cliquez sur **Suivant**.  

26. Dans l'écran **Inscription du SCP**, sélectionnez **Inscrire SCP**, puis cliquez sur **Suivant**.  

27. Dans l'écran **Confirmation**, cliquez sur **Installer**.  

28. Dans l'écran **Résultats**, cliquez sur **Fermer**, puis cliquez sur **Fermer** dans l'écran **Progression de l'installation**. Lorsque vous avez terminé, déconnectez-vous et connectez-vous en tant que contoso\rms avec le mot de passe fourni (<strong>pass@word1</strong>).  

29. Démarrez la console AD RMS et accédez à **Modèles de stratégies de droits**.  

    Pour ouvrir la console AD RMS, dans le Gestionnaire de serveur, cliquez sur **Serveur local** dans l'arborescence de la console, cliquez sur **Outils**, puis sur **Services AD RMS (Active Directory Rights Management Services)** .  

30. Cliquez sur **Créer un modèle de stratégie de droits distribué** dans le panneau de droite, cliquez sur **Ajouter**et sélectionnez les informations suivantes :  

    -   Language (Langue) : Anglais (US)  

    -   Nom : Contoso Finance Admin Only  

    -   Description : Contoso Finance Admin Only  

    Cliquez sur **Ajouter**, puis sur **Suivant**.  

31. Dans la section utilisateurs et droits, cliquez sur **utilisateurs et droits**, cliquez sur **ajouter**, type <strong>financeadmin@contoso.com</strong>, puis cliquez sur **OK**.  

32. Sélectionnez **Contrôle total** et laissez l'option **Octroyer le contrôle total au propriétaire (auteur) sans date d'expiration** sélectionnée.  

33. Parcourez les autres onglets sans rien changer, puis cliquez sur **Terminer**. Connectez-vous en tant que CONTOSO\Administrateur.  

34. Accédez au dossier, C:\inetpub\wwwroot\\_wmcs\certification, sélectionnez le fichier ServerCertification.asmx et ajouter des utilisateurs authentifiés ont autorisations lecture et écriture au fichier.  

35. Ouvrez Windows PowerShell et exécutez `Get-FsrmRmsTemplate`. Vérifiez que vous pouvez voir le modèle RMS créé à l'étape précédente de cette procédure avec cette commande.  

> [!IMPORTANT]  
> Si vous voulez que vos serveurs changent immédiatement pour pouvoir les tester, procédez comme suit :  
>   
> 1.  Sur le serveur de fichiers, FILE1, ouvrez une invite de commandes avec élévation de privilèges et exécutez les commandes suivantes :  
>   
>     -   gpupdate /force.  
>     -   NLTEST /SC_RESET:contoso.com  
> 2.  Sur le contrôleur de domaine (DC1), répliquez Active Directory.  
>   
>     Pour plus d’informations sur les étapes nécessaires pour forcer la réplication d’Active Directory, voir [Réplication Active Directory](https://technet.microsoft.com/library/cc794809(WS.10).aspx)  

Si vous le souhaitez, au lieu d'utiliser l'Assistant Ajout de rôles et de fonctionnalités dans le Gestionnaire de serveur, vous pouvez utiliser Windows PowerShell pour installer et configurer le rôle serveur AD RMS comme indiqué dans la procédure ci-dessous.  

###### <a name="to-install-and-configure-an-ad-rms-cluster-in-windows-server-2012-using-windows-powershell"></a>Pour installer et configurer un cluster AD RMS dans Windows Server 2012 à l'aide de Windows PowerShell  

1. Ouvrez une session en tant que Contoso\Administrateur avec le mot de passe : <strong>pass@word1</strong>.  

   > [!IMPORTANT]  
   > Pour installer le rôle serveur AD RMS, le compte d'installation (ici, CONTOSO\Administrateur) doit être membre du groupe Administrateurs local sur l'ordinateur serveur où les services AD RMS doivent être installés et du groupe Administrateurs de l'entreprise dans Active Directory.  

2. Sur le Bureau du serveur, cliquez avec le bouton droit sur l'icône Windows PowerShell dans la barre des tâches et sélectionnez **Exécuter en tant qu'administrateur** pour ouvrir une invite Windows PowerShell avec des privilèges d'administration.  

3. Pour utiliser les applets de commande du Gestionnaire de serveur pour installer le rôle serveur AD RMS, tapez :  

   ```  
   Add-WindowsFeature ADRMS '"IncludeAllSubFeature '"IncludeManagementTools  
   ```  

4. Créez le lecteur Windows PowerShell qui représente le serveur AD RMS que vous installez.  

   Par exemple, pour créer un lecteur Windows PowerShell nommé RC pour installer et configurer le premier serveur d'un cluster racine AD RMS, tapez :  

   ```  
   Import-Module ADRMS  
   New-PSDrive -PSProvider ADRMSInstall -Name RC -Root RootCluster  
   ```  

5. Définissez les propriétés sur les objets dans l'espace de noms du lecteur qui représentent les paramètres de configuration requis.  

   Par exemple, pour définir le compte de service AD RMS, à l'invite de commandes Windows PowerShell, tapez :  

   ```  
   $svcacct = Get-Credential  
   ```  

   Quand la boîte de dialogue de sécurité Windows apparaît, tapez le nom d'utilisateur de domaine du compte de service AD RMS CONTOSO\RMS et le mot de passe assigné.  

   Ensuite, pour assigner le compte de service AD RMS aux paramètres de cluster AD RMS, tapez ce qui suit :  

   ```  
   Set-ItemProperty -Path RC:\ -Name ServiceAccount -Value $svcacct  
   ```  

   Ensuite, pour configurer le serveur AD RMS pour utiliser la base de données interne Windows, à l'invite de commandes Windows PowerShell, tapez :  

   ```  
   Set-ItemProperty -Path RC:\ClusterDatabase -Name UseWindowsInternalDatabase -Value $true  
   ```  

   Ensuite, pour stocker de manière sécurisée le mot de passe de clé de cluster dans une variable, à l'invite de commandes Windows PowerShell, tapez :  

   ```  
   $password = Read-Host -AsSecureString -Prompt "Password:"  
   ```  

   Tapez le mot de passe de clé de cluster, puis appuyez sur la touche Entrée.  

   Ensuite, pour assigner le mot de passe à votre installation AD RMS, à l'invite de commandes Windows PowerShell, tapez :  

   ```  
   Set-ItemProperty -Path RC:\ClusterKey -Name CentrallyManagedPassword -Value $password  
   ```  

   Ensuite, pour définir l'adresse du cluster AD RMS, à l'invite de commandes Windows PowerShell, tapez :  

   ```  
   Set-ItemProperty -Path RC:\ -Name ClusterURL -Value "http://file1.contoso.com:80"  
   ```  

   Ensuite, pour assigner le nom SLC de votre installation AD RMS, à l'invite de commandes Windows PowerShell, tapez :  

   ```  
   Set-ItemProperty -Path RC:\ -Name SLCName -Value "FILE1"  
   ```  

   Ensuite, pour définir le point de connexion de service (SCP) pour le cluster AD RMS, à l'invite de commandes Windows PowerShell, tapez :  

   ```  
   Set-ItemProperty -Path RC:\ -Name RegisterSCP -Value $true  
   ```  

6. Exécutez l'applet de commande **Install-ADRMS**. En plus d'installer le rôle serveur AD RMS et de configurer le serveur, cette applet de commande installe si nécessaire d'autres fonctionnalités requises par les services AD RMS.  

   Par exemple, pour basculer vers le lecteur Windows PowerShell nommé RC et installer et configurer les services AD RMS, tapez :  

   ```  
   Set-Location RC:\  
   Install-ADRMS -Path.  
   ```  

   Tapez « O » quand l'applet de commande vous invite à confirmer que vous souhaitez démarrer l'installation.  

7. Fermeture de session en tant que Contoso\Administrateur et le journal sur tant que CONTOSO\RMS avec le mot de passe fourni («pass@word1»).  

   > [!IMPORTANT]  
   > Pour que vous puissiez gérer le serveur AD RMS, le compte sous lequel vous êtes connecté et que vous utilisez pour gérer le serveur (ici, CONTOSO\RMS) doit être membre du groupe Administrateurs local sur l'ordinateur serveur AD RMS et du groupe Administrateurs de l'entreprise dans Active Directory.  

8. Sur le Bureau du serveur, cliquez avec le bouton droit sur l'icône Windows PowerShell dans la barre des tâches et sélectionnez **Exécuter en tant qu'administrateur** pour ouvrir une invite Windows PowerShell avec des privilèges d'administration.  

9. Créez le lecteur Windows PowerShell qui représente le serveur AD RMS que vous configurez.  

    Par exemple, pour créer un lecteur Windows PowerShell nommé RC pour configurer le cluster racine AD RMS, tapez :  

    ```  
    Import-Module ADRMSAdmin `  
    New-PSDrive -PSProvider ADRMSAdmin -Name RC -Root http://localhost -Force -Scope Global  
    ```  

10. Pour créer un modèle de droits pour l'administrateur du service Finance de Contoso et lui assigner des droits de l'utilisateur avec un contrôle total dans votre installation AD RMS, à l'invite de commandes Windows PowerShell, tapez :  

    ```  
    New-Item -Path RC:\RightsPolicyTemplate '"LocaleName en-us -DisplayName "Contoso Finance Admin Only" -Description "Contoso Finance Admin Only" -UserGroup financeadmin@contoso.com  -Right ('FullControl')  
    ```  

11. Pour vérifier que vous pouvez voir le nouveau modèle de droits pour l'administrateur du service Finance de Contoso, à l'invite de commandes Windows PowerShell, tapez :  

    ```  
    Get-FsrmRmsTemplate  
    ```  

    Examinez la sortie de cette applet de commande pour confirmer que le modèle RMS que vous avez créé à l'étape précédente est présent.  

### <a name="build-the-mail-server-srv1"></a>Créer le serveur de messagerie (SRV1)  
SRV1 est le serveur de messagerie SMTP/POP3. Vous devez le configurer pour pouvoir envoyer des notifications par courrier électronique dans le cadre du scénario d'assistance en cas d'accès refusé.  

Configurez Microsoft Exchange Server sur cet ordinateur. Pour plus d’informations, voir [Comment installer Exchange Server](https://go.microsoft.com/fwlink/?prd=12364).  

### <a name="build-the-client-virtual-machine-client1"></a>Créer l'ordinateur virtuel client (CLIENT1)  

##### <a name="to-build-the-client-virtual-machine"></a>Pour créer l'ordinateur virtuel client  

1. Connectez CLIENT1 au réseau ID_AD_Network.  

2. Installez Microsoft Office 2010.  

3. Connectez-vous en tant que Contoso\Administrateur et utilisez les informations suivantes pour configurer Microsoft Outlook.  

   - Votre nom : Administrateur de fichiers  

   - Adresse de messagerie : fileadmin@contoso.com  

   - Type de compte : POP3  

   - Serveur de courrier entrant : Adresse IP statique de SRV1  

   - Serveur de courrier sortant : Adresse IP statique de SRV1  

   - Nom d’utilisateur : fileadmin@contoso.com  

   - Mémoriser le mot de passe : Sélectionner  

4. Créez un raccourci vers Outlook sur le Bureau de contoso\administrateur.  

5. Ouvrez Outlook et répondez de tous les messages « premier démarrage ».  

6. Supprimez les messages tests qui ont été générés.  

7. Créer un nouveau raccourci sur le bureau pour tous les utilisateurs sur la machine virtuelle cliente qui pointe vers \\\FILE1\Finance Documents.  

8. Redémarrez l'ordinateur si nécessaire.  

##### <a name="enable-access-denied-assistance-on-the-client-virtual-machine"></a>Activer l'assistance en cas d'accès refusé sur l'ordinateur client virtuel  

1.  Ouvrez l'Éditeur du Registre et accédez à **HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows\Explorer**.  

    -   Affectez à **EnableShellExecuteFileStreamCheck** la valeur **1**.  

    -   Valeur : DWORD  

## <a name="BKMK_CF"></a>Configuration du laboratoire pour le déploiement de revendications dans le scénario de forêts  

### <a name="BKMK_2.1"></a>Créer une machine virtuelle pour DC2  

-   Créer une machine virtuelle à partir de l’image ISO de Windows Server 2012.  

-   Créez un ordinateur virtuel nommé DC2.  

-   Connectez l'ordinateur virtuel au réseau ID_AD_Network.  

> [!IMPORTANT]  
> Pour que vous puissiez joindre des ordinateurs virtuels à un domaine et déployer des types de revendications interforêts, il faut que les ordinateurs virtuels puissent résoudre les noms de domaine complets des domaines en question. Vous devrez peut-être pour cela configurer manuellement les paramètres DNS sur les ordinateurs virtuels. Pour plus d’informations, voir [Configuration d’un réseau virtuel](https://technet.microsoft.com/library/cc732470%28v=ws.10%29.aspx).  
>   
> Toutes les images des ordinateurs virtuels (serveurs et clients) doivent être reconfigurées pour utiliser une adresse IP statique de version 4 (IPv4) et des paramètres clients DNS (Domain Name System). Pour plus d’informations, voir [Configurer un client DNS pour des adresses IP statiques](https://go.microsoft.com/fwlink/?LinkId=150952).  

### <a name="BKMK_2.2"></a>Configurer une nouvelle forêt nommée adatum.com  

##### <a name="to-install-active-directory-domain-services"></a>Pour installer les services de domaine Active Directory  

1. Connectez l'ordinateur virtuel au réseau ID_AD_Network. Connectez-vous à DC2 en tant qu’administrateur avec le mot de passe <strong>Pass@word1</strong>.  

2. Dans le Gestionnaire de serveur, cliquez sur **Gérer**, puis sur **Ajouter des rôles et des fonctionnalités**.  

3. Dans la page **Avant de commencer** , cliquez sur **Suivant**.  

4. Dans la page **Sélectionner le type d'installation**, cliquez sur **Installation basée sur un rôle ou une fonctionnalité**, puis sur **Suivant**.  

5. Dans la page **Sélectionner le serveur de destination** , cliquez sur **Sélectionner un serveur du pool de serveurs**, cliquez sur le nom du serveur où vous voulez installer les services AD DS, puis cliquez sur **Suivant**.  

6. Dans la page **Sélectionner des rôles de serveurs** , cliquez sur **Services de domaine Active Directory**. Dans la boîte de dialogue **Assistant Ajout de rôles et de fonctionnalités**, cliquez sur **Ajouter des fonctionnalités**, puis sur **Suivant**.  

7. Dans la page **Sélectionner des fonctionnalités**, cliquez sur **Suivant**.  

8. Dans la page **AD DS** , examinez les informations, puis cliquez sur **Suivant**.  

9. Dans la page **Confirmation**, cliquez sur **Installer**. La barre de progression Installation de fonctionnalité de la page Résultats indique que le rôle est en cours d'installation.  

10. Dans la page **Résultats**, vérifiez que l'installation a réussi, puis cliquez sur l'icône d'avertissement avec un point d'exclamation qui se trouve en haut à droite de l'écran, à côté de **Gérer**. Dans la liste Tâches, cliquez sur le lien **Promouvoir ce serveur en contrôleur de domaine** .  

    > [!IMPORTANT]  
    > Si vous fermez l'Assistant Installation à ce stade au lieu de cliquer sur **Promouvoir ce serveur en contrôleur de domaine**, vous pouvez continuer l'installation des services AD DS en cliquant sur **Tâches** dans le Gestionnaire de serveur.  

11. Dans la page **Configuration du déploiement**, cliquez sur **Ajouter une nouvelle forêt**, tapez le nom du domaine racine, **adatum.com**, puis cliquez sur **Suivant**.  

12. Sur le **Options du contrôleur de domaine** , sélectionnez les niveaux fonctionnels de domaine et de forêt en tant que Windows Server 2012, spécifiez le mot de passe DSRM <strong>pass@word1</strong>, puis cliquez sur **suivant**.  

13. Dans la page **Options DNS**, cliquez sur **Suivant**.  

14. Dans la page **Options supplémentaires**, cliquez sur **Suivant**.  

15. Dans la page **Chemins d'accès**, tapez les emplacements de la base de données Active Directory, des fichiers journaux et du dossier SYSVOL (ou acceptez les emplacements par défaut), puis cliquez sur **Suivant**.  

16. Dans la page **Examiner les options**, confirmez vos sélections, puis cliquez sur **Suivant**.  

17. Dans la page **Vérification de la configuration requise**, confirmez que la validation de la configuration requise est terminée, puis cliquez sur **Installer**.  

18. Dans la page **Résultats**, vérifiez que le serveur a été configuré correctement comme contrôleur de domaine, puis cliquez sur **Fermer**.  

19. Redémarrez le serveur pour terminer l'installation des services AD DS. (Par défaut, le redémarrage est automatique.)  

> [!IMPORTANT]  
> Pour vous assurer que le réseau est configuré correctement, après avoir configuré les deux forêts, vous devez procéder comme suit :  
>   
> -   Connectez-vous à adatum.com en tant qu'adatum\administrateur. Ouvrez une fenêtre Invite de commandes, tapez **nslookup contoso.com**, puis appuyez sur Entrée.  
> -   Connectez-vous à contoso.com en tant que contoso\administrateur. Ouvrez une fenêtre Invite de commandes, tapez **nslookup adatum.com**, puis appuyez sur Entrée.  
>   
> Si ces commandes s'exécutent sans erreur, cela signifie que les forêts peuvent communiquer. Pour plus d’informations sur les erreurs nslookup, voir la section Dépannage de la rubrique [Utilisation de NSlookup.exe](https://support.microsoft.com/kb/200525)  

### <a name="BKMK_2.22"></a>Définir contoso.com comme forêt adatum.com  
Lors de cette étape, vous allez créer une relation d'approbation entre le site d'Adatum Corporation et le site de Contoso, Ltd.  

##### <a name="to-set-contoso-as-a-trusting-forest-to-adatum"></a>Pour définir Contoso comme forêt approuvée par Adatum  

1.  Connectez-vous à DC2 en tant qu'administrateur. Dans l'écran d' **accueil** , tapez domain.msc.  

2.  Dans l'arborescence de la console, cliquez avec le bouton droit sur adatum.com, puis cliquez sur Propriétés.  

3.  Sous l’onglet **Approbations** , cliquez sur **Nouvelle approbation**, puis sur **Suivant**.  

4.  Dans la page **Nom d'approbation** , tapez **contoso.com**dans le champ de nom DNS (Domain Name System), puis cliquez sur **Suivant**.  

5.  Dans la page **Type d'approbation** , cliquez sur **Approbation de la forêt**, puis sur **Suivant**.  

6.  Dans la page **Direction de l'approbation**, cliquez sur **Bidirectionnelle**.  

7.  Dans la page **Sens de l'approbation**, cliquez sur **À la fois ce domaine et le domaine spécifié**, puis sur **Suivant**.  

8.  Continuez à suivre les instructions fournies dans l’Assistant.  

### <a name="BKMK_2.4"></a>Créer des utilisateurs supplémentaires dans la forêt Adatum  
Créez l’utilisateur Jeff Low avec le mot de passe <strong>pass@word1</strong>et assignez l’attribut de société avec la valeur **Adatum**.  

##### <a name="to-create-a-user-with-the-company-attribute"></a>Pour créer un utilisateur avec l'attribut Company  

1.  Ouvrez une invite de commandes avec élévation de privilèges dans Windows PowerShell et collez le code suivant :  

    ```  
    New-ADUser `  
    -SamAccountName jlow `  
    -Name "Jeff Low" `  
    -UserPrincipalName jlow@adatum.com `  
    -AccountPassword (ConvertTo-SecureString `  
    -AsPlainText "pass@word1" -Force) `  
    -Enabled $true `  
    -PasswordNeverExpires $true `  
    -Path 'CN=Users,DC=adatum,DC=com' `  
    -Company Adatum`  

    ```  

### <a name="BKMK_2.5"></a>Créer le type de revendication Company sur adatum.com  

##### <a name="to-create-a-claim-type-by-using-windows-powershell"></a>Pour créer un type de revendication à l'aide de Windows PowerShell  

1.  Connectez-vous à adatum.com en tant qu'administrateur.  

2.  Ouvrez une invite de commandes avec élévation de privilèges dans Windows PowerShell et tapez le code suivant :  

    ```  
    New-ADClaimType `  
    -AppliesToClasses:@('user') `  
    -Description:"Company" `  
    -DisplayName:"Company" `  
    -ID:"ad://ext/Company:ContosoAdatum" `  
    -IsSingleValued:$true `  
    -Server:"adatum.com" `  
    -SourceAttribute:Company `  
    -SuggestedValues:@((New-Object Microsoft.ActiveDirectory.Management.ADSuggestedValueEntry("Contoso", "Contoso", "")), (New-Object Microsoft.ActiveDirectory.Management.ADSuggestedValueEntry("Adatum", "Adatum", ""))) `  

    ```  

### <a name="BKMK_2.55"></a>Activer la propriété de ressource Company sur contoso.com  

##### <a name="to-enable-the-company-resource-property-on-contosocom"></a>Pour activer la propriété de ressource Company sur contoso.com  

1.  Connectez-vous à contoso.com en tant qu'administrateur.  

2.  Dans le Gestionnaire de serveur, cliquez sur **Outils**, puis sur **Centre d'administration Active Directory**.  

3.  Dans le volet gauche du Centre d'administration Active Directory, cliquez sur **Arborescence**. Dans le volet gauche, cliquez sur **Contrôle d'accès dynamique**, puis double-cliquez sur **Propriétés de ressource**.  

4.  Sélectionnez **Company** dans la liste **Propriétés de ressource** , cliquez avec le bouton droit et sélectionnez **Propriétés**. Dans la section **Valeurs suggérées**, cliquez sur **Ajouter** pour ajouter les valeurs suggérées : Contoso et Adatum, puis cliquez deux fois sur **OK**.  

5.  Sélectionnez **Company** dans la liste **Propriétés de ressource** , cliquez avec le bouton droit et sélectionnez **Activer**.  

### <a name="BKMK_2.6"></a>Activer le contrôle d’accès dynamique sur adatum.com  

##### <a name="to-enable-dynamic-access-control-for-adatumcom"></a>Pour activer le contrôle d'accès dynamique pour adatum.com  

1.  Connectez-vous à adatum.com en tant qu'administrateur.  

2.  Ouvrez la console de gestion des stratégies de groupe, cliquez sur **adatum.com**, puis double-cliquez sur **Contrôleurs de domaine**.  

3.  Cliquez avec le bouton droit sur **Stratégie des contrôleurs de domaine par défaut** et sélectionnez **Modifier**.  

4.  Dans la fenêtre Éditeur de gestion des stratégies de groupe, double-cliquez sur **Configuration ordinateur**, **Stratégies**, **Modèles d'administration**, **Système**, puis **Contrôleur de domaine Kerberos**.  

5.  Double-cliquez sur **Prise en charge des revendications, de l'authentification composée et du blindage Kerberos** et sélectionnez l'option à côté de **Activé**. Vous devez activer ce paramètre pour utiliser les stratégies d'accès centralisées.  

6.  Ouvrez une invite de commandes avec élévation de privilèges et exécutez la commande suivante :  

    ```  
    gpupdate /force  
    ```  

### <a name="BKMK_2.8"></a>Créer le type de revendication Company sur contoso.com  

##### <a name="to-create-a-claim-type-by-using-windows-powershell"></a>Pour créer un type de revendication à l'aide de Windows PowerShell  

1.  Connectez-vous à contoso.com en tant qu'administrateur.  

2.  Ouvrez une invite de commandes avec élévation de privilèges dans Windows PowerShell et tapez le code suivant :  

    ```  
    New-ADClaimType '"SourceTransformPolicy `  
    '"DisplayName 'Company' `  
    '"ID 'ad://ext/Company:ContosoAdatum' `  
    '"IsSingleValued $true `  
    '"ValueType 'string' `  

    ```  

### <a name="BKMK_2.9"></a>Créer la règle d’accès centralisée  

##### <a name="to-create-a-central-access-rule"></a>Pour créer une règle d'accès central  

1. Dans le volet gauche du Centre d'administration Active Directory, cliquez sur **Arborescence**. Dans le volet gauche, cliquez sur **Contrôle d'accès dynamique**, puis sur **Règles d'accès central**.  

2. Cliquez avec le bouton droit sur **Règles d'accès central**, cliquez sur **Nouveau**, puis sur **Règle d'accès central**.  

3. Dans le champ **Nom** , tapez **RègleAccèsEmployésAdatum**.  

4. Dans la section **Autorisations** , sélectionnez l'option **Utiliser les autorisations suivantes en tant qu'autorisations actuelles** , cliquez sur **Modifier**, puis sur **Ajouter**. Cliquez sur le lien **Sélectionnez un principal** , tapez **Utilisateurs authentifiés**, puis cliquez sur **OK**.  

5. Dans la boîte de dialogue **Autorisations pour Autorisations**, cliquez sur **Ajouter une condition** et entrez les conditions suivantes : [**Utilisateur**] [**Société**] [**Égal**] [**Valeur**] [**Adatum**]. Les autorisations doivent être **Modification, Lecture et exécution, Lecture, Écriture**.  

6. Cliquez sur **OK**.  

7. Cliquez sur **OK** à trois reprises pour terminer et revenir au Centre d'administration Active Directory.  

   ![guides de solutions](media/Appendix-B--Setting-Up-the-Test-Environment/PowerShellLogoSmall.gif)***<em>commandes Windows PowerShell équivalentes</em>***  

   L'applet ou les applets de commande Windows PowerShell suivantes remplissent la même fonction que la procédure précédente. Entrez chaque applet de commande sur une seule ligne, même si elles peuvent apparaître comme renvoyées sur plusieurs lignes ici en raison de contraintes de mise en forme.  

   ```  
   New-ADCentralAccessRule `  
   -CurrentAcl:"O:SYG:SYD:AR(A;;FA;;;OW)(A;;FA;;;BA)(A;;FA;;;SY)(XA;;0x1301bf;;;AU;(@USER.ad://ext/Company:ContosoAdatum == `"Adatum`"))" `  
   -Name:"AdatumEmployeeAccessRule" `  
   -ProposedAcl:$null `  
   -ProtectedFromAccidentalDeletion:$true `  
   -Server:"contoso.com" `  
   ```  

### <a name="BKMK_2.10"></a>Créer la stratégie d’accès centralisée  

##### <a name="to-create-a-central-access-policy"></a>Pour créer une stratégie d'accès centralisée  

1.  Connectez-vous à contoso.com en tant qu'administrateur.  

2.  Ouvrez une invite de commandes avec élévation de privilèges dans Windows PowerShell et collez le code suivant :  

    ```  
    New-ADCentralAccessPolicy "Adatum Only Access Policy"   
    Add-ADCentralAccessPolicyMember "Adatum Only Access Policy" `  
    -Member "AdatumEmployeeAccessRule" `  
    ```  

### <a name="BKMK_2.11"></a>Publier la nouvelle stratégie via la stratégie de groupe  

##### <a name="to-apply-the-central-access-policy-across-file-servers-through-group-policy"></a>Pour appliquer la stratégie d'accès centralisée sur les serveurs de fichiers à l'aide de la stratégie de groupe  

1.  Dans l’écran **Démarrer**, tapez **Outils d’administration**, puis dans la barre **Rechercher**, cliquez sur **Paramètres**. Dans les résultats **Paramètres** , cliquez sur **Outils d’administration**. Ouvrez la console de gestion des stratégies de groupe à partir du dossier **Outils d'administration**.  

    > [!TIP]  
    > Si le paramètre **Afficher les outils d’administration** est désactivé, le dossier Outils d’administration et son contenu ne figurent pas dans les résultats **Paramètres** .  

2.  Cliquez sur le domaine contoso.com, cliquez sur **créer un objet GPO dans ce domaine et le lier ici**  

3.  Tapez un nom descriptif pour l'objet de stratégie de groupe, tel que **GPOAccèsAdatum**, puis cliquez sur **OK**.  

##### <a name="to-apply-the-central-access-policy-to-the-file-server-through-group-policy"></a>Pour appliquer la stratégie d'accès centralisée au serveur de fichiers à l'aide de la stratégie de groupe  

1.  Dans l'écran d' **accueil** , tapez **Gestion des stratégies de groupe**dans la zone **Rechercher** . Ouvrez **Gestion des stratégies de groupe** à partir du dossier Outils d'administration.  

    > [!TIP]  
    > Si le paramètre **Afficher les outils d’administration** est désactivé, le dossier Outils d’administration et son contenu ne figurent pas dans les résultats Paramètres.  

2.  Accédez à Contoso et sélectionnez-le comme suit : Gestion des stratégies de groupe\Forêt : contoso.com\Domaines, contoso.com.  

3.  Cliquez avec le bouton droit sur la stratégie **GPOAccèsAdatum** et sélectionnez **Modifier**.  

4.  Dans l'Éditeur de gestion des stratégies de groupe, cliquez sur **Configuration ordinateur**, développez **Stratégies**, **Paramètres Windows**, puis cliquez sur **Paramètres de sécurité**.  

5.  Développez **Système de fichiers**, cliquez avec le bouton droit sur **Stratégie d'accès centralisée**, puis cliquez sur **Gérer les stratégies d'accès centralisées**.  

6.  Dans la boîte de dialogue **Configuration des stratégies d'accès centralisées**, cliquez sur **Ajouter**, sélectionnez **Stratégie d'accès Adatum uniquement**, puis cliquez sur **OK**.  

7.  Fermez l’éditeur de gestion des stratégies de groupe. Vous avez maintenant ajouté la stratégie d'accès centralisée à la stratégie de groupe.  

### <a name="BKMK_2.12"></a>Créer le dossier Earnings sur le serveur de fichiers  
Créez un volume NTFS sur FILE1 et créez le dossier suivant : D:\Earnings.  

> [!NOTE]  
> Les stratégies d'accès centralisées ne sont pas activées par défaut sur le volume système ou de démarrage C:.  

### <a name="BKMK_2.13"></a>Définir la classification et appliquer la stratégie d’accès centralisée sur le dossier Earnings  

##### <a name="to-assign-the-central-access-policy-on-the-file-server"></a>Pour assigner la stratégie d'accès centralisée sur le serveur de fichiers  

1. Dans le Gestionnaire Hyper-V, connectez-vous au serveur FILE1. Connectez-vous au serveur à l’aide de compte contoso\administrateur, avec le mot de passe <strong>pass@word1</strong>.  

2. Ouvrez une invite de commandes avec élévation de privilèges et tapez **gpupdate /force**. Cette commande permet de s'assurer que les modifications apportées à la stratégie de groupe prennent effet sur votre serveur.  

3. Vous devez aussi actualiser les propriétés de ressource globales à partir d'Active Directory. Ouvrez Windows PowerShell, tapez `Update-FSRMClassificationpropertyDefinition`, puis appuyez sur ENTRÉE. Fermez Windows PowerShell.  

4. Ouvrez l'Explorateur Windows et accédez à D:\EARNINGS. Cliquez avec le bouton droit sur le dossier **Earnings**, puis cliquez sur **Propriétés**.  

5. Cliquez sur l'onglet **Classification**. Sélectionnez Company, puis sélectionnez **Adatum** dans le champ **Valeur**.  

6. Cliquez sur **Modifier**, sélectionnez **Stratégie d'accès Adatum uniquement** dans le menu déroulant, puis cliquez sur **Appliquer**.  

7. Cliquez sur l'onglet **Sécurité** , sur **Avancé**, puis sur l'onglet **Stratégie centralisée** . La règle **RègleAccèsEmployésAdatum** doit figurer dans la liste. Vous pouvez développer l'élément pour afficher toutes les autorisations que vous avez définies lors de la création de la règle dans Active Directory.  

8. Cliquez sur **OK** pour revenir à Windows Explorer.  



