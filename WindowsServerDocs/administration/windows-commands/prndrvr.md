---
title: prndrvr
description: Rubrique relative aux commandes Windows pour * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 82b09e3e-bd38-4df1-9953-b0e9ee2565a3
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: eebb28ae50f4546ac5ab3d495994c96e293f6928
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80837302"
---
# <a name="prndrvr"></a>prndrvr

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Utilisez la commande **prndrvr** pour ajouter, supprimer et répertorier les pilotes d’imprimante.

## <a name="syntax"></a>Syntaxe
```
cscript prndrvr {-a | -d | -l | -x | -?} [-m <model>] [-v {0|1|2|3}] 
[-e <environment>] [-s <ServerName>] [-u <UserName>] [-w <Password>] 
[-h <path>] [-i <inf file>]
```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|-------|--------|
|-a|Installe un pilote.|
|-d|supprime un pilote.|
|-l|répertorie tous les pilotes d’imprimante installés sur le serveur spécifié par le paramètre **-s** . Si vous ne spécifiez pas de serveur, Windows répertorie les pilotes d’imprimante installés sur l’ordinateur local.|
|-x|supprime tous les pilotes d’imprimante et les pilotes d’imprimante supplémentaires qui ne sont pas utilisés par une imprimante logique sur le serveur spécifié par le paramètre **-s** . Si vous ne spécifiez pas de serveur à supprimer de la liste, Windows supprime tous les pilotes d’imprimante inutilisés sur l’ordinateur local.|
|-m \<DrivermodelName\>|Spécifie (par nom) le pilote que vous souhaitez installer. Les pilotes sont souvent nommés pour le modèle d’imprimante pris en charge. Pour plus d’informations, consultez la documentation de l’imprimante.|
|-v {0 &#124; 1 &#124; 2 &#124; 3}|Spécifie la version du pilote que vous voulez installer. Pour plus d’informations sur les versions disponibles pour l’environnement, consultez la description du paramètre **-e**. Si vous ne spécifiez pas de version, la version du pilote correspondant à la version de Windows en cours d’exécution sur l’ordinateur sur lequel vous installez le pilote est installée.<p>-la version **0** prend en charge Windows 95, Windows 98 et Windows Millennium Edition.<br />-la version **1** prend en charge Windows NT 3,51.<br />-la version **2** prend en charge Windows NT 4,0.<br />-la version **3** prend en charge les systèmes d’exploitation Windows Vista, Windows XP, Windows 2000 et windows Server 2003. Notez qu’il s’agit de la seule version du pilote d’imprimante prise en charge par Windows Vista.|
|-e \<> de l’environnement|Spécifie l’environnement pour le pilote que vous souhaitez installer. Si vous ne spécifiez pas d’environnement, l’environnement de l’ordinateur sur lequel vous installez le pilote est utilisé. Les paramètres d’environnement pris en charge sont les suivants :<p>-   **Windows NT x86**<br />-   **Windows x64**<br />-   **Windows IA64**|
|-s \<ServerName >|Spécifie le nom de l’ordinateur distant qui héberge l’imprimante que vous souhaitez gérer. Si vous ne spécifiez pas d’ordinateur, l’ordinateur local est utilisé.|
|-u \<nom_utilisateur >-w \<mot de passe >|Spécifie un compte disposant des autorisations nécessaires pour se connecter à l’ordinateur qui héberge l’imprimante que vous souhaitez gérer. Tous les membres du groupe Administrateurs local de l’ordinateur cible disposent de ces autorisations, mais les autorisations peuvent également être accordées à d’autres utilisateurs. Si vous ne spécifiez pas de compte, vous devez avoir ouvert une session sous un compte disposant de ces autorisations pour que la commande fonctionne.|
|-h \<chemin d’accès >|Spécifie le chemin d’accès au fichier de pilote. Si vous ne spécifiez pas de chemin d’accès, le chemin d’accès à l’emplacement d’installation de Windows est utilisé.|
|-i \<nom_fichier. inf >|Spécifie le chemin d’accès complet et le nom de fichier pour le pilote que vous souhaitez installer. Si vous ne spécifiez pas de nom de fichier, le script utilise l’un des fichiers Printer. inf de la boîte de réception dans le sous-répertoire INF du répertoire Windows.<p>Si le chemin d’accès du pilote n’est pas spécifié, le script recherche des fichiers de pilote dans le fichier. cab du pilote.|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="remarks"></a>Notes
- La commande **prndrvr** est un script Visual Basic situé dans le répertoire%windir%\system32\ printing_Admin_Scripts\\<language>. Pour utiliser cette commande, à l’invite de commandes, tapez **cscript** suivi du chemin d’accès complet au fichier prndrvr ou accédez au dossier approprié.

  Par exemple :
  ```
  cscript %WINdir%\System32\printing_Admin_Scripts\en-US\prndrvr
  ```
- Si les informations que vous fournissez contiennent des espaces, utilisez des guillemets autour du texte (par exemple, `computer Name`).
- L’option-x supprime tous les pilotes d’imprimante supplémentaires (pilotes installés pour une utilisation sur les clients exécutant d’autres versions de Windows), même si le pilote principal est en cours d’utilisation. Si le composant Télécopie est installé, cette option supprime également les pilotes de télécopie. Le pilote de télécopie principal est supprimé s’il n’est pas en cours d’utilisation (autrement dit, s’il n’y a aucune file d’attente qui l’utilise). Si le pilote de télécopie principal est supprimé, la seule façon de réactiver la télécopie consiste à réinstaller le composant de télécopie.
- Utilisé sans paramètres, **prndrvr** affiche l’aide de la ligne de commande pour la commande **prndrvr** .

## <a name="examples"></a><a name=BKMK_examples></a>Illustre

Pour répertorier tous les pilotes sur le serveur \\\printServer1, tapez :
```
cscript Prndrvr -l -s
```

Pour ajouter un pilote d’imprimante Windows x64 version 3 pour le modèle d’imprimante laser modèle 1 de l’imprimante à l’aide du fichier d’informations du pilote C:\temp\Laserprinter1.inf pour un pilote stocké dans le type de dossier C:\temp :
```
cscript Prndrvr -a -m Laser printer model 1 -v 3 -e Windows x64 -i c:\temp\Laserprinter1.inf -h c:\temp
```

Pour supprimer un pilote d’imprimante Windows NT x86 version 3 pour l’imprimante laser modèle 1, tapez :
```
cscript Prndrvr -a -m Laser printer model 1 -v 3 -e Windows NT x86 
```

## <a name="additional-references"></a>Références supplémentaires
- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
[référence de commande d’impression](print-command-reference.md)
