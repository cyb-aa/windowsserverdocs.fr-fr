---
ms.assetid: 40bc24b1-2e7d-4e77-bd0f-794743250888
title: Unicité des noms SPN et UPN
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: c9a769fdd9fb7d13c47da465b25bc59e7f55237f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59856740"
---
# <a name="spn-and-upn-uniqueness"></a>Unicité des noms SPN et UPN

>S'applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

**Auteur**: Justin Turner, ingénieur Support résolution Senior auprès du groupe Windows  
  
> [!NOTE]  
> Ce contenu est écrit par un ingénieur du support client Microsoft et est destiné aux administrateurs expérimentés et aux architectes système qui recherchent des explications techniques plus approfondies des fonctionnalités et des solutions Windows Server 2012 R2 que n'en proposent généralement les rubriques de TechNet. Toutefois, il n'a pas subi les mêmes passes de correction. De ce fait, une partie du langage peut sembler moins finalisée que le contenu de TechNet.  
  
## <a name="overview"></a>Vue d'ensemble  
Contrôleurs de domaine exécutant Windows Server 2012 R2 bloquer la création de dupliquer les noms de principal du service (SPN) et les noms d’utilisateur principal (UPN). Cela inclut si la restauration ou la réanimation d’un objet supprimé ou le changement de nom d’un objet donne lieu à un doublon.  
  
### <a name="background"></a>Arrière-plan  
En double Service noms principaux (SPN) couramment se produire et entraîne des échecs d’authentification et peut entraîner une utilisation excessive de l’UC de processus LSASS. Il n’existe aucune méthode de l’emploi pour bloquer l’ajout d’un nom SPN en double ou un UPN. *  
  
Les valeurs de nom UPN en double rompre la synchronisation entre le site AD et Office 365.  
  
*Setspn.exe est couramment utilisé pour créer de nouveaux noms principaux de service et fonctionnellement a été intégrée à la version fournie avec Windows Server 2008 qui ajoute une vérification des doublons.  
  
**Table SEQ Table \\ \* arabe 1 : Unicité des noms UPN et le SPN**  
  
|Fonctionnalité|Commentaire|  
|-----------|-----------|  
|Unicité du nom UPN|Synchronisation de saut UPN en double de AD comptes locaux avec les services Windows Azure basée sur Active Directory telles qu’Office 365.|  
|Unicité du nom principal de service|Kerberos nécessite des noms principaux de service pour l’authentification mutuelle.  SPN en double entraînent des échecs d’authentification.|  
  
Pour plus d’informations sur les conditions d’unicité des noms d’utilisateurs principaux et les noms principaux de service, consultez [contraintes d’unicité](https://msdn.microsoft.com/library/dn392337.aspx).  
  
## <a name="symptoms"></a>Symptômes  
Codes d’erreur 8467 ou 8468 ou leurs hex, symbolique ou les chaînes équivalentes sont consignés à l’écran dans différentes boîtes de dialogue et dans l’événement 2974 d’ID dans le journal des événements Services d’annuaire. La tentative de création d’un UPN ou un SPN en double est bloquée uniquement dans les circonstances suivantes :  
  
-   L’écriture est traité par un contrôleur de domaine Windows Server 2012 R2  
  
**Table SEQ Table \\ \* arabe 2 : Codes d’erreur de l’unicité UPN et le SPN**  
  
|Décimal|Hexadécimal|Symbolique|Chaîne|  
|-----------|-------|------------|----------|  
|8467|21C7|ERROR_DS_SPN_VALUE_NOT_UNIQUE_IN_FOREST|L’opération a échoué, car fournie pour l’ajout/modification de valeur de SPN n’est pas unique de la forêt.|  
|8648|21C8|ERROR_DS_UPN_VALUE_NOT_UNIQUE_IN_FOREST|L’opération a échoué, car valeur UPN fournie pour l’ajout/modification n’est pas unique de la forêt.|  
  
## <a name="new-user-creation-fails-if-upn-is-not-unique"></a>Création d’un utilisateur échoue si l’UPN n’est pas unique  
  
### <a name="dsamsc"></a>DSA.msc  
Le nom d’ouverture de session d’utilisateur que vous avez choisi est déjà en cours d’utilisation de cette entreprise. Choisissez un autre nom d’ouverture de session, puis recommencez l’opération.  
  
![Unicité des noms SPN et UPN](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig01_DupUPN.gif)  
  
Modifier un compte existant :  
  
Le nom d’utilisateur spécifié existe déjà dans l’entreprise. Spécifiez un autre, soit en modifiant le préfixe ou en sélectionnant un autre suffixe dans la liste.  
  
![Unicité des noms SPN et UPN](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig02_DupUPNMod.gif)  
  
### <a name="active-directory-administrative-center-dsacexe"></a>Centre d’administration Active Directory (DSAC.exe)  
Une tentative de création d’un nouvel utilisateur dans le centre d’administration Active Directory avec un UPN qui existe déjà génèrera l’erreur suivante.  
  
![Unicité des noms SPN et UPN](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig03_DupUPNADAC.gif)  
  
**Figure SEQ Figure \\ \* arabe 1 erreur affiché dans le centre d’administration Active Directory lors de la création d’un utilisateur échoue en raison de l’UPN en double**  
  
### <a name="event-2974-source-activedirectorydomainservice"></a>Source d’événement 2974 : ActiveDirectory_DomainService  
![Unicité des noms SPN et UPN](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig04_Event2974.gif)  
  
**Figure SEQ Figure \\ \* arabe 2974 d’ID d’événement 2 avec l’erreur 8648**  
  
L’événement 2974 répertorie la valeur qui a été bloquée et une liste d’un ou plusieurs objets (jusqu'à 10) qui contient déjà cette valeur.  Dans l’illustration suivante, vous pouvez voir cette valeur de l’attribut UPN ***dhunt@blue.contoso.com*** existe déjà sur les quatre autres objets.  Dans la mesure où il s’agit d’une nouvelle fonctionnalité dans Windows Server 2012 R2, création accidentelle de l’UPN et les noms SPN en double dans un environnement mixte se produiront lors de la tentative d’écriture traitement les contrôleurs de domaine de bas niveau.  
  
![Unicité des noms SPN et UPN](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig05_Event2974ShowAllDups.gif)  
  
**Figure SEQ Figure \\ \* arabe 2974 d’événement 3 montrant tous les objets qui contient l’UPN en double**  
  
> [!TIP]  
> Passez en revue l’événement ID 2974s régulièrement :  
>   
> -   identifier les tentatives de création d’UPN en double ou les noms principaux de service  
> -   identifier les objets qui contiennent déjà des doublons  
  
8648 = « L’opération a échoué, car la valeur UPN fournie pour l’ajout/modification n’est pas unique de la forêt. »  
  
### <a name="setspn"></a>SetSPN :  
Setspn.exe a été intégrée à celle-ci depuis la version de Windows Server 2008 de détection de SPN en double lorsque vous utilisez le **»-S «** option.  Vous pouvez ignorer la détection de SPN en double à l’aide de la **»-A »** option toutefois.  La création d’un SPN en double est bloquée lorsque vous ciblez un Windows Server 2012 R2 contrôleur de domaine à l’aide de SetSPN avec l’option - A.  Message d’erreur affiché est identique à celui qui est affiché lorsque vous utilisez l’option-s : « SPN en double trouvé, abandon de l’opération ! »  
  
### <a name="adsiedit"></a>ADSIEDIT :  
  
```  
Operation failed. Error code: 0x21c8  
The operation failed because UPN value provided for addition/modification is not unique forest-wide.  
000021C8: AtrErr: DSID-03200BBA, #1: 0: 000021C8: DSID-03200BBA, problem 1005 (CONSTRAINT_ATT_TYPE), data 0, Att 90290 (userPrincipalName)  
```  
  
![Unicité des noms SPN et UPN](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig06_ADSI21c8.gif)  
  
**Figure SEQ Figure \\ \* message d’erreur de 4 arabe affiché dans ADSIEdit lors de l’ajout de l’UPN en double est bloqué**  
  
### <a name="windows-powershell"></a>Windows PowerShell  
Windows Server 2012 R2 :  
  
![Unicité des noms SPN et UPN](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig07_SetADUser2012.gif)  
  
PS en cours d’exécution à partir de 2012 Server ciblant un contrôleur de domaine Windows Server 2012 R2 :  
  
![Unicité des noms SPN et UPN](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig08_SetADUser2012R2.gif)  
  
DSAC.exe s’exécutant sur Windows Server 2012 ciblant un contrôleur de domaine Windows Server 2012 R2 :  
  
![Unicité des noms SPN et UPN](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig09_UserCreateError.gif)  
  
**Figure SEQ Figure \\ \* erreur de création d’utilisateur arabe DSAC 5 sur non - Windows Server 2012 R2 tout en ciblant le contrôleur de domaine Windows Server 2012 R2**  
  
![Unicité des noms SPN et UPN](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig10_UserModError.gif)  
  
**Figure SEQ Figure \\ \* erreur de modification d’utilisateur arabe DSAC 6 sur non - Windows Server 2012 R2 tout en ciblant le contrôleur de domaine Windows Server 2012 R2**  
  
### <a name="restore-of-an-object-that-would-result-in-a-duplicate-upn-fails"></a>Échec de la restauration d’un objet qui entraînerait un UPN en double :  
![Unicité des noms SPN et UPN](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig11_RestoreDupUPN.gif)  
  
![Unicité des noms SPN et UPN](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig12_RestoreDupUPNError.gif)  
  
Aucun événement n’est consigné quand un objet ne parvient pas à restaurer en raison d’un UPN en double / SPN.  
  
L’UPN de l’objet doit être unique dans l’ordre pour pouvoir être restaurées.  
  
1.  Identifier le nom UPN qui existe sur l’objet dans la Corbeille  
  
2.  Identifier tous les objets qui ont la même valeur  
  
3.  Supprimer le UPN(s) en double  
  
### <a name="identify-the-conflicting-upn-on-the-deleted-objectusing-repadminexe"></a>Identifier l’UPN en conflit sur repadmin.exe utilisation supprimé  
  
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
  
### <a name="to-identify-all-objects-with-the-same-upnusing-repadminexe"></a>Pour identifier tous les objets avec le même UPN : à l’aide de Repadmin.exe  
  
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
> Jusqu’ici non documentées **/ supprimé** paramètre dans repadmin.exe est utilisé pour inclure des objets supprimés dans le jeu de résultats  
  
### <a name="using-global-search"></a>À l’aide de la recherche globale  
  
-   Ouvrez le centre d’administration Active Directory et accédez à **recherche globale**  
  
-   Sélectionnez le **convertir en LDAP** case d’option  
  
-   Type **(userPrincipalName =*ConflictingUPN*)**  
  
    -   Remplacez ***ConflictingUPN*** avec l’UPN réels qui est en conflit  
  
-   Sélectionnez **appliquer**  
  
![Unicité des noms SPN et UPN](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig13_GlobalSearch.gif)  
  
### <a name="using-windows-powershell"></a>Utilisation de Windows PowerShell  
  
```  
Get-ADObject -LdapFilter "(userPrincipalName=dhunt@blue.contoso.com)" -IncludeDeletedObjects -SearchBase "DC=blue,DC=Contoso,DC=com" -SearchScope Subtree -Server winbluedc1.blue.contoso.com  
```  
  
![Unicité des noms SPN et UPN](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig13_GlobalSearchPS.gif)  
  
Si l’objet doit être restaurée, vous devez supprimera les noms UPN en double à partir des autres objets.  Pour qu’un seul objet, il est assez simple d’utiliser ADSIEdit pour supprimer le doublon.  S’il existe plusieurs objets avec des doublons, Windows PowerShell peut être le meilleur outil à utiliser.  
  
L’attribut UserPrincipalName à l’aide de Windows PowerShell, la valeur est null :  
  
![Unicité des noms SPN et UPN](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig15_NullUPN.gif)  
  
> [!NOTE]  
> L’attribut userPrincipalName est l’attribut à valeur unique, donc cette procédure supprime uniquement l’UPN en double.  
  
### <a name="duplicate-spn"></a>Nom SPN en double  
![Unicité des noms SPN et UPN](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig16_DupSPN.gif)  
  
**Figure SEQ Figure \\ \* message d’erreur de 8 arabe affiché dans ADSIEdit lors de l’ajout du nom SPN en double est bloqué**  
  
Enregistré dans les Services de répertoire de journal des événements est un **ActiveDirectory_DomainService** ID d’événement **2974**.  
  
```  
Operation failed. Error code: 0x21c7  
The operation failed   
The attribute value provided is not unique in the forest or partition. Attribute:  
servicePrincipalName Value=<SPN>  
<Object DN> Winerror: 8467  
```  
  
![Unicité des noms SPN et UPN](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig17_DupSPN2974.gif)  
  
**Figure SEQ Figure \\ \* arabe erreur 9 connecté lors de la création de SPN en double est bloquée**  
  
### <a name="workflow"></a>Flux de travail  
  
-   **If DC == GC**  
  
    -   Aucun appel offbox ne requis, requête peut être satisfaite localement  
  
    -   ***Cas d’UPN***  
  
        -   Requête locale forêt UPN index UPN fourni (*userPrincipalName ; index global*)  
  
            -   Si les entrées retournées == 0 -> écriture continue  
  
            -   Si les entrées retournées ! = 0 -> écriture échoue  
  
                -   Événement enregistré  
  
                -   Retourne également l’erreur étendue :  
  
                    -   **8648:**  
  
                        *ERROR_DS_UPN_VALUE_NOT_UNIQUE_IN_FOREST*  
  
    -   ***Cas SPN***  
  
        -   Index de SPN de forêt locale requête SPN fourni (*servicePrincipalName ; index global*)  
  
            -   If entries returned == 0 ->           write proceeds  
  
            -   Si les entrées retournées ! = 0 -> écriture échoue  
  
                -   Événement enregistré  
  
                -   Retourne également l’erreur étendue :  
  
                    -   **8647:**  
  
                        **ERROR_DS_SPN_VALUE_NOT_UNIQUE_IN_FOREST**  
  
-   **If DC != GC**  
  
    -   Appel de Offbox **souhaitable** mais pas critique, par exemple, il s’agit d’une vérification de l’unicité de meilleur effort  
  
        -   Vérification se fait par rapport à DIT local uniquement si le GC ne peut pas être localisé  
  
        -   Événement consigné pour indiquer ces  
  
    -   ***Cas d’UPN***  
  
        -   Envoyer la requête LDAP sur GC le plus proche ? index de UPN de forêt de requête GC pour UPN fourni (*userPrincipalName ; index global*)  
  
            -   If entries returned == 0 ->           write proceeds  
  
            -   Si les entrées retournées ! = 0 -> écriture échoue  
  
                -   Événement enregistré  
  
                -   Retourne également l’erreur étendue :  
  
                    -   **8648:**  
  
                        *ERROR_DS_UPN_VALUE_NOT_UNIQUE_IN_FOREST*  
  
    -   ***Cas SPN***  
  
        -   Envoyer la requête LDAP sur GC le plus proche ? index de SPN de forêt de requête du GC SPN fourni (*servicePrincipalName ; index global*)  
  
            -   Si les entrées retournées == 0 -> écriture continue  
  
            -   Si les entrées retournées ! = 0 -> écriture échoue  
  
                -   Événement enregistré  
  
                -   Retourne également l’erreur étendue :  
  
                    -   **8647:**  
  
                        *ERROR_DS_SPN_VALUE_NOT_UNIQUE_IN_FOREST*  
  
Lorsque les objets supprimés sont ré-animées, valeurs SPN ou UPN présents sont activés pour l’unicité. Si un doublon est trouvé, la demande échoue.  
  
-   Pour certaines modifications d’attribut comme nom d’hôte DNS, etc. de nom de compte SAM, la modification est apportée, noms principaux de service sont mis à jour en conséquence. Dans le processus, les SPN obsolètes sont supprimés et de nouveaux noms principaux de service sont créés et ajoutés à la base de données. Les modifications d’attribut requis par rapport à laquelle ce chemin d’accès est déclenchée sont :  
  
    -   ATT_DNS_HOST_NAME  
  
    -   ATT_MS_DS_ADDITIONAL_DNS_HOST_NAME  
  
    -   ATT_SAM_ACCOUNT_NAME  
  
    -   ATT_MS_DS_ADDITIONAL_SAM_ACCOUNT_NAME  
  
    -   ATT_SERVER_REFERENCE_BL  
  
    -   ATT_USER_ACCOUNT_CONTROL  
  
Si une de la nouvelle valeur de nom principal de service est un doublon, nous ne la modification. De la liste ci-dessus, les attributs importants sont ATT_DNS_HOST_NAME (nom de l’ordinateur) et ATT_SAM_ACCOUNT_NAME (nom de compte SAM).  
  
### <a name="try-this-exploring-spn-and-upn-uniqueness"></a>Essayez ceci : Explorer l’unicité des noms SPN et UPN  
C’est la première de plusieurs «**essayer cela**« activités dans le module.  Il n’est pas un guide de laboratoire distinct pour ce module.  Le **essayer cela** activités sont essentiellement les activités de forme libre qui permettent de vous explorez le matériel de la leçon dans l’environnement de laboratoire.  Vous avez la possibilité d’après l’invite ou sortir de script et élaborer votre propre activité.  
  
> [!NOTE]  
> -   C’est la première de plusieurs «**essayer cela**« activités.  
> -   Il n’est pas un guide de laboratoire distinct pour ce module.  
> -   Le **essayer cela** activités sont essentiellement les activités de forme libre qui permettent de vous explorez le matériel de la leçon dans l’environnement de laboratoire.  
> -   Vous avez la possibilité d’après l’invite ou sortir de script et élaborer votre propre activité.  
> -   Bien que pas toutes les sections ont un **essayer cela** invite, il est toujours conseillé d’Explorer le contenu de la leçon dans le laboratoire, le cas échéant.  
  
Faites des essais avec l’unicité des noms SPN et UPN.  Suivez ces indications ou effectuer votre propre.  
  
1.  Créer de nouveaux utilisateurs avec UPN  
  
2.  Créez des comptes avec des noms principaux de service  
  
3.  Créer un nouvel utilisateur avec un UPN déjà précédemment défini ou modifier le nom UPN d’un compte existant.  Faites de même pour un SPN dans un autre compte  
  
    1.  Remplir un compte d’utilisateur existant avec un UPN déjà en cours d’utilisation  
  
        1.  À l’aide de PowerShell, ADSIEDIT ou Active Directory Administrative Center (DSAC.exe)  
  
    2.  Remplir un compte existant avec un nom principal de service déjà en cours d’utilisation  
  
        1.  À l’aide de Windows PowerShell, ADSIEDIT ou SetSPN  
  
4.  Observez les erreurs  
  
**Si vous le souhaitez**  
  
1.  Vérifiez auprès de l’instructeur en classe qu’il est OK pour activer la *[Corbeille AD](https://technet.microsoft.com/library/jj574144.aspx#BKMK_EnableRecycleBin)* dans le centre d’administration Active Directory.  Dans ce cas, passer à l’étape suivante.  
  
2.  Remplir l’UPN sur un compte d’utilisateur  
  
3.  Supprimer le compte  
  
4.  Remplir un autre compte avec le même UPN en tant que le compte supprimé  
  
5.  Tentative d’utilisation de l’interface utilisateur graphique Bin recycler pour restaurer le compte  
  
6.  Imaginez que vous avez simplement été présenté avec l’erreur que vous voyez dans l’étape précédente.  (et n’avez pas l’historique des étapes réalisées uniquement) Votre objectif consiste à effectuer la restauration du compte.  Consultez que les étapes par exemple le classeur.  
  


