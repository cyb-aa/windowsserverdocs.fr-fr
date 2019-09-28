---
title: bitsadmin create
description: La rubrique relative aux commandes Windows pour **Bitsadmin Create** -crée une tâche de transfert avec le nom complet donné.
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 9a8c53af-900b-4c24-9265-5b8b08213fac
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9f6d641d44c56ea4ff11f48a725367de7dcf472a
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71381810"
---
# <a name="bitsadmin-create"></a>bitsadmin create

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2 et Windows Server 2012

Crée une tâche de transfert avec le nom complet donné. Les travaux de téléchargement transfèrent les données d’un serveur vers un fichier local. Les travaux de chargement transfèrent les données d’un fichier local vers un serveur. Les travaux de chargement-réponse transfèrent les données d’un fichier local vers un serveur et reçoivent un fichier de réponse du serveur.

Utilisez le commutateur [Bitsadmin Resume](bitsadmin-resume.md) pour activer le travail dans la file d’attente de transfert.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /create [type] DisplayName
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|-------|--------|
|type|-    **/Download** transfère les données d’un serveur vers un fichier local.<br />-    **/upload** transfère les données d’un fichier local vers un serveur.<br />-    **/upload-reply** transfère les données d’un fichier local vers un serveur et reçoit un fichier de réponse du serveur.<br />-Ce paramètre est défini par défaut sur **/Download** lorsqu’il n’est pas spécifié sur la ligne de commande.|
|DisplayName|Nom complet affecté à la tâche nouvellement créée.|

**BITS 1,2 et versions antérieures**: Les types/upload et/Upload-Reply ne sont pas disponibles.

## <a name="BKMK_examples"></a>Illustre

Crée une tâche de téléchargement nommée *myDownloadJob*.

```
C:\>bitsadmin /create myDownloadJob
```

## <a name="additional-references"></a>Références supplémentaires

[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
