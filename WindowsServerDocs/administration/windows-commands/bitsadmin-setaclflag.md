---
title: bitsadmin setaclflag
description: La rubrique commandes Windows pour Bitsadmin setaclflag, qui définit les indicateurs de propagation de la liste de contrôle d’accès.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 6e3bcda0-827d-4dfd-8384-d1da018f3e10
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4ac47e554dde6a555e891d89668cd12fec3179d4
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80849672"
---
# <a name="bitsadmin-setaclflag"></a>bitsadmin setaclflag

Définit les indicateurs de propagation de la liste de contrôle d’accès (ACL) pour le travail. Les indicateurs indiquent que vous souhaitez conserver les informations relatives au propriétaire et à la liste de contrôle d’accès avec le fichier en cours de téléchargement. Par exemple, pour conserver le propriétaire et le groupe avec le fichier, affectez à **flags** la valeur `OG`.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /SetAclFlags <Job> <Flags>
```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|---------|-----------|
|Tâche|Nom complet ou GUID du travail|
|Flags|Spécifiez une ou plusieurs des valeurs d’indicateur suivantes :</br>-O : copie des informations de propriétaire avec le fichier.</br>-G : copie des informations de groupe avec le fichier.</br>-D : copie des informations DACL avec le fichier.</br>-S : copie des informations SACL avec le fichier.|

## <a name="remarks"></a>Notes

Le commutateur SetAclFlags est utilisé pour gérer les informations de propriétaire et de liste de contrôle d’accès lorsqu’un travail télécharge des données à partir d’un partage Windows (SMB).

## <a name="examples"></a><a name=BKMK_examples></a>Illustre

L’exemple suivant définit les indicateurs de propagation de la liste de contrôle d’accès pour le travail nommé *myDownloadJob* afin de conserver les informations de propriétaire et de groupe avec les fichiers téléchargés.
```
C:\>bitsadmin /setaclflags myDownloadJob OG
```

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)