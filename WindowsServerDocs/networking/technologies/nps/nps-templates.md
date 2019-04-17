---
title: Modèles NPS
description: Cette rubrique fournit une vue d’ensemble des modèles de serveur de stratégie de réseau dans Windows Server2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: fdfc0df1-21c7-492c-9fad-38fe9c7d935a
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 2835959b8c076ef7b6aeb1fca31a62717ef95037
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/28/2018
---
# <a name="nps-templates"></a>Modèles NPS

>S’applique à: Windows Server (canal annuel un point-virgule), Windows Server2016

Modèles \(NPS\) serveur de stratégie de réseau vous autorisant à créer des éléments de configuration, tels que les clients Remote Authentication Dial-In User Service \(RADIUS\) ou des secrets partagés, que vous pouvez réutiliser sur le serveur NPS local et l’exportation pour une utilisation sur d’autres serveurs NPS.

Modèles NPS sont conçus pour réduire la quantité de temps et le coût nécessaire pour configurer NPS sur un ou plusieurs serveurs. Les types de modèles NPS suivants sont disponibles pour la configuration dans la gestion des modèles:

- Secrets partagés
- Clients RADIUS
- Serveurs RADIUS distants
- Filtres IP
- Groupes de serveurs de mise à jour

Configuration d’un modèle est différente de celle de la configuration du serveur NPS directement. Création d’un modèle n’affecte pas les fonctionnalités du serveur NPS. Il est uniquement lorsque vous sélectionnez le modèle à l’emplacement approprié dans la console NPS que le modèle affecte la fonctionnalité du serveur NPS. 

Par exemple, si vous configurez un client RADIUS dans la console NPS sous Clients et serveurs RADIUS, vous avez modifié la configuration du serveur NPS et effectuées une seule étape dans la configuration du serveur NPS pour communiquer avec l’un de votre réseau accès serveurs \(NAS's\). \ (Serait l’étape suivante pour configurer le NAS pour communiquer avec le serveur NPS. \), toutefois, si vous configurez un nouveau modèle de Clients RADIUS dans la console NPS sous **gestion des modèles** plutôt que de la création d’un nouveau client RADIUS sous **Clients et serveurs RADIUS**, vous avez créé un modèle, mais vous n’avez pas encore modifié la fonctionnalité du serveur NPS. Pour modifier la fonctionnalité du serveur NPS, vous devez sélectionner le modèle à partir de l’emplacement approprié dans la console NPS.

## <a name="creating-templates"></a>Création de modèles

Pour créer un modèle, ouvrez la console NPS, cliquez sur un type de modèle, tel que **filtres IP**, puis cliquez sur **New**. Une boîte de dialogue Propriétés du modèle s’ouvre qui vous permet de configurer votre modèle.

## <a name="using-templates-locally"></a>À l’aide de modèles localement

Vous pouvez utiliser un modèle que vous avez créée dans **gestion des modèles** en accédant à un emplacement dans la console NPS où le modèle peut être appliqué. Par exemple, si vous créez un nouveau modèle Secrets partagés que vous souhaitez appliquer à une configuration de client RADIUS, dans **Clients et serveurs RADIUS** et **Clients RADIUS**, ouvrez les propriétés du client RADIUS. Dans **sélectionner un modèle de Secrets partagés existant**, sélectionnez le modèle que vous avez créé précédemment dans la liste des modèles disponibles.

Pour plus d’informations sur le serveur NPS, voir [serveur NPS (Network Policy)](nps-top.md).
