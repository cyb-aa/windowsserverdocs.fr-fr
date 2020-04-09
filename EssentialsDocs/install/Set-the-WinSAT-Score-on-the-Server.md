---
title: Définition du score WinSAT sur le serveur
description: Décrit comment utiliser Windows Server Essentials
ms.date: 10/03/2016
ms.prod: windows-server
ms.topic: article
ms.assetid: 911dc494-0f8f-4723-93d6-2106f914b906
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 2f469f902f28642890723552ac460e844281c7b8
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80819832"
---
# <a name="set-the-winsat-score-on-the-server"></a>Définition du score WinSAT sur le serveur

>S’applique à : Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Vous devez définir le score d’UC WinSAT pour un serveur qui exécute le système d’exploitation Windows Server Essentials afin d’optimiser la résolution de diffusion vidéo. Pour ce faire, il convient de créer et d’installer le fichier .xml contenant les indices de performance WinSAT.  
  
## <a name="obtain-the-winsat-cpu-score"></a>Obtention du score WinSAT du processeur  
 Le Kit de préinstallation OEM (OPK) est fourni avec un programme appelé WinServerSAT.exe. Celui-ci a pour fonction de relever la note du processeur obtenue au cours du test d’évaluation WinSAT et de placer cette information dans le fichier WinServerSAT.xml consultable par le système d’exploitation.  
  
#### <a name="to-obtain-the-winsat-cpu-score"></a>Pour obtenir le score du processeur lors de l’évaluation WinSAT  
  
1. Copiez le Resources\WinServerSAT\\* dans le support ADK sur l’ordinateur de référence.  
  
2. Sur l’ordinateur de référence, ouvrez une fenêtre d’invite de commandes avec élévation de privilèges.  
  
3. Si le dossier %ProgramFiles%\Windows Server\Bin\OEM n'existe pas, entrez la commande suivante et appuyez sur Entrée.  
  
    **mkdir "%ProgramFiles%\Windows dossier server\bin\oem"**  
  
4. Entrez la commande suivante et appuyez sur Entrée.  
  
    **WinServerSAT. exe "%ProgramFiles%\Windows Server\Bin\OEM\WinServerSAT.xml"**  
  
   L'exemple suivant montre les contenus XML du fichier WinServerSAT.xml créé.  
  
```  
  
<?xml version="1.0" encoding="UTF-16"?>  
<WinSAT>  
   <CpuScore description="WinSAT CPU score">WinSAT_Score</CpuScore>  
</WinSAT>  
```  
  
 *Score_WinSAT* représente la valeur relevée sur le serveur.  
  
> [!IMPORTANT]
>  Avant d'affectuer une capture de l’image, n’oubliez pas de supprimer les fichiers WinServerSAT.exe, winsat.prx, winsat.wmv et WinSATEncode.wmv de l’ordinateur de référence.
