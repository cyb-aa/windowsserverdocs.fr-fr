---
ms.assetid: 4d21d27d-5523-4993-ad4f-fbaa43df7576
title: "Gestion services avancée AD DS à l’aide du centre d’administration Active Directory (niveau 200)"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 19e83ce0123bd484c61d5a4522ebdd90cd40241e
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="advanced-ad-ds-management-using-active-directory-administrative-center-level-200"></a>Gestion services avancée AD DS à l’aide du centre d’administration Active Directory (niveau 200)

>S’applique à: Windows Server2016, Windows Server2012R2, Windows Server2012

Cette rubrique couvre le centre d’administration mis à jour Active Directory avec son nouveau la Corbeille Active Directory, stratégie de mot de passe affinées et Visionneuse de l’historique de Windows PowerShell plus en détail, notamment l’architecture, les exemples de tâches courantes et informations de dépannage. Pour obtenir une présentation, voir [Introduction aux améliorations du centre d’administration Active Directory & #40; Niveau 100 & #41; ](../../../ad-ds/get-started/adac/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-.md).  
  
-   [Architecture du centre d’administration Active Directory](../../../ad-ds/get-started/adac/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-.md#BKMK_Arch)  
  
-   [L’activation et la gestion de l’annuaire Active Directory de la Corbeille à l’aide du centre d’administration Active Directory](../../../ad-ds/get-started/adac/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-.md#BKMK_EnableRecycleBin)  
  
-   [Configuration et gestion des stratégies de mot de passe affinée à l’aide du centre d’administration Active Directory](../../../ad-ds/get-started/adac/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-.md#BKMK_FGPP)  
  
-   [À l’aide de la visionneuse de l’historique de PowerShell Windows Centre d’administration Active Directory](../../../ad-ds/get-started/adac/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-.md#BKMK_HistoryViewer)  
  
-   [Résolution des problèmes de gestion AD DS](../../../ad-ds/get-started/adac/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-.md#BKMK_Tshoot)  
  
## <a name="BKMK_Arch"></a>Architecture du centre d’administration Active Directory  
  
### <a name="adprep-executables-dlls"></a>ADPrep Exécutables, DLL  
Le module et l’architecture sous-jacente du centre d’administration Active Directory n’a pas changé avec la nouvelle corbeille AFFINÉES, fonctionnalités et de l’historique Observateur.  
  
-   Microsoft.ActiveDirectory.Management.UI.dll  
  
-   Microsoft.ActiveDirectory.Management.UI.resources.dll  
  
-   Microsoft.ActiveDirectory.Management.dll  
  
-   Microsoft.ActiveDirectory.Management.resources.dll  
  
-   ActiveDirectoryPowerShellResources.dll  
  
Le sous-jacent Windows PowerShell et couche d’opérations de la nouvelle fonctionnalité de Corbeille sont illustrés ci-dessous:  
  
![Gestion avancée AD DS](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/adds_adrestore.png)  
  
## <a name="BKMK_EnableRecycleBin"></a>L’activation et la gestion de l’annuaire Active Directory de la Corbeille à l’aide du centre d’administration Active Directory  
  
### <a name="capabilities"></a>Fonctionnalités  
  
-   Le Windows Server 2012 Active Directory Administrative Center vous permet de configurer et gérer la Corbeille Active Directory pour n’importe quelle partition de domaine dans une forêt. Il n’est plus nécessaire d’utiliser Windows PowerShell ni Ldp.exe pour activer la Corbeille Active Directory ou de restaurer des objets dans les partitions de domaine.  
  
-   Le centre d’administration Active Directory a avancé des critères de filtrage, la simplification de la restauration ciblée dans des environnements de grande taille comportant de nombreux objets supprimés de manière intentionnelle.  
  
### <a name="limitations"></a>Limitations  
  
-   Étant donné que le centre d’administration Active Directory peut uniquement gérer les partitions de domaine, il ne peut pas restaurer des objets supprimés des partitions Configuration, DNS du domaine ou forêt DNS (vous ne pouvez pas supprimer des objets à partir de la partition de schéma). Pour restaurer des objets de partitions de domaine, utilisez [Restore-ADObject](https://technet.microsoft.com/library/ee617262.aspx).  
  
-   Le centre d’administration Active Directory ne peut pas restaurer les sous-arbres d’objets en une seule action. Par exemple, si vous supprimez une unité d’organisation avec imbriqués des unités d’organisation, des utilisateurs, des groupes et des ordinateurs, la restauration de l’unité d’organisation de base ne restaure pas les objets enfants.  
  
    > [!NOTE]  
    > L’opération de restauration par lot centre d’administration Active Directory est un «meilleur effort» tri des objets supprimés *au sein de la sélection uniquement* donc parents sont donc classés avant les enfants pour la liste de restauration. Dans le cas de test simples, sous-arbres d’objets peuvent être restaurés en une seule action. Mais les cas particuliers, comme une sélection contenant des arbres partiels - arbres avec certains nœuds parents manquants - ou erreur cas, par exemple, en ignorant les objets enfants lors de la restauration du parent échoue, peuvent ne pas fonctionner comme prévu. Pour cette raison, vous devez toujours restaurer les sous-arbres d’objets en tant qu’une action distincte après avoir restauré les objets parents.  
  
La Corbeille Active Directory requiert un Windows Server 2008 R2 niveau fonctionnel de forêt, et vous devez être membre du groupe Administrateurs de l’entreprise. Une fois activée, vous ne pouvez pas désactiver la Corbeille Active Directory. La Corbeille Active Directory augmente la taille de la base de données Active Directory (NTDS. DIT) sur chaque contrôleur de domaine dans la forêt. Espace disque utilisé par la Corbeille continue d’augmenter au fil du temps, car il conserve les objets et toutes leurs données d’attribut.  
  
Pour plus d’informations, voir [Active Directory Step Guide de la Corbeille](https://technet.microsoft.com/library/dd392261(v=WS.10).aspx) et [évaluer la configuration matérielle requise (pour la Corbeille Active Directory)](https://technet.microsoft.com/library/cc753439(WS.10).aspx).  
  
Vous pouvez *inférieur* les forêt et du domaine niveaux fonctionnels de Windows Server 2012 vers Windows Server 2008 R2, même après avoir activé la Corbeille Active Directory. Cela nécessite l’utilisation du **Set-ADForestMode** et **Set-ADDomainMode** applets de commande Active Directory.  
  
Par exemple:  
  
```  
Set-AdForestMode -identity corp.contoso.com -server dc1.corp.contoso.com -forestmode Windows2008R2Forest  
Set-AdDomainMode -identity research.corp.contoso.com -server dc3.research.corp.contoso.com -domainmode Windows2008R2Domain  
  
```  
  
Vous ne pouvez pas utiliser le centre d’administration Active Directory pour effectuer ce changement: il déclenche uniquement des niveaux fonctionnels.  
  
### <a name="enabling-active-directory-recycle-bin-using-active-directory-administrative-center"></a>L’activation de la Corbeille Active Directory à l’aide du centre d’administration Active Directory  
Pour activer la Corbeille Active Directory, ouvrez le **centre d’administration Active Directory** et cliquez sur le nom de votre forêt dans le volet de navigation. À partir de la **tâches** volet, cliquez sur **activer la Corbeille**.  
  
![Gestion avancée AD DS](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_EnableRecycleBin.png)  
  
Le centre d’administration Active Directory montre la **Confirmation de la Corbeille activer la Corbeille** boîte de dialogue. Cette boîte de dialogue vous avertit que l’activation de la Corbeille est irréversible. Cliquez sur **OK** pour activer la Corbeille Active Directory. Le centre d’administration Active Directory montre une autre boîte de dialogue pour vous rappeler que la Corbeille Active Directory n’est pas entièrement fonctionnelle jusqu'à ce que tous les contrôleurs de domaine répliquent le changement de configuration.  
  
> [!IMPORTANT]  
> La possibilité d’activer la Corbeille Active Directory n’est pas disponible si:  
>   
> -   Le niveau fonctionnel de forêt est inférieur à Windows Server 2008 R2  
> -   Il est déjà activé  
  
L’applet de commande Active Directory Windows PowerShell équivalente est toujours:  
  
```  
Enable-ADOptionalFeature  
```  
  
Pour plus d’informations sur l’utilisation de Windows PowerShell pour activer la Corbeille Active Directory, voir la [Active Directory Step Guide de la Corbeille](https://technet.microsoft.com/library/dd392261(v=WS.10).aspx).  
  
### <a name="managing-active-directory-recycle-bin-using-active-directory-administrative-center"></a>La gestion de la Corbeille Active Directory à l’aide du centre d’administration Active Directory  
Cette section utilise l’exemple d’un domaine existant nommé **corp.contoso.com**. Ce domaine organise les utilisateurs en une unité d’organisation nommée parent **UserAccounts**. Le **UserAccounts** unité d’organisation contient trois unités d’organisation enfants nommées par service, lequel chaque contient d’autres unités d’organisation, des utilisateurs et groupes.  
  
![Gestion avancée AD DS](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_EnableRecycleBinExampleOU.png)  
  
#### <a name="storage-and-filtering"></a>Stockage et filtrage  
La Corbeille Active Directory conserve tous les objets supprimés dans la forêt. Elle enregistre ces objets en fonction de la **msDS-deletedObjectLifetime** attribut, qui par défaut est défini pour correspondre à la **tombstoneLifetime** attribut de la forêt. Dans toute forêt créée à l’aide de Windows Server 2003 SP1 ou version ultérieure, la valeur de **tombstoneLifetime** est défini sur 180 jours par défaut. Dans toute forêt mise à niveau à partir de Windows 2000 ou installée avec Windows Server 2003 (sans service pack), l’attribut tombstoneLifetime par défaut n’est pas défini et par conséquent, Windows utilise la valeur par défaut interne de 60 jours. Tout cela est configurable. Vous pouvez utiliser le centre d’administration Active Directory pour restaurer les objets supprimés des partitions de domaine de la forêt. Vous devez continuer à utiliser l’applet de commande **Restore-ADObject** pour restaurer les objets supprimés à partir d’autres partitions, telles que la Corbeille Active Directory rend configuration. l’activation du **objets supprimés** conteneur visible sous chaque partition de domaine dans le centre d’administration Active Directory.  
  
![Gestion avancée AD DS](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_DeletedObjectsContainer.png)  
  
Le **objets supprimés** conteneur affiche tous les objets peuvent être restaurées de cette partition de domaine. Objets supprimés antérieurs à **msDS-deletedObjectLifetime** sont des objets recyclés. Le centre d’administration Active Directory n’affiche pas les objets recyclés et vous ne pouvez pas restaurer ces objets à l’aide du centre d’administration Active Directory.  
  
Pour obtenir une explication plus approfondie de l’architecture et les règles de traitement de la Corbeille, voir [la Corbeille Active Directory: présentation, implémentation, recommandations et résolution des problèmes](http://blogs.technet.com/b/askds/archive/2009/08/27/the-ad-recycle-bin-understanding-implementing-best-practices-and-troubleshooting.aspx).  
  
Le centre d’administration Active Directory limite artificiellement le nombre par défaut des objets renvoyés à partir d’un conteneur à 20 000 objets. Vous pouvez augmenter cette limite à 100 000 objets en cliquant sur le **gérer** menu, puis **Options de gestion de la liste**.  
  
![Gestion avancée AD DS](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_MgmtList.png)  
  
#### <a name="restoration"></a>Restauration  
  
##### <a name="filtering"></a>Le filtrage  
Centre d’administration Active Directory offre des critères de puissantes options de filtrage et que vous devez vous familiariser avec avant de les utiliser dans une restauration réelle. Domaines supprimer intentionnellement nombreux objets pendant leur durée de vie. Avec une objet supprimé probablement durée de vie de 180 jours, vous ne pouvez pas restaurer tous les objets quand un accident se produit.  
  
![Gestion avancée AD DS](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_AddCriteria.png)  
  
Au lieu d’écrire des filtres LDAP complexes et convertir des valeurs UTC en dates et heures, utilisez la base et avancées **filtre** menu pour répertorier uniquement les objets pertinents. Si vous connaissez le jour de la suppression, les noms des objets ou toute autre donnée essentielle, utiliser à votre avantage lors du filtrage. Activer/désactiver les options de filtre avancées en cliquant sur le chevron à droite de la zone de recherche.  
  
L’opération de restauration prend en charge toutes les options de critères filtre standard, le même que les autres recherches. Parmi les filtres intégrés, les plus importants pour restaurer des objets sont généralement:  
  
-   *ANR (résolution de nom ambigu - non répertoriée dans le menu, mais ce qui est utilisé lorsque vous tapez dans la *** filtre *** zone)*  
  
-   Dernière modification entre des dates données  
  
-   Objet est unité utilisateur/inetorgperson/ordinateur/groupe/organisation  
  
-   Nom  
  
-   Lors de la suppression  
  
-   Dernier parent connu  
  
-   Type  
  
-   Description  
  
-   Ville  
  
-   Pays /region  
  
-   Service  
  
-   ID de l’employé  
  
-   Prénom  
  
-   Titre de la tâche  
  
-   Nom de famille  
  
-   SAMaccountname  
  
-   Département/Province  
  
-   Numéro de téléphone  
  
-   UPN  
  
-   LE code Postal  
  
Vous pouvez ajouter plusieurs critères. Par exemple, vous pouvez trouver tous les objets utilisateurs supprimés le 24 septembre<sup>th</sup> 2012 de Chicago, Illinois avec un titre du gestionnaire.  
  
Vous pouvez également ajouter, modifier ou réorganiser les en-têtes de colonne pour fournir plus de détails lors de l’évaluation des objets à récupérer.  
  
![Gestion avancée AD DS](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_ColumnHeaders.png)  
  
Pour plus d’informations sur la résolution de noms ambigu, voir [attributs ANR](https://msdn.microsoft.com/library/ms675092(VS.85).aspx).  
  
##### <a name="single-object"></a>Objet unique  
Restaurer des objets supprimés a toujours été une seule opération.  Le centre d’administration Active Directory facilite cette opération. Pour restaurer un objet supprimé, par exemple, un utilisateur unique:  
  
1.  Cliquez sur le nom de domaine dans le volet de navigation du centre d’administration Active Directory.  
  
2.  Double-cliquez sur **objets supprimés** dans la liste de gestion.  
  
3.  Cliquez sur l’objet, puis cliquez sur **restaurer**, ou cliquez sur **restaurer** à partir de la **tâches** volet.  
  
L’objet est restauré à son emplacement d’origine.  
  
![Gestion avancée AD DS](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_RestoreSingle.gif)  
  
Cliquez sur **restaurer sur … ** pour modifier l’emplacement de restauration. Cela est utile si le conteneur parent de l’objet supprimé a également été supprimé, mais vous ne souhaitez pas restaurer le parent.  
  
![Gestion avancée AD DS](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_RestoreToSingle.gif)  
  
##### <a name="multiple-peer-objects"></a>Plusieurs objets homologues  
Vous pouvez restaurer plusieurs objets au niveau de l’homologue, tels que tous les utilisateurs dans une unité d’organisation. Maintenez la touche CTRL enfoncée et cliquez sur un ou plusieurs objets supprimés à restaurer. Cliquez sur **restaurer** à partir du volet de tâches. Vous pouvez également sélectionner tous les objets affichés en maintenant enfoncée la touche CTRL et A enfoncées, ou une plage d’objets à l’aide de la touche MAJ ENFONCÉE et en cliquant sur.  
  
![Gestion avancée AD DS](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_RestorePeers.png)  
  
##### <a name="multiple-parent-and-child-objects"></a>Plusieurs objets parents et enfants  
Il est essentiel de comprendre le processus de restauration pour une restauration impliquant plusieurs parents-enfants car le centre d’administration Active Directory ne peut pas restaurer un arbre imbriqué d’objets supprimés en une seule action.  
  
1.  Restaurez l’objet supprimé en premier dans une arborescence.  
  
2.  Restaurez les enfants immédiats de cet objet parent.  
  
3.  Restaurez les enfants immédiats de ces objets parents.  
  
4.  Répétez cette procédure jusqu'à ce que tous les objets restaurer.  
  
Vous ne pouvez pas restaurer un objet enfant avant de restaurer son parent. Tentative de restauration renvoie l’erreur suivante:  
  
**L’opération n’a pas pu être effectuée car le parent de l’objet est instancié ou supprimé.**  
  
Le **dernier Parent connu** attribut montre la relation parent de chaque objet. Le **dernier Parent connu** attribut modifie vers l’emplacement restauré à partir de l’emplacement supprimé lorsque vous actualisez le centre d’administration Active Directory après la restauration d’un parent. Par conséquent, vous pouvez restaurer cet objet enfant lorsque l’emplacement d’un objet parent n’affiche plus le nom unique du conteneur d’objets supprimés.  
  
Considérez le scénario où un administrateur supprime accidentellement l’unité d’organisation Ventes, qui contient les unités d’organisation enfants et les utilisateurs.  
  
Observez d’abord la valeur de la **dernier Parent connu** attribut pour tous les utilisateurs supprimés et comment il lit * *unité d’organisation = Sales\0ADEL:*< nom unique guid + conteneur d’objets supprimés >***:  
  
![Gestion avancée AD DS](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_LastKnownParent.gif)  
  
Filtrer sur le nom ambigu ventes pour renvoyer l’unité d’organisation supprimée, que vous restaurez ensuite:  
  
![Gestion avancée AD DS](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_LastKnownParentSales.png)  
  
Actualisez le centre d’administration Active Directory pour voir attribut dernier Parent connu de l’objet utilisateur supprimé par le nom unique unité d’organisation Ventes restauré:  
  
![Gestion avancée AD DS](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_LastKnownParentSalesRestored.gif)  
  
Filtrer sur tous les utilisateurs ventes. Maintenez la touche CTRL et A enfoncées pour sélectionner tous les utilisateurs ventes supprimés. Cliquez sur **restaurer** pour déplacer les objets à partir de la **objets supprimés** conteneur à l’unité d’organisation ventes avec leurs appartenances aux groupes et les attributs intacts.  
  
![Gestion avancée AD DS](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_LastKnownParentSalesUndelete.png)  
  
Si le **ventes** unité d’organisation contenues unités d’organisation enfants qui lui sont propres, puis vous restaurez les unités d’organisation enfants avant de restaurer leurs enfants et ainsi de suite.  
  
Pour restaurer des objets supprimés imbriqués en spécifiant un conteneur parent supprimé, voir [annexe b: restaurer plusieurs objets Active Directory supprimés (exemple de Script)](https://technet.microsoft.com/library/dd379504(WS.10).aspx).  
  
L’applet de commande Active Directory Windows PowerShell pour restaurer des objets supprimés est la suivante:  
  
```  
Restore-adobject  
  
```  
  
Le **Restore-ADObject** fonctionnalités de l’applet de commande n’a pas changé entre Windows Server 2008 R2 et Windows Server 2012.  
  
##### <a name="server-side-filtering"></a>Filtrage côté serveur  
Il est possible qu’au fil du temps, le conteneur objets supprimés s’accumulent plus de 20 000 (ou même 100 000) objets dans les moyennes et grandes entreprises et avoir des difficultés à afficher tous les objets. Dans la mesure où le mécanisme de filtrage dans le centre d’administration Active Directory s’appuie sur le filtrage côté client, il ne peut pas afficher ces objets supplémentaires. Pour contourner cette limitation, utilisez les étapes suivantes pour effectuer une recherche côté serveur:  
  
1.  Bouton droit sur le **objets supprimés** conteneur et cliquez sur **Rechercher sous ce nœud**.  
  
2.  Cliquez sur le chevron pour afficher la **+ ajouter des critères** menu, sélectionnez et ajoutez **dernière modification entre des dates données**. L’heure de dernière modification (le **whenChanged** attribut) est une approximation de l’heure de suppression; dans la plupart des environnements, ils sont identiques. Cette requête effectue une recherche côté serveur.  
  
3.  Recherchez les objets supprimés à restaurer à l’aide supplémentaire affichage le filtrage, tri, et ainsi de suite dans les résultats et puis restaurez-les normalement.  
  
## <a name="BKMK_FGPP"></a>Configuration et gestion des stratégies de mot de passe affinée à l’aide du centre d’administration Active Directory  
  
### <a name="configuring-fine-grained-password-policies"></a>La configuration des stratégies de mot de passe affinée  
Le centre d’administration Active Directory vous permet de créer et gérer des objets de stratégie de mot de passe (affinée). Windows Server 2008 a introduit la fonctionnalité AFFINÉES mais Windows Server 2012 a la première interface de gestion graphique pour celui-ci. Vous appliquez des stratégies de mot de passe affinées au niveau du domaine et il permet de remplacer le mot de passe de domaine unique requis par Windows Server 2003. En créant différentes AFFINÉES avec différents paramètres, des utilisateurs ou groupes obtiennent différentes stratégies de mot de passe dans un domaine.  
  
Pour plus d’informations sur la stratégie de mot de passe affinée, voir [mot de passe affinées ADDS et la stratégie de verrouillage de compte Step-by-Step Guide (Windows Server2008R2)](https://technet.microsoft.com/library/cc770842(WS.10).aspx).  
  
Dans le volet de Navigation, cliquez sur arborescence, cliquez sur votre domaine **système**, cliquez sur **Password Settings Container**, puis, dans le volet tâches, cliquez sur **New** et **les paramètres de mot de passe**.  
  
![Gestion avancée AD DS](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_PasswordSettings.png)  
  
### <a name="managing-fine-grained-password-policies"></a>Gestion des stratégies de mot de passe affinée  
Création d’un nouveau FGPP ou modifier un existant fait apparaître les **les paramètres de mot de passe** éditeur. À partir de là, vous configurez toutes les stratégies de mot de passe souhaitées, comme il vous faudrait dans Windows Server 2008 ou Windows Server 2008 R2, désormais avec un éditeur spécifique intégré.  
  
![Gestion avancée AD DS](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_CreatePasswordSettings.png)  
  
Renseignez tous les champs (astérisque rouge) et les champs facultatifs, puis cliquez sur **ajouter** pour définir les utilisateurs ou groupes qui reçoit cette stratégie. AFFINÉES remplace les paramètres de stratégie de domaine par défaut pour les principaux de sécurité spécifiés. Dans la figure ci-dessus, une stratégie particulièrement restrictive s’applique uniquement au compte administrateur intégré, pour éviter toute méprise. La stratégie est bien trop complexe pour les utilisateurs standard pour se conformer, mais il est parfaite pour un compte à haut risque utilisé uniquement par les professionnels de l’informatique.  
  
Vous définissez également la priorité et les utilisateurs et les groupes de la stratégie s’applique au sein d’un domaine donné.  
  
![Gestion avancée AD DS](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_Precedence.png)  
  
Les applets de commande Active Directory Windows PowerShell pour la stratégie de mot de passe affinée sont:  
  
```  
Add-ADFineGrainedPasswordPolicySubject  
Get-ADFineGrainedPasswordPolicy  
Get-ADFineGrainedPasswordPolicySubject  
New-ADFineGrainedPasswordPolicy  
Remove-ADFineGrainedPasswordPolicy  
Remove-ADFineGrainedPasswordPolicySubject  
Set-ADFineGrainedPasswordPolicy  
  
```  
  
Fonctionnalité d’applet de commande de stratégie de mot de passe affinée n’a pas changé entre le serveur Windows Server 2008 R2 et Windows Server 2012. Dans un souci de commodité, le diagramme suivant illustre les arguments associés pour les applets de commande:  
  
![Gestion avancée AD DS](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_FGPP.gif)  
  
Le centre d’administration Active Directory vous permet également de rechercher l’ensemble d’AFFINÉES appliquée pour un utilisateur spécifique. Avec le bouton droit sur n’importe quel utilisateur, puis cliquez sur **afficher les paramètres de mot de passe résultants... ** pour ouvrir le *les paramètres de mot de passe* page qui s’applique à cet utilisateur via une affectation implicite ou explicite:  
  
![Gestion avancée AD DS](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_RSOP.png)  
  
Examiner le **propriétés** de n’importe quel utilisateur ou groupe montre le **directement associées les paramètres de mot de passe**, qui sont les FGPPs explicitement attribués:  
  
![Gestion avancée AD DS](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_FGPPSettings.gif)  
  
Affectation implicite d’AFFINÉES n’affiche pas. Pour cela, vous devez utiliser le **afficher les paramètres de mot de passe résultants... ** option.  
  
## <a name="BKMK_HistoryViewer"></a>À l’aide de la visionneuse de l’historique de PowerShell Windows Centre d’administration Active Directory  
La gestion future de Windows est Windows PowerShell. En disposant des outils graphiques au-dessus d’une infrastructure d’automatisation de tâches, la gestion des systèmes distribués plus complexes devient cohérente et efficace. Vous devez comprendre le fonctionnement de Windows PowerShell pour votre potentiel et optimiser vos investissements informatiques.  
  
Le centre d’administration Active Directory fournit désormais un historique complet de toutes les applets de commande Windows PowerShell qu’il s’exécute et leurs arguments et leurs valeurs. Vous pouvez copier l’historique de l’applet de commande ailleurs pour étude ou la modification et la réutilisation. Vous pouvez créer des notes tâche pour permettre d’isoler les votre centre d’administration a entraîné des commandes dans Windows PowerShell. Vous pouvez également filtrer l’historique pour trouver des points d’intérêt.  
  
De la visionneuse Active Directory Administrative Center Windows PowerShell l’historique vise vous permet d’apprendre en pratiquant.  
  
![Gestion avancée AD DS](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_HistoryViewer.gif)  
  
Cliquez sur le chevron (flèche) pour afficher la visionneuse de l’historique de Windows PowerShell.  
  
![Gestion avancée AD DS](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_RaiseViewer.png)  
  
Ensuite, créez un utilisateur ou modifier l’appartenance d’un groupe. La visionneuse de l’historique des mises à jour en permanence avec un affichage réduit de chaque applet de commande qui le centre d’administration Active Directory a été exécuté avec les arguments spécifiés.  
  
Développez un élément de ligne de vos centres d’intérêt pour afficher les valeurs fournies aux arguments de l’applet de commande:  
  
![Gestion avancée AD DS](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_ViewArgs.png)  
  
Cliquez sur le **démarrer une tâche** menu pour créer une note manuelle avant que le centre d’administration Active Directory vous permet de créer, modifier ou supprimer un objet. Tapez ce que vous faisiez.  Lorsque vous avez terminé la modification, sélectionnez **fin de tâche**. La note de tâche regroupe toutes les actions effectuées dans une note réductible, que vous pouvez utiliser pour mieux comprendre.  
  
Par exemple, pour afficher les commandes Windows PowerShell permet de modifier le mot de passe d’un utilisateur et le supprimer d’un groupe:  
  
![Gestion avancée AD DS](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_RemoveUser.gif)  
  
En cochant la case Afficher tout montre également Get-* verbe applets de commande Windows PowerShell uniquement récupérer des données.  
  
![Gestion avancée AD DS](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_ShowAll.png)  
  
La visionneuse de l’historique affiche le littéral commandes exécutées par le centre d’administration Active Directory et vous remarquerez peut-être que certaines applets de commande semblent s’exécuter inutilement. Par exemple, vous pouvez créer un nouvel utilisateur avec:  
  
```  
new-aduser   
```  
  
et n’avez pas besoin d’utiliser:  
  
```  
set-adaccountpassword  
enable-adaccount  
set-aduser  
  
```  
  
Conception du centre d’administration requis une modularité et une utilisation du code. Par conséquent, au lieu d’un ensemble de fonctions crée des utilisateurs et un autre ensemble modifie des utilisateurs existants, il remplit chaque fonction et puis les regroupe ainsi que les applets de commande. Gardez à l’esprit si vous étudiez Active Directory Windows PowerShell. Vous pouvez également utiliser qui comme technique d’apprentissage, où vous voyez comment simplement, vous pouvez utiliser Windows PowerShell pour effectuer une tâche unique.  
  
## <a name="BKMK_Tshoot"></a>Résolution des problèmes de gestion AD DS  
  
### <a name="introduction-to-troubleshooting"></a>Introduction à la résolution des problèmes  
En raison de sa nouveauté relative et l’absence de l’utilisation dans les environnements clients existants, le centre d’administration Active Directory a limité d’options de dépannage.  
  
### <a name="troubleshooting-options"></a>Options de dépannage  
  
#### <a name="logging-options"></a>Options de journalisation  
Le centre d’administration Active Directory contient désormais la journalisation intégrée dans Windows Server 2012, dans le cadre d’un fichier de configuration de suivi. Créer/modifier le fichier suivant dans le même dossier que dsac.exe:  
  
**Dsac.exe.config**  
  
Créez le contenu suivant:  
  
```  
<appSettings>  
  <add key="DsacLogLevel" value="Verbose" />  
</appSettings>  
<system.diagnostics>   
 <trace autoflush="false" indentsize="4">   
  <listeners>   
   <add name="myListener"   
    type="System.Diagnostics.TextWriterTraceListener"   
    initializeData="dsac.trace.log" />   
   <remove name="Default" />   
  </listeners>   
 </trace>   
</system.diagnostics>  
  
```  
  
Les niveaux de détail pour **DsacLogLevel** sont **aucun**, **erreur**, **avertissement**, **informations**, et **Verbose**. Le nom de fichier de sortie est configurable et écrit dans le même dossier que dsac.exe. La sortie peut indiquer vous en savoir plus sur comment ADAC fonctionne, les contrôleurs de domaine qu’il a contacté, les commandes Windows PowerShell exécutées, les réponses obtenues et autres détails.  
  
Par exemple, tout en utilisant le niveau INFO, qui renvoie tous les résultats sauf le détail au niveau du suivi:  
  
-   DSAC.exe démarre  
  
-   La journalisation démarre  
  
-   Contrôleur de domaine doit renvoyer les informations de domaine initial  
  
    ```  
    [12:42:49][TID 3][Info] Command Id, Action, Command, Time, Elapsed Time ms (output), Number objects (output)  
    [12:42:49][TID 3][Info] 1, Invoke, Get-ADDomainController, 2012-04-16T12:42:49  
    [12:42:49][TID 3][Info] Get-ADDomainController-Discover:$null-DomainName:"CORP"-ForceDiscover:$null-Service:ADWS-Writable:$null  
    ```  
  
-   Contrôleur de domaine DC1 a renvoyé depuis le domaine Corp  
  
-   PS Chargé lecteur virtuel AD  
  
    ```  
    [12:42:49][TID 3][Info] 1, Output, Get-ADDomainController, 2012-04-16T12:42:49, 1  
    [12:42:49][TID 3][Info] Found the domain controller 'DC1' in the domain 'CORP'.  
    [12:42:49][TID 3][Info] 2, Invoke, New-PSDrive, 2012-04-16T12:42:49  
    [12:42:49][TID 3][Info] New-PSDrive-Name:"ADDrive0"-PSProvider:"ActiveDirectory"-Root:""-Server:"dc1.corp.contoso.com"  
    [12:42:49][TID 3][Info] 2, Output, New-PSDrive, 2012-04-16T12:42:49, 1  
    [12:42:49][TID 3][Info] 3, Invoke, Get-ADRootDSE, 2012-04-16T12:42:49  
  
    ```  
  
-   Obtenir des informations de la DSE racine de domaine  
  
    ```  
    [12:42:49][TID 3][Info] Get-ADRootDSE  
    -Server:"dc1.corp.contoso.com"  
    [12:42:49][TID 3][Info] 3, Output, Get-ADRootDSE, 2012-04-16T12:42:49, 1  
    [12:42:49][TID 3][Info] 4, Invoke, Get-ADOptionalFeature, 2012-04-16T12:42:49  
  
    ```  
  
-   Obtenir des informations de domaine Active Directory la Corbeille  
  
    ```  
    [12:42:49][TID 3][Info] Get-ADOptionalFeature  
    -LDAPFilter:"(msDS-OptionalFeatureFlags=1)"  
    -Server:"dc1.corp.contoso.com"  
    [12:42:49][TID 3][Info] 4, Output, Get-ADOptionalFeature, 2012-04-16T12:42:49, 1  
    [12:42:49][TID 3][Info] 5, Invoke, Get-ADRootDSE, 2012-04-16T12:42:49  
    [12:42:49][TID 3][Info] Get-ADRootDSE  
    -Server:"dc1.corp.contoso.com"  
    [12:42:49][TID 3][Info] 5, Output, Get-ADRootDSE, 2012-04-16T12:42:49, 1  
    [12:42:49][TID 3][Info] 6, Invoke, Get-ADRootDSE, 2012-04-16T12:42:49  
    [12:42:49][TID 3][Info] Get-ADRootDSE  
    -Server:"dc1.corp.contoso.com"  
    [12:42:49][TID 3][Info] 6, Output, Get-ADRootDSE, 2012-04-16T12:42:49, 1  
    [12:42:49][TID 3][Info] 7, Invoke, Get-ADOptionalFeature, 2012-04-16T12:42:49  
    [12:42:49][TID 3][Info] Get-ADOptionalFeature  
    -LDAPFilter:"(msDS-OptionalFeatureFlags=1)"  
    -Server:"dc1.corp.contoso.com"  
    [12:42:50][TID 3][Info] 7, Output, Get-ADOptionalFeature, 2012-04-16T12:42:50, 1  
    [12:42:50][TID 3][Info] 8, Invoke, Get-ADForest, 2012-04-16T12:42:50  
  
    ```  
  
-   Obtenir la forêt Active Directory  
  
    ```  
    [12:42:50][TID 3][Info] Get-ADForest  
    -Identity:"corp.contoso.com"  
    -Server:"dc1.corp.contoso.com"  
    [12:42:50][TID 3][Info] 8, Output, Get-ADForest, 2012-04-16T12:42:50, 1  
    [12:42:50][TID 3][Info] 9, Invoke, Get-ADObject, 2012-04-16T12:42:50  
  
    ```  
  
-   Obtenir des informations de schéma pour les types de chiffrement pris en charge, AFFINÉES, certaines informations utilisateur  
  
    ```  
    [12:42:50][TID 3][Info] Get-ADObject  
    -LDAPFilter:"(|(ldapdisplayname=msDS-PhoneticDisplayName)(ldapdisplayname=msDS-PhoneticCompanyName)(ldapdisplayname=msDS-PhoneticDepartment)(ldapdisplayname=msDS-PhoneticFirstName)(ldapdisplayname=msDS-PhoneticLastName)(ldapdisplayname=msDS-SupportedEncryptionTypes)(ldapdisplayname=msDS-PasswordSettingsPrecedence))"  
    -Properties:lDAPDisplayName  
    -ResultPageSize:"100"  
    -ResultSetSize:$null  
    -SearchBase:"CN=Schema,CN=Configuration,DC=corp,DC=contoso,DC=com"  
    -SearchScope:"OneLevel"  
    -Server:"dc1.corp.contoso.com"  
    [12:42:50][TID 3][Info] 9, Output, Get-ADObject, 2012-04-16T12:42:50, 7  
    [12:42:50][TID 3][Info] 10, Invoke, Get-ADObject, 2012-04-16T12:42:50  
  
    ```  
  
-   Obtenir toutes les informations sur l’objet de domaine à afficher pour l’administrateur qui a cliqué sur l’en-tête du domaine.  
  
    ```  
    [12:42:50][TID 3][Info] Get-ADObject  
    -IncludeDeletedObjects:$false  
    -LDAPFilter:"(objectClass=*)"  
    -Properties:allowedChildClassesEffective,allowedChildClasses,lastKnownParent,sAMAccountType,systemFlags,userAccountControl,displayName,description,whenChanged,location,managedBy,memberOf,primaryGroupID,objectSid,msDS-User-Account-Control-Computed,sAMAccountName,lastLogonTimestamp,lastLogoff,mail,accountExpires,msDS-PhoneticCompanyName,msDS-PhoneticDepartment,msDS-PhoneticDisplayName,msDS-PhoneticFirstName,msDS-PhoneticLastName,pwdLastSet,operatingSystem,operatingSystemServicePack,operatingSystemVersion,telephoneNumber,physicalDeliveryOfficeName,department,company,manager,dNSHostName,groupType,c,l,employeeID,givenName,sn,title,st,postalCode,managedBy,userPrincipalName,isDeleted,msDS-PasswordSettingsPrecedence  
    -ResultPageSize:"100"  
    -ResultSetSize:"20201"  
    -SearchBase:"DC=corp,DC=contoso,DC=com"  
    -SearchScope:"Base"  
    -Server:"dc1.corp.contoso.com"  
  
    ```  
  
Si vous définissez le Verbose niveau montre également les piles .NET pour chaque fonction, mais celles-ci n’incluent pas de suffisamment de données pour être particulièrement utiles, sauf en cas de dépannage de Dsac.exe après une violation d’accès ou un incident.  
  
> [!IMPORTANT]  
> Il existe également une version hors-bande du service appelée le [passerelle de gestion Active Directory](https://www.microsoft.com/download/en/details.aspx?displaylang=en&id=2852), qui s’exécute sur Windows Server 2008 SP2 et Windows Server 2003 SP2.  
  
Les deux causes probables de ce problème sont:  
  
-   Le service ADWS n’est pas en cours d’exécution sur des contrôleurs de domaine accessible.  
  
-   Communications réseau sont bloquées vers les services Web Active Directory à partir de l’ordinateur exécutant le centre d’administration Active Directory  
  
Les erreurs affichées quand aucune instance des Services Web Active Directory n’est disponibles sont:  
  
|||  
|-|-|  
|Erreur|Opération|  
|«Impossible de se connecter à un domaine. Actualisez ou réessayez lorsque la connexion est disponible»|Affiché au démarrage de l’application du centre d’administration Active Directory|  
|«Impossible de trouver un serveur disponible dans le * <NetBIOS domain name> * domaine qui exécute le Service Web de Active Directory (ADWS)»|Apparaît lorsque vous essayez de sélectionner un nœud de domaine dans l’application du centre d’administration Active Directory|  
  
Pour résoudre ce problème, utilisez les étapes suivantes:  
  
1.  Valider les Services Web Active Directory sont démarrés sur au moins un contrôleur de domaine dans le domaine (et, de préférence, tous les contrôleurs de domaine dans la forêt). Assurez-vous qu’il est configuré pour démarrer automatiquement sur tous les contrôleurs de domaine également.  
  
2.  À partir de l’ordinateur exécutant le centre d’administration Active Directory, vérifiez que vous pouvez localiser un serveur exécutant les services Web Active Directory en exécutant des commandes NLTest.exe suivantes:  
  
    ```  
    nltest /dsgetdc:<domain NetBIOS name> /ws /force   
    nltest /dsgetdc:<domain fully qualified DNS name> /ws /force  
  
    ```  
  
    Si ces tests échouent même si le service ADWS est en cours d’exécution, le problème survient avec la résolution de noms ou LDAP et pas les services ADWS ou le centre d’administration Active Directory. Ce test échoue avec l’erreur «1355 0x54B ERROR_NO_SUCH_DOMAIN» si les services Web Active Directory n’est pas exécuté sur des contrôleurs de domaine mais, par conséquent, vérifiez à nouveau hâtive.  
  
3.  Sur le contrôleur de domaine renvoyé par NLTest, videz la liste des ports à l’écoute avec la commande:  
  
    ```  
    Netstat -anob > ports.txt  
  
    ```  
  
    Examinez le fichier ports.txt et vérifiez que le service ADWS est à l’écoute sur le port 9389. Exemple:  
  
    ```  
    TCP    0.0.0.0:9389    0.0.0.0:0    LISTENING    1828  
    [Microsoft.ActiveDirectory.WebServices.exe]  
  
    TCP    [::]:9389       [::]:0       LISTENING    1828  
    [Microsoft.ActiveDirectory.WebServices.exe]  
  
    ```  
  
    Si à l’écoute, valider les règles de pare-feu Windows et assurez-vous qu’elles autorisent le port TCP 9389 entrant. Par défaut, les contrôleurs de domaine activent la règle de pare-feu «Services Web Active Directory (TCP-in)». Si vous ne détecte ne pas, vérifiez à nouveau que le service est en cours d’exécution sur ce serveur et redémarrez-le. Vérifiez qu’aucun autre processus n’est déjà à l’écoute sur le port 9389.  
  
4.  Installez NetMon ou un autre utilitaire de capture réseau sur l’ordinateur exécutant le centre d’administration Active Directory et sur le contrôleur de domaine renvoyé par NLTEST. Rassemblez les captures du réseau simultanées depuis les deux ordinateurs, dans laquelle vous démarrez le centre d’administration Active Directory et que vous voyez l’erreur avant d’arrêter les captures. Vérifiez que le client est en mesure d’envoyer et recevoir à partir du contrôleur de domaine sur le port TCP 9389. Si les paquets sont envoyés mais n’arrivent jamais ou arrivent et le contrôleur de domaine répond mais ils n’atteignent jamais le client, il est probablement un pare-feu entre les ordinateurs sur le réseau supprime des paquets sur ce port. Ce pare-feu peut être logiciel ou matériel et peut faire partie d’un point de terminaison protection (antivirus) logiciel tiers.  
  
#### <a name="knownlikely-issues-and-support-scenarios"></a>Problèmes connus/probables et scénarios de prise en charge  
Le centre d’administration Active Directory probablement son seul problème est une incapacité à se connecter aux Active Directory Service (Web) en cours d’exécution sur un contrôleur de domaine Windows Server 2012 ou Windows Server 2008 R2.  
  
## <a name="see-also"></a>Voir aussi  
[Introduction aux améliorations du centre d’administration ActiveDirectory et #40; niveau 100 et #41;](../../../ad-ds/get-started/adac/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-.md)  
  

