---
title: macfile
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e2ce586c-b316-41d3-90f8-4be0d074cc0e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: bd7646ada96cb02ae434d4ba846da7a9c4dca51b
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66437472"
---
# <a name="macfile"></a>macfile

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Gère le serveur de fichiers pour les serveurs, volumes, des répertoires et fichiers Macintosh. Vous pouvez automatiser les tâches administratives en incluant une série de commandes dans des fichiers batch et en démarrant manuellement ou à des moments prédéterminés. 
-   [Pour modifier les répertoires situés dans des volumes accessible par Macintosh](#BKMK_Moddirs)
-   [Pour joindre des branches de ressources et les données d’un fichier Macintosh](#BKMK_Joinforks)
-   [Pour modifier le message d’ouverture de session et de limiter les sessions](#BKMK_LogonLimit)
-   [Pour ajouter, modifier ou supprimer des volumes accessibles par Macintosh](#BKMK_addvol)

## <a name="BKMK_Moddirs"></a>Pour modifier les répertoires situés dans des volumes accessible par Macintosh

### <a name="syntax"></a>Syntaxe
```
macfile directory[/server:\\<computerName>] /path:<directory> [/owner:<OwnerName>] [/group:<GroupName>] [/permissions:<Permissions>]
```

### <a name="parameters"></a>Paramètres
-   /server:\\\\<computerName> Spécifie le serveur sur lequel modifier un répertoire. Si omis, l’opération est effectuée sur l’ordinateur local.
-   / Path :<directory> Obligatoire. Spécifie le chemin d’accès au répertoire que vous souhaitez modifier. Le répertoire doit exister. **répertoire de fichiers Macintosh** ne crée pas de répertoires.
-   /Owner :<OwnerName> modifie le propriétaire de l’annuaire. Si omis, le propriétaire reste inchangé.
-   /Group :<GroupName> Spécifie ou modifie le groupe principal Macintosh associé à l’annuaire. Si omis, le groupe principal reste inchangé.
-   autorisations :<Permissions> Définit des autorisations sur le répertoire pour le propriétaire, groupe principal et le monde (tout le monde). Un numéro à 11 chiffres est utilisé pour définir les autorisations. Le numéro 1 accorde l’autorisation et 0 révoque une autorisation (par exemple, 11111011000). Si omis, les autorisations restent inchangées.
    La position du chiffre détermine l’autorisation définie, comme décrit dans le tableau suivant.

    |Position|Définit l’autorisation de|
    |------|------------|
    |Première|OwnerSeeFiles|
    |Seconde|OwnerSeeFolders|
    |Troisième|OwnerMakechanges|
    |Quatrième|GroupSeeFiles|
    |Cinquième|GroupSeeFolders|
    |Sixième|GroupMakechanges|
    |Septième|WorldSeeFiles|
    |Huitième|WorldSeeFolders|
    |Neuvième|WorldMakechanges|
    |Dixième|Le répertoire ne peut pas être renommé, déplacé ou supprimé.|
    |Onzième|Les modifications s’appliquent pour le répertoire actif et tous les sous-répertoires.|

-   /?
    Affiche l'aide à l'invite de commandes.

### <a name="remarks"></a>Notes
- Si les informations que vous fournissez contient des espaces ou des caractères spéciaux, utilisez des guillemets autour du texte (par exemple, **»** <em>nom ordinateur</em> **»** ).
- Utilisez **macfiledirectory** de proposer un répertoire existant dans un volume accessible par Macintosh aux utilisateurs de Macintosh. Le **macfiledirectory** commande ne crée pas de répertoires. Utilisez le Gestionnaire de fichiers, l’invite de commandes, ou le **dossier macintosh** commande pour créer un répertoire dans un volume accessible par Macintosh avant d’utiliser le **répertoire de fichiers Macintosh** commande.
  ### <a name="BKMK_Examples"></a>Exemples
  L’exemple suivant modifie les autorisations des ventes de mai de sous-répertoire, dans le volume accessible par Macintosh statistiques, sur le lecteur E du serveur local. L’exemple assigne consultez fichiers, consultez les dossiers et vérifiez les autorisations au propriétaire et voir les fichiers et dossiers de voir les autorisations à tous les autres utilisateurs, tout en empêchant le répertoire soit renommé, déplacé ou supprimé.
  ```
  macfile directory /path:"e:\statistics\may sales" /permissions:11111011000
  ```

## <a name="BKMK_Joinforks"></a>Pour joindre des branches de ressources et les données d’un fichier Macintosh

### <a name="syntax"></a>Syntaxe
```
macfile forkize[/server:\\<computerName>] [/creator:<CreatorName>] [/type:<typeName>]  [/datafork:<Filepath>] [/resourcefork:<Filepath>] /targetfile:<Filepath>
```

### <a name="parameters"></a>Paramètres

|         Paramètre          |                                                                                                           Description                                                                                                            |
|----------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| /server:\\\\<computerName> |                                                            Spécifie le serveur sur laquelle joindre des fichiers. Si omis, l’opération est effectuée sur l’ordinateur local.                                                            |
|   /Creator :<CreatorName>   |                                      Spécifie le créateur du fichier. Le finder Macintosh utilise le **/creator** une option de ligne de commande pour déterminer l’application qui a créé le fichier.                                       |
|      / type :<typeName>      |                                 Spécifie le type de fichier. Le finder Macintosh utilise le **/type** une option de ligne de commande pour déterminer le type de fichier dans l’application qui a créé le fichier.                                 |
|    /DATAFORK :<Filepath>    |                                                                   Spécifie l’emplacement de la branche de données qui doit être joint. Vous pouvez spécifier un chemin d’accès à distance.                                                                   |
|  /RESOURCEFORK :<Filepath>  |                                                                 Spécifie l’emplacement de la branche de ressources qui doit être joint. Vous pouvez spécifier un chemin d’accès à distance.                                                                 |
|   /TargetFile :<Filepath>   | Obligatoire. Spécifie l’emplacement du fichier qui est créé en joignant une branche de données et une branche de ressources, ou spécifie l’emplacement du fichier dont l’ou les creator vous modifiez. Le fichier doit être sur le serveur spécifié. |
|             /?             |                                                                                               Affiche l'aide à l'invite de commandes.                                                                                               |

### <a name="remarks"></a>Notes
- Si les informations que vous fournissez contient des espaces ou des caractères spéciaux, utilisez des guillemets autour du texte (par exemple, **»** <em>nom ordinateur</em> **»** ).

### <a name="examples"></a>Exemples
Pour créer le fichier treeapp sur le volume accessible par Macintosh D:\Release à l’aide de la branche de ressources C:\Cross\Mac\Appcode et pour présenter ce nouveau fichier aux clients Macintosh en tant qu’application (applications Macintosh utilisent le type APPL) avec le créateur (signature ) la valeur MAGNOLIA, type :
```
macfile forkize /resourcefork:c:\cross\mac\appcode /type:APPL /creator:MAGNOLIA /targetfile:D:\Release\treeapp
```
Pour modifier le créateur en Microsoft Word 5.1 pour le fichier Word.txt situé dans le répertoire D:\Word documents\Group files, sur le serveur \\\SERverA, tapez :
```
macfile forkize /server:\\servera /creator:MSWD /type:TEXT /targetfile:"d:\Word documents\Group files\Word.txt"
```

## <a name="BKMK_LogonLimit"></a>Pour modifier le message d’ouverture de session et de limiter les sessions
### <a name="syntax"></a>Syntaxe
```
macfile server [/server:\\<computerName>] [/maxsessions:{Number | unlimited}] [/loginmessage:<Message>]
```

### <a name="parameters"></a>Paramètres

|               Paramètre                |                                                                                                                                                                           Description                                                                                                                                                                            |
|----------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|       /server:\\\\<computerName>       |                                                                                                                        Spécifie le serveur sur lequel modifier les paramètres. Si omis, l’opération est effectuée sur l’ordinateur local.                                                                                                                         |
| / maxsessions : {nombre &#124; illimité} |                                                                                         Spécifie le nombre maximal d’utilisateurs qui peuvent utiliser le fichier et serveurs d’impression pour Macintosh simultanément. Si omis, le **maxsessions** paramètre pour le serveur reste inchangé.                                                                                         |
|        loginmessage :<Message>         | modifications que les utilisateurs de Macintosh de message voient lors de la connexion au serveur de fichiers pour Macintosh serveur. Le nombre maximal de caractères pour le message d’ouverture de session est 199. Si omis, le **loginmessage** du message pour le serveur reste inchangé. Pour supprimer un message d’ouverture de session existant, vous devez inclure le **loginmessage** paramètre, mais ne modifiez pas le *Message* variable vide. |
|                   /?                   |                                                                                                                                                               Affiche l'aide à l'invite de commandes.                                                                                                                                                               |

### <a name="remarks"></a>Notes
- Si les informations que vous fournissez contient des espaces ou des caractères spéciaux, utilisez des guillemets autour du texte (par exemple, **»** <em>nom ordinateur</em> **»** ).

### <a name="examples"></a>Exemples
Pour modifier le nombre de fichiers et serveur d’impression pour les sessions Macintosh qui sont autorisées sur le serveur local à partir du paramètre actuel de cinq sessions et d’ajouter le message d’ouverture de session « Se déconnecter à partir du serveur pour Macintosh lorsque vous avez terminé. », tapez :
```
macfile server /maxsessions:5 /loginmessage:"Log off from Server for Macintosh when you are finished."
```

## <a name="BKMK_addvol"></a>Pour ajouter, modifier ou supprimer des volumes accessibles par Macintosh
### <a name="syntax"></a>Syntaxe
```
macfile volume {/add|/set} [/server:\\<computerName>] /name:<volumeName>/path:<directory>[/readonly:{true | false}] [/guestsallowed:{true | false}] [/password:<Password>] [/maxusers:{<Number>>|unlimited}]
macfile volume /remove[/server:\\<computerName>] /name:<volumeName>
```

### <a name="parameters"></a>Paramètres

|              Paramètre               |                                                                                                                                                                       Description                                                                                                                                                                        |
|--------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|          {/add &#124; /set}          |                                                                                                                      Obligatoire lorsque vous ajoutez ou modifiez un volume accessible par Macintosh. Ajoute ou modifie le volume spécifié.                                                                                                                       |
|      /server:\\\\<computerName>      |                                                                                                             Spécifie le serveur sur lequel ajouter, modifier ou supprimer un volume. Si omis, l’opération est effectuée sur l’ordinateur local.                                                                                                              |
|          /Name :<volumeName>          |                                                                                                                                          Obligatoire. Spécifie le nom de volume à être ajouté, modifié ou supprimé.                                                                                                                                           |
|          / Path :<directory>           |                                                                                                                Obligatoire et valide uniquement lorsque vous ajoutez un volume. Spécifie le chemin d’accès au répertoire racine du volume doit être ajouté.                                                                                                                 |
|    /readonly:{true &#124; false}     | Spécifie si les utilisateurs peuvent modifier des fichiers dans le volume. Tapez la valeur true pour spécifier que les utilisateurs ne peuvent pas modifier les fichiers du volume. Tapez false pour indiquer que les utilisateurs peuvent modifier des fichiers dans le volume. Si omis lors de l’ajout d’un volume, les modifications apportées aux fichiers sont autorisées. Si omis lors de la modification d’un volume, le **readonly** paramètre pour le volume reste inchangé. |
|  /guestsallowed:{true &#124; false}  |      Spécifie si les utilisateurs qui se connectent en tant qu’invités peuvent utiliser le volume. Tapez la valeur true pour spécifier que les invités peuvent utiliser le volume. Tapez false pour spécifier que les invités ne peuvent pas utiliser le volume. Si omis lors de l’ajout d’un volume, les invités peuvent utiliser le volume. Si omis lors de la modification d’un volume, le **guestsallowed** paramètre pour le volume reste inchangé.       |
|         / Password :<Password>         |                                                                               Spécifie un mot de passe qui sera requise pour accéder au volume. Si omis lors de l’ajout d’un volume, aucun mot de passe n’est créé. Si omis lors de la modification d’un volume, le mot de passe reste inchangé.                                                                               |
| /maxusers : {<Number>>&#124;illimité} |                                                 Spécifie le nombre maximal d’utilisateurs qui peuvent utiliser simultanément les fichiers sur le volume. Si omis lors de l’ajout d’un volume, un nombre illimité d’utilisateurs peut utiliser le volume. Si omis lors de la modification d’un volume, le **maxusers** valeur reste inchangée.                                                 |
|               /remove                |                                                                                                                                Obligatoire lorsque vous supprimez un volume accessible aux Macintosh. Supprime le volume spécifié.                                                                                                                                |
|                  /?                  |                                                                                                                                                           Affiche l'aide à l'invite de commandes.                                                                                                                                                           |

### <a name="remarks"></a>Notes
- Si les informations que vous fournissez contient des espaces ou des caractères spéciaux, utilisez des guillemets autour du texte (par exemple, **»** <em>nom ordinateur</em> **»** ).

### <a name="examples"></a>Exemples
Pour créer un volume appelé nous Marketing de statistiques sur le serveur local, à l’aide de l’annuaire de statistiques dans le lecteur E et pour spécifier que le volume n’est pas accessible aux invités, tapez :
```
macfile volume /add /name:"US Marketing Statistics" /guestsallowed:false /path:e:\Stats
```
Pour modifier le volume créé ci-dessus en lecture seule et d’exiger un mot de passe et pour définir le nombre maximal d’utilisateurs à cinq, type :
```
macfile volume /set /name:"US Marketing Statistics" /readonly:true /password:saturn /maxusers:5
```
Pour ajouter un volume appelé Conception paysage sur le serveur \\\Magnolia, à l’aide de l’annuaire d’arborescences dans le lecteur E et pour spécifier que le volume est accessible aux invités, type :
```
macfile volume /add /server:\\Magnolia /name:"Landscape Design" /path:e:\trees
```
Pour supprimer le volume de rapports des ventes sur le serveur local, tapez :
```
macfile volume /remove /name:"Sales Reports"
```

## <a name="additional-references"></a>Références supplémentaires
-   [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
