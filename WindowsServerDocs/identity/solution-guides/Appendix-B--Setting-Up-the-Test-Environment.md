---
ms.assetid: 82918181-525d-4e93-af96-957dac6aedb6
title: "Annexe B configuration de l’environnement de Test"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: deb08b0663e5f349df7cce51ddabd4aae7f624c5
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="appendix-b-setting-up-the-test-environment"></a>Annexe b: configuration de l’environnement de Test

>S’applique à: Windows Server2016, Windows Server2012R2, Windows Server2012

Cette rubrique décrit les étapes permettant de créer un laboratoire pour tester le contrôle d’accès dynamique. Les instructions sont destinées à l’ordre indiqué car de nombreux composants qui ont des dépendances.  
  
## <a name="prerequisites"></a>Conditions préalables  
**Configuration matérielle et logicielle requise**  
  
Configuration requise pour la configuration du laboratoire de test:  
  
-   Un serveur hôte exécutant Windows Server 2008 R2 avec SP1 et Hyper-V  
  
-   Une copie de l’image ISO de Windows Server 2012  
  
-   Une copie de l’image ISO de Windows 8  
  
-   Microsoft Office 2010  
  
-   Un serveur exécutant Microsoft Exchange Server 2003 ou version ultérieure  
  
Vous avez besoin pour créer les ordinateurs virtuels suivants pour tester les scénarios de contrôle d’accès dynamique:  
  
-   DC1 (contrôleur de domaine)  
  
-   DC2 (contrôleur de domaine)  
  
-   File1 (serveur de fichiers et Active Directory Rights Management Services)  
  
-   SRV1 (serveur POP3 et SMTP)  
  
-   CLIENT1 (ordinateur client avec Microsoft Outlook)  
  
Les mots de passe pour les ordinateurs virtuels doivent se présenter comme suit:  
  
-   BUILTIN\Administrator:pass@word1  
  
-   Contoso\administrator:pass@word1  
  
-   Tous les autres comptes:pass@word1  
  
## <a name="build-the-test-lab-virtual-machines"></a>Générer le test virtuels du laboratoire  
  
### <a name="install-the-hyper-v-role"></a>Installer le rôle Hyper-V  
Vous devez installer le rôle Hyper-V sur un ordinateur exécutant Windows Server 2008 R2 avec SP1.  
  
##### <a name="to-install-the-hyper-v-role"></a>Pour installer le rôle Hyper-V  
  
1.  Cliquez sur **Démarrer**, puis cliquez sur Gestionnaire de serveur.  
  
2.  Dans la zone Résumé des rôles de la fenêtre principale du Gestionnaire de serveur, cliquez sur **ajouter des rôles**.  
  
3.  Sur le **sélectionner des rôles de serveur** , cliquez sur **Hyper-V**.  
  
4.  Sur le **créer des réseaux virtuels** , cliquez sur une ou plusieurs cartes réseau si vous souhaitez rendre leur connexion réseau disponible pour les ordinateurs virtuels.  
  
5.  Sur le **confirmer les sélections d’Installation** , cliquez sur **installer**.  
  
6.  L’ordinateur doit être redémarré pour terminer l’installation. Cliquez sur **fermer** pour terminer l’Assistant, puis cliquez sur **Oui** à redémarrer l’ordinateur.  
  
7.  Après avoir redémarré l’ordinateur, connectez-vous avec le même compte que vous avez utilisé pour installer le rôle. Une fois l’Assistant reprise de la Configuration a terminé l’installation, cliquez sur **fermer** pour terminer l’Assistant.  
  
### <a name="create-an-internal-virtual-network"></a>Créer un réseau virtuel interne  
Vous allez maintenant créer un réseau virtuel interne nommé ID_AD_Network.  
  
##### <a name="to-create-a-virtual-network"></a>Pour créer un réseau virtuel  
  
1.  Ouvrez le Gestionnaire Hyper-V.  
  
2.  À partir de la **Actions** menu, cliquez sur **Gestionnaire de réseau virtuel**.  
  
3.  Sous **créer un réseau virtuel**, sélectionnez le **interne**.  
  
4.  Cliquez sur **ajouter**. Le **nouveau réseau virtuel** page s’affiche.  
  
5.  Type **ID_AD_Network** comme nom pour le nouveau réseau. Passez en revue les autres propriétés et modifiez-les si nécessaire.  
  
6.  Cliquez sur **OK** pour créer le réseau virtuel et fermer le Gestionnaire de réseau virtuel ou cliquez sur **appliquer** pour créer le réseau virtuel et continuer à l’aide du Gestionnaire de réseau virtuel.  
  
### <a name="BKMK_Build"></a>Créer le contrôleur de domaine  
Créer un ordinateur virtuel à utiliser en tant que contrôleur de domaine (DC1). Installez l’ordinateur virtuel à l’aide de Windows Server 2012 ISO et nommez-le DC1.  
  
##### <a name="to-install-active-directory-domain-services"></a>Pour installer les Services de domaine Active Directory  
  
1.  Se connecter à l’ordinateur virtuel au réseau ID_AD_Network. Connectez-vous à DC1 en tant qu’administrateur avec le mot de passe ** pass@word1 **.  
  
2.  Dans le Gestionnaire de serveur, cliquez sur **gérer**, puis cliquez sur **Ajout de rôles et fonctionnalités**.  
  
3.  Sur le **avant de commencer** , cliquez sur **suivant**.  
  
4.  Sur le **sélectionner le type d’installation** , cliquez sur **installer un rôle ou une fonctionnalité**, puis cliquez sur **suivant**.  
  
5.  Sur le **serveur de destination sélectionnez** , cliquez sur **suivant**.  
  
6.  Sur le **sélectionner des rôles de serveur** , cliquez sur **les Services de domaine Active Directory**. Dans le **Assistant Ajouter des rôles et fonctionnalités** boîte de dialogue, cliquez sur **ajouter des fonctionnalités**, puis cliquez sur **suivant**.  
  
7.  Sur le **sélectionner des fonctionnalités** , cliquez sur **suivant**.  
  
8.  Sur le **les Services de domaine Active Directory** page, passez en revue les informations, puis cliquez sur **suivant**.  
  
9. Sur le **confirmer les sélections d’installation** , cliquez sur **installer**. La barre de progression installation fonctionnalité sur la page résultats indique que le rôle est installé.  
  
10. Sur le **résultats** , vérifiez que l’installation a réussi, puis cliquez sur **fermer**. Dans le Gestionnaire de serveur, cliquez sur l’icône d’avertissement avec un point d’exclamation sur le coin supérieur droit de l’écran, en regard **gérer**. Dans la liste des tâches, cliquez sur le **promouvoir ce serveur en contrôleur de domaine** lien.  
  
11. Sur le **Configuration de déploiement** , cliquez sur **ajouter une nouvelle forêt**, tapez le nom du domaine racine, **contoso.com**, puis cliquez sur **suivant**.  
  
12. Sur le **Options du contrôleur de domaine** page, sélectionnez les niveaux fonctionnels de domaine et de forêt Windows Server 2012, spécifiez le mot de passe DSRM ** pass@word1 **, puis cliquez sur **suivant**.  
  
13. Sur le **Options DNS** , cliquez sur **suivant**.  
  
14. Sur le **Options supplémentaires** , cliquez sur **suivant**.  
  
15. Sur le **chemins d’accès** page, tapez les emplacements de la base de données Active Directory, le fichiers journaux et le dossier SYSVOL (ou acceptez les emplacements par défaut), puis cliquez sur **suivant**.  
  
16. Sur le **examiner les Options** page, confirmez vos sélections, puis cliquez sur **suivant**.  
  
17. Sur le **vérification des conditions requises** page, confirmez que la validation des conditions préalables terminée, puis cliquez sur **installer**.  
  
18. Sur le **résultats** page, vérifiez que le serveur a été correctement configuré comme un contrôleur de domaine, puis cliquez sur **fermer**.  
  
19. Redémarrez le serveur pour terminer l’installation des services AD DS. (Par défaut, cela se produit automatiquement.)  
  
Créez les utilisateurs suivants à l’aide du centre d’administration Active Directory.  
  
##### <a name="create-users-and-groups-on-dc1"></a>Créer des utilisateurs et des groupes sur DC1  
  
1.  Connectez-vous à contoso.com en tant qu’administrateur. Lancer le centre d’administration Active Directory.  
  
2.  Créer des groupes de sécurité suivants:  
  
    |Nom du groupe|Adresse de messagerie|  
    |--------------|-----------------|  
    |FinanceAdmin|financeadmin@contoso.com|  
    |FinanceException|financeexception@contoso.com|  
  
3.  Créer l’unité d’organisation suivante (UO):  
  
    |Nom de l’unité d’organisation|Ordinateurs|  
    |-----------|-------------|  
    |FileServerOU|FILE1|  
  
4.  Créez les utilisateurs suivants avec les attributs indiqués:  
  
    |Utilisateur|Nom d’utilisateur|Adresse de messagerie|Service|Groupe|Pays/région|  
    |--------|------------|-----------------|--------------|---------|-------------------|  
    |Myriam Delesalle|MDelesalle|MDelesalle@contoso.com|Finance||NOUS|  
    |Miles Reid|MReid|MReid@contoso.com|Finance|FinanceAdmin|NOUS|  
    |Esther Valle|EValle|EValle@contoso.com|Opérations|FinanceException|NOUS|  
    |Maira Wenzel|MWenzel|MWenzel@contoso.com|RESSOURCES HUMAINES||NOUS|  
    |Jeff Low|JLow|JLow@contoso.com|RESSOURCES HUMAINES||NOUS|  
    |Serveur RMS|RMS|rms@contoso.com||||  
  
    Pour plus d’informations sur la création de groupes de sécurité, voir [créer un nouveau groupe](https://technet.microsoft.com/library/dd861305.aspx) sur le site Web de Windows Server.  
  
##### <a name="to-create-a-group-policy-object"></a>Pour créer un objet de stratégie de groupe  
  
1.  Placez le curseur sur le coin supérieur droit de l’écran et cliquez sur l’icône de recherche. Dans la zone de recherche, tapez **gestion des stratégies de groupe**, puis cliquez sur **gestion des stratégies de groupe**.  
  
2.  Développez **forêt: contoso.com**, puis développez **domaines**, accédez à **contoso.com**, développez **(contoso.com)**, puis sélectionnez **FileServerOU**. Avec le bouton droit **créer un objet GPO dans ce domaine et le lier ici**
  
3.  Tapez un nom descriptif pour l’objet de stratégie de groupe, tel que **FlexibleAccessGPO**, puis cliquez sur **OK**.  
  
##### <a name="to-enable-dynamic-access-control-for-contosocom"></a>Pour activer le contrôle d’accès dynamique pour contoso.com  
  
1.  Ouvrez la Console de gestion de stratégie de groupe, cliquez sur **contoso.com**, puis double-cliquez sur **contrôleurs de domaine**.  
  
2.  Avec le bouton droit **stratégie des contrôleurs de domaine par défaut**, puis sélectionnez **modifier**.  
  
3.  Dans la fenêtre Éditeur de gestion de stratégie de groupe, double-cliquez sur **Configuration ordinateur**, double-cliquez sur **stratégies**, double-cliquez sur **modèles d’administration**, double-cliquez sur **système**, puis double-cliquez sur **KDC**.  
  
4.  Double-cliquez sur **prise en charge des revendications, l’authentification composée et le blindage Kerberos** et sélectionnez l’option regard **activé**. Vous devez activer ce paramètre pour utiliser des stratégies d’accès centralisées.  
  
5.  Ouvrez une invite de commandes avec élévation de privilèges et exécutez la commande suivante:  
  
    ```  
    gpupdate /force  
    ```  
  
### <a name="BKMK_FS1"></a>Créer le serveur de fichiers et le serveur AD RMS (FILE1)  
  
1.  Créer un ordinateur virtuel avec le nom FILE1 à partir de l’image ISO de Windows Server 2012.  
  
2.  Se connecter à l’ordinateur virtuel au réseau ID_AD_Network.  
  
3.  Joindre l’ordinateur virtuel au domaine contoso.com, puis connectez-vous à FILE1 en tant que Contoso\Administrateur avec le mot de passe ** pass@word1 **.  
  
#### <a name="install-file-services-resource-manager"></a>Installer le Gestionnaire de ressources de Services de fichiers  
  
###### <a name="to-install-the-file-services-role-and-the-file-server-resource-manager"></a>Pour installer le rôle Services de fichiers et le Gestionnaire de ressources du serveur de fichiers  
  
1.  Dans le Gestionnaire de serveur, cliquez sur **Ajout de rôles et fonctionnalités**.  
  
2.  Sur le **avant de commencer** , cliquez sur **suivant**.  
  
3.  Sur le **sélectionner le type d’installation** , cliquez sur **suivant**.  
  
4.  Sur le **serveur de destination sélectionnez** , cliquez sur **suivant**.  
  
5.  Sur le **sélectionner des rôles de serveur** page, développez **Services de fichiers et stockage**, cochez la case à cocher **de fichiers et iSCSI Services**, développez, puis sélectionnez **File Server Resource Manager**.  
  
    Dans l’ajout de rôles et fonctionnalités Assistant, cliquez sur **ajouter des fonctionnalités**, puis cliquez sur **suivant**.  
  
6.  Sur le **sélectionner des fonctionnalités** , cliquez sur **suivant**.  
  
7.  Sur le **confirmer les sélections d’installation** , cliquez sur **installer**.  
  
8.  Sur le **progression de l’Installation** , cliquez sur **fermer**.  
  
#### <a name="install-the-microsoft-office-filter-packs-on-the-file-server"></a>Installer les Filter Packs de Microsoft Office sur le serveur de fichiers  
Vous devez installer les Filter Packs de Microsoft Office sur Windows Server 2012 pour activer les IFilters pour une plus large gamme de fichiers Office que ceux fournis par défaut.  Windows Server 2012 ne possède pas les IFilters pour les fichiers Microsoft Office installé par défaut, et l’infrastructure de classification de fichiers utilise les IFilters pour effectuer l’analyse du contenu.  
  
Pour télécharger et installer les IFilters, voir [Microsoft Office 2010 Filter Packs](https://go.microsoft.com/fwlink/?LinkID=234122).  
  
#### <a name="configure-email-notifications-on-file1"></a>Configurer des notifications par courrier électronique sur FILE1  
Lorsque vous créez les quotas et les filtres de fichiers, vous avez la possibilité d’envoyer des notifications par courrier électronique aux utilisateurs lorsque leur limite de quota approche ou quand ils ont essayé d’enregistrer des fichiers qui ont été bloqués. Si vous souhaitez informer certains administrateurs de quota et de filtrage des événements de fichier, vous pouvez configurer un ou plusieurs destinataires par défaut. Pour envoyer ces notifications, vous devez spécifier le serveur SMTP à utiliser pour le transfert des messages électroniques.  
  
###### <a name="to-configure-email-options-in-file-server-resource-manager"></a>Pour configurer les options de messagerie dans le Gestionnaire de ressources du serveur de fichiers  
  
1.  Ouvrez le Gestionnaire de ressources du serveur de fichiers. Pour ouvrir le Gestionnaire de ressources du serveur de fichiers, cliquez sur **Démarrer**, type **FSRM**, puis cliquez sur **File Server Resource Manager**.  
  
2.  Dans l’interface du Gestionnaire de ressources de serveur de fichiers, cliquez sur **File Server Resource Manager**, puis cliquez sur **configurer les options de**. Le **File Server Resource Manager Options** boîte de dialogue s’ouvre.  
  
3.  Sur le **Notifications par courrier électronique** onglet, sous le nom du serveur SMTP ou adresse IP, tapez le nom d’hôte ou l’adresse IP du serveur SMTP qui transfèrera notifications par courrier électronique.  
  
4.  Si vous souhaitez systématiquement certains administrateurs des quotas ou des fichiers de filtrage des événements, sous **Administrateurs destinataires par défaut**, tapez chaque adresse de messagerie comme fileadmin@contoso.com. Utilisez le format account@domainet utilisez des points-virgules pour séparer les comptes.  
  
#### <a name="create-groups-on-file1"></a>Créer des groupes sur FILE1  
  
###### <a name="to-create-security-groups-on-file1"></a>Pour créer des groupes de sécurité sur FILE1  
  
1.  Connectez-vous à FILE1 en tant que Contoso\Administrateur avec le mot de passe: ** pass@word1 **.  
  
2.  Ajoutez NT Authority\utilisateurs authentifiés pour le **WinRMRemoteWMIUsers__** groupe.  
  
#### <a name="create-files-and-folders-on-file1"></a>Créer des fichiers et dossiers sur FILE1  
  
1.  Créez un volume NTFS sur FILE1 et puis créez le dossier suivant: D:\Finance Documents.  
  
2.  Créez les fichiers suivants avec les détails indiqués:  
  
    -   **Finance Memo.docx**: ajouter du texte dans le document. Par exemple, «les règles professionnelles concernant qui peuvent accéder aux documents financiers ont changé. Les documents financiers sont désormais accessibles uniquement aux membres du groupe FinanceExpert. Aucun autre service ou groupes n’ont accès.» Vous devez évaluer l’impact de cette modification avant de l’implémenter dans l’environnement. Vérifiez que ce document confidentiel CONTOSO en tant que le pied de page sur chaque page.  
  
    -   **Demande de for Approval to Hire.docx**: créer un formulaire dans ce document, qui collecte les informations du demandeur. Vous devez disposer les champs suivants dans le document: **nom du postulant, numéro de sécurité sociale, emploi, salaire proposé, Date de début, nom du responsable, service**. Ajoutez une section supplémentaire au document qui comporte un formulaire pour **Signature du responsable, salaire approuvé, confirmation de l’offre**, et **statut de l’offre**.   
        Vérifiez la document la gestion des droits.  
  
    -   **Word Document1.docx**: ajoutez du contenu test à ce document.  
  
    -   **Word Document2.docx**: ajouter contenu test à ce document.  
  
    -   **Workbook1.xlsx**  
  
    -   **Workbook2.xlsx**  
  
    -   Créez un dossier sur le bureau nommé Expressions régulières. Créer un document texte sous le dossier appelé **RegEx-SSN**. Tapez le contenu suivant dans le fichier, puis enregistrez et fermez le fichier:   
        ^(?! 000)([0-7]\d{2}|7([0-7]\d|7[012])) ([-]?) (?! 00) \d\d\3 (?! 0000) \d {4} $  
  
3.  Partagez le dossier D:\Finance Documents en tant que Documents financiers et autoriser tout le monde à avoir lu et l’accès en écriture au partage.  
  
> [!NOTE]  
> Stratégies d’accès centralisées ne sont pas activées par défaut sur le système ou démarrer le volume C:.  
  
#### <a name="BKMK_CS1"></a>Installer Active Directory Rights Management Services  
Ajoutez Active Directory Rights Management Services (AD RMS) et toutes les fonctionnalités requises par le biais du Gestionnaire de serveur. Choisissez les paramètres par défaut.  
  
###### <a name="to-install-active-directory-rights-management-services"></a>Pour installer Active Directory Rights Management Services  
  
1.  Connectez-vous à la FILE1 en tant que Contoso\Administrateur ou en tant que membre du groupe Admins du domaine.  
  
    > [!IMPORTANT]  
    > Pour installer le rôle de serveur AD RMS le programme d’installation compte (dans ce cas, contoso\administrateur) devra être donnée membre à la fois le groupe Administrateurs local sur l’ordinateur serveur où les services AD RMS doit être installé et l’appartenance au groupe Administrateurs de l’entreprise dans Active Directory.  
  
2.  Dans le Gestionnaire de serveur, cliquez sur **Ajout de rôles et fonctionnalités**. L’ajout de rôles et fonctionnalités Assistant s’affiche.  
  
3.  Sur le **avant de commencer** , cliquez sur **suivant**.  
  
4.  Sur le **sélectionner le Type d’Installation** , cliquez sur **l’installation basée sur un rôle ou une fonctionnalité**, puis cliquez sur **suivant**.  
  
5.  Sur le **sélectionner les serveurs cibles** , cliquez sur **suivant**.  
  
6.  Sur le **sélectionner des rôles de serveur** de l’écran, cochez la case en regard **Active Directory Rights Management Services**, puis cliquez sur **suivant**.  
  
7.  Dans le **ajouter des fonctionnalités qui sont nécessaires pour Active Directory Rights Management Services? ** boîte de dialogue, cliquez sur **ajouter des fonctionnalités**.  
  
8.  Sur le **sélectionner des rôles de serveur** , cliquez sur **suivant**.  
  
9. Sur le **sélectionner des fonctionnalités à installer** , cliquez sur **suivant**.  
  
10. Sur le **Active Directory Rights Management Services** , cliquez sur Suivant.  
  
11. Sur le **sélectionner les Services de rôle** , cliquez sur **suivant**.  
  
12. Sur le **rôle de serveur Web (IIS)** , cliquez sur **suivant**.  
  
13. Sur le **sélectionner les Services de rôle** , cliquez sur **suivant**.  
  
14. Sur le **confirmer les sélections d’Installation** , cliquez sur **installer**.  
  
15. Une fois l’installation terminée, dans le **progression de l’Installation** , cliquez sur **effectuer une configuration supplémentaire**. L’Assistant de Configuration AD RMS s’affiche.  
  
16. Sur le **AD RMS** , cliquez sur **suivant**.  
  
17. Sur le **Cluster AD RMS** écran, puis sélectionnez **créer un cluster racine AD RMS** puis cliquez sur **suivant**.  
  
18. Sur le **base de données de Configuration** , cliquez sur **utilisation Windows Internal Database sur ce serveur**, puis cliquez sur **suivant**.  
  
    > [!NOTE]  
    > À l’aide de la base de données interne Windows est recommandé pour les environnements de test uniquement, car il ne prend pas en charge plusieurs serveurs dans le cluster AD RMS. Les déploiements de production doivent utiliser un serveur de base de données séparée.  
  
19. Sur le **compte de Service** écran en **compte utilisateur de domaine**, cliquez sur **spécifier** , puis spécifiez le nom d’utilisateur (**contoso\rms**) et le mot de passe (**pass@word1**) et cliquez sur **OK**, puis cliquez sur **suivant**.  
  
20. Sur le **le Mode de chiffrement** , cliquez sur **Mode de chiffrement 2**.  
  
21. Sur le **stockage de clé de Cluster** , cliquez sur **suivant**.  
  
22. Sur le **mot de passe de clé de Cluster** de l’écran, dans le **mot de passe** et **confirmer le mot de passe** zones, tapez ** pass@word1 **, puis cliquez sur **suivant**.  
  
23. Sur le **Site Web de Cluster** écran, assurez-vous que **Site Web par défaut** est sélectionné, puis cliquez sur **suivant**.  
  
24. Sur le **adresse du Cluster** écran, puis sélectionnez le **utiliser une connexion non chiffrée** option dans le **nom de domaine complet** , tapez **FILE1.contoso.com**, puis cliquez sur **suivant**.  
  
25. Sur le **nom de certificat de licence** écran, acceptez le nom par défaut (**FILE1**) dans la zone de texte et cliquez sur **suivant**.  
  
26. Sur le **inscription SCP** écran, puis sélectionnez **inscrire SCP**, puis cliquez sur **suivant**.  
  
27. Sur le **Confirmation** , cliquez sur **installer**.  
  
28. Sur le **résultats** , cliquez sur **fermer**, puis cliquez sur **fermer** sur **progression de l’Installation** écran. Lorsque vous avez terminé, fermez la session et connectez-vous en tant que contoso\rms avec le mot de passe fourni (**pass@word1**).  
  
29. Lancer la console AD RMS et accédez à **modèles de stratégie de droits**.  
  
    Pour ouvrir la console AD RMS, dans le Gestionnaire de serveur, cliquez sur **serveur Local** dans l’arborescence de la console, puis cliquez sur **outils**, puis cliquez sur **Active Directory Rights Management Services**.  
  
30. Cliquez sur le **créer une stratégie de droits distribué** modèle se trouve dans le volet droit, cliquez sur **ajouter**, puis sélectionnez les informations suivantes:  
  
    -   Langue: Anglais des États-Unis  
  
    -   Nom: Contoso Finance Admin Only  
  
    -   Description: Contoso Finance Admin Only  
  
    Cliquez sur **ajouter**, puis cliquez sur **suivant**.  
  
31. Sous la section utilisateurs et droits, cliquez sur **utilisateurs et droits**, cliquez sur **ajouter**, type ** financeadmin@contoso.com **, puis cliquez sur **OK**.  
  
32. Sélectionnez **contrôle total**et laissez **octroyer le contrôle total propriétaire (auteur) vers la droite avec sans date d’expiration** sélectionné.  
  
33. Si les autres onglets sans apporter de modifications, puis **Terminer**. Connectez-vous en tant que Contoso\Administrateur.  
  
34. Accédez à la sélection de dossier, C:\inetpub\wwwroot\\_wmcs\certification, le fichier ServerCertification.asmx, puis ajoutez utilisateurs authentifiés ont de lecture et d’autorisations en écriture sur le fichier.  
  
35. Ouvrez Windows PowerShell et exécutez `Get-FsrmRmsTemplate`. Vérifiez que vous êtes en mesure de voir le modèle RMS que vous avez créé à l’étape précédente de cette procédure avec cette commande.  
  
> [!IMPORTANT]  
> Si vous souhaitez que vos serveurs changent immédiatement pour pouvoir les tester, vous devez effectuer les opérations suivantes:  
>   
> 1.  Sur le serveur de fichiers, FILE1, ouvrez une invite de commandes avec élévation de privilèges et exécutez les commandes suivantes:  
>   
>     -   gpupdate /force.  
>     -   NLTEST /SC_RESET:contoso.com  
> 2.  Sur le contrôleur de domaine (DC1), répliquez Active Directory.  
>   
>     Pour plus d’informations sur les étapes pour forcer la réplication d’Active Directory, voir [la réplication Active Directory](https://technet.microsoft.com/library/cc794809(WS.10).aspx)  
  
Si vous le souhaitez, au lieu d’utiliser l’ajout de rôles et fonctionnalités Assistant dans le Gestionnaire de serveur, vous pouvez utiliser Windows PowerShell pour installer et configurer le rôle de serveur AD RMS comme indiqué dans la procédure suivante.  
  
###### <a name="to-install-and-configure-an-ad-rms-cluster-in-windows-server-2012-using-windows-powershell"></a>Pour installer et configurer un cluster AD RMS dans Windows Server 2012 à l’aide de Windows PowerShell  
  
1.  Ouvrez une session en tant que Contoso\Administrateur avec le mot de passe: ** pass@word1 **.  
  
    > [!IMPORTANT]  
    > Pour installer le rôle de serveur AD RMS le programme d’installation compte (dans ce cas, contoso\administrateur) devra être donnée membre à la fois le groupe Administrateurs local sur l’ordinateur serveur où les services AD RMS doit être installé et l’appartenance au groupe Administrateurs de l’entreprise dans Active Directory.  
  
2.  Sur le bureau du serveur, avec le bouton droit sur la barre des tâches, puis sélectionnez l’icône de Windows PowerShell **exécuter en tant qu’administrateur** pour ouvrir une invite Windows PowerShell avec des privilèges d’administration.  
  
3.  Pour utiliser les applets de commande Gestionnaire de serveur pour installer le rôle serveur AD RMS, tapez:  
  
    ```  
    Add-WindowsFeature ADRMS '"IncludeAllSubFeature '"IncludeManagementTools  
    ```  
  
4.  Créer le lecteur Windows PowerShell qui représente le serveur AD RMS que vous installez.  
  
    Par exemple, pour créer un lecteur Windows PowerShell nommé RC pour installer et configurer le premier serveur dans un cluster racine AD RMS, tapez:  
  
    ```  
    Import-Module ADRMS  
    New-PSDrive -PSProvider ADRMSInstall -Name RC -Root RootCluster  
    ```  
  
5.  Définir des propriétés sur les objets qui représentent des paramètres de configuration requis dans l’espace de noms du lecteur.  
  
    Par exemple, pour définir le compte de service AD RMS, à l’invite de commandes Windows PowerShell, tapez:  
  
    ```  
    $svcacct = Get-Credential  
    ```  
  
    Lorsque la boîte de dialogue de sécurité Windows apparaît, tapez le nom d’utilisateur de domaine de services AD RMS service compte CONTOSO\RMS et le mot de passe.  
  
    Ensuite, pour attribuer le compte de service AD RMS pour les paramètres de cluster AD RMS, tapez la commande suivante:  
  
    ```  
    Set-ItemProperty -Path RC:\ -Name ServiceAccount -Value $svcacct  
    ```  
  
    Ensuite, pour définir le serveur AD RMS pour utiliser la base de données interne de Windows, à l’invite de commande Windows PowerShell, tapez:  
  
    ```  
    Set-ItemProperty -Path RC:\ClusterDatabase -Name UseWindowsInternalDatabase -Value $true  
    ```  
  
    Ensuite, pour stocker de manière sécurisée le mot de passe de clé de cluster dans une variable, à l’invite de commande Windows PowerShell, tapez:  
  
    ```  
    $password = Read-Host -AsSecureString -Prompt "Password:"  
    ```  
  
    Tapez le mot de passe de clé de cluster, puis appuyez sur la touche ENTRÉE.  
  
    Ensuite, pour assigner le mot de passe pour votre installation AD RMS, à l’invite de commande Windows PowerShell, tapez:  
  
    ```  
    Set-ItemProperty -Path RC:\ClusterKey -Name CentrallyManagedPassword -Value $password  
    ```  
  
    Ensuite, pour définir l’adresse du cluster AD RMS, à l’invite de commandes Windows PowerShell, tapez:  
  
    ```  
    Set-ItemProperty -Path RC:\ -Name ClusterURL -Value "http://file1.contoso.com:80"  
    ```  
  
    Ensuite, pour assigner le nom SLC de votre installation AD RMS, à l’invite de commande Windows PowerShell, tapez:  
  
    ```  
    Set-ItemProperty -Path RC:\ -Name SLCName -Value "FILE1"  
    ```  
  
    Ensuite, pour définir le point de connexion de service (SCP) pour le cluster AD RMS, à l’invite de commandes Windows PowerShell, tapez:  
  
    ```  
    Set-ItemProperty -Path RC:\ -Name RegisterSCP -Value $true  
    ```  
  
6.  Exécutez le **Install-ADRMS** applet de commande. Outre l’installation du rôle de serveur AD RMS et la configuration du serveur, cette applet de commande installe également d’autres fonctionnalités requises par les services AD RMS, si nécessaire.  
  
    Par exemple, pour modifier le lecteur Windows PowerShell nommé RC et installer et configurer les services AD RMS, tapez:  
  
    ```  
    Set-Location RC:\  
    Install-ADRMS -Path.  
    ```  
  
    Tapez «O» lorsque l’applet de commande vous invite à confirmer que vous souhaitez démarrer l’installation.  
  
7.  Fermeture de session en tant que Contoso\Administrateur et le journal sur tant que CONTOSO\RMS avec le mot de passe fourni («pass@word1»).  
  
    > [!IMPORTANT]  
    > Afin de gérer le serveur AD RMS, le compte que vous êtes connecté et utilisez pour gérer le serveur (dans ce cas, CONTOSO\RMS) aura à attribuer l’appartenance dans les deux du groupe Administrateurs local sur l’ordinateur serveur AD RMS, ainsi que l’appartenance au groupe Administrateurs d’entreprise dans Active Directory.  
  
8.  Sur le bureau du serveur, avec le bouton droit sur la barre des tâches, puis sélectionnez l’icône de Windows PowerShell **exécuter en tant qu’administrateur** pour ouvrir une invite Windows PowerShell avec des privilèges d’administration.  
  
9. Créer le lecteur Windows PowerShell qui représente le serveur AD RMS que vous configurez.  
  
    Par exemple, pour créer un lecteur Windows PowerShell nommé RC pour configurer le cluster racine AD RMS, tapez:  
  
    ```  
    Import-Module ADRMSAdmin `  
    New-PSDrive -PSProvider ADRMSAdmin -Name RC -Root http://localhost -Force -Scope Global  
    ```  
  
10. Pour créer un modèle de droits pour l’administrateur du service finance Contoso et lui assigner des droits d’utilisateur avec un contrôle total dans votre installation AD RMS, à l’invite de commande Windows PowerShell, tapez:  
  
    ```  
    New-Item -Path RC:\RightsPolicyTemplate '"LocaleName en-us -DisplayName "Contoso Finance Admin Only" -Description "Contoso Finance Admin Only" -UserGroup financeadmin@contoso.com  -Right ('FullControl')  
    ```  
  
11. Pour vérifier que vous pouvez afficher le nouveau modèle de droits de l’administrateur du service finance de Contoso, à l’invite de commande Windows PowerShell:  
  
    ```  
    Get-FsrmRmsTemplate  
    ```  
  
    Examinez la sortie de cette applet de commande pour confirmer le modèle RMS que vous avez créé à l’étape précédente est présent.  
  
### <a name="build-the-mail-server-srv1"></a>Créer le serveur de messagerie (SRV1)  
SRV1 est le serveur de messagerie SMTP/POP3. Vous devez configurer afin que vous pouvez envoyer des notifications par courrier électronique en tant que partie du scénario d’assistance accès refusé.  
  
Configurer Microsoft Exchange Server sur cet ordinateur. Pour plus d’informations, voir [comment installer Exchange Server](https://go.microsoft.com/fwlink/?prd=12364).  
  
### <a name="build-the-client-virtual-machine-client1"></a>Créer l’ordinateur virtuel de client (CLIENT1)  
  
##### <a name="to-build-the-client-virtual-machine"></a>Pour créer l’ordinateur virtuel client  
  
1.  Connectez CLIENT1 au réseau ID_AD_Network.  
  
2.  Installez Microsoft Office 2010.  
  
3.  Connectez-vous en tant que Contoso\Administrator, puis utilisez les informations suivantes pour configurer Microsoft Outlook.  
  
    -   Votre nom: administrateur de fichiers  
  
    -   Adresse de messagerie:fileadmin@contoso.com  
  
    -   Type de compte: POP3  
  
    -   Serveur de courrier entrant: adresse IP statique de SRV1  
  
    -   Serveur de courrier sortant: adresse IP statique de SRV1  
  
    -   Nom d’utilisateur:fileadmin@contoso.com  
  
    -   N’oubliez pas de mot de passe: sélectionner  
  
4.  Créer un raccourci vers Outlook sur le bureau contoso\administrateur.  
  
5.  Ouvrez Outlook et tous les messages «première lancée».  
  
6.  Supprimer les messages de test qui ont été générées.  
  
7.  Créer un nouveau raccourci sur le bureau pour tous les utilisateurs sur l’ordinateur virtuel client qui pointe vers \\\FILE1\Finance Documents.  
  
8.  Redémarrez si nécessaire.  
  
##### <a name="enable-access-denied-assistance-on-the-client-virtual-machine"></a>Activer l’accès refusé pour l’assistance sur l’ordinateur virtuel client  
  
1.  Ouvrez l’Éditeur du Registre et accédez à **HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows\Explorer**.  
  
    -   Définissez **EnableShellExecuteFileStreamCheck** à **1**.  
  
    -   Valeur: DWORD  
  
## <a name="BKMK_CF"></a>Configuration du laboratoire pour le déploiement de revendications dans le scénario de forêts  
  
### <a name="BKMK_2.1"></a>Créez un ordinateur virtuel pour DC2  
  
-   Créez un ordinateur virtuel à partir de l’image ISO de Windows Server 2012.  
  
-   Créer le nom de l’ordinateur virtuel en tant que DC2.  
  
-   Se connecter à l’ordinateur virtuel au réseau ID_AD_Network.  
  
> [!IMPORTANT]  
> Joindre des ordinateurs virtuels à un domaine et le déploiement de types de revendications dans les forêts exigent que les ordinateurs virtuels soient en mesure de résoudre les noms de domaine complets des domaines en question. Vous devrez peut-être configurer manuellement les paramètres DNS sur les ordinateurs virtuels pour effectuer cette opération. Pour plus d’informations, voir [configuration d’un réseau virtuel](https://technet.microsoft.com/library/cc732470%28v=ws.10%29.aspx).  
>   
> Toutes les images d’ordinateur virtuel (serveurs et clients) doivent être reconfigurées pour utiliser une adresse IP version 4 (IPv4) statique adresse et les paramètres de client système DNS (Domain Name). Pour plus d’informations, voir [configurer un Client DNS pour des adresses IP statiques](https://go.microsoft.com/fwlink/?LinkId=150952).  
  
### <a name="BKMK_2.2"></a>Configurer une nouvelle forêt nommée adatum.com  
  
##### <a name="to-install-active-directory-domain-services"></a>Pour installer les Services de domaine Active Directory  
  
1.  Se connecter à l’ordinateur virtuel au réseau ID_AD_Network. Connectez-vous à DC2 en tant qu’administrateur avec le mot de passe ** Pass@word1 **.  
  
2.  Dans le Gestionnaire de serveur, cliquez sur **gérer**, puis cliquez sur **Ajout de rôles et fonctionnalités**.  
  
3.  Sur le **avant de commencer** , cliquez sur **suivant**.  
  
4.  Sur le **sélectionner le Type d’Installation** , cliquez sur **installer un rôle ou une fonctionnalité**, puis cliquez sur **suivant**.  
  
5.  Sur le **serveur de destination sélectionnez** , cliquez sur **sélectionner un serveur du pool de serveurs**, cliquez sur le nom du serveur sur lequel vous souhaitez installer les Services de domaine Active Directory (AD DS), puis cliquez sur **suivant**.  
  
6.  Sur le **sélectionner des rôles de serveur** , cliquez sur **les Services de domaine Active Directory**. Dans le **Assistant Ajouter des rôles et fonctionnalités** boîte de dialogue, cliquez sur **ajouter des fonctionnalités**, puis cliquez sur **suivant**.  
  
7.  Sur le **sélectionner des fonctionnalités** , cliquez sur **suivant**.  
  
8.  Sur le **AD DS** page, passez en revue les informations, puis cliquez sur **suivant**.  
  
9. Sur le **Confirmation** , cliquez sur **installer**. La barre de progression installation fonctionnalité sur la page résultats indique que le rôle est installé.  
  
10. Sur le **résultats** page, vérifiez que l’installation a réussi, puis cliquez sur l’icône d’avertissement avec un point d’exclamation sur le coin supérieur droit de l’écran, en regard **gérer**. Dans la liste des tâches, cliquez sur le **promouvoir ce serveur en contrôleur de domaine** lien.  
  
    > [!IMPORTANT]  
    > Si vous fermez l’Assistant installation à ce stade au lieu de cliquer **promouvoir ce serveur en contrôleur de domaine**, vous pouvez continuer l’installation des services AD DS en cliquant sur **tâches** dans le Gestionnaire de serveur.  
  
11. Sur le **Configuration de déploiement** , cliquez sur **ajouter une nouvelle forêt**, tapez le nom du domaine racine, **adatum.com**, puis cliquez sur **suivant**.  
  
12. Sur le **Options du contrôleur de domaine** page, sélectionnez les niveaux fonctionnels de domaine et de forêt Windows Server 2012, spécifiez le mot de passe DSRM ** pass@word1 **, puis cliquez sur **suivant**.  
  
13. Sur le **Options DNS** , cliquez sur **suivant**.  
  
14. Sur le **Options supplémentaires** , cliquez sur **suivant**.  
  
15. Sur le **chemins d’accès** page, tapez les emplacements de la base de données Active Directory, le fichiers journaux et le dossier SYSVOL (ou acceptez les emplacements par défaut), puis cliquez sur **suivant**.  
  
16. Sur le **examiner les Options** page, confirmez vos sélections, puis cliquez sur **suivant**.  
  
17. Sur le **vérification des conditions requises** page, confirmez que la validation des conditions préalables terminée, puis cliquez sur **installer**.  
  
18. Sur le **résultats** page, vérifiez que le serveur a été correctement configuré comme un contrôleur de domaine, puis cliquez sur **fermer**.  
  
19. Redémarrez le serveur pour terminer l’installation des services AD DS. (Par défaut, cela se produit automatiquement.)  
  
> [!IMPORTANT]  
> Pour vous assurer que le réseau est configuré correctement, une fois que vous avez configuré les deux forêts, vous devez procédez comme suit:  
>   
> -   Connectez-vous à adatum.com en tant qu’adatum\administrateur. Ouvrez une fenêtre d’invite de commandes, tapez **nslookup contoso.com**, puis appuyez sur ENTRÉE.  
> -   Connectez-vous à contoso.com en tant que Contoso\Administrateur. Ouvrez une fenêtre d’invite de commandes, tapez **nslookup adatum.com**, puis appuyez sur ENTRÉE.  
>   
> Si ces commandes s’exécutent sans erreur, les forêts peuvent communiquer entre eux. Pour plus d’informations sur les erreurs nslookup, voir la section Dépannage de la rubrique [utilisation de NSlookup.exe](https://support.microsoft.com/kb/200525)  
  
### <a name="BKMK_2.22"></a>Définir contoso.com comme forêt adatum.com  
Dans cette étape, vous créez une relation d’approbation entre le site d’Adatum Corporation et le site de Contoso, Ltd.  
  
##### <a name="to-set-contoso-as-a-trusting-forest-to-adatum"></a>Pour définir Contoso comme forêt Adatum  
  
1.  Connectez-vous à DC2 en tant qu’administrateur. Sur le **Démarrer** , tapez domain.msc.  
  
2.  Dans l’arborescence de la console, cliquez sur adatum.com, puis cliquez sur Propriétés.  
  
3.  Sur le **approbations** , cliquez sur **nouvelle approbation**, puis cliquez sur **suivant**.  
  
4.  Sur le **nom d’approbation** , tapez **contoso.com**, champ de nom dans le système DNS (Domain Name), puis cliquez sur **suivant**.  
  
5.  Sur le **Type d’approbation** , cliquez sur **approbation de forêt**, puis cliquez sur **suivant**.  
  
6.  Sur le **Direction de l’approbation** , cliquez sur **bidirectionnelle**.  
  
7.  Sur le **côtés de l’approbation** , cliquez sur **à la fois ce domaine et le domaine spécifié**, puis cliquez sur **suivant**.  
  
8.  Continuez à suivre les instructions de l’Assistant.  
  
### <a name="BKMK_2.4"></a>Créer des utilisateurs supplémentaires dans la forêt Adatum  
Créez l’utilisateur Jeff Low avec le mot de passe ** pass@word1 **et assignez l’attribut de société avec la valeur **Adatum**.  
  
##### <a name="to-create-a-user-with-the-company-attribute"></a>Pour créer un utilisateur avec l’attribut de société  
  
1.  Ouvrez une invite de commandes avec élévation de privilèges dans Windows PowerShell et collez le code suivant:  
  
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
  
##### <a name="to-create-a-claim-type-by-using-windows-powershell"></a>Pour créer un type de revendication à l’aide de Windows PowerShell  
  
1.  Connectez-vous à adatum.com en tant qu’administrateur.  
  
2.  Ouvrez une invite de commandes avec élévation de privilèges dans Windows PowerShell et tapez le code suivant:  
  
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
  
1.  Connectez-vous à contoso.com en tant qu’administrateur.  
  
2.  Dans le Gestionnaire de serveur, cliquez sur **outils**, puis cliquez sur **centre d’administration Active Directory**.  
  
3.  Dans le volet gauche du centre d’administration Active Directory, cliquez sur **arborescence**. Dans le volet gauche, cliquez sur **contrôle d’accès dynamique**, puis double-cliquez sur **propriétés de ressource**.  
  
4.  Sélectionnez **société** à partir de la **propriétés de ressource** liste, avec le bouton droit et sélectionnez **propriétés**. Dans le **valeurs suggérées** , cliquez sur **ajouter** pour ajouter les valeurs suggérées: Contoso et Adatum, puis cliquez sur **OK** deux fois.  
  
5.  Sélectionnez **société** à partir de la **propriétés de ressource** liste, avec le bouton droit et sélectionnez **activer**.  
  
### <a name="BKMK_2.6"></a>Activer le contrôle d’accès dynamique sur adatum.com  
  
##### <a name="to-enable-dynamic-access-control-for-adatumcom"></a>Pour activer le contrôle d’accès dynamique pour adatum.com  
  
1.  Connectez-vous à adatum.com en tant qu’administrateur.  
  
2.  Ouvrez la Console de gestion de stratégie de groupe, cliquez sur **adatum.com**, puis double-cliquez sur **contrôleurs de domaine**.  
  
3.  Avec le bouton droit **stratégie des contrôleurs de domaine par défaut**, puis sélectionnez **modifier**.  
  
4.  Dans la fenêtre Éditeur de gestion de stratégie de groupe, double-cliquez sur **Configuration ordinateur**, double-cliquez sur **stratégies**, double-cliquez sur **modèles d’administration**, double-cliquez sur **système**, puis double-cliquez sur **KDC**.  
  
5.  Double-cliquez sur **prise en charge des revendications, l’authentification composée et le blindage Kerberos** et sélectionnez l’option regard **activé**. Vous devez activer ce paramètre pour utiliser des stratégies d’accès centralisées.  
  
6.  Ouvrez une invite de commandes avec élévation de privilèges et exécutez la commande suivante:  
  
    ```  
    gpupdate /force  
    ```  
  
### <a name="BKMK_2.8"></a>Créer le type de revendication Company sur contoso.com  
  
##### <a name="to-create-a-claim-type-by-using-windows-powershell"></a>Pour créer un type de revendication à l’aide de Windows PowerShell  
  
1.  Connectez-vous à contoso.com en tant qu’administrateur.  
  
2.  Ouvrez une invite de commandes avec élévation de privilèges dans Windows PowerShell, puis tapez le code suivant:  
  
    ```  
    New-ADClaimType '"SourceTransformPolicy `  
    '"DisplayName 'Company' `  
    '"ID 'ad://ext/Company:ContosoAdatum' `  
    '"IsSingleValued $true `  
    '"ValueType 'string' `  
  
    ```  
  
### <a name="BKMK_2.9"></a>Créer la règle d’accès centralisée  
  
##### <a name="to-create-a-central-access-rule"></a>Pour créer une règle d’accès centralisée  
  
1.  Dans le volet gauche du centre d’administration Active Directory, cliquez sur **arborescence**. Dans le volet gauche, cliquez sur **contrôle d’accès dynamique**, puis cliquez sur **règles d’accès Central**.  
  
2.  Avec le bouton droit **règles d’accès Central**, cliquez sur **New**, puis **règle d’accès Central**.  
  
3.  Dans le **nom** , tapez **Règleaccèsemployésadatum**.  
  
4.  Dans le **autorisations** section, sélectionnez le **utiliser les autorisations suivantes en tant qu’autorisations actuelles** , cliquez sur **modifier**, puis cliquez sur **ajouter**. Cliquez sur le **sélectionnez un principal** lier, tapez **utilisateurs authentifiés**, puis cliquez sur **OK**.  
  
5.  Dans le **entrée d’autorisation pour les autorisations** boîte de dialogue, cliquez sur **ajouter une condition**et entrez les conditions suivantes: [**utilisateur**] [**société**] [**est égal à**] [**valeur**] [**Adatum**]. Les autorisations doivent être **modification, lecture et exécution, lecture, écriture**.  
  
6.  Cliquez sur **OK**.  
  
7.  Cliquez sur **OK** trois fois pour terminer et revenir au centre d’administration Active Directory.  
  
    ![guides de solutions](media/Appendix-B--Setting-Up-the-Test-Environment/PowerShellLogoSmall.gif)Windows PowerShell équivalentes commandes ***  
  
    L’applet de commande Windows PowerShell ou les applets de commande suivantes remplissent la même fonction que la procédure précédente. Entrez chaque applet de commande sur une seule ligne, même si elles peuvent apparaître comme renvoyées sur plusieurs lignes ici en raison de contraintes de mise en forme.  
  
    ```  
    New-ADCentralAccessRule `  
    -CurrentAcl:"O:SYG:SYD:AR(A;;FA;;;OW)(A;;FA;;;BA)(A;;FA;;;SY)(XA;;0x1301bf;;;AU;(@USER.ad://ext/Company:ContosoAdatum == `"Adatum`"))" `  
    -Name:"AdatumEmployeeAccessRule" `  
    -ProposedAcl:$null `  
    -ProtectedFromAccidentalDeletion:$true `  
    -Server:"contoso.com" `  
    ```  
  
### <a name="BKMK_2.10"></a>Créer la stratégie d’accès centralisée  
  
##### <a name="to-create-a-central-access-policy"></a>Pour créer une stratégie d’accès centralisée  
  
1.  Connectez-vous à contoso.com en tant qu’administrateur.  
  
2.  Ouvrez une invite de commandes avec élévation de privilèges dans Windows PowerShell, puis collez le code suivant:  
  
    ```  
    New-ADCentralAccessPolicy "Adatum Only Access Policy"   
    Add-ADCentralAccessPolicyMember "Adatum Only Access Policy" `  
    -Member "AdatumEmployeeAccessRule" `  
    ```  
  
### <a name="BKMK_2.11"></a>Publier la nouvelle stratégie via la stratégie de groupe  
  
##### <a name="to-apply-the-central-access-policy-across-file-servers-through-group-policy"></a>Pour appliquer la stratégie d’accès centralisée sur les serveurs de fichiers via la stratégie de groupe  
  
1.  Sur le **Démarrer** , tapez **outils d’administration**et dans le **recherche** , cliquez sur **paramètres**. Dans le **paramètres** des résultats, cliquez sur **outils d’administration**. Ouvrez la Console de gestion de stratégie de groupe à partir de la **outils d’administration** dossier.  
  
    > [!TIP]  
    > Si le **outils afficher d’administration** paramètre est désactivé, le dossier Outils d’administration et son contenu s’affiche pas dans le **paramètres** résultats.  
  
2.  Cliquez sur le domaine contoso.com, cliquez sur **créer un objet GPO dans ce domaine et le lier ici**  
  
3.  Tapez un nom descriptif pour l’objet de stratégie de groupe, tel que **Gpoaccèsadatum**, puis cliquez sur **OK**.  
  
##### <a name="to-apply-the-central-access-policy-to-the-file-server-through-group-policy"></a>Pour appliquer la stratégie d’accès centralisée pour le serveur de fichiers via la stratégie de groupe  
  
1.  Sur le **Démarrer** , tapez **gestion des stratégies de groupe**, dans le **recherche** zone. Ouvrez **gestion des stratégies de groupe** à partir du dossier Outils d’administration.  
  
    > [!TIP]  
    > Si le **outils afficher d’administration** paramètre est désactivé, le dossier Outils d’administration et son contenu s’affiche pas dans les résultats de paramètres.  
  
2.  Accédez à Contoso et sélectionnez-le comme suit: stratégies de groupe\forêt: contoso.com\Domains\contoso.com.  
  
3.  Cliquez sur le **Gpoaccèsadatum** stratégie, puis sélectionnez **modifier**.  
  
4.  Dans l’éditeur de gestion de stratégie de groupe, cliquez sur **Configuration ordinateur**, développez **stratégies**, développez **paramètres Windows**, puis cliquez sur **paramètres de sécurité**.  
  
5.  Développez **système de fichiers**, avec le bouton droit **stratégie d’accès centralisée**, puis cliquez sur **stratégies d’accès gérer centralisées**.  
  
6.  Dans le **Configuration des stratégies d’accès Central** boîte de dialogue, cliquez sur **ajouter**, sélectionnez **uniquement stratégie d’accès Adatum**, puis cliquez sur **OK**.  
  
7.  Fermez l’éditeur de gestion de stratégie de groupe. Vous avez maintenant ajouté la stratégie d’accès centralisée à la stratégie de groupe.  
  
### <a name="BKMK_2.12"></a>Créer le dossier Earnings sur le serveur de fichiers  
Créez un volume NTFS sur FILE1 et créez le dossier suivant: D:\Earnings.  
  
> [!NOTE]  
> Stratégies d’accès centralisées ne sont pas activées par défaut sur le système ou démarrer le volume C:.  
  
### <a name="BKMK_2.13"></a>Définir la classification et appliquer la stratégie d’accès centralisée sur le dossier Earnings  
  
##### <a name="to-assign-the-central-access-policy-on-the-file-server"></a>Pour attribuer la stratégie d’accès centralisée sur le serveur de fichiers  
  
1.  Dans le Gestionnaire Hyper-V, connectez-vous au serveur FILE1. Connectez-vous au serveur avec le compte contoso\administrateur, avec le mot de passe ** pass@word1 **.  
  
2.  Ouvrez une invite de commandes avec élévation de privilèges et le type: **gpupdate /force**. Cela permet de garantir que les modifications de la stratégie de groupe prennent effet sur votre serveur.  
  
3.  Vous devez aussi actualiser les propriétés de ressource globales à partir d’Active Directory. Ouvrez Windows PowerShell, tapez `Update-FSRMClassificationpropertyDefinition`, puis appuyez sur ENTRÉE. Fermez Windows PowerShell.  
  
4.  Ouvrez l’Explorateur Windows et accédez à D:\EARNINGS. Cliquez sur le **revenus** dossier, puis cliquez sur **propriétés**.  
  
5.  Cliquez sur le **Classification** onglet **société**, puis sélectionnez **Adatum** dans les **valeur** champ.  
  
6.  Cliquez sur **modification**, sélectionnez **uniquement stratégie d’accès Adatum** dans le menu déroulant, puis cliquez sur **appliquer**.  
  
7.  Cliquez sur le **sécurité**, cliquez sur **avancé**, puis cliquez sur le **stratégie centralisée** onglet. Vous devez voir le **Règleaccèsemployésadatum** répertoriés. Vous pouvez développer l’élément pour afficher toutes les autorisations que vous avez définies lors de la création de la règle dans Active Directory.  
  
8.  Cliquez sur **OK** pour revenir à l’Explorateur Windows.  
  


