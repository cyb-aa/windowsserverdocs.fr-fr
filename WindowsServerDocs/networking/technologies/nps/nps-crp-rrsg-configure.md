---
title: Configurer des groupes de serveurs RADIUS distants
description: Cette rubrique fournit des informations sur la configuration de groupes de serveurs RADIUS distants dans le serveur de stratégie réseau dans Windows Server 2016.
manager: brianlic
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: ca125e57-249c-4d97-85d1-2929cbf871f1
ms.author: pashort
author: shortpatti
ms.openlocfilehash: fe34d25d2b54b02bb56fcad99c433054a309f60b
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71405457"
---
# <a name="configure-remote-radius-server-groups"></a>Configurer des groupes de serveurs RADIUS distants

>S’applique à : Windows Server (Canal semi-annuel), Windows Server 2016

Vous pouvez utiliser cette rubrique pour configurer des groupes de serveurs RADIUS distants lorsque vous souhaitez configurer NPS pour agir en tant que serveur proxy et transférer les demandes de connexion à d’autres NPSs à des fins de traitement.

## <a name="add-a-remote-radius-server-group"></a>Ajouter un groupe de serveurs RADIUS distants

Vous pouvez utiliser cette procédure pour ajouter un nouveau groupe de serveurs RADIUS distants dans le composant logiciel enfichable NPS (Network Policy Server).

Quand vous configurez NPS en tant que proxy RADIUS, vous créez une stratégie de demande de connexion que NPS utilise pour déterminer les demandes de connexion à transférer vers d’autres serveurs RADIUS. En outre, la stratégie de demande de connexion est configurée en spécifiant un groupe de serveurs RADIUS distants qui contient un ou plusieurs serveurs RADIUS, ce qui indique à NPS où envoyer les demandes de connexion qui correspondent à la stratégie de demande de connexion.

>[!NOTE]
>Vous pouvez également configurer un nouveau groupe de serveurs RADIUS distants au cours du processus de création d’une stratégie de demande de connexion.

L'appartenance au groupe **Admins du domaine**, ou équivalent, est la condition minimale requise pour effectuer cette procédure.

### <a name="to-add-a-remote-radius-server-group"></a>Pour ajouter un groupe de serveurs RADIUS distants 

1. Dans Gestionnaire de serveur, cliquez sur **Outils**, puis sur **serveur de stratégie réseau** pour ouvrir la console NPS.
2. Dans l’arborescence de la console, double-cliquez sur **clients et serveurs RADIUS**, cliquez avec le bouton droit sur **groupes de serveurs RADIUS distants**, puis cliquez sur **nouveau**.
3. La boîte **de dialogue Nouveau groupe de serveurs RADIUS distants** s’ouvre. Dans **nom du groupe**, tapez un nom pour le groupe de serveurs RADIUS distants.
4. **Dans serveurs RADIUS**, cliquez sur **Ajouter**. La boîte de dialogue **Ajouter des serveurs RADIUS** s’ouvre. Tapez l’adresse IP du serveur RADIUS que vous souhaitez ajouter au groupe, ou tapez le nom de domaine complet \(FQDN @ no__t-1 du serveur RADIUS, puis cliquez sur **vérifier**.
5. Dans **Ajouter des serveurs RADIUS**, cliquez sur l’onglet **authentification/comptabilité** . Dans **secret partagé** et **confirmer le secret partagé**, tapez le secret partagé. Lorsque vous configurez l’ordinateur local en tant que client RADIUS sur le serveur RADIUS distant, vous devez utiliser le même secret partagé.
6. Si vous n’utilisez pas le protocole EAP (Extensible Authentication Protocol) pour l’authentification, cliquez sur **la demande doit contenir l’attribut de l’authentificateur de message**. EAP utilise l’attribut d’authentificateur de message par défaut.
7. Vérifiez que les numéros de port d’authentification et de gestion des comptes sont corrects pour votre déploiement.
8. Si vous utilisez un secret partagé différent pour la gestion des comptes, dans **comptabilité**, désactivez la case à cocher **utiliser le même secret partagé pour l’authentification et la gestion des comptes** , puis tapez le secret partagé de comptabilité dans **secret partagé** et **confirmez partagé secret**.
9. Si vous ne souhaitez pas transférer les messages de démarrage et d’arrêt du serveur d’accès réseau vers le serveur RADIUS distant, désactivez la case à cocher **transférer le serveur d’accès réseau et arrêter les notifications à ce serveur** .

Pour plus d’informations sur la gestion de NPS, consultez [gérer un serveur de stratégie réseau](nps-manage-top.md).

Pour plus d’informations sur NPS, consultez [serveur NPS (Network Policy Server)](nps-top.md).

