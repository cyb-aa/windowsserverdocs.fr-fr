---
title: Désactiver le transfert de Notification NAS dans NPS
description: Cette rubrique fournit des instructions sur la configuration d’authentifications simultanées Network Policy Server dans Windows Server 2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: a09bfb03-95fc-4534-bf3c-97078ef6b07e
ms.author: pashort
author: shortpatti
ms.openlocfilehash: bc4c6afdcb02eb2bbab1f0373a5b3a28236269bf
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59882260"
---
# <a name="disable-nas-notification-forwarding-in-nps"></a>Désactiver le transfert de Notification NAS dans NPS

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Vous pouvez utiliser cette procédure pour désactiver le transfert de démarrer et arrêter des messages à partir de serveurs d’accès réseau (NAS) aux membres du groupe de serveurs RADIUS distants configuré dans NPS.

Lorsque vous avez des groupes de serveurs RADIUS distants configurés et, dans NPS **stratégies de demande de connexion**, vous désactivez le **transférer les demandes de gestion des comptes à ce groupe de serveurs RADIUS distants** case à cocher, ces groupes sont toujours envoyés NAS démarrer et arrêter des messages de notification. 

Cette opération crée un trafic réseau inutile. Pour éliminer ce trafic, désactiver la notification de NAS transfert pour des serveurs individuels dans chaque groupe de serveurs RADIUS distants.

Pour réaliser cette procédure, vous devez être membre du groupe **Administrateurs**.

### <a name="to-disable-nas-notification-forwarding"></a>Pour désactiver le transfert de notification NAS

1. Dans le Gestionnaire de serveur, cliquez sur **outils**, puis cliquez sur **Network Policy Server**. La console NPS s’ouvre.

2. Dans la console NPS, double-cliquez sur **Clients et serveurs RADIUS**, cliquez sur **des groupes de serveurs RADIUS distants**, puis double-cliquez sur le groupe de serveurs RADIUS distants que vous souhaitez configurer. Le groupe de serveurs RADIUS distants **propriétés** boîte de dialogue s’ouvre.

3. Double-cliquez sur le membre du groupe que vous souhaitez configurer, puis cliquez sur le **authentification/gestion** onglet.

4. Dans **comptabilité**, désactivez le **transférer le démarrage de serveur d’accès réseau et d’arrêter les notifications pour ce serveur** case à cocher, puis cliquez sur **OK**.

5. Répétez les étapes 3 et 4 pour tous les membres du groupe que vous souhaitez configurer.

Pour plus d’informations sur la gestion de serveur NPS, consultez [gérer un serveur de stratégie réseau](nps-manage-top.md).

Pour plus d’informations sur le serveur NPS, consultez [serveur NPS (Network Policy Server)](nps-top.md).
