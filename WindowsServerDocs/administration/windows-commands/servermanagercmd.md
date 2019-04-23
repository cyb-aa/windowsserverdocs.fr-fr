---
title: Servermanagercmd
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 507c4b87-8e13-4872-8b34-0c7508eecbc1
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/11/2018
ms.openlocfilehash: ba0b85814d942323b12e1874b852fcf28b8ac068
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59883240"
---
# <a name="servermanagercmd"></a>Servermanagercmd

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

> [!IMPORTANT]
> Cette commande est disponible uniquement sur les serveurs qui exécutent Windows Server 2008 ou Windows Server 2008 R2. **ServerManagerCmd.exe** a été déconseillée et n’est pas disponible dans Windows Server 2012. Pour plus d’informations sur la façon d’installer ou supprimer des rôles, services de rôle et fonctionnalités dans Windows Server 2012, consultez [installer ou désinstaller des rôles, services de rôle et fonctionnalités](https://go.microsoft.com/fwlink/?LinkID=239563) sur Microsoft TechNet.

Installe et supprime des rôles, services de rôle et fonctionnalités. Également affiche la liste de tous les rôles, services de rôle et fonctionnalités disponibles et montre ce qui sont installé sur cet ordinateur. Pour plus d’informations sur les rôles, les services de rôle et les fonctionnalités que vous pouvez spécifier à l’aide de cet outil, consultez le [aide du Gestionnaire de serveur](https://go.microsoft.com/fwlink/?LinkID=137387). Pour obtenir des exemples d’utilisation de cette commande, consultez [Exemples](#BKMK_examples).

## <a name="syntax"></a>Syntaxe
```
servermanagercmd -query [[[<Drive>:]<path>]<query.xml>] [-logpath   [[<Drive>:]<path>]<log.txt>]
servermanagercmd -inputpath  [[<Drive>:]<path>]<answer.xml> [-resultpath <result.xml> [-restart] | -whatif] [-logpath [[<Drive>:]<path>]<log.txt>]
servermanagercmd -install <Id> [-allSubFeatures] [-resultpath   [[<Drive>:]<path>]<result.xml> [-restart] | -whatif] [-logpath   [[<Drive>:]<path>]<log.txt>]
servermanagercmd -remove <Id> [-resultpath    <result.xml> [-restart] | -whatif] [-logpath  [[<Drive>:]<path>]<log.txt>]
servermanagercmd [-help | -?]
servermanagercmd -version
```

## <a name="parameters"></a>Paramètres
|Paramètre|Description|
|-------|--------|
|-query [[[\<Drive>:]\<path>]\<*query.xml*>]|Affiche une liste de tous les rôles, services de rôle et fonctionnalités installées et disponibles pour installation sur le serveur. Vous pouvez également utiliser la forme abrégée de ce paramètre, **- q**. Si vous souhaitez que les résultats de requête enregistrés dans un fichier XML, spécifiez un fichier XML pour remplacer *query.xml*.|
|-inputpath < [[\<lecteur > :]\<chemin d’accès >]*answer.xml*>|Installe ou supprime les rôles, les services de rôle et les fonctionnalités spécifiées dans un fichier de réponses XML représenté par *answer.xml*. Vous pouvez également utiliser la forme abrégée de ce paramètre, **- p.**|
|-installer \< *Id*>|Installe le rôle, le service de rôle ou la fonctionnalité spécifiée par *Id*. Les identificateurs respectent la casse. Plusieurs rôles, services de rôle et fonctionnalités doivent être séparées par des espaces. Les paramètres facultatifs suivants sont utilisés avec la **-installer** paramètre.<br /><br />-   **-définition** \< *SettingName*>=\<*SettingValue*> Spécifie les paramètres requis pour l’installation.<br />-   **-allSubFeatures** Spécifie l’installation de tous les services et fonctionnalités secondaires, ainsi que le rôle parent, un service de rôle ou une fonctionnalité nommée dans le *Id* valeur. **Remarque :**     Certains conteneurs de rôle ne disposent pas d’un identificateur de ligne de commande pour permettre l’installation de tous les services de rôle. C’est le cas lorsque les services de rôle ne peut pas être installés dans la même instance de la commande du Gestionnaire de serveur. Par exemple, le service de rôle Service de fédération des Services de fédération active directory et le service de rôle Proxy du Service de fédération ne peut pas être installé à l’aide de la même instance de commande du Gestionnaire de serveur.<br />-   **-resultpath** \< *result.xml > enregistre les résultats de l’installation dans un fichier XML représenté par *result.xml*. Vous pouvez également utiliser la forme abrégée de ce paramètre, **- r**. **Remarque :**     Vous ne pouvez pas exécuter **servermanagercmd** avec à la fois le **- resultpath** paramètre et le **- whatif** paramètre spécifié.<br /> -    **-redémarrez** redémarre automatiquement l’ordinateur une fois l’installation terminée (si le redémarrage est requis par les rôles ou les fonctionnalités installées).<br /> -    **- whatif** affiche toutes les opérations spécifiées pour le **-installer** paramètre. Vous pouvez également utiliser la forme abrégée de le **- whatif** paramètre, **-w**. Vous ne pouvez pas exécuter **servermanagercmd** avec à la fois le **- resultpath** paramètre et le **- whatif** paramètre spécifié.<br /> -    **- logpath** \<[[\<lecteur > :]\<chemin d’accès >]* log.txt* > Spécifie le nom et l’emplacement du fichier journal, autre que la valeur par défaut, **%windir%\temp\servermanager.log**.|
|-supprimer \< *Id*>|Supprime le rôle, le service de rôle ou la fonctionnalité spécifiée par *Id*. Les identificateurs respectent la casse. Plusieurs rôles, services de rôle et fonctionnalités doivent être séparées par des espaces. Les paramètres facultatifs suivants sont utilisés avec la **-supprimer** paramètre.<br /><br />-   **-resultpath** \<[[\<lecteur > :]\<chemin d’accès >]*result.xml*> enregistre les résultats de la suppression dans un fichier XML représenté par *result.xml*. Vous pouvez également utiliser la forme abrégée de ce paramètre, **- r**. **Remarque :**     Vous ne pouvez pas exécuter **servermanagercmd** avec à la fois le **- resultpath** paramètre et le **- whatif** paramètre spécifié.<br />-   **-Redémarrez** redémarre l’ordinateur automatiquement lorsque la suppression est terminée (si le redémarrage est requis par les rôles et fonctionnalités restants).<br />-   **-whatif** affiche toutes les opérations spécifiées pour le **-supprimer** paramètre. Vous pouvez également utiliser la forme abrégée de le **- whatif** paramètre, **-w**. Vous ne pouvez pas exécuter **servermanagercmd** avec à la fois le **- resultpath** paramètre et le **- whatif** paramètre spécifié.<br />-   **-logpath**\<[[\<lecteur > :]\<chemin d’accès >]*log.txt*> Spécifie le nom et l’emplacement du fichier journal, autre que la valeur par défaut, **%windir%\temp\ servermanager.log**.|
|-help|Affiche l’aide dans la fenêtre d’invite de commandes. Vous pouvez également utiliser la forme abrégée, **- ?**.|
|-version|Affiche le numéro de version du Gestionnaire de serveur. Vous pouvez également utiliser la forme abrégée, **- v**.|

## <a name="remarks"></a>Notes
**ServerManagerCmd** est déconseillée et n’est pas garanti d’être pris en charge dans les versions futures de Windows. Nous recommandons que si vous exécutez le Gestionnaire de serveur sur les ordinateurs qui exécutent Windows Server 2008 R2, vous utilisez les applets de commande Windows PowerShell qui sont disponibles pour le Gestionnaire de serveur. Pour plus d’informations, consultez [applets de commande Gestionnaire de serveur](https://go.microsoft.com/fwlink/?LinkID=137653).
ServerManagerCmd peut être exécuté à partir de n’importe quel répertoire sur les lecteurs locaux du serveur. Vous devez être membre du groupe Administrateurs sur le serveur sur lequel vous souhaitez installer ou supprimer le logiciel.

> [!IMPORTANT]
> En raison des restrictions de sécurité imposées par le contrôle de compte d’utilisateur dans Windows Server 2008 R2, vous devez exécuter **Servermanagercmd** dans une fenêtre d’invite de commandes ouverte avec des autorisations élevées. Pour ce faire, cliquez sur l’invite de commande exécutable, ou le **invite de commandes** de l’objet sur le **Démarrer** menu, puis sur **exécuter en tant qu’administrateur**.

## <a name="BKMK_examples"></a>Exemples
L’exemple suivant montre comment utiliser **servermanagercmd** pour afficher une liste de tous les rôles, services de rôle et fonctionnalités disponibles et les rôles, les services de rôle et les fonctionnalités sont installées sur l’ordinateur.
```
servermanagercmd -query
```
L’exemple suivant montre comment utiliser **servermanagercmd** pour installer le rôle de serveur Web (IIS) et enregistrer les résultats de l’installation dans un fichier XML représenté par *installResult.xml*.
```
servermanagercmd -install Web-Server -resultpath installResult.xml
```
L’exemple suivant montre comment utiliser les ** paramètre whatif ** avec **servermanagercmd** pour afficher des informations détaillées sur les rôles, les services de rôle et les fonctionnalités qui seraient installer ou à supprimer, en fonction des instructions qui sont spécifiés dans un fichier de réponses XML représenté par *install.xml*.
```
servermanagercmd -inputpath install.xml -whatif
```

#### <a name="additional-references"></a>Références supplémentaires
-   Pour obtenir la liste complète de rôle, service de rôle ou les identificateurs de fonctionnalité, vous pouvez spécifier pour le *Id* paramètre ou plus d’informations sur l’utilisation d’un fichier de réponses XML avec **Servermanagercmd**, consultez le [Aide du Gestionnaire de serveur](https://go.microsoft.com/fwlink/?LinkID=137387). (https://go.microsoft.com/fwlink/?LinkID=137387).
-   Consultez [applets de commande Gestionnaire de serveur](https://go.microsoft.com/fwlink/?LinkID=137653) pour obtenir la liste des applets de commande Windows PowerShell qui sont disponibles pour le Gestionnaire de serveur.
-   [Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)
