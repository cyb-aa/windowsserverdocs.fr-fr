---
title: bitsadmin Transfer
description: Rubrique de commandes de Windows pour **bitsadmin transfert** -transfère un ou plusieurs fichiers.
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 2ef29a242a834fae42d1de3994a82aedcf87ec2d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59852460"
---
# <a name="bitsadmin-transfer"></a>bitsadmin Transfer

Transfère un ou plusieurs fichiers. Pour transférer plusieurs fichiers, spécifiez plusieurs \<RemoteFileName\>-\<LocalFileName\> paires. Les paires sont délimitées par des espaces.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /Transfer <Name> [<Type>] [/Priority <Job_Priority>] [/ACLFlags <Flags>] [/DYNAMIC] <RemoteFileName> <LocalFileName>
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|Nom|Le nom de la tâche. Contrairement à la plupart des commandes, **nom** peut uniquement être un nom et pas un GUID.|
|Type|Facultatif : spécifiez le type de travail. Utilisez **/télécharger** (la valeur par défaut) pour une tâche de téléchargement ou **/télécharger** pour une tâche de téléchargement.|
|Priority|Facultatif : définir le job_priority à une des valeurs suivantes :</br>-PREMIER PLAN</br>-ÉLEVÉ</br>-   NORMAL</br>-   LOW|
|ACLFlags|Facultatif : indique que vous souhaitez conserver le propriétaire et les informations ACL avec le fichier en cours de téléchargement. Par exemple, pour conserver le propriétaire et le groupe avec le fichier, définir des indicateurs `OG`. Spécifiez un ou plusieurs des indicateurs suivants :</br>-O: Copier les informations relatives au propriétaire avec le fichier.</br>-G : Copier les informations de groupe avec le fichier.</br>-D: Copier les informations de la DACL par fichier.</br>-S: Copier les informations de la liste SACL avec le fichier.|
|\/DYNAMIC|Configure la tâche avec [ **BITS_JOB_PROPERTY_DYNAMIC_CONTENT**](/windows/desktop/api/bits5_0/ne-bits5_0-bits_job_property_id), lequel assouplit la configuration requise côté serveur.|
|RemoteFileName|Le nom du fichier lors du transfert vers le serveur.|
|LocalFileName|Le nom du fichier qui réside en local.|

## <a name="remarks"></a>Notes

Par défaut, le service BITSAdmin crée une tâche de téléchargement qui s’exécute à **NORMAL** priorité et met à jour de la fenêtre de commande avec informations sur la progression jusqu'à ce que le transfert est terminé ou jusqu'à ce qu’une erreur critique se produit. Le service a terminé le travail s’il a été de transfère tous les fichiers et annule le travail si une erreur critique se produit. Le service ne crée pas le travail s’il est impossible d’ajouter des fichiers au travail ou si vous spécifiez une valeur non valide pour *Type* ou *Job_Priority*. Pour transférer plusieurs fichiers, spécifiez plusieurs *RemoteFileName*-*LocalFileName* paires. Les paires sont délimitées par des espaces.

> [!NOTE]
> La commande BITSAdmin continue à s’exécuter si une erreur temporaire se produit. Pour mettre fin à la commande, appuyez sur CTRL + C.

## <a name="BKMK_examples"></a>Exemples

L’exemple suivant démarre un transfert du travail avec nommé *myDownloadJob*.
```
C:\>bitsadmin /Transfer myDownloadJob http://prodserver/audio.wma c:\downloads\audio.wma
```

#### <a name="additional-references"></a>Références supplémentaires

[Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)