---
title: ntbackup
description: 'Rubrique relative aux commandes Windows pour * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: ebbe71fd5547311beb36747d32d695823e0f0059
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71372681"
---
# <a name="ntbackup"></a>ntbackup



La commande **ntbackup** n’est pas disponible dans Windows Vista ou windows Server 2008. Au lieu de cela, vous devez utiliser la commande et les sous-commandes **Wbadmin** pour sauvegarder et restaurer votre ordinateur et vos fichiers à partir d’une invite de commandes.

Vous ne pouvez pas récupérer des sauvegardes créées avec **ntbackup** à l'aide de **wbadmin**. Toutefois, une version de **ntbackup** est disponible en téléchargement pour les utilisateurs de windows Server 2008 et Windows Vista qui souhaitent récupérer des sauvegardes créées à l’aide de **ntbackup**. Cette version téléchargeable de **ntbackup** vous permet d’effectuer des récupérations uniquement des sauvegardes héritées et ne peut pas être utilisée sur des ordinateurs exécutant windows Server 2008 ou Windows Vista pour créer de nouvelles sauvegardes. Pour télécharger cette version de **ntbackup**, consultez [https://go.microsoft.com/fwlink/?LinkId=82917](https://go.microsoft.com/fwlink/?LinkId=82917).

#### <a name="additional-references"></a>Références supplémentaires

[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)

[Wbadmin](wbadmin.md)