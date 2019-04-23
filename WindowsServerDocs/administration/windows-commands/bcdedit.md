---
title: bcdedit
description: Rubrique de commandes de Windows pour **bcdedit** - créer des magasins de modifier les magasins existants et ajouter des paramètres de menu de démarrage.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ab2da47d-3aac-44a0-b7fd-bd9561d61553
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 03/27/2018
ms.openlocfilehash: c1ac016b299cbd72a406121c54762f4457b24286
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59872440"
---
# <a name="bcdedit"></a>bcdedit



Les fichiers de données de Configuration (BCD) démarrage fournissent un magasin qui est utilisé pour décrire les applications de démarrage et leurs paramètres. Les objets et les éléments dans le magasin remplacent efficacement le fichier Boot.ini.

BCDEdit est un outil de ligne de commande pour la gestion des magasins BCD. Il peut être utilisé pour diverses raisons, y compris la création de nouveaux magasins, la modification des magasins existants, ajout de paramètres de menu de démarrage et ainsi de suite. BCDEdit sert essentiellement de la même fonction que Bootcfg.exe dans les versions antérieures de Windows, mais avec deux améliorations majeures :
-   Expose un éventail plus large des paramètres de démarrage que Bootcfg.exe.
-   A amélioré la prise en charge de script.

> [!NOTE]
> Des privilèges d’administrateur sont requis pour utiliser BCDEdit pour modifier BCD.

BCDEdit est le principal outil pour modifier la configuration de démarrage de Windows Vista et versions ultérieures de Windows. Il est inclus avec la distribution de Windows Vista dans le dossier %WINDIR%\System32.

BCDEdit est limité aux types de données standard et est principalement conçu pour effectuer des modifications courantes uniques pour BCD. Pour des opérations plus complexes ou des types de données non standard, envisagez d’utiliser l’interface de programmation d’application (API) de BCD Windows Management Instrumentation (WMI) pour créer des outils personnalisés plus puissants et flexibles.

## <a name="syntax"></a>Syntaxe

```
BCDEdit /Command [<Argument1>] [<Argument2>] ...
```

## <a name="parameters"></a>Paramètres

### <a name="general-bcdedit-command-line-option"></a>Option de ligne de commande BCDEdit de général

|Option|Description|
|------|-----------|
|/?|Affiche une liste de commandes de BCDEdit. Exécution de cette commande sans argument affiche un résumé des commandes disponibles. Pour afficher une aide détaillée pour une commande particulière, exécutez **bcdedit / ?** \<commande >, où <command> est le nom de la commande que vous recherchez plus d’informations sur les. Par exemple, **bcdedit / ? createstore** affiche une aide détaillée pour la commande Createstore.|

### <a name="parameters-that-operate-on-a-store"></a>Paramètres qui opèrent sur un Store

|Option|Description|
|------|-----------|
|/createstore|Crée un nouveau magasin de données de configuration de démarrage vide. Le magasin créé n’est pas un magasin système.|
|/Export|Exporte le contenu du système de stocke dans un fichier. Ce fichier peut être utilisé ultérieurement pour restaurer l’état du magasin système. Cette commande est valide uniquement pour le magasin système.|
|/import|Restaure l’état du magasin système à l’aide d’un fichier de données de sauvegarde précédemment généré à l’aide de la **/export** option. Cette commande supprime toutes les entrées existantes dans le magasin du système avant de lancer l’importation. Cette commande est valide uniquement pour le magasin système.|
|/store|Cette option peut être utilisée avec la plupart des commandes de BCDedit pour spécifier le magasin à utiliser. Si cette option n’est pas spécifiée, BCDEdit opère sur le magasin système. En cours d’exécution le **bcdedit /store** commande en elle-même équivaut à exécuter la **bcdedit/enum active** commande.|

### <a name="parameters-that-operate-on-entries-in-a-store"></a>Paramètres qui opèrent sur des entrées dans un Store

|Paramètre|Description|
|---------|-----------|
|/ Copy|Effectue une copie d’une entrée de démarrage spécifiée dans le même magasin système.|
|/ créer|Crée une nouvelle entrée dans le magasin de données de configuration de démarrage. Si un identificateur connu est spécifié, puis le **application/**, **/ héritent**, et **/Device** paramètres ne peut pas être spécifiés. Si un identificateur n’est pas spécifié ou pas connue, un **application/**, **/ héritent**, ou **/Device** option doit être spécifiée.|
|/delete|Supprime un élément d’une entrée spécifiée.|

### <a name="parameters-that-operate-on-entry-options"></a>Paramètres qui fonctionnent sur les Options d’entrée

|Paramètre|Description|
|---------|-----------|
|/deletevalue|Supprime un élément spécifié à partir d’une entrée de démarrage.|
|/set|Définit une valeur d’option entrée.|

### <a name="parameters-that-control-output"></a>Paramètres qui contrôlent la sortie

|Paramètre|Description|
|---------|-----------|
|/ enum|Répertorie les entrées dans un magasin. Le **/enum** option est la valeur par défaut pour BCEdit, par conséquent, en cours d’exécution le **bcdedit** commande sans paramètres équivaut à exécuter la **bcdedit/enum active** commande.|
|/v|Mode détaillé. En règle générale, les identificateurs d’entrée bien connus sont représentés par leur forme raccourcie convivial. Spécification **/v** comme une option de ligne de commande affiche tous les identificateurs dans sa totalité. En cours d’exécution le **bcdedit /v** commande en elle-même équivaut à exécuter la **bcdedit/enum active /v** commande.|

### <a name="parameters-that-control-the-boot-manager"></a>Paramètres qui contrôlent le Gestionnaire de démarrage

|Paramètre|Description|
|---------|-----------|
|/bootsequence|Spécifie un ordre d’affichage à usage unique à utiliser pour le prochain démarrage. Cette commande est similaire à la **/displayorder** option, à ceci près qu’elle est utilisée uniquement la prochaine fois que l’ordinateur démarre. Par la suite, l’ordinateur revient à l’ordre d’affichage d’origine.|
|/default|Spécifie l’entrée par défaut que le Gestionnaire de démarrage sélectionne lorsque le délai d’attente expire.|
|/displayorder|Spécifie l’ordre d’affichage par le Gestionnaire de démarrage lors de l’affichage des paramètres de démarrage à un utilisateur.|
|/timeout|Spécifie le délai d’attente, en secondes, avant que le Gestionnaire de démarrage sélectionne l’entrée par défaut.|
|/TOOLSDISPLAYORDER|Spécifie l’ordre d’affichage pour le Gestionnaire de démarrage à utiliser lors de l’affichage le **outils** menu.|

### <a name="parameters-that-control-emergency-management-services"></a>Paramètres qui contrôlent les Services de gestion d’urgence

|Paramètre|Description|
|---------|-----------|
|/bootems|Active ou désactive les Services de gestion d’urgence (EMS) pour l’entrée spécifiée.|
|/EMS|Active ou désactive l’EMS pour l’entrée de démarrage du système d’exploitation spécifié.|
|/emssettings|Définit les paramètres EMS globaux pour l’ordinateur. **/emssettings** ne pas activer ou désactiver EMS pour toute entrée de démarrage spécifique.|

### <a name="parameters-that-control-debugging"></a>Paramètres qui contrôlent le débogage

|Paramètre|Description|
|---------|-----------|
|/bootdebug|Active ou désactive le débogueur de démarrage pour une entrée de démarrage spécifiée. Bien que cette commande fonctionne pour toute entrée de démarrage, il est efficace uniquement pour les applications de démarrage.|
|/dbgsettings|Spécifie ou affiche les paramètres globaux du débogueur pour le système. Cette commande ne pas activer ou désactiver le débogueur de noyau ; utiliser le **/debug** option à cet effet. Pour définir un paramètre de débogueur global individuel, utilisez le **bcdedit /set** \<dbgsettings > <type> <value> le cas échéant.|
|/debug|Active ou désactive le débogueur du noyau pour une entrée de démarrage spécifiée.|

## <a name="examples"></a>Exemples

Pour obtenir des exemples de BCDEdit, consultez le [référence des Options de BCDEdit](https://docs.microsoft.com/windows-hardware/drivers/devtest/bcd-boot-options-reference).