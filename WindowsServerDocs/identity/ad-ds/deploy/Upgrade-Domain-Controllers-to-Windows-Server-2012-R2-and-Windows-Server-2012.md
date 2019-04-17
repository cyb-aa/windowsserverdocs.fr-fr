---
ms.assetid: e4c31187-f15f-410b-bb79-8d63e2f2b421
title: "Mettre à niveau des contrôleurs de domaine vers Windows Server2012R2 et Windows Server2012"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: e317c5a939d417bac844c4080223d7b5e0eec149
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="upgrade-domain-controllers-to-windows-server-2012-r2-and-windows-server-2012"></a>Mettre à niveau des contrôleurs de domaine vers Windows Server2012R2 et Windows Server2012

>S’applique à: Windows Server2016, Windows Server2012R2, Windows Server2012

Cette rubrique fournit des informations générales sur les Services de domaine Active Directory dans Windows Server 2012 R2 et Windows Server 2012 et décrit le processus de mise à niveau des contrôleurs de domaine à partir de Windows Server 2008 ou Windows Server 2008 R2.  
  
-   [Étapes mise à niveau de contrôleur de domaine](../../ad-ds/deploy/Upgrade-Domain-Controllers-to-Windows-Server-2012-R2-and-Windows-Server-2012.md#BKMK_UpgradeWorkflow)  
  
-   [Quelles sont les nouveautés dans Windows Server 2012?](../../ad-ds/deploy/Upgrade-Domain-Controllers-to-Windows-Server-2012-R2-and-Windows-Server-2012.md#BKMK_WhatsNewEight)  
  
-   [Quelles sont les nouveautés dans AD DS dans Windows Server 2012 R2?](../../ad-ds/deploy/Upgrade-Domain-Controllers-to-Windows-Server-2012-R2-and-Windows-Server-2012.md#BKMK_NewWS2012R2)  
  
-   [Quelles sont les nouveautés dans AD DS dans Windows Server 2012?](../../ad-ds/deploy/Upgrade-Domain-Controllers-to-Windows-Server-2012-R2-and-Windows-Server-2012.md#BKMK_WhatsNewAD)  
  
-   [Modifications d’installation de rôle de serveur AD DS](../../ad-ds/deploy/Upgrade-Domain-Controllers-to-Windows-Server-2012-R2-and-Windows-Server-2012.md#BKMK_InstallationChanges)  
  
-   [Fonctionnalités déconseillées et modifications de comportement associées aux services AD DS dans Windows Server 2012](../../ad-ds/deploy/Upgrade-Domain-Controllers-to-Windows-Server-2012-R2-and-Windows-Server-2012.md#BKMK_DeprecatedFeatures)  
  
-   [Configuration requise du système d’exploitation](../../ad-ds/deploy/Upgrade-Domain-Controllers-to-Windows-Server-2012-R2-and-Windows-Server-2012.md#BKMK_SysReqs)  
  
-   [Chemins de mise à niveau sur place pris en charge](../../ad-ds/deploy/Upgrade-Domain-Controllers-to-Windows-Server-2012-R2-and-Windows-Server-2012.md#BKMK_UpgradePaths)  
  
-   [Configuration requise et les fonctionnalités des niveaux fonctionnels](../../ad-ds/deploy/Upgrade-Domain-Controllers-to-Windows-Server-2012-R2-and-Windows-Server-2012.md#BKMK_FunctionalLevels)  
  
-   [Interopérabilité du service d’annuaire Active Directory avec d’autres rôles de serveur et les systèmes d’exploitation Windows](../../ad-ds/deploy/Upgrade-Domain-Controllers-to-Windows-Server-2012-R2-and-Windows-Server-2012.md#BKMK_ServerRoles)  
  
-   [Rôles de maître d’opérations](../../ad-ds/deploy/Upgrade-Domain-Controllers-to-Windows-Server-2012-R2-and-Windows-Server-2012.md#BKMK_OpsMasters)  
  
-   [Virtualisation de contrôleurs de domaine qui exécutent Windows Server 2012](../../ad-ds/deploy/Upgrade-Domain-Controllers-to-Windows-Server-2012-R2-and-Windows-Server-2012.md#BKMK_Virtual)  
  
-   [Administration des serveurs Windows Server 2012](../../ad-ds/deploy/Upgrade-Domain-Controllers-to-Windows-Server-2012-R2-and-Windows-Server-2012.md#BKMK_Admin)  
  
-   [Compatibilité des applications](../../ad-ds/deploy/Upgrade-Domain-Controllers-to-Windows-Server-2012-R2-and-Windows-Server-2012.md#BKMK_AppCompat)  
  
-   [Problèmes connus](../../ad-ds/deploy/Upgrade-Domain-Controllers-to-Windows-Server-2012-R2-and-Windows-Server-2012.md#BKMK_KnownIssues)  
  
## <a name="BKMK_UpgradeWorkflow"></a>Étapes mise à niveau de contrôleur de domaine  
La méthode recommandée pour mettre à niveau d’un domaine consiste à promouvoir les contrôleurs de domaine qui exécutent des versions plus récentes de Windows Server et rétrograder les contrôleurs de domaine plus anciens en fonction des besoins. Cette méthode est préférable à la mise à niveau le système d’exploitation d’un contrôleur de domaine existant. Cette liste couvre les étapes générales à suivre avant de promouvoir un contrôleur de domaine qui exécute une version plus récente de Windows Server:  
  
1.  Vérifiez le serveur cible répond à [requise](https://technet.microsoft.com/library/dn303418.aspx).  
  
2.  Vérifiez [compatibilité des applications](../../ad-ds/deploy/Upgrade-Domain-Controllers-to-Windows-Server-2012-R2-and-Windows-Server-2012.md#BKMK_AppCompat).  
  
3.  Vérifiez les paramètres de sécurité. Pour plus d’informations, voir [fonctionnalités déconseillées et modifications de comportement associées aux services AD DS dans Windows Server 2012](../../ad-ds/deploy/Upgrade-Domain-Controllers-to-Windows-Server-2012-R2-and-Windows-Server-2012.md#BKMK_DeprecatedFeatures) et [les paramètres par défaut dans Windows Server 2008 et Windows Server 2008 R2 sécurisés](https://technet.microsoft.com/library/upgrade-domain-controllers-to-windows-server-2008-r2(WS.10).aspx#BKMK_SecureDefault).  
  
4.  Vérifiez la connectivité au serveur cible à partir de l’ordinateur sur lequel vous prévoyez d’exécuter l’installation.  
  
5.  Vérifiez la disponibilité des rôles de maître d’opérations nécessaires:  
  
    -   Pour installer le premier contrôleur de domaine qui exécute Windows Server 2012 dans une forêt et un domaine existant, l’ordinateur sur lequel vous exécutez l’installation requiert une connectivité au contrôleur de schéma afin d’exécuter adprep /forestprep et au maître d’infrastructure afin d’exécuter adprep /domainprep.  
  
    -   Pour installer le premier contrôleur de domaine dans un domaine où le schéma de la forêt est déjà étendu, vous devez uniquement une connectivité au maître d’infrastructure.  
  
    -   Pour installer ou supprimer un domaine dans une forêt existante, vous avez besoin de connectivité pour le maître d’attribution de noms de domaine.  
  
    -   Installation de n’importe quel contrôleur de domaine requiert également une connectivité au maître RID.  
  
    -   Si vous installez le premier contrôleur de domaine en lecture seule dans une forêt existante, vous avez besoin d’une connectivité au maître d’infrastructure pour chaque partition d’annuaire application, également appelé un contexte d’appellation du domaine ou nommage.  
  
6.  Veillez à fournir les informations d’identification nécessaires pour exécuter l’installation des services AD DS.  
  
    |Action d’installation|Informations d’identification requises|  
    |-----------------------|---------------------------|  
    |Installer une nouvelle forêt|Administrateur local sur le serveur cible|  
    |Installer un nouveau domaine dans une forêt existante|Administrateurs de l’entreprise|  
    |Installer un contrôleur de domaine supplémentaire dans un domaine existant|Admins du domaine|  
    |Exécuter adprep /forestprep|Administrateurs du schéma, administrateurs de l’entreprise et Admins du domaine|  
    |Exécuter adprep /domainprep|Admins du domaine|  
    |Exécuter adprep /domainprep /gpprep|Admins du domaine|  
    |Exécuter adprep /rodcprep|Administrateurs de l’entreprise|  
  
    Vous pouvez déléguer des autorisations nécessaires pour installer les services AD DS. Pour plus d’informations, voir [tâches de gestion de l’Installation](https://technet.microsoft.com/library/cc773327(WS.10).aspx).  
  
Vous trouverez des instructions étapes-à-pas pour promouvoir des nouveaux et contrôleurs de domaine réplica Windows Server 2012 à l’aide des applets de commande Windows PowerShell et le Gestionnaire de serveur dans les liens suivants:  
  
-   [Installer les Services de domaine ActiveDirectory (niveau 100)](https://technet.microsoft.com/library/hh472162.aspx)  
  
-   [Installer une nouvelle forêt d’ActiveDirectory de Windows Server2012 (niveau 200)](https://technet.microsoft.com/library/jj574166.aspx)  
  
-   [Installer un contrôleur de domaine Windows Server2012 répliqué dans un domaine existant (niveau 200)](https://technet.microsoft.com/library/jj574134.aspx)  
  
-   [Installer un nouveau Windows Server2012 ActiveDirectory enfant ou un domaine d’arborescence (niveau 200)](https://technet.microsoft.com/library/jj574105.aspx)  
  
-   [Installer un serveur Windows Server2012 ActiveDirectory Read-Only le contrôleur de domaine (RODC) (niveau 200)](https://technet.microsoft.com/library/jj574152.aspx)  
  
-   [Guide pas à pas pour configurer Windows Server 2012 domaine un contrôleur (en-US)](https://social.technet.microsoft.com/wiki/contents/articles/12370.step-by-step-guide-for-setting-up-windows-server-2012-domain-controller-en-us.aspx)  
  
## <a name="BKMK_WhatsNewEight"></a>Quelles sont les nouveautés dans Windows Server 2012?  
Les nouvelles fonctionnalités classées par rôle de serveur et domaine technologique sont répertoriées dans le tableau suivant. Pour plus d’autres livres blancs, démonstrations vidéo et présentations sur d’autres fonctionnalités dans Windows Server 2012, voir [serveur et plateforme Cloud](https://www.microsoft.com/server-cloud/default.aspx).  
  
||||  
|-|-|-|  
|[Services de certificats ActiveDirectory (ADCS)](https://technet.microsoft.com/library/hh831373.aspx)|[Active Directory Rights Management Services AD RMS)](https://technet.microsoft.com/library/hh831554.aspx)|[Chiffrement de lecteur BitLocker](https://technet.microsoft.com/library/hh831412.aspx)|  
|[BranchCache](https://technet.microsoft.com/library/jj127252.aspx)|[Protocole Dynamic Host Configuration protocol (DHCP)](https://technet.microsoft.com/library/jj200226.aspx)|[Nom de domaine (DNS)](https://technet.microsoft.com/library/jj200224.aspx)|  
|[Clustering de basculement](https://technet.microsoft.com/library/hh831414.aspx)|[Gestionnaire de ressources du serveur de fichiers](https://technet.microsoft.com/library/hh831746.aspx)|[Stratégie de groupe](https://technet.microsoft.com/library/jj574108.aspx)|  
|[Hyper-V](https://technet.microsoft.com/library/hh831410.aspx)|[Gestion des adresses IP (IPAM)](https://technet.microsoft.com/library/jj200214.aspx)|[Authentification Kerberos](https://technet.microsoft.com/library/hh831747.aspx)|  
|[Comptes de Service administrés](https://technet.microsoft.com/library/hh831451.aspx)|[Mise en réseau](https://technet.microsoft.com/library/jj200215.aspx)|[Services Bureau à distance](https://technet.microsoft.com/library/hh831527.aspx)|  
|[L’audit de sécurité](https://technet.microsoft.com/library/hh849638.aspx)|[Gestionnaire de serveur](https://blogs.technet.com/b/servermanager/archive/2012/06/27/server-manager-power-of-many-simplicity-of-one.aspx)|[Cartes à puce](https://technet.microsoft.com/library/hh849637.aspx)|  
|[TLS/SSL (SSP Schannel)](https://technet.microsoft.com/library/hh831771.aspx)|[Services de déploiement Windows](https://technet.microsoft.com/library/hh974416.aspx)|[Windows PowerShell 3.0](https://technet.microsoft.com/library/hh857339)|  
  
### <a name="automatic-maintenance-and-changes-to-restart-behavior-after-updates-are-applied-by-windows-update"></a>Maintenance automatique et modifications de comportement de redémarrage après l’application des mises à jour par Windows Update  
Avant la version de Windows 8, Windows Update gérait sa propre planification interne pour rechercher les mises à jour et pour télécharger et installer les. Il nécessaire que l’Agent Windows Update était toujours en cours d’exécution en arrière-plan, consommation de mémoire et autres ressources système.  
  
Windows 8 et Windows Server 2012 introduisent une nouvelle fonctionnalité appelée [Maintenance automatique](https://msdn.microsoft.com/library/windows/desktop/hh848037(v=vs.85).aspx). Maintenance automatique consolide de nombreuses fonctionnalités que chaque utilisé pour gérer sa propre planification et la logique d’exécution. Cette consolidation permet à tous ces composants d’utiliser beaucoup moins de ressources système, de fonctionner de façon cohérente, de respecter le nouvel [veille connectée](https://msdn.microsoft.com/library/windows/hardware/jj248729.aspx) d’état pour les nouveaux types d’appareils et de consommer moins de batterie sur les appareils mobiles.  
  
Étant donné que Windows Update fait partie de la Maintenance automatique dans Windows 8 et Windows Server 2012, sa propre planification interne pour définir un jour et l’heure d’installation des mises à jour n’est plus efficace. Pour garantir cohérente et prévisible redémarrent le comportement de tous les ordinateurs et périphériques de votre entreprise, y compris ceux qui exécutent Windows 8 et Windows Server 2012, Microsoft KB de voir l’article [2885694](https://support.microsoft.com/kb/2885694) (ou consultez cumulatif d’octobre 2013 [2883201](https://support.microsoft.com/kb/2883201)), puis configurez les paramètres de stratégie décrits dans le billet de blog WSUS [l’activation d’une expérience plus prévisible de la mise à jour de Windows pour Windows 8 et Windows Server 2012 (KB 2885694)](http://blogs.technet.com/b/wsus/archive/2013/10/08/enabling-a-more-predictable-windows-update-experience-for-windows-8-and-windows-server-2012-kb-2885694.aspx).  
  

## <a name="BKMK_NewWS2012R2"></a>Quelles sont les nouveautés dans AD DS dans Windows Server 2012 R2?  
Le tableau suivant récapitule les nouvelles fonctionnalités pour les services AD DS dans Windows Server 2012 R2, avec un lien vers des informations plus détaillées sur lequel il est disponible. Pour obtenir une explication plus détaillée de certaines fonctionnalités, notamment les conditions requises, voir [Nouveautés dans Active Directory dans Windows Server 2012 R2](https://technet.microsoft.com/library/dn268294.aspx).  
  
|Fonctionnalité|Description|  
|-----------|---------------|  
|[Jonction d’espace](https://technet.microsoft.com/library/dn280945.aspx)|Permet aux employés de leurs appareils personnels et leur entreprise pour accéder aux ressources d’entreprise et de services.|  
|[Proxy d’Application Web](https://technet.microsoft.com/library/dn280942.aspx)|Fournit l’accès aux applications web à l’aide d’un nouveau service de rôle accès à distance.|  
|[ActiveDirectory Federation Services](https://technet.microsoft.com/library/hh831502.aspx)|AD FS a simplifié le déploiement et améliorations pour permettre aux utilisateurs d’accéder aux ressources à partir d’appareils personnels et aider les services informatiques à gérer le contrôle d’accès.|  
|[Unicité des noms SPN et UPN](https://technet.microsoft.com/library/dn535779.aspx)|Contrôleurs de domaine exécutant Windows Server 2012 R2 bloquent la création de noms de principal du service en double (SPN) et les noms d’utilisateurs principaux (UPN).|  
|[Automatique Winlogon redémarrage Sign-On (ARSO)](https://technet.microsoft.com/library/dn535772.aspx)|Permet de verrouille des applications d’écran d’être redémarrées et disponibles sur les appareils Windows 8.1.|  
|[Attestation de clé TPM](https://technet.microsoft.com/library/dn581921.aspx)|Permet d’autorités de certification d’attester par chiffrement dans un certificat émis que la clé privée du demandeur du certificat est réellement protégée par un Module de plateforme sécurisée (TPM).|  
|[Gestion et Protection des informations d’identification](https://technet.microsoft.com/library/dn408190.aspx)|Nouvelles informations d’identification domaine et protection contrôles d’authentification pour réduire le vol d’informations d’identification.|  
|[Désapprobation du Service de réplication de fichiers (FRS)](https://technet.microsoft.com/library/dn535775.aspx)|Le niveau fonctionnel du domaine Windows Server 2003 est également déconseillé, car le niveau fonctionnel, FRS est utilisé pour répliquer SYSVOL. Que signifie que lorsque vous créez un nouveau domaine sur un serveur qui exécute Windows Server 2012 R2, le niveau fonctionnel du domaine doit être Windows Server 2008 ou version ultérieure. Vous pouvez toujours ajouter un contrôleur de domaine qui exécute Windows Server 2012 R2 à un domaine existant qui a un niveau fonctionnel du domaine Windows Server 2003; Vous ne pouvez pas simplement créer un nouveau domaine à ce niveau.|  
|[Nouveaux niveaux fonctionnels de domaine et de forêt](../active-directory-functional-levels.md)|Il existe de nouveaux niveaux fonctionnels pour Windows Server 2012 R2. Nouvelles fonctionnalités sont disponibles au niveau fonctionnel du domaine de Windows Server 2012 R2.|  
|[Modifications optimiseur de requête LDAP](https://technet.microsoft.com/library/dn535775.aspx)|Amélioration des performances de l’efficacité des recherches LDAP et délai de recherche LDAP de requêtes complexes.|  
|[Événement1644](https://technet.microsoft.com/library/dn535775.aspx)|Statistiques des résultats de recherche LDAP ont été ajoutés à l’événement 1644 ID pour faciliter le dépannage.|  
|[Amélioration du débit de réplication Active Directory](https://technet.microsoft.com/library/dn535775.aspx)|Ajuste le débit de réplication Active Directory maximal de 40 à environ 600 Mbits/s|  
  
## <a name="BKMK_WhatsNewAD"></a>Quelles sont les nouveautés dans AD DS dans Windows Server 2012?  
Le tableau suivant récapitule les nouvelles fonctionnalités pour les services AD DS dans Windows Server 2012, avec un lien vers des informations plus détaillées sur lequel il est disponible. Pour obtenir une explication plus détaillée de certaines fonctionnalités, notamment les conditions requises, voir [Nouveautés dans les Services de domaine Active Directory (AD DS)](https://technet.microsoft.com/library/hh831477.aspx).  
  
|Fonctionnalité|Description|  
|-----------|---------------|  
|L’Activation basée sur Active Directory (AD BA) voir [vue d’ensemble de l’activation en Volume](https://technet.microsoft.com/library/hh831612.aspx)|Simplifie la tâche de configuration de la distribution et la gestion des licences logicielles en volume.|  
|[Active Directory Federation Services (ADFS)](https://technet.microsoft.com/library/hh831502.aspx)|Ajoute l’installation du rôle via le Gestionnaire de serveur, simplifié la configuration d’approbation, gestion de la relation d’approbation automatique, prise en charge du protocole SAML et plus encore.|  
|Événements de vidage des pages perdues Active Directory|Événement NTDS ISAM 530 avec erreur jet -1119 est consigné pour détecter les événements de vidage des pages perdues aux bases de données Active Directory.|  
|[Active Directory Interface de la Corbeille utilisateur](https://technet.microsoft.com/library/hh831702.aspx#ad_recycle_bin_mgmt)|Centre d’administration Active Directory (ADAC) ajoute la gestion de l’interface graphique utilisateur de la fonctionnalité de Corbeille introduite initialement dans Windows Server 2008 R2.|  
|[Applets de commande Windows PowerShell de topologie et de réplication Active Directory](https://technet.microsoft.com/library/hh831757.aspx)|Prend en charge la création et la gestion de sites Active Directory, les liens de sites, les objets de connexion et plus à l’aide de Windows PowerShell.|  
|[Contrôle d’accès dynamique](https://technet.microsoft.com/library/hh831717.aspx)|Nouvelle plateforme d’autorisation basée sur les revendications qui améliore le modèle de contrôle d’accès hérité.|  
|[Interface utilisateur de stratégie de mot de passe affinée](https://technet.microsoft.com/library/hh831702.aspx#fine_grained_pswd_policy_mgmt)|ADAC ajoute la prise en charge de l’interface graphique utilisateur de création, modification et au affectation des objets PSO ajoutés à l’origine dans Windows Server 2008.|  
|[Groupe des comptes de Service administrés (gMSA)](https://technet.microsoft.com/library/hh831782.aspx)|Un nouveau type sécurité de principal appelé un compte gMSA. Les services exécutés sur plusieurs ordinateurs hôtes peuvent être exécutés sous le même compte de service administré de groupe.|  
|[Jonction de domaine hors connexion DirectAccess](https://technet.microsoft.com/library/jj574150.aspx)|Étend la jonction de domaine hors connexion en incluant les conditions préalables DirectAccess.|  
|[Déploiement rapide via le clonage de contrôleur de domaine virtuel](https://technet.microsoft.com/library/hh831734.aspx#virtualized_dc_cloning)|Contrôleurs de domaine virtualisés peuvent être rapidement déployés en clonant des contrôleurs de domaine virtuel existant à l’aide des applets de commande Windows PowerShell.|  
|[Modifications du pool RID](https://technet.microsoft.com/library/jj574229.aspx)|Ajoute les nouveaux événements de surveillance et les quotas afin d’éviter une consommation excessive du pool RID global. Double éventuellement la taille du pool RID global si le pool d’origine est épuisé.|  
|Service de temps sécurisé|Améliore la sécurité pour W32tm en supprimant des secrets du réseau, en supprimant les fonctions de hachage MD5 et en demandant au serveur pour s’authentifier auprès des clients de temps Windows 8|  
|[Protection des restaurations USN pour les contrôleurs de domaine virtualisés](https://technet.microsoft.com/library/hh831734.aspx#safe_virt_dc)|La restauration accidentelle des sauvegardes de captures instantanées des contrôleurs de domaine virtualisés ne sont plus provoque une restauration USN.|  
|[Visionneuse de l’historique de PowerShell de Windows](https://technet.microsoft.com/library/hh831702.aspx#windows_powershell_history_viewer)|Permettre aux administrateurs d’afficher les commandes Windows PowerShell exécutées lors de l’aide d’ADAC.|  
  
### <a name="BKMK_"></a>Maintenance automatique et modifications de comportement de redémarrage après l’application des mises à jour par Windows Update  
Avant la version de Windows 8, Windows Update gérait sa propre planification interne pour rechercher les mises à jour et pour télécharger et installer les. Il nécessaire que l’Agent Windows Update était toujours en cours d’exécution en arrière-plan, consommation de mémoire et autres ressources système.  
  
Windows 8 et Windows Server 2012 introduisent une nouvelle fonctionnalité appelée [Maintenance automatique](https://msdn.microsoft.com/library/windows/desktop/hh848037(v=vs.85).aspx). Maintenance automatique consolide de nombreuses fonctionnalités que chaque utilisé pour gérer sa propre planification et la logique d’exécution. Cette consolidation permet à tous ces composants d’utiliser beaucoup moins de ressources système, de fonctionner de façon cohérente, de respecter le nouvel [veille connectée](https://msdn.microsoft.com/library/windows/hardware/jj248729.aspx) d’état pour les nouveaux types d’appareils et de consommer moins de batterie sur les appareils mobiles.  
  
Étant donné que Windows Update fait partie de la Maintenance automatique dans Windows 8 et Windows Server 2012, sa propre planification interne pour définir un jour et l’heure d’installation des mises à jour n’est plus efficace. Pour garantir le comportement de redémarrage cohérent et prévisible pour tous les périphériques et ordinateurs de votre entreprise, y compris ceux qui exécutent Windows 8 et Windows Server 2012, vous pouvez configurer les paramètres de stratégie de groupe suivants:  
  
-   **Configuration ordinateur | Stratégies | Modèles d’administration | Composants Windows | Mise à jour Windows | Configurer les mises à jour automatiques**  
  
-   **Configuration ordinateur | Stratégies | Modèles d’administration | Composants Windows | Mise à jour Windows | Pas de redémarrage automatique avec des utilisateurs connectés**  
  
-   **Configuration ordinateur | Stratégies | Modèles d’administration | Composants Windows | Le Planificateur de maintenance | Délai aléatoire de maintenance**  
  
Le tableau suivant répertorie certains exemples illustrant comment configurer ces paramètres pour fournir le comportement de redémarrage souhaité.  
  
|||  
|-|-|  
|**Scénario**|**Configurations recommandées**|  
|**Gérés par WSUS**<br /><br />-Installer les mises à jour une fois par semaine<br />-Redémarrer les vendredis à 23 h 00|Définir des ordinateurs virtuels pour l’installation automatique, le redémarrage automatique jusqu’au moment voulu<br /><br />**Stratégie**: configurer les mises à jour automatiques (activées)<br /><br />Configurer la mise à jour automatique: 4 - téléchargement automatique et planification des installations<br /><br />**Stratégie**: aucun redémarrage automatique avec des utilisateurs connectés (désactivée)<br /><br />**Échéance WSUS**: la valeur vendredis à 23 h 00|  
|**Gérés par WSUS**<br /><br />-Échelonner les installations sur différentes heures/journées|Définition de groupes cibles pour différents groupes d’ordinateurs virtuels qui doivent être mis à jour ensemble<br /><br />Utilisation des étapes ci-dessus pour le scénario précédent<br /><br />Définition de différentes échéances pour différents groupes cibles|  
|**Pas ne gérés par WSUS - aucune prise en charge des échéances**<br /><br />-Échelonner les installations à différents moments|**Stratégie**: configurer les mises à jour automatiques (activées)<br /><br />Configurer la mise à jour automatique: 4 - téléchargement automatique et planification des installations<br /><br />**Clé de Registre:** activez la clé de Registre présentée dans l’article KB Microsoft [2835627](https://support.microsoft.com/kb/2835627)<br /><br />**Stratégie:** délai aléatoire de Maintenance automatique (activée)<br /><br />Définissez **délai aléatoire de maintenance régulière** à PT6H pour délai aléatoire de 6 heures pour fournir le comportement suivant:<br /><br />-Les mises à jour seront installées à l’heure de maintenance configurée plus un délai aléatoire<br /><br />-Le redémarrage de chaque ordinateur aura lieu exactement 3 jours plus tard<br /><br />Vous pouvez également définir une heure de maintenance différente pour chaque groupe d’ordinateurs|  
  
Pour plus d’informations sur la raison pour laquelle l’équipe d’ingénieurs Windows a implémenté ces modifications, voir [Minimizing restarts après la mise à jour automatique Windows Update](http://blogs.msdn.com/b/b8/archive/2011/11/14/minimizing-restarts-after-automatic-updating-in-windows-update.aspx).  
  
## <a name="BKMK_InstallationChanges"></a>Modifications d’installation de rôle de serveur AD DS  
Dans Windows Server 2003 par le biais de Windows Server 2008 R2, vous avez exécuté le x86 ou X64 version de l’outil de ligne de commande Adprep.exe avant d’exécuter l’Assistant Installation Active Directory, Dcpromo.exe et Dcpromo.exe proposait des variantes facultatives pour installer à partir du média ou pour l’installation sans assistance.  
  
À compter de Windows Server 2012, les installations de ligne de commande sont effectuées à l’aide du ADDSDeployment Module dans Windows PowerShell. Des promotions basées sur une interface graphique utilisateur sont effectuées dans le Gestionnaire de serveur à l’aide d’un Assistant de Configuration AD DS entièrement nouveau. Pour simplifier le processus d’installation, ADPREP a été intégré à l’installation des services AD DS et s’exécute automatiquement en fonction des besoins. L’Assistant de Configuration de service d’annuaire Active Directory basé sur Windows PowerShell cible automatiquement les rôles de maître infrastructure et de schéma dans les domaines où les contrôleurs de domaine sont ajoutés, puis exécutant à distance les commandes ADPREP requises sur les contrôleurs de domaine appropriés.  
  
Les conditions préalables dans l’Assistant Installation du service d’annuaire Active Directory identifient les erreurs potentielles avant le début de l’installation. Conditions d’erreur peuvent être corrigées pour éliminer les problèmes résultant d’une mise à niveau partiellement terminée. L’Assistant exporte également un script Windows PowerShell qui contient toutes les options qui ont été spécifiées pendant l’installation graphique.  
  
Ensemble, les modifications d’installation AD DS simplifient le processus d’installation de rôle du contrôleur de domaine et réduisent la probabilité d’erreurs d’administration, en particulier lorsque vous déployez plusieurs contrôleurs de domaine sur des régions et domaines globaux.  
Plus d’informations sur l’interface graphique utilisateur et les installations Windows PowerShell, notamment la syntaxe de ligne de commande et les instructions de l’Assistant pas à pas, voir [installer Active Directory Domain Services](https://technet.microsoft.com/library/hh472162.aspx). Pour les administrateurs qui veulent contrôler l’introduction des modifications de schéma dans une forêt Active Directory indépendamment de l’installation des contrôleurs de domaine de Windows Server 2012 dans une forêt existante, les commandes Adprep.exe peuvent toujours être exécutées à une invite de commandes avec élévation de privilèges.  
  
## <a name="BKMK_DeprecatedFeatures"></a>Fonctionnalités déconseillées et modifications de comportement associées aux services AD DS dans Windows Server 2012  
Il existe quelques modifications associées aux services AD DS:  
  
-   **Désapprobation d’Adprep32.exe**  
  
    Il n'existe qu’une seule version d’Adprep.exe et elle peut être exécutée en fonction des besoins sur des serveurs 64 bits qui exécutent Windows Server 2008 ou version ultérieure. Il peut être exécuté à distance et doit être exécuté à distance si qui cible les opérations rôle principal est hébergé sur un système d’exploitation 32 bits ou de Windows Server 2003.  
  
-   **Désapprobation de Dcpromo.exe**  
  
    Dcpromo est déconseillé, même si dans Windows Server 2012 uniquement, il peut toujours être exécuté avec un fichier de réponses ou des paramètres de ligne de commande pour donner aux organisations le temps de convertir l’automatisation existante vers les nouvelles options d’installation de Windows PowerShell.  
  
-   **LMHash est désactivé sur les comptes d’utilisateur**  
  
    Paramètres par défaut sécurisés dans sécurité modèles sur Windows Server 2008, Windows Server 2008 R2 et Windows Server 2012 activent la stratégie NoLMHash qui est désactivée dans les modèles de sécurité de Windows 2000 et les contrôleurs de domaine Windows Server 2003. Désactivez la stratégie NoLMHash pour les clients dépendant de LMHash en fonction des besoins, en suivant les étapes dans l’article [946405](https://support.microsoft.com/kb/946405).  
  
À compter de Windows Server 2008, les contrôleurs de domaine ont également les paramètres par défaut sécurisés suivants, comparés aux contrôleurs de domaine qui exécutent Windows Server 2003 ou Windows 2000.  
  
|||||  
|-|-|-|-|  
|Type de chiffrement ou stratégie|Par défaut de Windows Server 2008|Par défaut de Windows Server 2012 et Windows Server 2008 R2|Commentaire|  
|AllowNT4Crypto|Désactivé|Désactivé|Les clients de bloc de Message serveur (SMB) tiers peuvent être incompatibles avec les paramètres par défaut sécurisés sur les contrôleurs de domaine. Dans tous les cas, ces paramètres de valeur peuvent être moins stricte pour permettre l’interopérabilité, mais uniquement au détriment de la sécurité. Pour plus d’informations, voir [article 942564](https://go.microsoft.com/fwlink/?LinkId=164558) dans la Base de connaissances Microsoft (https://go.microsoft.com/fwlink/?LinkId=164558).|  
|DES|Activé|Désactivé|[Article977321](https://go.microsoft.com/fwlink/?LinkId=177717) dans la Base de connaissances Microsoft (https://go.microsoft.com/fwlink/?LinkId=177717)|  
|Distance/Protection étendue pour l’authentification intégrée|NON APPLICABLE|Activé|Voir [avis de sécurité Microsoft (937811)](https://go.microsoft.com/fwlink/?LinkId=164559) (https://go.microsoft.com/fwlink/?LinkId=164559) et [article 976918](https://go.microsoft.com/fwlink/?LinkId=178251) dans la Base de connaissances Microsoft (https://go.microsoft.com/fwlink/?LinkId=178251).<br /><br />Examinez et installez le correctif logiciel de [article 977073](https://go.microsoft.com/fwlink/?LinkId=186394) (https://go.microsoft.com/fwlink/?LinkId=186394) dans la Base de connaissances Microsoft, en fonction des besoins.|  
|LMv2|Activé|Désactivé|[Article976918](https://go.microsoft.com/fwlink/?LinkId=178251) dans la Base de connaissances Microsoft (https://go.microsoft.com/fwlink/?LinkId=178251)|  
  
## <a name="BKMK_SysReqs"></a>Configuration requise du système d’exploitation  
La configuration minimale requise pour Windows Server 2012 est répertoriée dans le tableau suivant. Pour plus d’informations sur la configuration système requise et informations de préinstallation, voir [l’installation de Windows Server 2012](https://technet.microsoft.com/library/jj134246.aspx). Il n’y a aucune configuration supplémentaire requise pour installer une nouvelle forêt Active Directory, mais vous devez ajouter suffisamment de mémoire pour mettre en cache le contenu de base de données Active Directory afin d’améliorer les performances des contrôleurs de domaine, les demandes de client LDAP et applications prenant en charge Active Directory. Si vous mettez à niveau un contrôleur de domaine existant ou que vous ajoutez un nouveau contrôleur de domaine à une forêt existante, passez en revue la section suivante pour vérifier le serveur répond aux exigences d’espace disque.  
  
|||  
|-|-|  
|Processeur|1,4 processeur Ghz 64 bits|  
|MÉMOIRE RAM|512 MO|  
|Espace disque requis|32GO|  
|Résolution d’écran|800 × 600 ou supérieure|  
|Divers|Lecteur de DVD, clavier, un accès à Internet|  
  
### <a name="BKMK_DiskSpaceDCWin8"></a>Espace disque requis pour la mise à niveau des contrôleurs de domaine  
Cette section couvre l’espace disque requis uniquement pour la mise à niveau des contrôleurs de domaine à partir de Windows Server 2008 ou Windows Server 2008 R2. Pour plus d’informations sur l’espace disque requis pour la mise à niveau des contrôleurs de domaine aux versions antérieures de Windows Server, voir [espace disque requis pour la mise à niveau vers Windows Server 2008](https://technet.microsoft.com/library/cc754463(WS.10).aspx#BKMK_2008) ou [espace disque requis pour la mise à niveau vers Windows Server 2008 R2](https://technet.microsoft.com/library/cc754463(WS.10).aspx#BKMK_2008R2).  
  
La taille du disque qui héberge les fichiers de base de données et journal Active Directory afin de prendre en charge les extensions de schéma personnalisées et pilotées par l’application, application et index initiés par l’administrateur, plus l’espace pour les objets et attributs que vous seront ajoutés à l’annuaire sur la durée de vie de déploiement du contrôleur de domaine (généralement 5 à 8 ans). Dimensionnement droit au moment du déploiement est généralement un bon investissement par rapport à une plus grande coûts d’étendre le stockage de disque après le déploiement. Pour plus d’informations, voir [planification de capacité pour les Services de domaine Active Directory](https://social.technet.microsoft.com/wiki/contents/articles/14355.capacity-planning-for-active-directory-domain-services.aspx).  
  
Sur les contrôleurs de domaine que vous envisagez de mettre à niveau, assurez-vous que le lecteur qui héberge Active Directory de base de données (NTDS. DIT) possède un espace disque qui représente au moins 20 % du fichier NTDS. Fichier DIT avant de commencer la mise à niveau du système d’exploitation. S’il existe un espace disque libre insuffisant sur le volume, la mise à niveau peut échouer et le rapport de compatibilité de mise à niveau retourne une erreur indiquant un espace disque libre insuffisant:  
  
Dans ce cas, vous pouvez essayer une défragmentation hors connexion de la base de données Active Directory pour libérer plus d’espace et recommencez la mise à niveau. Pour plus d’informations, voir [compacter le fichier de base de données d’annuaire (défragmentation hors connexion)](https://technet.microsoft.com/library/cc794920(v=WS.10).aspx).  
  
### <a name="available-skus"></a>Références disponibles  
Il existe 4 éditions de Windows Server: Foundation, Essentials, Standard et Datacenter.   
Les deux éditions qui prennent en charge le rôle AD DS sont Standard et Datacenter.  
  
Dans les versions précédentes, les éditions de Windows Server étaient différentes dans leur prise en charge des rôles de serveur, nombre de processeurs et prise en charge de grande mémoire. Les éditions Standard et Datacenter de Windows Server prend en charge toutes les fonctionnalités et le matériel sous-jacent, mais varient dans leurs droits de virtualisation - deux instances virtuelles sont autorisées pour l’Édition Standard et un nombre illimité d’instances virtuelles sont autorisées pour l’Édition Datacenter.  
  
### <a name="windows-client-and-windows-server-operating-systems-that-are-supported-to-join-windows-server-domains"></a>Systèmes d’exploitation Windows Server qui sont pris en charge pour rejoindre des domaines Windows Server et clients Windows  
Systèmes d’exploitation Windows Server et clients Windows suivants sont pris en charge pour les ordinateurs membres du domaine avec des contrôleurs de domaine qui exécutent Windows Server 2012 ou version ultérieure:  
  
-   Systèmes d’exploitation clients: Windows 8.1, Windows 8, Windows 7, Windows Vista 
  
    Les ordinateurs qui exécutent Windows 8.1 ou Windows 8 sont également en mesure de rejoindre des domaines qui ont des contrôleurs de domaine qui exécutent une version antérieure de Windows Server, y compris Windows Server 2003 ou version ultérieure. Dans ce cas toutefois, certaines fonctionnalités de Windows 8 peuvent nécessiter une configuration supplémentaire ou ne peuvent pas être disponibles. Pour plus d’informations sur ces fonctionnalités et d’autres recommandations pour la gestion des clients Windows 8 dans les domaines de niveau inférieur, voir [d’ordinateurs exécutant Windows 8 membres dans les domaines Windows Server 2003](https://social.technet.microsoft.com/wiki/contents/articles/17361.running-windows-8-member-computers-in-windows-server-2003-domains.aspx).  
  
-   Systèmes d’exploitation serveur: Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows Server 2008, Windows Server 2003 R2, Windows Server 2003  
  
## <a name="BKMK_UpgradePaths"></a>Chemins de mise à niveau sur place pris en charge  
Les contrôleurs de domaine qui exécutent des versions 64 bits de Windows Server 2008 ou Windows Server 2008 R2 peuvent être mis à niveau vers Windows Server 2012. Vous ne pouvez pas mettre à niveau des contrôleurs de domaine qui exécutent Windows Server 2003 ou des versions 32 bits de Windows Server 2008. Pour les remplacer, installez des contrôleurs de domaine qui exécutent une version ultérieure de Windows Server dans le domaine, puis supprimez les contrôleurs de domaine Windows Server 2003.  
  
|Si vous exécutez ces éditions|Vous pouvez mettre à niveau vers ces éditions|  
|-------------------------------------|-------------------------------------|  
|Windows Server 2008 Standard avec SP2<br /><br />OU<br /><br />Windows Server 2008 entreprise avec SP2|Windows Server 2012 Standard<br /><br />OU<br /><br />Windows Server 2012 Datacenter|  
|Windows Server 2008 Datacenter avec Service Pack 2|Windows Server 2012 Datacenter|  
|Windows Web Server 2008|Windows Server 2012 Standard|  
|Windows Server 2008 R2 Standard avec SP1<br /><br />OU<br /><br />Windows Server 2008 R2 entreprise avec SP1|Windows Server 2012 Standard<br /><br />OU<br /><br />Windows Server 2012 Datacenter|  
|Windows Server 2008 R2 Datacenter avec Service Pack 1|Windows Server 2012 Datacenter|  
|Windows Web Server 2008 R2|Windows Server 2012 Standard|  
  
Pour plus d’informations sur les chemins de mise à niveau pris en charge, voir [Versions d’évaluation et Options de mise à niveau pour Windows Server 2012](https://go.microsoft.com/fwlink/?LinkId=260917). Notez que vous ne pouvez pas convertir un contrôleur de domaine qui exécute une version d’évaluation de Windows Server 2012 directement vers une version commercialisée. Au lieu de cela, installez un contrôleur de domaine supplémentaire sur un serveur qui exécute une version commerciale et supprimer les services AD DS à partir du contrôleur de domaine qui exécute la version d’évaluation.  
  
En raison d’un problème connu, vous ne pouvez pas mettre à niveau un contrôleur de domaine qui exécute une installation Server Core de Windows Server 2008 R2 vers une installation Server Core de Windows Server 2012. La mise à niveau se bloque sur un écran noir au plus tard dans le processus de mise à niveau. Le redémarrage de ces contrôleurs de domaine expose une option dans le fichier boot.ini pour restaurer la version de système d’exploitation précédent. Un autre redémarrage déclenche la restauration automatique vers la version de système d’exploitation précédent. Jusqu'à ce qu’une solution n’est disponible, il est recommandé d’installer un nouveau contrôleur de domaine exécutant une installation Server Core de Windows Server 2012 au lieu de la mise à niveau un contrôleur de domaine existant qui exécute une installation Server Core de Windows Server 2008 R2 sur place. Pour plus d’informations, voir l’article [2734222](https://support.microsoft.com/kb/2734222).  
  
## <a name="BKMK_FunctionalLevels"></a>Configuration requise et les fonctionnalités des niveaux fonctionnels  
 Windows Server 2012 nécessite un niveau fonctionnel de forêt Windows Server 2003. Autrement dit, avant de pouvoir ajouter un contrôleur de domaine qui exécute Windows Server 2012 à une forêt Active Directory existante, le niveau fonctionnel de forêt doit être Windows Server 2003 ou version ultérieure. Cela signifie que les contrôleurs de domaine qui exécutent Windows Server 2008 R2, Windows Server 2008 ou Windows Server 2003 peuvent fonctionner dans la même forêt, mais les contrôleurs de domaine qui exécutent Windows 2000 Server ne sont pas pris en charge et bloque l’installation d’un contrôleur de domaine qui exécute Windows Server 2012. Si la forêt contient des contrôleurs de domaine exécutant Windows Server 2003 ou version ultérieure, mais le fonctionnel de forêt niveau est toujours Windows 2000, l’installation est également bloquée.  
  
Contrôleurs de domaine Windows 2000 doivent être supprimés avant d’ajouter des contrôleurs de domaine Windows Server 2012 à votre forêt. Dans ce cas, prenez en compte le workflow suivant:  
  
1.  Installez des contrôleurs de domaine qui exécutent Windows Server 2003 ou version ultérieure. Ces contrôleurs de domaine peuvent être déployés sur une version d’évaluation de Windows Server. Cette étape nécessite également [exécution d’adprep.exe](https://technet.microsoft.com/library/dd464018(WS.10).aspx) pour ce système d’exploitation version comme condition préalable.  
  
2.  Supprimez les contrôleurs de domaine Windows 2000. Plus précisément, rétrogradez correctement ou supprimez de force les contrôleurs de domaine Windows Server 2000 du domaine utilisé utilisateurs Active Directory et les ordinateurs à supprimer les comptes de contrôleur de domaine pour tous les contrôleurs de domaine supprimés.  
  
3.  Augmenter le niveau fonctionnel de forêt à Windows Server 2003 ou version ultérieure.  
  
4.  Installez des contrôleurs de domaine qui exécutent Windows Server 2012.  
  
5.  Supprimez les contrôleurs de domaine qui exécutent des versions antérieures de Windows Server.  
  
Le nouveau niveau fonctionnel Windows Server 2012 offre une nouvelle fonctionnalité: la **prise en charge des revendications, l’authentification composée et le blindage Kerberos** stratégie de modèle d’administration KDC a deux paramètres (**toujours fournir des revendications** et **rejeter les demandes d’authentification non blindées**) qui nécessitent le niveau fonctionnel du domaine Windows Server 2012.  
  
Le niveau fonctionnel de forêt Windows Server2012 ne fournit pas de nouvelles fonctionnalités, mais il garantit que tout nouveau domaine créé dans la forêt fonctionne automatiquement au niveau fonctionnel du domaine Windows Server2012. Le niveau fonctionnel du domaine Windows Server 2012 ne fournit pas d’autres nouvelles fonctionnalités au-delà de prise en charge des revendications, l’authentification composée et le blindage Kerberos. Il garantit toutefois que tout contrôleur de domaine dans le domaine exécute Windows Server 2012. Pour plus d’informations sur d’autres fonctionnalités qui sont disponibles à différents niveaux fonctionnels, voir [niveaux fonctionnels des Services de domaine d’ActiveDirectory présentation (ADDS)](../active-directory-functional-levels.md).  
  
Une fois que vous définissez le niveau fonctionnel de forêt à une certaine valeur, vous ne pouvez pas restaurer ou diminuer le niveau fonctionnel de forêt, avec les exceptions suivantes: lorsque vous augmentez le niveau fonctionnel de forêt vers Windows Server 2012, vous pouvez le réduire à Windows Server 2008 R2. Si la Corbeille Active Directory n’a pas été activée, vous pouvez aussi diminuer la niveau fonctionnel de Windows Server 2012 pour Windows Server 2008 R2 ou Windows Server 2008 ou de Windows Server 2008 R2 forêt vers Windows Server 2008. Si le niveau fonctionnel de forêt est défini sur Windows Server 2008 R2, il ne peut pas être restauré, par exemple, pour Windows Server 2003.  
  
Après avoir défini le niveau fonctionnel du domaine à une certaine valeur, vous ne pouvez pas effectuer de restauration ou diminuer le niveau fonctionnel du domaine, avec les exceptions suivantes: lorsque vous augmentez le niveau fonctionnel du domaine à Windows Server 2008 R2 ou Windows Server 2012, et que le niveau fonctionnel de forêt est Windows Server 2008 ou inférieur, vous avez la possibilité de restaurer le niveau fonctionnel du domaine sauvegarder sur Windows Server 2008 ou Windows Server 2008 R2. Vous pouvez diminuer le niveau fonctionnel du domaine uniquement à partir de Windows Server 2012 vers Windows Server 2008 R2 ou Windows Server 2008 ou de Windows Server 2008 R2 vers Windows Server 2008. Si le niveau fonctionnel du domaine est défini sur Windows Server 2008 R2, il ne peut pas être restauré, par exemple, pour Windows Server 2003.  
  
Pour plus d’informations sur les fonctionnalités qui sont disponibles à des niveaux fonctionnels inférieurs, voir [niveaux fonctionnels des Services de domaine d’Active Directory présentation (AD DS)](../active-directory-functional-levels.md).  
  
Au-delà des niveaux fonctionnels, un contrôleur de domaine qui exécute Windows Server2012 fournit des fonctionnalités supplémentaires qui ne sont pas disponibles sur un contrôleur de domaine qui exécute une version antérieure de Windows Server. Par exemple, un contrôleur de domaine qui exécute Windows Server2012 peut servir de clonage de contrôleur de domaine virtuel, tandis que le contrôleur de domaine qui exécute une version antérieure de Windows Server ne peut pas. Mais le clonage de contrôleur de domaine virtuel et de protections de contrôleur de domaine virtuel dans Windows Server 2012 n’ont pas de n’importe quel niveau fonctionnel.  
  
> [!NOTE]  
> Microsoft Exchange Server 2013 requiert un niveau fonctionnel de forêt de Windows server 2003 ou version ultérieure.  
  
## <a name="BKMK_ServerRoles"></a>Interopérabilité du service d’annuaire Active Directory avec d’autres rôles de serveur et les systèmes d’exploitation Windows  
Les services AD DS n’est pas pris en charge sur les systèmes d’exploitation Windows suivants:  
  
-   Windows MultiPoint Server  
  
-   Windows Server 2012 Essentials  
  
Les services AD DS ne peut pas être installés sur un serveur qui exécute également les rôles de serveur suivants ou les services de rôle:  
  
-   Hyper-V Server  
  
-   Service Broker pour les connexions Bureau à distance  
  
## <a name="BKMK_OpsMasters"></a>Rôles de maître d’opérations  
Nouvelles fonctionnalités dans Windows Server 2012 affectent des rôles de maître d’opérations:  
  
-   L’émulateur de contrôleur de domaine principal doit exécuter Windows Server 2012 pour prendre en charge le clonage des contrôleurs de domaine virtuel. Il existe des conditions préalables supplémentaires pour le clonage des contrôleurs de domaine. Pour plus d’informations, voir [virtualisation des Services de domaine Active Directory (AD DS)](https://technet.microsoft.com/library/hh831734.aspx).  
  
-   Nouvelles entités de sécurité sont créées lorsque l’émulateur de contrôleur de domaine principal exécute Windows Server 2012.  
  
-   Le maître RID a nouvelle émission RID et des fonctionnalités d’analyse. Les améliorations incluent une meilleure journalisation des événements, des limites plus appropriées, et la capacité, en cas d’urgence - augmenter le RID global de l’allocation de pool d’un bit. Pour plus d’informations, voir [Managing RID Issuance](../../ad-ds/manage/Managing-RID-Issuance.md).  
  
> [!NOTE]  
> S’ils ne sont pas des rôles de maître d’opérations, une autre modification dans l’installation des services AD DS est que le rôle de serveur DNS et le catalogue global sont installés par défaut sur tous les contrôleurs de domaine qui exécutent Windows Server 2012.  
  
## <a name="BKMK_Virtual"></a>Virtualisation de contrôleurs de domaine  
Améliorations de début des services AD DS dans Windows Server 2012 permettent une virtualisation plus fiable des contrôleurs de domaine et la possibilité de cloner des contrôleurs de domaine. Le clonage des contrôleurs de domaine à son tour permet au déploiement rapide des contrôleurs de domaine supplémentaires dans un nouveau domaine et d’autres avantages. Pour plus d’informations, voir [Introduction aux Services de domaine Active Directory & #40; Les services AD DS & #41; La virtualisation & #40; Niveau 100 & #41; ](../../ad-ds/Introduction-to-Active-Directory-Domain-Services-AD-DS-Virtualization-Level-100.md).  
  
## <a name="BKMK_Admin"></a>Administration des serveurs Windows Server 2012  
Utilisez le [à distance Server Outils d’Administration pour Windows 8](https://www.microsoft.com/download/details.aspx?id=28972) pour gérer les contrôleurs de domaine et d’autres serveurs qui exécutent Windows Server 2012. Vous pouvez exécuter les outils Administration de serveur distant de Windows Server 2012 sur un ordinateur qui exécute Windows 8.  
  
## <a name="BKMK_AppCompat"></a>Compatibilité des applications  
Le tableau suivant décrit les applications Microsoft intégrées à Active Directory courantes. Le tableau indique les versions de Windows Server que les applications peuvent être installées sur et indique si l’introduction des contrôleurs de domaine Windows Server 2012 a une incidence sur la compatibilité des applications.  
  
|Produit|Notes de publication|  
|-----------|---------|  
|[Microsoft Configuration Manager 2007](http://blogs.technet.com/b/configmgrteam/archive/2012/09/10/support-questions-about-windows-8-and-windows-server-2012.aspx)|Configuration Manager 2007 avec SP2 (inclut Configuration Manager 2007 R2 et Configuration Manager 2007 R3):<br /><br />-Windows 8 Professionnel<br />-Windows 8 entreprise<br />-Windows Server 2012 Standard<br />-Windows Server 2012 Datacenter **Remarque:** si ces seront être entièrement pris en charge en tant que clients, il n’existe aucun plan d’ajouter la prise en charge pour les déployer en tant que systèmes d’exploitation à l’aide de la fonctionnalité de déploiement de système d’exploitation de Configuration Manager 2007. En outre, aucun serveur de site ou les systèmes de site ne prendra en charge sur n’importe quel référence (SKU) de Windows Server 2012.|  
|[Microsoft SharePoint 2007](https://support.microsoft.com/kb/2728964)|Microsoft Office SharePoint Server 2007 n’est pas prise en charge pour l’installation sur Windows Server 2012.|  
|[Microsoft SharePoint 2010](https://support.microsoft.com/kb/2724471)|SharePoint 2010 Service Pack 2 est requis pour installer et utiliser <br />SharePoint 2010 sur les serveurs Windows Server 2012<br /><br />SharePoint 2010 Foundation Service Pack 2 est requis pour installer et faire fonctionner SharePoint 2010 Foundation sur les serveurs Windows Server 2012<br /><br />Le processus d’installation de SharePoint Server 2010 (sans les service packs) échoue sur Windows Server 2012<br /><br />Le programme d’installation requis de SharePoint Server 2010 (PrerequisiteInstaller.exe) échoue avec l’erreur «ce programme présente des problèmes de compatibilité.» En cliquant sur «Exécuter le programme sans aide» affiche l’erreur «vérification si SharePoint peut être installé & #124; SharePoint Server 2010 (sans service packs) ne peut pas être installé sur Windows Server 2012.»|  
|[Microsoft SharePoint 2013](https://technet.microsoft.com/library/cc262485(v=office.15).aspx)|Configuration minimale requise pour un serveur de base de données dans une batterie de serveurs<br /><br />L’Édition 64 bits de Windows Server 2008 R2 Service Pack 1 (SP1) Standard, Enterprise, Datacenter ou l’Édition 64 bits de Windows Server 2012 Standard ou Datacenter<br /><br />Configuration minimale requise pour un serveur unique avec une base de données intégrée:<br /><br />L’Édition 64 bits de Windows Server 2008 R2 Service Pack 1 (SP1) Standard, Enterprise, Datacenter ou l’Édition 64 bits de Windows Server 2012 Standard ou Datacenter<br /><br />Configuration minimale requise pour les serveurs web frontaux et les serveurs d’applications dans une batterie de serveurs:<br /><br />L’Édition 64 bits de Windows Server 2008 R2 Service Pack 1 (SP1) Standard, Enterprise, Datacenter ou l’Édition 64 bits de Windows Server 2012 Standard ou Datacenter.|  
|[Microsoft System Center Configuration Manager 2012](http://blogs.technet.com/b/configmgrteam/archive/2012/09/10/support-questions-about-windows-8-and-windows-server-2012.aspx)|System Center 2012 Configuration Manager Service Pack 1:<br /><br />Microsoft ajoutera les systèmes d’exploitation suivants à notre matrice de prise en charge des clients avec la version de Service Pack 1:<br /><br />-Windows 8 Professionnel<br />-Windows 8 entreprise<br />-Windows Server 2012 Standard<br />-Windows Server 2012 Datacenter<br /><br />Tous les rôles de serveur de site - y compris les serveurs de site, les fournisseurs SMS et les points de gestion - peuvent être déployées vers les serveurs avec les éditions du système d’exploitation suivantes:<br /><br />-Windows Server 2012 Standard<br />-Windows Server 2012 Datacenter|  
|[Microsoft Lync Server 2013](https://technet.microsoft.com/library/gg412883.aspx)|Lync Server 2013 requiert Windows Server 2008 R2 ou Windows Server 2012. Il ne peut pas être exécuté sur une installation Server Core. Il peut être exécuté sur [serveurs virtuels](https://technet.microsoft.com/library/gg399035.aspx).|  
|[Lync Server 2010](https://support.microsoft.com/kb/2777359)|Lync Server 2010 peut être installé sur une nouvelle installation (et non une mise à niveau) de Windows Server 2012 si [octobre 2012 les mises à jour cumulatives pour Lync Server](https://support.microsoft.com/?kbid=2493736) sont installés. La mise à niveau le système d’exploitation vers Windows Server 2012 pour une installation existante de Lync Server 2010 n’est pas pris en charge. Serveur de conversation Microsoft Lync Server 2010 groupe n’est pas également pris en charge sur Windows Server 2012.|  
|[System Center 2012 Endpoint Protection](http://blogs.technet.com/b/configmgrteam/archive/2012/09/10/support-questions-about-windows-8-and-windows-server-2012.aspx)|System Center 2012 Endpoint Protection Service Pack 1 mettra à jour la matrice de prise en charge des clients pour inclure les systèmes d’exploitation suivants<br /><br />-Windows 8 Professionnel<br />-Windows 8 entreprise<br />-Windows Server 2012 Standard<br />-Windows Server 2012 Datacenter|  
|[System Center 2012 Forefront Endpoint Protection](http://blogs.technet.com/b/configmgrteam/archive/2012/09/10/support-questions-about-windows-8-and-windows-server-2012.aspx)|FEP 2010 avec le correctif cumulatif 1 mettra à jour la matrice de prise en charge des clients pour inclure les systèmes d’exploitation suivants:<br /><br />-Windows 8 Professionnel<br />-Windows 8 entreprise<br />-Windows Server 2012 Standard<br />-Windows Server 2012 Datacenter|  
|Forefront Threat Management Gateway (TMG)|TMG est prise en charge pour exécuter uniquement sur Windows Server 2008 et Windows Server 2008 R2. Pour plus d’informations, voir [configuration système requise pour Forefront TMG](https://technet.microsoft.com/library/dd896981.aspx).|  
|WindowsServerUpdateServices|Cette version de WSUS prend déjà en charge les ordinateurs basés sur Windows 8 ou Windows Server 2012 en tant que clients.|  
|Windows Server Update Services 3.0|Article de mise à jour KB [2734608](https://support.microsoft.com/kb/2734608) permet aux serveurs qui exécutent Windows Server Update Services (WSUS) 3.0 SP2 fournissent des mises à jour sur les ordinateurs qui exécutent Windows 8 ou Windows Server 2012: **Remarque:** les clients avec des environnements WSUS 3.0 SP2 autonomes ou des environnements de System Center Configuration Manager 2007 Service Pack 2 avec WSUS 3.0 SP2 nécessitent [2734608](https://support.microsoft.com/kb/2734608) pour gérer correctement les ordinateurs basés sur Windows 8 ou Windows Server 2012 sur des ordinateurs en tant que clients.|  
|[Exchange 2013](https://technet.microsoft.com/library/bb691354.aspx)|Windows Server 2012 Standard et Datacenter sont prises en charge pour les rôles suivants: contrôleur de schéma, serveur de catalogue global, contrôleur de domaine, rôle de serveur d’accès client et de la boîte aux lettres<br /><br />Niveau fonctionnel de forêt: Windows Server 2003 ou version ultérieure<br /><br />Source: Configuration requise Exchange 2013|  
|Exchange 2010|[Source: Exchange 2010 Service Pack 3](https://blogs.technet.com/b/exchange/archive/2012/09/25/announcing-exchange-2010-service-pack-3.aspx)<br /><br />Exchange 2010 avec Service Pack 3 peut être installé sur les serveurs membres Windows Server 2012.<br /><br />[Configuration système requise pour Exchange 2010](https://technet.microsoft.com/library/aa996719(EXCHG.141).aspx) répertorie le dernier schéma pris en charge maître, global catalogue et contrôleur de domaine en tant que Windows Server 2008 R2.<br /><br />Niveau fonctionnel de forêt: Windows Server 2003 ou version ultérieure|  
|SQL Server 2012|Source: Ko [2681562](https://support.microsoft.com/kb/2681562)<br /><br />SQL Server 2012 RTM est pris en charge sur Windows Server 2012.|  
|SQL Server 2008 R2|Source: Ko [2681562](https://support.microsoft.com/kb/2681562)<br /><br />Requiert SQL Server 2008 R2 avec Service Pack 1 ou version ultérieure pour une installation sur Windows Server 2012.|  
|SQL Server 2008|Source: Ko [2681562](https://support.microsoft.com/kb/2681562)<br /><br />Requiert SQL Server 2008 avec Service Pack 3 ou version ultérieure pour une installation sur Windows Server 2012.|  
|SQL Server 2005|Source: Ko [2681562](https://support.microsoft.com/kb/2681562)<br /><br />Pas de prise en charge pour l’installation sur Windows Server 2012.|  
  
## <a name="BKMK_KnownIssues"></a>Problèmes connus  
Le tableau suivant répertorie les problèmes connus liés à l’installation des services AD DS.  
  
||||  
|-|-|-|  
|Titre et le numéro d’article de base de connaissances|Domaine technologique concerné|Description du problème /|  
|[2830145](https://support.microsoft.com/kb/2830145): SID S-1-18-1 et SID S-1-18-2 ne peuvent pas être mappés sur les ordinateurs Windows Server 2008 R2 dans un environnement de domaine ou de Windows 7|AD DS gestion/compatibilité des applications|Les applications qui mappent SID S-1-18-1 et SID S-1-18-2, qui sont nouvelles dans Windows Server 2012, risquent d’échouer car les identificateurs de sécurité ne peut pas être résolu sur les ordinateurs Windows 7 ou Windows Server 2008 R2. Pour résoudre ce problème, installez le correctif logiciel sur les ordinateurs basés sur Windows Server 2008 R2 et Windows 7 dans le domaine.|  
|[2737129](https://support.microsoft.com/kb/2737129): préparation de la stratégie de groupe n’est pas effectuée lorsque vous préparez automatiquement un domaine existant pour Windows Server 2012|Installation du service d’annuaire Active Directory|Adprep /domainprep /gpprep n’est pas exécutée automatiquement dans le cadre de l’installation du premier contrôleur de domaine qui exécute Windows Server 2012 dans un domaine. Si elle a jamais été exécutée auparavant dans le domaine, il doit être exécuté manuellement.|  
|[2737416](https://support.microsoft.com/kb/2737416): Windows déploiement de contrôleur de domaine PowerShell multiplie les avertissements|Installation du service d’annuaire Active Directory|Avertissements peuvent apparaître lors de la validation de la configuration requise et puis réapparaître pendant l’installation.|  
|[2737424](https://support.microsoft.com/kb/2737424): erreur «Le format du nom de domaine spécifié n’est pas valide» lorsque vous tentez de supprimer les Services de domaine Active Directory à partir d’un contrôleur de domaine|Installation du service d’annuaire Active Directory|Cette erreur apparaît si vous supprimez le dernier contrôleur de domaine dans un domaine où les comptes de contrôleur créés au préalable existent toujours. Ce problème concerne Windows Server 2012, Windows Server 2008 R2 et Windows Server 2008.|  
|[2737463](https://support.microsoft.com/kb/2737463): contrôleur de domaine ne démarre pas, erreur c00002e2 se produit, ou «Choisir une option» s’affiche.|Installation du service d’annuaire Active Directory|Un contrôleur de domaine ne démarre pas, car un administrateur a utilisé Dism.exe, Pkgmgr.exe ou Ocsetup.exe pour supprimer le rôle DirectoryServices-DomainController.|  
|[2737516](https://support.microsoft.com/kb/2737516): limites de vérification dans le Gestionnaire de serveur Windows Server 2012|Installation du service d’annuaire Active Directory|Vérification IFM peut avoir des limites comme expliqué dans l’article.|  
|[2737535](https://support.microsoft.com/kb/2737535): applet de commande install-AddsDomainController retourne le paramètre défini erreur pour RODC|Installation du service d’annuaire Active Directory|Vous pouvez recevoir une erreur lorsque vous essayez d’attacher un serveur à un compte RODC si vous spécifiez des arguments qui figurent déjà sur le compte RODC précréé.|  
|[2737560](https://support.microsoft.com/kb/2737560): «Impossible d’effectuer le contrôle de conflit de schéma Exchange» Échec de la vérification d’erreur et configuration requise|Installation du service d’annuaire Active Directory|Vérification de la configuration requise échoue lorsque vous configurez le premier Windows Server 2012 du contrôleur de domaine dans un domaine existant, car les contrôleurs de domaine sont manquantes le SeServiceLogonRight pour le Service réseau ou parce que les protocoles WMI ou DCOM sont bloqués.|  
|[2737797](https://support.microsoft.com/kb/2737797): le module AddsDeployment avec l’argument - Whatif affiche des résultats DNS incorrects|Installation du service d’annuaire Active Directory|Le paramètre - WhatIf affiche DNS server ne sera pas installé, mais il sera.|  
|[2737807](https://support.microsoft.com/kb/2737807): le bouton suivant n’est pas disponible sur la page Options du contrôleur de domaine|Installation du service d’annuaire Active Directory|Le bouton suivant est désactivé sur la page Options du contrôleur de domaine, car l’adresse IP du contrôleur de domaine cible ne mappe à un sous-réseau existant ou un site, ou parce que le mot de passe DSRM n’est pas tapé et confirmé correctement.|  
|[2737935](https://support.microsoft.com/kb/2737935): installation d’active Directory s’arrête au niveau de l’étape «Création de l’objet Paramètres NTDS»|Installation du service d’annuaire Active Directory|L’installation se bloque, car le mot de passe administrateur local correspond à l’administrateur de domaine, ou parce que des problèmes réseau empêchent de fin de la réplication critique.|  
|[2738060](https://support.microsoft.com/kb/2738060): «Accès refusé» message d’erreur lorsque vous créez un domaine enfant à distance à l’aide de Install-AddsDomain|Installation du service d’annuaire Active Directory|Vous recevez l’erreur lorsque vous exécutez Install-ADDSDomain avec l’applet de commande Invoke-Command si DNSDelegationCredential a un mot de passe incorrect.|  
|[2738697](https://support.microsoft.com/kb/2738697): «le serveur n’est pas opérationnel» erreur contrôleur de domaine configuration lorsque vous configurez un serveur à l’aide du Gestionnaire de serveur|Installation du service d’annuaire Active Directory|Vous recevez cette erreur lorsque vous tentez d’installer les services AD DS sur un ordinateur de groupe de travail, car l’authentification NTLM est désactivée.|  
|[2738746](https://support.microsoft.com/kb/2738746): vous recevez des erreurs d’accès refusé après avoir une session sur un compte de domaine d’administrateur local|Installation du service d’annuaire Active Directory|Lorsque vous ouvrez une session à l’aide d’un compte d’administrateur local plutôt que le compte administrateur intégré et ensuite créez un nouveau domaine, le compte n’est pas ajouté au groupe Admins du domaine.|  
|[2743345](https://support.microsoft.com/kb/2743345): «le système ne peut pas trouver le fichier spécifié», erreur Adprep/gpprep, ou outil se bloque|Installation du service d’annuaire Active Directory|Vous recevez cette erreur lorsque vous exécutez adprep/gpprep, car le maître d’infrastructure est implémente un espace de noms disjoint|  
|[2743367](https://support.microsoft.com/kb/2743367): Adprep «pas une application Win32 valide» erreur sur Windows Server 2003, version 64 bits|Installation du service d’annuaire Active Directory|Vous recevez cette erreur, car Windows Server 2012 Adprep ne peut pas être exécuté sur Windows Server 2003.|  
|[2753560](https://support.microsoft.com/kb/2753560): ADMT 3.2 et PES 3.1 erreurs d’installation sur Windows Server 2012|ADMT|ADMT 3.2 ne peut pas être installé sur Windows Server 2012 par conception.|  
|[2750857](https://support.microsoft.com/kb/2750857): la réplication DFS les rapports de diagnostic ne s’affichent pas correctement dans Internet Explorer 10|Réplication DFS|Rapport de Diagnostics de la réplication DFS ne s’affiche pas correctement en raison de modifications dans Internet Explorer 10.|  
|[2741537](https://support.microsoft.com/kb/2741537): mises à jour de la stratégie de groupe à distance sont visibles pour les utilisateurs|Stratégie de groupe|Cela est dû à des tâches planifiées s’exécutés dans le contexte de chaque utilisateur qui est connecté. La conception du Planificateur de tâches Windows requiert une invite interactive dans ce scénario.|  
|[2741591](https://support.microsoft.com/kb/2741591): les fichiers ADM ne sont pas présents dans SYSVOL dans l’option d’état d’Infrastructure de la console GPMC|Stratégie de groupe|Réplication de la stratégie de groupe peut signaler «réplication en cours», car l’état d’Infrastructure de la console GPMC ne suit pas les règles de filtrage personnalisées.|  
|[2737880](https://support.microsoft.com/kb/2737880): «Impossible de démarrer le service» une erreur lors de la configuration des services AD DS|Le clonage de contrôleur de domaine virtuel|Vous recevez cette erreur lors de l’installation ou suppression des services AD DS ou le clonage, car le service de serveur de rôles DS est désactivé.|  
|[2742836](https://support.microsoft.com/kb/2742836): deux baux DHCP est créés pour chaque contrôleur de domaine lorsque vous utilisez la fonctionnalité de clonage de VDC|Le clonage de contrôleur de domaine virtuel|Cela se produit car le contrôleur de domaine cloné a reçu un bail avant le clonage et quand le clonage a été terminé.|  
|[2742844](https://support.microsoft.com/kb/2742844): contrôleur de domaine le clonage échoue et le serveur redémarre en mode DSRM dans Windows Server 2012|Le clonage de contrôleur de domaine virtuel|Le contrôleur de domaine cloné démarre en mode DSRM, car le clonage a échoué pour une des diverses raisons répertoriées dans l’article.|  
|[2742874](https://support.microsoft.com/kb/2742874): le clonage de contrôleur de domaine ne recrée pas tous les noms principaux de service|Le clonage de contrôleur de domaine virtuel|Certains noms principaux de service trois parties ne sont pas recréés sur le contrôleur de domaine cloné en raison d’une limitation du processus de changement de nom de domaine.|  
|[2742908](https://support.microsoft.com/kb/2742908): erreur de «aucun serveur d’ouverture de session n’est disponible» après le clonage de contrôleur de domaine|Le clonage de contrôleur de domaine virtuel|Vous recevez cette erreur lorsque vous essayez d’ouvrir une session après le clonage d’un contrôleur de domaine virtualisé, car le clonage a échoué et le contrôleur de domaine est démarré en mode DSRM. Ouvrez une session en tant que. \administrator pour résoudre le problème de clonage.|  
|[2742916](https://support.microsoft.com/kb/2742916): le clonage de contrôleur de domaine échoue avec l’erreur 8610 dans dcpromo.log|Le clonage de contrôleur de domaine virtuel|Échec du clonage, car l’émulateur de contrôleur de domaine principal n’a pas effectué une réplication entrante de la partition de domaine, probablement parce que le rôle a été transféré.|  
|[2742927](https://support.microsoft.com/kb/2742927): «L’Index était hors limites» erreur New-AdDcCloneConfig|Le clonage de contrôleur de domaine virtuel|Vous recevez l’erreur après avoir exécuté l’applet de commande New-ADDCCloneConfigFile lors du clonage des contrôleurs de domaine virtuels, soit parce que l’applet de commande n’a pas été exécutée à partir d’une invite de commandes avec élévation de privilèges, car votre jeton d’accès ne contient-elle pas le groupe Administrateurs.|  
|[2742959](https://support.microsoft.com/kb/2742959): le clonage de contrôleur de domaine échoue avec l’erreur 8437: «paramètre non valide a été spécifié pour cette opération de réplication»|Le clonage de contrôleur de domaine virtuel|Le clonage a échoué car un nom de clone non valide ou un nom NetBIOS dupliqué a été spécifié.|  
|[2742970](https://support.microsoft.com/kb/2742970): clonage de contrôleur de domaine échoue avec aucun mode DSRM, ordinateur source et le clone en double|Le clonage de contrôleur de domaine virtuel|Le contrôleur de domaine virtuel cloné démarre dans Services de réparation Mode annuaire (DSRM), à l’aide d’un nom dupliqué comme contrôleur de domaine source car le fichier DCCloneConfig.xml n’a pas été créé à l’emplacement approprié ou parce que le contrôleur de domaine source a été redémarré avant le clonage.|  
|[2743278](https://support.microsoft.com/kb/2743278): erreur 0 x 80041005 de clonage de contrôleur de domaine|Le clonage de contrôleur de domaine virtuel|Le contrôleur de domaine cloné démarre en mode DSRM, car un seul serveur WINS a été spécifié. Si un serveur WINS est spécifié, les serveurs à la fois WINS préféré et auxiliaire doivent être spécifiés.|  
|[2745013](https://support.microsoft.com/kb/2745013): message d’erreur «Le serveur n’est pas opérationnel» si vous exécutez New-AdDcCloneConfigFile dans Windows Server 2012|Le clonage de contrôleur de domaine virtuel|Vous recevez cette erreur après avoir exécuté l’applet de commande New-ADDCCloneConfigFile, car le serveur ne peut pas contacter un serveur de catalogue global.|  
|[2747974](https://support.microsoft.com/kb/2747974): contrôleur de domaine le clonage de l’événement 2224 fournit des instructions incorrectes|Le clonage de contrôleur de domaine virtuel|ID d’événement 2224 indique à tort que les comptes de service administrés doivent être supprimés avant le clonage. Comptes de service administrés autonomes doivent être supprimés, mais administrés de groupe ne bloquent pas le clonage.|  
|[2748266](https://support.microsoft.com/kb/2748266): vous ne pouvez pas déverrouiller un lecteur chiffré par BitLocker après la mise à niveau vers Windows 8|BitLocker|Une erreur «Application introuvable» lorsque vous essayez de déverrouiller un lecteur sur un ordinateur qui a été mis à niveau à partir de Windows 7.|  
  
## <a name="see-also"></a>Voir aussi  
[Ressources d’évaluation de Windows Server 2012](https://technet.microsoft.com/evalcenter/hh708766.aspx)  
[Guide d’évaluation de Windows Server 2012](https://download.microsoft.com/download/5/B/2/5B254183-FA53-4317-B577-7561058CEF42/WS%202012%20Evaluation%20Guide.pdf)  
[Installer et déployer Windows Server 2012](https://technet.microsoft.com/library/hh831620.aspx)  
  


