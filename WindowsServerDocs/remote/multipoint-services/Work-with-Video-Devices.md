---
title: Utiliser des périphériques vidéo
description: Découvrez comment la vidéo surveille et projecteurs fonctionnent avec les stations MultiPoint Services
ms.custom: na
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2f7f5a97-efd2-4184-8ad3-cf029d615eab
author: lizap
manager: dongill
ms.author: elizapo
ms.date: 08/04/2016
ms.openlocfilehash: d828ea911aaff27a1df79d0380dfe92987c3d2aa
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59844200"
---
# <a name="work-with-video-devices"></a>Utiliser des périphériques vidéo
Découvrez comment fonctionnent les périphériques vidéo, comme les moniteurs ou les projecteurs, quand ils sont connectés à un ordinateur de votre système MultiPoint Services ou une *station* MultiPoint Services.  
  
## <a name="working-with-video-monitors"></a>Utilisation des moniteurs vidéo  
En fonction du matériel de votre système MultiPoint Services, deux méthodes permettent de connecter un moniteur vidéo :  
  
-   Pour *systèmes concentrateur USB*, connectez le câble du moniteur vidéo à un port vidéo ouvert sur l’ordinateur, comme illustré dans l’illustration suivante :  
  
    ![Image d’une connexion vidéo sur un système à concentrateur USB](./media/WMSVideoConnection.gif)  
  
-   Pour *systèmes concentrateur multifonctions* avec prise en charge vidéo intégrée, connectez le câble du moniteur vidéo au port vidéo du concentrateur multifonction :  
  
    ![Image d’un concentrateur multifonction et d’une connexion vidéo](./media/WMSMultifunctionHubVideoConnection.gif)  
  
Pour plus d’informations, voir la rubrique [Installer une station](Set-Up-a-Station.md).  
  
## <a name="working-with-video-projectors"></a>Utilisation des projecteurs vidéo  
Vous pouvez connecter un projecteur vidéo à votre système MultiPoint Services quand vous voulez projeter une grande image pour qu’elle soit visible par d’autres personnes, par exemple dans le cadre d’un laboratoire. Pour les deux USB en fonction du hub et multifonction basée sur le hub stations, vous avez deux options pour connecter un projecteur à une station :  
  
-   Remplacez un moniteur par un projecteur et utilisez le projecteur comme périphérique d’affichage pour cette station, comme illustré ci-après :  
  
    ![Image d’un projecteur connecté à un ordinateur](./media/WMSVideoProjectorConnection.gif)  
  
-   Utilisez un périphérique vidéo de répartition pour connecter le projecteur et le moniteur au port vidéo de la station.  
  
    MultiPoint Services affiche la même image sur les deux périphériques d’affichage. Quand vous ne projetez pas d’image, vous pouvez désactiver le projecteur et utiliser uniquement le moniteur vidéo.  
  
Quand vous utilisez l’une des deux méthodes, notez ce qui suit :  
  
-   La connexion d’un moniteur vidéo peut impliquer d’*associer la station* une nouvelle fois pour que MultiPoint Services puisse reconnaître le nouvel écran. Suivez les instructions affichées sur le périphérique vidéo de la station.  
  
-   Vous devrez peut-être vous procurer un adaptateur ou un convertisseur pour procéder à la conversion des prises DVI et VGA.  
  
-   L’utilisation d’un câble répartiteur en « Y » peut diminuer la qualité vidéo des deux périphériques vidéo.  
  
-   Quand vous utilisez à la fois un projecteur et un moniteur via un câble répartiteur en « Y », MultiPoint Services ajuste la résolution d’écran des deux périphériques sur la résolution la plus basse de l’un des deux, généralement le projecteur.  
  
-   MultiPoint Services ne prend pas en charge l’extension de l’affichage d’une seule station sur plusieurs moniteurs.  
  
## <a name="see-also"></a>Voir aussi  
[Gérer le matériel de la Station](Manage-Station-Hardware.md)  
[Configuration d’une Station](Set-Up-a-Station.md) 