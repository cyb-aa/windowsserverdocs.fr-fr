---
title: Désactiver le transfert de notifications NAS dans NPS
description: Cette rubrique fournit des instructions sur la configuration d’authentifications simultanées du serveur de stratégie réseau dans Windows Server2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: a09bfb03-95fc-4534-bf3c-97078ef6b07e
ms.author: pashort
author: shortpatti
ms.openlocfilehash: c25f3b5a94624a35099e84ede3296f7ab860da7c
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/28/2018
---
# <a name="disable-nas-notification-forwarding-in-nps"></a>Désactiver le transfert de notifications NAS dans NPS

>S’applique à: Windows Server (canal annuel un point-virgule), Windows Server2016

Vous pouvez utiliser cette procédure pour désactiver le transfert du menu Démarrer et arrêter des messages à partir des serveurs d’accès réseau (NAS) aux membres du groupe de serveurs RADIUS distants configurés dans NPS.

Lorsque vous avez des groupes de serveurs RADIUS distants configurés et, dans NPS **stratégies de demande de connexion**, vous désactivez le **transférer les demandes de gestion des comptes à ce groupe de serveurs RADIUS distants** case à cocher, ces groupes sont toujours envoyés NAS démarrer et arrêter les messages de notification. 

Cela crée un trafic réseau inutile. Pour éliminer ce trafic, désactiver la notification de NAS de transfert pour des serveurs individuels dans chaque groupe de serveurs RADIUS distants.

Pour effectuer cette procédure, vous devez être un membre de la **administrateurs** groupe.

### <a name="to-disable-nas-notification-forwarding"></a>Pour désactiver le transfert de notifications NAS

1. Dans le Gestionnaire de serveur, cliquez sur **outils**, puis cliquez sur **serveur NPS**. La console NPS s’ouvre.

2. Dans la console NPS, double-cliquez sur **Clients et serveurs RADIUS**, cliquez sur **des groupes de serveurs RADIUS distants**, puis double-cliquez sur le groupe de serveurs RADIUS distants que vous souhaitez configurer. Le groupe de serveurs RADIUS distants **propriétés** boîte de dialogue s’ouvre.

3. Double-cliquez sur le membre du groupe que vous souhaitez configurer, puis cliquez sur le **/gestion** onglet.

4. Dans **comptabilité**, désactivez le **transférer le démarrage de serveur d’accès réseau et arrêter les notifications de ce serveur** case à cocher, puis cliquez sur **OK**.

5. Répétez les étapes3 et 4 pour tous les membres du groupe que vous souhaitez configurer.

Pour plus d’informations sur la gestion du serveur NPS, voir [gérer un serveur de stratégie réseau](nps-manage-top.md).

Pour plus d’informations sur le serveur NPS, voir [serveur NPS (Network Policy)](nps-top.md).
