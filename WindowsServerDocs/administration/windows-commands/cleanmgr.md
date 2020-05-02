---
title: cleanmgr
description: Configurez l’outil Nettoyage de disque (Cleanmgr. exe) pour nettoyer automatiquement certains fichiers.
ms.prod: windows-server
ms.reviewer: cosmosdarwin
author: iangpgh
ms.author: jgerend
manager: daveba
ms.technology: storage-spaces
ms.date: 06/20/2019
ms.openlocfilehash: 49d85fe0c8ec1bbba810a502724fd7aac0c2f55d
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82712848"
---
# <a name="cleanmgr"></a>cleanmgr

> S’applique à : Windows Server 2019, Windows Server 2016, Windows Server 2012, Windows Server 2008 R2, Windows Server (canal semi-annuel)

Efface les fichiers inutiles du disque dur de votre ordinateur. Vous pouvez utiliser les options de ligne de commande pour spécifier que **cleanmgr** nettoie les fichiers temporaires, les fichiers Internet, les fichiers téléchargés et les fichiers de la corbeille. Vous pouvez ensuite planifier la tâche pour qu’elle s’exécute à une heure spécifique à l’aide de l’outil **tâches planifiées** .

## <a name="syntax"></a>Syntaxe

```
cleanmgr [/d <driveletter>] [/sageset:n]  [/sagerun:n] [/TUNEUP:n] [/LOWDISK] [/VERYLOWDISK]
```

### <a name="parameters"></a>Paramètres

| Paramètre | Description |
| --------- | ----------- |
| /d`<driveletter>` | Spécifie le lecteur dont vous souhaitez nettoyer le nettoyage de disque.<p>**Remarque :** L’option **/d** n’est pas utilisée avec `/sagerun:n`. |
| /sageset : n | Affiche la boîte de dialogue **paramètres de nettoyage de disque** et crée également une clé de Registre pour stocker les paramètres que vous sélectionnez. La `n` valeur, qui est stockée dans le registre, vous permet de spécifier les tâches à exécuter pour le nettoyage de disque. La `n` valeur peut être toute valeur entière comprise entre 0 et 65535. |
| /sagerun : n | Exécute les tâches spécifiées qui sont assignées à la valeur n Si vous utilisez l’option **\sageset** . Tous les lecteurs de l’ordinateur sont énumérés et le profil sélectionné s’exécute sur chaque lecteur. |
| /TuneUp : n | Exécutez **/sageset** et **/sagerun** pour le même `n` . |
| /lowdisk | Exécutez avec les paramètres par défaut. |
| /verylowdisk | Exécutez avec les paramètres par défaut, sans invite utilisateur. |
| /? | Affiche l'aide à l'invite de commandes. |

#### <a name="options"></a>Options

Les options pour les fichiers que vous pouvez spécifier pour le nettoyage de disque à l’aide de **/sageset** et **/sagerun** sont les suivantes :

- **Fichiers d’installation temporaires** : fichiers créés par un programme d’installation qui n’est plus en cours d’exécution.

- **Fichiers programmes téléchargés** : les fichiers programmes téléchargés sont des contrôles ActiveX et des programmes Java qui sont téléchargés automatiquement à partir d’Internet lorsque vous affichez certaines pages. Ces fichiers sont temporairement stockés dans le dossier Program Files téléchargé sur le disque dur. Cette option comprend un bouton afficher les fichiers qui vous permet de voir les fichiers avant que le nettoyage de disque ne les supprime. Le bouton ouvre le dossier Program Files C:\Winnt\Downloaded.

- **Fichiers Internet temporaires** : le dossier Temporary Internet Files contient les pages Web qui sont stockées sur votre disque dur pour une consultation rapide. Le nettoyage de disque supprime cette page, mais laisse vos paramètres personnalisés pour les pages Web intacts. Cette option comprend également un bouton afficher les fichiers, qui ouvre le dossier C:\Documents and Settings\Username\Local Settings\Temporary Internet Files\Content.IE5.

- **Anciens fichiers CHKDSK** -lorsque CHKDSK recherche des erreurs sur un disque, CHKDSK peut enregistrer les fragments de fichiers perdus en tant que fichiers dans le dossier racine sur le disque. Ces fichiers ne sont pas nécessaires.

- **Corbeille** : la corbeille contient des fichiers que vous avez supprimés de l’ordinateur. Ces fichiers ne sont pas supprimés définitivement tant que vous ne videz pas la corbeille. Cette option comprend un bouton afficher les fichiers qui ouvre la corbeille.<p>**Remarque :** Une corbeille peut apparaître dans plusieurs lecteurs, par exemple, pas seulement dans% SystemRoot%.

- **Fichiers temporaires** : les programmes stockent parfois des informations temporaires dans un dossier temporaire. Avant la fermeture d’un programme, le programme supprime généralement ces informations. Vous pouvez supprimer en toute sécurité les fichiers temporaires qui n’ont pas été modifiés au cours de la semaine dernière.

- **Fichiers hors connexion temporaires** -les fichiers hors connexion temporaires sont des copies locales des fichiers réseau récemment utilisés. Ces fichiers sont automatiquement mis en cache afin que vous puissiez les utiliser une fois que vous vous êtes déconnecté du réseau. Un bouton **afficher les fichiers** ouvre le dossier fichiers hors connexion.

- **Fichiers hors connexion** -les fichiers hors connexion sont des copies locales de fichiers réseau que vous souhaitez utiliser en mode hors connexion afin de pouvoir les utiliser une fois que vous vous êtes déconnecté du réseau. Un bouton **afficher les fichiers** ouvre le dossier fichiers hors connexion.

- **Compresser les anciens fichiers** -Windows peut compresser les fichiers que vous n’avez pas utilisés récemment. La compression de fichiers économise de l’espace disque, mais vous pouvez toujours utiliser les fichiers. Aucun fichier n'est supprimé. Étant donné que les fichiers sont compressés à des taux différents, la quantité d’espace disque que vous obtiendrez est approximative. Un bouton Options vous permet de spécifier le nombre de jours à attendre avant que le nettoyage de disque compresse un fichier inutilisé.

- **Fichiers de catalogue pour l’indexeur de contenu** : le service d’indexation accélère et améliore les recherches de fichiers en conservant un index des fichiers qui se trouvent sur le disque. Ces fichiers de catalogue sont conservés à partir d’une opération d’indexation précédente et peuvent être supprimés en toute sécurité.<p>**Remarque :** Un fichier catalogue peut apparaître dans plusieurs lecteurs, par exemple, pas seulement dans `%SystemRoot%`.

>[!NOTE]
> Si vous spécifiez le nettoyage du lecteur contenant l’installation de Windows, toutes ces options sont disponibles dans l’onglet **nettoyage de disque** . Si vous spécifiez un autre lecteur, seules les options corbeille et fichiers catalogue pour l’index de contenu sont disponibles dans l’onglet **nettoyage de disque** .

## <a name="examples"></a>Exemples

Pour exécuter l’application de nettoyage de disque afin de pouvoir utiliser sa boîte de dialogue pour spécifier les options à utiliser ultérieurement, en enregistrant les paramètres dans le jeu **1**, tapez la commande suivante :

```
cleanmgr /sageset:1
```

Pour exécuter le nettoyage de disque et inclure les options que vous avez spécifiées à l’aide de la commande cleanmgr/sageset : 1, tapez :

```
cleanmgr /sagerun:1
```

Pour exécuter `cleanmgr /sageset:1` et `cleanmgr /sagerun:1` ensemble, tapez :

```
cleanmgr /tuneup:1
```

## <a name="additional-references"></a>Références supplémentaires

- [Libérer de l’espace disque dans Windows 10](https://support.microsoft.com/help/12425/windows-10-free-up-drive-space)

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
