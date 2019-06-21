---
title: cleanmgr
description: Découvrez comment utiliser les options de ligne de commande pour configurer l’outil de nettoyage de disque (Cleanmgr.exe) pour nettoyer automatiquement les certains fichiers.
ms.prod: windows-server-threshold
ms.reviewer: cosmosdarwin
author: iangpgh
ms.author: jgerend
manager: daveba
ms.technology: storage-spaces
ms.date: 06/20/2019
ms.openlocfilehash: edcf878723653c3b51c8b5b29e9cd93106159446
ms.sourcegitcommit: 078304c4b92bb57eb85ba29634afc92cc028c644
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67301603"
---
# <a name="cleanmgr"></a>cleanmgr

> S’applique à : Windows Server 2019, Windows Server 2016, Windows Server 2012, Windows Server 2008 R2, Windows Server (canal semi-annuel)

Efface les fichiers inutiles sur votre disque dur. Vous pouvez utiliser les options de ligne de commande pour spécifier que Cleanmgr nettoie Temp fichiers Internet fichiers, fichiers téléchargés et fichiers de la Corbeille. Vous pouvez ensuite planifier la tâche s’exécute à un moment donné à l’aide de l’outil tâches planifiées.

Pour obtenir des exemples d’utilisation de cette commande, consultez [Exemples](#examples).

## <a name="syntax"></a>Syntaxe

```
cleanmgr [/d <driveletter>] [/sageset:n]  [/sagerun:n] [/TUNEUP:n] [/LOWDISK] [/VERYLOWDISK]
```

## <a name="parameters"></a>Paramètres

|      Paramètre      |    Description     |
| ------------------- | ------------------ |
|  /d \<driveletter>          | Spécifie le lecteur que vous souhaitez que le nettoyage de disque à nettoyer.<br>**Remarque :** L’option /d n’est pas utilisée avec /sagerun:n. |
| /sageset:n | Affiche la boîte de dialogue Paramètres de nettoyage de disque et crée également une clé de Registre pour stocker les paramètres que vous sélectionnez. Le `n` valeur, qui est stocké dans le Registre, vous permet de spécifier des tâches de nettoyage de disque à exécuter. Le `n` valeur peut être toute valeur entière comprise entre 0 et 65535. Pour que toutes les options disponibles lorsque vous utilisez l’option /sageset, spécifiez le lecteur où Windows est installé.  |
|  /sagerun:n  |  Exécute les tâches spécifiées qui sont affectées à la valeur n Si vous utilisez l’option \sageset. Tous les lecteurs sur l’ordinateur sont énumérés et le profil sélectionné s’exécute sur chaque lecteur.           |
| /TUNEUP:n    | Exécutez /sageset et /sagerun pour le même `n` . |
| / LOWDISK     | Exécuter avec les paramètres par défaut. |
| / VERYLOWDISK | N’exécuter avec les paramètres par défaut, aucun utilisateur vous invite à entrer. |
| /?           | Afficher l’aide. |

## <a name="options"></a>Options

Les options pour les fichiers que vous pouvez spécifier pour le nettoyage de disque à l’aide de /sageset et /sagerun incluent :

- **Les fichiers d’installation temporaire** -il s’agit de fichiers qui ont été créés par un programme d’installation qui n’est plus exécuté.

- **Les fichiers programme téléchargés** : fichiers programmes téléchargés sont des contrôles ActiveX et les programmes Java qui sont automatiquement téléchargés à partir d’Internet lorsque vous affichez certaines pages. Ces fichiers sont temporairement stockés dans le dossier de fichiers programmes téléchargés sur le disque dur. Cette option inclut un bouton Afficher les fichiers afin que vous puissiez voir les fichiers avant le nettoyage de disque les supprime. Le bouton ouvre le dossier C:\Winnt\Downloaded Program Files.

- **Fichiers Internet temporaires** -dossier les fichiers Internet temporaires contient des pages Web qui sont stockés sur votre disque dur pour une consultation rapide. Nettoyage de disque supprime ces pages mais toucher à vos paramètres personnalisés pour les pages Web. Cette option inclut également un bouton Afficher les fichiers, qui ouvre le dossier C:\Documents and Settings\ Settings\Temporary Internet Files\Content.IE5. 

- **Anciens fichiers Chkdsk** -lorsque Chkdsk vérifie un disque pour les erreurs, il peut enregistrer des fragments de fichiers perdus en tant que fichiers dans le dossier racine sur le disque. Ces fichiers ne sont pas nécessaires.

- **Corbeille** -la Corbeille contient des fichiers que vous avez supprimé de l’ordinateur. Ces fichiers ne sont pas définitivement supprimés jusqu'à ce que vous videz la Corbeille. Cette option inclut un bouton Afficher les fichiers qui s’ouvre à la Corbeille. **Remarque :** Une Corbeille peuvent apparaître dans plusieurs lecteurs, par exemple, pas seulement dans % SystemRoot%.

- **Fichiers temporaires** -programmes stockent parfois des informations temporaires dans un dossier Temp. Avant la fermeture d’un programme, le programme supprime généralement ces informations. Vous pouvez supprimer en toute sécurité des fichiers temporaires qui n’ont pas été modifiées la semaine dernière.

- **Fichiers hors connexion temporaires** -temporairement hors connexion sont des copies locales de fichiers réseau récemment utilisés. Ces fichiers sont automatiquement mises en cache afin que vous puissiez les utiliser après la déconnexion du réseau. Un bouton Afficher les fichiers s’ouvre le dossier fichiers hors connexion.

- **Fichiers hors connexion** -fichiers hors connexion sont des copies locales de fichiers réseau que vous voulez avoir disponibles hors connexion afin que vous puissiez les utiliser après la déconnexion du réseau. Un bouton Afficher les fichiers s’ouvre le dossier fichiers hors connexion.

- **Compresser les anciens fichiers** -Windows peut compresser des fichiers que vous n’avez pas récemment utilisé. La compression des fichiers économise de l’espace de disque, mais vous pouvez toujours utiliser les fichiers. Aucun fichier n’est supprimés. Étant donné que les fichiers sont compressés à des vitesses différentes, la quantité affichée d’espace disque que vous gagnerez est approximative. Un bouton Options permet de spécifier le nombre de jours à attendre avant le nettoyage de disque compresse un fichier inutilisé.

- **Fichiers de catalogue d’indexation du contenu** -le service d’indexation accélère et enrichit les recherches en conservant un index des fichiers qui se trouvent sur le disque. Ces fichiers de catalogue restent à partir d’une opération d’indexation précédente et peuvent être supprimés en toute sécurité. **Remarque :** Fichier de catalogue semble dans plus d’un lecteur, par exemple, pas seulement dans % SystemRoot%.

**Remarque** si vous spécifiez le nettoyage du lecteur qui contient l’installation de Windows, toutes ces options sont disponibles sous l’onglet de nettoyage de disque. Si vous spécifiez un autre lecteur, uniquement la Corbeille et les fichiers de catalogue pour les options d’index de contenu sont disponibles sous l’onglet de nettoyage de disque. 

## <a name="examples"></a>Exemples

Pour exécuter l’application de nettoyage de disque afin que vous pouvez utiliser sa boîte de dialogue pour spécifier les options pour une utilisation ultérieure, l’enregistrement des paramètres à l’ensemble **1**, tapez la commande suivante :

```
cleanmgr /sageset:1
```

Pour exécuter le nettoyage de disque et inclure les options que vous avez spécifié avec la commande de /sageset:1 cleanmgr, tapez :

```
cleanmgr /sagerun:1
```

Pour exécuter ```cleanmgr /sageset:1``` et ```cleanmgr /sagerun:1``` ensemble, tapez :

```
cleanmgr /tuneup:1
```

## <a name="additional-references"></a>Références supplémentaires

[Libérer de l’espace disque dans Windows 10](https://support.microsoft.com/en-us/help/12425/windows-10-free-up-drive-space)
