---
ms.assetid: ac727bd1-a892-47ed-a7ba-439b34187d4e
title: Descriptions des pages des Assistants Installation et Suppression des services de domaine Active Directory
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 7370dfed68e22ca88030aec913db4eb52eef9ec3
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80825452"
---
# <a name="ad-ds-installation-and-removal-wizard-page-descriptions"></a>Descriptions des pages des Assistants Installation et Suppression des services de domaine Active Directory

>S’applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Cette rubrique décrit les commandes figurant sur les pages des Assistants suivants en rapport avec l’installation et la suppression du rôle serveur AD DS dans le Gestionnaire de serveur.  
  
-   [Configuration du déploiement](../../ad-ds/deploy/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions.md#BKMK_DepConfigPage)  
  
-   [Options du contrôleur de domaine](../../ad-ds/deploy/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions.md#BKMK_DCOptionsPage)  
  
-   [Options DNS](../../ad-ds/deploy/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions.md#BKMK_DNSOptionsPage)  
  
-   [Options RODC](../../ad-ds/deploy/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions.md#BKMK_RODCOptionsPage)  
  
-   [Options supplémentaires](../../ad-ds/deploy/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions.md#BKMK_AdditionalOptionsPage)  
  
-   [Trajet](../../ad-ds/deploy/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions.md#BKMK_Paths)  
  
-   [Options de préparation](../../ad-ds/deploy/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions.md#BKMK_AdprepCreds)  
  
-   [Options de révision](../../ad-ds/deploy/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions.md#BKMK_ViewInstallOptionsPage)  
  
-   [Vérification de la configuration requise](../../ad-ds/deploy/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions.md#BKMK_PrerqCheckPage)  
  
-   [About](../../ad-ds/deploy/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions.md#BKMK_Results)  
  
-   [Informations d’identification de suppression de rôle](../../ad-ds/deploy/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions.md#BKMK_RemovalCredsPage)  
  
-   [AD DS les options de suppression et les avertissements](../../ad-ds/deploy/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions.md#BKMK_RemovalOptionsPage)  
  
-   [Nouveau mot de passe administrateur](../../ad-ds/deploy/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions.md#BKMK_NewAdminPwdPage)  
  
-   [Confirmer les sélections de suppression de rôle](../../ad-ds/deploy/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions.md#BKMK_ConfirmRoleRemovalPage)  
  
## <a name="deployment-configuration"></a><a name="BKMK_DepConfigPage"></a>Configuration du déploiement  
Dans le Gestionnaire de serveur, chaque installation d’un contrôleur de domaine débute par la page **Configuration de déploiement**. Dans cette page et les pages suivantes, les options disponibles et les champs requis varient selon l’opération de déploiement que vous sélectionnez. Par exemple, si vous créez une forêt, la page **options de préparation** n’apparaît pas, mais c’est le cas si vous installez le premier contrôleur de domaine qui exécute Windows Server 2012 dans une forêt ou un domaine existant.  
  
Des tests de validation sont effectués sur cette page et, par la suite, dans le cadre de la vérification de la configuration requise. Par exemple, si vous essayez d’installer le premier contrôleur de domaine Windows Server 2012 dans une forêt qui a le niveau fonctionnel Windows 2000, une erreur s’affiche sur cette page.  
  
Les options suivantes apparaissent lorsque vous créez une forêt.  
  
![AD DS installer](media/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions/ADDS_SMI_DeploymentConfiguration_Forest.gif)  
  
-   Lorsque vous créez une forêt, vous devez spécifier le nom du domaine racine de forêt. Le nom de domaine racine de la forêt ne peut pas être à étiquette unique (par exemple, il doit être « contoso.com » au lieu de « Contoso »). Il doit respecter les conventions d’affectation des noms des domaines DNS. Vous pouvez spécifier un nom de domaine international. Pour plus d’informations sur les conventions d’affectation des noms de domaine DNS, voir l’[article 909264 de la Base de connaissances Microsoft](https://support.microsoft.com/kb/909264).  
  
-   Ne créez pas de forêts Active Directory portant le même nom que votre nom DNS externe. Par exemple, si votre URL DNS Internet est http :\//contoso.com, vous devez choisir un nom différent pour votre forêt interne afin d’éviter d’éventuels problèmes de compatibilité. Ce nom doit être unique et faire l’objet d’une utilisation peu probable en termes de trafic Web, comme corp.contoso.com.  
  
-   Vous devez être membre du groupe Administrateurs sur le serveur sur lequel vous voulez créer une forêt.  
  
Pour plus d’informations sur la création d’une forêt, voir [installer un nouveau Windows Server 2012 Active Directory &#40;au niveau&#41;de la forêt 200](../../ad-ds/deploy/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-.md).  
  
Les options suivantes apparaissent lorsque vous créez un domaine.  
  
![AD DS installer](media/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions/ADDS_SMI_DeploymentConfiguration_ChildDomain.gif)  
  
> [!NOTE]  
> Si vous créez un domaine d’arborescence, vous devez spécifier le nom du domaine racine de forêt à la place du domaine parent, mais les pages et les options restantes de l’Assistant restent les mêmes.  
  
-   Cliquez sur **Sélectionner** pour accéder au domaine parent ou à l’arborescence Active Directory, ou bien tapez un domaine parent valide ou un nom d’arborescence. Tapez ensuite le nom du nouveau domaine dans **Nouveau nom de domaine**.  
  
-   Domaine d’arborescence : entrez un nom de domaine racine complet valide ; le nom ne doit pas être en une partie et doit satisfaire aux conditions d’attribution des noms de domaine DNS.  
  
-   Domaine enfant : entrez un nom de domaine enfant en une partie valide ; le nom doit satisfaire aux conditions d’attribution des noms de domaine DNS.  
  
-   L’Assistant Configuration des services de domaine Active Directory vous invite à entrer les informations d’identification du domaine si vos informations d’identification actuelles ne proviennent pas du domaine. Cliquez sur **Modifier** pour entrer les informations d’identification du domaine.  
  
Pour plus d’informations sur la création d’un domaine, voir [installer un nouveau Windows Server 2012 Active Directory niveau de domaine &#40;enfant ou&#41;d’arborescence 200](../../ad-ds/deploy/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-.md).  
  
Les options suivantes apparaissent lorsque vous ajoutez un nouveau contrôleur de domaine à un domaine existant.  
  
![AD DS installer](./media/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions/ADDS_SMI_DeploymentConfiguration_Replica.gif)  
  
-   Cliquez sur **Sélectionner** pour accéder au domaine ou tapez un nom de domaine valide.  
  
-   Le cas échéant, le Gestionnaire de serveur vous invite à entrer des informations d’identification valides. Pour installer un contrôleur de domaine supplémentaire, vous devez être membre du groupe Admins du domaine.  
  
    En outre, l’installation du premier contrôleur de domaine qui exécute Windows Server 2012 dans une forêt nécessite des informations d’identification qui incluent des appartenances aux groupes dans les groupes Administrateurs de l’entreprise et administrateurs du schéma. L’Assistant Configuration des services de domaine Active Directory vous avertit par la suite si vos informations d’identification actuelles ne comptent pas les autorisations ou les appartenances aux groupes appropriées.  
  
Pour plus d’informations sur l’ajout d’un contrôleur de domaine à un domaine existant, voir [installer un contrôleur de domaine Windows Server 2012 de réplication dans un domaine &#40;existant au niveau 200&#41;](../../ad-ds/deploy/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-.md).  
  
## <a name="domain-controller-options"></a><a name="BKMK_DCOptionsPage"></a>Options du contrôleur de domaine  
Si vous créez une forêt, la page Options du contrôleur de domaine recense les options suivantes :  
  
![AD DS installer](media/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions/ADDS_SMI_DCOptions_Forest.gif)  
  
-   Les niveaux fonctionnels de forêt et de domaine sont définis sur Windows Server 2012 par défaut.  
  
    Une nouvelle fonctionnalité est disponible au niveau fonctionnel du domaine Windows Server 2012 : la prise en charge de la stratégie de modèle d’administration KDC de l’Access Control dynamique et du blindage Kerberos a deux paramètres (toujours fournir des revendications et rejeter les demandes d’authentification non blindées) qui nécessitent le niveau fonctionnel de domaine Windows Server 2012. Pour plus d’informations, consultez « prise en charge des revendications, de l’authentification composée et du blindage Kerberos » dans [Nouveautés de l’authentification Kerberos](https://technet.microsoft.com/library/hh831747.aspx).    
    Le niveau fonctionnel de la forêt Windows Server 2012 ne fournit aucune nouvelle fonctionnalité, mais il garantit que tout nouveau domaine créé dans la forêt fonctionne automatiquement au niveau fonctionnel du domaine Windows Server 2012. Le niveau fonctionnel de domaine Windows Server 2012 ne fournit pas de nouvelles fonctionnalités en plus de la prise en charge des Access Control dynamiques et du blindage Kerberos, mais il garantit que tous les contrôleurs de domaine du domaine exécutent Windows Server 2012. Pour plus d’informations sur les autres fonctionnalités disponibles à différents niveaux fonctionnels, voir [Présentation des niveaux fonctionnels des services de domaine Active Directory (AD DS)](../active-directory-functional-levels.md).  
  
    Au-delà des niveaux fonctionnels, un contrôleur de domaine qui exécute Windows Server 2012 fournit des fonctionnalités supplémentaires qui ne sont pas disponibles sur un contrôleur de domaine qui exécute une version antérieure de Windows Server. Par exemple, un contrôleur de domaine qui exécute Windows Server 2012 peut être utilisé pour le clonage de contrôleur de domaine virtuel, alors qu’un contrôleur de domaine qui exécute une version antérieure de Windows Server ne le peut pas.  
  
-   Le serveur DNS est sélectionné par défaut lorsque vous créez une forêt. Le premier contrôleur de domaine dans la forêt doit être un serveur de catalogue global. Il ne peut pas s’agir d’un contrôleur de domaine en lecture seule.  
  
-   Le mot de passe du mode de restauration des services d’annuaire (DSRM) est requis pour ouvrir une session sur un contrôleur de domaine où les services AD DS ne s’exécutent pas. Le mot de passe que vous spécifiez doit respecter la stratégie de mot de passe appliquée au serveur. Celle-ci, par défaut, ne requiert pas un mot de passe fort, mais seulement un mot de passe non vide. Choisissez toujours un mot de passe fort complexe ou, de préférence, une phrase secrète. Pour plus d’informations sur la façon synchroniser le mot de passe DSRM avec le mot de passe d’un compte utilisateur de domaine, voir l’[l’article 961320 de la Base de connaissances Microsoft](https://support.microsoft.com/kb/961320).  
  
Pour plus d’informations sur la création d’une forêt, voir [installer un nouveau Windows Server 2012 Active Directory &#40;au niveau&#41;de la forêt 200](../../ad-ds/deploy/../../ad-ds/deploy/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-.md).  
  
Si vous créez un domaine enfant, la page Options du contrôleur de domaine recense les options suivantes :  
  
![AD DS installer](media/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions/ADDS_SMI_DCOptions_Child.gif)  
  
-   Le niveau fonctionnel du domaine est défini sur Windows Server 2012 par défaut. Vous pouvez spécifier n’importe quelle autre valeur tant que celle-ci est au moins égale à la valeur du niveau fonctionnel de la forêt.  
  
-   Parmi les options du contrôleur de domaine configurables figurent **Serveur DNS** et **Catalogue global**. Notez que vous ne pouvez pas configurer un contrôleur de domaine en lecture seule comme premier contrôleur de domaine dans un nouveau domaine.  
  
    Microsoft recommande que tous les contrôleurs de domaine fournissent des services DNS et de catalogue global à des fins de haute disponibilité dans les environnements distribués. Ces options sont donc activées par défaut dans l’Assistant lors de la création d’un domaine.  
  
-   La page **Options du contrôleur de domaine** vous permet également de choisir le **nom de site** logique Active Directory approprié à partir de la configuration de la forêt. Par défaut, le sous-réseau le mieux adapté est sélectionné. S’il n’y a qu’un site, celui-ci est automatiquement sélectionné.  
  
    > [!IMPORTANT]  
    > Si le serveur n’appartient pas à un sous-réseau Active Directory et que plusieurs sites existent, rien n’est sélectionné et le bouton **Suivant** n’apparaît que lorsque vous choisissez un site dans la liste.  
  
Pour plus d’informations sur la création d’un domaine, voir [installer un nouveau Windows Server 2012 Active Directory niveau de domaine &#40;enfant ou&#41;d’arborescence 200](../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-.md).  
  
Si vous ajoutez un contrôleur de domaine à un domaine, la page Options du contrôleur de domaine recense les options suivantes :  
  
![AD DS installer](media/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions/ADDS_SMI_DCOptions_Replica.gif)  
  
-   Parmi les options du contrôleur de domaine configurables figurent **Serveur DNS**, **Catalogue global** et **Contrôleur de domaine en lecture seule**.  
  
    Microsoft recommande que tous les contrôleurs de domaine fournissent des services DNS et de catalogue global à des fins de haute disponibilité dans les environnements distribués. Ces options sont donc activées par défaut dans l’Assistant. Pour plus d’informations sur le déploiement de contrôleurs de domaine en lecture seule, voir le [guide de planification et de déploiement relatif aux contrôleurs de domaine en lecture seule](https://technet.microsoft.com/library/cc771744(v=WS.10).aspx).  
  
Pour plus d’informations sur l’ajout d’un contrôleur de domaine à un domaine existant, voir [installer un contrôleur de domaine Windows Server 2012 de réplication dans un domaine &#40;existant au niveau 200&#41;](../../ad-ds/deploy/../../ad-ds/deploy/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-.md).  
  
## <a name="dns-options"></a><a name="BKMK_DNSOptionsPage"></a>Options DNS  
Si vous installez Serveur DNS, la page **Options DNS** suivante s’affiche :  
  
![AD DS installer](media/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions/ADDS_SMI_DNSOptions_Replica.gif)  
  
Lorsque vous installez Serveur DNS, des enregistrements de délégation qui pointent vers le serveur DNS comme faisant autorité pour la zone doivent être créés dans la zone DNS (Domain Name System) parente. Les enregistrements de délégation transfèrent l’autorité en matière de résolution de noms et fournissent une référence correcte aux autres serveurs et clients DNS pour les nouveaux serveurs faisant autorité pour la nouvelle zone. Il s’agit des enregistrements de ressources suivants :  
  
-   Un enregistrement de serveur de noms (NS) pour rendre la délégation effective. Cet enregistrement de ressource annonce que le serveur nommé ns1.na.example.microsoft.com est un serveur faisant autorité pour le sous-domaine délégué.  
  
-   Un enregistrement de ressource hôte (A ou AAAA) également appelé « enregistrement de Glue » doit être présent pour résoudre le nom du serveur spécifié dans l’enregistrement de ressource de serveur de noms (NS) sur son adresse IP. Le processus consistant à résoudre le nom de l’hôte dans cet enregistrement de ressource par le serveur DNS délégué dans l’enregistrement de serveur de noms est parfois appelé « recherche d’enregistrements de type glue ».  
  
Vous pouvez configurer l’Assistant Configuration des services de domaine Active Directory de manière à ce qu’il les crée automatiquement. L’Assistant vérifie que les enregistrements appropriés existent dans la zone DNS parente une fois que vous avez cliqué sur **Suivant** dans la page **Options du contrôleur de domaine**. Si l’Assistant ne peut pas vérifier que les enregistrements existent dans le domaine parent, l’Assistant vous donne l’option de créer automatiquement une délégation DNS pour un nouveau domaine (ou de mettre à jour la délégation existante) avant de poursuivre l’installation du nouveau contrôleur de domaine.  
  
Vous pouvez également créer ces enregistrements de délégation DNS avant d’installer le serveur DNS. Pour créer une délégation de zone, ouvrez le **Gestionnaire DNS**, cliquez avec le bouton droit sur le domaine parent, puis cliquez sur **Nouvelle délégation**. Suivez les étapes de l’Assistant Nouvelle délégation pour créer la délégation.  
  
Le processus d’installation tente de créer la délégation pour vérifier que les ordinateurs des autres domaines peuvent résoudre les requêtes DNS pour les hôtes, notamment les contrôleurs de domaine et les ordinateurs membres, dans le sous-domaine DNS. Notez que les enregistrements de délégation ne peuvent être automatiquement créés que sur des serveurs DNS Microsoft. Si la zone de domaine DNS parente réside sur des serveurs DNS tiers tels que BIND, un avertissement vous informant de l’impossibilité de créer les enregistrements de délégation DNS apparaît sur la page Vérification de la configuration requise. Pour plus d’informations sur l’avertissement, voir [Problèmes connus liés à l’installation des services de domaine Active Directory (AD DS)](https://technet.microsoft.com/library/cc754463(v=WS.10).aspx).  
  
Il est possible de créer et de valider des délégations entre le domaine parent et le sous-domaine qui est promu avant ou après l’installation. Il n’y a aucune raison de différer l’installation d’un nouveau contrôleur de domaine si vous ne pouvez pas créer ou mettre à jour la délégation DNS.  
  
Pour plus d’informations sur la délégation, voir présentation de la [délégation de zone](https://go.microsoft.com/fwlink/?LinkId=164773) (https://go.microsoft.com/fwlink/?LinkId=164773). Si la délégation de zone n’est pas possible dans votre situation, vous pouvez envisager d’autres méthodes pour assurer la résolution de noms à partir d’autres domaines aux hôtes de votre domaine. Par exemple, l’administrateur DNS d’un autre domaine pourrait configurer des zones de stub de transfert conditionnel, ou des zones secondaires pour résoudre des noms dans votre domaine. Pour plus d'informations, voir les rubriques suivantes :  
  
-   [Fonctionnement des types de zone](https://go.microsoft.com/fwlink/?LinkID=157399) (https://go.microsoft.com/fwlink/?LinkID=157399)  
  
-   [Fonctionnement des zones de stub](https://go.microsoft.com/fwlink/?LinkId=164776) (https://go.microsoft.com/fwlink/?LinkId=164776)  
  
-   [Fonctionnement des redirecteurs](https://go.microsoft.com/fwlink/?LinkId=164778) (https://go.microsoft.com/fwlink/?LinkId=164778)  
  
## <a name="rodc-options"></a><a name="BKMK_RODCOptionsPage"></a>Options RODC  
Les options suivantes apparaissent lorsque vous installez un contrôleur de domaine en lecture seule (RODC).  
  
![AD DS installer](media/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions/ADDS_SMI_RODCOptions.gif)  
  
-   Les comptes d’administrateurs délégués se voient octroyer des autorisations administratives locales au contrôleur de domaine en lecture seule. Ces utilisateurs peuvent utiliser des privilèges équivalents au groupe administrateurs de l’ordinateur local. Ils ne sont membres ni du groupe Admins du domaine ni du groupe Administrateurs intégré au domaine. Cette option est utile pour déléguer l’administration de filiales sans octroyer d’autorisations administratives au domaine. La configuration de la délégation de l’administration n’est pas requise. Pour plus d’informations, voir [Séparation des rôles d’administrateur](https://technet.microsoft.com/library/cc753170(v=WS.10).aspx).  
  
-   La stratégie de réplication de mot de passe joue le rôle de liste de contrôle d’accès. Elle détermine si un contrôleur de domaine en lecture seule est autorisé à mettre en cache un mot de passe. Lorsque le contrôleur de domaine en lecture seule reçoit une demande d’ouverture de session d’un utilisateur ou d’un d’ordinateur authentifié, il se rapporte à la Stratégie de réplication de mot de passe pour déterminer si le mot de passe pour le compte doit être mis en cache. Lors des ouvertures de session suivantes, le même compte peut alors opérer plus efficacement.  
  
    La Stratégie de réplication de mot de passe (PRP) répertorie les comptes dont les mots de passe sont autorisés à être mis en cache et les comptes pour lesquels la mise en cache des mots de passe est explicitement refusée. Même si des comptes d’utilisateurs et d’ordinateurs étant autorisés à être mis en cache figurent dans la liste, cela ne signifie pas nécessairement que le contrôleur de domaine en lecture seule a mis en cache les mots de passe pour ces comptes. Un administrateur peut, par exemple, spécifier à l’avance les comptes qu’un contrôleur de domaine en lecture seule mettra en cache. De cette façon, le contrôleur de domaine en lecture seule peut authentifier ces comptes, même si le lien WAN vers le site hub est hors connexion.  
  
    Les utilisateurs ou ordinateurs qui ne sont pas autorisés (même implicitement) ou qui font l’objet d’un refus ne mettent pas en cache leur mot de passe. Si ces utilisateurs ou ordinateurs n’ont pas accès à un contrôleur de domaine accessible en écriture, ils ne peuvent pas accéder aux ressources ou aux fonctionnalités fournies par les services AD DS. Pour plus d’informations sur la stratégie de réplication de mot de passe, voir [Stratégie de réplication de mot de passe](https://technet.microsoft.com/library/cc730883(v=ws.10).aspx). Pour plus d’informations sur la gestion de la stratégie de réplication de mot de passe, voir [Administration de la stratégie de réplication de mot de passe](https://technet.microsoft.com/library/rodc-guidance-for-administering-the-password-replication-policy(v=ws.10).aspx).  
  
Pour plus d’informations sur l’installation des [contrôleurs de domaine en lecture seule, consultez Installation d' &#40;un&#41; &#40;contrôleur de&#41;domaine Windows Server 2012 Active Directory niveau de contrôleur de domaine en lecture seule 200](../../ad-ds/deploy/RODC/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-.md).  
  
## <a name="additional-options"></a><a name="BKMK_AdditionalOptionsPage"></a>Options supplémentaires  
L’option suivante apparaît dans la page **Options supplémentaires** lorsque vous créez un domaine :  
  
![AD DS installer](media/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions/ADDS_SMI_AdditionalOptions_Child.gif)  
  
Les options suivantes apparaissent dans la page **Options supplémentaires** lorsque vous installez un contrôleur de domaine supplémentaire dans un domaine existant :  
  
![AD DS installer](media/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions/ADDS_SMI_AdditionalOptions_Replica.gif)  
  
-   Vous pouvez soit spécifier un contrôleur de domaine comme source de réplication, soit autoriser l’Assistant à choisir n’importe quel contrôleur de domaine comme source de réplication.  
  
-   Vous pouvez également choisir d’installer le contrôleur de domaine à partir d’un support sauvegardé à l’aide de l’option Installation à partir du support (IFM). Si le support d’installation est stocké localement, l’option **Installation à partir du support** vous permet d’accéder à l’emplacement du fichier. L’option Parcourir n’est pas disponible dans le cadre d’une installation à distance. Vous pouvez cliquer sur **Vérifier** pour vous assurer que le chemin d’accès fourni pointe vers un support valide. Le support utilisé par l’option IFM doit être créé avec Sauvegarde Windows Server ou Ntdsutil. exe à partir d’un autre ordinateur Windows Server 2012 existant uniquement ; vous ne pouvez pas utiliser un système d’exploitation Windows Server 2008 R2 ou antérieur pour créer un média pour un contrôleur de domaine Windows Server 2012. Si le support est protégé avec SYSKEY, le Gestionnaire de serveur vous invite à entrer le mot de passe de l’image pendant la vérification.  
  
Pour plus d’informations sur la création d’un domaine, voir [installer un nouveau Windows Server 2012 Active Directory niveau de domaine &#40;enfant ou&#41;d’arborescence 200](../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-.md). Pour plus d’informations sur l’ajout d’un contrôleur de domaine à un domaine existant, voir [installer un contrôleur de domaine Windows Server 2012 de réplication dans un domaine &#40;existant au niveau 200&#41;](../../ad-ds/deploy/../../ad-ds/deploy/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-.md).  
  
## <a name="paths"></a><a name="BKMK_Paths"></a>Trajet  
Les options suivantes apparaissent dans la page **Chemins d’accès**.  
  
![AD DS installer](media/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions/ADDS_SMI_Paths.gif)  
  
-   La page **Chemins d’accès** vous permet de remplacer les emplacements de dossier par défaut de la base de données AD DS, des journaux de transaction de base de données et du partage SYSVOL. Les emplacements par défaut sont toujours dans %systemroot%.  
  
Spécifiez l’emplacement de la base de données AD DS (NTDS.DIT), des fichiers journaux et de SYSVOL. Pour une installation locale, vous pouvez accéder à l’emplacement où vous voulez stocker les fichiers.  
  
## <a name="preparation-options"></a><a name="BKMK_AdprepCreds"></a>Options de préparation  
![AD DS installer](media/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions/ADDS_SMI_PreparationOptions.gif)  
  
Si vous n’êtes pas actuellement connecté avec des informations d’identification suffisantes pour exécuter les commandes adprep.exe et que l’exécution d’adprep est requise afin d’effectuer l’installation des services AD DS, vous êtes invité à entrer des informations d’identification pour exécuter adprep.exe. Adprep doit être exécuté pour ajouter le premier contrôleur de domaine qui exécute Windows Server 2012 à un domaine ou une forêt existant. Plus spécifiquement :  
  
-   Adprep/forestprep doit être exécuté pour ajouter le premier contrôleur de domaine qui exécute Windows Server 2012 à une forêt existante. Cette commande doit être exécutée par un membre du groupe Administrateurs de l’entreprise, du groupe Administrateurs du schéma et du groupe Admins du domaine dans le domaine qui héberge le contrôleur de schéma. Pour que cette commande s’exécute correctement, une connectivité doit être établie entre l’ordinateur sur lequel vous exécutez la commande et le contrôleur de schéma pour la forêt.  
  
-   Adprep/domainprep doit être exécuté pour ajouter le premier contrôleur de domaine qui exécute Windows Server 2012 à un domaine existant. Cette commande doit être exécutée par un membre du groupe Admins du domaine du domaine dans lequel vous installez le contrôleur de domaine qui exécute Windows Server 2012. Pour que cette commande s’exécute correctement, une connectivité doit être établie entre l’ordinateur sur lequel vous exécutez la commande et le maître d’infrastructure pour le domaine.  
  
-   Vous devez exécuter la commande adprep /rodcprep pour ajouter le premier contrôleur de domaine en lecture seule à une forêt existante. Cette commande doit être exécutée par un membre du groupe Administrateurs de l’entreprise. Pour que cette commande s’exécute correctement, une connectivité doit être établie entre l’ordinateur sur lequel vous exécutez la commande et le maître d’infrastructure pour chaque partition de l’annuaire d’applications dans la forêt.  
  
Pour plus d’informations sur adprep. exe, voir [intégration d’Adprep. exe](../../ad-ds/deploy/What-s-New-in-Active-Directory-Domain-Services-Installation-and-Removal.md#BKMK_NewAdprep) et exécution d’Adprep [. exe](https://technet.microsoft.com/library/dd464018(WS.10).aspx).  
  
## <a name="review-options"></a><a name="BKMK_ViewInstallOptionsPage"></a>Options de révision  
![AD DS installer](media/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions/ADDS_SMI_ReviewOptions.gif)  
  
-   La page **Examiner les options** vous permet de valider vos paramètres et de vérifier qu’ils répondent à vos exigences avant le démarrage de l’installation. Notez que vous avez encore la possibilité d’arrêter l’installation à l’aide du Gestionnaire de serveur. Cette page vous permet simplement d’examiner et de confirmer vos paramètres avant de poursuivre la configuration.  
  
-   La page **Examiner les options** du Gestionnaire de serveur offre également un bouton **Afficher le script** facultatif pour créer un fichier texte Unicode qui contient la configuration ADDSDeployment actuelle sous forme d’un script Windows PowerShell unique. Vous pouvez ainsi utiliser l’interface graphique Gestionnaire de serveur sous forme d’un studio de déploiement Windows PowerShell. Utilisez l’Assistant Configuration des services de domaine Active Directory pour configurer les options, exportez la configuration, puis annulez l’Assistant. Ce processus crée un exemple valide et correct du point de vue syntaxique pour permettre des modifications ultérieures ou une utilisation directe.  
  
## <a name="prerequisites-check"></a><a name="BKMK_PrerqCheckPage"></a>Vérification de la configuration requise  
![AD DS installer](media/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions/ADDS_SMI_PrerequisitesCheck.gif)  
  
Parmi les avertissements qui apparaissent sur cette page, citons les suivants :  
  
-   Les contrôleurs de domaine qui exécutent Windows Server 2008 ou une version ultérieure ont un paramètre par défaut « autoriser les algorithmes de chiffrement compatibles avec Windows NT 4 » qui empêche les algorithmes de chiffrement plus faibles lors de l’établissement de sessions de canal sécurisé. Pour plus d’informations sur l’impact potentiel et une solution de contournement, voir l’article [942564](https://support.microsoft.com/kb/942564) de la Base de connaissances.  
  
-   Impossible de créer ou de mettre à jour la délégation DNS. Pour plus d’informations, voir [Options DNS](../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions.md#BKMK_DNSOptionsPage).  
  
-   La vérification de la configuration requise nécessite des appels WMI. Ceux-ci peuvent échouer s’ils sont bloqués par un bloc de règles de pare-feu et retourner une erreur indiquant l’indisponibilité du serveur RPC.  
  
Pour plus d’informations sur les vérifications spécifiques de la configuration requise qui sont effectuées pour l’installation des services de domaine Active Directory, voir [Tests de configuration requise](../../ad-ds/manage/AD-DS-Simplified-Administration.md#BKMK_ADDSInstallPrerequisiteTests).  
  
## <a name="results"></a><a name="BKMK_Results"></a>About  
![AD DS installer](media/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions/ADDS_SMI_SMResultsBeta.gif)  
  
Dans cette page, vous pouvez examiner les résultats de l’installation.  
  
Vous pouvez également choisir de redémarrer le serveur cible une fois l’Assistant terminé ; toutefois, si l’installation réussit, le serveur redémarre toujours, et ce quel que soit l’état de cette option. Dans certains cas, lorsque l’Assistant se termine sur un serveur cible qui n’a pas été joint au domaine avant l’installation, l’état du système du serveur cible peut rendre le serveur inaccessible sur le réseau ou l’état du système peut vous empêcher d’avoir les autorisations nécessaires pour gérer le serveur distant.  
  
Si le serveur cible ne parvient pas à redémarrer dans ce cas, vous devez le redémarrer manuellement. Il est impossible de le redémarrer à l’aide d’outils tels que shutdown.exe ou Windows PowerShell. Vous pouvez utiliser les services Bureau à distance pour ouvrir une session et arrêter à distance le serveur cible.  
  
## <a name="role-removal-credentials"></a><a name="BKMK_RemovalCredsPage"></a>Informations d’identification de suppression de rôle  
![AD DS installer](media/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions/ADDS_RRW_Credentials.gif)  
  
La page **Informations d’identification** vous permet de configurer des options de rétrogradation. Entrez les informations d’identification nécessaires pour effectuer la rétrogradation à partir de la liste suivante :  
  
-   La rétrogradation d’un contrôleur de domaine supplémentaire nécessite des informations d’identification d’administrateur de domaine. Le fait **de sélectionner forcer la suppression du contrôleur de domaine** rétrograde le contrôleur de domaine sans supprimer les métadonnées de l’objet contrôleur de domaine de Active Directory.  
  
    > [!IMPORTANT]  
    > Sélectionnez cette option uniquement si le contrôleur de domaine ne parvient pas à contacter d’autres contrôleurs de domaine et qu’*aucun moyen raisonnable* ne permet de résoudre ce problème réseau. La rétrogradation forcée laisse des métadonnées orphelines dans Active Directory sur les contrôleurs de domaine restants dans la forêt. Par ailleurs, toutes les modifications non répliquées sur ce contrôleur de domaine, notamment les mots de passe ou les nouveaux comptes d’utilisateur, sont définitivement perdues. Les métadonnées orphelines sont la cause première d’un grand nombre de problèmes soumis au support technique Microsoft pour AD DS, Exchange, SQL et d’autres logiciels. Si vous rétrogradez de force un contrôleur de domaine, vous *devez* immédiatement effectuer un nettoyage manuel des métadonnées. Pour la procédure à suivre, voir [Nettoyage des métadonnées du serveur](https://technet.microsoft.com/library/cc816907(WS.10).aspx).  
  
-   La rétrogradation du dernier contrôleur de domaine dans un domaine nécessite l’appartenance au groupe Administrateurs de l’entreprise, cette opération entraînant la suppression du domaine à proprement parler (s’il s’agit du dernier domaine dans la forêt, la forêt est supprimée). Le Gestionnaire de serveur vous informe si le contrôleur de domaine actuel est le dernier contrôleur de domaine dans le domaine. Sélectionnez **Dernier contrôleur de domaine du domaine** pour confirmer que le contrôleur de domaine est bien le dernier contrôleur de domaine dans le domaine.  
  
Pour plus d’informations sur la suppression des AD DS, consultez [supprimer des Active Directory Domain Services (niveau 100)](assetId:///99b97af0-aa7e-41ed-8c81-4eee6c03eb4c) et [rétrograder les contrôleurs de domaine et les domaines &#40;au niveau 200&#41;](Demoting-Domain-Controllers-and-Domains--Level-200-.md).  
  
## <a name="ad-ds-removal-options-and-warnings"></a><a name="BKMK_RemovalOptionsPage"></a>AD DS les options de suppression et les avertissements  
Si vous avez besoin d’aide au sujet de la page Examiner les options, voir Examiner les options.  
  
Si le contrôleur de domaine héberge des rôles supplémentaires, tels que le rôle Serveur DNS ou le serveur de catalogue global, la page d’avertissement suivante apparaît :  
  
![AD DS installer](media/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions/ADDS_RRW_Warnings.gif)  
  
Avant de cliquer sur **Suivant** pour continuer, vous devez cliquer sur **Procéder à la suppression** pour confirmer que les rôles supplémentaires ne seront plus disponibles.  
  
Si vous forcez la suppression d’un contrôleur de domaine, toutes les modifications apportées à un objet Active Directory qui n’ont pas été répliquées vers d’autres contrôleurs de domaine dans le domaine seront perdues. Par ailleurs, si le contrôleur de domaine héberge des rôles de maître d’opérations, le catalogue global ou le rôle Serveur DNS, des opérations critiques dans le domaine et la forêt peuvent être touchées de la manière suivante. Avant de supprimer un contrôleur de domaine qui héberge un rôle quelconque de maître d’opérations, essayez de transférer le rôle vers un autre contrôleur de domaine. S’il n’est pas possible de transférer le rôle, supprimez d’abord les services de domaine Active Directory de cet ordinateur, puis utilisez Ntdsutil.exe pour prendre le rôle. Utilisez Ntdsutil sur le contrôleur de domaine auquel vous envisagez de prendre le rôle ; si possible, utilisez un partenaire de réplication récent dans le même site que ce contrôleur de domaine. Pour plus d’informations sur le transfert et la prise des rôles de maître d’opérations, voir l’[article 255504](https://go.microsoft.com/fwlink/?LinkId=80395) de la Base de connaissances Microsoft. Si l’Assistant n’est pas en mesure de déterminer si le contrôleur de domaine héberge un rôle de maître d’opérations, exécutez la commande netdom.exe pour déterminer si ce contrôleur de domaine effectue des rôles de maître d’opérations.  
  
-   Catalogue global : les utilisateurs peuvent avoir des difficultés à ouvrir une session aux domaines de la forêt. Avant de supprimer un serveur de catalogue global, assurez-vous qu’il y a suffisamment de serveurs de ce type dans cette forêt et ce site pour traiter les ouvertures de session utilisateur. Au besoin, désignez un autre serveur de catalogue global et mettez à jour les clients et les applications avec les nouvelles informations.  
  
-   Serveur DNS : toutes les données DNS qui sont stockées dans des zones intégrées à Active Directory seront perdues. Une fois les services AD DS supprimés, ce serveur DNS n’est plus en mesure d’effectuer la résolution de noms pour les zones DNS précédemment intégrées à Active Directory. Par conséquent, nous vous recommandons de mettre à jour la configuration DNS de tous les ordinateurs qui se rapportent actuellement à l’adresse IP de ce serveur DNS à des fins de résolution de noms avec l’adresse IP d’un nouveau serveur DNS.  
  
-   Maître d’infrastructure : les clients du domaine peuvent avoir des difficultés à localiser des objets dans d’autres domaines. Avant de continuer, transférez le rôle de maître d’infrastructure à un contrôleur de domaine qui n’est pas un serveur de catalogue global.  
  
-   Maître RID : des problèmes peuvent se produire lors de la création de nouveaux comptes d’utilisateur, comptes d’ordinateur et groupes de sécurité. Avant de continuer, transférez le rôle maître RID à un contrôleur de domaine dans le même domaine que ce contrôleur de domaine.  
  
-   Émulateur de contrôleur de domaine principal : les opérations qui sont effectuées par l’émulateur de contrôleur de domaine principal, notamment les mises à jour de stratégie de groupe et les réinitialisations du mot de passe pour les comptes non-AD DS, ne fonctionnent pas correctement. Avant de continuer, transférez le rôle maître d’émulateur de contrôleur de domaine principal à un contrôleur de domaine qui se trouve dans le même domaine que ce contrôleur de domaine.  
  
-   Contrôleur de schéma : vous ne pouvez plus modifier le schéma pour cette forêt. Avant de continuer, transférez le rôle de contrôleur de schéma à un contrôleur de domaine dans le domaine racine de la forêt.  
  
-   Maître d’opérations des noms de domaine : vous ne pouvez plus ajouter de domaines à cette forêt ni en supprimer. Avant de continuer, transférez le rôle de maître d’opérations des noms de domaine à un contrôleur de domaine dans le domaine racine de la forêt.  
  
-   Toutes les partitions de l’annuaire d’applications sur ce contrôleur de domaine Active Directory seront supprimées. Si un contrôleur de domaine contient le dernier réplica d’une ou plusieurs partitions de répertoire d’application, ces partitions n’existeront plus au terme de l’opération de suppression.  
  
Sachez que le domaine n’existera plus à l’issue de la désinstallation des services de domaine Active Directory du dernier contrôleur de domaine dans le domaine.  
  
Si le contrôleur de domaine est un serveur DNS qui est délégué pour héberger la zone DNS, la page suivante vous donne l’option de supprimer le serveur DNS de la délégation de zone DNS.  
  
![AD DS installer](media/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions/ADDS_RRW_RemovalOptions.gif)  
  
Pour plus d’informations sur la suppression des AD DS, consultez [supprimer des Active Directory Domain Services (niveau 100)](assetId:///99b97af0-aa7e-41ed-8c81-4eee6c03eb4c) et [rétrograder les contrôleurs de domaine et les domaines &#40;au niveau 200&#41;](Demoting-Domain-Controllers-and-Domains--Level-200-.md).  
  
## <a name="new-administrator-password"></a><a name="BKMK_NewAdminPwdPage"></a>Nouveau mot de passe administrateur  
La page **nouveau mot de passe d’administrateur** vous oblige à fournir un mot de passe pour le compte administrateur de l’ordinateur local intégré, une fois que la rétrogradation est terminée et que l’ordinateur devient un serveur membre de domaine ou un ordinateur de groupe de travail.  
  
![AD DS installer](media/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions/ADDS_RRW_NewAdminPwd.gif)  
  
Pour plus d’informations sur la suppression des AD DS, consultez [supprimer des Active Directory Domain Services (niveau 100)](assetId:///99b97af0-aa7e-41ed-8c81-4eee6c03eb4c) et [rétrograder les contrôleurs de domaine et les domaines &#40;au niveau 200&#41;](Demoting-Domain-Controllers-and-Domains--Level-200-.md).  
  
## <a name="review-options"></a><a name="BKMK_ConfirmRoleRemovalPage"></a>Options de révision  
La page **Examiner les options** vous donne la possibilité d’exporter les paramètres de configuration pour la rétrogradation vers un script Windows PowerShell de manière à automatiser des rétrogradations supplémentaires. Cliquez sur **Rétrograder** pour supprimer les services AD DS.  
  
![AD DS installer](media/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions/ADDS_RRW_ReviewOptions.gif)  
  


