---
title: Gérer les modèles NPS
description: Cette rubrique fournit des instructions sur la façon de créer, appliquer, exporter et importer des modèles de serveur NPS pour le serveur NPS dans Windows Server 2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 989b00c5-4767-4081-ace5-6321f8b2c55e
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 170a0e86f7dcca77c6efe841318b522554f8e78e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59845130"
---
# <a name="manage-nps-templates"></a>Gérer les modèles NPS

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Vous pouvez utiliser le serveur de stratégie réseau \(NPS\) modèles pour créer des éléments de configuration, tels que Remote Authentication Dial-In User Service \(RADIUS\) clients ou des secrets partagés, que vous pouvez réutiliser sur l’ordinateur local NPS et exportation pour une utilisation sur d’autres NPSs. 

Gestion des modèles fournit un nœud dans la console NPS où vous pouvez créer, modifier, supprimer, dupliquer et afficher l’utilisation des modèles de serveur NPS. Modèles NPS sont conçus pour réduire la quantité de temps et le coût nécessaire pour configurer NPS sur un ou plusieurs serveurs.

Les types de modèles NPS suivants sont disponibles pour la configuration dans la gestion des modèles.

- **Secrets partagés**. Ce type de modèle permet à vous permettent de spécifier un secret partagé que vous pouvez réutiliser (en sélectionnant le modèle dans l’emplacement approprié dans la console NPS) lorsque vous configurez des clients et serveurs RADIUS. 

- **Les Clients RADIUS**. Ce type de modèle qui permet de configurer les paramètres de client RADIUS que vous pouvez réutiliser en sélectionnant le modèle dans l’emplacement approprié dans la console NPS.

- **Serveurs RADIUS distants**. Ce modèle permet de configurer les paramètres de serveur RADIUS à distance que vous pouvez réutiliser en sélectionnant le modèle dans l’emplacement approprié dans la console NPS. 

- **Filtres IP**. Ce modèle permet de créer d’Internet Protocol version 4 (IPv4) et Internet Protocol version 6 \(IPv6\) filtres que vous pouvez réutiliser \(en sélectionnant le modèle dans l’emplacement approprié dans le serveur NPS console\) lorsque vous configurez des stratégies de réseau.

## <a name="create-an-nps-template"></a>Créer un modèle de serveur NPS

Configuration d’un modèle est différent de la configuration de serveur NPS directement. Création d’un modèle n’affecte pas les fonctionnalités du serveur NPS. Il est uniquement lorsque vous sélectionnez le modèle dans l’emplacement approprié dans la console NPS et appliquez le modèle que le modèle affecte les fonctionnalités de serveur NPS. 

Par exemple, si vous configurez un client RADIUS dans la console NPS sous **Clients et serveurs RADIUS**, modifier la configuration du serveur NPS et d’effectuer une étape de la configuration de serveur NPS pour communiquer avec l’un de vos serveurs d’accès réseau. \(L’étape suivante consiste à configurer le serveur d’accès réseau \(NAS\) pour communiquer avec le serveur NPS.\) 

Toutefois, si vous configurez un nouveau **Clients RADIUS** modèle dans la console NPS sous **gestion des modèles** au lieu de créer un nouveau client RADIUS sous **Clients RADIUS et serveurs**, vous avez créé un modèle, mais vous n’avez pas encore modifié la fonctionnalité de serveur NPS. Pour modifier les fonctionnalités de serveur NPS, vous devez appliquer le modèle à partir de l’emplacement approprié dans la console NPS.

La procédure suivante fournit des instructions sur la création d’un nouveau modèle.

Pour mener à bien cette procédure, il faut appartenir au groupe **Administrateurs** ou à un groupe équivalent.

### <a name="to-create-an-nps-template"></a>Pour créer un modèle de serveur NPS


1. Sur le serveur NPS, dans le Gestionnaire de serveur, cliquez sur **outils**, puis cliquez sur **Network Policy Server**. La console NPS s’ouvre. 

2. Dans la console NPS, développez **gestion des modèles**, cliquez sur un type de modèle, tel que **Clients RADIUS**, puis cliquez sur **New**.

3. Une boîte de dialogue de propriétés de modèle s’ouvre et que vous pouvez utiliser pour configurer votre modèle.

## <a name="apply-an-nps-template"></a>Appliquer un modèle de serveur NPS

Vous pouvez utiliser un modèle que vous avez créé dans **gestion des modèles** en accédant à un emplacement dans la console NPS, où vous pouvez appliquer le modèle. Par exemple, si vous souhaitez appliquer un modèle de Secrets partagés à une configuration de client RADIUS, vous pouvez utiliser la procédure suivante.

Pour mener à bien cette procédure, il faut appartenir au groupe **Administrateurs** ou à un groupe équivalent.

### <a name="to-apply-an-nps-template"></a>Pour appliquer un modèle de serveur NPS

1. Sur le serveur NPS, dans le Gestionnaire de serveur, cliquez sur **outils**, puis cliquez sur **Network Policy Server**. La console NPS s’ouvre.

2. Dans la console NPS, développez **Clients et serveurs RADIUS**, puis développez **Clients RADIUS**.

3.dans **Clients RADIUS**, dans le volet de détails, cliquez sur le client RADIUS auquel vous souhaitez appliquer le modèle de serveur NPS, puis cliquez sur **propriétés**.

4. Dans la boîte de dialogue Propriétés zone pour le client RADIUS, en **sélectionner un modèle de Secrets partagés existant**, sélectionnez le modèle que vous souhaitez appliquer à partir de la liste des modèles.

## <a name="export-or-import-nps-templates"></a>Exporter ou importer des modèles de serveur NPS

Vous pouvez exporter des modèles pour une utilisation sur d’autres NPSs, ou vous pouvez importer des modèles dans **gestion des modèles** pour une utilisation sur l’ordinateur local. 

Pour mener à bien cette procédure, il faut appartenir au groupe **Administrateurs** ou à un groupe équivalent.

### <a name="to-export-or-import-nps-templates"></a>Pour exporter ou importer des modèles NPS

1. Pour exporter des modèles de serveur NPS, dans la console NPS, avec le bouton droit **gestion des modèles**, puis cliquez sur **exporter les modèles dans un fichier**.

2. Pour importer des modèles de serveur NPS, dans la console NPS, avec le bouton droit **gestion des modèles**, puis cliquez sur **importer des modèles à partir d’un ordinateur** ou **importer des modèles à partir d’un fichier**.


