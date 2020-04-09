---
title: Configurer une station USB connectée sans client dans MultiPoint services
description: Découvrez comment créer une station cliente zéro USB dans MultiPoint services
ms.date: 07/22/2016
ms.prod: windows-server
ms.technology: multipoint-services
ms.topic: article
ms.assetid: d2908865-6be3-474d-88f1-995f40bb61d0
author: lizap
manager: dongill
ms.author: elizapo
ms.openlocfilehash: 688b4908cd52dd53ba88f0a35cabccb58c289ba1
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80855642"
---
# <a name="set-up-a-usb-zero-client-connected-station-in-multipoint-services"></a>Configurer une station USB connectée sans client dans MultiPoint services
Lorsque vous utilisez des clients USB à zéro pour créer des stations MultiPoint services, le moniteur de chaque station est connecté au port vidéo sur le client USB Zero, comme indiqué dans l’illustration suivante. Pour plus d’informations sur ce type de station et les autres, consultez la page sur les [stations multipoint](MultiPoint-services-Stations.md).
  
**Système MultiPoint services avec une station connectée à une vidéo directe et deux stations USB connectées à zéro client**  
  
![Stations USB connectées zéro client](./media/WMS11_diagram7.gif)  
  
> [!IMPORTANT]  
> Avant de configurer les stations USB connectées à un client, veillez à installer les pilotes les plus récents pour vos cartes vidéo et le client Zero USB. Les pilotes obsolètes peuvent empêcher la configuration MultiPoint services de se terminer correctement. Pour obtenir des instructions, consultez [mettre à jour et installer des pilotes de périphérique si nécessaire](Update-and-install-device-drivers-if-needed.md).  
  
> [!IMPORTANT]  
> Si vous utilisez un client zéro USB sur Ethernet, suivez les instructions de votre fournisseur, au lieu de cette procédure, pour utiliser la connexion Ethernet afin de configurer l’appareil sur le réseau.  
  
### <a name="to-set-up-a-usb-zero-client-connected-station"></a>Pour configurer une station USB connectée sans client  
  
1.  Connectez le câble du moniteur vidéo au port d’affichage vidéo DVI ou VGA sur le client zéro USB, comme indiqué dans l’illustration suivante.  
  
    ![Image d’une connexion vidéo sur un système à concentrateur USB](./media/WMSVideoConnection.gif)  
  
2.  Connectez le client Zero USB à un port USB ouvert sur l’ordinateur.  
  
    ![Image de la connexion du concentrateur USB MultiPoint services](./media/WMSUSBHubConnection.gif)  
  
3.  Connectez un clavier et une souris au client USB Zero.  
  
    ![Image de connexion de périphériques d’entrée de concentrateur USB](./media/WMSUSBDeviceConnection.gif)  
  
4.  Si vous utilisez un client zéro USB alimenté en externe, connectez le cordon d’alimentation du client zéro USB à une prise d’alimentation.  
  
5.  Branchez le cordon d’alimentation du moniteur vidéo sur une prise.  
  
6.  Si vous êtes invité à associer des appareils à la station, suivez les instructions à l’écran pour terminer l’installation. (En règle générale, les stations USB connectées par le client ne sont associées à des stations automatiquement que lorsque vous les ajoutez au serveur.)