---
title: ftp
description: Rubrique de référence pour * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 758335e1-fd8d-448c-a654-993126239dd9
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4878377225f9c58e40256a3d151d0d8f3761afca
ms.sourcegitcommit: fad2ba64bbc13763772e21ed3eabd010f6a5da34
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/09/2020
ms.locfileid: "82993338"
---
# <a name="ftp"></a>ftp

> S’applique à : Windows Server (canal semi-annuel), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Transfère les fichiers vers et à partir d’un ordinateur exécutant un service de serveur protocole FTP (FTP). **FTP** peut être utilisé de manière interactive ou en mode batch en traitant des fichiers texte ASCII.
## <a name="syntax"></a>Syntaxe
```
ftp [-v] [-d] [-i] [-n] [-g] [-s:<FileName>] [-a] [-A] [-x:<SendBuffer>] [-r:<RecvBuffer>] [-b:<AsyncBuffers>][-w:<WindowsSize>]  [-?] [<Host>]
```
#### <a name="parameters"></a>Paramètres

|     Paramètre     |                                                                                                                                                      Description                                                                                                                                                      |
|-------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|        -v         |                                                                                                                                    Supprime l’affichage des réponses du serveur distant.                                                                                                                                     |
|        -d         |                                                                                                               Active le débogage, en affichant toutes les commandes passées entre le client FTP et le serveur FTP.                                                                                                                |
|        -i         |                                                                                                                            Désactive les invites interactives pendant plusieurs transferts de fichiers.                                                                                                                             |
|        -n         |                                                                                                                                    Supprime la connexion automatique lors de la connexion initiale.                                                                                                                                     |
|        -g         |                                         Désactive le nom de fichier globbing.  **Glob** permet l’utilisation de l’astérisque (\*) et du point d’interrogation ( ?) comme caractères génériques dans les noms de fichier et de chemin d’accès locaux. Pour plus d’informations, consultez [Références supplémentaires](ftp.md#BKMK_additionalRef).                                          |
|   x<FileName>   | Spécifie un fichier texte qui contient des commandes **FTP** . Ces commandes s’exécutent automatiquement après le démarrage de **FTP** . Ce paramètre n’autorise aucun espace. Utilisez ce paramètre au lieu de redirection (**<**). **Remarque :** Dans les systèmes d’exploitation Windows 8 et Windows Server 2012 ou versions ultérieures, le fichier texte doit être écrit en UTF-8. |
|        -a         |                                                                                                                 Spécifie qu’une interface locale peut être utilisée lors de la liaison de la connexion de données FTP.                                                                                                                  |
|        -A         |                                                                                                                                        Ouvre une session sur le serveur FTP comme anonyme.                                                                                                                                         |
|  x<SendBuffer>  |                                                                                                                                     Remplace la taille de SO_SNDBUF par défaut de 8192.                                                                                                                                     |
|  r<RecvBuffer>  |                                                                                                                                     Remplace la taille de SO_RCVBUF par défaut de 8192.                                                                                                                                     |
| p<AsyncBuffers> |                                                                                                                                    Remplace le nombre de tampons Async par défaut de 3.                                                                                                                                     |
| s<WindowsSize>  |                                                                                                                   Spécifie la taille de la mémoire tampon de transfert. La taille de fenêtre par défaut est de 4096 octets.                                                                                                                   |
|        -?         |                                                                                                                                         Affiche l'aide à l'invite de commandes.                                                                                                                                          |
|      <host>       |                                                                    Spécifie le nom de l’ordinateur, l’adresse IP ou l’adresse IPv6 du serveur FTP auquel se connecter. Le nom d’hôte ou l’adresse, s’il est spécifié, doit être le dernier paramètre de la ligne.                                                                    |

## <a name="remarks"></a>Notes 
- Pour plus d’informations sur les commandes **FTP** sur Windows Server 2003, voir [FTP](https://technet.microsoft.com/library/cc756013(v=ws.10).aspx).
- les paramètres de ligne de commande **FTP** respectent la casse.
- Cette commande est disponible uniquement si le protocole **TCP/IP (Internet Protocol)** est installé en tant que composant dans les propriétés d’une carte réseau dans connexions réseau.
- **FTP** peut être utilisé de manière interactive. Une fois le démarrage effectué, **FTP** crée un sous-environnement dans lequel vous pouvez utiliser des commandes **FTP** . Vous pouvez revenir à l’invite de commandes en tapant la commande **Quit** . Lorsque le sous-environnement **FTP** est en cours d’exécution, il est indiqué par l’invite de commandes **FTP >** . Pour plus d’informations, consultez les commandes **FTP** .
- **FTP** prend en charge l’utilisation d’IPv6 lorsque le protocole IPv6 est installé. Pour plus d’informations, consultez [Références supplémentaires](ftp.md#BKMK_additionalRef).
  ## <a name="examples"></a>Exemples
  Pour ouvrir une session sur le serveur FTP nommé ftp.example.microsoft.com, tapez :
  ```
  ftp ftp.example.microsoft.com
  ```
  Pour ouvrir une session sur le serveur FTP nommé ftp.example.microsoft.com et exécuter les commandes **FTP** contenues dans un fichier nommé Resync. txt, tapez :
  ```
  ftp -s:resync.txt ftp.example.microsoft.com
  ```
  ## <a name="additional-references"></a><a name=BKMK_additionalRef></a>Références supplémentaires
- [IP version 6](https://technet.microsoft.com/library/cc738636(v=ws.10).aspx)
- [Applications IPv6](https://technet.microsoft.com/library/cc782509(v=ws.10).aspx)
- - [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
