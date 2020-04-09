---
ms.assetid: 40bc24b1-2e7d-4e77-bd0f-794743250888
title: Unicité des noms SPN et UPN
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: f182f79b5bb97e45f1cfd34ad59cf52322f09063
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80823062"
---
# <a name="spn-and-upn-uniqueness"></a>Unicité des noms SPN et UPN

>S’applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

**Auteur**: Justin Turner, ingénieur du support technique senior avec le groupe Windows  
  
> [!NOTE]  
> Ce contenu est écrit par un ingénieur du support client Microsoft et est destiné aux administrateurs expérimentés et aux architectes système qui recherchent des explications techniques plus approfondies des fonctionnalités et des solutions Windows Server 2012 R2 que n'en proposent généralement les rubriques de TechNet. Toutefois, il n'a pas subi les mêmes passes de correction. De ce fait, une partie du langage peut sembler moins finalisée que le contenu de TechNet.  
  
## <a name="overview"></a>Overview  
Les contrôleurs de domaine exécutant Windows Server 2012 R2 bloquent la création de noms de principal du service (SPN) et de noms principaux d’utilisateur (UPN) dupliqués. Cela implique si la restauration ou la réanimation d’un objet supprimé ou le changement de nom d’un objet entraînerait un doublon.  
  
### <a name="background"></a>Arrière-plan  
Les noms de principal du service (SPN) dupliqués se produisent généralement et entraînent des échecs d’authentification et peuvent entraîner une utilisation excessive du processeur LSASS. Il n’existe aucune méthode intégrée pour bloquer l’ajout d’un SPN ou d’un UPN en double. *  
  
Les valeurs UPN dupliquées rompent la synchronisation entre les services AD locaux et Office 365.  
  
\* Setspn. exe est couramment utilisé pour créer de nouveaux noms de principal du service (SPN). il a été conçu de façon fonctionnelle dans la version publiée avec Windows Server 2008 qui ajoute une vérification des doublons.  
  
**Table SEQ Table \\\* arabe 1 : UPN et unicité du nom de principal du service**  
  
|Composant|Commentaire|  
|-----------|-----------|  
|Unicité UPN|Les UPN dupliqués interrompent la synchronisation des comptes AD locaux avec les services Windows Azure AD tels que Office 365.|  
|Unicité du SPN|Kerberos requiert des noms de principal du service pour l’authentification mutuelle.  Les SPN en double entraînent des échecs d’authentification.|  
  
Pour plus d’informations sur les exigences d’unicité pour les UPN et les noms de principal du service, consultez [contraintes d’unicité](https://msdn.microsoft.com/library/dn392337.aspx).  
  
## <a name="symptoms"></a>Symptômes  
Les codes d’erreur 8467 ou 8468 ou leurs équivalents Hex, symboliques ou de chaîne sont consignés dans différentes boîtes de dialogue à l’écran et dans l’ID d’événement 2974 dans le journal des événements des services d’annuaire. La tentative de création d’un UPN ou SPN en double est bloquée uniquement dans les circonstances suivantes :  
  
-   L’écriture est traitée par un contrôleur de périphérique Windows Server 2012 R2  
  
**Table SEQ Table \\\* arabe 2 : codes d’erreur d’unicité UPN et SPN**  
  
|Decimal|Hex|Symbol|String|  
|-----------|-------|------------|----------|  
|8467|21C7|ERROR_DS_SPN_VALUE_NOT_UNIQUE_IN_FOREST|L’opération a échoué, car la valeur SPN fournie pour l’ajout/la modification n’est pas unique à l’ensemble de la forêt.|  
|8648|21C8|ERROR_DS_UPN_VALUE_NOT_UNIQUE_IN_FOREST|L’opération a échoué, car la valeur UPN fournie pour l’ajout/la modification n’est pas unique à l’ensemble de la forêt.|  
  
## <a name="new-user-creation-fails-if-upn-is-not-unique"></a>La création d’un nouvel utilisateur échoue si l’UPN n’est pas unique  
  
### <a name="dsamsc"></a>DSA. msc  
Le nom d’ouverture de session de l’utilisateur que vous avez choisi est déjà utilisé dans cette entreprise. Choisissez un autre nom de connexion, puis réessayez.  
  
![Unicité des noms SPN et UPN](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig01_DupUPN.gif)  
  
Modifier un compte existant :  
  
Le nom d’ouverture de session de l’utilisateur spécifié existe déjà dans l’entreprise. Spécifiez-en un nouveau, soit en modifiant le préfixe, soit en sélectionnant un suffixe différent dans la liste.  
  
![Unicité des noms SPN et UPN](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig02_DupUPNMod.gif)  
  
### <a name="active-directory-administrative-center-dsacexe"></a>Centre d’administration Active Directory (DSAC. exe)  
Une tentative de création d’un nouvel utilisateur dans Centre d’administration Active Directory avec un UPN qui existe déjà génère l’erreur suivante.  
  
![Unicité des noms SPN et UPN](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig03_DupUPNADAC.gif)  
  
**Figure SEQ figure \\\* erreur de l’arabe 1 affichée dans le centre d’administration Active Directory en cas d’échec de la création d’un nouvel utilisateur en raison d’un UPN dupliqué**  
  
### <a name="event-2974-source-activedirectory_domainservice"></a>Source de l’événement 2974 : ActiveDirectory_DomainService  
![Unicité des noms SPN et UPN](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig04_Event2974.gif)  
  
**Figure SEQ figure \\\* l’ID d’événement 2974 arabe 2 avec l’erreur 8648**  
  
L’événement 2974 répertorie la valeur qui a été bloquée, ainsi qu’une liste d’un ou plusieurs objets (jusqu’à 10) qui contiennent déjà cette valeur.  Dans l’illustration suivante, vous pouvez voir que la valeur de l’attribut UPN **<em>dhunt@blue.contoso.com</em>** existe déjà sur quatre autres objets.  Étant donné qu’il s’agit d’une nouvelle fonctionnalité de Windows Server 2012 R2, la création accidentelle d’un UPN et de SPN en double dans un environnement mixte se produit quand les contrôleurs de service de niveau supérieur traitent la tentative d’écriture.  
  
![Unicité des noms SPN et UPN](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig05_Event2974ShowAllDups.gif)  
  
**Figure SEQ figure \\\* l’événement arabe 3 2974 indiquant tous les objets contenant l’UPN en double**  
  
> [!TIP]  
> Examinez régulièrement l’ID d’événement 2974s pour :  
>   
> -   identification des tentatives de création d’un UPN ou SPN en double  
> -   identifier les objets qui contiennent déjà des doublons  
  
8648 = « échec de l’opération, car la valeur UPN fournie pour l’ajout/la modification n’est pas unique à l’ensemble de la forêt ».  
  
### <a name="setspn"></a>Setspn  
Setspn. exe dispose d’une détection de SPN en double intégrée depuis la sortie de Windows Server 2008 lors de l’utilisation de l’option **« -S »** .  Toutefois, vous pouvez ignorer la détection de SPN en double en utilisant l’option **« -A »** .  La création d’un SPN en double est bloquée lors du ciblage d’un contrôleur de service Windows Server 2012 R2 à l’aide de SetSPN avec l’option-A.  Le message d’erreur affiché est le même que celui affiché lors de l’utilisation de l’option-S : « SPN en double trouvé, abandon de l’opération ! »  
  
### <a name="adsiedit"></a>UTILITAIRE  
  
```  
Operation failed. Error code: 0x21c8  
The operation failed because UPN value provided for addition/modification is not unique forest-wide.  
000021C8: AtrErr: DSID-03200BBA, #1: 0: 000021C8: DSID-03200BBA, problem 1005 (CONSTRAINT_ATT_TYPE), data 0, Att 90290 (userPrincipalName)  
```  
  
![Unicité des noms SPN et UPN](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig06_ADSI21c8.gif)  
  
**Figure SEQ figure \\\* message d’erreur arabe 4 affiché dans ADSIEdit quand l’ajout d’un UPN dupliqué est bloqué**  
  
### <a name="windows-powershell"></a>Windows PowerShell  
Windows Server 2012 R2 :  
  
![Unicité des noms SPN et UPN](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig07_SetADUser2012.gif)  
  
PS exécuté à partir du serveur 2012 ciblant un contrôleur de service Windows Server 2012 R2 :  
  
![Unicité des noms SPN et UPN](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig08_SetADUser2012R2.gif)  
  
DSAC. exe s’exécutant sur Windows Server 2012 ciblant un contrôleur de service Windows Server 2012 R2 :  
  
![Unicité des noms SPN et UPN](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig09_UserCreateError.gif)  
  
**Figure SEQ figure \\\* erreur de création de l’utilisateur DSAC arabe 5 sur un autre serveur que Windows Server 2012 R2 en ciblant Windows Server 2012 R2 DC**  
  
![Unicité des noms SPN et UPN](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig10_UserModError.gif)  
  
**Figure SEQ figure \\\* erreur de modification de l’utilisateur en arabe 6 DSAC sur un serveur autre que Windows Server 2012 R2 en ciblant Windows Server 2012 R2 DC**  
  
### <a name="restore-of-an-object-that-would-result-in-a-duplicate-upn-fails"></a>La restauration d’un objet qui entraînerait l’échec d’un UPN en double :  
![Unicité des noms SPN et UPN](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig11_RestoreDupUPN.gif)  
  
![Unicité des noms SPN et UPN](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig12_RestoreDupUPNError.gif)  
  
Aucun événement n’est enregistré en cas d’échec de la restauration d’un objet en raison d’un UPN/SPN en double.  
  
L’UPN de l’objet doit être unique pour pouvoir être restauré.  
  
1.  Identifier l’UPN qui existe sur l’objet dans la corbeille  
  
2.  Identifier tous les objets qui ont la même valeur  
  
3.  Supprimer le ou les UPN en double  
  
### <a name="identify-the-conflicting-upn-on-the-deleted-objectusing-repadminexe"></a>Identifier l’UPN en conflit sur le objectUsing supprimé repadmin. exe  
  
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
  
### <a name="to-identify-all-objects-with-the-same-upnusing-repadminexe"></a>Pour identifier tous les objets ayant le même nom d’utilisateur principal : à l’aide de repadmin. exe  
  
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
> Le paramètre **/Deleted** précédemment non documenté dans repadmin. exe est utilisé pour inclure les objets supprimés dans le jeu de résultats  
  
### <a name="using-global-search"></a>Utilisation de la recherche globale  
  
-   Ouvrir Centre d’administration Active Directory et accéder à la **recherche globale**  
  
-   Sélectionnez la case **d’option convertir en LDAP**  
  
-   Type **(UserPrincipalName =*ConflictingUPN*)**  
  
    -   Remplacer ***ConflictingUPN*** par l’UPN réel en conflit  
  
-   Sélectionner **appliquer**  
  
![Unicité des noms SPN et UPN](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig13_GlobalSearch.gif)  
  
### <a name="using-windows-powershell"></a>Utilisation de Windows PowerShell  
  
```  
Get-ADObject -LdapFilter "(userPrincipalName=dhunt@blue.contoso.com)" -IncludeDeletedObjects -SearchBase "DC=blue,DC=Contoso,DC=com" -SearchScope Subtree -Server winbluedc1.blue.contoso.com  
```  
  
![Unicité des noms SPN et UPN](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig13_GlobalSearchPS.gif)  
  
Si l’objet doit être restauré, vous devrez supprimer les noms UPN en double des autres objets.  Pour un seul objet, il est assez simple d’utiliser ADSIEdit pour supprimer le doublon.  S’il y a plusieurs objets avec des doublons, Windows PowerShell peut être le meilleur outil à utiliser.  
  
Pour utiliser la valeur null pour l’attribut UserPrincipalName à l’aide de Windows PowerShell :  
  
![Unicité des noms SPN et UPN](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig15_NullUPN.gif)  
  
> [!NOTE]  
> L’attribut userPrincipalName étant un attribut à valeur unique, cette procédure supprime uniquement l’UPN dupliqué.  
  
### <a name="duplicate-spn"></a>SPN en double  
![Unicité des noms SPN et UPN](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig16_DupSPN.gif)  
  
**Figure SEQ figure \\\* message d’erreur en arabe 8 affiché dans ADSIEdit quand l’ajout d’un SPN en double est bloqué**  
  
Consigné dans le journal des événements des services d’annuaire est un **ActiveDirectory_DomainService** l’ID d’événement **2974**.  
  
```  
Operation failed. Error code: 0x21c7  
The operation failed   
The attribute value provided is not unique in the forest or partition. Attribute:  
servicePrincipalName Value=<SPN>  
<Object DN> Winerror: 8467  
```  
  
![Unicité des noms SPN et UPN](media/SPN-and-UPN-uniqueness/GTR_ADDS_Fig17_DupSPN2974.gif)  
  
**Figure SEQ figure \\\* erreur arabe 9 consignée lors du blocage de la création d’un SPN en double**  
  
### <a name="workflow"></a>Flux de travail  
  
-   **Si DC = = GC**  
  
    -   Aucun appel offbox requis, la requête peut être satisfaite localement  
  
    -   ***Cas UPN***  
  
        -   Interroger l’index UPN à l’échelle de la forêt locale pour l’UPN fourni (*userPrincipalName ; un index global*)  
  
            -   Si les entrées retournées = = 0-> l’écriture se poursuit  
  
            -   Si les entrées retournées sont ! = 0-> écriture échoue  
  
                -   Événement journalisé  
  
                -   Retourne également une erreur étendue :  
  
                    -   **8648 :**  
  
                        *ERROR_DS_UPN_VALUE_NOT_UNIQUE_IN_FOREST*  
  
    -   ***Cas SPN***  
  
        -   Interroger l’index SPN à l’échelle de la forêt local pour le SPN fourni (*servicePrincipalName ; un index global*)  
  
            -   Si les entrées retournées = = 0-> l’écriture se poursuit  
  
            -   Si les entrées retournées sont ! = 0-> écriture échoue  
  
                -   Événement journalisé  
  
                -   Retourne également une erreur étendue :  
  
                    -   **8647 :**  
  
                        **ERROR_DS_SPN_VALUE_NOT_UNIQUE_IN_FOREST**  
  
-   **Si DC ! = GC**  
  
    -   L’appel de Offbox est **souhaitable** mais pas critique, c’est-à-dire qu’il s’agit d’un contrôle d’unicité optimal.  
  
        -   Vérifier les Proceeds sur un DIT local uniquement si GC est introuvable  
  
        -   Événement consigné pour indiquer une telle  
  
    -   ***Cas UPN***  
  
        -   Envoyer une requête LDAP au GC le plus proche ? interroger l’index UPN de l’ensemble de la forêt du GC pour l’UPN fourni (*userPrincipalName ; un index global*)  
  
            -   Si les entrées retournées = = 0-> l’écriture se poursuit  
  
            -   Si les entrées retournées sont ! = 0-> écriture échoue  
  
                -   Événement journalisé  
  
                -   Retourne également une erreur étendue :  
  
                    -   **8648 :**  
  
                        *ERROR_DS_UPN_VALUE_NOT_UNIQUE_IN_FOREST*  
  
    -   ***Cas SPN***  
  
        -   Envoyer une requête LDAP au GC le plus proche ? interroger l’index SPN à l’échelle de la forêt du GC pour le SPN fourni (*servicePrincipalName ; un index global*)  
  
            -   Si les entrées retournées = = 0-> l’écriture se poursuit  
  
            -   Si les entrées retournées sont ! = 0-> écriture échoue  
  
                -   Événement journalisé  
  
                -   Retourne également une erreur étendue :  
  
                    -   **8647 :**  
  
                        *ERROR_DS_SPN_VALUE_NOT_UNIQUE_IN_FOREST*  
  
Lorsque les objets supprimés sont réanimés, l’unicité des valeurs SPN ou UPN est vérifiée. Si un doublon est trouvé, la demande échoue.  
  
-   Pour certains changements d’attributs tels que le nom d’hôte DNS, le nom de compte SAM, etc., lorsque la modification est effectuée, les noms de principal du service sont mis à jour en conséquence. Dans le processus, les SPN obsolètes sont supprimés et les nouveaux SPN sont générés et ajoutés à la base de données. Les modifications d’attribut nécessaires pour lesquelles ce chemin est déclenché sont les suivantes :  
  
    -   ATT_DNS_HOST_NAME  
  
    -   ATT_MS_DS_ADDITIONAL_DNS_HOST_NAME  
  
    -   ATT_SAM_ACCOUNT_NAME  
  
    -   ATT_MS_DS_ADDITIONAL_SAM_ACCOUNT_NAME  
  
    -   ATT_SERVER_REFERENCE_BL  
  
    -   ATT_USER_ACCOUNT_CONTROL  
  
Si une nouvelle valeur SPN est un doublon, la modification échoue. Dans la liste ci-dessus, les attributs importants sont ATT_DNS_HOST_NAME (nom de l’ordinateur) et ATT_SAM_ACCOUNT_NAME (nom du compte SAM).  
  
### <a name="try-this-exploring-spn-and-upn-uniqueness"></a>Essayez ceci : exploration du SPN et de l’unicité UPN  
Il s’agit de la première des nombreuses activités «**essayer**» dans le module.  Il n’existe pas de guide de laboratoire distinct pour ce module.  Les activités **try this** sont essentiellement des activités de forme libre qui vous permettent d’explorer le matériel de la leçon dans l’environnement Lab.  Vous avez la possibilité de suivre l’invite ou de passer le script et d’afficher votre propre activité.  
  
> [!NOTE]  
> -   Il s’agit de la première des nombreuses activités «**try this**».  
> -   Il n’existe pas de guide de laboratoire distinct pour ce module.  
> -   Les activités **try this** sont essentiellement des activités de forme libre qui vous permettent d’explorer le matériel de la leçon dans l’environnement Lab.  
> -   Vous avez la possibilité de suivre l’invite ou de passer le script et d’afficher votre propre activité.  
> -   Même si toutes les sections ne comportent pas **d’invite de commandes, il est** encore recommandé d’explorer le contenu de la leçon dans le laboratoire, le cas échéant.  
  
Expérimentez l’unicité du SPN et de l’UPN.  Suivez ces invites ou effectuez les vôtres.  
  
1.  Créer des utilisateurs avec UPN  
  
2.  Créer des comptes avec des noms de principal du service  
  
3.  Créez un nouvel utilisateur avec un UPN déjà défini précédemment ou modifiez l’UPN d’un compte existant.  Faire de même pour un SPN sur un autre compte  
  
    1.  Remplir un compte d’utilisateur existant avec un UPN déjà utilisé  
  
        1.  Utilisation de PowerShell, ADSIEdit ou Centre d’administration Active Directory (DSAC. exe)  
  
    2.  Remplir un compte existant avec un nom de principal du service déjà utilisé  
  
        1.  Utilisation de Windows PowerShell, d’ADSIEdit ou de SetSPN  
  
4.  Observer les erreurs  
  
**Éventuellement**  
  
1.  Vérifiez auprès de l’instructeur qu’il est OK d’activer la *[Corbeille ad](https://technet.microsoft.com/library/jj574144.aspx#BKMK_EnableRecycleBin)* dans Centre d’administration Active Directory.  Si c’est le cas, passez à l’étape suivante.  
  
2.  Remplir l’UPN sur un compte d’utilisateur  
  
3.  Supprimer le compte  
  
4.  Remplir un compte différent avec le même UPN que le compte supprimé  
  
5.  Tentative d’utilisation de l’interface utilisateur graphique de la Corbeille pour restaurer le compte  
  
6.  Imaginez que vous venez de voir l’erreur que vous voyez à l’étape précédente.  (et n’ont pas d’historique des étapes que vous venez d’effectuer) Votre objectif est de terminer la restauration du compte.  Consultez le classeur pour obtenir des exemples d’étapes.  
  


