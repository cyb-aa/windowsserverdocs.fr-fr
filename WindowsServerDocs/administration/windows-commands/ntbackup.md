---
title: ntbackup
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 6bce6b0d-646b-46b6-b833-0ff1d6f082c2
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 783f73eba2aeaf9f30c5c1e12a623f1f87f24ede
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59846340"
---
# <a name="ntbackup"></a>ntbackup



Le **ntbackup** commande n’est pas disponible dans Windows Vista ou Windows Server 2008. Au lieu de cela, vous devez utiliser le **wbadmin** sous-commandes pour sauvegarder et restaurer votre ordinateur et les fichiers à partir d’une invite de commandes et la commande.

Vous ne pouvez pas récupérer des sauvegardes créées avec **ntbackup** à l'aide de **wbadmin**. Toutefois, une version de **ntbackup** est disponible en téléchargement pour les utilisateurs de Windows Server 2008 et Windows Vista qui souhaitent récupérer des sauvegardes créées à l’aide de **ntbackup**. Cette version téléchargeable de **ntbackup** active vous permettent d’effectuer des récupérations uniquement de sauvegardes héritées et il ne peut pas être utilisé pour créer des sauvegardes sur les ordinateurs exécutant Windows Server 2008 ou Windows Vista. Pour télécharger cette version de **ntbackup**, consultez [ https://go.microsoft.com/fwlink/?LinkId=82917 ](https://go.microsoft.com/fwlink/?LinkId=82917).

#### <a name="additional-references"></a>Références supplémentaires

[Clé de la syntaxe de ligne de commande](command-line-syntax-key.md)

[Wbadmin](wbadmin.md)