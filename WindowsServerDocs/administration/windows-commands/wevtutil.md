---
title: wevtutil
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: d4c791e0-7e59-45c5-aa55-0223b77a4822 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e6d57f95379fce80bec9cb5e8445b28f887123c8
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59826730"
---
# <a name="wevtutil"></a>wevtutil



Permet de récupérer des informations sur les journaux des événements et les serveurs de publication. Vous pouvez également utiliser cette commande pour installer et désinstaller les fichiers manifeste des événements, pour exécuter des requêtes et pour exporter, archiver et vider les journaux. Pour obtenir des exemples d’utilisation de cette commande, consultez [Exemples](#BKMK_examples).

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

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|{el \| enum-logs}|Affiche les noms de tous les journaux.|
|{gl \| get-log} \<Logname > [/ f:\<Format >]|Affiche des informations de configuration pour le journal spécifié, qui inclut une valeur indiquant si le journal est activé ou non, la limite de taille maximale actuelle du journal et le chemin d’accès au fichier où est stocké le journal.|
|{sl \| set-log} \<Logname > [/ e:\<activé >] [/ i :\<Isolation >] [/ noms de fichiers longs :\<Logpath >] [/ rt :\<rétention >] [/ ab :\<automatique >] [/ ms :\< MaxSize >] [/ l:\<niveau >] [/ k :\<mots clés >] [/ autorité de certification :\<canal >] [/ c:\<Config >]|Modifie la configuration du journal spécifié.|
|{ep \| enum-publishers}|Affiche les éditeurs d’événements sur l’ordinateur local.|
|{gp \| get-publisher} \<Publishername > [/ ge :\<métadonnées >] [/ gm :\<Message >] [/ f:\<Format >]]|Affiche les informations de configuration pour l’éditeur d’événements spécifié.|
|{im \| install-manifest} \<manifeste >|Installe les journaux et éditeurs d’événements à partir d’un manifeste. Pour plus d’informations sur les manifestes d’événements et à l’aide de ce paramètre, consultez le Kit de développement du journal des événements Windows sur le site Web de Microsoft Developers Network (MSDN) (https://msdn.microsoft.com).|
|{um \| manifeste désinstaller} \<manifeste >|Désinstalle tous les journaux et éditeurs d’un manifeste. Pour plus d’informations sur les manifestes d’événements et à l’aide de ce paramètre, consultez le Kit de développement du journal des événements Windows sur le site Web de Microsoft Developers Network (MSDN) (https://msdn.microsoft.com).|
|{qe \| événements de requête} \<chemin > [/ lf :\<Logfile >] [/ sq :\<Structquery >] [/ q :\<requête >] [/ bm :\<signet >] [/ sbm :\<Savebm >] [/ Bureau à distance :\< Direction >] [/ f:\<Format >] [/ l:\<paramètres régionaux >] [/ c:\<nombre >] [/ e:\<élément >]|Lit les événements à partir d’un journal des événements à partir d’un fichier journal, ou à l’aide d’une requête structurée. Par défaut, vous fournissez un nom de journal pour \<chemin d’accès >. Toutefois, si vous utilisez le **/lf** option, puis \<chemin d’accès > doit être un chemin d’accès dans un fichier journal. Si vous utilisez le **/sq** paramètre, \<chemin d’accès > doit être un chemin d’accès à un fichier qui contient une requête structurée.|
|{gli \| get-loginfo} \<Logname > [/ lf :\<Logfile >]|Affiche les informations d’état sur un journal des événements ou le fichier journal. Si le **/lf** option est utilisée, \<Logname > est un chemin d’accès dans un fichier journal. Vous pouvez exécuter **wevtutil el** pour obtenir une liste de noms de journaux.|
|{epl \| -journal d’exportation} \<chemin d’accès > \<Exportfile > [/ lf :\<Logfile >] [/ sq :\<Structquery >] [/ q :\<requête >] [/ oriser :\<remplacer >]|Exporte des événements à partir d’un journal des événements à partir d’un fichier journal, ou à l’aide d’une requête structurée dans le fichier spécifié. Par défaut, vous fournissez un nom de journal pour \<chemin d’accès >. Toutefois, si vous utilisez le **/lf** option, puis \<chemin d’accès > doit être un chemin d’accès dans un fichier journal. Si vous utilisez le **/sq** option, \<chemin d’accès > doit être un chemin d’accès à un fichier qui contient une requête structurée. \<Exportfile > est un chemin d’accès au fichier où les événements exportés seront stockés.|
|{al \| archivelog} \<Logpath > [/ l:\<paramètres régionaux >]|Archive le fichier journal spécifié dans un format autonome. Un sous-répertoire avec le nom des paramètres régionaux est créé et toutes les informations spécifiques de paramètres régionaux sont enregistrées dans ce sous-répertoire. Une fois que le répertoire et le fichier journal sont créées en exécutant **wevtutil al**, événements dans le fichier peuvent être lu si le serveur de publication est installé ou non.|
|{cl \| clear-log} \<Logname > [/ bu :\<sauvegarde >]|Efface les événements du journal des événements spécifié. Le **/BU** option peut être utilisée pour sauvegarder les événements effacés.|

## <a name="options"></a>Options

|Option|Description|
|------|-----------|
|/f :\<format >|Spécifie que la sortie doit être au format XML ou texte. Si \<Format > est XML, la sortie s’affiche au format XML. Si \<Format > est le texte, la sortie s’affiche sans les balises XML. La valeur par défaut est texte.|
|/ e:\<activé >|Active ou désactive un journal. \<Activé > peut être true ou false.|
|/ i:\<isolation >|Définit le mode d’isolation de journal. \<Isolation > peut être système, application ou personnalisé. Le mode d’isolation d’un journal détermine si un journal partage une session avec les autres journaux dans la même classe d’isolation. Si vous spécifiez isolation des systèmes, le journal cible partagent au moins les autorisations avec le journal système d’écriture. Si vous spécifiez d’isolation des applications, le journal cible partagent au moins les autorisations avec le journal des applications d’écriture. Si vous spécifiez isolation personnalisée, vous devez également fournir un descripteur de sécurité à l’aide de la **/ca** option.|
|/LFN :\<Logpath >|Définit le nom du fichier journal. \<LogPath > est un chemin d’accès complet au fichier dans lequel le service journal des événements stocke les événements pour ce journal.|
|/RT :\<rétention >|Définit le mode de rétention de journal. \<Rétention > peut être true ou false. Le mode de rétention du journal détermine le comportement du service journal des événements lorsqu’un journal atteint sa taille maximale. Si un journal des événements atteint sa taille maximale et le mode de rétention de journal est true, les événements existants sont conservés et les événements entrants sont ignorées. Si le mode de rétention de journal est false, les événements entrants remplacent les événements les plus anciens dans le journal.|
|/AB :\<automatique >|Spécifie la stratégie de sauvegarde automatique du journal. \<Automatique > peut être true ou false. Si cette valeur est true, le journal est être sauvegardé automatiquement lorsqu’elle atteint la taille maximale. Si cette valeur est true, la rétention (spécifiée avec le **/rt** option) doit également être défini sur true.|
|/ ms :\<MaxSize >|Définit la taille maximale du journal en octets. La taille minimale du journal est 1 048 576 octets (1 024 Ko) et les fichiers journaux sont toujours des multiples de 64 Ko, donc la valeur que vous entrez sera arrondie en conséquence.|
|/ l:\<niveau >|Définit le filtre au niveau du journal. \<Niveau > peut être toute valeur de niveau valide. Cette option s’applique uniquement aux journaux à une session dédiée. Vous pouvez supprimer un filtre au niveau en définissant <Level> sur 0.|
|/ k:\<mots clés >|Spécifie le filtre de mots clés du journal. \<Mots clés > peut être n’importe quel masque de mot clé de 64 bits valide. Cette option s’applique uniquement aux journaux à une session dédiée.|
|/CA :\<canal >|Définit l’autorisation d’accès pour un journal des événements. \<Canal > est un descripteur de sécurité qui utilise la définition de langage SDDL (Security Descriptor). Pour plus d’informations sur le format SDDL, consultez le site Web de Microsoft Developers Network (MSDN) (https://msdn.microsoft.com).|
|/ c:\<Config >|Spécifie le chemin d’accès à un fichier de configuration. Cette option fait que les propriétés du journal à lire à partir du fichier de configuration défini dans \<Config >. Si vous utilisez cette option, vous ne devez pas spécifier un <Logname> paramètre. Le nom du journal sera lue dans le fichier de configuration.|
|/GE :\<métadonnées >|Obtient les informations de métadonnées pour les événements qui peuvent être déclenchés par ce serveur de publication. \<Métadonnées > peut être true ou false.|
|/GM :\<message >|Affiche le message réel au lieu de l’ID du message numérique. \<Message > peut être true ou false.|
|/lf:\<Logfile>|Spécifie que les événements doivent être lues à partir d’un journal ou d’un fichier journal. \<Fichier journal > peut être true ou false. Si la valeur est true, le paramètre à la commande est le chemin d’accès dans un fichier journal.|
|/ sq :\<Structquery >|Spécifie que les événements doivent être obtenus avec une requête structurée. \<Structquery > peut être true ou false. Si la valeur est true, <Path> est le chemin d’accès à un fichier qui contient une requête structurée.|
|/q :\<requête >|Définit la requête XPath pour filtrer les événements qui sont lues ou exportés. Si cette option n’est pas spécifiée, tous les événements seront retournés ou exportées. Cette option n’est pas disponible lorsque **/sq** a la valeur true.|
|/BM :\<signet >|Spécifie le chemin d’accès à un fichier qui contient un signet à partir d’une requête précédente.|
|/SBM :\<Savebm >|Spécifie le chemin d’accès à un fichier qui est utilisé pour enregistrer un signet de cette requête. L’extension de nom de fichier doit être .xml.|
|/RD :\<direction >|Spécifie la direction dans laquelle les événements sont lus. \<Direction > peut être true ou false. Si la valeur est true, les événements les plus récents sont renvoyées en premier.|
|/ l:\<paramètres régionaux >|Définit une chaîne de paramètres régionaux qui est utilisée pour imprimer le texte de l’événement dans des paramètres régionaux spécifiques. Disponible uniquement lors de l’impression des événements dans le format de texte à l’aide du **/f** option.|
|/ c:\<nombre >|Définit le nombre maximal d’événements à lire.|
|/ e:\<élément >|Inclut un élément racine lors de l’affichage des événements au format XML. \<Élément > est la chaîne que vous souhaitez au sein de l’élément racine. Par exemple, **/e:root** produit un contenu XML qui contient la paire d’élément racine \<racine ></root>.|
|/ oriser :\<remplacer >|Spécifie que le fichier d’exportation doit être remplacé. \<Remplacer > peut être true ou false. Si la valeur true et le fichier d’exportation spécifiés dans <Exportfile> existe déjà, elle sera remplacée sans confirmation.|
|/ bu :\<sauvegarde >|Spécifie le chemin d’accès à un fichier où les événements effacés seront stockés. Inclure l’extension .evtx, le nom du fichier de sauvegarde.|
|/ r:\<distant >|Exécute la commande sur un ordinateur distant. \<À distance > est le nom de l’ordinateur distant. Le **im** et **um** paramètres ne prennent pas en charge les opération distante.|
|/ u:\<nom d’utilisateur >|Spécifie un autre utilisateur pour vous connecter à un ordinateur distant. \<Nom d’utilisateur > est un nom d’utilisateur dans le format domaine\utilisateur ou l’utilisateur. Cette option est applicable uniquement lorsque le **/r** option est spécifiée.|
|/ p:\<mot de passe >|Spécifie le mot de passe pour l’utilisateur. Si le **/u** option est utilisée et que cette option n’est pas spécifiée ou \<mot de passe > est « * », l’utilisateur sera invité à entrer un mot de passe. Cette option est applicable uniquement lorsque le **/u** option est spécifiée.|
|/ a:\<Auth >|Définit le type d’authentification pour la connexion à un ordinateur distant. \<Authentification > peut être par défaut, Negotiate, Kerberos ou NTLM. La valeur par défaut est négocier.|
|/Uni :\<Unicode >|Affiche la sortie au format Unicode. \<Unicode > peut être true ou false. Si <Unicode> a la valeur true, la sortie est au format Unicode.|

## <a name="remarks"></a>Notes

-   À l’aide d’un fichier de configuration avec le paramètre sl

    Le fichier de configuration est un fichier XML avec le même format que la sortie de wevtutil gl \<Logname > /f:xml. L’exemple suivant montre le format de fichier de configuration qui permet la rétention, permet la sauvegarde automatique et définit la taille maximale du journal sur le journal des applications :  
    ```
    <?xml version="1.0" encoding="UTF-8"?>
    <channel name="Application" isolation="Application"
    xmlns="https://schemas.microsoft.com/win/2004/08/events">
    <logging>
    <retention>true</retention>
    <autoBackup>true</autoBackup>
    <maxSize>9000000</maxSize>
    </logging>
    <publishing>
    </publishing>
    </channel>
    ```

## <a name="BKMK_examples"></a>Exemples

Répertorier les noms de tous les journaux :
```
wevtutil el
```
Afficher les informations de configuration sur le journal système sur l’ordinateur local au format XML :
```
wevtutil gl System /f:xml
```
Utiliser un fichier de configuration pour définir les attributs de journal des événements (consultez la section Notes pour obtenir un exemple de fichier de configuration) :
```
wevtutil sl /c:config.xml
```
Afficher des informations sur l’éditeur d’événements Microsoft-Windows-Eventlog, notamment les métadonnées sur les événements que le serveur de publication peut déclencher :
```
wevtutil gp Microsoft-Windows-Eventlog /ge:true
```
Installer les serveurs de publication et de journaux à partir du fichier manifeste monManifeste.XML :
```
wevtutil im myManifest.xml
```
Désinstaller les journaux et éditeurs à partir du fichier manifeste monManifeste.XML :
```
wevtutil um myManifest.xml
```
Afficher les trois derniers événements dans le journal des applications dans le format texte :
```
wevtutil qe Application /c:3 /rd:true /f:text
```
Afficher l’état du journal d’Application :
```
wevtutil gli Application 
```
Exporter les événements de journal système vers C:\backup\system0506.evtx :
```
wevtutil epl System C:\backup\system0506.evtx
```
Effacer tous les événements dans le journal des applications après les avoir enregistrées à C:\admin\backups\a10306.evtx :
```
wevtutil cl Application /bu:C:\admin\backups\a10306.evtx
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)
