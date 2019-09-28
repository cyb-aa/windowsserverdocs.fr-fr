---
title: Stockage de fichiers avec MultiPoint Services
description: En savoir plus sur le stockage de fichiers dans MultiPoint services
ms.custom: na
ms.date: 07/22/2016
ms.prod: windows-server
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: c9eb0461-3846-4ddc-97ff-de10f03f30cf
author: evaseydl
manager: scottman
ms.author: evas
ms.openlocfilehash: bf31f5c582cffb5b38cff8cb15fcfdb4b3f76386
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71389301"
---
# <a name="storing-files-with-multipoint-services"></a>Stockage de fichiers avec MultiPoint Services
MultiPoint Services prend en charge le stockage des fichiers utilisateur des manières suivantes :  
  
-   **Sur la partition du système d’exploitation du lecteur de disque dur.** Par défaut, MultiPoint Services stocke les fichiers utilisateur sur le disque dur avec le système d’exploitation.  
  
-   **Sur une partition distincte du lecteur de disque dur.** Lorsque le système MultiPoint services est configuré pour la première fois, vous pouvez *partitionner* le disque dur. Autrement dit, vous pouvez configurer une section du lecteur de sorte qu’elle fonctionne comme s’il s’agissait d’un lecteur distinct. Cela facilite la restauration ou la mise à niveau du système d’exploitation sans affecter les fichiers utilisateur. Pour plus d’informations, voir [créer une partition ou un lecteur logique](https://go.microsoft.com/fwlink/?LinkId=182618) dans la bibliothèque technique Windows Server.  
  
-   **Sur un lecteur de disque dur interne ou externe supplémentaire.** Vous pouvez attacher des lecteurs de disque dur internes ou externes supplémentaires à MultiPoint services pour enregistrer et sauvegarder des données.  
  
-   **Dans un dossier réseau partagé.** Pour rendre les fichiers utilisateur disponibles à partir de n’importe quelle station, vous pouvez créer un dossier partagé sur le réseau. Cela nécessite un autre ordinateur ou serveur en plus de l’ordinateur qui exécute MultiPoint services. Il s’agit de la méthode recommandée pour stocker des fichiers si un serveur de fichiers est disponible.  
  
    Pour les petits systèmes de 2-3 ordinateurs exécutant MultiPoint services sans serveur de fichiers disponible, l’un des ordinateurs MultiPoint services peut agir en tant que serveur de fichiers pour tous les ordinateurs MultiPoint services. Vous créez ensuite des comptes d’utilisateur pour tous les utilisateurs de MultiPoint services qui agit en tant que serveur de fichiers.  
  
