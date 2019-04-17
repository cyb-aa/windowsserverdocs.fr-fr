---
title: Modifier le port d’écoute du Bureau à distance
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
ms.openlocfilehash: b70479b644f4984c93136d6493483c372703244d
ms.sourcegitcommit: d3f73936160505a40633ad8dd5931ac5fe3eccdb
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/12/2019
ms.locfileid: "9297318"
---
# Modifier le port d’écoute pour le Bureau à distance sur votre ordinateur

>S’applique à: Windows 10, Windows 8.1, Windows 8, Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2008 R2

Lorsque vous vous connectez à un ordinateur (un client Windows ou Windows Server) par le biais du client Bureau à distance, la fonctionnalité de bureau à distance sur votre ordinateur «entend du «la demande de connexion par le biais d’un port d’écoute défini (3389 par défaut). Vous pouvez modifier ce port à l’écoute sur les ordinateurs Windows en modifiant le Registre.

1. Démarrez l’Éditeur du Registre. (Dans la zone de recherche, tapez regedit.)
2. Accédez à la sous-clé de Registre suivante: HKEY_LOCAL_MACHINE\System\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp\PortNumber
3. Cliquez sur **Modifier > modifier**, puis cliquez sur **décimal**.
4. Entrez le nouveau numéro de port, puis cliquez sur **OK**. 
5. Fermez l’Éditeur du Registre et redémarrez votre ordinateur.

La prochaine fois que vous vous connectez à cet ordinateur à l’aide de la connexion Bureau à distance, vous devez taper le nouveau port. Si vous utilisez un pare-feu, veillez à configurer votre pare-feu pour autoriser les connexions au nouveau numéro de port.
