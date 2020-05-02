---
title: wevtutil
description: Rubrique de référence pour wevtutil, qui vous permet de récupérer des informations sur les journaux des événements et les serveurs de publication.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: d4c791e0-7e59-45c5-aa55-0223b77a4822 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: af38d4eb1aed778574fdbba00851d9ed2b98ec6f
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82725825"
---
# <a name="wevtutil"></a>wevtutil



Permet de récupérer des informations sur les journaux des événements et les serveurs de publication. Vous pouvez également utiliser cette commande pour installer et désinstaller les fichiers manifeste des événements, pour exécuter des requêtes et pour exporter, archiver et vider les journaux. 

## <a name="syntax"></a>Syntaxe

```
wevtutil [{el | enum-logs}] [{gl | get-log} <Logname> [/f:<Format>]]
[{sl | set-log} <Logname> [/e:<Enabled>] [/i:<Isolation>] [/lfn:<Logpath>] [/rt:<Retention>] [/ab:<Auto>] [/ms:<MaxSize>] [/l:<Level>] [/k:<Keywords>] [/ca:<Channel>] [/c:<Config>]] 
[{ep | enum-publishers}] 
[{gp | get-publisher} <Publishername> [/ge:<Metadata>] [/gm:<Message>] [/f:<Format>]] [{im | install-manifest} <Manifest>] 
[{um | uninstall-manifest} <Manifest>] [{qe | query-events} <Path> [/lf:<Logfile>] [/sq:<Structquery>] [/q:<Query>] [/bm:<Bookmark>] [/sbm:<Savebm>] [/rd:<Direction>] [/f:<Format>] [/l:<Locale>] [/c:<Count>] [/e:<Element>]] 
[{gli | get-loginfo} <Logname> [/lf:<Logfile>]] 
[{epl | export-log} <Path> <Exportfile> [/lf:<Logfile>] [/sq:<Structquery>] [/q:<Query>] [/ow:<Overwrite>]] 
[{al | archive-log} <Logpath> [/l:<Locale>]] 
[{cl | clear-log} <Logname> [/bu:<Backup>]] [/r:<Remote>] [/u:<Username>] [/p:<Password>] [/a:<Auth>] [/uni:<Unicode>]
```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|{El \| enum-logs}|Affiche les noms de tous les journaux.|
|{GL \| -récupération-Journal} \<LogName> [/F :\<format>]|Affiche des informations de configuration pour le journal spécifié, qui indique si le journal est activé ou non, la limite de taille maximale actuelle du journal et le chemin d’accès au fichier dans lequel le journal est stocké.|
|{SL \| Set-log} \<LogName> [/E :\<Enabled>] [/i :\<isolation>] [/LFN :\<LogPath>] [/RT :\<Retention>] [\</AB : auto>] [\</ms. : MaxSize>] [\</L : Level>] [\</k : Keywords>\<] [/ca : Channel>\<] [/c : config>]|Modifie la configuration du journal spécifié.|
|{EP \| enum-Publishers}|Affiche les éditeurs d’événements sur l’ordinateur local.|
|{GP \| -éditeur} \<PublisherName> [/GE :\<Metadata>] [/GM :\<message>] [/f :\<format>]]|Affiche les informations de configuration de l’éditeur d’événements spécifié.|
|{installation \| de messagerie instantanée-manifeste} \<> du manifeste|Installe les journaux et les éditeurs d’événements à partir d’un manifeste. Pour plus d’informations sur les manifestes d’événements et l’utilisation de ce paramètre, consultez le kit de développement logiciel (SDK) du journal des[https://msdn.microsoft.com](https://msdn.microsoft.com)événements Windows sur le site Web Microsoft Developers Network (MSDN) ().|
|{désinstallation de la messagerie unifiée \| -manifeste} \<> du manifeste|Désinstalle tous les serveurs de publication et journaux d’un manifeste. Pour plus d’informations sur les manifestes d’événements et l’utilisation de ce paramètre, consultez le kit de développement logiciel (SDK) du journal des[https://msdn.microsoft.com](https://msdn.microsoft.com)événements Windows sur le site Web Microsoft Developers Network (MSDN) ().|
|{qe \| Query-Events} \<Path> [/LF :\<logfile>] [/sq :\<Structquery>] [/q :\<Query>] [/BM :\<Bookmark>] [/SBM :\<Savebm>] [/RD :\<direction>] [/F :\<format>] [/l :\<locale>] [/c :\<Count>] [/e :\<Element>]|Lit des événements à partir d’un journal des événements, à partir d’un fichier journal ou à l’aide d’une requête structurée. Par défaut, vous fournissez un nom de \<journal pour le chemin d’accès>. Toutefois, si vous utilisez l’option **/LF** , \<Path> doit être un chemin d’accès à un fichier journal. Si vous utilisez le paramètre **/sq** , \<Path> doit être un chemin d’accès à un fichier qui contient une requête structurée.|
|{Gli \| -loginfo} \<LogName> [/LF :\<logfile>]|Affiche des informations d’État sur un journal des événements ou un fichier journal. Si l’option **/LF** est utilisée, \<LogName> est le chemin d’accès à un fichier journal. Vous pouvez exécuter **wevtutil El** pour obtenir une liste des noms de journaux.|
|{EPL \| exporter-Journal} \<Path> \<exportfile> [/LF :\<logfile>] [/sq :\<Structquery>] [/q :\<Query>] [/ow :\<overwrite>]|Exporte des événements à partir d’un journal des événements, à partir d’un fichier journal ou à l’aide d’une requête structurée vers le fichier spécifié. Par défaut, vous fournissez un nom de \<journal pour le chemin d’accès>. Toutefois, si vous utilisez l’option **/LF** , \<Path> doit être un chemin d’accès à un fichier journal. Si vous utilisez l’option **/sq** , \<Path> doit être un chemin d’accès à un fichier qui contient une requête structurée. \<Exportfile> est le chemin d’accès au fichier dans lequel les événements exportés seront stockés.|
|{Al \| Archive-log} \<LogPath> [/L :\<locale>]|Archive le fichier journal spécifié dans un format autonome. Un sous-répertoire avec le nom des paramètres régionaux est créé et toutes les informations spécifiques aux paramètres régionaux sont enregistrées dans ce sous-répertoire. Une fois que le répertoire et le fichier journal sont créés en exécutant **wevtutil al**, les événements du fichier peuvent être lus, que le serveur de publication soit installé ou non.|
|{CL \| effacer-Journal} \<LogName> [/BU :\<Backup>]|Efface les événements du journal des événements spécifié. L’option **/bu** peut être utilisée pour sauvegarder les événements effacés.|

## <a name="options"></a>Options

|       Option       |                                                                                                                                                                                                                                                                 Description                                                                                                                                                                                                                                                                  |
|--------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    /f :\<format>    |                                                                                                                                                               Spécifie que la sortie doit être au format XML ou texte. Si \<format> est XML, la sortie est affichée au format XML. Si \<format> est text, la sortie s’affiche sans balises XML. La valeur par défaut est Text.                                                                                                                                                                |
|   /e :\<activé>    |                                                                                                                                                                                                                                         Active ou désactive un journal. \<Activé> peut avoir la valeur true ou false.                                                                                                                                                                                                                                          |
|  /i :\<> d’isolation   | Définit le mode d’isolation des journaux. \<Les> d’isolation peuvent être système, application ou personnalisé. Le mode d’isolation d’un journal détermine si un journal partage une session avec d’autres journaux dans la même classe d’isolation. Si vous spécifiez l’isolement système, le journal cible partagera au moins les autorisations d’écriture avec le journal système. Si vous spécifiez l’isolation des applications, le journal cible partagera au moins les autorisations d’écriture avec le journal des applications. Si vous spécifiez l’isolation personnalisée, vous devez également fournir un descripteur de sécurité à l’aide de l’option **/ca** . |
|  /LFN :\<LogPath>   |                                                                                                                                                                                                           Définit le nom du fichier journal. \<LogPath> est un chemin d’accès complet au fichier dans lequel le service journal des événements stocke les événements de ce journal.                                                                                                                                                                                                           |
|  /RT :\<rétention>  |                                                            Définit le mode de rétention du journal. \<> de rétention peut avoir la valeur true ou false. Le mode de rétention du journal détermine le comportement du service journal des événements lorsqu’un journal atteint sa taille maximale. Si un journal des événements atteint sa taille maximale et que le mode de rétention du journal a la valeur true, les événements existants sont conservés et les événements entrants sont ignorés. Si le mode de rétention du journal est false, les événements entrants remplacent les événements les plus anciens dans le journal.                                                             |
|    /AB :\<auto>     |                                                                                                                                   Spécifie la stratégie de sauvegarde automatique des journaux. \<La> automatique peut avoir la valeur true ou false. Si cette valeur est true, le journal est sauvegardé automatiquement lorsqu’il atteint la taille maximale. Si cette valeur est true, la rétention (spécifiée avec l’option **/RT** ) doit également avoir la valeur true.                                                                                                                                    |
|   /ms. :\<MaxSize>   |                                                                                                                                                                        Définit la taille maximale du journal, en octets. La taille minimale du journal est de 1048576 octets (1 024 Ko) et les fichiers journaux sont toujours des multiples de 64 Ko, donc la valeur que vous entrez sera arrondie en conséquence.                                                                                                                                                                         |
|    /l :\<niveau>     |                                                                                                                                                                     Définit le filtre de niveau du journal. \<Le niveau> peut être n’importe quelle valeur de niveau valide. Cette option s’applique uniquement aux journaux avec une session dédiée. Vous pouvez supprimer un filtre de niveau en <Level> affectant à la valeur 0.                                                                                                                                                                      |
|   /k :\<mots clés>   |                                                                                                                                                                                         Spécifie le filtre de mots clés du journal. \<Les mots clés> peuvent être n’importe quel masque de mot clé 64 bits valide. Cette option s’applique uniquement aux journaux avec une session dédiée.                                                                                                                                                                                         |
|   /ca :\<canal>   |                                                                                                                   Définit l’autorisation d’accès pour un journal des événements. \<Le> de canal est un descripteur de sécurité qui utilise le langage SDDL (Security Descriptor Definition Language). Pour plus d’informations sur le format SDDL, consultez le site Web Microsoft Developers Network ([https://msdn.microsoft.com](https://msdn.microsoft.com)MSDN) ().                                                                                                                    |
|    /c :\<config>    |                                                                                                                                  Spécifie le chemin d’accès à un fichier de configuration. Cette option entraîne la lecture des propriétés du journal à partir du fichier de configuration \<défini dans le> de configuration. Si vous utilisez cette option, vous ne devez pas spécifier <Logname> de paramètre. Le nom du journal sera lu à partir du fichier de configuration.                                                                                                                                   |
|  /GE :\<métadonnées>   |                                                                                                                                                                                                                 Obtient des informations de métadonnées pour les événements qui peuvent être déclenchés par ce serveur de publication. \<Les métadonnées> peuvent avoir la valeur true ou false.                                                                                                                                                                                                                 |
|   /GM :\<message>   |                                                                                                                                                                                                                       Affiche le message réel à la place de l’ID de message numérique. \<Le> de message peut avoir la valeur true ou false.                                                                                                                                                                                                                        |
|   /LF :\<logfile>   |                                                                                                                                                                                  Spécifie que les événements doivent être lus à partir d’un journal ou d’un fichier journal. \<Logfile> peut avoir la valeur true ou false. Si la valeur est true, le paramètre de la commande est le chemin d’accès à un fichier journal.                                                                                                                                                                                   |
| /sq :\<Structquery> |                                                                                                                                                                                Spécifie que les événements doivent être obtenus à l’aide d’une requête structurée. \<Structquery> peut avoir la valeur true ou false. Si la valeur <Path> est true, est le chemin d’accès à un fichier qui contient une requête structurée.                                                                                                                                                                                |
|    /q :\<> de requête     |                                                                                                                                                                     Définit la requête XPath pour filtrer les événements qui sont lus ou exportés. Si cette option n’est pas spécifiée, tous les événements sont retournés ou exportés. Cette option n’est pas disponible quand **/sq** a la valeur true.                                                                                                                                                                     |
|  /BM :\<signet>   |                                                                                                                                                                                                                                 Spécifie le chemin d’accès à un fichier qui contient un signet d’une requête précédente.                                                                                                                                                                                                                                 |
|   /SBM :\<Savebm>   |                                                                                                                                                                                                             Spécifie le chemin d’accès à un fichier qui est utilisé pour enregistrer un signet de cette requête. L’extension de nom de fichier doit être. Xml.                                                                                                                                                                                                              |
|  /RD :\<direction>  |                                                                                                                                                                                                   Spécifie la direction dans laquelle les événements sont lus. \<La direction> peut avoir la valeur true ou false. Si la valeur est true, les événements les plus récents sont retournés en premier.                                                                                                                                                                                                   |
|    /l :\<paramètres régionaux>    |                                                                                                                                                                                          Définit une chaîne de paramètres régionaux utilisée pour imprimer le texte de l’événement dans des paramètres régionaux spécifiques. Disponible uniquement lors de l’impression d’événements au format texte à l’aide de l’option **/f** .                                                                                                                                                                                          |
|    /c :\<nombre>     |                                                                                                                                                                                                                                                  Définit le nombre maximal d’événements à lire.                                                                                                                                                                                                                                                  |
|   /e :\<élément>    |                                                                                                                                                           Comprend un élément racine lors de l’affichage d’événements au format XML. \<L’élément> est la chaîne que vous souhaitez dans l’élément racine. Par exemple, l’élément **/e : root** génère du code XML qui contient le>\< </root>racine de la paire d’éléments racines.                                                                                                                                                            |
|  /ow :\<remplacer>  |                                                                                                                                                                 Spécifie que le fichier d’exportation doit être remplacé. \<Le remplacement de> peut avoir la valeur true ou false. Si la valeur est true et que le fichier <Exportfile> d’exportation spécifié dans existe déjà, il sera remplacé sans confirmation.                                                                                                                                                                 |
|   /BU :\<sauvegarde>    |                                                                                                                                                                                                      Spécifie le chemin d’accès à un fichier dans lequel les événements effacés seront stockés. Incluez l’extension. evtx dans le nom du fichier de sauvegarde.                                                                                                                                                                                                       |
|    /r :\<> à distance    |                                                                                                                                                                                            Exécute la commande sur un ordinateur distant. \<Remote> est le nom de l’ordinateur distant. Les paramètres **im** et **um** ne prennent pas en charge l’opération distante.                                                                                                                                                                                            |
|   /u :\<nom d’utilisateur>   |                                                                                                                                                                          Spécifie un autre utilisateur pour se connecter à un ordinateur distant. \<Username> est un nom d’utilisateur au format domaine\utilisateur ou User. Cette option s’applique uniquement lorsque l’option **/r** est spécifiée.                                                                                                                                                                          |
|   /p :\<> de mot de passe   |                                                                                                                                               Spécifie le mot de passe de l’utilisateur. Si l’option **/u** est utilisée et si cette option n’est pas \<spécifiée ou si le mot de passe> est *, l’utilisateur est invité à entrer un mot de passe. Cette option s’applique uniquement lorsque l' \* \*option/u* \* est spécifiée.                                                                                                                                                |
|     /a :\<authentification>     |                                                                                                                                                                                             Définit le type d’authentification pour la connexion à un ordinateur distant. \<Les> d’authentification peuvent être par défaut, négocier, Kerberos ou NTLM. La valeur par défaut est Negotiate.                                                                                                                                                                                              |
|  /Uni :\<Unicode>   |                                                                                                                                                                                                             Affiche la sortie au format Unicode. \<Les> Unicode peuvent avoir la valeur true ou false. Si <Unicode> a la valeur true, la sortie est au format Unicode.                                                                                                                                                                                                             |

## <a name="remarks"></a>Notes 

-   Utilisation d’un fichier de configuration avec le paramètre SL

    Le fichier de configuration est un fichier XML avec le même format que la sortie de wevtutil \<gl LogName>/f : XML. Pour afficher le format d’un fichier de configuration qui active la rétention, active la sauvegarde automatique et définit la taille maximale du journal dans le journal des applications :  
    ```
    <?xml version=1.0 encoding=UTF-8?>
    <channel name=Application isolation=Application
    xmlns=https://schemas.microsoft.com/win/2004/08/events>
    <logging>
    <retention>true</retention>
    <autoBackup>true</autoBackup>
    <maxSize>9000000</maxSize>
    </logging>
    <publishing>
    </publishing>
    </channel>
    ```

## <a name="examples"></a>Exemples

Répertorier les noms de tous les journaux :
```
wevtutil el
```
Affichez les informations de configuration relatives au journal système sur l’ordinateur local au format XML :
```
wevtutil gl System /f:xml
```
Utilisez un fichier de configuration pour définir les attributs du journal des événements (consultez la section Notes pour obtenir un exemple de fichier de configuration) :
```
wevtutil sl /c:config.xml
```
Affichez des informations sur l’éditeur d’événements Microsoft-Windows-EventLog, y compris les métadonnées relatives aux événements que le serveur de publication peut déclencher :
```
wevtutil gp Microsoft-Windows-Eventlog /ge:true
```
Installez les serveurs de publication et les journaux à partir du fichier manifeste myManifest. xml :
```
wevtutil im myManifest.xml
```
Désinstallez les serveurs de publication et les journaux du fichier manifeste myManifest. xml :
```
wevtutil um myManifest.xml
```
Affichez les trois événements les plus récents à partir du journal des applications au format textuel :
```
wevtutil qe Application /c:3 /rd:true /f:text
```
Affichez l’état du journal des applications :
```
wevtutil gli Application 
```
Exporter les événements du journal système vers C:\backup\system0506.evtx :
```
wevtutil epl System C:\backup\system0506.evtx
```
Effacez tous les événements du journal des applications après les avoir enregistrés dans C:\admin\backups\a10306.evtx :
```
wevtutil cl Application /bu:C:\admin\backups\a10306.evtx
```

#### <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
