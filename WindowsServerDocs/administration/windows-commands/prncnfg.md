---
title: prncnfg
description: Découvrez comment configurer une imprimante à l’aide de la commande prncfg.
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 6426b053a5c56918768f82cbd7631631abcf0a6f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59859150"
---
# <a name="prncnfg"></a>prncnfg

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Configure ou affiche des informations de configuration sur une imprimante.

## <a name="syntax"></a>Syntaxe
```
cscript Prncnfg {-g | -t | -x | -?} [-S <ServerName>] [-P <printerName>] [-z <NewprinterName>] [-u <UserName>] [-w <Password>] [-r <PortName>] [-l <Location>] [-h <Sharename>] [-m <Comment>] [-f <SeparatorFileName>] [-y <Datatype>] [-st <starttime>] [-ut <Untiltime>] [-i <DefaultPriority>] [-o <Priority>] [<+|->shared] [<+|->direct] [<+|->hidden] [<+|->published] [<+|->rawonly] [<+|->queued] [<+|->enablebidi] [<+|->keepprintedjobs] [<+|->workoffline] [<+|->enabledevq] [<+|->docompletefirst]
```

## <a name="parameters"></a>Paramètres
|Paramètre|Description|
|-------|--------|
|-g|Affiche des informations de configuration sur une imprimante.|
|-t|Configure une imprimante.|
|-x|Renomme une imprimante.|
|S - \<nom_serveur\>|Spécifie le nom de l’ordinateur distant qui héberge l’imprimante que vous souhaitez gérer. Si vous ne spécifiez pas un ordinateur, l’ordinateur local est utilisé.|
|-P \<printerName\>|Spécifie le nom de l’imprimante que vous souhaitez gérer. Obligatoire.|
|-z \<NewprinterName\>|Spécifie le nouveau nom de l’imprimante. Nécessite le **- x** et **-P** paramètres.|
|-u \<nom d’utilisateur\> -w \<mot de passe\>|Spécifie un compte disposant d’autorisations pour se connecter à l’ordinateur qui héberge l’imprimante que vous souhaitez gérer. Tous les membres du groupe Administrateurs local de l’ordinateur cible disposent de ces autorisations, mais les autorisations peuvent également être accordées aux autres utilisateurs. Si vous ne spécifiez pas un compte, vous devez être connecté sous un compte disposant de ces autorisations pour la commande fonctionne.|
|r - \<Nom_Port\>|Spécifie le port auquel l’imprimante est connectée. S’il s’agit d’une action parallèle ou un port série, puis utilisez l’ID du port (par exemple, LPT1 ou COM1). S’il s’agit d’un port TCP/IP, utilisez le nom de port spécifié lors de l’ajout du port.|
|-l \<emplacement\>|Spécifie l’emplacement de l’imprimante, telles que « copie salle ».|
|-h \<nom_partage\>|Spécifie le nom de partage de l’imprimante.|
|m - \<commentaire\>|Spécifie la chaîne de commentaire de l’imprimante.|
|-f \<SeparatorFileName\>|Spécifie un fichier qui contient le texte qui apparaît sur la page de séparation.|
|-y \<type de données\>|Spécifie les types de données que l’imprimante peut accepter.|
|-st \<starttime\>|Configure l’imprimante pour la disponibilité limitée. Spécifie l’heure du jour de que l’imprimante est disponible. Si vous envoyez un document à une imprimante lorsqu’il n’est pas disponible, le document est en attente (mises en attente) jusqu'à ce que l’imprimante devienne disponible. Vous devez spécifier l’heure en tant qu’une horloge de 24 heures. Par exemple, pour spécifier 11:00 PM, tapez **2300**.|
|-ut \<Endtime\>|Configure l’imprimante pour la disponibilité limitée. Spécifie l’heure du jour de que l’imprimante n’est plus disponible. Si vous envoyez un document à une imprimante lorsqu’il n’est pas disponible, le document est en attente (mises en attente) jusqu'à ce que l’imprimante devienne disponible. Vous devez spécifier l’heure en tant qu’une horloge de 24 heures. Par exemple, pour spécifier 11:00 PM, tapez **2300**.|
|-o \<priorité\>|Spécifie une priorité que le spouleur utilise pour acheminer les travaux d’impression dans la file d’attente. Une file d’attente avec une priorité supérieure reçoit tous ses travaux avant toute file d’attente avec une priorité plus faible.|
|-i \<DefaultPriority\>|Spécifie la priorité par défaut affectée à chaque travail d’impression.|
|{+&#124;-}shared|Spécifie si cette imprimante est partagée sur le réseau.|
|{+&#124;-}direct|Spécifie si le document doit être envoyé directement à l’imprimante sans mis en attente.|
|{+&#124;-}published|Spécifie si cette imprimante doit être publiée dans active directory. Si vous publiez l’imprimante, les autres utilisateurs peuvent rechercher en fonction de son emplacement et les fonctionnalités (telles que l’impression en couleur et agrafage).|
|{+&#124;-}hidden|Fonction réservé.|
|{+&#124;-}rawonly|Spécifie si uniquement des données brutes travaux d’impression peuvent être mis en attente dans cette file d’attente.|
|{+ &#124; -}queued|Spécifie que l’imprimante ne doit pas commencer à imprimer que lorsque la dernière page du document est mis en attente. Le programme d’impression n’est pas disponible jusqu'à ce que la fin du document d’impression. Toutefois, à l’aide de ce paramètre garantit que l’ensemble du document est disponible à l’imprimante.|
|{+ &#124; -}keepprintedjobs|Spécifie si le spouleur doit conserver les documents sont imprimés. L’activation de cette option permet à un utilisateur de renvoyer un document à l’imprimante à partir de la file d’attente au lieu d’à partir du programme d’impression.|
|{+ &#124; -}workoffline|Spécifie si un utilisateur est en mesure d’envoyer des travaux d’impression à la file d’attente si l’ordinateur n’est pas connecté au réseau.|
|{+ &#124; -}enabledevq|Indique si les travaux d’impression qui ne correspondent pas à la configuration de l’imprimante (par exemple, PostScript fichiers mis en attente sur des imprimantes non PostScript) doit être conservé dans la file d’attente au lieu d’en cours d’impression.|
|{+ &#124; -}docompletefirst|Spécifie si le spouleur doit envoyer des travaux d’impression avec une priorité plus faible qui ont terminé la mise en attente avant d’envoyer des travaux d’impression avec une priorité plus élevée qui n’est pas terminées en attente. Si cette option est activée et aucun document ne terminées mise en attente, le spouleur d’envoyer des documents plus volumineux avant les petits. Vous devez activer cette option si vous souhaitez optimiser l’efficacité d’imprimante au détriment de la priorité du travail. Si cette option est désactivée, le spouleur envoie toujours les travaux de priorité plus élevés pour leurs files d’attente respectives tout d’abord.|
|{+ &#124; -}enablebidi|Spécifie si l’imprimante envoie des informations d’état pour le spouleur.|
|/?|Affiche l'aide à l'invite de commandes.|

## <a name="remarks"></a>Notes
-   Le **prncnfg** commande est un script Visual Basic situé dans le %WINdir%\System32\printing_Admin_Scripts\\ <language> directory. Pour utiliser cette commande, à une invite de commandes, tapez **cscript** suivie du chemin d’accès complet au fichier de prncnfg, ou modifiez les répertoires vers le dossier approprié. Exemple :
    ```
    cscript %WINdir%\System32\printing_Admin_Scripts\en-US\prncnfg
    ```
-   Si les informations que vous fournissez contient des espaces, utilisez des guillemets autour du texte (par exemple, `"computer Name"`).

## <a name="BKMK_examples"></a>Exemples
Pour afficher les informations de configuration de l’imprimante appelée ImprimanteCouleur_2 avec une file d’attente à l’impression hébergée par l’ordinateur distant appelé HRServeur, tapez :
```
cscript prncnfg -g -S HRServer -P colorprinter_2 
```

Pour configurer une imprimante appelée ImprimanteCouleur_2 afin que le spouleur sur l’ordinateur distant appelé HRServeur conserve les travaux d’impression une fois qu’ils ont été imprimés, tapez :
```
cscript prncnfg -t -S HRServer -P colorprinter_2 +keepprintedjobs 
```

Pour modifier le nom d’une imprimante sur l’ordinateur distant HRServeur ImprimanteCouleur_2 à ImprimanteCouleur 3, type :
```
cscript prncnfg -x -S HRServer -P colorprinter_2 -z "colorprinter 3" 
```

#### <a name="additional-references"></a>Références supplémentaires
[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
[référence des commandes d’impression](print-command-reference.md)
