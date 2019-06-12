---
title: prndrvr
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 82b09e3e-bd38-4df1-9953-b0e9ee2565a3
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: edf27852c13566ba8c7ca8c16d789d586e749160
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66436213"
---
# <a name="prndrvr"></a>prndrvr

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Utilisez le **prndrvr** commande pour ajouter, supprimer et répertorier des pilotes d’imprimante.

## <a name="syntax"></a>Syntaxe
```
cscript prndrvr {-a | -d | -l | -x | -?} [-m <model>] [-v {0|1|2|3}] 
[-e <environment>] [-s <ServerName>] [-u <UserName>] [-w <Password>] 
[-h <path>] [-i <inf file>]
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|-------|--------|
|-a|Installe un pilote.|
|-d|Supprime un pilote.|
|-l|Répertorie tous les pilotes d’imprimante installés sur le serveur spécifié par le **-s** paramètre. Si vous ne spécifiez pas un serveur, Windows répertorie les pilotes d’imprimante installés sur l’ordinateur local.|
|-x|Supprime tous les pilotes d’imprimante et pilotes d’imprimante supplémentaires pas en cours d’utilisation par une imprimante logique sur le serveur spécifié par le **-s** paramètre. Si vous ne spécifiez pas un serveur à supprimer dans la liste, Windows supprime tous les pilotes d’imprimante non utilisés sur l’ordinateur local.|
|-m \<DrivermodelName\>|Spécifie le pilote que vous souhaitez installer (par nom). Pilotes sont souvent nommées pour le modèle d’imprimante que prises en charge. Consultez la documentation de l’imprimante pour plus d’informations.|
|-v {0 &#124; 1 &#124; 2 &#124; 3}|Spécifie la version du pilote que vous souhaitez installer. Consultez la description de la **-e**paramètre pour plus d’informations sur lequel les versions sont disponibles pour connaître l’environnement. Si vous ne spécifiez pas une version, la version du pilote approprié pour la version de Windows en cours d’exécution sur l’ordinateur où vous installez le pilote est installée.<br /><br />-version **0** prend en charge Windows 95, Windows 98 et Windows Millennium edition.<br />-version **1** prend en charge de Windows NT 3.51.<br />-version **2** prend en charge de Windows NT 4.0.<br />-version **3** prend en charge de Windows Vista, Windows XP, Windows 2000 et les systèmes d’exploitation Windows Server 2003. Notez qu’il s’agit de la seule version de pilote d’imprimante qui prend en charge de Windows Vista.|
|e - \<environnement >|Spécifie l’environnement pour le pilote que vous souhaitez installer. Si vous ne spécifiez pas un environnement, l’environnement de l’ordinateur sur lequel vous installez le pilote est utilisé. Les paramètres d’environnement pris en charge sont :<br /><br />-    **« Windows NT x86 »**<br />-    **« Windows x64 »**<br />-    **« IA64 Windows »**|
|s - \<nom_serveur >|Spécifie le nom de l’ordinateur distant qui héberge l’imprimante que vous souhaitez gérer. Si vous ne spécifiez pas un ordinateur, l’ordinateur local est utilisé.|
|-u \<nom d’utilisateur > -w \<mot de passe >|Spécifie un compte disposant d’autorisations pour se connecter à l’ordinateur qui héberge l’imprimante que vous souhaitez gérer. Tous les membres du groupe Administrateurs local de l’ordinateur cible disposent de ces autorisations, mais les autorisations peuvent également être accordées aux autres utilisateurs. Si vous ne spécifiez pas un compte, vous devez être connecté sous un compte disposant de ces autorisations pour la commande fonctionne.|
|-h \<path>|Spécifie le chemin d’accès au fichier du pilote. Si vous ne spécifiez pas un chemin d’accès, le chemin d’accès à l’emplacement d’installation de Windows est utilisé.|
|-i \<Filename.inf>|Spécifie le chemin d’accès et le nom complet pour le pilote que vous souhaitez installer. Si vous ne spécifiez pas un nom de fichier, le script utilise l’un des fichiers .inf imprimante boîte de réception dans le sous-répertoire du répertoire Windows.<br /><br />Si le chemin d’accès du pilote n’est pas spécifié, le script recherche les fichiers de pilote dans le fichier driver.cab.|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="remarks"></a>Notes
- Le **prndrvr** commande est un script Visual Basic situé dans le %WINdir%\System32\printing_Admin_Scripts\\ <language> directory. Pour utiliser cette commande, à une invite de commandes, tapez **cscript** suivie du chemin d’accès complet au fichier de prndrvr, ou modifiez les répertoires vers le dossier approprié.

  Exemple :
  ```
  cscript %WINdir%\System32\printing_Admin_Scripts\en-US\prndrvr
  ```
- Si les informations que vous fournissez contient des espaces, utilisez des guillemets autour du texte (par exemple, `"computer Name"`).
- Le x - option supprime tous les pilotes d’imprimante supplémentaires (pilotes installés pour une utilisation sur les clients en cours d’exécution autres versions de Windows), même si le pilote principal est en cours d’utilisation. Si le composant de télécopie est installé, cette option supprime également les pilotes de télécopieur. Le pilote de télécopie principal est supprimé si elle n’est pas en cours d’utilisation (autrement dit, s’il n’existe aucune file d’attente à l’utiliser). Si le pilote de télécopie principal est supprimé, la seule façon de réactiver la fonctionnalité télécopie consiste à réinstaller le composant de télécopie.
- Utilisée sans paramètres, **prndrvr** affiche l’aide en ligne de commande pour le **prndrvr** commande.

## <a name="BKMK_examples"></a>Exemples

Pour répertorier tous les pilotes sur le \\\printServer1 serveur, tapez :
```
cscript Prndrvr -l -s
```

Pour ajouter un pilote d’imprimante Windows x64 version 3 pour le modèle « modèle d’imprimante Laser 1 » de l’imprimante à l’aide du fichier d’informations de pilote C:\temp\Laserprinter1.inf pour un pilote stocké dans le type de dossier C:\temp :
```
cscript Prndrvr -a -m "Laser printer model 1" -v 3 -e "Windows x64" -i c:\temp\Laserprinter1.inf -h c:\temp
```

Pour supprimer un pilote d’imprimante Windows NT x86 version 3 pour « modèle d’imprimante Laser 1 », tapez :
```
cscript Prndrvr -a -m "Laser printer model 1" -v 3 -e "Windows NT x86" 
```

#### <a name="additional-references"></a>Références supplémentaires
[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
[référence des commandes d’impression](print-command-reference.md)
