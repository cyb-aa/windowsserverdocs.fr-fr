---
title: Définition du score WinSAT sur le serveur
description: Décrit comment utiliser Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 911dc494-0f8f-4723-93d6-2106f914b906
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 4e5ce037c7a8c802419cd980fc0272c4f687c6a6
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66433453"
---
# <a name="set-the-winsat-score-on-the-server"></a>Définition du score WinSAT sur le serveur

>S'applique à : Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Vous devez définir le score WinSAT du processeur pour un serveur qui exécute le système d’exploitation Windows Server Essentials pour optimiser la résolution vidéo de diffusion en continu. Pour ce faire, il convient de créer et d’installer le fichier .xml contenant les indices de performance WinSAT.  
  
## <a name="obtain-the-winsat-cpu-score"></a>Obtention du score WinSAT du processeur  
 Le Kit de préinstallation OEM (OPK) est fourni avec un programme appelé WinServerSAT.exe. Celui-ci a pour fonction de relever la note du processeur obtenue au cours du test d’évaluation WinSAT et de placer cette information dans le fichier WinServerSAT.xml consultable par le système d’exploitation.  
  
#### <a name="to-obtain-the-winsat-cpu-score"></a>Pour obtenir le score du processeur lors de l’évaluation WinSAT  
  
1. Copier le Resources\WinServerSAT\\* dans le support ADK sur l’ordinateur de référence.  
  
2. Sur l’ordinateur de référence, ouvrez une fenêtre d’invite de commandes avec élévation de privilèges.  
  
3. Si le dossier %ProgramFiles%\Windows Server\Bin\OEM n'existe pas, entrez la commande suivante et appuyez sur Entrée.  
  
    **mkdir "%ProgramFiles%\Windows Server\Bin\OEM"**  
  
4. Entrez la commande suivante et appuyez sur Entrée.  
  
    **WinServerSAT.exe "%ProgramFiles%\Windows Server\Bin\OEM\WinServerSAT.xml"**  
  
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
