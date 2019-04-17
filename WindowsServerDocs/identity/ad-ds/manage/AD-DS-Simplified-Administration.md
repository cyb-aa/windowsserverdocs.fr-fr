---
ms.assetid: f74eec9a-2485-4ee0-a0d8-cce01250a294
title: "Administration simplifiée ADDS"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 6232e281c47f3b5b4627bc9d8ccf53269aafc390
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="ad-ds-simplified-administration"></a>Administration simplifiée ADDS

>S’applique à: Windows Server2016, Windows Server2012R2, Windows Server2012

Cette rubrique décrit les nouvelles fonctionnalités et les avantages de déploiement de contrôleur de domaine Windows Server2012 et administration et les différences entre précédent déploiement de système d’exploitation du contrôleur de domaine et la nouvelle implémentation de Windows Server2012.  
  
Windows Server2012 introduit la nouvelle génération d’ActiveDirectory domaine Administration simplifiée des Services et est la plus radicale domaine nouveau considération depuis Windows2000 Server. Administration simplifiée ADDS tire ses enseignements de douze années d’ActiveDirectory et rend une expérience d’administration plus intuitive plus pris en charge, plus souple pour les architectes et administrateurs. Cela revient à créer de nouvelles versions des technologies existantes ainsi qu’à étendre les fonctionnalités des composants fournis dans Windows Server2008R2.  
  
Administration simplifiée ADDS réinvente le déploiement de domaine.  
  
-   Déploiement de rôle ADDS fait maintenant partie de la nouvelle architecture de gestionnaire de serveur et permet l’installation à distance  
  
-   Le moteur de déploiement et la configuration des services ADDS est maintenant Windows PowerShell, même si vous utilisez l’Assistant de Configuration nouvelle ADDS  
  
-   Extension du schéma, préparation de la forêt et la préparation du domaine font automatiquement partie de promotion du contrôleur de domaine et ne nécessitent plus des tâches séparées sur des serveurs spéciaux tels que le contrôleur de schéma  
  
-   La promotion inclut maintenant vérification de la configuration requise qui valide la disponibilité de la forêt et du domaine pour le nouveau contrôleur de domaine, ce qui réduit le risque d’échec des promotions.  
  
-   Module ActiveDirectory pour Windows PowerShell inclut désormais des applets de commande pour la gestion de topologie de réplication, le contrôle d’accès dynamique et d’autres opérations  
  
-   La forêt de Windows Server2012 fonctionnel niveau n’implémente pas de nouvelles fonctionnalités et le niveau fonctionnel du domaine est requis uniquement pour un sous-ensemble de nouvelles fonctionnalités Kerberos, ce qui décharge les administrateurs de fréquent besoin d’un environnement de contrôleur de domaine homogène  
  
-   Prise en charge complète de contrôleurs de domaine virtualisés pour inclure la protection de déploiement et à la restauration automatique  
  
Pour plus d’informations sur les contrôleurs de domaine virtualisés, voir [Introduction aux Services de domaine ActiveDirectory et #40; les services ADDS et #41; la virtualisation et #40; niveau 100 et #41; ](../../ad-ds/Introduction-to-Active-Directory-Domain-Services-AD-DS-Virtualization-Level-100.md).  
  
En outre, il existe de nombreuses d’administration et de maintenance:  
  
-   Le centre d’administration ActiveDirectory inclut un graphique la Corbeille ActiveDirectory, la gestion de la stratégie de mot de passe affinée et la visionneuse de l’historique de Windows PowerShell  
  
-   Le nouveau gestionnaire de serveur dispose d’interfaces spécifiques à ADDS dans l’analyse des performances, analyse des meilleures pratiques, les services critiques et les journaux des événements  
  
-   Comptes de Service administrés groupe prennent en charge plusieurs ordinateurs à l’aide des même principaux de sécurité  
  
-   Améliorations d’émission d’identificateurs relatifs (RID) et de surveillance pour une meilleure gérabilité dans des domaines ActiveDirectory matures  
  
Enfin, les services ADDS tirent parti des autres nouvelles fonctionnalités incluses dans Windows Server2012, telles que:  
  
-   Association de cartes réseau et le pontage de centre de données  
  
-   Sécurité DNS et la disponibilité de zone intégrée à ActiveDirectory plus rapide après le démarrage  
  
-   Améliorations de la fiabilité et l’extensibilité de Hyper-V  
  
-   Déverrouillage réseau BitLocker  
  
-   Modules d’administration supplémentaires du composant Windows PowerShell  
  
## <a name="technical-overview"></a>Vue d’ensemble technique  
  
### <a name="adprep-integration"></a>Intégration d’ADPREP  
Préparation ActiveDirectory forêt schéma domaine et l’extension désormais intégrer dans le processus de configuration de contrôleur de domaine. Si vous promouvez un contrôleur de domaine dans une forêt existante, le processus détecte l’état de mise à niveau et les phases de préparation du domaine et l’extension schéma se produisent automatiquement. L’utilisateur qui installe le premier contrôleur de domaine Windows Server2012 doit toujours être un administrateur d’entreprise et les administrateurs du schéma ou fournir des informations d’identification autre valides.  
  
Adprep.exe demeure sur le DVD pour la préparation du domaine et de forêt distincte. La version de l’outil inclus avec Windows Server2012 est une compatibilité descendante avec Windows Server2008 x64 et Windows Server2008R2. Adprep.exe prend également en charge à distance forestprep et domainprep, tout comme les outils de configuration de contrôleur de domaine basé sur ADDSDeployment.  
  
Pour plus d’informations sur Adprep et la préparation de la forêt précédent système d’exploitation, voir [exécution d’Adprep.exe (Windows Server2008R2)](https://technet.microsoft.com/library/dd464018(WS.10).aspx).  
  
### <a name="server-manager-ad-ds-integration"></a>Intégration de gestionnaire de serveur ADDS  
![Administration simplifiée](media/AD-DS-Simplified-Administration/ADDS_SMI_TR_Dashboard.png)  
  
Le Gestionnaire de serveur joue le rôle de hub pour les tâches de gestion de serveur. Son aspect de style tableau de bord actualise régulièrement les vues des rôles installés et des groupes de serveur distant. Le Gestionnaire de serveur offre une gestion centralisée des serveurs locaux et distants, sans avoir besoin de l’accès à la console.  
  
Les Services de domaine ActiveDirectory est un de ces rôles de hub; en exécutant le Gestionnaire de serveur sur un contrôleur de domaine ou les outils d’Administration de serveur distant sur un Windows8, vous consultez des problèmes récents et importants sur les contrôleurs de domaine dans votre forêt.  
  
Ces vues sont notamment:  
  
-   Disponibilité du serveur  
  
-   Alertes de performances pour une utilisation élevée du processeur et mémoire  
  
-   L’état des services Windows spécifiques aux services ADDS  
  
-   Récentes liées aux Services d’annuaire avertissement et erreur entrées dans le journal des événements  
  
-   Analyse des meilleures pratiques d’un contrôleur de domaine par rapport à un ensemble de règles recommandées de Microsoft  
  
### <a name="active-directory-administrative-center-recycle-bin"></a>Corbeille du centre d’administration ActiveDirectory  
![Administration simplifiée](media/AD-DS-Simplified-Administration/ADDS_SMI_TR_ADAC.png)  
  
Windows Server2008R2 a introduit la Corbeille ActiveDirectory, qui Récupère des objets ActiveDirectory supprimés sans restauration à partir de la sauvegarde, le redémarrage du service de domaine ActiveDirectory ou le redémarrage des contrôleurs de domaine.  
  
Windows Server2012 améliore les capacités de restauration Windows PowerShell existantes avec une nouvelle interface graphique dans le centre d’administration ActiveDirectory. Cela permet aux administrateurs d’activer la Corbeille et de localiser ou de restaurer des objets supprimés dans les contextes de domaine de la forêt, le tout sans exécuter directement les applets de commande Windows PowerShell. Le centre d’administration ActiveDirectory et la Corbeille ActiveDirectory toujours utiliseront Windows PowerShell en coulisse, procédures et scripts précédents sont encore utiles.  
  
Pour plus d’informations sur ActiveDirectory [la Corbeille, voir la Corbeille ActiveDirectory Step-by-Step Guide (Windows Server2008R2)](https://technet.microsoft.com/library/dd392261(WS.10).aspx).  
  
### <a name="active-directory-administrative-center-fine-grained-password-policy"></a>Stratégie de mot de passe affinée du centre d’administration ActiveDirectory  
![Administration simplifiée](media/AD-DS-Simplified-Administration/ADDS_SMI_TR_FGPP.png)  
  
Windows Server2008 a introduit la stratégie de mot de passe affinées, ce qui permet aux administrateurs de configurer plusieurs stratégies de verrouillage de compte et de mot de passe par domaine. Cela permet à une solution flexible pour appliquer des règles de mot de passe plus ou moins restrictives, selon les utilisateurs et groupes de domaines. Il n’avait aucune interface de gestion et les nécessaires aux administrateurs de configurer à l’aide de Ldp.exe ou Adsiedit.msc. Windows Server2008R2 a introduit le module ActiveDirectory pour Windows PowerShell, qui accordait aux administrateurs une interface de ligne de commande pour spécifier.  
  
Windows Server2012 apporte une interface graphique à la stratégie de mot de passe affinée. Le centre d’administration ActiveDirectory est à l’origine de cette boîte de dialogue Nouveau, qui offre une gestion simplifiée AFFINÉES à tous les administrateurs.  
  
Pour plus d’informations sur la stratégie de mot de passe affinée, voir [mot de passe affinées ADDS et la stratégie de verrouillage de compte Step-by-Step Guide (Windows Server2008R2)](https://technet.microsoft.com/library/cc770842(WS.10).aspx).  
  
### <a name="active-directory-administrative-center-windows-powershell-history-viewer"></a>Visionneuse de l’historique de PowerShell de Windows du centre d’administration ActiveDirectory  
![Administration simplifiée](media/AD-DS-Simplified-Administration/ADDS_SMI_TR_HistoryViewer.png)  
  
Windows Server2008R2 a introduit le centre d’administration ActiveDirectory, qui remplaçait l’ancien Active Active utilisateurs et ordinateurs enfichable créé dans Windows2000. Le centre d’administration ActiveDirectory crée une interface d’administration graphique au alors tout nouveau module ActiveDirectory pour Windows PowerShell.  
  
Alors que le module ActiveDirectory contient plus d’une centaine applets de commande, la courbe d’apprentissage pour un administrateur peut être difficile. Dans la mesure où Windows PowerShell est fortement intégré à la stratégie d’administration de Windows, le centre d’administration ActiveDirectory comprend maintenant une visionneuse qui vous permet de voir l’exécution de l’applet de commande dans l’interface graphique. Vous pouvez rechercher, copier, effacer l’historique et ajouter des notes avec une interface simple. Le but est d’un administrateur d’utiliser l’interface graphique pour créer et modifier des objets et passez en revue dans la visionneuse de l’historique pour en savoir plus sur l’exécution de scripts Windows PowerShell et modifier les exemples.  
  
### <a name="ad-replication-windows-powershell"></a>Windows PowerShell de la réplication AD  
![Administration simplifiée](media/AD-DS-Simplified-Administration/ADDS_PSNewADReplSite.png)  
  
Windows Server2012 ajoute les autres applets de commande de réplication ActiveDirectory pour le module ActiveDirectory pour Windows PowerShell. Elles permettent la configuration de sites de nouveau ou existants, des sous-réseaux, les connexions, des liens de sites et ponts. Elles retournent également ActiveDirectory les métadonnées de réplication, état de la réplication, queuing et informations de vecteur de version de mise à jour. L’introduction des applets de commande de réplication - combiné avec le déploiement et d’autres applets de commande ADDS existantes - permet d’administrer une forêt à l’aide de Windows PowerShell uniquement. Cela crée de nouvelles opportunités pour les administrateurs souhaitant configurer et gérer Windows Server2012sans interface graphique, ce qui réduit la surface d’attaque du système d’exploitation puis et les besoins en maintenance. Cela est particulièrement important lors du déploiement de serveurs dans des réseaux de haute sécurité tels que Secret Internet Protocol Router SIPR () et les réseaux DMZ d’entreprise.  
  
Pour plus d’informations sur la topologie de site ADDS et la réplication, voir la [référence technique de Windows Server](https://technet.microsoft.com/library/cc739127(WS.10).aspx).  
  
### <a name="rid-management-and-issuance-improvements"></a>Gestion RID et améliorations d’émission  
ActiveDirectory Windows2000 a introduit le maître RID, qui émet des pools d’identificateurs relatifs aux contrôleurs de domaine afin de créer des identificateurs de sécurité (SID) de clients approuvés de sécurité telles que les utilisateurs, groupes et des ordinateurs.  Par défaut, cet espace RID global est limité à 2<sup>30</sup> (ou 1073 741 823) identificateurs SID au total créés dans un domaine. SID ne peut pas retourner au pool ou réémettre. Au fil du temps, un domaine de grande taille peut commencer à manquer d’identificateurs RID, ou accidents peuvent aboutir à l’épuisement de RID inutile et un épuisement final.  
  
Windows Server2012 traite un certain nombre de problèmes d’émission et la gestion RID non traités par les clients et le support technique Microsoft en tant que les services ADDS ont évolué depuis la création des première les domaines ActiveDirectory en 1999. Ceux-ci sont les suivantes:  
  
-   Avertissements de la consommation de RID périodiques sont écrits dans le journal des événements  
  
-   Quand un administrateur invalide un pool RID du journal des événements  
  
-   Une limite maximale sur la stratégie RID de que taille de bloc de RID est maintenant appliquée.  
  
-   Des plafonds RID artificiels sont maintenant appliquées et connectés lorsque l’espace RID global est faible, permettant à un administrateur d’agir avant que l’espace global est épuisé  
  
-   L’espace RID global peut être augmenté d’un bit, ce qui double la taille à 2<sup>31</sup> (2 147483 648identificateurs SID)  
  
Pour plus d’informations sur les identificateurs RID et le maître RID, passez en revue [fonctionnement des identificateurs de sécurité](https://technet.microsoft.com/library/cc778824(WS.10).aspx).  
  
## <a name="new-ad-ds-deployment-architecture"></a>Architecture de déploiement de nouveaux services ADDS  
  
### <a name="ad-ds-role-deployment-and-management-architecture"></a>Architecture de gestion et déploiement des services ADDS rôle  
Gestionnaire de serveur et WindowsPowerShellADDSDeployment reposent sur les principaux assemblys suivants pour la fonctionnalité lors du déploiement ou la gestion du rôle ADDS:  
  
-   Microsoft.ADroles.Aspects.dll  
  
-   Microsoft.ADroles.Instrumentation.dll  
  
-   Microsoft.ADRoles.ServerManager.Common.dll  
  
-   Microsoft.ADRoles.UI. Common.dll  
  
-   Microsoft.DirectoryServices.Deployment.Types.dll  
  
-   Microsoft.DirectoryServices.ServerManager.dll  
  
-   Addsdeployment.psm1  
  
-   Addsdeployment.psd1  
  
Les deux s’appuient sur Windows PowerShell et sa commande invoke à distance pour l’installation de rôle à distance et de configuration.  
  
![Administration simplifiée](media/AD-DS-Simplified-Administration/ADDS_SMI_TR_DepDLLs.png)  
  
Windows Server2012 refactorise également certaines opérations de promotion précédentes hors LSASS.EXE, dans le cadre de:  
  
-   Service de serveur de rôles DS (DsRoleSvc)  
  
-   De DSRoleSvc.dll (chargé par le service DsRoleSvc)  
  
Ce service doit être présent et en cours d’exécution pour promouvoir, rétrograder ou cloner des contrôleurs de domaine virtuel. Installation du rôle ADDS ajoute ce service et définit le type de démarrage est manuel, par défaut. Ne désactivez pas ce service.  
  
### <a name="adprep-and-prerequisite-checking-architecture"></a>Architecture de vérification de configuration requise et ADPrep  
Adprep n’a plus besoin en cours d’exécution sur le contrôleur de schéma. Il peut être exécutée à distance à partir d’un ordinateur qui exécute Windows Server2008 x64 ou version ultérieure.  
  
> [!NOTE]  
> Adprep utilise LDAP pour importer des fichiers Schxx.ldf et ne pas se reconnecte automatiquement si la connexion au contrôleur de schéma est perdue pendant l’importation. Dans le cadre du processus d’importation, le contrôleur de schéma est défini dans un mode spécifique et la reconnexion automatique est désactivée, car si LDAP se reconnecte une fois la connexion est perdue, la connexion rétablie ne serait pas dans le mode spécifique. Dans ce cas, le schéma ne serait pas être mis à jour correctement.  
  
La vérification de la configuration requise permet de s’assurer que certaines conditions sont remplies. Ces conditions sont requises pour réussir l’installation ADDS. Si certaines conditions requises ne sont pas remplies, elles peuvent être résolues avant de poursuivre l’installation. Cette vérification détecte également qu’une forêt ou un domaine ne sont pas encore préparés, afin que le code de déploiement Adprep s’exécute automatiquement.  
  
#### <a name="adprep-executables-dlls-ldfs-files"></a>ADPrep Exécutables, DLL, ldf, fichiers  
  
-   ADprep.dll  
  
-   Ldifde.dll  
  
-   Csvde.dll  
  
-   Sch14.ldf - Sch56.ldf  
  
-   Schupgrade.cat  
  
-   *Dcpromo.csv  
  
Le code de préparation ActiveDirectory auparavant hébergé dans ADprep.exe est refactorisé en adprep.dll. Cela permet d’ADPrep.exe et le module WindowsPowerShellADDSDeployment utiliser la bibliothèque pour les mêmes tâches et avoir les mêmes fonctions. Adprep.exe est inclus avec le support d’installation, mais des processus automatisés ne l’appellent pas directement: seul un administrateur l’exécute manuellement. Il ne peut être exécuté que sur Windows Server2008 x64 et systèmes d’exploitation ultérieurs. Ldifde.exe et csvde.exe également disposeront de versions refactorisées en tant que DLL chargées par le processus de préparation. Extension de schéma utilise toujours les fichiers LDF signature vérifiée, comme dans les versions précédentes du système d’exploitation.  
  
![Administration simplifiée](media/AD-DS-Simplified-Administration/ADDS_SMI_TR_AdprepDLLs.png)  
  
> [!IMPORTANT]  
> Il n’existe aucun outil de Adprep32.exe 32bits de Windows Server2012. Vous devez disposer d’au moins un x64 de Windows Server2008, Windows Server2008R2 ou Windows Server2012 ordinateur, comme un contrôleur de domaine, serveur membre, ou dans un groupe de travail pour préparer la forêt et le domaine. Adprep.exe n’exécute pas sur Windows Server2003 x64.  
  
#### <a name="BKMK_PrereuisiteChecking"></a>La vérification de la configuration requise  
La configuration requise la vérification système intégré au code managé WindowsPowerShellADDSDeployment fonctionne dans des modes différents, selon l’opération. Les tableaux ci-dessous décrivent chaque test, lorsqu’il est utilisé et une explication de l’et des éléments validés. Ces tableaux peuvent être utiles si vous rencontrez des problèmes lorsque la validation échoue et l’erreur ne suffit pas à résoudre le problème.  
  
Ces tests connectez-vous les **DirectoryServices-Deployment** canal du journal des événements opérationnels sous la catégorie de tâche **Core**, toujours en tant qu’ID d’événement **103**.  
  
##### <a name="prerequisite-windows-powershell"></a>Configuration requise Windows PowerShell  
Il existe des applets de commande WindowsPowerShellADDSDeployment pour toutes les applets de commande du déploiement du contrôleur de domaine. Ils ont approximativement les mêmes arguments que leurs applets de commande associées.  
  
-   Test-ADDSDomainControllerInstallation  
  
-   Test-ADDSDomainControllerUninstallation  
  
-   Test-ADDSDomainInstallation  
  
-   Test-ADDSForestInstallation  
  
-   Test-ADDSReadOnlyDomainControllerAccountCreation  
  
Il n’est pas nécessaire pour exécuter ces applets de commande, d’ordinaire; ils exécutent déjà automatiquement avec les applets de commande de déploiement par défaut.  
  
##### <a name="BKMK_ADDSInstallPrerequisiteTests"></a>Tests de configuration requise  
  
||||  
|-|-|-|  
|Nom du test|Protocoles<br /><br />utilisé|Description et remarques|  
|VerifyAdminTrusted<br /><br />ForDelegationProvider|LDAP|Valide que vous disposez du privilège «permettre des comptes utilisateur et ordinateur d’être approuvés pour la délégation» (seenabledelegationprivilege) sur le contrôleur de domaine partenaire existant. Cela nécessite un accès à votre attribut TokenGroups construit est requis.<br /><br />Pas utilisé lors du contact avec des contrôleurs de domaine Windows Server2003. Vous devez manuellement confirmer ce privilège avant la promotion|  
|VerifyADPrep<br /><br />Conditions préalables (forêt)|LDAP|Détecte et contacte le contrôleur de schéma à l’aide de l’attribut namingContexts rootDSE et l’attribut fsmoRoleOwner du contexte d’attribution de noms schéma. Détermine les opérations préparatoires (forestprep, domainprep ou rodcprep) sont requises pour l’installation des services ADDS. Valide le schéma objectVersion est prévue et si elle nécessite davantage d’extension.|  
|VerifyADPrep<br /><br />Conditions préalables (domaine et RODC)|LDAP|Détecte et contacte le maître d’Infrastructure à l’aide de l’attribut namingContexts rootDSE et l’attribut fsmoRoleOwner du conteneur Infrastructure. Dans le cas d’une installation de contrôleur de domaine, ce test détecte le maître d’attribution de noms de domaine et assurez-vous qu’il est en ligne.|  
|CheckGroup<br /><br />Appartenance|LDAP,<br /><br />RPC sur SMB (LSARPC)|Valider l’utilisateur est membre du groupe Admins du domaine ou administrateurs de l’entreprise, en fonction de l’opération (Admins pour l’ajout ou la rétrogradation d’un contrôleur de domaine, EA pour ajouter ou supprimer un domaine)|  
|CheckForestPrep<br /><br />GroupMembership|LDAP,<br /><br />RPC sur SMB (LSARPC)|Valider l’utilisateur est membre du groupe Administrateurs du schéma et administrateurs de l’entreprise regroupe a l’Audit de gérer et privilège de sécurité (Sesscurityprivilege) sur les contrôleurs de domaine existants|  
|CheckDomainPrep<br /><br />GroupMembership|LDAP,<br /><br />RPC sur SMB (LSARPC)|Valide que l’utilisateur est membre du groupe Admins du domaine et a l’Audit de gérer et privilège de sécurité (Sesscurityprivilege) sur les contrôleurs de domaine existants|  
|CheckRODCPrep<br /><br />GroupMembership|LDAP,<br /><br />RPC sur SMB (LSARPC)|Valide que l’utilisateur est membre du groupe Administrateurs de l’entreprise et a l’Audit de gérer et privilège de sécurité (Sesscurityprivilege) sur les contrôleurs de domaine existants|  
|VerifyInitSync<br /><br />AfterReboot|LDAP|Valider que le contrôleur de schéma a été répliqué au moins une fois depuis son redémarrage en définissant une valeur factice sur l’attribut rootDSE becomeSchemaMaster|  
|VerifySFUHotFix<br /><br />Appliqué|LDAP|Valider la forêt existante schéma ne contient-elle pas l’extension SFU2 de problème connu pour l’attribut UID avec l’OID 1.2.840.113556.1.4.7000.187.102<br /><br />([https://support.microsoft.com/kb/821732](https://support.microsoft.com/kb/821732))|  
|VerifyExchange<br /><br />SchemaFixed|LDAP, WMI, DCOM, RPC|Valider la forêt existante n’est pas le cas de schéma contient toujours problème Exchange2000extensions ms-Exch-Assistant-Name, ms-Exch-LabeledURI et ms-Exch-House-Identifier ([https://support.microsoft.com/kb/314649](https://support.microsoft.com/kb/314649))|  
|VerifyWin2KSchema<br /><br />Cohérence|LDAP|Valider la forêt existante du schéma a cohérente (pas incorrectement modifié par un tiers) classes et attributs de base.|  
|DCPromo|DRSR sur RPC,<br /><br />LDAP,<br /><br />DNS<br /><br />RPC sur SMB (SAMR)|Valider la syntaxe de ligne de commande passée à la promotion de code et de test de la promotion. Valider la forêt ou de domaine n’existe pas déjà si la création de nouveaux.|  
|VerifyOutbound<br /><br />Activation_de_la_réplication|LDAP, DRSR sur SMB, RPC sur SMB (LSARPC)|Valider le contrôleur de domaine existant spécifié comme le partenaire de réplication a une réplication sortante activée en vérifiant l’attribut des options de l’objet Paramètres NTDS pour NTDSDSA_OPT_DISABLE_OUTBOUND_REPL (0 x 00000004)|  
|VerifyMachineAdmin<br /><br />Mot de passe|DRSR sur RPC,<br /><br />LDAP,<br /><br />DNS<br /><br />RPC sur SMB (SAMR)|Valider le mot de passe du mode sans échec défini pour DSRM répond aux exigences de complexité de domaine.|  
|VerifySafeModePassword|*NON APPLICABLE*|Valider local administrateur ensemble répond aux ordinateur sécurité stratégie exigences de complexité.|  
  


