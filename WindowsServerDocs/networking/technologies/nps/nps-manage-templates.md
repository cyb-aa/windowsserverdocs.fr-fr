---
title: Gérer les modèles NPS
description: Cette rubrique fournit des instructions sur la façon de créer, appliquer, exporter et importer des modèles NPS pour le serveur NPS dans Windows Server2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 989b00c5-4767-4081-ace5-6321f8b2c55e
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 0a7f4c50d87c155c1adcd445eae8df23aab7730b
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/28/2018
---
# <a name="manage-nps-templates"></a>Gérer les modèles NPS

>S’applique à: Windows Server (canal annuel un point-virgule), Windows Server2016

Vous pouvez utiliser le serveur NPS \(NPS\) modèles pour créer des éléments de configuration, tels que les clients Remote Authentication Dial-In User Service \(RADIUS\) ou des secrets partagés, que vous pouvez réutiliser sur le serveur NPS local et l’exportation pour une utilisation sur d’autres serveurs NPS. 

Gestion des modèles fournit un nœud dans la console NPS, où vous pouvez créer, modifier, supprimer, dupliquer et afficher l’utilisation de modèles NPS. Modèles NPS sont conçus pour réduire la quantité de temps et le coût nécessaire pour configurer NPS sur un ou plusieurs serveurs.

Les types de modèles NPS suivants sont disponibles pour la configuration dans la gestion des modèles.

- **Secrets partagés**. Ce type de modèle permet de vous pouvez spécifier un secret partagé que vous pouvez réutiliser (en sélectionnant le modèle à l’emplacement approprié dans la console NPS) lorsque vous configurez des clients et serveurs RADIUS. 

- **Les Clients RADIUS**. Ce type de modèle permet de configurer les paramètres de client RADIUS que vous pouvez réutiliser en sélectionnant le modèle à l’emplacement approprié dans la console NPS.

- **Serveurs RADIUS distants**. Ce modèle permet de configurer les paramètres de serveur RADIUS à distance que vous pouvez réutiliser en sélectionnant le modèle à l’emplacement approprié dans la console NPS. 

- **Filtres IP**. Ce modèle permet de créer Internet Protocol version4 (IPv4) et Internet Protocol version6 \(IPv6\) filtres que vous pouvez réutiliser \ (en sélectionnant le modèle dans l’emplacement approprié dans la console NPS) lorsque vous configurez des stratégies de réseau.

## <a name="create-an-nps-template"></a>Créer un modèle de NPS

Configuration d’un modèle est différente de celle de la configuration du serveur NPS directement. Création d’un modèle n’affecte pas les fonctionnalités du serveur NPS. Il est uniquement lorsque vous sélectionnez le modèle à l’emplacement approprié dans la console NPS et appliquez le modèle que le modèle affecte la fonctionnalité du serveur NPS. 

Par exemple, si vous configurez un client RADIUS dans la console NPS sous **Clients et serveurs RADIUS**, vous modifiez la configuration du serveur NPS et effectuer une étape de la configuration NPS pour communiquer avec l’un de vos serveurs d’accès réseau. \ (L’étape suivante consiste à configurer le serveur d’accès réseau \(NAS\) pour communiquer avec le serveur NPS. \) 

Toutefois, si vous configurez un nouveau **Clients RADIUS** modèle dans la console NPS sous **gestion des modèles** plutôt que de la création d’un nouveau client RADIUS sous **Clients et serveurs RADIUS**, vous avez créé un modèle, mais vous n’avez pas encore modifié la fonctionnalité du serveur NPS. Pour modifier la fonctionnalité du serveur NPS, vous devez appliquer le modèle à partir de l’emplacement approprié dans la console NPS.

La procédure suivante fournit des instructions sur la création d’un nouveau modèle.

L’appartenance au groupe **administrateurs**, ou équivalente, est la condition minimale requise pour effectuer cette procédure.

### <a name="to-create-an-nps-template"></a>Pour créer un modèle NPS


1. Sur le serveur NPS, dans le Gestionnaire de serveur, cliquez sur **outils**, puis cliquez sur **serveur NPS**. La console NPS s’ouvre. 

2. Dans la console NPS, développez **gestion des modèles**, cliquez sur un type de modèle, tel que **Clients RADIUS**, puis cliquez sur **New**.

3. Une boîte de dialogue Propriétés du modèle s’ouvre, que vous pouvez utiliser pour configurer votre modèle.

## <a name="apply-an-nps-template"></a>Appliquer un modèle de NPS

Vous pouvez utiliser un modèle que vous avez créé dans **gestion des modèles** en accédant à un emplacement dans la console NPS, où vous pouvez appliquer le modèle. Par exemple, si vous souhaitez appliquer un modèle de Secrets partagés pour une configuration de client RADIUS, vous pouvez utiliser la procédure suivante.

L’appartenance au groupe **administrateurs**, ou équivalente, est la condition minimale requise pour effectuer cette procédure.

### <a name="to-apply-an-nps-template"></a>Pour appliquer un modèle NPS

1. Sur le serveur NPS, dans le Gestionnaire de serveur, cliquez sur **outils**, puis cliquez sur **serveur NPS**. La console NPS s’ouvre.

2. Dans la console NPS, développez **Clients et serveurs RADIUS**, puis développez **Clients RADIUS**.

3. Dans **Clients RADIUS**, dans le volet d’informations, le client RADIUS auquel vous souhaitez appliquer le modèle de serveur NPS, puis cliquez avec le bouton droit **propriétés**.

4. Dans la boîte de dialogue Propriétés zone pour le client RADIUS, en **sélectionner un modèle de Secrets partagés existant**, sélectionnez le modèle que vous souhaitez appliquer à partir de la liste de modèles.

## <a name="export-or-import-nps-templates"></a>Exporter ou importer des modèles de NPS

Vous pouvez exporter les modèles à utiliser sur d’autres serveurs NPS, ou vous pouvez importer des modèles dans **gestion des modèles** pour une utilisation sur l’ordinateur local. 

L’appartenance au groupe **administrateurs**, ou équivalente, est la condition minimale requise pour effectuer cette procédure.

### <a name="to-export-or-import-nps-templates"></a>Pour exporter ou importer des modèles NPS

1. Pour exporter les modèles NPS, dans la console NPS, faites un clic droit **gestion des modèles**, puis cliquez sur **exporter les modèles vers un fichier**.

2. Pour importer des modèles NPS, dans la console NPS, faites un clic droit **gestion des modèles**, puis cliquez sur **importer des modèles à partir d’un ordinateur** ou **importer des modèles à partir d’un fichier**.


