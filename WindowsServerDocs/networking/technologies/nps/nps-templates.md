---
title: Modèles NPS
description: Cette rubrique fournit une vue d’ensemble des modèles de serveur de stratégie réseau dans Windows Server 2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: fdfc0df1-21c7-492c-9fad-38fe9c7d935a
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 0647dbf0f99a01e32ba68475b439501e2dbeebfe
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59823310"
---
# <a name="nps-templates"></a>Modèles NPS

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Network Policy Server \(NPS\) les modèles vous permettent de créer des éléments de configuration, tels que Remote Authentication Dial-In User Service \(RADIUS\) clients ou des secrets partagés, que vous pouvez réutiliser sur l’ordinateur local NPS et exportation pour une utilisation sur d’autres NPSs.

Modèles NPS sont conçus pour réduire la quantité de temps et le coût nécessaire pour configurer NPS sur un ou plusieurs serveurs. Les types de modèles NPS suivants sont disponibles pour la configuration dans la gestion des modèles :

- Secrets partagés
- Clients RADIUS
- Serveurs RADIUS distants
- Filtres IP
- Groupes de serveurs de mise à jour

Configuration d’un modèle est différent de la configuration de serveur NPS directement. Création d’un modèle n’affecte pas les fonctionnalités du serveur NPS. Il est uniquement lorsque vous sélectionnez le modèle dans l’emplacement approprié dans la console NPS que le modèle affecte les fonctionnalités de serveur NPS. 

Par exemple, si vous configurez un client RADIUS dans la console NPS sous Clients et serveurs RADIUS, vous avez modifié la configuration de serveur NPS et effectuées une seule étape dans la configuration du serveur NPS pour communiquer avec l’un de vos serveurs d’accès réseau \(du NAS\) . \(L’étape suivante consisterait à configurer le NAS pour communiquer avec le serveur NPS.\) Toutefois, si vous configurez un nouveau modèle de Clients RADIUS dans la console NPS sous **gestion des modèles** au lieu de créer un nouveau client RADIUS sous **Clients et serveurs RADIUS**, vous avez créé un modèle, mais vous n’altéreront pas la fonctionnalité de serveur NPS encore. Pour modifier les fonctionnalités de serveur NPS, vous devez sélectionner le modèle à partir de l’emplacement approprié dans la console NPS.

## <a name="creating-templates"></a>Création de modèles

Pour créer un modèle, ouvrez la console NPS, cliquez sur un type de modèle, tel que **filtres IP**, puis cliquez sur **New**. Une boîte de dialogue de propriétés de modèle s’ouvre et vous permet de configurer votre modèle.

## <a name="using-templates-locally"></a>À l’aide de modèles localement

Vous pouvez utiliser un modèle que vous avez créées dans **gestion des modèles** en accédant à un emplacement dans la console NPS où le modèle peut être appliqué. Par exemple, si vous créez un nouveau modèle de Secrets partagés que vous souhaitez appliquer à une configuration de client RADIUS, dans **Clients et serveurs RADIUS** et **Clients RADIUS**, ouvrez les propriétés du client RADIUS. Dans **sélectionner un modèle de Secrets partagés existant**, sélectionnez le modèle que vous avez créé précédemment dans la liste des modèles disponibles.

Pour plus d’informations sur le serveur NPS, consultez [serveur NPS (Network Policy Server)](nps-top.md).
