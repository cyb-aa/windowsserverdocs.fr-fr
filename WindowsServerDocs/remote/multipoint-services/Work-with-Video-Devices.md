---
title: Utiliser des périphériques vidéo
description: Découvrez comment les moniteurs vidéo et les projecteurs fonctionnent avec les stations dans MultiPoint services
ms.prod: windows-server
ms.technology: multipoint-services
ms.topic: article
ms.assetid: 2f7f5a97-efd2-4184-8ad3-cf029d615eab
author: lizap
manager: dongill
ms.author: elizapo
ms.date: 08/04/2016
ms.openlocfilehash: 6b967d4523058fe1dfcb086e5918f84257bd51bf
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80820442"
---
# <a name="work-with-video-devices"></a>Utiliser des périphériques vidéo
Découvrez comment fonctionnent les périphériques vidéo, comme les moniteurs ou les projecteurs, quand ils sont connectés à un ordinateur de votre système MultiPoint Services ou une *station* MultiPoint Services.  
  
## <a name="working-with-video-monitors"></a>Utilisation des moniteurs vidéo  
En fonction du matériel de votre système MultiPoint Services, deux méthodes permettent de connecter un moniteur vidéo :  
  
-   Pour les *systèmes basés sur un concentrateur USB*, connectez le câble du moniteur vidéo à un port vidéo ouvert sur l’ordinateur, comme indiqué dans l’illustration suivante :  
  
    ![Image d’une connexion vidéo sur un système à concentrateur USB](./media/WMSVideoConnection.gif)  
  
-   Pour les *systèmes multifonctions Hub* avec prise en charge vidéo intégrée, connectez le câble du moniteur vidéo au port vidéo sur le concentrateur multifonction :  
  
    ![Image d’un concentrateur multifonction et d’une connexion vidéo](./media/WMSMultifunctionHubVideoConnection.gif)  
  
Pour plus d’informations, voir la rubrique [Installer une station](Set-Up-a-Station.md).  
  
## <a name="working-with-video-projectors"></a>Utilisation des projecteurs vidéo  
Vous pouvez connecter un projecteur vidéo à votre système MultiPoint Services quand vous voulez projeter une grande image pour qu’elle soit visible par d’autres personnes, par exemple dans le cadre d’un laboratoire. Pour les stations basées sur un concentrateur USB et multifonction, vous avez deux options pour connecter un projecteur à une station :  
  
-   Remplacez un moniteur par un projecteur et utilisez le projecteur comme périphérique d’affichage pour cette station, comme illustré ci-après :  
  
    ![Image d’un projecteur connecté à un ordinateur](./media/WMSVideoProjectorConnection.gif)  
  
-   Obtenez un appareil de séparation vidéo pour connecter un projecteur et un moniteur au port vidéo de la station.  
  
    MultiPoint Services affiche la même image sur les deux périphériques d’affichage. Quand vous ne projetez pas d’image, vous pouvez désactiver le projecteur et utiliser uniquement le moniteur vidéo.  
  
Quand vous utilisez l’une des deux méthodes, notez ce qui suit :  
  
-   La connexion d’un moniteur vidéo peut impliquer d’*associer la station* une nouvelle fois pour que MultiPoint Services puisse reconnaître le nouvel écran. Suivez les instructions qui s’affichent sur le périphérique d’affichage vidéo de la station.  
  
-   Vous devrez peut-être vous procurer un adaptateur ou un convertisseur pour procéder à la conversion des prises DVI et VGA.  
  
-   L’utilisation d’un câble de séparateur « Y » peut réduire la qualité vidéo sur les deux périphériques vidéo.  
  
-   Lorsque vous utilisez un projecteur et un moniteur à l’aide d’un câble de séparation « Y », MultiPoint services ajuste la résolution d’écran des deux appareils à la résolution maximale la plus basse de l’un des appareils, le plus souvent le projecteur.  
  
-   MultiPoint services ne prend pas en charge l’extension de l’affichage d’une seule station sur plusieurs moniteurs.  
  
## <a name="see-also"></a>Voir aussi  
[Gérer le matériel de la station](Manage-Station-Hardware.md)  
[Installer une station](Set-Up-a-Station.md) 