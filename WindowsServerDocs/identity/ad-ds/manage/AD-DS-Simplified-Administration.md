---
ms.assetid: f74eec9a-2485-4ee0-a0d8-cce01250a294
title: Administration simplifiée AD DS
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 863e5352253d53941e64b52d1ca58d565a3aa8b1
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59890590"
---
# <a name="ad-ds-simplified-administration"></a>Administration simplifiée AD DS

>S'applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Cette rubrique explique les fonctionnalités et les avantages du déploiement du contrôleur de domaine Windows Server 2012 et administration et les différences entre le déploiement de système d’exploitation DC précédent et la nouvelle implémentation de Windows Server 2012.  
  
Windows Server 2012 a introduit la prochaine génération d’Active Directory domaine Administration simplifiée des Services et a été la plus radicale nouvelle conception de domaine depuis Windows 2000 Server. L'administration simplifiée AD DS tire ses enseignements de douze années d'Active Directory pour créer une expérience d'administration mieux prise en charge, plus souple et plus intuitive pour les architectes et administrateurs. Cela revient à créer des versions de technologies existantes ainsi qu'à étendre les fonctions des composants fournis dans Windows Server 2008 R2.  
  
L'administration simplifiée AD DS réinvente le déploiement de domaine.  
  
- Le déploiement de rôle AD DS fait maintenant partie de la nouvelle architecture de Gestionnaire de serveur et permet une installation à distance.  
- Le moteur de configuration et de déploiement des services AD DS est maintenant Windows PowerShell, même pendant l'utilisation du nouvel Assistant Configuration des services de domaine Active Directory.  
- L'extension de schéma, la préparation de la forêt et la préparation du domaine font automatiquement partie de la promotion du contrôleur de domaine et ne nécessitent plus des tâches séparées sur des serveurs spéciaux tels que le contrôleur de schéma.  
- La promotion inclut maintenant la vérification de la configuration requise qui valide la disponibilité de la forêt et du domaine pour le nouveau contrôleur de domaine, ce qui réduit le risque d'échec des promotions.  
- Le module Active Directory pour Windows PowerShell comprend maintenant des applets de commande pour la gestion de la topologie et de la réplication, un contrôle d'accès dynamique et d'autres opérations.  
- Le niveau fonctionnel de forêt Windows Server 2012 n'implémente pas de nouvelles fonctionnalités et le niveau fonctionnel de domaine est requis uniquement pour un sous-ensemble de nouvelles fonctionnalités Kerberos, ce qui décharge les administrateurs du besoin fréquent d'un environnement de contrôleur de domaine homogène.  
- Ajout de la prise en charge complète des contrôleurs de domaine virtualisés pour inclure le déploiement automatisé et la protection des restaurations.  
   - Pour plus d’informations sur les contrôleurs de domaine virtualisés, consultez [Introduction aux Services de domaine Active Directory &#40;AD DS&#41; virtualisation &#40;niveau 100&#41;](../../ad-ds/Introduction-to-Active-Directory-Domain-Services-AD-DS-Virtualization-Level-100.md).

En outre, de nombreuses améliorations ont été apportées au niveau de l'administration et de la maintenance :  

- Le Centre d'administration Active Directory inclut une Corbeille Active Directory graphique, la gestion de la stratégie de mot de passe affinée et la Visionneuse de l'historique Windows PowerShell.
- Le nouveau Gestionnaire de serveur dispose d'interfaces spécifiques aux services de domaine Active Directory pour l'analyse des performances, l'analyse des meilleures pratiques, les services critiques et les journaux des événements.  
- Les comptes de service administrés de groupe prennent en charge plusieurs ordinateurs avec les mêmes principaux de sécurité.  
- Améliorations apportées à l'émission d'identificateurs relatifs (RID, Relative Identifier) et à l'analyse pour une plus grande facilité de gestion dans les domaines Active Directory développés.  

Les services AD DS tirent parti des autres nouvelles fonctionnalités incluses dans Windows Server 2012, tels que :  

- Association de cartes réseau et DCB (Data Center Bridging)  
- Sécurité DNS et disponibilité de zone intégrée à Active Directory plus rapide après le démarrage  
- Améliorations de la fiabilité et de l'extensibilité Hyper-V  
- Déverrouillage réseau BitLocker  
- Autres modules d'administration de composants Windows PowerShell  

## <a name="adprep-integration"></a>Intégration d'ADPREP

La préparation du domaine et l'extension de schéma de la forêt Active Directory sont maintenant intégrées au processus de configuration de contrôleur de domaine. Si vous promouvez un nouveau contrôleur de domaine dans une forêt existante, le processus détecte l'état de la mise à niveau et les phases de préparation du domaine et d'extension de schéma se produisent automatiquement. L'utilisateur qui installe le premier contrôleur de domaine Windows Server 2012 doit cependant être un Administrateur de l'entreprise et Administrateur du schéma ou fournir d'autres informations d'identification valides.  
  
Adprep.exe demeure sur le DVD pour une préparation distincte du domaine et de la forêt. La version de l'outil qui figure dans Windows Server 2012 est à compatibilité descendante avec Windows Server 2008 x64 et Windows Server 2008 R2. Adprep.exe prend également en charge les commandes forestprep et domainprep à distance, tout comme les outils de configuration du contrôleur de domaine basé sur ADDSDeployment.  
  
Pour plus d’informations sur Adprep et la préparation de la forêt sur des systèmes d’exploitation précédents, voir [Exécution d’Adprep.exe (Windows Server 2008 R2)](https://technet.microsoft.com/library/dd464018(WS.10).aspx).  

## <a name="server-manager-ad-ds-integration"></a>Intégration des services de domaine Active Directory au Gestionnaire de serveur

![administration simplifiée](media/AD-DS-Simplified-Administration/ADDS_SMI_TR_Dashboard.png)  
  
Le Gestionnaire de serveur joue le rôle de Hub pour les tâches de gestion de serveur. Son aspect de style tableau de bord actualise régulièrement les vues des groupes de serveurs distants et rôles installés. Le Gestionnaire de serveur offre une gestion centralisée des serveurs locaux et distants, sans besoin d'accéder à la console.  
  
Les Services de domaine Active Directory est un de ces rôles de hub ; en exécutant le Gestionnaire de serveur sur un contrôleur de domaine ou les outils d’Administration de serveur distant sur un appareil Windows 8, vous voyez des problèmes récents et importants sur les contrôleurs de domaine dans votre forêt.  
  
Ces vues sont notamment les suivantes :  
  
- Disponibilité du serveur  
- Alertes de l'Analyseur de performances pour l'utilisation élevée de la mémoire et du processeur  
- État des services Windows spécifiques aux services de domaine Active Directory  
- Entrées d'erreur et d'avertissement récentes liées aux services d'annuaire dans le journal des événements  
- Analyse des meilleures pratiques d'un contrôleur de domaine par rapport à un ensemble de règles recommandées par Microsoft  

## <a name="active-directory-administrative-center-recycle-bin"></a>Corbeille du Centre d'administration Active Directory

![administration simplifiée](media/AD-DS-Simplified-Administration/ADDS_SMI_TR_ADAC.png)  
  
Windows Server 2008 R2 a introduit la Corbeille Active Directory, qui récupère les objets Active Directory supprimés sans restauration à partir de la sauvegarde, redémarrage des services de domaine Active Directory ni redémarrage des contrôleurs de domaine.  
  
Windows Server 2012 améliore les fonctions de restauration Windows PowerShell existantes avec une nouvelle interface graphique dans le Centre d'administration Active Directory. Cela permet aux administrateurs d'activer la Corbeille et de localiser ou de restaurer les objets supprimés dans les contextes de domaine de la forêt, le tout sans exécuter directement les applets de commande Windows PowerShell. Le Centre d'administration Active Directory et la Corbeille Active Directory utilisant toujours Windows PowerShell en coulisse, les procédures et scripts précédents sont encore utiles.  
  
Pour plus d’informations sur la Corbeille Active Directory, voir [Guide pas à pas de la corbeille Active Directory (Windows Server 2008 R2)](https://technet.microsoft.com/library/dd392261(WS.10).aspx).  
  
## <a name="active-directory-administrative-center-fine-grained-password-policy"></a>Stratégie de mot de passe affinée du Centre d'administration Active Directory

![administration simplifiée](media/AD-DS-Simplified-Administration/ADDS_SMI_TR_FGPP.png)  
  
Windows Server 2008 a introduit la stratégie de mot de passe affinée, qui permet aux administrateurs de configurer plusieurs stratégies de verrouillage de compte et de mot de passe par domaine. Les domaines disposent ainsi d'une solution flexible pour appliquer des règles de mot de passe plus ou moins restrictives, selon les utilisateurs et les groupes. Elle ne présentait aucune interface de gestionnaire et les administrateurs devaient la configurer à l'aide de Ldp.exe ou d'Adsiedit.msc. Windows Server 2008 R2 a introduit le module Active Directory pour Windows PowerShell, qui accordait aux administrateurs une interface en ligne de commande pour la stratégie de mot de passe affinée.  
  
Windows Server 2012 apporte une interface graphique à la stratégie de mot de passe affinée. Le Centre d'administration Active Directory abrite cette nouvelle boîte de dialogue, qui offre une gestion simplifiée de la stratégie de mot de passe affinée à tous les administrateurs.  
  
Pour plus d’informations sur la stratégie de mot de passe affinée, voir [Guide pas à pas relatif à la configuration des stratégies de verrouillage de compte et de mot de passe affinées (Windows Server 2008 R2)](https://technet.microsoft.com/library/cc770842(WS.10).aspx).  
  
## <a name="active-directory-administrative-center-windows-powershell-history-viewer"></a>Visionneuse de l'historique Windows PowerShell du Centre d'administration Active Directory

![administration simplifiée](media/AD-DS-Simplified-Administration/ADDS_SMI_TR_HistoryViewer.png)  
  
Windows Server 2008 R2 a introduit le Centre d'administration Active Directory, qui remplaçait l'ancien composant logiciel enfichable Utilisateurs et ordinateurs Active Directory créé dans Windows 2000. Le Centre d'administration Active Directory crée une interface d'administration graphique pour le module Active Directory pour Windows PowerShell alors tout nouveau.  
  
Alors que le module Active Directory contient plus d'une centaine d'applets de commande, le processus d'apprentissage pour un administrateur peut être difficile. Comme Windows PowerShell est fortement intégré à la stratégie de l'administration Windows, le Centre d'administration Active Directory comprend maintenant une Visionneuse qui vous permet de voir l'exécution de l'applet de commande dans l'interface graphique. Vous pouvez effectuer des recherches dans l'historique, le copier et l'effacer ainsi qu'ajouter des remarques avec une simple interface. Le but pour un administrateur est d'utiliser l'interface graphique pour créer et modifier des objets, puis de les passer en revue dans la Visionneuse de l'historique pour en savoir plus sur les scripts Windows PowerShell et modifier les exemples.  

## <a name="ad-replication-windows-powershell"></a>Réplication Active Directory avec Windows PowerShell

![administration simplifiée](media/AD-DS-Simplified-Administration/ADDS_PSNewADReplSite.png)  
  
Windows Server 2012 ajoute d'autres applets de commande de réplication Active Directory au module Active Directory pour Windows PowerShell. Elles permettent la configuration de sites, de sous-réseaux, de connexions, de liens de sites et de ponts nouveaux ou existants. Elles retournent également des métadonnées de réplication Active Directory, l'état de la réplication, la mise en file d'attente et les informations de vecteur de version de mise à jour. L'introduction des applets de commande de réplication, associée au déploiement et à d'autres applets de commande des services AD DS existantes, permet d'administrer une forêt à l'aide de Windows PowerShell uniquement. De nouvelles opportunités sont ainsi offertes aux administrateurs qui veulent configurer et gérer Windows Server 2012 sans interface graphique ; la surface d'attaque du système d'exploitation et les besoins en maintenance sont ainsi réduits. Cela est particulièrement important pendant le déploiement de serveurs sur des réseaux de haute sécurité, tels que SIPR (Secret Internet Protocol Router) et les réseaux DMZ d'entreprise.  
  
Pour plus d’informations sur la topologie et la réplication de site AD DS, voir [Informations techniques de référence sur Windows Server](https://technet.microsoft.com/library/cc739127(WS.10).aspx).  

## <a name="rid-management-and-issuance-improvements"></a>Améliorations apportées à l'émission et à la gestion RID

Active Directory Windows 2000 a introduit le maître RID, qui émet des pools d'identificateurs relatifs vers les contrôleurs de domaine pour créer des identificateurs de sécurité (SID, Security Identifier) de clients approuvés de sécurité, tels que les utilisateurs, groupes et ordinateurs.  Par défaut, cet espace RID global est limité à 2<sup>30</sup> (ou 1 073 741 823) identificateurs SID au total créés dans un domaine. Les identificateurs SID ne peuvent pas retourner au pool ni être émis à nouveau. Au fil du temps, un domaine de grande taille peut commencer à manquer d'identificateurs RID, ou des accidents peuvent aboutir à une diminution et un épuisement final des identificateurs RID inutiles.  
  
Windows Server 2012 traite un certain nombre des problèmes de gestion et d'émission RID non abordés par les clients ni par le support technique Microsoft, car les services de domaine Active Directory ont évolué depuis la création des premiers domaines Active Directory en 1999. Par exemple :  

- Des avertissements liés à la consommation d'identificateurs RID périodiques sont écrits dans le journal des événements.  
- Des événements sont consignés quand un administrateur invalide un pool RID.  
- Une limite maximale sur la taille de bloc RID de stratégie RID est maintenant appliquée.  
- Des plafonds RID artificiels sont maintenant appliqués et consignés quand l'espace RID global est faible, ce qui permet à un administrateur d'agir avant que l'espace global ne soit épuisé.
- L'espace RID global peut être augmenté d'un bit, ce qui double la taille à 2<sup>31</sup> (2 147 483 648 identificateurs SID)  

Pour plus d’informations sur les identificateurs RID et le maître RID, voir [Fonctionnement des identificateurs de sécurité](https://technet.microsoft.com/library/cc778824(WS.10).aspx).  
  
## <a name="ad-ds-role-deployment-and-management-architecture"></a>Architecture de gestion et de déploiement de rôle AD DS

Le Gestionnaire de serveur et Windows PowerShell ADDSDeployment reposent sur les principaux assemblys suivants pour les fonctions pendant le déploiement ou la gestion du rôle AD DS :  

- Microsoft.ADroles.Aspects.dll  
- Microsoft.ADroles.Instrumentation.dll  
- Microsoft.ADRoles.ServerManager.Common.dll  
- Microsoft.ADRoles.UI.Common.dll  
- Microsoft.DirectoryServices.Deployment.Types.dll  
- Microsoft.DirectoryServices.ServerManager.dll  
- Addsdeployment.psm1  
- Addsdeployment.psd1  

Les deux s'appuient sur Windows PowerShell et sa commande invoke-command distante pour l'installation et la configuration de rôle à distance.  

![administration simplifiée](media/AD-DS-Simplified-Administration/ADDS_SMI_TR_DepDLLs.png)  

Windows Server 2012 refactorise également certaines opérations de promotion précédentes hors de LSASS.EXE, dans le cadre :  

- du service de serveur de rôles du service d'annuaire (DsRoleSvc) ;  
- de DSRoleSvc.dll (chargé par le service DsRoleSvc).  

Ce service doit être présent et en cours d'exécution pour promouvoir, rétrograder ou cloner des contrôleurs de domaine virtuels. L'installation de rôle AD DS ajoute ce service et définit le type de démarrage Manuel par défaut. Ne désactivez pas ce service.  

## <a name="adprep-and-prerequisite-checking-architecture"></a>Architecture de vérification de la configuration requise et ADPrep

Il n'est plus nécessaire d'exécuter Adprep sur le contrôleur de schéma. Il peut être exécuté à distance à partir d'un ordinateur qui exécute Windows Server 2008 x64 ou version ultérieure.  
  
> [!NOTE]  
> Adprep utilise LDAP pour importer des fichiers Schxx.ldf et ne se reconnecte pas automatiquement si la connexion au contrôleur de schéma est perdue pendant l'importation. Dans le cadre du processus d'importation, le contrôleur de schéma est défini dans un mode spécifique et la reconnexion automatique est désactivée, car elle n'est pas rétablie dans le mode spécifique si LDAP se reconnecte une fois la connexion perdue. Dans ce cas, le schéma n'est pas correctement mis à jour.  
  
La vérification de la configuration requise permet de garantir que certaines conditions sont remplies. Ces conditions sont requises pour la réussite de l'installation des services de domaine Active Directory. Si certaines conditions requises ne sont pas remplies, elles peuvent être résolues avant de poursuivre l'installation. Cette vérification détecte également si une forêt ou un domaine ne sont pas encore préparés pour que le code de déploiement Adprep soit automatiquement exécuté.  

### <a name="adprep-executables-dlls-ldfs-files"></a>Exécutables ADPrep, DLL, LDF, fichiers

- ADprep.dll  
- Ldifde.dll  
- Csvde.dll  
- Sch14.ldf - Sch56.ldf  
- Schupgrade.cat  
- *dcpromo.csv  

Le code de préparation Active Directory auparavant hébergé dans ADprep.exe est refactorisé en adprep.dll. Cela permet à la fois à ADPrep.exe et au module Windows PowerShell ADDSDeployment d'utiliser la bibliothèque pour les mêmes tâches et d'avoir les mêmes fonctions. Adprep.exe est inclus avec le support d'installation, mais les processus automatisés ne l'appellent pas directement ; seul un administrateur l'exécute manuellement. Il ne peut être exécuté que sur les systèmes d'exploitation Windows Server 2008 x64 et ultérieurs. Ldifde.exe et csvde.exe disposent également de versions refactorisées en tant que DLL chargées par le processus de préparation. L'extension de schéma utilise toujours les fichiers LDF dont la signature est vérifiée, comme dans les versions précédentes du système d'exploitation.  
  
![administration simplifiée](media/AD-DS-Simplified-Administration/ADDS_SMI_TR_AdprepDLLs.png)  
  
> [!IMPORTANT]  
> Il n'existe pas d'outil Adprep32.exe 32 bits pour Windows Server 2012. Vous devez disposer d'au moins un ordinateur Windows Server 2008 x64, Windows Server 2008 R2 ou Windows Server 2012 qui fonctionne en tant que contrôleur de domaine, serveur membre ou dans un groupe de travail pour préparer la forêt et le domaine. Adprep.exe ne s'exécute pas sur Windows Server 2003 x64.  
  
## <a name="BKMK_PrereuisiteChecking"></a>La vérification des prérequis

Le système de vérification de la configuration requise intégré au code managé Windows PowerShell ADDSDeployment fonctionne dans différents modes, selon l'opération. Les tableaux ci-dessous décrivent chaque test et son utilisation, et fournissent une description du mode de validation et des éléments validés. Ces tableaux peuvent être utiles si vous rencontrez des problèmes quand la validation échoue et que l'erreur ne suffit pas à résoudre le problème.  
  
Ces tests sont consignés dans le canal du journal des événements opérationnel **DirectoryServices-Deployment** sous la catégorie de tâche **Core**, toujours en tant qu'ID d'événement **103**.  
  
### <a name="prerequisite-windows-powershell"></a>Configuration requise Windows PowerShell

Il existe des applets de commande Windows PowerShell ADDSDeployment pour toutes les applets de commande de déploiement de contrôleur de domaine. Elles ont approximativement les mêmes arguments que leurs applets de commande associées.  

- Test-ADDSDomainControllerInstallation  
- Test-ADDSDomainControllerUninstallation  
- Test-ADDSDomainInstallation  
- Test-ADDSForestInstallation  
- Test-ADDSReadOnlyDomainControllerAccountCreation  

D'ordinaire, il n'est pas nécessaire d'exécuter ces applets de commande ; elles s'exécutent déjà automatiquement avec les applets de commande de déploiement par défaut.  

#### <a name="BKMK_ADDSInstallPrerequisiteTests"></a>Tests de configuration requise

||||  
|-|-|-|  
|Nom du test|Protocoles<br /><br />utilisés|Description et remarques|  
|VerifyAdminTrusted<br /><br />ForDelegationProvider|LDAP|Valide que vous disposez du privilège « Permettre à l'ordinateur et aux comptes d'utilisateurs d'être approuvés pour la délégation » (SeEnableDelegationPrivilege) sur le contrôleur de domaine partenaire existant. L'accès à votre attribut tokenGroups construit est requis.<br /><br />Ce test n'est pas utilisé pour les communications avec les contrôleurs de domaine Windows Server 2003. Vous devez manuellement confirmer ce privilège avant la promotion|  
|VerifyADPrep<br /><br />Conditions préalables (forêt)|LDAP|Détecte et contacte le contrôleur de schéma à l'aide de l'attribut namingContexts rootDSE et de l'attribut fsmoRoleOwner du contexte de nommage de schéma. Identifie les opérations préparatoires (forestprep, domainprep ou rodcprep) requises pour l'installation des services AD DS. Valide que l'attribut objectVersion du schéma est prévu et vérifie si une extension supplémentaire est requise.|  
|VerifyADPrep<br /><br />Conditions préalables (domaine et contrôleur de domaine en lecture seule)|LDAP|Détecte et contacte le maître d'infrastructure à l'aide de l'attribut namingContexts rootDSE et de l'attribut fsmoRoleOwner du conteneur d'infrastructure. Dans le cas de l'installation d'un contrôleur de domaine en lecture seule, ce test détecte le maître d'opérations des noms de domaine et s'assure qu'il est en ligne.|  
|CheckGroup<br /><br />Membership|LDAP,<br /><br />RPC sur SMB (LSARPC)|Valide que l'utilisateur est membre du groupe Admins du domaine ou Administrateurs de l'entreprise, en fonction de l'opération (Admins du domaine pour l'ajout ou la rétrogradation d'un contrôleur de domaine, Administrateurs de l'entreprise pour l'ajout ou la suppression d'un domaine)|  
|CheckForestPrep<br /><br />GroupMembership|LDAP,<br /><br />RPC sur SMB (LSARPC)|Valide que l'utilisateur est membre des groupes Administrateurs de l'entreprise et Administrateurs du schéma, et qu'il dispose du privilège Gérer le journal d'audit et de la sécurité (SesScurityPrivilege) sur les contrôleurs de domaine existants|  
|CheckDomainPrep<br /><br />GroupMembership|LDAP,<br /><br />RPC sur SMB (LSARPC)|Valide que l'utilisateur est membre du groupe Admins du domaine et qu'il dispose du privilège Gérer le journal d'audit et de la sécurité (SesScurityPrivilege) sur les contrôleurs de domaine existants|  
|CheckRODCPrep<br /><br />GroupMembership|LDAP,<br /><br />RPC sur SMB (LSARPC)|Valide que l'utilisateur est membre du groupe Administrateurs de l'entreprise et qu'il dispose du privilège Gérer le journal d'audit et de la sécurité (SesScurityPrivilege) sur les contrôleurs de domaine existants|  
|VerifyInitSync<br /><br />AfterReboot|LDAP|Valide que le contrôleur de schéma a été répliqué au moins une fois depuis son redémarrage en définissant une valeur factice sur l'attribut rootDSE becomeSchemaMaster|  
|VerifySFUHotFix<br /><br />Applied|LDAP|Valide que le schéma de la forêt existant ne contient pas l'extension SFU2 de problème connu pour l'attribut UID avec l'OID 1.2.840.113556.1.4.7000.187.102<br /><br />([https://support.microsoft.com/kb/821732](https://support.microsoft.com/kb/821732))|  
|VerifyExchange<br /><br />SchemaFixed|LDAP, WMI, DCOM, RPC|Valider la forêt existante n’est pas le cas du schéma contiennent toujours problème Exchange 2000 extensions ms-Exch-Assistant-Name, ms-Exch-LabeledURI et ms-Exch-House-Identifier ([https://support.microsoft.com/kb/314649](https://support.microsoft.com/kb/314649))|  
|VerifyWin2KSchema<br /><br />Consistency|LDAP|Valide que le schéma de la forêt existant a des attributs et classes de base cohérents (qui ne sont pas modifiés de façon incorrecte par un tiers).|  
|DCPromo|DRSR sur RPC,<br /><br />LDAP,<br /><br />DNS<br /><br />RPC sur SMB (SAMR)|Valide la syntaxe de ligne de commande passée au code de promotion et à la promotion de test. Valide que la forêt ou le domaine n'existe pas déjà si vous en créez un.|  
|VerifyOutbound<br /><br />ReplicationEnabled|LDAP, DRSR sur SMB, RPC sur SMB (LSARPC)|Valide que la réplication sortante du contrôleur de domaine existant spécifié comme partenaire de réplication est activée en vérifiant l'attribut des options de l'objet Paramètres NTDS pour NTDSDSA_OPT_DISABLE_OUTBOUND_REPL (0x00000004)|  
|VerifyMachineAdmin<br /><br />Mot de passe|DRSR sur RPC,<br /><br />LDAP,<br /><br />DNS<br /><br />RPC sur SMB (SAMR)|Valide que le mot de passe du mode sans échec défini pour DSRM répond aux exigences de complexité pour les domaines.|  
|VerifySafeModePassword|*N/A*|Valide que le mot de passe d'administrateur local défini répond aux exigences de complexité de la stratégie de sécurité de l'ordinateur.|  
