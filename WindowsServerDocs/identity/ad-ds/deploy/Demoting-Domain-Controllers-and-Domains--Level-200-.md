---
ms.assetid: 65ed5956-6140-4e06-8d99-8771553637d1
title: Rétrogradation de contrôleurs de domaine et de domaines (niveau 200)
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 11/14/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 9b3db1390e8191451ef270ce29a2a37463b1de3c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59869770"
---
# <a name="demoting-domain-controllers-and-domains"></a>La rétrogradation de contrôleurs de domaine et de domaines

>S'applique à : Windows Server

Cette rubrique explique comment supprimer les services AD DS à l'aide du Gestionnaire de serveur ou de Windows PowerShell.
  
## <a name="ad-ds-removal-workflow"></a>Workflow de suppression de AD DS

![Graphique de flux de travail suppression AD DS](media/Demoting-Domain-Controllers-and-Domains--Level-200-/adds_demotedomainforest.png)  

> [!CAUTION]
> La suppression des rôles AD DS avec Dism.exe ou le module DISM Windows PowerShell après la promotion vers un contrôleur de domaine n'est pas prise en charge et empêche le démarrage normal du serveur.
>
> À la différence du Gestionnaire de serveur ou du module ADDSDeployment pour Windows PowerShell, DISM est un système de maintenance natif sans connaissance inhérente des services AD DS ni de leur configuration. N'utilisez pas Dism.exe ni le module DISM Windows PowerShell pour désinstaller le rôle AD DS sauf si le serveur n'est plus un contrôleur de domaine.

## <a name="demotion-and-role-removal-with-powershell"></a>Suppression de rôle et la rétrogradation avec PowerShell

|||  
|-|-|  
|**ADDSDeployment et ServerManager applets de commande**|Arguments (les arguments en **gras** sont obligatoires. Les arguments en *italique* peuvent être spécifiés à l'aide de Windows PowerShell ou de l'Assistant Configuration des services de domaine Active Directory.)|  
|Uninstall-AddsDomainController|-SkipPreChecks<br /><br />*-LocalAdministratorPassword*<br /><br />-Confirm<br /><br />***-Credential***<br /><br />-DemoteOperationMasterRole<br /><br />*-DNSDelegationRemovalCredential*<br /><br />-Force<br /><br />*-ForceRemoval*<br /><br />*-IgnoreLastDCInDomainMismatch*<br /><br />*-IgnoreLastDNSServerForZone*<br /><br />*-LastDomainControllerInDomain*<br /><br />-Norebootoncompletion<br /><br />*-RemoveApplicationPartitions*<br /><br />*-RemoveDNSDelegation*<br /><br />-RetainDCMetadata|  
|Uninstall-WindowsFeature/Remove-WindowsFeature|***-Name***<br /><br />***-IncludeManagementTools***<br /><br />*-Restart*<br /><br />-Remove<br /><br />-Force<br /><br />-ComputerName<br /><br />-Credential<br /><br />-LogPath<br /><br />-Vhd|  
  
> [!NOTE]  
> L'argument **-credential** n'est requis que si vous n'êtes pas déjà connecté en tant que membre du groupe Administrateurs de l'entreprise (rétrogradation du dernier contrôleur de domaine dans un domaine) ou du groupe Admins du domaine (rétrogradation d'un contrôleur de domaine répliqué). L'argument **-includemanagementtools** n'est requis que si vous voulez supprimer tous les utilitaires de gestion des services AD DS.  
  
## <a name="demote"></a>Rétrograder  
  
### <a name="remove-roles-and-features"></a>Supprimer des rôles et des fonctionnalités

Le Gestionnaire de serveur offre deux interfaces permettant de supprimer le rôle Services de domaine Active Directory :  
  
* Menu **Gérer** du tableau de bord principal, avec **Supprimer des rôles et fonctionnalités**  

   ![Gestionnaire de serveur - suppression de rôles et fonctionnalités](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_RRW_TR_Manage.png)  

* Cliquez sur **AD DS** ou **Tous les serveurs** dans le volet de navigation. Faites défiler les options jusqu'à la section **Rôles et fonctionnalités** . Cliquez avec le bouton droit sur **Services de domaine Active Directory** dans la liste **Rôles et fonctionnalités** et cliquez sur **Supprimer un rôle ou une fonctionnalité**. Cette interface ignore la page **Sélection du serveur** .  

   ![Gestionnaire de serveur - tous les serveurs - suppression de rôles et fonctionnalités](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_RRW_TR_ServerSelection.png)  

Les applets de commande ServerManager **Uninstall-WindowsFeature** et **Remove-WindowsFeature** vous empêche de supprimer le rôle AD DS jusqu'à ce que vous rétrogradez le contrôleur de domaine.
  
### <a name="server-selection"></a>Sélection du serveur

![Suppression de rôles Assistant et de fonctionnalités sélectionner le serveur de destination](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_RRW_TR_ServerSelection2.png)  

La boîte de dialogue **Sélection du serveur** vous permet de choisir l'un des serveurs précédemment ajoutés au pool, tant qu'il est accessible. Le serveur local exécutant le Gestionnaire de serveur est toujours automatiquement accessible.  

### <a name="server-roles-and-features"></a>Rôles et fonctionnalités de serveur

![Supprimer des rôles et fonctionnalités Assistant - sélectionnez des rôles à supprimer](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_RRW_TR_ServerRoles.png)  

Décochez la case **Services de domaine Active Directory** pour rétrograder un contrôleur de domaine ; si le serveur est actuellement un contrôleur de domaine, le rôle AD DS n'est pas supprimé, mais une boîte de dialogue **Résultats de la validation** s'affiche avec la proposition de rétrogradation. Sinon, cette opération supprime simplement les fichiers binaires comme toute autre fonctionnalité de rôle.  

* Ne supprimez aucun autre rôle ni fonctionnalité associés aux services de domaine Active Directory (par exemple, DNS, GPMC ou les outils RSAT) si vous envisagez de promouvoir à nouveau le contrôleur de domaine dans l'immédiat. La suppression d'autres rôles et fonctionnalités augmente le temps nécessaire à une nouvelle promotion, car le Gestionnaire de serveur réinstalle ces fonctionnalités quand vous réinstallez le rôle.  
* Supprimez les rôles et fonctionnalités des services de domaine Active Directory inutiles selon vos souhaits si vous envisagez de rétrograder le contrôleur de domaine de façon définitive. Vous devez pour cela décocher les cases associées à ces rôles et fonctionnalités.  

   La liste complète des rôles et fonctionnalités associés aux services de domaine Active Directory contient notamment :  
  
   * Module Active Directory pour la fonctionnalité Windows PowerShell  
   * Fonctionnalité des outils AD DS et AD LDS  
   * Fonctionnalité du Centre d'administration Active Directory  
   * Fonctionnalité des composants logiciels enfichables et outils en ligne de commande AD DS  
   * Serveur DNS  
   * Console de gestion des stratégies de groupe  
  
Les applets de commande Windows PowerShell ADDSDeployment et ServerManager équivalentes sont les suivantes :  
  
```
Uninstall-addsdomaincontroller  
Uninstall-windowsfeature  
```

![Supprimer des rôles et fonctionnalités Assistant - boîte de dialogue de Confirmation](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_RRW_TR_RemoveFeatures.png)  

![Supprimer des rôles et fonctionnalités Assistant - Validation](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_RRW_TR_Demote.png)  

### <a name="credentials"></a>Informations d’identification

![Active Directory domaine Assistant Configuration des Services - sélection des informations d’identification](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_RRW_TR_Credentials.png)  

La page **Informations d’identification** vous permet de configurer des options de rétrogradation. Entrez les informations d’identification nécessaires pour effectuer la rétrogradation à partir de la liste suivante :  

* La rétrogradation d’un contrôleur de domaine supplémentaire nécessite des informations d’identification d’administrateur de domaine. En sélectionnant **forcer la suppression de ce contrôleur de domaine** rétrograde le contrôleur de domaine sans supprimer les métadonnées de l’objet de contrôleur de domaine d’Active Directory.  

   > [!WARNING]  
   > Sélectionnez cette option uniquement si le contrôleur de domaine ne parvient pas à contacter d’autres contrôleurs de domaine et qu’*aucun moyen raisonnable* ne permet de résoudre ce problème réseau. La rétrogradation forcée laisse des métadonnées orphelines dans Active Directory sur les contrôleurs de domaine restants dans la forêt. Par ailleurs, toutes les modifications non répliquées sur ce contrôleur de domaine, notamment les mots de passe ou les nouveaux comptes d’utilisateur, sont définitivement perdues. Les métadonnées orphelines sont la cause première d’un grand nombre de problèmes soumis au support technique Microsoft pour AD DS, Exchange, SQL et d’autres logiciels.  
   >
   > Si vous rétrogradez de force un contrôleur de domaine, vous *devez* immédiatement effectuer un nettoyage manuel des métadonnées. Pour la procédure à suivre, voir [Nettoyage des métadonnées du serveur](https://technet.microsoft.com/library/cc816907(WS.10).aspx).  

   ![Active Directory domaine Assistant Configuration des Services - informations d’identification forcer la suppression](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_RRW_TR_ForceDemote.png)  
  
* La rétrogradation du dernier contrôleur de domaine dans un domaine nécessite l'appartenance au groupe Administrateurs de l'entreprise, cette opération entraînant la suppression du domaine à proprement parler (s'il s'agit du dernier domaine dans la forêt, la forêt est supprimée). Le Gestionnaire de serveur vous informe si le contrôleur de domaine actuel est le dernier contrôleur de domaine dans le domaine. Cochez la case **Dernier contrôleur de domaine du domaine** pour confirmer que le contrôleur de domaine est bien le dernier contrôleur de domaine dans le domaine.  

Les arguments Windows PowerShell ADDSDeployment équivalents sont les suivants :  

```
-credential <pscredential>  
-forceremoval <{ $true | false }>  
-lastdomaincontrollerindomain <{ $true | false }>  
```

### <a name="warnings"></a>Avertissements

![Assistant Configuration de Services du domaine Active Directory - impact sur les rôles FSMO informations d’identification](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_RRW_TR_Warnings.png)  

La page **Avertissements** vous informe des conséquences possibles de la suppression de ce contrôleur de domaine. Pour continuer, vous devez sélectionner **Procéder à la suppression**.

> [!WARNING]  
> Si vous avez auparavant sélectionné **Forcer la suppression de ce contrôleur de domaine** dans la page **Informations d'identification**, la page **Avertissements** affiche tous les rôles FSMO (Flexible Single Master Operations) hébergés par ce contrôleur de domaine. Vous *devez* prendre les rôles d'un autre contrôleur de domaine *immédiatement* après la rétrogradation de ce serveur. Pour plus d’informations sur la prise des rôles FSMO, voir [Prendre le rôle de maître d’opérations](https://technet.microsoft.com/library/cc816779(WS.10).aspx).

Cette page n'a pas d'argument Windows PowerShell ADDSDeployment équivalent.

### <a name="removal-options"></a>Options de suppression

![Active Directory domaine Services Assistant Configuration - DNS de supprimer des informations d’identification et les partitions d’Application](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_RRW_TR_ReviewOptions.png)  

La page **Options de suppression** s'affiche en fonction de la sélection précédente du **Dernier contrôleur de domaine du domaine** dans la page **Informations d'identification** . Cette page vous permet de configurer d'autres options de suppression. Sélectionnez **Ignorer le dernier serveur DNS pour zone**, **Supprimer les partitions d'application** et **Supprimer la délégation DNS** pour exposer le bouton **Suivant**.

Les options ne s'affichent que si elles sont applicables à ce contrôleur de domaine. Par exemple, s'il n'existe aucune délégation DNS pour ce serveur, cette case ne s'affiche pas.

Cliquez sur **Modifier** pour spécifier d'autres informations d'identification d'administration DNS. Cliquez sur **Afficher les partitions** pour afficher d'autres partitions que l'Assistant supprime pendant la rétrogradation. Par défaut, les seules autres partitions sont les zones DNS du domaine et DNS de la forêt. Toutes les autres partitions sont des partitions non-Windows.

Les arguments de l'applet de commande ADDSDeployment équivalents sont les suivants :  

```
-ignorelastdnsserverforzone <{ $true | false }>  
-removeapplicationpartitions <{ $true | false }>  
-removednsdelegation <{ $true | false }>  
-dnsdelegationremovalcredential <pscredential>  
```

### <a name="new-administrator-password"></a>Nouveau mot de passe d’administrateur

![Assistant Configuration de Services du domaine Active Directory - mot de passe administrateur nouvelles informations d’identification](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_RRW_TR_NewAdminPwd.png)  

Le **nouveau mot de passe administrateur** page vous oblige à fournir un mot de passe pour le compte d’administrateur de l’ordinateur local intégré, lorsque la rétrogradation est terminée et que l’ordinateur devient un serveur membre de domaine ou un ordinateur de groupe de travail.

L'applet de commande **Uninstall-ADDSDomainController** et les arguments suivent les mêmes paramètres par défaut que le Gestionnaire de serveur s'ils ne sont pas spécifiés.

L'argument **LocalAdministratorPassword** est spécial :

* S'il n'est *pas spécifié* en tant qu'argument, l'applet de commande vous invite à entrer et à confirmer un mot de passe masqué. Il s’agit du mode d’utilisation préféré en cas d’exécution interactive de l’applet de commande.
* S'il est spécifié *avec une valeur*, la valeur doit être une chaîne sécurisée. Il ne s’agit pas du mode d’utilisation préféré en cas d’exécution interactive de l’applet de commande.

Par exemple, vous pouvez inviter manuellement un mot de passe à l’aide de la **Read-Host** applet de commande pour inviter l’utilisateur d’une chaîne sécurisée.

```
-localadministratorpassword (read-host -prompt "Password:" -assecurestring)
```

> [!WARNING]
> Comme les deux options précédentes ne confirment pas le mot de passe, soyez très vigilant : le mot de passe n’est pas visible.

Vous pouvez également fournir une chaîne sécurisée sous forme d'une variable en texte clair convertie, bien que ceci soit fortement déconseillé. Exemple :

```
-localadministratorpassword (convertto-securestring "Password1" -asplaintext -force)
```

> [!WARNING]
> Il n’est pas recommandé de fournir ou de stocker un mot de passe en texte clair. Toute personne qui exécute cette commande dans un script ou qui regarde par-dessus votre épaule connaît le mot de passe d'administrateur local de cet ordinateur. Elle peut ensuite accéder à l'ensemble des données qu'il contient et emprunter l'identité du serveur lui-même.

### <a name="confirmation"></a>Confirmation

![Assistant Configuration de Services du domaine Active Directory - examiner les Options](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_RRW_TR_Confirmation.png)

La page **Confirmation** indique la rétrogradation planifiée ; elle ne répertorie pas les options de configuration de la rétrogradation. Il s'agit de la dernière page affichée par l'Assistant avant le début de la rétrogradation. Le bouton Afficher le script crée un script de rétrogradation Windows PowerShell.

Cliquez sur **Rétrograder** pour exécuter l'applet de commande de déploiement des services AD DS suivante :

```
Uninstall-DomainController
```

Utilisez l'argument **Whatif** facultatif avec **Uninstall-ADDSDomainController** et l'applet de commande pour passer en revue les informations de configuration. Cela vous permet de voir les valeurs explicites et implicites des arguments d'une applet de commande.

Exemple :

![Exemple de PowerShell désinstaller-ADDSDomainController](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_PSUninstall.png)

L'invite de redémarrage représente votre dernière possibilité d'annuler cette opération quand vous utilisez Windows PowerShell ADDSDeployment. Pour remplacer cette invite, utilisez les arguments **-force** ou **confirm:$false**.  

### <a name="demotion"></a>Rétrogradation

![Active Directory domaine Assistant Configuration des Services - rétrogradation en cours](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_RRW_TR_Demotion.png)  

Quand la page **Rétrogradation** s'affiche, la configuration du contrôleur de domaine commence et ne peut pas être arrêtée ni annulée. Les opérations détaillées s'affichent dans cette page et sont écrites dans les journaux :  

* %systemroot%\debug\dcpromo.log
* %systemroot%\debug\dcpromoui.log

Comme **Uninstall-AddsDomainController** et **Uninstall-WindowsFeature** n'ont qu'une action chacune, elles sont indiquées ici dans la phase de confirmation avec les arguments requis minimaux. En appuyant sur Entrée, vous démarrez le processus de rétrogradation irrévocable et redémarrez l'ordinateur.

![Exemple de PowerShell désinstaller-ADDSDomainController](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_PSUninstallConfirm.png)

![Exemple de PowerShell désinstaller-WindowsFeature](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_PSUninstallWindowsFeature.png)

Pour accepter automatiquement l'invite de redémarrage, utilisez les arguments **-force** ou **-confirm:$false** avec n'importe quelle applet de commande Windows PowerShell ADDSDeployment. Pour empêcher le serveur de redémarrer automatiquement à la fin de la promotion, utilisez l'argument **-norebootoncompletion:$false**.

> [!WARNING]
> Il n'est pas conseillé de remplacer le redémarrage. Le serveur membre doit redémarrer pour fonctionner correctement.

![Exemple de Force PowerShell désinstaller-ADDSDomainController](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_PSUninstallFinished.png)

Voici un exemple de rétrogradation forcée avec ses arguments requis minimaux pour **-forceremoval** et **-demoteoperationmasterrole**. L’argument **-credential** n’est pas requis, car l’utilisateur a ouvert une session en tant que membre du groupe Administrateurs de l’entreprise :

![Exemple de PowerShell désinstaller-ADDSDomainController](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_PSUninstallExampleForce.png)

Voici un exemple illustrant la suppression du dernier contrôleur de domaine dans le domaine avec ses arguments requis minimaux **-lastdomaincontrollerindomain** et **-removeapplicationpartitions**:

![PowerShell désinstaller-ADDSDomainController - LastDomainControllerInDomain exemple](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_PSUninstallExampleLastDC.png)

Si vous essayez de supprimer le rôle AD DS avant de rétrograder le serveur, Windows PowerShell vous bloque avec une erreur :

![Une étape de configuration requise de la désinstallation a échoué lors de la suppression des Services de domaine Active Directory, et la désinstallation ne peut pas continuer. 1. Le contrôleur de domaine doit être rétrogradé avant que le rôle de Services DirectoryDomain Active peut être désinstallé.](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_PSUninstallError.png)

> [!IMPORTANT]
> Vous devez redémarrer l'ordinateur après la rétrogradation du serveur avant de pouvoir supprimer les fichiers binaires du rôle AD-Domain-Services.

### <a name="results"></a>Résultats

![Vous êtes sur le point d’être déconnectés d’avertissement après la suppression des services AD DS](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_RRW_TR_DemoteSignoff.png)

La page **Résultats** indique la réussite ou l'échec de la promotion et toute information d'administration importante. Le contrôleur de domaine redémarre automatiquement après 10 secondes.
