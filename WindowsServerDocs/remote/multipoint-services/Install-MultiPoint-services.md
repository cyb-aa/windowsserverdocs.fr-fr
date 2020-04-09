---
title: Installer MultiPoint Services
description: Découvrez comment installer et configurer MultiPoint services dans Windows Server 2016
ms.date: 07/22/2016
ms.prod: windows-server
ms.technology: multipoint-services
ms.topic: article
ms.assetid: f6f8970b-de3f-4255-b2a1-5472a16ed02f
author: evaseydl
manager: scottman
ms.author: evas
ms.openlocfilehash: ab82782f4ac1ffa8532dc23ad9340329a65765de
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80859212"
---
# <a name="install-multipoint-services"></a>Installer MultiPoint Services
Si vous installez un serveur à partir de zéro, suivez ces instructions pour installer MultiPoint services.  

Une fois que vous avez installé Windows Server 2016, connectez-vous en tant qu’administrateur. Utilisez le Gestionnaire de serveur où vous pouvez activer MultiPoint services. Le Gestionnaire de serveur s’ouvre automatiquement au démarrage. Dans le tableau de bord, sélectionnez **Ajouter des rôles et des fonctionnalités** pour activer multipoint services et suivez les instructions de l’Assistant.

Dans la section pour le type d’installation, vous pouvez utiliser le 
- Installation basée sur un rôle ou une fonctionnalité
- Installation des services Bureau à distance

Pour les déploiements standard de MultiPoint services, nous vous recommandons de sélectionner l’installation Services Bureau à distance qui vous permet de sélectionner facilement le rôle MultiPoint services sous type de déploiement. Pour l’installation basée sur les rôles, vous devez sélectionner **multipoint services** dans la liste des rôles. Le serveur redémarrera après une installation réussie.  
  
## <a name="configure-your-primary-station"></a>Configurer votre station principale  
  
1.  Sur la page **créer une station multipoint Server** , tapez la lettre spécifiée à partir du clavier pour cette analyse. L’entrée de clé appropriée associe le clavier et la souris de cette station.  
2.  Connectez-vous en tant qu’administrateur.  