---
ms.assetid: 40bc24b1-2e7d-4e77-bd0f-794743250888
title: "Unicité des noms SPN et UPN"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 81d686c81082ad29384585d541c1304d654e1924
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="spn-and-upn-uniqueness"></a>Unicité des noms SPN et UPN

>S’applique à: Windows Server2016, Windows Server2012R2, Windows Server2012

**Auteur**: Justin Turner, ingénieur Support résolution Senior avec le groupe de Windows  
  
> [!NOTE]  
> Ce contenu est écrit par un ingénieur du support client Microsoft et est destiné aux administrateurs expérimentés et architectes système qui recherchent des explications techniques plus approfondies des fonctionnalités et des solutions Windows Server2012R2 que proposent généralement les rubriques sur TechNet. Toutefois, il n’a pas subi les mêmes passes, afin de la partie du langage peut sembler que moins finalisée que ce qui se trouvent généralement sur TechNet.  
  
## <a name="overview"></a>Vue d’ensemble  
Contrôleurs de domaine exécutant Windows Server2012R2 bloquer la création de dupliquer les noms de principal du service (SPN) et les noms d’utilisateur principal (UPN). Cela inclut si la restauration ou la réanimation des objets d’un objet supprimé ou le changement de nom d’un objet entraînerait un doublon.  
  
### <a name="background"></a>En arrière-plan  
Noms Principal à en double de Service (SPN) couramment se produisent et entraîner des échecs d’authentification et peut conduire à une utilisation excessive du processeur LSASS. Il n’existe aucune méthode de boîte aux lettres pour bloquer l’ajout d’un SPN en double ou un nom d’utilisateur principal. *  
  
Les valeurs de nom UPN en double saut synchronisation entre local AD et Office 365.  
  
*Setspn.exe est couramment utilisé pour créer de nouveaux SPN et fonctionnellement a été intégrée à la version fournie avec Windows Server2008 qui ajoute une vérification de la présence de doublons.  
  
**Table de Table SEQ \\\ * ARABIC 1: unicité des noms UPN et le SPN**  
  
|Fonctionnalité|Commentaire|  
|-----------|-----------|  
|Unicité des noms UPN|Synchronisation de coupure UPN en double des locaux de comptes ActiveDirectory avec les services de basée sur ActiveDirectory de Windows Azure comme Office 365.|  
|Unicité des noms SPN|Kerberos requiert des SPN pour l’authentification mutuelle.  SPN en double entraîner des échecs d’authentification.|  
  
Pour plus d’informations sur les conditions d’unicité des noms UPN et le SPN, voir [contraintes d’unicité](https://msdn.microsoft.com/library/dn392337.aspx).  
  
## <a name="symptoms"></a>Symptômes  
Codes d’erreur 8467 ou 8468 ou leur hex, symbolique ou les équivalents de la chaîne sont consignés dans différents à l’écran boîtes de dialogue et dans l’événement 2974 ID dans le journal des événements Services d’annuaire. La tentative de création d’un UPN ou un SPN en double est bloquée uniquement dans les circonstances suivantes:  
  
-   L’écriture est traitée par un contrôleur de domaine de Windows Server2012R2  
  
**Table de Table SEQ \\\ * ARABIC 2: codes d’erreur unicité des noms UPN et le SPN**  
  
|Décimal|Hex|Symbolique|Chaîne|  
|-----------|-------|------------|----------|  
|8467|7 21C|ERROR_DS_SPN_VALUE_NOT_UNIQUE_IN_FOREST|L’opération a échoué car valeur SPN fournie pour l’ajout/modification n’est pas unique de la forêt.|  
|8648|8 21C|ERROR_DS_UPN_VALUE_NOT_UNIQUE_IN_FOREST|L’opération a échoué car valeur UPN fournie pour l’ajout/modification n’est pas unique de la forêt.|  
  
## <a name="new-user-creation-fails-if-upn-is-not-unique"></a>Création du nouvel utilisateur échoue si le nom d’utilisateur principal n’est pas unique  
  
### <a name="dsamsc"></a>DSA.msc  
Le nom d’ouverture de session d’utilisateur que vous avez choisi est déjà en cours d’utilisation de cette entreprise. Choisissez un autre nom d’ouverture de session, puis essayez à nouveau.  
  
![Unicité des noms SPN et UPN](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig01_DupUPN.gif)  
  
Modifier un compte existant:  
  
Le nom d’ouverture de session d’utilisateur spécifié existe déjà dans l’entreprise. Spécifiez un autre, soit en modifiant le préfixe ou en choisissant un autre suffixe dans la liste.  
  
![Unicité des noms SPN et UPN](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig02_DupUPNMod.gif)  
  
### <a name="active-directory-administrative-center-dsacexe"></a>ActiveDirectory Administrative Center (DSAC.exe)  
Une tentative de création d’un nouvel utilisateur dans le centre d’administration ActiveDirectory avec un nom UPN qui existe déjà produira l’erreur suivante.  
  
![Unicité des noms SPN et UPN](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig03_DupUPNADAC.gif)  
  
**Figure SEQ Figure \\\ * ARABIC 1 une erreur s’affichée dans le centre d’administration ActiveDirectory en cas d’échec de création du nouvel utilisateur en raison de l’UPN en double**  
  
### <a name="event-2974-source-activedirectorydomainservice"></a>Source de l’événement 2974: ActiveDirectory_DomainService  
![Unicité des noms SPN et UPN](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig04_Event2974.gif)  
  
**Figure SEQ Figure \\\ * ARABIC 2événements ID2974 avec l’erreur 8648**  
  
L’événement 2974 répertorie la valeur qui a été bloquée et une liste d’un ou plusieurs objets (jusqu'à 10) contenant déjà la valeur.  Dans l’illustration suivante, vous pouvez voir cette valeur d’attribut UPN ***dhunt@blue.contoso.com***existe déjà sur quatre autres objets.  Dans la mesure où il s’agit d’une nouvelle fonctionnalité de Windows Server2012R2, création accidentelle des UPN et le SPN en double dans un environnement mixte toujours se produit lors de la tentative d’écriture traitent les contrôleurs de domaine de niveau inférieur.  
  
![Unicité des noms SPN et UPN](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig05_Event2974ShowAllDups.gif)  
  
**Figure SEQ Figure \\\ * ARABIC2974 d’événement 3 montrant tous les objets contenant l’UPN en double**  
  
> [!TIP]  
> Passez en revue les événements ID2974s régulièrement à:  
>   
> -   Identifier les tentatives de création UPN en double ou des noms principaux de service  
> -   Identifier les objets qui contiennent déjà des doublons  
  
8648 = «L’opération a échoué car la valeur de nom UPN fournie pour Ajout/modification n’est pas forêt unique à l’échelle».  
  
### <a name="setspn"></a>SetSPN:  
Setspn.exe a été intégré depuis la version de Windows Server2008 de détection de SPN en double lors de l’utilisation du **»-S»** option.  Vous pouvez ignorer la détection de SPN en double à l’aide de la **»-une «** option toutefois.  La création d’un SPN en double est bloquée lorsque vous ciblez un Windows Server2012R2 contrôleur de domaine à l’aide de SetSPN avec l’option - A.  Le message d’erreur affiché est identique à celle affichée lorsque vous utilisez l’option -S: «Duplicate SPN trouvé, abandon de l’opération!»  
  
### <a name="adsiedit"></a>ADSIEDIT:  
  
```  
Operation failed. Error code: 0x21c8  
The operation failed because UPN value provided for addition/modification is not unique forest-wide.  
000021C8: AtrErr: DSID-03200BBA, #1: 0: 000021C8: DSID-03200BBA, problem 1005 (CONSTRAINT_ATT_TYPE), data 0, Att 90290 (userPrincipalName)  
```  
  
![Unicité des noms SPN et UPN](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig06_ADSI21c8.gif)  
  
**Figure SEQ Figure \\\ * message d’erreur de 4 arabe affiché dans ADSIEdit lors de l’ajout d’un UPN en double est bloquée**  
  
### <a name="windows-powershell"></a>Windows PowerShell  
Windows Server2012R2:  
  
![Unicité des noms SPN et UPN](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig07_SetADUser2012.gif)  
  
PS en cours d’exécution à partir de Server2012 ciblant un contrôleur de domaine de Windows Server2012R2:  
  
![Unicité des noms SPN et UPN](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig08_SetADUser2012R2.gif)  
  
DSAC.exe s’exécutant sur Windows Server2012 ciblant un contrôleur de domaine de Windows Server2012R2:  
  
![Unicité des noms SPN et UPN](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig09_UserCreateError.gif)  
  
**Figure SEQ Figure \\\ * erreur de création d’utilisateur arabe DSAC 5 sur non - Windows Server2012R2 tout en ciblant le contrôleur de domaine Windows Server2012R2**  
  
![Unicité des noms SPN et UPN](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig10_UserModError.gif)  
  
**Figure SEQ Figure \\\ * erreur de modification d’utilisateur arabe DSAC 6 sur non - Windows Server2012R2 tout en ciblant le contrôleur de domaine Windows Server2012R2**  
  
### <a name="restore-of-an-object-that-would-result-in-a-duplicate-upn-fails"></a>Échec de la restauration d’un objet qui entraînerait un UPN en double:  
![Unicité des noms SPN et UPN](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig11_RestoreDupUPN.gif)  
  
![Unicité des noms SPN et UPN](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig12_RestoreDupUPNError.gif)  
  
Aucun événement n’est consigné lorsqu’un objet ne parvient pas à restaurer en raison d’un UPN en double / SPN.  
  
Le nom UPN de l’objet doit être unique afin qu’il peut être restauré.  
  
1.  Identifier le nom d’utilisateur principal qui existe sur l’objet dans la Corbeille  
  
2.  Identifier tous les objets qui ont la même valeur  
  
3.  Supprimer l’UPN en double  
  
### <a name="identify-the-conflicting-upn-on-the-deleted-objectusing-repadminexe"></a>Identifiez l’UPN sur l'utilisation supprimés repadmin.exe en conflit  
  
```  
Repadmin /showattr DCName "DN of deleted objects container" /subtree /filter:"(msDS-LastKnownRDN=<NAME>)" /deleted /atts:userprincipalname  
```  
  
```  
repadmin /showattr DCName "CN=Deleted Objects,DC=blue,DC=contoso,DC=com" /subtree /filter:"(msDS-LastKnownRDN=Dianne Hunt2)" /deleted /atts:userprincipalname  
  
C:\>repadmin /showattr winbluedc1 "cn=deleted objects,dc=blue,dc=contoso,dc=com" /subtree /filter:"(msds-lastknownrdn=Dianne Hunt2)" /deleted /atts:userprincipalname  
DN: CN=Dianne Hunt2\0ADEL:dd3ab8a4-3005-4f2f-814f-d6fc54a1a1c0,CN=Deleted Object  
s,DC=blue,DC=contoso,DC=com  
    1> userPrincipalName: dhunt@blue.contoso.com  
```  
  
### <a name="to-identify-all-objects-with-the-same-upnusing-repadminexe"></a>Pour identifier tous les objets avec le même nom d’utilisateur principal: à l’aide de Repadmin.exe  
  
```  
repadmin /showattr WinBlueDC1 "DC=blue,DC=contoso,DC=com" /subtree /filter:"(userPrincipalName=dhunt@blue.contoso.com)" /deleted /atts:DN  
  
C:\>repadmin /showattr winbluedc1 "dc=blue,dc=contoso,dc=com" /subtree /filter:"(userPrincipalName=dhunt@blue.contoso.com)" /deleted /atts:DN  
DN: CN=Administrator,CN=Users,DC=blue,DC=contoso,DC=com  
DN: CN=xouser1,CN=Users,DC=blue,DC=contoso,DC=com  
DN: CN=xouser10,CN=Users,DC=blue,DC=contoso,DC=com  
DN: CN=xouser100,CN=Users,DC=blue,DC=contoso,DC=com  
DN: CN=Dianne Hunt,OU=Marketing,DC=blue,DC=contoso,DC=com  
DN: CN=Dianne Hunt2\0ADEL:dd3ab8a4-3005-4f2f-814f-d6fc54a1a1c0,CN=Deleted Objects,DC=blue,DC=contoso,DC=com  
```  
  
> [!TIP]  
> Le précédemment non documentées **/ supprimé** paramètre dans repadmin.exe est utilisé pour inclure des objets supprimés dans le jeu de résultats  
  
### <a name="using-global-search"></a>À l’aide de la recherche globale  
  
-   Ouvrir le centre d’administration ActiveDirectory et accédez à **recherche globale**  
  
-   Sélectionnez le **convertir en LDAP** case d’option  
  
-   Type **(userPrincipalName =*ConflictingUPN*) **  
  
    -   Remplacer ***ConflictingUPN*** avec l’UPN réelle qui est en conflit  
  
-   Sélectionnez **appliquer**  
  
![Unicité des noms SPN et UPN](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig13_GlobalSearch.gif)  
  
### <a name="using-windows-powershell"></a>À l’aide de Windows PowerShell  
  
```  
Get-ADObject -LdapFilter "(userPrincipalName=dhunt@blue.contoso.com)" -IncludeDeletedObjects -SearchBase "DC=blue,DC=Contoso,DC=com" -SearchScope Subtree -Server winbluedc1.blue.contoso.com  
```  
  
![Unicité des noms SPN et UPN](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig13_GlobalSearchPS.gif)  
  
Si l’objet doit être restaurée, vous devez supprimera l’UPN dupliqué à partir des autres objets.  Pour qu’un seul objet, il est assez simple d’utiliser ADSIEdit pour supprimer le doublon.  S’il existe plusieurs objets avec les doublons, Windows PowerShell peut être le meilleur outil à utiliser.  
  
Pour annuler l’attribut UserPrincipalName à l’aide de Windows PowerShell:  
  
![Unicité des noms SPN et UPN](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig15_NullUPN.gif)  
  
> [!NOTE]  
> L’attribut userPrincipalName étant attribut à valeur simple, cette procédure supprime uniquement l’UPN en double.  
  
### <a name="duplicate-spn"></a>SPN en double  
![Unicité des noms SPN et UPN](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig16_DupSPN.gif)  
  
**Figure SEQ Figure \\\ * message d’erreur de 8 arabe affiché dans ADSIEdit lors de l’ajout de SPN en double est bloquée**  
  
Enregistré dans les Services d’annuaire journal des événements est un **ActiveDirectory_DomainService** ID d’événement **2974**.  
  
```  
Operation failed. Error code: 0x21c7  
The operation failed   
The attribute value provided is not unique in the forest or partition. Attribute:  
servicePrincipalName Value=<SPN>  
<Object DN> Winerror: 8467  
```  
  
![Unicité des noms SPN et UPN](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig17_DupSPN2974.gif)  
  
**Figure SEQ Figure \\\ * ARABIC erreur 9 connecté lors de la création de SPN en double est bloquée**  
  
### <a name="workflow"></a>Flux de travail  
  
-   **Si contrôleur de domaine == dans le catalogue global**  
  
    -   Aucun appel à distance ne requis, requête peut être satisfaite localement  
  
    -   ***Cas UPN***  
  
        -   Requête locale forêt UPN index pour UPN fourni (*userPrincipalName; index global*)  
  
            -   Si les entrées retournées == 0 -> écriture revenus  
  
            -   Si les entrées retournées! = 0 -> écriture échoue  
  
                -   Événement enregistré  
  
                -   Renvoie également l’erreur étendue:  
  
                    -   **8648:**  
  
                        *ERROR_DS_UPN_VALUE_NOT_UNIQUE_IN_FOREST*  
  
    -   ***Cas des SPN***  
  
        -   Requête locale forêt SPN index SPN fourni (*servicePrincipalName; index global*)  
  
            -   Si les entrées retournées == 0 -> écriture revenus  
  
            -   Si les entrées retournées! = 0 -> écriture échoue  
  
                -   Événement enregistré  
  
                -   Renvoie également l’erreur étendue:  
  
                    -   **8647:**  
  
                        **ERROR_DS_SPN_VALUE_NOT_UNIQUE_IN_FOREST**  
  
-   **Si contrôleur de domaine! = dans le catalogue global**  
  
    -   Appel à distance **souhaitable** mais pas critique, autrement dit, il s’agit d’une vérification de l’unicité de mieux  
  
        -   Vérification de revenus contre DIT local uniquement si le serveur de catalogue global ne peut pas se trouver  
  
        -   Événement consigné pour indiquer ces  
  
    -   ***Cas UPN***  
  
        -   Soumettre la requête LDAP sur le plus proche dans le catalogue global? Index UPN forêt requête GC pour UPN fourni (*userPrincipalName; index global*)  
  
            -   Si les entrées retournées == 0 -> écriture revenus  
  
            -   Si les entrées retournées! = 0 -> écriture échoue  
  
                -   Événement enregistré  
  
                -   Renvoie également l’erreur étendue:  
  
                    -   **8648:**  
  
                        *ERROR_DS_UPN_VALUE_NOT_UNIQUE_IN_FOREST*  
  
    -   ***Cas des SPN***  
  
        -   Soumettre la requête LDAP sur le plus proche dans le catalogue global? Index SPN requête GC forêt SPN fourni (*servicePrincipalName; index global*)  
  
            -   Si les entrées retournées == 0 -> écriture revenus  
  
            -   Si les entrées retournées! = 0 -> écriture échoue  
  
                -   Événement enregistré  
  
                -   Renvoie également l’erreur étendue:  
  
                    -   **8647:**  
  
                        *ERROR_DS_SPN_VALUE_NOT_UNIQUE_IN_FOREST*  
  
Lorsque des objets supprimés sont ré-animées, SPN ou UPN valeurs présentes sont vérifiés pour l’unicité. Si un doublon est trouvé, la demande échoue.  
  
-   Certaines modifications des attributs comme nom d’hôte DNS, etc. de nom de compte SAM, la modification est apportée, des SPN sont mis à jour en conséquence. Dans le processus, les SPN obsolètes sont supprimés et nouveaux SPN sont créés et ajoutés à la base de données. Les modifications de l’attribut requis par rapport auquel ce chemin d’accès est déclenchée sont:  
  
    -   ATT_DNS_HOST_NAME  
  
    -   ATT_MS_DS_ADDITIONAL_DNS_HOST_NAME  
  
    -   ATT_SAM_ACCOUNT_NAME  
  
    -   ATT_MS_DS_ADDITIONAL_SAM_ACCOUNT_NAME  
  
    -   ATT_SERVER_REFERENCE_BL  
  
    -   ATT_USER_ACCOUNT_CONTROL  
  
Si un de la nouvelle valeur de nom principal de service est un doublon, nous échouer la modification. De la liste ci-dessus, les attributs importants sont ATT_DNS_HOST_NAME (nom de l’ordinateur) et ATT_SAM_ACCOUNT_NAME (nom de compte SAM).  
  
### <a name="try-this-exploring-spn-and-upn-uniqueness"></a>Procédez comme suit: Envisage d’unicité des noms SPN et UPN  
Il s’agit de la première de plusieurs «**essayer**«activités dans le module.  Il n’est pas un guide de laboratoire distinct pour ce module.  Le **essayer** activités sont essentiellement des activités libres qui permettent de vous explorez le matériel leçon dans l’environnement de laboratoire.  Vous avez la possibilité d’après l’invite de commandes ou de sortir de script et de faire votre propre activité.  
  
> [!NOTE]  
> -   Il s’agit de la première de plusieurs «**essayer**«activités.  
> -   Il n’est pas un guide de laboratoire distinct pour ce module.  
> -   Le **essayer** activités sont essentiellement des activités libres qui permettent de vous explorez le matériel leçon dans l’environnement de laboratoire.  
> -   Vous avez la possibilité d’après l’invite de commandes ou de sortir de script et de faire votre propre activité.  
> -   Alors que pas toutes les sections ont un **essayer** invite, il est toujours conseillé d’Explorer le contenu leçon dans le laboratoire, le cas échéant.  
  
Expérimenter unicité des noms SPN et UPN.  Suivez ces instructions, ou effectuer vos propres.  
  
1.  Créer de nouveaux utilisateurs avec UPN  
  
2.  Créer des comptes avec des SPN  
  
3.  Créez un nouvel utilisateur avec un nom UPN déjà précédemment défini ou modifier le nom d’utilisateur principal d’un compte existant.  Faites de même pour un nom principal de service sur un autre compte  
  
    1.  Remplir un compte d’utilisateur existant avec un nom UPN déjà en cours d’utilisation  
  
        1.  À l’aide de PowerShell, ActiveDirectory ou ADSIEDIT (DSAC.exe) Centre d’administration  
  
    2.  Remplir un compte existant avec un SPN déjà en cours d’utilisation  
  
        1.  À l’aide de Windows PowerShell, ADSIEDIT ou SetSPN  
  
4.  Observez les erreurs  
  
**(Facultatif)**  
  
1.  Vérifiez avec le formateur de salle de classe qu’il est OK pour activer la *[Corbeille ActiveDirectory](https://technet.microsoft.com/library/jj574144.aspx#BKMK_EnableRecycleBin)* dans le centre d’administration ActiveDirectory.  Dans ce cas, passer à l’étape suivante.  
  
2.  Remplir l’UPN sur un compte d’utilisateur  
  
3.  Supprimer le compte  
  
4.  Remplir un autre compte avec le même nom d’utilisateur principal en tant que le compte supprimé  
  
5.  Essayez d’utiliser l’interface graphique utilisateur Corbeille recycler restaurer le compte  
  
6.  Imaginez qu'ont été présentées simplement avec l’erreur que vous voyez à l’étape précédente.  (et n’avez pas un historique des étapes que vous venez) Votre objectif est d’effectuer la restauration du compte.  Consultez que les étapes par exemple le classeur.  
  


