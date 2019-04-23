---
title: Modifier le port d’écoute dans le Bureau à distance
description: Découvrez comment modifier le port d’écoute pour le client Bureau à distance.
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
ms.openlocfilehash: 5bf90010143e742f7a0c9b5c262be01e6ccf8c5c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59882720"
---
# <a name="change-the-listening-port-for-remote-desktop-on-your-computer"></a>Modifier le port d’écoute pour le Bureau à distance sur votre ordinateur

>S’applique à : Windows 10, Windows 8.1, Windows 8, Windows Server 2016, Windows Server 2012 R2, Windows Server 2008 R2

Lorsque vous vous connectez à un ordinateur (un client de Windows ou Windows Server) via le client Bureau à distance, la fonctionnalité de bureau à distance sur votre ordinateur » entend « la demande de connexion via un port d’écoute défini (3389 par défaut). Vous pouvez modifier ce port d’écoute sur les ordinateurs Windows en modifiant le Registre.

1. Démarrez l’Éditeur du Registre. (Dans la zone de recherche, tapez regedit.)
2. Naviguez jusqu’à la sous-clé de Registre suivante : HKEY_LOCAL_MACHINE\System\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp\PortNumber
3. Cliquez sur **Modifier > modifier**, puis cliquez sur **décimal**.
4. Tapez le nouveau numéro de port, puis cliquez sur **OK**. 
5. Fermez l’Éditeur du Registre et redémarrez votre ordinateur.

La prochaine fois que vous vous connectez à cet ordinateur à l’aide de la connexion Bureau à distance, vous devez taper le nouveau port. Si vous utilisez un pare-feu, veillez à configurer votre pare-feu pour autoriser les connexions au nouveau numéro de port.
