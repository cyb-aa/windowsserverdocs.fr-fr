---
title: bcdedit
description: Rubrique de référence pour la commande bcdedit, qui crée de nouveaux magasins, modifie les magasins existants et ajoute les paramètres du menu de démarrage.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: ab2da47d-3aac-44a0-b7fd-bd9561d61553
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 03/27/2018
ms.openlocfilehash: df68c2c1b1087f7e32dbe270657a54e072261c46
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82718699"
---
# <a name="bcdedit"></a>bcdedit

Les fichiers Données de configuration de démarrage (BCD) (BCD) fournissent un magasin utilisé pour décrire les applications de démarrage et les paramètres d’application de démarrage. Les objets et les éléments du magasin remplacent effectivement Boot. ini.

BCDEdit est un outil en ligne de commande permettant de gérer les magasins BCD. Il peut être utilisé à diverses fins, notamment la création de magasins, la modification de magasins existants, l’ajout de paramètres de menu de démarrage, etc. BCDEdit remplit essentiellement la même fonction que Bootcfg. exe sur les versions antérieures de Windows, mais avec deux améliorations majeures :

- Expose un plus grand nombre de paramètres de démarrage que Bootcfg. exe.

- A amélioré la prise en charge des scripts.

> [!NOTE]
> Des privilèges d’administrateur sont nécessaires pour modifier BCD à l’aide de BCDEdit.

BCDEdit est l’outil principal pour la modification de la configuration de démarrage de Windows Vista et des versions ultérieures de Windows. Il est inclus dans la distribution de Windows Vista dans le dossier%WINDIR%\System32

BCDEdit est limité aux types de données standard et est conçu principalement pour effectuer des modifications communes uniques à BCD. Pour des opérations plus complexes ou des types de données non standard, envisagez d’utiliser l’interface de programmation d’applications (API) BCD Windows Management Instrumentation (WMI) pour créer des outils personnalisés plus puissants et plus flexibles.

## <a name="syntax"></a>Syntaxe

```
bcdedit /command [<argument1>] [<argument2>] ...
```

### <a name="parameters"></a>Paramètres

### <a name="general-bcdedit-command-line-options"></a>Options générales de ligne de commande BCDEdit

| Option | Description |
| ------ | ----------- |
| /? | Affiche la liste des commandes BCDEdit. L’exécution de cette commande sans argument affiche un résumé des commandes disponibles. Pour afficher une aide détaillée pour une commande particulière, exécutez **bcdedit/ ?** `<command>`, où `<command>` est le nom de la commande sur laquelle vous recherchez des informations supplémentaires. Par exemple, **bcdedit/ ? CreateStore** affiche une aide détaillée pour la commande CreateStore. |

#### <a name="parameters-that-operate-on-a-store"></a>Paramètres qui fonctionnent sur un magasin

| Option | Description |
| ------ | ----------- |
| /createstore | Crée un magasin de données de configuration de démarrage vide. Le magasin créé n’est pas un magasin système. |
| /Export. | Exporte le contenu du magasin système dans un fichier. Ce fichier peut être utilisé ultérieurement pour restaurer l’état du magasin système. Cette commande est valide uniquement pour le magasin système. |
| /Import | Restaure l’état du magasin système à l’aide d’un fichier de données de sauvegarde généré précédemment à l’aide de l’option **/Export** . Cette commande supprime toutes les entrées existantes dans le magasin système avant l’importation. Cette commande est valide uniquement pour le magasin système. |
| /Store | Cette option peut être utilisée avec la plupart des commandes BCDedit pour spécifier le magasin à utiliser. Si cette option n’est pas spécifiée, BCDEdit fonctionne sur le magasin système. L’exécution de la commande **/Store bcdedit** en soi revient à exécuter la commande **active bcdedit/enum** . |

#### <a name="parameters-that-operate-on-entries-in-a-store"></a>Paramètres qui fonctionnent sur les entrées d’un magasin

| Paramètre | Description |
| ------ | ----------- |
| /Copy | Effectue une copie d’une entrée de démarrage spécifiée dans le même magasin système. |
| /Create | Crée une entrée dans le magasin de données de configuration de démarrage. Si un identificateur bien connu est spécifié, les paramètres **/application**, **/inherit**et **/Device** ne peuvent pas être spécifiés. Si un identificateur n’est pas spécifié ou n’est pas bien connu, une option **/application**, **/inherit**ou **/Device** doit être spécifiée. |
| /delete | Supprime un élément d’une entrée spécifiée. |

#### <a name="parameters-that-operate-on-entry-options"></a>Paramètres qui opèrent sur les options d’entrée

| Paramètre | Description |
| ------ | ----------- |
| /deletevalue | Supprime un élément spécifié d’une entrée de démarrage. |
| /Set | Définit une valeur d’option d’entrée. |

#### <a name="parameters-that-control-output"></a>Paramètres qui contrôlent la sortie

| Paramètre | Description |
| ------ | ----------- |
| /enum All | Répertorie les entrées d’un magasin. L’option **/enum** étant la valeur par défaut de BCEdit, l’exécution de la commande **bcdedit** sans paramètre équivaut à exécuter la commande **active bcdedit/enum** . |
| /v | Mode documenté. En règle générale, tous les identificateurs d’entrée connus sont représentés par leur forme abrégée conviviale. Si vous spécifiez **/v** en tant qu’option de ligne de commande, tous les identificateurs sont intégralement affichés. L’exécution de la commande **bcdedit/v** en soi revient à exécuter la commande **bcdedit/enum active/v** . |

#### <a name="parameters-that-control-the-boot-manager"></a>Paramètres qui contrôlent le gestionnaire de démarrage

| Paramètre | Description |
| ------ | ----------- |
| /bootsequence | Spécifie l’ordre d’affichage à usage unique à utiliser pour le prochain démarrage. Cette commande est similaire à l’option **/displayorder** , à ceci près qu’elle est utilisée uniquement lors du prochain démarrage de l’ordinateur. Par la suite, l’ordre d’affichage d’origine de l’ordinateur est rétabli. |
| /par défaut | Spécifie l’entrée par défaut que le gestionnaire de démarrage sélectionne lorsque le délai d’attente expire. |
| /displayorder | Spécifie l’ordre d’affichage que le gestionnaire de démarrage utilise lors de l’affichage des paramètres de démarrage pour un utilisateur. |
| /Timeout | Spécifie le délai d’attente, en secondes, avant que le gestionnaire de démarrage sélectionne l’entrée par défaut. |
| /toolsdisplayorder | Spécifie l’ordre d’affichage du gestionnaire de démarrage à utiliser lors de l’affichage du menu **Outils** . |

#### <a name="parameters-that-control-emergency-management-services"></a>Paramètres qui contrôlent les services de gestion d’urgence

| Paramètre | Description |
| ------ | ----------- |
| /bootems | Active ou désactive les services de gestion d’urgence (EMS) pour l’entrée spécifiée. |
| /EMS | Active ou désactive EMS pour l’entrée de démarrage du système d’exploitation spécifiée. |
| /emssettings | Définit les paramètres EMS globaux de l’ordinateur. **/emssettings** n’active pas ou ne désactive pas EMS pour une entrée de démarrage particulière. |

#### <a name="parameters-that-control-debugging"></a>Paramètres qui contrôlent le débogage

| Paramètre | Description |
| ------ | ----------- |
| /bootdebug | Active ou désactive le débogueur de démarrage pour une entrée de démarrage spécifiée. Bien que cette commande fonctionne pour n’importe quelle entrée de démarrage, elle est effective uniquement pour les applications de démarrage. |
| /dbgsettings | Spécifie ou affiche les paramètres globaux du débogueur pour le système. Cette commande n’est pas enablepose. Pour définir un paramètre de débogueur global individuel, utilisez la commande **bcdedit/set** `<dbgsettings> <type> <value>` . |
| /debug | Active ou désactive le débogueur de noyau pour une entrée de démarrage spécifiée. |

## <a name="additional-references"></a>Références supplémentaires

Pour obtenir des exemples d’utilisation de BCDEdit, consultez l’article de référence sur les [options bcdedit](https://docs.microsoft.com/windows-hardware/drivers/devtest/bcd-boot-options-reference) .

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
