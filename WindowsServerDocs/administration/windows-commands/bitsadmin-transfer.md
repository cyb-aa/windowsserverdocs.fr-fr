---
title: bitsadmin Transfer
description: Rubrique relative aux commandes Windows pour **Bitsadmin Transfer** -transfère un ou plusieurs fichiers.
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: fe302141-b33a-4a05-835e-dc4fc4db7d5a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2a12e6e2023c979d5b0c095c1eddd77eb5155d1e
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71380352"
---
# <a name="bitsadmin-transfer"></a>bitsadmin Transfer

Transfère un ou plusieurs fichiers. Pour transférer plusieurs fichiers, spécifiez plusieurs paires \<RemoteFileName\>-\<LocalFileName\>. Les paires sont délimitées par des espaces.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /Transfer <Name> [<Type>] [/Priority <Job_Priority>] [/ACLFlags <Flags>] [/DYNAMIC] <RemoteFileName> <LocalFileName>
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|Nom|Nom du travail. Contrairement à la plupart des commandes, **Name** ne peut être qu’un nom et non un GUID.|
|Type|Facultatif : spécifiez le type de travail. Utilisez **/Download** (valeur par défaut) pour un travail de téléchargement ou **/upload** pour un travail de chargement.|
|Priority|Facultatif : définissez le job_priority sur l’une des valeurs suivantes :</br>-PREMIER plan</br>-ÉLEVÉ</br>-NORMAL</br>-FAIBLE|
|ACLFlags|Facultatif : indique que vous souhaitez conserver les informations relatives au propriétaire et à la liste de contrôle d’accès avec le fichier en cours de téléchargement. Par exemple, pour conserver le propriétaire et le groupe avec le fichier, affectez à indicateurs la valeur `OG`. Spécifiez un ou plusieurs des indicateurs suivants :</br>-O : copie des informations de propriétaire avec le fichier.</br>-G : copie des informations de groupe avec le fichier.</br>-D : copie des informations DACL avec le fichier.</br>-S : copie des informations SACL avec le fichier.|
|\/dynamique|Configure le travail avec [**BITS_JOB_PROPERTY_DYNAMIC_CONTENT**](/windows/desktop/api/bits5_0/ne-bits5_0-bits_job_property_id), ce qui assouplit les exigences côté serveur.|
|RemoteFileName|Nom du fichier en cas de transfert vers le serveur.|
|LocalFileName|Nom du fichier qui réside localement.|

## <a name="remarks"></a>Notes

Par défaut, le service BITSAdmin crée un travail de téléchargement qui s’exécute à la priorité **normale** et met à jour la fenêtre de commande avec les informations de progression jusqu’à ce que le transfert soit terminé ou qu’une erreur critique se produise. Le service termine le travail s’il transfère avec succès tous les fichiers et annule le travail si une erreur critique se produit. Le service ne crée pas le travail s’il n’est pas en mesure d’ajouter des fichiers au travail ou si vous spécifiez une valeur non valide pour le *type* ou le *Job_Priority*. Pour transférer plusieurs fichiers, spécifiez plusieurs paires *RemoteFileName*-*localFileName* . Les paires sont délimitées par des espaces.

> [!NOTE]
> La commande BITSAdmin continue à s’exécuter si une erreur temporaire se produit. Pour mettre fin à la commande, appuyez sur CTRL + C.

## <a name="BKMK_examples"></a>Illustre

L’exemple suivant démarre une tâche de transfert avec nommé *myDownloadJob*.
```
C:\>bitsadmin /Transfer myDownloadJob http://prodserver/audio.wma c:\downloads\audio.wma
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)