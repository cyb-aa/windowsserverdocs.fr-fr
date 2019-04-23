---
title: ftp
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 758335e1-fd8d-448c-a654-993126239dd9
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 76b8a2ee7ce81a6db75e0d375d017746e4361df1
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59887380"
---
# <a name="ftp"></a>ftp

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Transfère les fichiers vers et depuis un ordinateur exécutant un service de serveur de File Transfer Protocol (ftp). **FTP** peut être utilisé interactivement ou en mode batch par le traitement des fichiers texte ASCII. 
## <a name="syntax"></a>Syntaxe
```
ftp [-v] [-d] [-i] [-n] [-g] [-s:<FileName>] [-a] [-A] [-x:<SendBuffer>] [-r:<RecvBuffer>] [-b:<AsyncBuffers>][-w:<WindowsSize>]  [-?] [<Host>]
```
### <a name="parameters"></a>Paramètres
|Paramètre|Description|
|-------|--------|
|-v|Supprime l’affichage des réponses du serveur distant.|
|-d|Active le débogage, affichant toutes les commandes transmises entre le client et le serveur FTP.|
|-i|Désactive les messages interactifs lors des transferts de fichiers multiples.|
|-n|Supprime la connexion automatique lors de la connexion initiale.|
|-g|Désactive la globalisation de nom de fichier.  **Glob** permet l’utilisation de l’astérisque (*) et le point d’interrogation ( ?) comme caractères génériques dans les noms de fichier et chemin d’accès locales. Pour plus d’informations, consultez [références supplémentaires](ftp.md#BKMK_additionalRef).|
|-s:.<FileName>|Spécifie un fichier texte qui contient **ftp** commandes. Ces commandes s’exécutent automatiquement après **ftp** démarre. Ce paramètre ne permet aucuns espaces. Utilisez ce paramètre au lieu de la redirection (**<**). **Remarque :** Dans Windows 8 et Windows Server 2012 ou versions ultérieures, le fichier texte doit être écrit en UTF-8.|
|-a|Spécifie que n’importe quelle interface locale peut être utilisée lors de la liaison de la connexion de données ftp.|
|-A|Ouvre une session sur le serveur ftp anonyme.|
|-x:<SendBuffer>|Remplace la taille SO_SNDBUF par défaut de 8 192.|
|-r.<RecvBuffer>|Remplace la taille SO_RCVBUF de valeur par défaut de 8 192.|
|-b:<AsyncBuffers>|Remplace le nombre de tampons async par défaut de 3.|
|-w:<WindowsSize>|Spécifie la taille de la mémoire tampon de transfert. La taille de fenêtre par défaut est 4 096 octets.|
|-?|Affiche l'aide à l'invite de commandes.|
|<host>|Spécifie le nom de l’ordinateur, une adresse IP ou une adresse IPv6 du serveur ftp à laquelle se connecter. Le nom d’hôte ou l’adresse, si spécifié, doit être le dernier paramètre sur la ligne.|
## <a name="remarks"></a>Notes
-   Pour plus d’informations sur **ftp** commandes sur Windows Server 2003, consultez [ftp](https://technet.microsoft.com/library/cc756013(v=ws.10).aspx).
-   **FTP** des paramètres de ligne de commande respectent la casse.
-   Cette commande est disponible uniquement si le **Internet Protocol (TCP/IP)** protocole est installé en tant que composant dans les propriétés d’une carte réseau dans Connexions réseau.
-   **FTP** peut être utilisé de manière interactive. Après son démarrage, **ftp** crée un environnement de sous-système dans lequel vous pouvez utiliser **ftp** commandes. Vous pouvez revenir à l’invite de commandes en tapant la **quitter** commande. Lorsque le **ftp** environnement secondaire est en cours d’exécution, il est indiqué par le **ftp >** invite de commandes. Pour plus d’informations, consultez le **ftp** commandes.
-   **FTP** prend en charge l’utilisation du protocole IPv6 lorsque le protocole IPv6 est installé. Pour plus d’informations, consultez [références supplémentaires](ftp.md#BKMK_additionalRef).
## <a name="BKMK_Examples"></a>Exemples
Pour vous connecter au serveur ftp nommé ftp.example.microsoft.com, tapez :
```
ftp ftp.example.microsoft.com
```
Ouvrir une session sur le serveur ftp nommé ftp.example.microsoft.com et exécuter le **ftp** commandes contenues dans un fichier nommé resync.txt, type :
```
ftp -s:resync.txt ftp.example.microsoft.com
```
## <a name="BKMK_additionalRef"></a>Références supplémentaires
-   [IP version 6](https://technet.microsoft.com/library/cc738636(v=ws.10).aspx)
-   [Applications IPv6](https://technet.microsoft.com/library/cc782509(v=ws.10).aspx)
-   [Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)
