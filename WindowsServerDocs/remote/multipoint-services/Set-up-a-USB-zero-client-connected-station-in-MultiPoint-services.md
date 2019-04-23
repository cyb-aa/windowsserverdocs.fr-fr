---
title: Configurer une clé USB zéro station client connecté dans MultiPoint Services
description: Découvrez comment créer une clé USB zéro station client dans MultiPoint Services
ms.custom: na
ms.date: 07/22/2016
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: d2908865-6be3-474d-88f1-995f40bb61d0
author: lizap
manager: dongill
ms.author: elizapo
ms.openlocfilehash: 1a64373f4ed5e0d1ac96a0257ac5697ff94ffcbe
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59878930"
---
# <a name="set-up-a-usb-zero-client-connected-station-in-multipoint-services"></a>Configurer une clé USB zéro station client connecté dans MultiPoint Services
Lorsque vous utilisez des clients USB zéro pour créer des stations MultiPoint Services, le moniteur pour chaque station est connecté au port vidéo sur le client USB zéro, comme indiqué dans l’illustration suivante. Pour plus d’informations sur cela et d’autres types de station, voir [Stations MultiPoint](MultiPoint-services-Stations.md).
  
**Système multiPoint Services avec un poste de vidéo-connectés directement et deux les stations USB zéro client connecté**  
  
![Stations USB connectées zéro client](./media/WMS11_diagram7.gif)  
  
> [!IMPORTANT]  
> Avant de configurer USB zéro stations client connecté, veillez à installer les pilotes les plus récents pour vos cartes vidéo et le client USB zéro. Pilotes obsolètes peuvent empêcher la configuration de MultiPoint Services de se terminer avec succès. Pour obtenir des instructions, consultez [mise à jour et installer des pilotes de périphérique si nécessaire](Update-and-install-device-drivers-if-needed.md).  
  
> [!IMPORTANT]  
> Si vous utilisez un client over-Ethernet USB zéro, suivez les instructions de votre fournisseur, au lieu de cette procédure, à utiliser la connexion Ethernet pour configurer l’appareil sur le réseau.  
  
### <a name="to-set-up-a-usb-zero-client-connected-station"></a>Comment configurer la clé USB zéro station client connecté  
  
1.  Se connecter le câble du moniteur vidéo au port vidéo DVI ou VGA sur le périphérique USB zéro client, comme indiqué dans l’illustration suivante.  
  
    ![Image d’une connexion vidéo sur un système à concentrateur USB](./media/WMSVideoConnection.gif)  
  
2.  Connecter le client USB zéro à un port USB ouvert sur l’ordinateur.  
  
    ![Image de connexion par concentrateur USB de Services MultiPoint](./media/WMSUSBHubConnection.gif)  
  
3.  Connecter un clavier et une souris au client USB zéro.  
  
    ![Image de connexion de périphériques d’entrée de concentrateur USB](./media/WMSUSBDeviceConnection.gif)  
  
4.  Si vous utilisez un client USB zéro alimenté de façon externe, connectez le cordon d’alimentation du client USB zéro pour une prise de courant.  
  
5.  Branchez le cordon d’alimentation du moniteur vidéo sur une prise.  
  
6.  Si vous êtes invité à associer des appareils à la station, suivez les instructions sur l’écran pour terminer l’installation. (En règle générale, USB zéro client connecté les stations sont associées stations automatiquement lorsque vous les ajoutez au serveur.)