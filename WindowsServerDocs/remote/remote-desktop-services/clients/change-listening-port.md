---
title: Modifier le port d’écoute dans Bureau à distance
description: Découvrez comment modifier le port d’écoute pour un client Bureau à distance.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.tgt_pltfrm: na
ms.topic: article
author: lizap
ms.author: elizapo
ms.date: 07/19/2018
ms.localizationpriority: medium
ms.openlocfilehash: b70479b644f4984c93136d6493483c372703244d
ms.sourcegitcommit: 3743cf691a984e1d140a04d50924a3a0a19c3e5c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/17/2019
ms.locfileid: "63749021"
---
# <a name="change-the-listening-port-for-remote-desktop-on-your-computer"></a>Modifier le port d’écoute pour Bureau à distance sur votre ordinateur

>S’applique à : Windows 10, Windows 8.1, Windows 8, Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2008 R2

Lorsque vous vous connectez à un ordinateur (un client Windows ou Windows Server) via le client Bureau à distance, la fonctionnalité Bureau à distance sur votre ordinateur « entend » la requête de connexion via un port d’écoute défini (3389 par défaut). Vous pouvez modifier ce port d’écoute sur les ordinateurs Windows en modifiant le registre.

1. Démarrez l’éditeur du registre. (Dans la zone de recherche, tapez regedit.)
2. Naviguez jusqu’à la sous-clé de Registre suivante : HKEY_LOCAL_MACHINE\System\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp\PortNumber
3. Cliquez sur **Édition > Modifier**, puis cliquez sur **Décimal**.
4. Tapez le numéro du nouveau port, puis cliquez sur **OK**. 
5. Fermez l’éditeur du registre et redémarrez votre ordinateur.

La prochaine fois que vous vous connectez à cet ordinateur à l’aide de la connexion Bureau à distance, vous devez taper le nouveau port. Si vous utilisez un pare-feu, veillez à configurer votre pare-feu pour autoriser les connexions par le nouveau numéro de port.
