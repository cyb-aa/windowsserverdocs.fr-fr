---
ms.assetid: ac727bd1-a892-47ed-a7ba-439b34187d4e
title: "Installation du service d’annuaire ActiveDirectory et les Descriptions de Page d’Assistant Suppression"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: fa023398822e79ca8c3e93d44bb1e87fc9190cee
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="ad-ds-installation-and-removal-wizard-page-descriptions"></a>Installation du service d’annuaire ActiveDirectory et les Descriptions de Page d’Assistant Suppression

>S’applique à: Windows Server2016, Windows Server2012R2, Windows Server2012

Cette rubrique fournit des descriptions des contrôles sur les pages d’Assistant suivantes qui composent l’installation du rôle serveur ADDS et la suppression dans le Gestionnaire de serveur.  
  
-   [Configuration de déploiement](../../ad-ds/deploy/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions.md#BKMK_DepConfigPage)  
  
-   [Options du contrôleur de domaine](../../ad-ds/deploy/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions.md#BKMK_DCOptionsPage)  
  
-   [Options DNS](../../ad-ds/deploy/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions.md#BKMK_DNSOptionsPage)  
  
-   [Options RODC](../../ad-ds/deploy/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions.md#BKMK_RODCOptionsPage)  
  
-   [Options supplémentaires](../../ad-ds/deploy/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions.md#BKMK_AdditionalOptionsPage)  
  
-   [Chemins d’accès](../../ad-ds/deploy/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions.md#BKMK_Paths)  
  
-   [Options de préparation](../../ad-ds/deploy/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions.md#BKMK_AdprepCreds)  
  
-   [Examiner les Options](../../ad-ds/deploy/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions.md#BKMK_ViewInstallOptionsPage)  
  
-   [Vérification des conditions préalables](../../ad-ds/deploy/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions.md#BKMK_PrerqCheckPage)  
  
-   [Résultats](../../ad-ds/deploy/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions.md#BKMK_Results)  
  
-   [Informations d’identification de suppression de rôle](../../ad-ds/deploy/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions.md#BKMK_RemovalCredsPage)  
  
-   [Options de suppression de ADDS et les avertissements](../../ad-ds/deploy/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions.md#BKMK_RemovalOptionsPage)  
  
-   [Nouveau mot de passe administrateur](../../ad-ds/deploy/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions.md#BKMK_NewAdminPwdPage)  
  
-   [Confirmer les sélections de suppression de rôle](../../ad-ds/deploy/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions.md#BKMK_ConfirmRoleRemovalPage)  
  
## <a name="BKMK_DepConfigPage"></a>Configuration de déploiement  
Le Gestionnaire de serveur commence à chaque installation du contrôleur de domaine avec le **Configuration de déploiement** page. Les options disponibles et les champs obligatoires modifier sur cette page et les pages suivantes, en fonction de l’opération de déploiement que vous sélectionnez. Par exemple, si vous créez une nouvelle forêt, le **Options de préparation** page n’apparaît pas, mais c’est le cas si vous installez le premier contrôleur de domaine qui exécute Windows Server2012dans une forêt existante ou un domaine.  
  
Des tests de validation sont effectuées sur cette page et plus tard dans le cadre de la vérification. Par exemple, si vous essayez d’installer le premier contrôleur de domaine Windows Server2012dans une forêt ayant le niveau fonctionnel Windows2000, une erreur s’affiche sur cette page.  
  
Les options suivantes apparaissent lorsque vous créez une nouvelle forêt.  
  
![Installation du service d’annuaire ActiveDirectory](media/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions/ADDS_SMI_DeploymentConfiguration_Forest.gif)  
  
-   Lorsque vous créez une nouvelle forêt, vous devez spécifier un nom pour le domaine racine de forêt. Le nom de domaine de racine de forêt ne peut pas être une partie (par exemple, il doit être «contoso.com» au lieu de «contoso»). Il doit utiliser autorisées conventions d’affectation de noms de domaine DNS. Vous pouvez spécifier un nom de domaine internationaux (IDN). Pour plus d’informations sur les conventions d’affectation de noms de domaine DNS, voir [Ko909264](https://support.microsoft.com/kb/909264).  
  
-   Ne créez pas de forêts ActiveDirectory avec le même nom que votre nom DNS externe. Par exemple, si votre URL DNS Internet est http://contoso.com, vous devez choisir un autre nom pour votre forêt interne éviter les problèmes de compatibilité future. Ce nom doit être unique et peu de chances de trafic web, tels que corp.contoso.com.  
  
-   Vous devez être membre du groupe Administrateurs sur le serveur où vous souhaitez créer une nouvelle forêt.  
  
Pour plus d’informations sur la création d’une forêt, voir [installer une nouvelle forêt Windows Server2012 ActiveDirectory et #40; niveau 200 et #41;](../../ad-ds/deploy/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-.md).  
  
Les options suivantes apparaissent lorsque vous créez un nouveau domaine.  
  
![Installation du service d’annuaire ActiveDirectory](media/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions/ADDS_SMI_DeploymentConfiguration_ChildDomain.gif)  
  
> [!NOTE]  
> Si vous créez un nouveau domaine d’arborescence, vous devez spécifier le nom du domaine racine de forêt au lieu du domaine parent, mais les pages restantes de l’Assistant et les options sont les mêmes.  
  
-   Cliquez sur **sélectionnez** pour rechercher le domaine parent ou l’arborescence ActiveDirectory, ou tapez un nom de domaine ou d’arborescence parent valide. Puis tapez le nom du nouveau domaine dans **nouveau nom de domaine**.  
  
-   Domaine d’arborescence: entrez un nom de domaine racine complet valide; le nom ne peut pas être une partie et doit utiliser des exigences de nom de domaine DNS.  
  
-   Domaine enfant: fournissent un enfant valide, une partie de nom de domaine; le nom doit satisfaire les exigences de nom de domaine DNS.  
  
-   L’Assistant Configuration des Services de domaine ActiveDirectory vous demande les informations d’identification de domaine si vos informations d’identification actuelles ne proviennent pas du domaine. Cliquez sur **modification** pour fournir des informations d’identification de domaine.  
  
Pour plus d’informations sur la création d’un domaine, voir [installer le nouveau Windows Server2012 ActiveDirectory enfant ou domaine d’arborescence et #40; niveau 200 et #41;](../../ad-ds/deploy/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-.md).  
  
Les options suivantes apparaissent lorsque vous ajoutez un nouveau contrôleur de domaine à un domaine existant.  
  
![Installation du service d’annuaire ActiveDirectory](./media/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions/ADDS_SMI_DeploymentConfiguration_Replica.gif)  
  
-   Cliquez sur **sélectionnez** pour accéder au domaine, ou tapez un nom de domaine valide.  
  
-   Le Gestionnaire de serveur vous invite aux informations d’identification valides si nécessaire. Installation d’un contrôleur de domaine supplémentaire nécessite l’appartenance au groupe Admins du domaine.  
  
    En outre, l’installation du premier contrôleur de domaine qui exécute Windows Server2012dans une forêt nécessite des informations d’identification qui incluent des appartenances aux groupes dans des groupes à la fois les administrateurs de l’entreprise et administrateurs du schéma. L’Assistant Configuration des Services de domaine ActiveDirectory vous invite ultérieurement si vos informations d’identification actuelles ne disposent pas des autorisations appropriées ou les appartenances au groupe.  
  
Pour plus d’informations sur l’ajout d’un contrôleur de domaine à un domaine existant, voir [installer un contrôleur de domaine réplica Windows Server2012dans un domaine existant et #40; niveau 200 et #41;](../../ad-ds/deploy/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-.md).  
  
## <a name="BKMK_DCOptionsPage"></a>Options du contrôleur de domaine  
Si vous créez une nouvelle forêt, la page Options du contrôleur de domaine recense les options:  
  
![Installation du service d’annuaire ActiveDirectory](media/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions/ADDS_SMI_DCOptions_Forest.gif)  
  
-   Les niveaux fonctionnels de forêt et du domaine sont définies pour Windows Server2012 par défaut.  
  
    Une nouvelle fonctionnalité est disponible au niveau fonctionnel du domaine de Windows Server2012: la prise en charge pour le contrôle d’accès dynamique et la stratégie des modèles d’administration KDC de blindage Kerberos comprend deux paramètres (toujours fournir des revendications et rejeter les demandes d’authentification non blindées) qui nécessitent le niveau fonctionnel du domaine Windows Server2012. Pour plus d’informations, voir «Prise en charge des revendications, l’authentification composée et le blindage Kerberos» dans [Nouveautés de l’authentification Kerberos](https://technet.microsoft.com/library/hh831747.aspx).    
    Le niveau fonctionnel de forêt Windows Server2012 ne fournit pas de nouvelles fonctionnalités, mais il garantit que tout nouveau domaine créé dans la forêt fonctionne automatiquement au niveau fonctionnel du domaine Windows Server2012. Le niveau fonctionnel du domaine Windows Server2012 ne fournit aucune nouvelle autres fonctionnalités en regard de la prise en charge de contrôle d’accès dynamique et le blindage Kerberos, mais il garantit que tout contrôleur de domaine dans le domaine exécute Windows Server2012. Pour plus d’informations sur d’autres fonctionnalités qui sont disponibles à différents niveaux fonctionnels, voir [niveaux fonctionnels des Services de domaine d’ActiveDirectory présentation (ADDS)](../active-directory-functional-levels.md).  
  
    Au-delà des niveaux fonctionnels, un contrôleur de domaine qui exécute Windows Server2012 fournit des fonctionnalités supplémentaires qui ne sont pas disponibles sur un contrôleur de domaine qui exécute une version antérieure de Windows Server. Par exemple, un contrôleur de domaine qui exécute Windows Server2012 peut servir de clonage de contrôleur de domaine virtuel, tandis que le contrôleur de domaine qui exécute une version antérieure de Windows Server ne peut pas.  
  
-   Serveur DNS est sélectionné par défaut lorsque vous créez une nouvelle forêt. Le premier contrôleur de domaine dans la forêt doit être un serveur de catalogue global (GC), et il ne peut pas être un contrôleur de domaine seule (RODC) en lecture.  
  
-   Le mot de passe du Mode de restauration des Services annuaire (DSRM) est requis pour ouvrir une session sur un contrôleur de domaine dans lequel les services ADDS ne fonctionne pas. Le mot de passe doit respecter la stratégie de mot de passe appliquée au serveur, qui ne nécessite pas un mot de passe fort; par défaut seul un mot de passe non vide. Choisissez toujours un mot de passe fort complexe ou, de préférence, une phrase secrète. Pour plus d’informations sur comment synchroniser le mot de passe DSRM avec le mot de passe d’un compte d’utilisateur de domaine, consultez [Ko961320](https://support.microsoft.com/kb/961320).  
  
Pour plus d’informations sur la création d’une forêt, voir [installer une nouvelle forêt Windows Server2012 ActiveDirectory et #40; niveau 200 et #41;](../../ad-ds/deploy/../../ad-ds/deploy/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-.md).  
  
Si vous créez un domaine enfant, la page Options du contrôleur de domaine recense les options:  
  
![Installation du service d’annuaire ActiveDirectory](media/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions/ADDS_SMI_DCOptions_Child.gif)  
  
-   Par défaut, le niveau fonctionnel du domaine est défini à Windows Server2012. Vous pouvez spécifier une autre valeur est au moins la valeur du niveau fonctionnel de la forêt ou une version ultérieure.  
  
-   Les options du contrôleur de domaine configurables incluent **serveur DNS** et **catalogue Global**; Vous ne pouvez pas configurer le contrôleur de domaine en lecture seule comme premier contrôleur de domaine dans un nouveau domaine.  
  
    Microsoft recommande que tous les contrôleurs de domaine fournissent des services DNS et catalogue global pour la haute disponibilité dans les environnements distribués, c’est pourquoi l’Assistant permet de ces options par défaut lors de la création d’un nouveau domaine.  
  
-   Le **Options du contrôleur de domaine** page vous permet également de choisir la logique ActiveDirectory approprié **nom du site** à partir de la configuration de la forêt. Par défaut, il sélectionne le site avec le sous-réseau le mieux adapté. S’il existe un seul site, il sélectionne automatiquement ce site.  
  
    > [!IMPORTANT]  
    > Si le serveur n’appartient pas à un sous-réseau ActiveDirectory, et il existe plusieurs sites, rien n’est sélectionné et le **suivant** bouton n’est pas disponible jusqu'à ce que vous choisissez un site dans la liste.  
  
Pour plus d’informations sur la création d’un domaine, voir [installer le nouveau Windows Server2012 ActiveDirectory enfant ou domaine d’arborescence et #40; niveau 200 et #41;](../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-.md).  
  
Si vous ajoutez un contrôleur de domaine à un domaine, la page Options du contrôleur de domaine recense les options:  
  
![Installation du service d’annuaire ActiveDirectory](media/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions/ADDS_SMI_DCOptions_Replica.gif)  
  
-   Les options du contrôleur de domaine configurables incluent **serveur DNS** et **catalogue Global**, et **contrôleur de domaine en lecture seule**.  
  
    Microsoft recommande que tous les contrôleurs de domaine fournissent des services DNS et catalogue global pour la haute disponibilité dans les environnements distribués, c’est pourquoi l’Assistant permet de ces options par défaut. Pour plus d’informations sur le déploiement RODC, voir [Guide de déploiement et de planification de contrôleur de domaine en lecture seule](https://technet.microsoft.com/library/cc771744(v=WS.10).aspx).  
  
Pour plus d’informations sur l’ajout d’un contrôleur de domaine à un domaine existant, voir [installer un contrôleur de domaine réplica Windows Server2012dans un domaine existant et #40; niveau 200 et #41;](../../ad-ds/deploy/../../ad-ds/deploy/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-.md).  
  
## <a name="BKMK_DNSOptionsPage"></a>Options DNS  
Si vous installez le serveur DNS, ce qui suit **Options DNS** page s’affiche:  
  
![Installation du service d’annuaire ActiveDirectory](media/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions/ADDS_SMI_DNSOptions_Replica.gif)  
  
Lorsque vous installez le serveur DNS, les enregistrements de délégation qui pointent vers le serveur DNS comme faisant autorité pour la zone doivent être créés dans la zone de nom DNS (Domain System) parente. Les enregistrements de délégation transférer l’autorité de résolution de nom et fournissent une référence correcte aux autres serveurs et clients DNS des nouveaux serveurs qui font autorités pour la nouvelle zone. Ces enregistrements de ressources sont les suivantes:  
  
-   Un enregistrement de ressource nom serveur (NS) pour rendre la délégation effective. Cet enregistrement de ressource annonce que le serveur nommé ns1.na.example.microsoft.com est un serveur faisant autorité pour le sous-domaine délégué.  
  
-   Un enregistrement de ressource hôte (A ou AAAA) également appelé enregistrement de type glue doit être présent pour résoudre le nom du serveur qui est spécifié dans l’enregistrement de ressource de nom serveur (NS) pour son adresse IP. Le processus de résolution du nom d’hôte dans cet enregistrement de ressource pour le serveur DNS délégué dans l’enregistrement de ressource de nom du serveur de (noms NS) est parfois appelé «successives.»  
  
Vous pouvez demander à l’Assistant Configuration des Services de domaine ActiveDirectory de les créer automatiquement. L’Assistant vérifie que les enregistrements appropriés existent dans la zone DNS parente une fois que vous cliquez sur **suivant** sur le **Options du contrôleur de domaine** page. Si l’Assistant ne peut pas vérifier que les enregistrements existent dans le domaine parent, l’Assistant fournit automatiquement avec l’option pour créer une délégation DNS pour un nouveau domaine (ou mettre à jour la délégation existante) et poursuivre la nouvelle installation de contrôleur de domaine.  
  
Vous pouvez également créer ces enregistrements de délégation DNS avant d’installer le serveur DNS. Pour créer une délégation de zone, ouvrez **Gestionnaire DNS**, cliquez sur le domaine parent, puis cliquez sur **nouvelle délégation**. Suivez les étapes de l’Assistant Nouvelle délégation pour créer la délégation.  
  
Le processus d’installation tente de créer la délégation pour vérifier que les ordinateurs dans d’autres domaines peuvent résoudre les requêtes DNS pour les ordinateurs hôtes, y compris les contrôleurs de domaine et les ordinateurs membres, dans le sous-domaine DNS. Notez que les enregistrements de délégation peuvent être automatiquement créés uniquement sur les serveurs DNS Microsoft. Si la zone de domaine DNS parente réside sur des serveurs DNS tiers tels que BIND, un avertissement concernant l’impossibilité de créer des enregistrements de délégation DNS apparaît sur la page de vérification de la configuration requise. Pour plus d’informations sur l’avertissement, voir [problèmes connus pour l’installation des services ADDS](https://technet.microsoft.com/library/cc754463(v=WS.10).aspx).  
  
Délégations entre le domaine parent et le sous-domaine qui est promu peuvent être créées et validées avant ou après l’installation. Il n’existe aucune raison de différer l’installation d’un nouveau contrôleur de domaine, car vous ne pouvez pas créer ou mettre à jour la délégation DNS.  
  
Pour plus d’informations sur la délégation, voir [présentation la délégation de Zone](https://go.microsoft.com/fwlink/?LinkId=164773) (https://go.microsoft.com/fwlink/?LinkId=164773). Si la délégation de zone n’est pas possible dans votre situation, vous pouvez envisager d’autres méthodes pour assurer la résolution de noms à partir d’autres domaines aux hôtes dans votre domaine. Par exemple, l’administrateur DNS d’un autre domaine pourrait configurer transfert conditionnel, zones de stub ou secondaire zones afin de résoudre les noms de votre domaine. Pour plus d’informations, consultez les rubriques suivantes:  
  
-   [Présentation des types de zone](https://go.microsoft.com/fwlink/?LinkID=157399) (https://go.microsoft.com/fwlink/?LinkID=157399)  
  
-   [Présentation des zones de stub](https://go.microsoft.com/fwlink/?LinkId=164776) (https://go.microsoft.com/fwlink/?LinkId=164776)  
  
-   [Présentation des redirecteurs](https://go.microsoft.com/fwlink/?LinkId=164778) (https://go.microsoft.com/fwlink/?LinkId=164778)  
  
## <a name="BKMK_RODCOptionsPage"></a>Options RODC  
Les options suivantes apparaissent lorsque vous installez un contrôleur de domaine en lecture seule (RODC).  
  
![Installation du service d’annuaire ActiveDirectory](media/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions/ADDS_SMI_RODCOptions.gif)  
  
-   Comptes d’administrateur délégué obtenir des autorisations d’administrateur local sur le RODC. Ces utilisateurs peuvent fonctionner avec des privilèges équivalents au groupe Administrateurs de l’ordinateur local. Ils ne sont pas membres du groupe Admins du domaine ou le groupe Administrateurs intégré au domaine. Cette option est utile pour déléguer l’administration de filiales sans octroyer d’autorisations d’administration de domaine. Configuration de la délégation de l’administration n’est pas nécessaire. Pour plus d’informations, voir [la séparation des rôles administrateur](https://technet.microsoft.com/library/cc753170(v=WS.10).aspx).  
  
-   La stratégie de réplication de mot de passe agit comme une liste de contrôle d’accès (ACL). Il détermine si un RODC est autorisé à mettre en cache un mot de passe. Une fois le RODC reçoit une demande d’ouverture de session utilisateur ou ordinateur authentifié, il fait référence à la stratégie de réplication de mot de passe pour déterminer si le mot de passe pour le compte doit être mis en cache. Le même compte peut effectuer des ouvertures de session plus efficacement.  
  
    La stratégie de réplication de mot de passe (PRP) répertorie les comptes dont les mots de passe sont autorisés à être mis en cache et dont les mots de passe sont explicitement refusées à partir de la mise en cache. La liste des comptes d’utilisateur et ordinateur autorisés à être mis en cache n’implique pas que le RODC a mis en cache les mots de passe pour ces comptes. Un administrateur peut, par exemple, spécifier à l’avance tous les comptes une seule mettra en cache. De cette façon, le RODC peut authentifier ces comptes, même si le lien WAN vers le site hub est hors connexion.  
  
    Des utilisateurs ou des ordinateurs qui ne sont pas autorisés (y compris implicite) ou refusée ne met ne pas en cache leur mot de passe. Si ces utilisateurs ou ordinateurs n’ont pas accès à un contrôleur de domaine accessible en écriture, ils ne peuvent accéder à ADDS fournie ressources ou des fonctionnalités. Pour plus d’informations sur cette stratégie, voir [stratégie de réplication de mot de passe](https://technet.microsoft.com/library/cc730883(v=ws.10).aspx). Pour plus d’informations sur la gestion de cette stratégie, voir [administration de la stratégie de réplication de mot de passe](https://technet.microsoft.com/library/rodc-guidance-for-administering-the-password-replication-policy(v=ws.10).aspx).  
  
Pour plus d’informations sur l’installation RODC, voir [installer un Windows Server2012 ActiveDirectory Read-Only le contrôleur de domaine et #40; rODC et #41; et #40; niveau 200 et #41;](../../ad-ds/deploy/RODC/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-.md).  
  
## <a name="BKMK_AdditionalOptionsPage"></a>Options supplémentaires  
L’option suivante s’affiche sur la **Options supplémentaires** page si vous créez un nouveau domaine:  
  
![Installation du service d’annuaire ActiveDirectory](media/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions/ADDS_SMI_AdditionalOptions_Child.gif)  
  
Les options suivantes apparaissent dans le **Options supplémentaires** page si vous installez un contrôleur de domaine supplémentaire dans un domaine existant:  
  
![Installation du service d’annuaire ActiveDirectory](media/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions/ADDS_SMI_AdditionalOptions_Replica.gif)  
  
-   Vous pouvez spécifier un contrôleur de domaine comme source de réplication, ou autoriser l’Assistant à choisir n’importe quel contrôleur de domaine comme source de réplication.  
  
-   Vous pouvez également choisir d’installer le contrôleur de domaine à l’aide de support à l’aide de l’installation à partir de l’option de support (IFM) sauvegardé. Si le support d’installation est stocké localement, le **installer à partir du support** option vous permet de naviguer jusqu'à l’emplacement du fichier. L’option de navigation n’est pas disponible pour une installation à distance. Vous pouvez cliquer sur **vérifier** pour garantir le chemin d’accès fourni est un support valide. Support utilisé par l’option IFM doit être créé avec sauvegarde Windows Server ou Ntdsutil.exe à partir d’un autre Windows Server2012 ordinateur existant uniquement; pour créer un média pour un contrôleur de domaine Windows Server2012, vous ne pouvez pas utiliser un Windows Server2008R2 ou un système d’exploitation précédent. Si le média est protégé avec SYSKEY, le Gestionnaire de serveur demande un mot de passe de l’image pendant la vérification.  
  
Pour plus d’informations sur la création d’un domaine, voir [installer le nouveau Windows Server2012 ActiveDirectory enfant ou domaine d’arborescence et #40; niveau 200 et #41;](../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-.md). Pour plus d’informations sur l’ajout d’un contrôleur de domaine à un domaine existant, voir [installer un contrôleur de domaine réplica Windows Server2012dans un domaine existant et #40; niveau 200 et #41;](../../ad-ds/deploy/../../ad-ds/deploy/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-.md).  
  
## <a name="BKMK_Paths"></a>Chemins d’accès  
Les options suivantes apparaissent dans le **chemins d’accès** page.  
  
![Installation du service d’annuaire ActiveDirectory](media/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions/ADDS_SMI_Paths.gif)  
  
-   Le **chemins d’accès** page vous permet de remplacer les emplacements de dossier par défaut de la base de données ADDS, les journaux des transactions de base de données, et du partage SYSVOL. Les emplacements par défaut sont toujours dans % SystemRoot%.  
  
Spécifiez l’emplacement de la base de données ADDS (NTDS.DIT), fichiers journaux et SYSVOL. Pour une installation locale, vous pouvez accéder à l’emplacement où vous souhaitez stocker les fichiers.  
  
## <a name="BKMK_AdprepCreds"></a>Options de préparation  
![Installation du service d’annuaire ActiveDirectory](media/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions/ADDS_SMI_PreparationOptions.gif)  
  
Si vous n’êtes pas actuellement connecté avec les informations d’identification suffisantes pour exécuter des commandes adprep.exe et adprep est requise pour exécuter afin de terminer l’installation des services ADDS, vous êtes invité à fournir les informations d’identification pour exécuter adprep.exe. Adprep est requise pour exécuter afin d’ajouter le premier contrôleur de domaine qui exécute Windows Server2012 à un domaine ou une forêt existante. Plus spécifiquement:  
  
-   Adprep /forestprep doit être exécuté pour ajouter le premier contrôleur de domaine qui exécute Windows Server2012 à une forêt existante. Cette commande doit être exécutée par un membre du groupe Administrateurs de l’entreprise, le groupe Administrateurs du schéma et Admins du domaine du domaine qui héberge le contrôleur de schéma. Pour cette commande s’exécute correctement, il doit être établie entre l’ordinateur sur lequel vous exécutez la commande et le contrôleur de schéma pour la forêt.  
  
-   Adprep /domainprep doit être exécuté pour ajouter le premier contrôleur de domaine qui exécute Windows Server2012 à un domaine existant. Cette commande doit être exécutée par un membre du groupe Admins du domaine du domaine dans lequel vous installez le contrôleur de domaine qui exécute Windows Server2012. Pour cette commande s’exécute correctement, il doit être établie entre l’ordinateur sur lequel vous exécutez la commande et le maître d’infrastructure pour le domaine.  
  
-   Adprep /rodcprep doit être exécuté pour ajouter le premier RODC à une forêt existante. Cette commande doit être exécutée par un membre du groupe Administrateurs de l’entreprise. Pour cette commande s’exécute correctement, il doit être établie entre l’ordinateur sur lequel vous exécutez la commande et le maître d’infrastructure pour chaque partition d’annuaire application dans la forêt.  
  
Pour plus d’informations sur Adprep.exe, voir [intégration d’Adprep.exe](../../ad-ds/deploy/What-s-New-in-Active-Directory-Domain-Services-Installation-and-Removal.md#BKMK_NewAdprep) et voir [exécution d’Adprep.exe](https://technet.microsoft.com/library/dd464018(WS.10).aspx).  
  
## <a name="BKMK_ViewInstallOptionsPage"></a>Examiner les Options  
![Installation du service d’annuaire ActiveDirectory](media/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions/ADDS_SMI_ReviewOptions.gif)  
  
-   Le **examiner les Options** page vous permet de valider vos paramètres et assurez-vous qu’elles répondent à vos exigences avant de commencer l’installation. Cela n’est pas la possibilité d’arrêter l’installation à l’aide du Gestionnaire de serveur. Cette page vous permet simplement de vérifier et confirmer vos paramètres avant de poursuivre la configuration.  
  
-   Le **examiner les Options** page dans le Gestionnaire de serveur offre également une option **afficher le Script** bouton pour créer un fichier texte Unicode qui contient la configuration ADDSDeployment actuelle sous forme de script Windows PowerShell unique. Cela vous permet d’utiliser l’interface graphique du Gestionnaire de serveur comme un studio de déploiement de Windows PowerShell. Utilisez l’Assistant Configuration des Services de domaine ActiveDirectory pour configurer les options, exportez la configuration et annulez ensuite l’Assistant. Ce processus crée un exemple valide et la syntaxe correct pour modification supplémentaire ou une utilisation directe.  
  
## <a name="BKMK_PrerqCheckPage"></a>Vérification des conditions préalables  
![Installation du service d’annuaire ActiveDirectory](media/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions/ADDS_SMI_PrerequisitesCheck.gif)  
  
Parmi les avertissements qui apparaissent sur cette page:  
  
-   Contrôleurs de domaine qui exécutent Windows Server2008 ou version ultérieure possèdent un paramètre par défaut pour «Autoriser algorithmes de chiffrement compatibles avec Windows NT 4» qui empêche les algorithmes de chiffrement plus faibles lors de l’établissement de sessions sur canal sécurisé. Pour plus d’informations sur l’impact potentiel et une solution de contournement, voir l’article [942564](https://support.microsoft.com/kb/942564).  
  
-   La délégation DNS ne peut pas être créée ou mis à jour. Pour plus d’informations, voir [Options DNS](../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions.md#BKMK_DNSOptionsPage).  
  
-   La vérification nécessite des appels WMI. Elles peuvent échouer s’ils sont le bloc de règles de pare-feu bloqués et renvoyer un serveur RPC erreur n’est pas disponible.  
  
Pour plus d’informations sur les vérifications spécifiques qui sont effectuées pour l’installation des services ADDS, voir [Tests de configuration requise](../../ad-ds/manage/AD-DS-Simplified-Administration.md#BKMK_ADDSInstallPrerequisiteTests).  
  
## <a name="BKMK_Results"></a>Résultats  
![Installation du service d’annuaire ActiveDirectory](media/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions/ADDS_SMI_SMResultsBeta.gif)  
  
Sur cette page, vous pouvez passer en revue les résultats de l’installation.  
  
Vous pouvez également sélectionner pour redémarrer le serveur cible une fois l’Assistant terminé, mais si l’installation réussit, le serveur redémarre toujours, quelle que soit la si vous sélectionnez cette option. Dans certains cas une fois l’Assistant terminé sur un serveur cible qui n’a pas été joint au domaine avant l’installation, l’état du système du serveur cible peut rendre le serveur inaccessible sur le réseau, ou l’état du système peut vous empêcher d’avoir les autorisations nécessaires pour gérer le serveur à distance.  
  
Si le serveur cible ne parvient pas à redémarrer dans ce cas, vous devez le redémarrer manuellement. Outils tels que shutdown.exe ou Windows PowerShell ne peut pas redémarrer. Vous pouvez utiliser les Services Bureau à distance pour ouvrir une session et arrêter à distance le serveur cible.  
  
## <a name="BKMK_RemovalCredsPage"></a>Informations d’identification de suppression de rôle  
![Installation du service d’annuaire ActiveDirectory](media/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions/ADDS_RRW_Credentials.gif)  
  
Vous configurez les options de rétrogradation sur le **informations d’identification** page. Fournir les informations d’identification nécessaires pour effectuer la rétrogradation à partir de la liste suivante:  
  
-   La rétrogradation d’un contrôleur de domaine supplémentaire nécessite des informations d’identification d’administrateur de domaine. En sélectionnant **forcer la suppression du contrôleur de domaine** rétrograde le contrôleur de domaine sans supprimer les métadonnées de l’objet de contrôleur de domaine d’ActiveDirectory.  
  
    > [!IMPORTANT]  
    > Ne sélectionnez pas cette option, sauf si le contrôleur de domaine ne peut pas contacter d’autres contrôleurs de domaine et qu’il existe *aucun moyen raisonnable* pour résoudre ce problème réseau. La rétrogradation forcée laisse des métadonnées orphelines dans ActiveDirectory sur les contrôleurs de domaine restants dans la forêt. En outre, toutes les modifications non répliquées sur ce contrôleur de domaine, tels que des mots de passe ou de nouveaux comptes d’utilisateur, sont définitivement perdues. Métadonnées orphelines sont la cause racine dans un pourcentage important des incidents de Support technique Microsoft pour les services ADDS, Exchange, SQL et autres logiciels. Si vous rétrogradez un contrôleur de domaine, vous *doit* manuellement effectuer immédiatement le nettoyage des métadonnées. Pour obtenir des instructions, consultez [nettoyage des métadonnées du serveur](https://technet.microsoft.com/library/cc816907(WS.10).aspx).  
  
-   La rétrogradation du dernier contrôleur de domaine dans un domaine nécessite l’appartenance au groupe Administrateurs de l’entreprise que cette opération supprime le domaine à proprement parler (s’il s’agit du dernier domaine dans la forêt, cette opération supprime la forêt). Le Gestionnaire de serveur vous informe si le contrôleur de domaine actuel est le dernier contrôleur de domaine dans le domaine. Sélectionnez **dernier contrôleur de domaine dans le domaine** pour vérifier le domaine contrôleur est le dernier contrôleur de domaine dans le domaine.  
  
Pour plus d’informations sur la suppression des services ADDS, voir [supprimer ActiveDirectory Domain Services (niveau 100)](assetId:///99b97af0-aa7e-41ed-8c81-4eee6c03eb4c) et [la rétrogradation des contrôleurs de domaine et les domaines et #40; niveau 200 et #41;](Demoting-Domain-Controllers-and-Domains--Level-200-.md).  
  
## <a name="BKMK_RemovalOptionsPage"></a>Options de suppression de ADDS et les avertissements  
Si vous avez besoin d’aide sur la page examiner les Options, voir examiner les Options  
  
Si le contrôleur de domaine héberge des rôles supplémentaires, telles que le rôle de serveur DNS ou le serveur de catalogue global, la page d’avertissement suivante s’affiche:  
  
![Installation du service d’annuaire ActiveDirectory](media/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions/ADDS_RRW_Warnings.gif)  
  
Vous devez cliquer sur **procéder à la suppression** pour confirmer que les rôles supplémentaires ne seront plus disponibles avant que vous pouvez cliquer sur **suivant** pour continuer.  
  
Si vous forcez la suppression d’un contrôleur de domaine, les modifications de l’objet ActiveDirectory qui n’ont pas été répliquées vers d’autres contrôleurs de domaine dans le domaine seront perdues. En outre, si le contrôleur de domaine héberge des rôles de maître d’opérations, le catalogue global ou le rôle de serveur DNS, des opérations critiques dans le domaine et la forêt peuvent être affectées comme suit. Avant de supprimer un contrôleur de domaine qui héberge un rôle de maître d’opérations, essayez de transférer le rôle vers un autre contrôleur de domaine. Si elle n’est pas possible de transférer le rôle, tout d’abord supprimer les Services de domaine ActiveDirectory de cet ordinateur, puis utilisez Ntdsutil.exe pour prendre le rôle. Utilisez Ntdsutil sur le contrôleur de domaine que vous envisagez de prendre le rôle Si possible, utilisez un partenaire de réplication récent dans le même site que ce contrôleur de domaine. Pour plus d’informations sur le transfert et la prise des rôles de maître d’opérations, voir [article 255504](https://go.microsoft.com/fwlink/?LinkId=80395) dans la Base de connaissances Microsoft. Si l’Assistant ne peut pas déterminer si le contrôleur de domaine héberge un rôle de maître d’opérations, exécutez la commande netdom.exe pour déterminer si ce contrôleur de domaine effectue des rôles de maître d’opérations.  
  
-   Catalogue global: les utilisateurs peuvent avoir des difficultés à ouvrir une session sur les domaines de la forêt. Avant de supprimer un serveur de catalogue global, assurez-vous que suffisamment serveurs de catalogue global sont dans cette forêt et le site pour le service des ouvertures de session utilisateur. Si nécessaire, désignez un autre serveur de catalogue global et mettre à jour des clients et des applications avec les nouvelles informations.  
  
-   Serveur DNS: toutes les données DNS qui sont stockées dans les zones intégrées à ActiveDirectory seront perdues. Après avoir supprimé les services ADDS, ce serveur DNS ne sera pas en mesure d’effectuer la résolution de noms pour les zones DNS qui ont été intégrées à ActiveDirectory. Par conséquent, nous recommandons que vous mettez à jour la configuration DNS de tous les ordinateurs qui se rapportent actuellement à l’adresse IP de ce serveur DNS pour la résolution de noms avec l’adresse IP d’un nouveau serveur DNS.  
  
-   Maître d’infrastructure: les clients dans le domaine peuvent avoir des difficultés à localiser des objets dans d’autres domaines. Avant de continuer, transférez le rôle de maître d’infrastructure à un contrôleur de domaine qui n’est pas un serveur de catalogue global.  
  
-   Maître RID: vous devrez peut-être des problèmes de création de nouveaux comptes d’utilisateur, les comptes d’ordinateurs et les groupes de sécurité. Avant de continuer, transférez le rôle de maître RID à un contrôleur de domaine dans le même domaine que ce contrôleur de domaine.  
  
-   Émulateur de contrôleur de domaine principal: les opérations effectuées par l’émulateur de contrôleur de domaine principal, tels que les mises à jour de la stratégie de groupe et réinitialise le mot de passe pour les comptes non-ADDS, ne fonctionnent pas correctement. Avant de continuer, transférez le rôle de maître d’émulateur PDC à un contrôleur de domaine qui se trouve dans le même domaine que ce contrôleur de domaine.  
  
-   Contrôleur de schéma: vous ne serez n’est plus en mesure de modifier le schéma pour cette forêt. Avant de continuer, transférez le rôle de maître de schéma à un contrôleur de domaine dans le domaine racine de la forêt.  
  
-   Maître d’attribution de noms de domaine: vous serez n’est plus en mesure d’ajouter ou supprimer des domaines de cette forêt. Avant de continuer, transférez le rôle de maître d’un contrôleur de domaine dans le domaine racine de la forêt de noms de domaine.  
  
-   Toutes les partitions d’annuaire d’applications sur ce contrôleur de domaine ActiveDirectory seront supprimées. Si un contrôleur de domaine conserve le dernier réplica d’une ou plusieurs partitions de répertoire d’application lorsque l’opération de suppression est terminée, ces partitions n’existe plus.  
  
N’oubliez pas que le domaine n’existera plus après la désinstallation de Services de domaine ActiveDirectory du dernier contrôleur de domaine dans le domaine.  
  
Si le contrôleur de domaine est un serveur DNS qui est délégué pour héberger la zone DNS, la page suivante vous donne la possibilité de supprimer le serveur DNS à partir de la délégation de zone DNS.  
  
![Installation du service d’annuaire ActiveDirectory](media/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions/ADDS_RRW_RemovalOptions.gif)  
  
Pour plus d’informations sur la suppression des services ADDS, voir [supprimer ActiveDirectory Domain Services (niveau 100)](assetId:///99b97af0-aa7e-41ed-8c81-4eee6c03eb4c) et [la rétrogradation des contrôleurs de domaine et les domaines et #40; niveau 200 et #41;](Demoting-Domain-Controllers-and-Domains--Level-200-.md).  
  
## <a name="BKMK_NewAdminPwdPage"></a>Nouveau mot de passe administrateur  
Le **nouveau mot de passe administrateur** page vous oblige à fournir un mot de passe pour le compte d’administrateur de l’ordinateur local intégré, une fois la rétrogradation est terminée et l’ordinateur devient un serveur membre de domaine ou d’un ordinateur de groupe de travail.  
  
![Installation du service d’annuaire ActiveDirectory](media/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions/ADDS_RRW_NewAdminPwd.gif)  
  
Pour plus d’informations sur la suppression des services ADDS, voir [supprimer ActiveDirectory Domain Services (niveau 100)](assetId:///99b97af0-aa7e-41ed-8c81-4eee6c03eb4c) et [la rétrogradation des contrôleurs de domaine et les domaines et #40; niveau 200 et #41;](Demoting-Domain-Controllers-and-Domains--Level-200-.md).  
  
## <a name="BKMK_ConfirmRoleRemovalPage"></a>Examiner les Options  
Le **examiner les Options** page vous offre la possibilité d’exporter les paramètres de configuration pour la rétrogradation vers un script Windows PowerShell afin que vous pouvez automatiser des rétrogradations supplémentaires. Cliquez sur **rétrograder** pour supprimer les services ADDS.  
  
![Installation du service d’annuaire ActiveDirectory](media/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions/ADDS_RRW_ReviewOptions.gif)  
  


