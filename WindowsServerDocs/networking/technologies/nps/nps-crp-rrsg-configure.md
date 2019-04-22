---
title: Configurer des groupes de serveurs RADIUS distants
description: Cette rubrique fournit des informations sur la façon de configurer des groupes de serveurs RADIUS distants dans le serveur NPS dans Windows Server 2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: ca125e57-249c-4d97-85d1-2929cbf871f1
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 02088a35196c0bfadeb65e8971a47fdcc741258d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59818760"
---
# <a name="configure-remote-radius-server-groups"></a>Configurer des groupes de serveurs RADIUS distants

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Vous pouvez utiliser cette rubrique pour configurer des groupes de serveurs RADIUS distants lorsque vous souhaitez configurer NPS pour agir comme un serveur proxy et les demandes de connexion vers l’avant vers les autres NPSs pour le traitement.

## <a name="add-a-remote-radius-server-group"></a>Ajouter un groupe de serveurs RADIUS distants

Vous pouvez utiliser cette procédure pour ajouter un nouveau groupe de serveurs RADIUS distant dans le composant logiciel enfichable serveur NPS (Network Policy Server).

Lorsque vous configurez le serveur NPS en tant que proxy RADIUS, vous créez une nouvelle stratégie de demande de connexion que NPS utilise pour déterminer les demandes de connexion pour transférer à d’autres serveurs RADIUS. En outre, la stratégie de demande de connexion est configurée en spécifiant un rayon groupe de serveurs distants qui contient un ou plusieurs serveurs RADIUS, qui indique à NPS où envoyer les demandes de connexion qui correspondent à la stratégie de demande de connexion.

>[!NOTE]
>Vous pouvez également configurer un nouveau groupe de serveurs RADIUS distant pendant le processus de création d’une nouvelle stratégie de demande de connexion.

L'appartenance au groupe **Admins du domaine**, ou équivalent, est la condition minimale requise pour effectuer cette procédure.

### <a name="to-add-a-remote-radius-server-group"></a>Pour ajouter un groupe de serveurs RADIUS distants 

1. Dans le Gestionnaire de serveur, cliquez sur **outils**, puis cliquez sur **Network Policy Server** pour ouvrir la console NPS.
2. Dans l’arborescence de la console, double-cliquez sur **Clients et serveurs RADIUS**, avec le bouton droit **des groupes de serveurs RADIUS distants**, puis cliquez sur **New**.
3. Le **nouveau groupe de serveurs RADIUS distants** boîte de dialogue s’ouvre. Dans **nom_groupe**, tapez un nom pour le groupe de serveurs RADIUS distants.
4. **Dans les serveurs RADIUS**, cliquez sur **ajouter**. Le **ajouter des serveurs RADIUS** boîte de dialogue s’ouvre. Tapez l’adresse IP du serveur RADIUS que vous souhaitez ajouter au groupe, ou tapez le nom de domaine entièrement qualifié \(FQDN\) d’et le serveur RADIUS, puis cliquez sur **Vérifiez**.
5. Dans **ajouter des serveurs RADIUS**, cliquez sur le **authentification/gestion** onglet. Dans **secret partagé** et **confirmer le secret partagé**, tapez le secret partagé. Vous devez utiliser le même secret partagé lorsque vous configurez l’ordinateur local en tant que client RADIUS sur le serveur RADIUS à distance.
6. Si vous n’utilisez pas le protocole EAP (Extensible Authentication) pour l’authentification, cliquez sur **demande doit contenir l’attribut de l’authentificateur de message**. EAP utilise l’attribut de l’authentificateur de Message par défaut.
7. Vérifiez que les numéros de port d’authentification et de gestion des comptes sont corrects pour votre déploiement.
8. Si vous utilisez un secret partagé différent pour la comptabilité, dans **comptabilité**, désactivez le **utiliser le même secret partagé pour l’authentification et de comptabilité** case à cocher, puis tapez le secret partagé de comptabilité dans  **Secret partagé** et **confirmer le secret partagé**.
9. Si vous ne souhaitez pas transférer le démarrage de serveur d’accès réseau et arrêter des messages sur le serveur RADIUS à distance, désactivez le **transférer le démarrage de serveur d’accès réseau et d’arrêter les notifications pour ce serveur** case à cocher.

Pour plus d’informations sur la gestion de serveur NPS, consultez [gérer un serveur de stratégie réseau](nps-manage-top.md).

Pour plus d’informations sur le serveur NPS, consultez [serveur NPS (Network Policy Server)](nps-top.md).

