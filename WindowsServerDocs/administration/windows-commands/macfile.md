---
title: macfile
description: Rubrique relative aux commandes Windows pour * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: e2ce586c-b316-41d3-90f8-4be0d074cc0e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0156be5a3209bf8cedf13b35ceab61ef38a0f49a
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80840312"
---
# <a name="macfile"></a>macfile

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Gère le serveur de fichiers pour les serveurs, les volumes, les répertoires et les fichiers Macintosh. Vous pouvez automatiser les tâches administratives en incluant une série de commandes dans des fichiers de commandes et en les démarrant manuellement ou à des moments prédéterminés. 
-   [Pour modifier des répertoires dans des volumes accessibles par Macintosh](#BKMK_Moddirs)
-   [Pour joindre les données et les branches de ressources d’un fichier Macintosh](#BKMK_Joinforks)
-   [Pour modifier le message d’ouverture de session et limiter les sessions](#BKMK_LogonLimit)
-   [Pour ajouter, modifier ou supprimer des volumes accessibles par Macintosh](#BKMK_addvol)

## <a name="to-modify-directories-in-macintosh-accessible-volumes"></a><a name=BKMK_Moddirs></a>Pour modifier des répertoires dans des volumes accessibles par Macintosh

### <a name="syntax"></a>Syntaxe
```
macfile directory[/server:\\<computerName>] /path:<directory> [/owner:<OwnerName>] [/group:<GroupName>] [/permissions:<Permissions>]
```

#### <a name="parameters"></a>Paramètres
-   /Server :\\\\<computerName> spécifie le serveur sur lequel modifier un répertoire. En cas d’omission, l’opération est effectuée sur l’ordinateur local.
-   /Path :<directory> requis. Spécifie le chemin d’accès au répertoire que vous souhaitez modifier. Le répertoire doit exister. le **répertoire MacFile** ne crée pas de répertoires.
-   /owner :<OwnerName> modifie le propriétaire du répertoire. En cas d’omission, le propriétaire reste inchangé.
-   /Group :<GroupName> spécifie ou modifie le groupe principal Macintosh qui est associé à l’annuaire. En cas d’omission, le groupe principal reste inchangé.
-   /Permissions :<Permissions> définit les autorisations sur le répertoire pour le propriétaire, le groupe principal et le monde (tout le monde). Un nombre de 11 chiffres est utilisé pour définir des autorisations. Le nombre 1 accorde l’autorisation et la valeur 0 révoque l’autorisation (par exemple, 11111011000). En cas d’omission, les autorisations restent inchangées.
    La position du chiffre détermine l’autorisation qui est définie, comme décrit dans le tableau suivant.

    |Position|Définit l’autorisation pour|
    |------|------------|
    |Premier|OwnerSeeFiles|
    |Deuxième|OwnerSeeFolders|
    |Troisième|OwnerMakechanges|
    |Quatrième|GroupSeeFiles|
    |Cinquième|GroupSeeFolders|
    |Sixième|GroupMakechanges|
    |Troisième|WorldSeeFiles|
    |Huitième|WorldSeeFolders|
    |Neuvième|WorldMakechanges|
    |Cérémonie|Le répertoire ne peut pas être renommé, déplacé ou supprimé.|
    |Onzième|Les modifications s’appliquent au répertoire actif et à tous ses sous-répertoires.|

-   /?
    Affiche l'aide à l'invite de commandes.

### <a name="remarks"></a>Notes
- Si les informations que vous fournissez contiennent des espaces ou des caractères spéciaux, utilisez des guillemets autour du texte (par exemple, * * * * nom de l'<em>ordinateur</em>* * * *).
- Utilisez **macfiledirectory** pour mettre un répertoire existant dans un volume accessible aux Macintosh à la disposition des utilisateurs Macintosh. La commande **macfiledirectory** ne crée pas de répertoires. Utilisez le gestionnaire de fichiers, l’invite de commandes ou la commande **Macintosh nouveau dossier** pour créer un répertoire dans un volume accessible par Macintosh avant d’utiliser la commande **macfile Directory** .
  ### <a name="examples"></a><a name=BKMK_Examples></a>Illustre
  L’exemple suivant modifie les autorisations du sous-répertoire mai Sales, dans les statistiques de volume accessibles aux Macintosh, sur le lecteur E du serveur local. L’exemple attribue les autorisations voir les fichiers, voir les dossiers et apporter des modifications au propriétaire et afficher les fichiers et voir les autorisations de dossiers pour tous les autres utilisateurs, tout en empêchant le renommage, le déplacement ou la suppression du répertoire.
  ```
  macfile directory /path:e:\statistics\may sales /permissions:11111011000
  ```

## <a name="to-join-a-macintosh-files-data-and-resource-forks"></a><a name=BKMK_Joinforks></a>Pour joindre les données et les branches de ressources d’un fichier Macintosh

### <a name="syntax"></a>Syntaxe
```
macfile forkize[/server:\\<computerName>] [/creator:<CreatorName>] [/type:<typeName>]  [/datafork:<Filepath>] [/resourcefork:<Filepath>] /targetfile:<Filepath>
```

#### <a name="parameters"></a>Paramètres

|         Paramètre          |                                                                                                           Description                                                                                                            |
|----------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| /Server :\\\\<computerName> |                                                            Spécifie le serveur sur lequel joindre des fichiers. En cas d’omission, l’opération est effectuée sur l’ordinateur local.                                                            |
|   /Creator :<CreatorName>   |                                      Spécifie le créateur du fichier. Macintosh Finder utilise l’option de ligne de commande **/Creator** pour déterminer l’application qui a créé le fichier.                                       |
|      /type :<typeName>      |                                 Spécifie le type de fichier. Macintosh Finder utilise l’option de ligne de commande **/type** pour déterminer le type de fichier au sein de l’application qui a créé le fichier.                                 |
|    /DATAFORK :<Filepath>    |                                                                   Spécifie l’emplacement de la branche de données qui doit être jointe. Vous pouvez spécifier un chemin d’accès distant.                                                                   |
|  /RESOURCEFORK :<Filepath>  |                                                                 Spécifie l’emplacement de la branche de ressource qui doit être jointe. Vous pouvez spécifier un chemin d’accès distant.                                                                 |
|   /TargetFile :<Filepath>   | Obligatoire. Spécifie l’emplacement du fichier créé en joignant une branche de données et une fourche de ressource, ou spécifie l’emplacement du fichier dont vous modifiez le type ou le créateur. Le fichier doit se trouver sur le serveur spécifié. |
|             /?             |                                                                                               Affiche l'aide à l'invite de commandes.                                                                                               |

### <a name="remarks"></a>Notes
- Si les informations que vous fournissez contiennent des espaces ou des caractères spéciaux, utilisez des guillemets autour du texte (par exemple, * * * * nom de l'<em>ordinateur</em>* * * *).

### <a name="examples"></a>Exemples
Pour créer le fichier Treeapp sur le volume accessible par Macintosh D:\Release, à l’aide de la branche de ressources C:\Cross\Mac\Appcode et pour que ce nouveau fichier apparaisse aux clients Macintosh en tant qu’application (les applications Macintosh utilisent le type APPL) avec le créateur (signature) défini sur MAGNOLIA, tapez :
```
macfile forkize /resourcefork:c:\cross\mac\appcode /type:APPL /creator:MAGNOLIA /targetfile:D:\Release\treeapp
```
Pour remplacer le créateur de fichier par Microsoft Word 5,1, pour le fichier WOrd. txt dans le répertoire D:\Word documents\Group files, sur le serveur \\\SERverA, tapez :
```
macfile forkize /server:\\servera /creator:MSWD /type:TEXT /targetfile:d:\Word documents\Group files\Word.txt
```

## <a name="to-change-the-logon-message-and-limit-sessions"></a><a name=BKMK_LogonLimit></a>Pour modifier le message d’ouverture de session et limiter les sessions
### <a name="syntax"></a>Syntaxe
```
macfile server [/server:\\<computerName>] [/maxsessions:{Number | unlimited}] [/loginmessage:<Message>]
```

#### <a name="parameters"></a>Paramètres

|               Paramètre                |                                                                                                                                                                           Description                                                                                                                                                                            |
|----------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|       /Server :\\\\<computerName>       |                                                                                                                        Spécifie le serveur sur lequel modifier les paramètres. En cas d’omission, l’opération est effectuée sur l’ordinateur local.                                                                                                                         |
| /maxsessions : {nombre &#124; illimité} |                                                                                         Spécifie le nombre maximal d’utilisateurs qui peuvent utiliser simultanément des serveurs de fichiers et d’impression pour Macintosh. En cas d’omission, le paramètre **maxsessions** du serveur reste inchangé.                                                                                         |
|        /loginmessage :<Message>         | modifie le message que les utilisateurs Macintosh voient quand ils se connectent au serveur de fichiers pour le serveur Macintosh. Le nombre maximal de caractères pour le message d’ouverture de session est 199. En cas d’omission, le message **loginmessage** pour le serveur reste inchangé. Pour supprimer un message d’ouverture de session existant, incluez le paramètre **/loginmessage** , mais laissez la variable *message* vide. |
|                   /?                   |                                                                                                                                                               Affiche l'aide à l'invite de commandes.                                                                                                                                                               |

### <a name="remarks"></a>Notes
- Si les informations que vous fournissez contiennent des espaces ou des caractères spéciaux, utilisez des guillemets autour du texte (par exemple, * * * * nom de l'<em>ordinateur</em>* * * *).

### <a name="examples"></a>Exemples
Pour modifier le nombre de sessions de serveur de fichiers et d’impression pour les sessions Macintosh autorisées sur le serveur local du paramètre actuel à cinq sessions, et pour ajouter le message d’ouverture de session à partir du serveur pour Macintosh Lorsque vous avez terminé., tapez :
```
macfile server /maxsessions:5 /loginmessage:Log off from Server for Macintosh when you are finished.
```

## <a name="to-add-change-or-remove-macintosh-accessible-volumes"></a><a name=BKMK_addvol></a>Pour ajouter, modifier ou supprimer des volumes accessibles par Macintosh
### <a name="syntax"></a>Syntaxe
```
macfile volume {/add|/set} [/server:\\<computerName>] /name:<volumeName>/path:<directory>[/readonly:{true | false}] [/guestsallowed:{true | false}] [/password:<Password>] [/maxusers:{<Number>>|unlimited}]
macfile volume /remove[/server:\\<computerName>] /name:<volumeName>
```

#### <a name="parameters"></a>Paramètres

|              Paramètre               |                                                                                                                                                                       Description                                                                                                                                                                        |
|--------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|          {/Add &#124; /Set}          |                                                                                                                      Obligatoire lorsque vous ajoutez ou modifiez un volume accessible par Macintosh. Ajoute ou modifie le volume spécifié.                                                                                                                       |
|      /Server :\\\\<computerName>      |                                                                                                             Spécifie le serveur sur lequel ajouter, modifier ou supprimer un volume. En cas d’omission, l’opération est effectuée sur l’ordinateur local.                                                                                                              |
|          /Name :<volumeName>          |                                                                                                                                          Obligatoire. Spécifie le nom du volume à ajouter, modifier ou supprimer.                                                                                                                                           |
|          /Path :<directory>           |                                                                                                                Obligatoire et valide uniquement lorsque vous ajoutez un volume. Spécifie le chemin d’accès au répertoire racine du volume à ajouter.                                                                                                                 |
|    /ReadOnly : {true &#124; false}     | Spécifie si les utilisateurs peuvent modifier des fichiers dans le volume. tapez true pour spécifier que les utilisateurs ne peuvent pas modifier les fichiers du volume. tapez false pour spécifier que les utilisateurs peuvent modifier des fichiers dans le volume. En cas d’omission lors de l’ajout d’un volume, les modifications apportées aux fichiers sont autorisées. En cas d’omission lors de la modification d’un volume, le paramètre **ReadOnly** du volume reste inchangé. |
|  /guestsallowed : {true &#124; false}  |      Spécifie si les utilisateurs qui ouvrent une session en tant qu’invités peuvent utiliser le volume. tapez true pour spécifier que les invités peuvent utiliser le volume. tapez false pour indiquer que les invités ne peuvent pas utiliser le volume. En cas d’omission lors de l’ajout d’un volume, les invités peuvent utiliser le volume. En cas d’omission lors de la modification d’un volume, le paramètre **guestsallowed** du volume reste inchangé.       |
|         /Password :<Password>         |                                                                               Spécifie un mot de passe qui sera requis pour accéder au volume. Si ce paramètre n’est pas spécifié lors de l’ajout d’un volume, aucun mot de passe n’est créé. Si ce paramètre n’est pas spécifié lors de la modification d’un volume, le mot de passe reste inchangé.                                                                               |
| /maxusers : {<Number>>&#124;illimité} |                                                 Spécifie le nombre maximal d’utilisateurs qui peuvent utiliser simultanément les fichiers sur le volume. En cas d’omission lors de l’ajout d’un volume, un nombre illimité d’utilisateurs peut utiliser le volume. En cas d’omission lors de la modification d’un volume, la valeur **maxusers** reste inchangée.                                                 |
|               /remove                |                                                                                                                                Obligatoire lorsque vous supprimez un volume accessible aux Macintosh. supprime le volume spécifié.                                                                                                                                |
|                  /?                  |                                                                                                                                                           Affiche l'aide à l'invite de commandes.                                                                                                                                                           |

### <a name="remarks"></a>Notes
- Si les informations que vous fournissez contiennent des espaces ou des caractères spéciaux, utilisez des guillemets autour du texte (par exemple, * * * * nom de l'<em>ordinateur</em>* * * *).

### <a name="examples"></a>Exemples
Pour créer un volume appelé statistiques marketing américaines sur le serveur local, en utilisant le répertoire stats du lecteur E et pour indiquer que le volume n’est pas accessible par les invités, tapez :
```
macfile volume /add /name:US Marketing Statistics /guestsallowed:false /path:e:\Stats
```
Pour modifier le volume créé ci-dessus pour qu’il soit en lecture seule et pour exiger un mot de passe, et pour définir le nombre d’utilisateurs maximum sur cinq, tapez :
```
macfile volume /set /name:US Marketing Statistics /readonly:true /password:saturn /maxusers:5
```
Pour ajouter un volume appelé conception paysage, sur le serveur \\\Magnolia, en utilisant le répertoire arbres du lecteur E et pour indiquer que le volume est accessible par les invités, tapez :
```
macfile volume /add /server:\\Magnolia /name:Landscape Design /path:e:\trees
```
Pour supprimer le volume appelé rapports sur les ventes sur le serveur local, tapez :
```
macfile volume /remove /name:Sales Reports
```

## <a name="additional-references"></a>Références supplémentaires
-   - [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
