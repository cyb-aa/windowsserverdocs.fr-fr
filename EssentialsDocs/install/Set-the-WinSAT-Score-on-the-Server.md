---
title: "Définition du Score WinSAT sur le serveur"
description: "Décrit comment utiliser WindowsServerEssentials"
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
ms.openlocfilehash: 77866acccac13ac48da8779700c8654f2c7f3277
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2017
---
# <a name="set-the-winsat-score-on-the-server"></a>Définition du Score WinSAT sur le serveur

>S’applique à: Windows Server2016Essentials, Windows Server2012R2 Essentials, Windows Server2012Essentials

Vous devez définir le score WinSAT du processeur pour un serveur qui exécute le système d’exploitation WindowsServerEssentials pour optimiser la résolution de diffusion vidéo. Pour cela, en créant et en installant le fichier .xml contenant les informations du score WinSAT.  
  
## <a name="obtain-the-winsat-cpu-score"></a>Obtenir le score WinSAT du processeur  
 Vous disposez d’un programme avec le Kit de préinstallation OEM appelé WinServerSAT.exe qui détecte le score WinSAT du processeur et de placer cette information dans le fichier WinServerSAT.xml qui lit le système d’exploitation.  
  
#### <a name="to-obtain-the-winsat-cpu-score"></a>Pour obtenir le score WinSAT du processeur  
  
1.  Copiez Resources\WinServerSAT\\ * dans le support ADK sur l’ordinateur de référence.  
  
2.  Sur l’ordinateur de référence, ouvrez une fenêtre d’invite de commandes avec élévation de privilèges.  
  
3.  Si le dossier %ProgramFiles%\Windows Server\Bin\OEM n’existe pas, tapez la commande suivante et appuyez sur ENTRÉE.  
  
     **Mkdir «%ProgramFiles%\Windows Server\Bin\OEM»**  
  
4.  Tapez la commande suivante et appuyez sur ENTRÉE.  
  
     **WinServerSAT.exe «%ProgramFiles%\Windows Server\Bin\OEM\WinServerSAT.xml»**  
  
 L’exemple suivant montre le contenu XML du fichier WinServerSAT.xml créé.  
  
```  
  
<?xml version="1.0" encoding="UTF-16"?>  
<WinSAT>  
   <CpuScore description="WinSAT CPU score">WinSAT_Score</CpuScore>  
</WinSAT>  
```  
  
 Où *Score_winsat* est remplacé par la valeur relevée sur le serveur.  
  
> [!IMPORTANT]
>  Vous devez supprimer les fichiers WinServerSAT.exe, winsat.prx, winsat.wmv et WinSATEncode.wmv de l’ordinateur de référence avant de capturer l’image.
