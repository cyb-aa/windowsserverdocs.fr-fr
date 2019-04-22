---
title: Stockage de fichiers avec MultiPoint Services
description: En savoir plus sur le stockage de fichiers dans MultiPoint Services
ms.custom: na
ms.date: 07/22/2016
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: c9eb0461-3846-4ddc-97ff-de10f03f30cf
author: evaseydl
manager: scottman
ms.author: evas
ms.openlocfilehash: b432ca793b156997761f9fadab7340c394e3b553
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59817340"
---
# <a name="storing-files-with-multipoint-services"></a>Stockage de fichiers avec MultiPoint Services
MultiPoint Services prend en charge le stockage de fichiers utilisateur comme suit :  
  
-   **Sur la partition de système d’exploitation du lecteur de disque dur.** Par défaut, MultiPoint Services stocke les fichiers utilisateur sur le lecteur de disque dur avec le système d’exploitation.  
  
-   **Sur une partition distincte du lecteur de disque dur.** Lorsque le système MultiPoint Services est configuré pour la première fois, vous pouvez *partition* le disque dur. Autrement dit, vous pouvez configurer une section du lecteur afin qu’elle fonctionne comme s’il s’agissait d’un lecteur distinct. Cela rend plus facile restaurer ou mettre à niveau le système d’exploitation sans affecter les fichiers de l’utilisateur. Pour plus d’informations, consultez [créer une partition ou un lecteur logique](https://go.microsoft.com/fwlink/?LinkId=182618) dans la bibliothèque technique Windows Server.  
  
-   **Sur un supplémentaire interne ou externe lecteur de disque dur.** Vous pouvez attacher supplémentaires internes ou externes lecteurs de disque dur à MultiPoint Services pour l’enregistrement et la sauvegarde des données.  
  
-   **Dans un dossier réseau partagé.** Pour rendre les fichiers de l’utilisateur disponible à partir de n’importe quelle station, vous pouvez créer un dossier partagé sur le réseau. Cela nécessite un autre ordinateur ou serveur, en plus de l’ordinateur qui exécute MultiPoint Services. Il s’agit de la méthode recommandée pour le stockage des fichiers si un serveur de fichiers est disponible.  
  
    Pour les petits systèmes d’ordinateurs 2 et 3 exécute MultiPoint Services avec aucun serveur de fichiers disponible, un des ordinateurs MultiPoint Services peut agir comme le serveur de fichiers pour tous les ordinateurs de MultiPoint Services. Vous créez des comptes d’utilisateur pour tous les utilisateurs sur les Services MultiPoint qui agit en tant que le serveur de fichiers.  
  
