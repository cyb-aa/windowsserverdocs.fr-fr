---
title: ntbackup
description: Rubrique relative aux commandes Windows pour * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 6bce6b0d-646b-46b6-b833-0ff1d6f082c2
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b68b4cca579a5fc27f921ce2f4fcc6976d8e5600
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80838012"
---
# <a name="ntbackup"></a>ntbackup



La commande **ntbackup** n’est pas disponible dans Windows Vista ou windows Server 2008. Au lieu de cela, vous devez utiliser la commande et les sous-commandes **Wbadmin** pour sauvegarder et restaurer votre ordinateur et vos fichiers à partir d’une invite de commandes.

Vous ne pouvez pas récupérer des sauvegardes créées avec **ntbackup** à l'aide de **wbadmin**. Toutefois, une version de **ntbackup** est disponible en téléchargement pour les utilisateurs de windows Server 2008 et Windows Vista qui souhaitent récupérer des sauvegardes créées à l’aide de **ntbackup**. Cette version téléchargeable de **ntbackup** vous permet d’effectuer des récupérations uniquement des sauvegardes héritées et ne peut pas être utilisée sur des ordinateurs exécutant windows Server 2008 ou Windows Vista pour créer de nouvelles sauvegardes. Pour télécharger cette version de **ntbackup**, consultez [https://go.microsoft.com/fwlink/?LinkId=82917](https://go.microsoft.com/fwlink/?LinkId=82917).

## <a name="additional-references"></a>Références supplémentaires

- [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)

[Wbadmin](wbadmin.md)