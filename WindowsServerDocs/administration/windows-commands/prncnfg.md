---
title: prncnfg
description: Découvrez comment configurer une imprimante à l’aide de la commande prncfg.
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 38a4e8fa-3122-495b-a125-35b926bc6415
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/11/2018
ms.openlocfilehash: 5cbbf82e832c50d168e0bef06b2b7c3022dd90e8
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71372138"
---
# <a name="prncnfg"></a>prncnfg

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Configure ou affiche les informations de configuration relatives à une imprimante.

## <a name="syntax"></a>Syntaxe
```
cscript Prncnfg {-g | -t | -x | -?} [-S <ServerName>] [-P <printerName>] [-z <NewprinterName>] [-u <UserName>] [-w <Password>] [-r <PortName>] [-l <Location>] [-h <Sharename>] [-m <Comment>] [-f <SeparatorFileName>] [-y <Datatype>] [-st <starttime>] [-ut <Untiltime>] [-i <DefaultPriority>] [-o <Priority>] [<+|->shared] [<+|->direct] [<+|->hidden] [<+|->published] [<+|->rawonly] [<+|->queued] [<+|->enablebidi] [<+|->keepprintedjobs] [<+|->workoffline] [<+|->enabledevq] [<+|->docompletefirst]
```

## <a name="parameters"></a>Paramètres
|Paramètre|Description|
|-------|--------|
|-g|Affiche les informations de configuration relatives à une imprimante.|
|-t|Configure une imprimante.|
|-x|renomme une imprimante.|
|-S \<ServerName\>|Spécifie le nom de l’ordinateur distant qui héberge l’imprimante que vous souhaitez gérer. Si vous ne spécifiez pas d’ordinateur, l’ordinateur local est utilisé.|
|-P \<nom_imprimante\>|Spécifie le nom de l’imprimante que vous souhaitez gérer. Obligatoire.|
|-z \<NewprinterName\>|Spécifie le nouveau nom de l’imprimante. Requiert les paramètres **-x** et **-P** .|
|-u \<nom_utilisateur\>-w \<mot de passe\>|Spécifie un compte disposant des autorisations nécessaires pour se connecter à l’ordinateur qui héberge l’imprimante que vous souhaitez gérer. Tous les membres du groupe Administrateurs local de l’ordinateur cible disposent de ces autorisations, mais les autorisations peuvent également être accordées à d’autres utilisateurs. Si vous ne spécifiez pas de compte, vous devez avoir ouvert une session sous un compte disposant de ces autorisations pour que la commande fonctionne.|
|-r \<PortName\>|Spécifie le port sur lequel l’imprimante est connectée. S’il s’agit d’un port parallèle ou d’un port série, utilisez l’ID du port (par exemple, LPT1 ou COM1). S’il s’agit d’un port TCP/IP, utilisez le nom de port qui a été spécifié lors de l’ajout du port.|
|-l \<emplacement\>|Spécifie l’emplacement de l’imprimante, par exemple « copie de l’espace ».|
|-h \<nom_partage\>|Spécifie le nom de partage de l’imprimante.|
|-m \<commentaire\>|Spécifie la chaîne de commentaire de l’imprimante.|
|-f \<SeparatorFileName\>|Spécifie un fichier qui contient le texte qui apparaît sur la page de séparation.|
|-y \<type de données\>|Spécifie les types de données que l’imprimante peut accepter.|
|-St \<StartTime\>|Configure l’imprimante pour une disponibilité limitée. Spécifie l’heure de la journée à laquelle l’imprimante est disponible. Si vous envoyez un document à une imprimante lorsqu’il n’est pas disponible, le document est conservé (mis en file d’attente) jusqu’à ce que l’imprimante soit disponible. Vous devez spécifier l’heure sous la forme d’une horloge de 24 heures. Par exemple, pour spécifier 11:00, tapez **2300**.|
|-UT \<EndTime\>|Configure l’imprimante pour une disponibilité limitée. Spécifie l’heure de la journée à laquelle l’imprimante n’est plus disponible. Si vous envoyez un document à une imprimante lorsqu’il n’est pas disponible, le document est conservé (mis en file d’attente) jusqu’à ce que l’imprimante soit disponible. Vous devez spécifier l’heure sous la forme d’une horloge de 24 heures. Par exemple, pour spécifier 11:00, tapez **2300**.|
|-o \<priorité\>|Spécifie une priorité que le spouleur utilise pour acheminer les travaux d’impression dans la file d’attente à l’impression. Une file d’attente à l’impression avec une priorité plus élevée reçoit tous ses travaux avant toute file d’attente avec une priorité plus faible.|
|-i \<DefaultPriority\>|Spécifie la priorité par défaut assignée à chaque travail d’impression.|
|{+&#124;-} partagé|Spécifie si cette imprimante est partagée sur le réseau.|
|{+&#124;-} direct|Spécifie si le document doit être envoyé directement à l’imprimante sans être mis en file d’attente.|
|{+&#124;-} publié (e)|Spécifie si cette imprimante doit être publiée dans Active Directory. Si vous publiez l’imprimante, d’autres utilisateurs peuvent la Rechercher en fonction de son emplacement et de ses fonctionnalités (telles que l’impression et l’agrafage des couleurs).|
|{+&#124;-} masqué|Fonction réservée.|
|{+&#124;-} RawOnly|Spécifie si seuls les travaux d’impression de données brutes peuvent être mis en attente dans cette file d’attente.|
|{+ &#124; -} mis en file d’attente|Spécifie que l’imprimante ne doit pas commencer à imprimer tant que la dernière page du document n’a pas été mise en file d’attente. Le programme d’impression n’est pas disponible tant que l’impression du document n’est pas terminée. Toutefois, l’utilisation de ce paramètre permet de s’assurer que l’ensemble du document est disponible pour l’imprimante.|
|{+ &#124; -} KeepPrintedJobs|Spécifie si le spouleur doit conserver les documents une fois qu’ils sont imprimés. L’activation de cette option permet à un utilisateur de soumettre à nouveau un document à l’imprimante à partir de la file d’attente à l’impression et non à partir du programme d’impression.|
|{+ &#124; -} WorkOffline|Spécifie si un utilisateur est en mesure d’envoyer des travaux d’impression à la file d’attente à l’impression si celui-ci n’est pas connecté au réseau.|
|{+ &#124; -} EnableDevq|Spécifie si les travaux d’impression qui ne correspondent pas à la configuration de l’imprimante (par exemple, les fichiers PostScript mis en attente pour les imprimantes non-PostScript) doivent être conservés dans la file d’attente au lieu d’être imprimés.|
|{+ &#124; -} DoCompleteFirst|Spécifie si le spouleur doit envoyer des travaux d’impression dont la priorité est inférieure à la mise en file d’attente avant d’envoyer des travaux d’impression avec une priorité plus élevée qui n’a pas terminé la mise en file d’attente. Si cette option est activée et qu’aucun document n’a été mis en file d’attente, le spouleur envoie des documents plus volumineux avant ceux qui sont plus petits. Vous devez activer cette option si vous souhaitez maximiser l’efficacité de l’imprimante au détriment de la priorité du travail. Si cette option est désactivée, le spouleur envoie toujours en premier les travaux avec une priorité plus élevée à leurs files d’attente respectives.|
|{+ &#124; -} EnableBIDI|Spécifie si l’imprimante envoie des informations d’État au spouleur.|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="remarks"></a>Notes
-   La commande **prncnfg** est un script Visual Basic situé dans le répertoire%windir%\system32\ printing_Admin_Scripts\\<language>. Pour utiliser cette commande, à l’invite de commandes, tapez **cscript** suivi du chemin d’accès complet au fichier prncnfg ou accédez au dossier approprié. Par exemple :
    ```
    cscript %WINdir%\System32\printing_Admin_Scripts\en-US\prncnfg
    ```
-   Si les informations que vous fournissez contiennent des espaces, utilisez des guillemets autour du texte (par exemple, `"computer Name"`).

## <a name="BKMK_examples"></a>Illustre
Pour afficher les informations de configuration de l’imprimante nommée colorprinter_2 avec une file d’attente à l’impression hébergée par l’ordinateur distant nommé HRServer, tapez :
```
cscript prncnfg -g -S HRServer -P colorprinter_2 
```

Pour configurer une imprimante nommée colorprinter_2 afin que le spouleur de l’ordinateur distant nommé HRServer conserve les travaux d’impression après leur impression, tapez :
```
cscript prncnfg -t -S HRServer -P colorprinter_2 +keepprintedjobs 
```

Pour modifier le nom d’une imprimante sur l’ordinateur distant nommé HRServer, de colorprinter_2 à colorprinter 3, tapez :
```
cscript prncnfg -x -S HRServer -P colorprinter_2 -z "colorprinter 3" 
```

#### <a name="additional-references"></a>Références supplémentaires
[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
[référence de commande d’impression](print-command-reference.md)
