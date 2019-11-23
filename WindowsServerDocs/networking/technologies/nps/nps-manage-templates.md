---
title: Gérer les modèles NPS
description: Cette rubrique fournit des instructions sur la création, l’application, l’exportation et l’importation de modèles NPS pour le serveur NPS (Network Policy Server) dans Windows Server 2016.
manager: brianlic
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: 989b00c5-4767-4081-ace5-6321f8b2c55e
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 5ac733c11b277f09e64779c33d3392303fc34d98
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71396154"
---
# <a name="manage-nps-templates"></a>Gérer les modèles NPS

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Vous pouvez utiliser le serveur de stratégie réseau \(les modèles de\) NPS pour créer des éléments de configuration, tels que protocole RADIUS (Remote Authentication Dial-In User Service) \(RADIUS\) des clients ou des secrets partagés, que vous pouvez réutiliser sur le serveur NPS local et exporter pour une utilisation sur d’autres NPSs. 

La gestion des modèles fournit un nœud dans la console NPS dans lequel vous pouvez créer, modifier, supprimer, dupliquer et afficher l’utilisation des modèles NPS. Les modèles NPS sont conçus pour réduire la durée et le coût nécessaires à la configuration de NPS sur un ou plusieurs serveurs.

Les types de modèles NPS suivants sont disponibles pour la configuration dans la gestion des modèles.

- **Secrets partagés**. Ce type de modèle vous permet de spécifier un secret partagé que vous pouvez réutiliser (en sélectionnant le modèle à l’emplacement approprié dans la console NPS) lorsque vous configurez des clients et des serveurs RADIUS. 

- **Clients RADIUS**. Ce type de modèle vous permet de configurer les paramètres du client RADIUS que vous pouvez réutiliser en sélectionnant le modèle à l’emplacement approprié dans la console NPS.

- **Serveurs RADIUS distants**. Ce modèle vous permet de configurer des paramètres de serveur RADIUS distants que vous pouvez réutiliser en sélectionnant le modèle à l’emplacement approprié dans la console NPS. 

- **Filtres IP**. Ce modèle vous permet de créer des filtres de\) IPv6 (Internet Protocol version 4) et IPv6 (Internet Protocol version 6) \(que vous pouvez réutiliser \(en sélectionnant le modèle à l’emplacement approprié dans la\) de la console NPS lorsque vous configurez des stratégies réseau.

## <a name="create-an-nps-template"></a>Créer un modèle NPS

La configuration d’un modèle est différente de la configuration du serveur NPS directement. La création d’un modèle n’affecte pas la fonctionnalité du serveur NPS. Ce n’est que lorsque vous sélectionnez le modèle à l’emplacement approprié dans la console NPS et que vous appliquez le modèle qui affecte les fonctionnalités du serveur NPS. 

Par exemple, si vous configurez un client RADIUS dans la console NPS sous **clients et serveurs RADIUS**, vous modifiez la configuration du serveur NPS et vous effectuez une étape dans la configuration de NPS pour communiquer avec l’un de vos serveurs d’accès réseau. \(l’étape suivante consiste à configurer le serveur d’accès réseau \(\) NAS pour communiquer avec NPS.\) 

Toutefois, si vous configurez un nouveau modèle **clients RADIUS** dans la console NPS sous **gestion des modèles** au lieu de créer un client RADIUS sous **clients et serveurs RADIUS**, vous avez créé un modèle, mais vous n’avez pas encore modifié la fonctionnalité du serveur NPS. Pour modifier la fonctionnalité de serveur NPS, vous devez appliquer le modèle à partir de l’emplacement approprié dans la console NPS.

La procédure suivante fournit des instructions sur la création d’un nouveau modèle.

Pour mener à bien cette procédure, il faut appartenir au groupe **Administrateurs** ou à un groupe équivalent.

### <a name="to-create-an-nps-template"></a>Pour créer un modèle NPS


1. Sur le serveur NPS, dans Gestionnaire de serveur, cliquez sur **Outils**, puis sur serveur NPS ( **Network Policy Server**). La console NPS s’ouvre. 

2. Dans la console NPS, développez **gestion des modèles**, cliquez avec le bouton droit sur un type de modèle, par exemple **clients RADIUS**, puis cliquez sur **nouveau**.

3. Une nouvelle boîte de dialogue Propriétés du modèle s’ouvre et vous permet de configurer votre modèle.

## <a name="apply-an-nps-template"></a>Appliquer un modèle NPS

Vous pouvez utiliser un modèle que vous avez créé dans **gestion des modèles** en accédant à un emplacement dans la console NPS où vous pouvez appliquer le modèle. Par exemple, si vous souhaitez appliquer un modèle de secrets partagés à une configuration de client RADIUS, vous pouvez utiliser la procédure suivante.

Pour mener à bien cette procédure, il faut appartenir au groupe **Administrateurs** ou à un groupe équivalent.

### <a name="to-apply-an-nps-template"></a>Pour appliquer un modèle NPS

1. Sur le serveur NPS, dans Gestionnaire de serveur, cliquez sur **Outils**, puis sur serveur NPS ( **Network Policy Server**). La console NPS s’ouvre.

2. Dans la console NPS, développez **clients et serveurs RADIUS**, puis **clients RADIUS**.

3.In les **clients RADIUS**, dans le volet d’informations, cliquez avec le bouton droit sur le client RADIUS auquel vous souhaitez appliquer le modèle NPS, puis cliquez sur **Propriétés**.

4. Dans la boîte de dialogue Propriétés du client RADIUS, dans **Sélectionner un modèle de secrets partagés existant**, sélectionnez le modèle que vous souhaitez appliquer dans la liste des modèles.

## <a name="export-or-import-nps-templates"></a>Exporter ou importer des modèles NPS

Vous pouvez exporter des modèles pour les utiliser sur d’autres NPSs, ou vous pouvez importer des modèles dans la **gestion des modèles** pour les utiliser sur l’ordinateur local. 

Pour mener à bien cette procédure, il faut appartenir au groupe **Administrateurs** ou à un groupe équivalent.

### <a name="to-export-or-import-nps-templates"></a>Pour exporter ou importer des modèles NPS

1. Pour exporter des modèles NPS, dans la console NPS, cliquez avec le bouton droit sur **gestion des modèles**, puis cliquez sur **Exporter les modèles vers un fichier**.

2. Pour importer des modèles NPS, dans la console NPS, cliquez avec le bouton droit sur **gestion des modèles**, puis cliquez sur **importer des modèles à partir d’un ordinateur** ou **importer des modèles à partir d’un fichier**.


