---
title: Problèmes de performances ou d’application pendant une connexion Bureau à distance
description: Résolution des problèmes de performances ou d’application pendant une connexion Bureau à distance.
ms.reviewer: rklemen
ms.topic: troubleshooting
author: kaushika-msft
manager: dcscontentpm
ms.author: delhan
ms.date: 07/24/2019
ms.localizationpriority: medium
ms.openlocfilehash: 91ced9e729966ee9c46e76d01d7ccbec9a510f5b
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80857222"
---
# <a name="poor-performance-or-application-problems-during-remote-desktop-connection"></a>Problèmes de performances ou d’application pendant une connexion Bureau à distance

Cet article aborde plusieurs problèmes courants que les utilisateurs peuvent rencontrer quand ils utilisent la fonctionnalité Bureau à distance.

### <a name="intermittent-problems-with-new-microsoft-azure-virtual-machines"></a>Problèmes intermittents avec les nouvelles machines virtuelles Microsoft Azure

Ce problème affecte les machines virtuelles provisionnées récemment. Une fois que l’utilisateur s’est connecté à la machine virtuelle, la session Bureau à distance ne charge pas correctement tous les paramètres de l’utilisateur.

Pour contourner ce problème, déconnectez-vous de la machine virtuelle, attendez au moins 20 minutes, puis reconnectez-vous.

Pour résoudre ce problème, appliquez les mises à jour suivantes aux machines virtuelles :

  - Windows 10 et Windows Server 2016 KB 4343884, [30 août 2018—KB4343884 (build du système d’exploitation 14393.2457)](https://support.microsoft.com/help/4343884/windows-10-update-kb4343884)
  - Windows Server 2012 R2 : KB 4343891, [30 août 2018—KB4343891 (préversion du correctif cumulatif mensuel)](https://support.microsoft.com/help/4343891/windows-81-update-kb4343891)

### <a name="video-playback-issues-on-windows-10-version-1709"></a>Problèmes de lecture vidéo sur Windows 10 version 1709

Ce problème se produit quand des utilisateurs se connectent à des ordinateurs distants qui exécutent Windows 10 version 1709. Quand ces utilisateurs lisent une vidéo à l’aide du codec VMR9 (Video Mixing Renderer 9), le lecteur n’affiche qu’une fenêtre noire.

Il s’agit d’un problème connu dans Windows 10 version 1709. Ce problème ne se produit pas dans Windows 10 version 1703.

### <a name="desktop-sharing-issues-on-windows-10"></a>Problèmes de partage de Bureau sur Windows 10

Ce problème se produit quand l’utilisateur a un profil utilisateur en lecture seule (et une ruche du Registre associée), comme dans un scénario de kiosque. Dans ce cas, quand l’utilisateur se connecte à un ordinateur distant qui exécute Windows 10 version 1803, il ne peut pas partager son Bureau.

Pour résoudre ce problème, appliquez la mise à jour Windows 10 4340917, [24 juillet 2018—KB4340917 (build du système d’exploitation 17134.191)](https://support.microsoft.com/help/4340917/windows-10-update-kb4340917).

### <a name="performance-issues-when-mixing-versions-of-windows-10-if-nla-is-disabled"></a>Problèmes de performances quand vous combinez des versions de Windows 10 si l’authentification au niveau du réseau est désactivée

Ce problème se produit quand l’authentification au niveau du réseau est désactivée et que des ordinateurs clients Bureau à distance qui exécutent Windows 10 se connectent à des Bureaux à distance qui exécutent des versions différentes de Windows 10. Les utilisateurs de clients Bureau à distance sur des ordinateurs qui exécutent Windows 10 version 1709 ou antérieure constatent une dégradation des performances quand ils se connectent à des Bureaux à distance qui exécutent Windows 10 version 1803 ou ultérieure.

Cela se produit car, lorsque l’authentification au niveau du réseau est désactivée, les ordinateurs clients plus anciens utilisent un protocole plus lent quand ils se connectent à Windows 10 version 1803 ou ultérieure.

Pour résoudre ce problème, appliquez le correctif KB 4340917, [24 juillet 2018—KB4340917 (build du système d’exploitation 17134.191)](https://support.microsoft.com/help/4340917/windows-10-update-kb4340917).

### <a name="black-screen-issue"></a>Problème d’écran noir

Ce problème se produit dans Windows 8.0, Windows 8.1, Windows 10 RTM et Windows Server 2012 R2. Un utilisateur lance plusieurs applications dans un Bureau à distance, puis se déconnecte de la session. L’utilisateur se reconnecte régulièrement au Bureau à distance pour interagir avec les applications, puis il se déconnecte à nouveau. À un moment donné, quand l’utilisateur se reconnecte, la session Bureau à distance affiche uniquement un écran noir. Pour que la session s’affiche à nouveau correctement, l’utilisateur doit mettre fin à la session à partir de la console de l’ordinateur distant ou de la console du serveur Hôte de session Bureau à distance, puis arrêter les applications de la session.

Pour résoudre ce problème, appliquez les mises à jour suivantes comme il convient :

  - Windows 8 et Windows Server 2012 : KB4103719, [17 mai 2018—KB4103719 (préversion du correctif cumulatif mensuel)](https://support.microsoft.com/help/4103719/windows-server-2012-update-kb4103719)
  - Windows 8.1 et Windows Server 2012 R2 : KB4103724, [17 mai 2018—KB4103724 (préversion du correctif cumulatif mensuel)](https://support.microsoft.com/help/4103724/windows-81-update-kb4103724) et KB4284863, [21 juin 2018—KB4284863 (préversion du correctif cumulatif mensuel)](https://support.microsoft.com/help/4284863/windows-81-update-kb4284863)
  - Windows 10 : Résolu dans KB4284860, [le 12 juin 2018 — KB4284860 (Build du système d’exploitation 10240.17889)](https://support.microsoft.com/help/4284860/windows-10-update-kb4284860)
