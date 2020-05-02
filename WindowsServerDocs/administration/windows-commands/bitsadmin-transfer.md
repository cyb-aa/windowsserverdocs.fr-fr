---
title: bitsadmin transfer
description: Rubrique de référence pour la commande Bitsadmin Transfer, qui transfère un ou plusieurs fichiers.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: fe302141-b33a-4a05-835e-dc4fc4db7d5a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9c7011f3ef3e85d7453e63d9a9c2e4a89a52cddf
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82707755"
---
# <a name="bitsadmin-transfer"></a>bitsadmin transfer

Transfère un ou plusieurs fichiers. Par défaut, le service BITSAdmin crée un travail de téléchargement qui s’exécute à la priorité **normale** et met à jour la fenêtre de commande avec les informations de progression jusqu’à ce que le transfert soit terminé ou qu’une erreur critique se produise.

Le service termine le travail s’il transfère avec succès tous les fichiers et annule le travail si une erreur critique se produit. Le service ne crée pas le travail s’il n’est pas en mesure d’ajouter des fichiers au travail ou si vous spécifiez une valeur non valide pour le *type* ou le *job_priority*. Pour transférer plusieurs fichiers, spécifiez plusieurs `<RemoteFileName>-<LocalFileName>` paires. Les paires doivent être délimitées par des espaces.

> [!NOTE]
> La commande BITSAdmin continue à s’exécuter si une erreur temporaire se produit. Pour mettre fin à la commande, appuyez sur CTRL + C.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /transfer <name> [<type>] [/priority <job_priority>] [/ACLflags <flags>] [/DYNAMIC] <remotefilename> <localfilename>
```

### <a name="parameters"></a>Paramètres

| Paramètre | Description |
| --------- | ----------- |
| name | Nom du travail. Cette commande ne peut pas être un GUID. |
| type | facultatif. Définit le type de travail, notamment :<ul><li>**Downloader.** Valeur par défaut. Choisissez ce type pour les travaux de téléchargement.</li><li>**Amont.** Choisissez ce type pour les travaux de téléchargement.</li></ul> |
| priority | facultatif. Définit la priorité du travail, y compris :<ul><li>FOREGROUND (avant-plan)</li><li>HIGH</li><li>NORMAL</li><li>LOW</li></ul> |
| ACLflags | facultatif. Indique que vous souhaitez conserver les informations relatives au propriétaire et à la liste de contrôle d’accès avec le fichier en cours de téléchargement. Spécifiez une ou plusieurs des valeurs, y compris :<ul><li>**o** -copier les informations de propriétaire avec le fichier.</li><li>**g** -copier les informations de groupe avec le fichier.</li><li>**d** -copie les informations discrétionnaires sur la liste de contrôle d’accès (DACL) avec le fichier.</li><li>**s** -copier les informations de la liste de contrôle d’accès système (SACL) avec le fichier.</li></ul> |
| /DYNAMIC | Configure la tâche à l’aide de [**BITS_JOB_PROPERTY_DYNAMIC_CONTENT**](https://docs.microsoft.com/windows/win32/api/bits5_0/ne-bits5_0-bits_job_property_id), ce qui assouplit les exigences côté serveur. |
| remotefilename | Nom du fichier après qu’il a été transféré vers le serveur. |
| localfilename | Nom du fichier qui réside localement. |

## <a name="examples"></a>Exemples

Pour démarrer une tâche de transfert nommée *myDownloadJob*:

```
bitsadmin /transfer myDownloadJob http://prodserver/audio.wma c:\downloads\audio.wma
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)

- [commande Bitsadmin](bitsadmin.md)
