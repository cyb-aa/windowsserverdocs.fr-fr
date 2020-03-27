---
title: Désactiver le transfert de notifications NAS dans NPS
description: Cette rubrique fournit des instructions sur la configuration des authentifications simultanées du serveur de stratégie réseau dans Windows Server 2016.
manager: brianlic
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: a09bfb03-95fc-4534-bf3c-97078ef6b07e
ms.author: lizross
author: eross-msft
ms.openlocfilehash: dd56dfd4db9dd41c98141e2239efcca544a364fe
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80316168"
---
# <a name="disable-nas-notification-forwarding-in-nps"></a>Désactiver le transfert de notifications NAS dans NPS

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Vous pouvez utiliser cette procédure pour désactiver le transfert des messages de démarrage et d’arrêt à partir des serveurs d’accès réseau (NAS) vers les membres d’un groupe de serveurs RADIUS distants configuré dans NPS.

Lorsque vous avez configuré des groupes de serveurs RADIUS distants et, dans **stratégies de demande de connexion**NPS, vous désactivez la case à cocher transférer les **demandes de gestion de comptes à ce groupe de serveurs RADIUS distants** , ces groupes reçoivent toujours les messages de notification de démarrage et d’arrêt NAS. 

Cela crée un trafic réseau inutile. Pour éliminer ce trafic, désactivez le transfert de notifications NAS pour des serveurs individuels dans chaque groupe de serveurs RADIUS distants.

Pour réaliser cette procédure, vous devez être membre du groupe **Administrateurs**.

### <a name="to-disable-nas-notification-forwarding"></a>Pour désactiver le transfert de notifications NAS

1. Dans Gestionnaire de serveur, cliquez sur **Outils**, puis sur **serveur NPS (Network Policy Server**). La console NPS s’ouvre.

2. Dans la console NPS, double-cliquez sur **clients et serveurs RADIUS**, cliquez sur **groupes de serveurs RADIUS distants**, puis double-cliquez sur le groupe de serveurs RADIUS distants que vous souhaitez configurer. La boîte de dialogue **Propriétés** du groupe de serveurs RADIUS distants s’ouvre.

3. Double-cliquez sur le membre du groupe que vous souhaitez configurer, puis cliquez sur l’onglet **authentification/comptabilité** .

4. Dans **comptabilité**, désactivez la case à cocher **transférer le serveur d’accès réseau et arrêter les notifications à ce serveur** , puis cliquez sur **OK**.

5. Répétez les étapes 3 et 4 pour tous les membres du groupe que vous souhaitez configurer.

Pour plus d’informations sur la gestion de NPS, consultez [gérer un serveur de stratégie réseau](nps-manage-top.md).

Pour plus d’informations sur NPS, consultez [serveur NPS (Network Policy Server)](nps-top.md).
