---
title: bitsadmin create
description: Rubrique de commandes de Windows pour **bitsadmin créer** -crée une tâche de transfert avec le nom d’affichage donné.
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: d6ce5a4fdc21d879bf0a265e3c4185d83311464a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59817190"
---
# <a name="bitsadmin-create"></a>bitsadmin create

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Crée une tâche de transfert avec le nom d’affichage donné. Télécharger les données de transfert de travaux à partir d’un serveur dans un fichier local. Télécharger des travaux transférer des données à partir d’un fichier local à un serveur. Travaux de chargement-réponse transférer des données à partir d’un fichier local vers un serveur et recevoir un fichier réponse du serveur.

Utilisez le [bitsadmin resume](bitsadmin-resume.md) commutateur pour activer la tâche dans la file d’attente de transfert.

## <a name="syntax"></a>Syntaxe

```
bitsadmin /create [type] DisplayName
```

## <a name="parameters"></a>Paramètres

|Paramètre|Description|
|-------|--------|
|type|-   **/ Télécharger** transfère les données à partir d’un serveur vers un fichier local.<br />-   **/ Télécharger** transfère des données à partir d’un fichier local vers un serveur.<br />-   **/ Chargement-réponse** transfère des données à partir d’un fichier local vers un serveur et reçoit un fichier réponse du serveur.<br />-Par défaut est ce paramètre **/télécharger** lorsque ne pas spécifié sur la ligne de commande.|
|DisplayName|Le nom complet assigné à la tâche qui vient d’être créée.|

**BITS 1.2 et versions antérieures**: Les types de /Upload-Reply et un/Upload ne sont pas disponibles.

## <a name="BKMK_examples"></a>Exemples

Crée une tâche de téléchargement nommée *myDownloadJob*.

```
C:\>bitsadmin /create myDownloadJob
```

## <a name="additional-references"></a>Références supplémentaires

[Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)
