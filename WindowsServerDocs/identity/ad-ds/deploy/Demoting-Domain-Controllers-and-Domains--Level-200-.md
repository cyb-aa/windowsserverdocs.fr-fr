---
ms.assetid: 65ed5956-6140-4e06-8d99-8771553637d1
title: "La rétrogradation de contrôleurs de domaine et de domaines (niveau 200)"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: c254a01da5534c1ddc673bc1e60382c166ddeda7
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="demoting-domain-controllers-and-domains-level-200"></a>La rétrogradation de contrôleurs de domaine et de domaines (niveau 200)

>S’applique à: Windows Server2016, Windows Server2012R2, Windows Server2012

Cette rubrique explique comment supprimer les services ADDS, à l’aide du Gestionnaire de serveur ou Windows PowerShell.  
  
-   [Workflow de suppression des services ADDS](../../ad-ds/deploy/Demoting-Domain-Controllers-and-Domains--Level-200-.md#BKMK_Workflow)  
  
-   [Windows PowerShell suppression de rôle et la rétrogradation](../../ad-ds/deploy/Demoting-Domain-Controllers-and-Domains--Level-200-.md#BKMK_PS)  
  
-   [Rétrograder](../../ad-ds/deploy/Demoting-Domain-Controllers-and-Domains--Level-200-.md#BKMK_Demote)  
  
## <a name="BKMK_Workflow"></a>Workflow de suppression des services ADDS  
![Rétrograder le contrôleur de domaine](media/Demoting-Domain-Controllers-and-Domains--Level-200-/adds_demotedomainforest.png)  
  
> [!CAUTION]  
> Suppression de rôles ADDS avec Dism.exe ou le module DISM Windows PowerShell après que la promotion du contrôleur de domaine n’est pas prise en charge et empêche le serveur de démarrer normalement.  
>   
> Contrairement aux Gestionnaire de serveur ou le module ADDSDeployment pour Windows PowerShell, DISM est un système de maintenance natif qui dispose d’aucune connaissance inhérente des services ADDS ou sa configuration. N’utilisez pas Dism.exe ou le module DISM Windows PowerShell pour désinstaller le rôle ADDS, sauf si le serveur n’est plus un contrôleur de domaine.  
  
## <a name="BKMK_PS"></a>Windows PowerShell suppression de rôle et la rétrogradation  
  
|||  
|-|-|  
|**ADDSDeployment et ServerManager applets de commande**|Arguments (**gras** arguments sont requis. *En italique* arguments peuvent être spécifiés à l’aide de Windows PowerShell ou l’Assistant de Configuration ADDS.)|  
|Uninstall-AddsDomainController|-SkipPreChecks<br /><br />*-LocalAdministratorPassword*<br /><br />-Confirmer<br /><br />***-Credential***<br /><br />-DemoteOperationMasterRole<br /><br />*-DNSDelegationRemovalCredential*<br /><br />-Force<br /><br />*-ForceRemoval*<br /><br />*-IgnoreLastDCInDomainMismatch*<br /><br />*-IgnoreLastDNSServerForZone*<br /><br />*-LastDomainControllerInDomain*<br /><br />-Norebootoncompletion<br /><br />*-RemoveApplicationPartitions*<br /><br />*-RemoveDNSDelegation*<br /><br />-RetainDCMetadata|  
|Désinstaller-WindowsFeature/Remove-WindowsFeature|***-Nom***<br /><br />***-IncludeManagementTools***<br /><br />*-Redémarrage*<br /><br />-Remove<br /><br />-Force<br /><br />-ComputerName<br /><br />-Credential<br /><br />-LogPath<br /><br />-Disque dur virtuel|  
  
> [!NOTE]  
> Le **-credential** argument est uniquement requis si vous ne sont pas déjà connecté en tant que membre du groupe Administrateurs de l’entreprise (rétrogradation du dernier contrôleur de domaine dans un domaine) ou le groupe Admins du domaine (rétrogradation d’un réplica DC).The **- includemanagementtools** argument est uniquement requis si vous souhaitez supprimer tous les utilitaires de gestion des services ADDS.  
  
## <a name="BKMK_Demote"></a>Rétrograder  
  
### <a name="remove-roles-and-features"></a>Supprimer des rôles et fonctionnalités  
Le Gestionnaire de serveur offre deux interfaces permettant de supprimer le rôle Services de domaine ActiveDirectory:  
  
-   Le **gérer** menu principal du tableau de bord, à l’aide de **suppression de rôles et fonctionnalités**  
  
 ![Rétrograder le contrôleur de domaine](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_RRW_TR_Manage.png)  
  
-   Cliquez sur **ADDS** ou **tous les serveurs** sur le volet de navigation. Faites défiler jusqu'à la **des rôles et fonctionnalités** section. Avec le bouton droit **les Services de domaine ActiveDirectory** dans les **des rôles et fonctionnalités** liste et cliquez sur **supprimer un rôle ou fonctionnalité **. Cette interface ignore la **sélection du serveur** page.  
  
 ![Rétrograder le contrôleur de domaine](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_RRW_TR_ServerSelection.png)  
  
Les applets de commande ServerManager **Uninstall-WindowsFeature** et **Remove-WindowsFeature** vous empêcher de supprimer le rôle ADDS jusqu'à ce que vous rétrogradez le contrôleur de domaine.  
  
### <a name="server-selection"></a>Sélection du serveur  
![Rétrograder le contrôleur de domaine](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_RRW_TR_ServerSelection2.png)  
  
Le **sélection du serveur** boîte de dialogue vous permet de choisir parmi un des serveurs précédemment ajoutés au pool, tant qu’il est accessible. Le serveur local exécutant le Gestionnaire de serveur est toujours automatiquement disponible.  
  
### <a name="server-roles-and-features"></a>Fonctionnalités et rôles de serveur  
![Rétrograder le contrôleur de domaine](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_RRW_TR_ServerRoles.png)  
  
Désactivez le **les Services de domaine ActiveDirectory** case à cocher pour rétrograder un contrôleur de domaine; si le serveur est actuellement un contrôleur de domaine, cela ne supprime pas le rôle ADDS et à la place bascule vers un **résultats de Validation** boîte de dialogue avec la proposition de rétrogradation. Dans le cas contraire, elle supprime simplement les fichiers binaires comme toute autre fonctionnalité de rôle.  
  
-   Ne supprimez pas tout autre des rôles ADDS liés ou fonctionnalités - tels que DNS, GPMC ou les outils RSAT - si vous envisagez de promouvoir le contrôleur de domaine à nouveau immédiatement. Suppression d’autres rôles et fonctionnalités augmente le temps de promouvoir à nouveau, le Gestionnaire de serveur réinstalle ces fonctionnalités lorsque vous réinstallez le rôle.  
  
-   Si vous envisagez de rétrograder le contrôleur de domaine définitivement, supprimez les rôles ADDS et des fonctionnalités à votre convenance. Pour cela, en désactivant les cases à cocher pour les rôles et fonctionnalités.  
  
    La liste complète des services ADDS-rôles et fonctionnalités associés sont les suivantes:  
  
    -   Fonctionnalité de Module active pour WindowsPowerShellActive  
  
    -   Fonctionnalité des outils ADDS et ADLDS  
  
    -   Fonctionnalité du centre d’administration ActiveDirectory  
  
    -   Fonctionnalité des services ADDS-composants logiciels enfichables et outils de ligne de commande  
  
    -   Serveur DNS  
  
    -   Console de gestion de stratégie de groupe  
  
Les applets de commande ADDSDeployment et ServerManager Windows PowerShell équivalentes sont:  
  
```  
Uninstall-addsdomaincontroller  
Uninstall-windowsfeature  
  
```  
  
![Rétrograder le contrôleur de domaine](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_RRW_TR_RemoveFeatures.png)  
  
![Rétrograder le contrôleur de domaine](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_RRW_TR_Demote.png)  
  
### <a name="credentials"></a>Informations d’identification  
![Rétrograder le contrôleur de domaine](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_RRW_TR_Credentials.png)  
  
Vous configurez les options de rétrogradation sur le **informations d’identification** page. Fournir les informations d’identification nécessaires pour effectuer la rétrogradation à partir de la liste suivante:  
  
-   La rétrogradation d’un contrôleur de domaine supplémentaire nécessite des informations d’identification d’administrateur de domaine. En sélectionnant **forcer la suppression de ce contrôleur de domaine** rétrograde le contrôleur de domaine sans supprimer les métadonnées de l’objet de contrôleur de domaine d’ActiveDirectory.  
  
    > [!WARNING]  
    > Ne sélectionnez pas cette option, sauf si le contrôleur de domaine ne peut pas contacter d’autres contrôleurs de domaine et qu’il existe *aucun moyen raisonnable* pour résoudre ce problème réseau. La rétrogradation forcée laisse des métadonnées orphelines dans ActiveDirectory sur les contrôleurs de domaine restants dans la forêt. En outre, toutes les modifications non répliquées sur ce contrôleur de domaine, tels que des mots de passe ou de nouveaux comptes d’utilisateur, sont définitivement perdues. Métadonnées orphelines sont la cause racine dans un pourcentage important des incidents de Support technique Microsoft pour les services ADDS, Exchange, SQL et autres logiciels.  
    >   
    > Si vous rétrogradez un contrôleur de domaine, vous *doit* manuellement effectuer immédiatement le nettoyage des métadonnées. Pour obtenir des instructions, consultez [nettoyage des métadonnées du serveur](https://technet.microsoft.com/library/cc816907(WS.10).aspx).  
  
   ![Rétrograder le contrôleur de domaine](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_RRW_TR_ForceDemote.png)  
  
-   La rétrogradation du dernier contrôleur de domaine dans un domaine nécessite l’appartenance au groupe Administrateurs de l’entreprise que cette opération supprime le domaine à proprement parler (si le dernier domaine dans la forêt, cette option supprime la forêt). Le Gestionnaire de serveur vous informe si le contrôleur de domaine actuel est le dernier contrôleur de domaine dans le domaine. Sélectionnez le **dernier contrôleur de domaine dans le domaine** case à cocher pour confirmer le contrôleur de domaine est le dernier contrôleur de domaine dans le domaine.  
  
Les arguments WindowsPowerShellADDSDeployment équivalents sont:  
  
```  
-credential <pscredential>  
-forceremoval <{ $true | false }>  
-lastdomaincontrollerindomain <{ $true | false }>  
  
```  
  
### <a name="warnings"></a>Avertissements  
![Rétrograder le contrôleur de domaine](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_RRW_TR_Warnings.png)  
  
Le **avertissements** page alerte vous informe des conséquences possibles de la suppression de ce contrôleur de domaine. Pour continuer, vous devez sélectionner **procéder à la suppression**.  
  
> [!WARNING]  
> Si vous avez sélectionné précédemment **forcer la suppression de ce contrôleur de domaine** sur le **informations d’identification** page, puis le **avertissements** page affiche tous les rôles d’opérations à maître unique flottant hébergés par ce contrôleur de domaine. Vous *doit* prendre les rôles à partir d’un autre contrôleur de domaine *immédiatement* après la rétrogradation de ce serveur. Pour plus d’informations sur la prise des rôles FSMO, voir [prendre le rôle de maître d’opérations](https://technet.microsoft.com/library/cc816779(WS.10).aspx).  
  
Cette page ne dispose pas d’argument WindowsPowerShellADDSDeployment équivalent.  
  
### <a name="removal-options"></a>Options de suppression  
![Rétrograder le contrôleur de domaine](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_RRW_TR_ReviewOptions.png)  
  
Le **Options de suppression** page s’affiche en fonction de la sélection précédente **dernier contrôleur de domaine dans le domaine** sur le **informations d’identification** page. Cette page vous permet de configurer d’autres options de suppression. Sélectionnez **ignorer le dernier serveur DNS pour zone**, **supprimer les partitions d’application**, et **supprimer la délégation DNS** pour exposer le **suivant** bouton.  
  
Les options apparaissent uniquement si applicables à ce contrôleur de domaine. Par exemple, s’il n’existe aucune délégation DNS pour ce serveur cette case à cocher n’affiche pas.  
  
Cliquez sur **modification** pour spécifier d’autres informations d’identification d’administration DNS. Cliquez sur **afficher les Partitions** pour afficher d’autres partitions que l’Assistant supprime pendant la rétrogradation. Par défaut, les seules autres partitions sont des Zones DNS de forêt et de domaine DNS. Toutes les autres partitions sont des partitions non-Windows.  
  
Les arguments d’applet de commande ADDSDeployment équivalents sont:  
  
```  
-ignorelastdnsserverforzone <{ $true | false }>  
-removeapplicationpartitions <{ $true | false }>  
-removednsdelegation <{ $true | false }>  
-dnsdelegationremovalcredential <pscredential>  
```  
  
### <a name="new-administrator-password"></a>Nouveau mot de passe administrateur  
![Rétrograder le contrôleur de domaine](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_RRW_TR_NewAdminPwd.png)  
  
Le **nouveau mot de passe administrateur** page vous oblige à fournir un mot de passe pour le compte d’administrateur de l’ordinateur local intégré, une fois la rétrogradation est terminée et l’ordinateur devient un serveur membre de domaine ou d’un ordinateur de groupe de travail.  
  
Le **Uninstall-ADDSDomainController** applet de commande et les arguments suivent les mêmes paramètres par défaut en tant que gestionnaire de serveur si non spécifié.  
  
Le **LocalAdministratorPassword** argument est spécial:  
  
-   Si *ne pas spécifié* en tant qu’argument, l’applet de commande vous invite à entrer et à confirmer un mot de passe masqué. Il s’agit du mode d’utilisation préféré lors de l’exécution interactive de l’applet de commande  
  
-   Si spécifié *avec une valeur*, la valeur doit être une chaîne sécurisée. Ce n’est pas le mode d’utilisation préféré cas d’exécution interactive de l’applet de commande  
  
Par exemple, vous pouvez demander manuellement un mot de passe à l’aide de la **Read-Host** applet de commande pour inviter l’utilisateur d’une chaîne sécurisée  
  
```  
-localadministratorpassword (read-host -prompt "Password:" -assecurestring)  
  
```  
  
> [!WARNING]  
> Comme les deux options précédentes ne confirment pas le mot de passe, faites preuve de prudence: le mot de passe n’est pas visible  
  
Vous pouvez également fournir une chaîne sécurisée en tant qu’une variable en texte clair convertie, bien que ceci soit fortement déconseillé. Par exemple:  
  
```  
-localadministratorpassword (convertto-securestring "Password1" -asplaintext -force)  
  
```  
  
> [!WARNING]  
> Fournir ou de stocker un mot de passe de texte en clair n’est pas recommandée. Toute personne qui exécute cette commande dans un script ou qui regarde par-dessus votre épaule connaît le mot de passe administrateur local de cet ordinateur. Avec cette information, ils ont accès à toutes ses données et peuvent emprunter l’identité de serveur lui-même.  
  
### <a name="confirmation"></a>Confirmation  
![Rétrograder le contrôleur de domaine](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_RRW_TR_Confirmation.png)  
  
Le **Confirmation** page indique la rétrogradation planifiée; la page ne répertorie pas les options de configuration de rétrogradation. Il s’agit de la dernière page de l’Assistant affiche avant le début de la rétrogradation. Le bouton Afficher le Script crée un script de rétrogradation Windows PowerShell.  
  
Cliquez sur **rétrograder** pour exécuter l’applet de commande de déploiement des services ADDS suivant:  
  
```  
Uninstall-DomainController  
  
```  
  
Utilisez l’option **Whatif** argument avec la **Uninstall-ADDSDomainController** et l’applet de commande pour passer en revue les informations de configuration. Cela vous permet de voir les valeurs explicites et implicites des arguments d’une applet de commande.  
  
Par exemple:  
  
![Rétrograder le contrôleur de domaine](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_PSUninstall.png)  
  
L’invite de redémarrage représente votre dernière possibilité d’annuler cette opération lors de l’utilisation de WindowsPowerShellADDSDeployment. Pour remplacer cette invite de commandes, utilisez la **-force** ou **confirmer: $false** arguments.  
  
### <a name="demotion"></a>Rétrogradation  
![Rétrograder le contrôleur de domaine](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_RRW_TR_Demotion.png)  
  
Lorsque le **rétrogradation** page s’affiche, la configuration de contrôleur de domaine commence et ne peut pas être interrompue ou annulée. Les opérations détaillées s’affichent sur cette page et écrivent dans les journaux:  
  
-   %Systemroot%\Debug\Dcpromo.log  
  
-   %SystemRoot%\debug\dcpromoui.log  
  
Dans la mesure où **Uninstall-AddsDomainController** et **Uninstall-WindowsFeature** uniquement comporter une action chacune, elles sont indiquées ici dans la phase de Confirmation avec les arguments requis minimaux. En appuyant sur entrée démarre le processus de rétrogradation irrévocable et redémarre l’ordinateur.  
  
![Rétrograder le contrôleur de domaine](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_PSUninstallConfirm.png)  
  
![Rétrograder le contrôleur de domaine](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_PSUninstallWindowsFeature.png)  
  
Pour accepter automatiquement l’invite de redémarrage, utilisez la **-force** ou **-confirmer: $false** arguments avec une applet de commande WindowsPowerShellADDSDeployment. Pour empêcher le serveur de redémarrer automatiquement à la fin de la promotion, utilisez le **- norebootoncompletion: $false** argument.  
  
> [!WARNING]  
> Il est déconseillé de remplacer le redémarrage. Le serveur membre doit redémarrer pour fonctionner correctement.  
  
![Rétrograder le contrôleur de domaine](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_PSUninstallFinished.png)  
  
Voici un exemple de rétrogradation forcée avec ses arguments requis minimaux **- forceremoval** et **- demoteoperationmasterrole**. Le **-credential** argument n’est pas nécessaire car l’utilisateur connecté en tant que membre du groupe Administrateurs de l’entreprise:  
  
![Rétrograder le contrôleur de domaine](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_PSUninstallExampleForce.png)  
  
Voici un exemple de suppression du dernier contrôleur de domaine dans le domaine avec ses arguments requis minimaux **- lastdomaincontrollerindomain** et **- removeapplicationpartitions**:  
  
![Rétrograder le contrôleur de domaine](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_PSUninstallExampleLastDC.png)  
  
Si vous tentez de supprimer le rôle ADDS avant de rétrograder le serveur, Windows PowerShell vous bloque avec une erreur intentionnelle:  
  
```  
Uninstall-WindowsFeature : An uninstallation prerequisite step failed duringthe removal of AD-Domain-Services, and uninstallation cannot continue.1. The domain controller needs to be demoted before the Active DirectoryDomain Services Role can be uninstalled.  
```  
  
![Rétrograder le contrôleur de domaine](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_PSUninstallError.png)  
  
> [!IMPORTANT]  
> Vous devez redémarrer l’ordinateur après la rétrogradation du serveur avant de pouvoir supprimer les binaires du rôle Services de domaine ActiveDirectory.  
  
### <a name="results"></a>Résultats  
![Rétrograder le contrôleur de domaine](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_RRW_TR_DemoteSignoff.png)  
  
Le **résultats** page indique la réussite ou l’échec de la promotion et toute information d’administration importante. Le contrôleur de domaine redémarre automatiquement après 10secondes.  
  

