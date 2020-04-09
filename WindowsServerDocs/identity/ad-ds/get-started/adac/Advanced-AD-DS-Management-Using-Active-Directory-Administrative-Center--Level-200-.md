---
ms.assetid: 4d21d27d-5523-4993-ad4f-fbaa43df7576
title: Gestion avancée des services AD DS à l’aide du Centre d’administration Active Directory (niveau200)
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/07/2018
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 197f994bdd5dedced24aa390dc562530c41e951d
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80824912"
---
# <a name="advanced-ad-ds-management-using-active-directory-administrative-center-level-200"></a>Gestion avancée des services AD DS à l’aide du Centre d’administration Active Directory (niveau200)

>S’applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Cette rubrique décrit la mise à jour du Centre d'administration Active Directory avec la nouvelle Corbeille Active Directory, la stratégie de mot de passe affinée ainsi que la Visionneuse de l'historique Windows PowerShell. Elle aborde notamment l'architecture, des exemples de tâches courantes et les informations de résolution des problèmes. Pour une introduction, consultez [Introduction à Centre d’administration Active Directory améliorations du &#40;niveau 100&#41;](../../../ad-ds/get-started/adac/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-.md).  
  
- [Architecture Centre d’administration Active Directory](../../../ad-ds/get-started/adac/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-.md#BKMK_Arch)  
- [Activation et gestion de la corbeille Active Directory à l’aide de Centre d’administration Active Directory](../../../ad-ds/get-started/adac/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-.md#BKMK_EnableRecycleBin)  
- [Configuration et gestion des stratégies de mot de passe affinées à l’aide de Centre d’administration Active Directory](../../../ad-ds/get-started/adac/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-.md#BKMK_FGPP)  
- [Utilisation de la visionneuse de l’historique de Windows PowerShell Centre d’administration Active Directory](../../../ad-ds/get-started/adac/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-.md#BKMK_HistoryViewer)  
- [Résolution des problèmes de gestion des AD DS](../../../ad-ds/get-started/adac/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-.md#BKMK_Tshoot)  
  
## <a name="active-directory-administrative-center-architecture"></a><a name="BKMK_Arch"></a>Architecture Centre d’administration Active Directory  
  
### <a name="active-directory-administrative-center-executables-dlls"></a>Exécutables Centre d’administration Active Directory, dll  

Le module et l'architecture sous-jacente du Centre d'administration Active Directory n'ont pas changé avec les nouvelles fonctionnalités : la Corbeille, la stratégie de mot de passe affinée et la Visionneuse de l'historique.  
  
- Microsoft.ActiveDirectory.Management.UI.dll  
- Microsoft.ActiveDirectory.Management.UI.resources.dll  
- Microsoft.ActiveDirectory.Management.dll  
- Microsoft.ActiveDirectory.Management.resources.dll  
- ActiveDirectoryPowerShellResources.dll  
  
Windows PowerShell et la couche d'opérations sous-jacents de la nouvelle fonctionnalité de Corbeille sont illustrés ci-dessous :  
  
![Gestion avancée des AD DS](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/adds_adrestore.png)  
  
## <a name="enabling-and-managing-the-active-directory-recycle-bin-using-active-directory-administrative-center"></a><a name="BKMK_EnableRecycleBin"></a>Activation et gestion de la corbeille Active Directory à l’aide de Centre d’administration Active Directory  
  
### <a name="capabilities"></a>Capacités  
  
- Le Centre d’administration Active Directory Windows Server 2012 ou plus récent vous permet de configurer et de gérer la corbeille Active Directory pour toute partition de domaine d’une forêt. Vous n'avez plus besoin d'utiliser Windows PowerShell ni Ldp.exe pour activer la Corbeille Active Directory ou restaurer des objets dans des partitions de domaine.
- Le Centre d'administration Active Directory dispose de critères de filtrage avancés, ce qui facilite la restauration ciblée dans des environnements de grande taille comportant de nombreux objets supprimés de manière intentionnelle.
  
### <a name="limitations"></a>Limitations  
  
- Le Centre d'administration Active Directory ne peut pas restaurer des objets supprimés des partitions Configuration, DNS du domaine ou DNS de la forêt car il peut uniquement gérer des partitions de domaine (vous ne pouvez pas supprimer d'objets de la partition Schéma). Pour restaurer des objets de partitions n’appartenant pas au domaine, utilisez [Restore-ADObject](https://technet.microsoft.com/library/ee617262.aspx).  

- Le Centre d'administration Active Directory ne peut pas restaurer des sous-arbres d'objets en une seule action. Par exemple, si vous supprimez une unité d'organisation comportant des unités d'organisation imbriquées, des utilisateurs, des groupes et des ordinateurs, la restauration de l'unité d'organisation de base ne restaure pas les objets enfants.  
  
    > [!NOTE]  
    > L’opération de restauration par lot Centre d’administration Active Directory effectue un « meilleur effort » pour le tri des objets supprimés *au sein de la sélection uniquement* afin que les parents soient classés avant les enfants pour la liste de restauration. Dans des cas de test simples, vous pouvez restaurer les sous-arbres d'objets en une seule action. Mais les cas d’angle, tels qu’une sélection contenant des arbres partiels-arbres avec certains nœuds parents supprimés, manquants ou cas d’erreur, tels que l’omission des objets enfants lorsque la restauration parente échoue, peut ne pas fonctionner comme prévu. C'est pourquoi vous devez toujours restaurer les sous-arbres des objets au cours d'une action distincte, une fois les objets parents restaurés.  
  
La corbeille Active Directory nécessite un niveau fonctionnel de forêt Windows Server 2008 R2 et vous devez être membre du groupe administrateurs de l’entreprise. Une fois activée, vous ne pouvez pas désactiver la Corbeille Active Directory. Celle-ci augmente la taille de la base de données Active Directory (NTDS.DIT) sur chaque contrôleur de domaine de la forêt. L'espace disque utilisé par la Corbeille continue d'augmenter au fil du temps car les objets et toutes leurs données d'attributs sont conservés.  
  
### <a name="enabling-active-directory-recycle-bin-using-active-directory-administrative-center"></a>Activation de la Corbeille Active Directory à l'aide du Centre d'administration Active Directory

Pour activer la Corbeille Active Directory, ouvrez le **Centre d'administration Active Directory**, puis cliquez sur le nom de votre forêt dans le volet de navigation. Dans le volet **Tâches**, cliquez sur **Activer la Corbeille**.  
  
![Gestion avancée des AD DS](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_EnableRecycleBin.png)  
  
Le Centre d'administration Active Directory montre la boîte de dialogue **Activer la confirmation de la Corbeille**. Cette boîte de dialogue vous avertit que l'activation de la Corbeille est irréversible. Cliquez sur **OK** pour activer la Corbeille Active Directory. Le Centre d'administration Active Directory montre une autre boîte de dialogue vous rappelant que la Corbeille Active Directory n'est pas entièrement fonctionnelle jusqu'à ce que tous les contrôleurs de domaine répliquent le changement de configuration.  
  
> [!IMPORTANT]  
> L'option permettant l'activation de la Corbeille Active Directory n'est pas disponible si :  
>
> - le niveau fonctionnel de la forêt est inférieur à Windows Server 2008 R2 ;  
> - elle est déjà activée.  

L’équivalent Active Directory applet de commande Windows PowerShell est le suivant :  

```powershell
Enable-ADOptionalFeature  
```

Pour plus d’informations sur l’utilisation de Windows PowerShell pour activer la Corbeille Active Directory, voir le [Guide pas à pas de la Corbeille Active Directory](https://docs.microsoft.com/windows-server/identity/ad-ds/get-started/adac/introduction-to-active-directory-administrative-center-enhancements--level-100-#active-directory-recycle-bin-step-by-step).  
  
### <a name="managing-active-directory-recycle-bin-using-active-directory-administrative-center"></a>Gestion de la Corbeille Active Directory à l'aide du Centre d'administration Active Directory

Cette section utilise l'exemple d'un domaine existant nommé **corp.contoso.com**. Ce domaine organise les utilisateurs en une unité d'organisation parente appelée **UserAccounts**. L'unité d'organisation **UserAccounts** contient trois unités d'organisation enfants nommées par service, chacune contenant d'autres unités d'organisation, utilisateurs et groupes.  
  
![Gestion avancée des AD DS](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_EnableRecycleBinExampleOU.png)  
  
#### <a name="storage-and-filtering"></a>Stockage et filtrage

La Corbeille Active Directory conserve tous les objets supprimés de la forêt. Elle enregistre ces objets selon l'attribut **msDS-deletedObjectLifetime**, qui est défini par défaut pour correspondre à l'attribut **tombstoneLifetime** de la forêt. Dans toute forêt créée à l'aide de Windows Server 2003 SP1 ou version ultérieure, l'attribut **tombstoneLifetime** a la valeur 180 jours par défaut. Dans toute forêt mise à niveau à partir de Windows 2000 ou installée avec Windows Server 2003 (sans Service Pack), l'attribut tombstoneLifetime par défaut N'EST PAS DÉFINI et Windows utilise la valeur interne par défaut de 60 jours. Vous pouvez configurer tous ces éléments et utiliser le Centre d'administration Active Directory pour restaurer les objets supprimés des partitions de domaine de la forêt. Vous devez continuer à utiliser l'applet de commande **Restore-ADObject** pour restaurer les objets supprimés à partir d'autres partitions, telles que Configuration. L'activation de la Corbeille Active Directory rend visible le conteneur **Objets supprimés** sous chaque partition de domaine dans le Centre d'administration Active Directory.  
  
![Gestion avancée des AD DS](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_DeletedObjectsContainer.png)  
  
Le conteneur **Objets supprimés** affiche tous les objets qui peuvent être restaurés dans cette partition de domaine. Les objets supprimés antérieurs à **msDS-deletedObjectLifetime** sont réputés comme étant des objets recyclés. Le Centre d'administration Active Directory n'affiche pas d'objets recyclés et vous ne pouvez pas restaurer ces objets à l'aide du Centre d'administration Active Directory.  
  
Pour une explication plus détaillée sur l’architecture et les règles de traitement de la Corbeille, voir [Corbeille Active Directory : présentation, implémentation, recommandations et dépannage](https://blogs.technet.com/b/askds/archive/2009/08/27/the-ad-recycle-bin-understanding-implementing-best-practices-and-troubleshooting.aspx).  
  
Le Centre d'administration Active Directory limite artificiellement le nombre d'objets par défaut renvoyés d'un conteneur à 20 000 objets. Vous pouvez augmenter cette limite à 100 000 objets en cliquant sur le menu **Gérer**, puis sur **Options de la liste des gestionnaires**.  
  
![Gestion avancée des AD DS](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_MgmtList.png)  
  
#### <a name="restoration"></a>Restauration  
  
##### <a name="filtering"></a>Filtrage

Le Centre d'administration Active Directory offre des options de filtrage et des critères puissants avec lesquels vous devez vous familiariser avant d'effectuer une restauration réelle. Les domaines suppriment de manière intentionnelle de nombreux objets pendant leur durée de vie. La durée de vie des objets supprimés est de 180 jours, vous ne pouvez donc pas restaurer simplement tous les objets quand un accident se produit.  
  
![Gestion avancée des AD DS](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_AddCriteria.png)  
  
Au lieu d'écrire des filtres LDAP complexes et de convertir des valeurs UTC en dates et heures, utilisez le menu **Filtre** de base et avancé pour répertorier uniquement les objets pertinents. Si vous connaissez le jour de suppression, le nom des objets ou toute autre donnée essentielle, vous pouvez affiner votre filtrage en les utilisant. Affichez ou masquez les options de filtre avancées en cliquant sur le chevron à droite de la zone de recherche.  
  
L'opération de restauration prend en charge toutes les options de critères de filtre standard, les mêmes que les autres recherches. Parmi les filtres intégrés, les plus importants pour restaurer des objets sont généralement :  
  
- *ANR (résolution de noms ambiguë) non listé dans le menu, mais ce qui est utilisé quand vous tapez dans la zone * * * * Filter * * * **  
- Dernière modification entre des dates données  
- Objet, à savoir utilisateur/inetOrgPerson/ordinateur/groupe/unité d'organisation  
- Nom  
- Quand supprimé  
- Dernier parent connu  
- Type  
- Description  
- Ville  
- Pays/région  
- Service  
- ID d'employé  
- Prénom  
- Fonction  
- Nom  
- SAMaccountname  
- Département/Province  
- Numéro de téléphone  
- UPN  
- Code postal  

Vous pouvez ajouter plusieurs critères. Par exemple, vous pouvez rechercher tous les objets utilisateurs supprimés le 24 septembre 2012 de Chicago, Illinois avec un poste de responsable.
  
Vous pouvez également ajouter, modifier ou réorganiser les en-têtes de colonne pour fournir plus de détails quand vous analysez les objets à récupérer.  
  
![Gestion avancée des AD DS](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_ColumnHeaders.png)  
  
Pour plus d’informations sur la résolution de noms ANR, voir [Attributs ANR](https://msdn.microsoft.com/library/ms675092(VS.85).aspx).  
  
##### <a name="single-object"></a>Objet unique

La restauration d'objets supprimés s'est toujours déroulée en une seule opération.  Le Centre d'administration Active Directory facilite cette opération. Pour restaurer un objet supprimé, tel qu'un utilisateur unique :  
  
1. Cliquez sur le nom de domaine dans le volet de navigation du Centre d'administration Active Directory.  
2. Double-cliquez sur **Objets supprimés** dans la liste de gestion.  
3. Cliquez avec le bouton droit sur l'objet, puis cliquez sur **Restaurer** ou cliquez sur **Restaurer** à partir du volet **Tâches**.  
  
L'objet est restauré à son emplacement d'origine.  
  
![Gestion avancée des AD DS](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_RestoreSingle.gif)  
  
Cliquez sur **restaurer sur...** pour modifier l’emplacement de restauration. Cela est utile si le conteneur parent de l’objet supprimé a également été supprimé, mais que vous ne voulez pas restaurer le parent.  
  
![Gestion avancée des AD DS](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_RestoreToSingle.gif)  
  
##### <a name="multiple-peer-objects"></a>Plusieurs objets homologues

Vous pouvez restaurer plusieurs objets de même niveau, tels que tous les utilisateurs d'une unité d'organisation. Maintenez la touche CTRL enfoncée et cliquez sur un ou plusieurs objets supprimés à restaurer. Cliquez sur **Restaurer** dans le volet Tâches. Vous pouvez également sélectionner tous les objets affichés en maintenant les touches CTRL et A enfoncées, ou une plage d'objets à l'aide de la touche Maj et en cliquant.  
  
![Gestion avancée des AD DS](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_RestorePeers.png)  
  
##### <a name="multiple-parent-and-child-objects"></a>Plusieurs objets parents et enfants

Il est essentiel de comprendre le processus de restauration dans le cas d'une restauration impliquant plusieurs parents-enfants car le Centre d'administration Active Directory ne peut pas restaurer un arbre imbriqué d'objets supprimés en une seule action.  
  
1. Restaurez l'objet supprimé en premier dans un arbre.  
2. Restaurez les enfants immédiats de cet objet parent.  
3. Restaurez les enfants immédiats de ces objets parents.  
4. Répétez cette opération autant de fois que nécessaire jusqu'à ce que tous les objets soient restaurés.  
  
Vous ne pouvez pas restaurer un objet enfant avant de restaurer son parent. La tentative de restauration renvoie l'erreur suivante :  
  
**L'opération n'a pas pu être effectuée car le parent de l'objet n'est pas instancié ou a été supprimé.**  
  
L'attribut **Dernier parent connu** montre la relation parent de chaque objet. L'attribut **Dernier parent connu** passe de l'emplacement supprimé à l'emplacement restauré quand vous actualisez le Centre d'administration Active Directory après la restauration d'un parent. Par conséquent, vous pouvez restaurer cet objet enfant lorsque l’emplacement d’un objet parent n’affiche plus le nom unique du conteneur d’objets supprimés.  
  
Envisagez le scénario dans lequel un administrateur supprime accidentellement l'unité d'organisation Ventes, qui contient les unités d'organisation enfants et utilisateurs.  
  
Tout d’abord, observez la valeur du **dernier attribut parent connu** pour tous les utilisateurs supprimés et la façon dont il lit **ou = ventes\0adel :* < GUID + nom unique du conteneur d’objets supprimés > * * * :  
  
![Gestion avancée des AD DS](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_LastKnownParent.gif)  
  
Filtrez sur le nom ambigu Ventes pour renvoyer l'unité d'organisation supprimée, que vous restaurez ensuite :  
  
![Gestion avancée des AD DS](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_LastKnownParentSales.png)  
  
Actualisez le Centre d’administration Active Directory pour afficher le dernier attribut parent connu de l’objet utilisateur supprimé sur le nom unique de l’unité d’organisation ventes restaurée :  
  
![Gestion avancée des AD DS](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_LastKnownParentSalesRestored.gif)  
  
Filtrez sur tous les utilisateurs Ventes. Maintenez les touches CTRL et A enfoncées pour sélectionner tous les utilisateurs Ventes supprimés. Cliquez sur **Restaurer** pour déplacer les objets du conteneur **Objets supprimés** vers l'unité d'organisation Ventes avec leurs appartenances au groupe et attributs intacts.  
  
![Gestion avancée des AD DS](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_LastKnownParentSalesUndelete.png)  
  
Si l'unité d'organisation **Ventes** contenait des unités d'organisation enfants en propre, vous pouvez commencer par restaurer les unités d'organisation enfants avant de restaurer leurs enfants, et ainsi de suite.  
  
Pour restaurer tous les objets supprimés imbriqués en spécifiant un conteneur parent supprimé, voir [Annexe B : Restaurer plusieurs objets Active Directory supprimés (exemple de script)](https://technet.microsoft.com/library/dd379504(WS.10).aspx).  
  
L'applet de commande Active Directory pour Windows PowerShell qui permet de restaurer les objets supprimés est la suivante :  

```powershell
Restore-adobject  
```

La fonctionnalité d'applet de commande **Restore-ADObject** n'a pas changé entre Windows Server 2008 R2 et Windows Server 2012.  
  
##### <a name="server-side-filtering"></a>Filtrage côté serveur

Au fil du temps, le conteneur Objets supprimés peut cumuler plus de 20 000 (ou même 100 000) objets dans les moyennes ou grandes entreprises et rencontre des difficultés pour les afficher tous. Le mécanisme de filtre du Centre d'administration Active Directory repose sur un filtrage côté client, il ne peut donc pas afficher ces objets supplémentaires. Pour contourner cette limitation, utilisez les étapes suivantes pour effectuer une recherche côté serveur :  
  
1. Cliquez avec le bouton droit sur le conteneur **Objets supprimés**, puis cliquez sur **Rechercher sous ce nœud**.  
2. Cliquez sur le chevron pour afficher le menu **Ajouter des critères**, sélectionnez et ajoutez **Dernière modification entre des dates données**. L'heure de dernière modification (l'attribut **whenChanged**) est une approximation fiable de l'heure de suppression ; dans la plupart des environnements, elles sont identiques. Cette requête effectue une recherche côté serveur.  
3. Identifiez les objets supprimés à restaurer à l'aide de filtres d'affichage supplémentaires et de tri avancé des résultats, puis restaurez-les normalement.  
  
## <a name="configuring-and-managing-fine-grained-password-policies-using-active-directory-administrative-center"></a><a name="BKMK_FGPP"></a>Configuration et gestion des stratégies de mot de passe affinées à l’aide de Centre d’administration Active Directory  
  
### <a name="configuring-fine-grained-password-policies"></a>Configuration des stratégies de mot de passe affinées

Le Centre d'administration Active Directory vous permet de créer et de gérer des objets de stratégie de mot de passe affinée. Windows Server 2008 a introduit la fonctionnalité de stratégies de mot de passe affinées mais dans Windows Server 2012, c'est la première fois qu'elle est dotée d'une interface de gestion graphique. Vous appliquez des stratégies de mot de passe affinées au niveau du domaine, ce qui permet la substitution du mot de passe de domaine unique nécessaire à Windows Server 2003. En créant différentes stratégies de mot de passe affinées avec différents paramètres, les utilisateurs individuels ou les groupes obtiennent des stratégies de mot de passe différentes dans un domaine.  
  
Pour plus d’informations sur la stratégie de mot de passe affinée, voir [Guide pas à pas relatif à la configuration des stratégies de verrouillage de compte et de mot de passe affinées (Windows Server 2008 R2)](https://technet.microsoft.com/library/cc770842(WS.10).aspx).  
  
Dans le volet Navigation, cliquez sur Arborescence, sur votre domaine, **Système**, **Classe d'objets PSC (Password Settings Container)** , puis dans le volet Tâches, cliquez sur **Nouveau** et **Paramètres de mot de passe**.  
  
![Gestion avancée des AD DS](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_PasswordSettings.png)  
  
### <a name="managing-fine-grained-password-policies"></a>Gestion des stratégies de mot de passe affinées

La création d'une stratégie de mot de passe affinée ou la modification d'une stratégie existante permet d'afficher l'éditeur **Paramètres de mot de passe**. À partir d'ici, vous configurez toutes les stratégies de mot de passe souhaitées, comme dans Windows Server 2008 ou Windows Server 2008 R2, à ceci près que vous disposez désormais d'un éditeur spécifique intégré.  
  
![Gestion avancée des AD DS](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_CreatePasswordSettings.png)  
  
Renseignez tous les champs (astérisque rouge) nécessaires et facultatifs, puis cliquez sur **Ajouter** pour définir les utilisateurs ou les groupes qui reçoivent cette stratégie. La stratégie de mot de passe affinée substitue les paramètres de la stratégie de domaine par défaut pour les principaux de sécurité spécifiés. Dans la figure ci-dessus, une stratégie particulièrement restrictive s'applique uniquement au compte Administrateur intégré pour éviter toute méprise. La stratégie est bien trop complexe pour des utilisateurs standard, mais elle est adaptée dans le cas d'un compte à haut risque, uniquement utilisé par les professionnels de l'informatique.  
  
Vous définissez également la priorité et les utilisateurs et groupes auxquels la stratégie s'applique dans un domaine donné.  
  
![Gestion avancée des AD DS](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_Precedence.png)  
  
Les applets de commande Active Directory pour Windows PowerShell relatives à la stratégie de mot de passe affinée sont :  
  
```powershell
Add-ADFineGrainedPasswordPolicySubject  
Get-ADFineGrainedPasswordPolicy  
Get-ADFineGrainedPasswordPolicySubject  
New-ADFineGrainedPasswordPolicy  
Remove-ADFineGrainedPasswordPolicy  
Remove-ADFineGrainedPasswordPolicySubject  
Set-ADFineGrainedPasswordPolicy  
```

La fonctionnalité d'applet de commande de stratégie de mot de passe affinée n'a pas changé entre Windows Server 2008 R2 et Windows Server 2012. Pour votre commodité, le diagramme suivant illustre les arguments associés pour les applets de commande :  
  
![Gestion avancée des AD DS](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_FGPP.gif)  
  
Le Centre d'administration Active Directory vous permet également de rechercher l'ensemble des stratégies de mot de passe affinées appliqué à un utilisateur donné. Cliquez avec le bouton droit sur n’importe quel utilisateur, puis cliquez sur **afficher les paramètres de mot de passe résultants...** pour ouvrir la page *paramètres de mot de passe* qui s’applique à cet utilisateur via une affectation implicite ou explicite :  
  
![Gestion avancée des AD DS](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_RSOP.png)  
  
L'examen des **Propriétés** d'un utilisateur ou d'un groupe montre les **Paramètres de mot de passe directement associés**, qui sont les stratégies de mot de passe affinées explicitement affectées :  
  
![Gestion avancée des AD DS](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_FGPPSettings.gif)  
  
L’assignation passe affinée implicite ne s’affiche pas ici. pour cela, vous devez utiliser l’option **afficher les paramètres de mot de passe résultants..** ..  
  
## <a name="using-the-active-directory-administrative-center-windows-powershell-history-viewer"></a><a name="BKMK_HistoryViewer"></a>Utilisation de la visionneuse de l’historique de Windows PowerShell Centre d’administration Active Directory

La gestion future de Windows est Windows PowerShell. En disposant des outils graphiques au-dessus d'une infrastructure d'automatisation des tâches, la gestion des systèmes distribués les plus complexes devient cohérente et efficace. Vous devez comprendre le fonctionnement de Windows PowerShell pour réaliser au mieux votre potentiel et optimiser vos investissements en matière d'informatique.  
  
Le Centre d'administration Active Directory fournit désormais un historique complet de toutes les applets de commande Windows PowerShell qu'il exécute ainsi que leurs arguments et leurs valeurs. Vous pouvez copier l'historique de l'applet de commande à un autre emplacement pour l'étudier ou la modifier et la réutiliser. Vous pouvez créer des notes Tâche pour permettre d'isoler le résultat des commandes du Centre d'administration Active Directory dans Windows PowerShell. Vous pouvez également filtrer l'historique pour trouver des points d'intérêt.  
  
La Visionneuse de l'historique Windows PowerShell du Centre d'administration Active Directory vous permet d'apprendre en pratiquant.  
  
![Gestion avancée des AD DS](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_HistoryViewer.gif)  
  
Cliquez sur le chevron (flèche) pour afficher la Visionneuse de l'historique Windows PowerShell.  
  
![Gestion avancée des AD DS](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_RaiseViewer.png)  
  
Créez ensuite un utilisateur ou modifiez l'appartenance à un groupe. La Visionneuse de l'historique se met à jour en permanence avec un affichage réduit de chaque applet de commande que le Centre d'administration Active Directory a exécuté avec les arguments spécifiés.  
  
Développez chaque élément de ligne pertinent pour afficher les valeurs fournies aux arguments de l'applet de commande :  
  
![Gestion avancée des AD DS](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_ViewArgs.png)  
  
Cliquez sur le menu **Démarrer la tâche** pour créer une note manuelle avant d'utiliser le Centre d'administration Active Directory pour créer, modifier ou supprimer un objet. Tapez ce que vous étiez en train de faire.  Quand vous avez terminé la modification, sélectionnez **Fin de tâche**. La note de tâche regroupe toutes les actions effectuées dans une note réductible, explicative.  
  
Par exemple, pour afficher les commandes Windows PowerShell utilisées pour modifier le mot de passe d'un utilisateur et le supprimer d'un groupe :  
  
![Gestion avancée des AD DS](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_RemoveUser.gif)  
  
En cochant la case Afficher tout, vous affichez également les applets de commande Windows PowerShell du verbe Get-* qui ne récupèrent que des données.  
  
![Gestion avancée des AD DS](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_ShowAll.png)  
  
La Visionneuse de l'historique affiche les commandes littérales exécutées par le Centre d'administration Active Directory. Vous remarquerez peut-être que certaines applets de commande semblent s'exécuter inutilement. Par exemple, vous pouvez créer un utilisateur avec :  

```powershell
new-aduser
```

et n'avez pas besoin d'utiliser :  

```powershell
set-adaccountpassword  
enable-adaccount  
set-aduser  
```

La conception du Centre d'administration Active Directory a exigé une modularité et une utilisation du code minimales. Ainsi, plutôt qu'un ensemble de fonctions crée des utilisateurs et qu'un autre ensemble modifie des utilisateurs existants, il remplit chaque fonction séparément, puis les regroupe à l'aide des applets de commande. Gardez cela à l'esprit si vous étudiez Active Directory pour Windows PowerShell. Vous pouvez également l'utiliser en tant technique d'apprentissage : en fait, Windows PowerShell permet d'accomplir facilement une tâche simple.  
  
## <a name="troubleshooting-ad-ds-management"></a><a name="BKMK_Tshoot"></a>Résolution des problèmes de gestion des AD DS  
  
### <a name="introduction-to-troubleshooting"></a>Introduction à la résolution des problèmes

Le Centre d'administration Active Directory est relativement récent et peu utilisé dans les environnements clients existants. Il offre donc des options limitées de résolution des problèmes.  
  
### <a name="troubleshooting-options"></a>Options de résolution des problèmes  
  
#### <a name="logging-options"></a>Options de journalisation

Le Centre d’administration Active Directory contient maintenant la journalisation intégrée, dans le cadre d’un fichier de configuration de suivi. Créez/modifiez le fichier suivant dans le même dossier que dsac.exe :  
  
**DSAC. exe. config**
  
Créez le contenu suivant :  
  
```xml
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

Les niveaux de détail de **DsacLogLevel** sont **Aucun**, **Erreur**, **Avertissement**, **Info** et **Commenté**. Le nom du fichier de sortie peut être configuré et enregistré dans le même dossier que dsac.exe. La sortie peut vous renseigner sur le mode de fonctionnement du Centre d'administration Active Directory, les contrôleurs de domaine qu'il a contactés, les commandes Windows PowerShell qu'il a exécutées, les réponses obtenues et autres informations.  

Par exemple, en utilisant le niveau INFO qui renvoie tous les résultats sauf le détail au niveau du suivi :  
  
- DSAC.exe démarre  
- La journalisation démarre  
- Le contrôleur de domaine doit renvoyer les informations de domaine initiales  

   ```
   [12:42:49][TID 3][Info] Command Id, Action, Command, Time, Elapsed Time ms (output), Number objects (output)  
   [12:42:49][TID 3][Info] 1, Invoke, Get-ADDomainController, 2012-04-16T12:42:49  
   [12:42:49][TID 3][Info] Get-ADDomainController-Discover:$null-DomainName:"CORP"-ForceDiscover:$null-Service:ADWS-Writable:$null  
   ```

- Le contrôleur de domaine DC1 a renvoyé depuis le domaine Corp  
- Lecteur virtuel AD PS chargé  

   ```
   [12:42:49][TID 3][Info] 1, Output, Get-ADDomainController, 2012-04-16T12:42:49, 1  
   [12:42:49][TID 3][Info] Found the domain controller 'DC1' in the domain 'CORP'.  
   [12:42:49][TID 3][Info] 2, Invoke, New-PSDrive, 2012-04-16T12:42:49  
   [12:42:49][TID 3][Info] New-PSDrive-Name:"ADDrive0"-PSProvider:"ActiveDirectory"-Root:""-Server:"dc1.corp.contoso.com"  
   [12:42:49][TID 3][Info] 2, Output, New-PSDrive, 2012-04-16T12:42:49, 1  
   [12:42:49][TID 3][Info] 3, Invoke, Get-ADRootDSE, 2012-04-16T12:42:49  
   ```
  
- Obtenir les informations de la DSE racine du domaine  

   ```
   [12:42:49][TID 3][Info] Get-ADRootDSE -Server:"dc1.corp.contoso.com"  
   [12:42:49][TID 3][Info] 3, Output, Get-ADRootDSE, 2012-04-16T12:42:49, 1  
   [12:42:49][TID 3][Info] 4, Invoke, Get-ADOptionalFeature, 2012-04-16T12:42:49  
   ```

- Obtenir les informations de la Corbeille AD du domaine  

   ```
   [12:42:49][TID 3][Info] Get-ADOptionalFeature -LDAPFilter:"(msDS-OptionalFeatureFlags=1)" -Server:"dc1.corp.contoso.com"
   [12:42:49][TID 3][Info] 4, Output, Get-ADOptionalFeature, 2012-04-16T12:42:49, 1
   [12:42:49][TID 3][Info] 5, Invoke, Get-ADRootDSE, 2012-04-16T12:42:49
   [12:42:49][TID 3][Info] Get-ADRootDSE -Server:"dc1.corp.contoso.com"
   [12:42:49][TID 3][Info] 5, Output, Get-ADRootDSE, 2012-04-16T12:42:49, 1
   [12:42:49][TID 3][Info] 6, Invoke, Get-ADRootDSE, 2012-04-16T12:42:49
   [12:42:49][TID 3][Info] Get-ADRootDSE -Server:"dc1.corp.contoso.com"
   [12:42:49][TID 3][Info] 6, Output, Get-ADRootDSE, 2012-04-16T12:42:49, 1
   [12:42:49][TID 3][Info] 7, Invoke, Get-ADOptionalFeature, 2012-04-16T12:42:49
   [12:42:49][TID 3][Info] Get-ADOptionalFeature -LDAPFilter:"(msDS-OptionalFeatureFlags=1)" -Server:"dc1.corp.contoso.com"
   [12:42:50][TID 3][Info] 7, Output, Get-ADOptionalFeature, 2012-04-16T12:42:50, 1
   [12:42:50][TID 3][Info] 8, Invoke, Get-ADForest, 2012-04-16T12:42:50
   ```

- Obtenir la forêt AD  

   ```  
   [12:42:50][TID 3][Info] Get-ADForest -Identity:"corp.contoso.com" -Server:"dc1.corp.contoso.com"
   [12:42:50][TID 3][Info] 8, Output, Get-ADForest, 2012-04-16T12:42:50, 1  
   [12:42:50][TID 3][Info] 9, Invoke, Get-ADObject, 2012-04-16T12:42:50  
   ```

- Obtenir les informations de schéma pour les types de chiffrement pris en charge, les stratégies de mot de passe affinées, certaines informations utilisateur  

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

- Obtenir toutes les informations concernant l'objet de domaine à afficher pour l'administrateur qui a cliqué sur le sommet du domaine  

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

La définition du niveau de commentaire montre également les piles .NET pour chaque fonction, mais celles-ci n'incluent pas assez de données pour être vraiment utiles, sauf en cas de dépannage de Dsac.exe après une violation d'accès ou un incident. Les deux causes probables de ce problème sont les suivantes :
  
- les services Web Active Directory (ADWS) ne s'exécutent sur aucun contrôleur de domaine accessible ;
- Les communications réseau sont bloquées vers le service Active Directory à partir de l’ordinateur qui exécute le Centre d’administration Active Directory.

> [!IMPORTANT]  
> Il existe également une version hors-bande du service appelée [Passerelle de gestion Active Directory](https://www.microsoft.com/download/en/details.aspx?displaylang=en&id=2852), qui s’exécute sur Windows Server 2008 SP2 et Windows Server 2003 SP2.
>

Les erreurs affichées quand aucune instance des services Web Active Directory ne sont disponibles sont les suivantes :  
  
|Error|Opération|
| --- | --- |  
|« Impossible de se connecter à un domaine. Actualisez ou réessayez lorsque la connexion est disponible. »|Affiché au démarrage de l'application Centre d'administration Active Directory|
|« Impossible de trouver un serveur disponible dans le domaine *<NetBIOS domain name>* qui exécute le Service Web Active Directory »|Affiché au cours d'une tentative de sélection d'un nœud de domaine dans l'application Centre d'administration Active Directory|
  
Pour résoudre ce problème, utilisez ces étapes :  
  
1. Vérifiez si les services Web Active Directory sont démarrés sur au moins un contrôleur de domaine du domaine (et de préférence tous les contrôleurs de domaine dans la forêt). Contrôlez qu'ils sont configurés pour démarrer automatiquement sur tous les contrôleurs de domaine également.
2. Sur l'ordinateur exécutant le Centre d'administration Active Directory, vérifiez que vous pouvez localiser un serveur exécutant les services Web Active Directory à l'aide des commandes NLTest.exe suivantes :  

   ```
   nltest /dsgetdc:<domain NetBIOS name> /ws /force
   nltest /dsgetdc:<domain fully qualified DNS name> /ws /force
   ```

   Si ces tests échouent même si les services Web Active Directory sont en cours d'exécution, le problème se pose avec la résolution de noms ou le protocole LDAP et non avec les services Web Active Directory ou le Centre d'administration Active Directory. Ce test échoue avec l'erreur « 1355 0x54B ERROR_NO_SUCH_DOMAIN » si par exemple les services Web Active Directory ne sont pas en cours d'exécution sur les contrôleurs de domaine. Il est donc important de revérifier avant de tirer une conclusion hâtive.  
  
3. Sur le contrôleur de domaine renvoyé par NLTest, videz la liste du port d'écoute avec la commande :  

   ```
   Netstat -anob > ports.txt  
   ```

   Examinez le fichier ports.txt, puis validez que les services Web Active Directory écoutent le port 9389. Exemple :  

   ```
   TCP    0.0.0.0:9389    0.0.0.0:0    LISTENING    1828  
   [Microsoft.ActiveDirectory.WebServices.exe]  

   TCP    [::]:9389       [::]:0       LISTENING    1828  
   [Microsoft.ActiveDirectory.WebServices.exe]  
   ```

   Si c'est le cas, validez les règles du Pare-feu Windows et vérifiez qu'elles autorisent le port TCP 9389 entrant. Par défaut, les contrôleurs du domaine activent la règle du pare-feu « Services Web Active Directory (TCP-entrant) ». Si le port n'est pas en train d'écouter, vérifiez à nouveau que les services sont en cours d'exécution sur ce serveur et redémarrez-le. Vérifiez qu'aucun autre processus n'est déjà en train d'écouter sur le port 9389.  
  
4. Installez NetMon ou un autre utilitaire de capture réseau sur l'ordinateur exécutant le Centre d'administration Active Directory et sur le contrôleur de domaine renvoyé par NLTEST. Rassemblez des captures du réseau simultanées depuis les deux ordinateurs, sur lesquels vous démarrez le Centre d'administration Active Directory et voyez l'erreur avant d'arrêter les captures. Vérifiez que le client est capable d'envoyer des données au contrôleur de domaine et d'en recevoir de ce dernier sur le port TCP 9389. Si les paquets sont envoyés mais qu'ils n'arrivent jamais à destination, ou s'ils arrivent, que le contrôleur de domaine répond mais qu'ils n'atteignent jamais le client, un pare-feu est probablement placé entre les ordinateurs sur les paquets ignorés du réseau sur ce port. Ce pare-feu peut être logiciel ou matériel et faire partie d'un logiciel tiers (antivirus) de protection de point de terminaison.  
  
## <a name="see-also"></a>Voir aussi

[Corbeille Active Directory, stratégie de mot de passe affinée et historique PowerShell](../../../ad-ds/get-started/adac/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-.md)  
